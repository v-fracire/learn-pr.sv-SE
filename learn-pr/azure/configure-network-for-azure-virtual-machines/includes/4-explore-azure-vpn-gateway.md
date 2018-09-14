Om du vill integrera din lokala miljö med Azure behöver du ha möjlighet att skapa en krypterad anslutning. Du kan ansluta via Internet eller via en dedikerad anslutning. Här ska vi titta på Azure VPN-Gateway, vilket ger en slutpunkt för inkommande anslutningar från lokala miljöer.

Du har konfigurerat ett Azure-nätverk och se till att alla data som överförs från Azure till din plats och mellan Azure virtuella nätverk som är krypterade. Du måste också veta hur du ansluter virtuella nätverk mellan regioner och prenumerationer.

## <a name="what-is-a-vpn-gateway"></a>Vad är en VPN-gateway?

En Azure VPN-gatewayen tillhandahåller en slutpunkt för inkommande krypterade anslutningar från lokala platser till Azure via Internet. Det kan också skicka krypterad trafik mellan Azure-nätverk över Microsofts dedikerat nätverk som länkar Azure-datacenter i olika regioner. Med den här konfigurationen kan du länka virtuella datorer och tjänster i olika regioner på ett säkert sätt.

Ett virtuellt nätverk kan endast ha en VPN-gateway. Alla anslutningar till den VPN-gatewayen delar den tillgängliga nätverksbandbredden.

Varje virtuell nätverksgateway består av två eller flera virtuella datorer (VM). Dessa virtuella datorer har distribuerats till ett särskilt undernät som du anger, vilket kallas _gatewayundernätet_. De innehåller routningstabeller för anslutningar till andra nätverk, tillsammans med specifika gatewaytjänster. De virtuella datorerna och gatewayundernätet liknar en förstärkt nätverksenhet. Du behöver inte konfigurera de här virtuella datorerna direkt och du bör inte distribuera några ytterligare resurser till gatewayundernätet.

Att skapa en virtuell nätverksgateway kan ta lite tid, så det är viktigt att planera på ett lämpligt sätt. När du skapar en virtuell nätverksgateway genererar etableringsprocessen de virtuella gatewaydatorerna och distribuerar dem till gatewayundernätet. De virtuella datorerna har de inställningar som du konfigurerar på gatewayen.

En viktig inställning är **_gatewaytypen_**, som för en VPN-gateway ska vara av typen ”vpn”. Alternativ för VPN-gatewayer är:

- Nätverk-till-network-anslutningar via IPsec/IKE VPN-tunnel, länka VPN-gatewayer till andra VPN-gatewayer.

- Tunnlar mellan IPsec/IKE VPN på olika platser, för anslutning av lokala nätverk till Azure via dedikerade VPN-enheter för att skapa plats-till-plats-anslutningar.

- Punkt-till-plats-anslutningar via IKEv2 eller SSTP, för att länka klientdatorer till resurser i Azure.

Nu ska vi titta på de faktorer som du behöver tänka på för att planera din VPN-gateway.

## <a name="plan-a-vpn-gateway"></a>Planera en VPN-gateway

När du planerar en VPN-gateway finns tre arkitekturer att tänka på:

- Punkt till plats via internet
- Plats till plats via internet
- Plats till plats via ett dedikerat nätverk, till exempel Azure ExpressRoute

### <a name="planning-factors"></a>Planera faktorer

Faktorer som du behöver ta med under planeringsprocessen är exempelvis:

- Dataflöde – Mbit/s eller Gbit/s
- Stamnät – internet eller privat?
- Tillgängligheten för en offentlig IP-adress (statisk)
- Kompatibilitet med VPN-enhet
- Flera klientanslutningar eller länk för plats-till-plats?
- Typ av VPN-gateway
- SKU för Azure VPN-gateway

I följande tabell sammanfattas några av dessa planeringsproblem. Övriga frågor diskuteras senare.

|                           |  Peka på plats            | Plats till plats                          |  ExpressRoute                 |
| -------------             | -------------             | -------------                         | ---------                     |
| Azure-tjänster som stöds  | Cloud Services och virtuella datorer    | Cloud Services och virtuella datorer                | Alla tjänster som stöds        |
| Vanliga bandbredd         | Beror på VPN-Gateway-SKU    | Upp till 1 Gbit/s med aggregering         | Från 50 Mbit/s till 10 Gbit/s       |
| Protokoll som stöds       | SSTP och IPsec            | IPsec                                 | Direktanslutning, VLAN      |
| Routning                   | RouteBased (dynamisk)      | PolicyBased (statisk) och RouteBased   | BGP                           |
| Anslutningsåterhämtning     | Aktiv-passiv            | aktivt-passivt eller aktivt-aktivt       | aktiv-aktiv                 |
| Användningsfall                  | Testning och prototyper   | Utveckling, testning och småskalig produktion  | Enterprise-/ verksamhetskritiska   |

### <a name="gateway-skus"></a>Gateway-SKU:er

Azure erbjuder följande SKU:er för gatewaytjänster:

| SKU              |  S2S/nätverk-till-network-tunnlar | P2S-anslutningar  |  Prestandamått för aggregerat dataflöde   | Använd för                         |
| -------------    | -------------             | -------------    | ---------                         | ---------                       |
| Basic            | Max 10                    | Max 128          | 100 Mbit/s                          | Dev/test/POC                    |
| VpnGw1           | Max 30                    | Max 128          | 650 Mbit/s                          | Produktion/kritiska arbetsbelastningar   |
| VpnGw2           | Max 30                    | Max 128          | 1 Gbit/s                            | Produktion/kritiska arbetsbelastningar   |
| VpnGw3           | Max 30                    | Max 128          | 1,25 Gbit/s                          | Produktion/kritiska arbetsbelastningar   |

> [!Note]
> Det är viktigt att du väljer rätt SKU: N. Om du har konfigurerat din VPN-gateway med det fel, måste du ta bort och återskapa gatewayen, som kan ta lång tid.

## <a name="workflow"></a>Arbetsflöde

När du utformar en strategi för molnanslutning med ett virtuellt privat nätverk i Azure bör du använda följande arbetsflöde:

1. Utforma din anslutningstopologi och skapa en lista över adressutrymmena för alla anslutningsnätverk.

1. Skapa ett virtuellt Azure-nätverk.

1. Skapa en VPN-gateway för det virtuella nätverket.

1. Skapa och konfigurera anslutningar till lokala nätverk eller andra virtuella nätverk som krävs.

1. Om det behövs kan du skapa och konfigurera en punkt-till-plats-anslutning för din Azure VPN-gateway.

### <a name="design-considerations"></a>Designöverväganden

När du utformar dina VPN-gatewayer för att ansluta virtuella nätverk behöver du tänka på följande faktorer:

- Undernät får inte överlappa

    Det är viktigt att ett undernät på en plats inte innehåller samma adressutrymmet som i en annan plats.

- IP-adresser måste vara unikt

    Du kan inte ha två värdar med samma IP-adress på olika platser, som det omöjligt att dirigera trafik mellan dessa två värdar och nätverk-till-network-anslutningen misslyckas.

- VPN-gatewayer behöver en gatewayundernät som kallas **GatewaySubnet**

    Det här namnet för att gatewayen ska fungera måste ha och det får inte innehålla andra resurser.

### <a name="create-an-azure-virtual-network"></a>Skapa en Azure-nätverk

Innan du skapar en VPN-gateway, måste du skapa virtuella Azure-nätverket.

### <a name="create-a-vpn-gateway"></a>Skapa en VPN-gateway

Typen av VPN-gateway som du skapar beror på din arkitektur. Alternativen är:

- Routningsbaserad

    Ruttbaserad VPN-enheter använder trafikväljare för alla-till-alla (med jokertecken) och låt routning/vidarebefordran tabeller dirigera trafik till olika IPsec-tunnlar. Routningsbaserade anslutningar bygger vanligtvis på routerplattformar där varje IPsec-tunnel modelleras som ett nätverksgränssnitt eller ett VTI (virtuellt tunnelgränssnitt).

- Principbaserad

    Principbaserad VPN-enheter använder kombinationer av prefix från båda nätverken för att definiera hur trafik är krypterad/dekrypteras via IPsec-tunnlar. En principbaserad anslutning bygger vanligtvis på brandväggsenheter som utför paketfiltrering. Kryptering och avkryptering av IPsec-tunnlar läggs till i motorn för paketfiltrering och bearbetning.

## <a name="set-up-a-vpn-gateway"></a>Konfigurera en VPN-gateway

Vilka åtgärder du behöver vidta beror på vilken typ av VPN-gateway du installerar. För att skapa en punkt-till-plats VPN-gateway med hjälp av Azure portal, skulle du till exempel har utfört följande steg:

1. Skapa ett virtuellt nätverk

2. Lägga till ett gatewayundernät

3. Ange en DNS-server (valfritt)

4. Skapa en virtuell nätverksgateway

5. Generera certifikat

6. Lägga till klientadresspoolen

7. Konfigurera Tunneltyp

8. Konfigurera autentiseringstypen

9. Ladda upp offentliga certifikatdata från rotcertifikatet

10. Installera ett exporterat klientcertifikat

11. Skapa och installera VPN-klientkonfigurationspaketet

12. Ansluta till Azure

Eftersom det finns flera vägar för konfigurationen med Azure VPN-gatewayer med flera alternativ, går det inte att täcka alla inställningarna i den här kursen. Mer information finns i avsnittet Ytterligare resurser.

## <a name="configure-the-gateway"></a>Konfigurera gatewayen

När du har skapat din gateway behöver du konfigurera den.  Det finns flera konfigurationsinställningar som du måste ange till exempel namn, plats, DNS-server, osv. Vi går igenom dessa i detalj i övningen.

## <a name="summary"></a>Sammanfattning

Azure VPN gateway är en komponent i Azure-nätverk som gör det möjligt för punkt-till-plats, plats-till-plats eller nätverket till nätverksanslutningar. Azure VPN-gatewayer att enskilda klientdatorer att ansluta till resurser i Azure, utöka lokala nätverk till Azure eller underlätta anslutningar mellan virtuella nätverk i olika regioner och prenumerationer.
