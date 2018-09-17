Om du vill integrera din lokala miljö med Azure behöver du möjlighet att skapa en krypterad anslutning. Du kan ansluta via det offentliga internet eller via en dedikerad länk. Här ska vi titta på Azures VPN Gateway, som utgör en slutpunkt för inkommande anslutningar från lokala miljöer.

Du har konfigurerat ett virtuellt Azure-nätverk och behöver se till att alla dataöverföringar från Azure till din plats och mellan virtuella Azure-nätverk krypteras. Du måste också veta hur du ansluter virtuella nätverk mellan regioner och prenumerationer.

## <a name="describe-a-vpn-gateway"></a>Beskriv en VPN-gateway

En Azure VPN-gateway tillhandahåller en slutpunkt för inkommande krypterade anslutningar från lokala platser till Azure via internet. Den kan också skicka krypterad trafik mellan virtuella Azure-nätverk över Microsofts dedikerade nätverk som länkar Azure-datacenter i olika regioner. Med den här konfigurationen kan du länka virtuella datorer och tjänster i olika regioner på ett säkert sätt.

Ett virtuellt nätverk kan endast ha en VPN-gateway. Alla anslutningar till den VPN-gatewayen delar den tillgängliga nätverksbandbredden.

I varje virtuell nätverksgateway finns två eller flera virtuella datorer (VM). Dessa virtuella datorer har distribuerats till ett särskilt undernät som du anger, vilket kallas _gatewayundernätet_. De innehåller routningstabeller för anslutningar till andra nätverk, tillsammans med specifika gatewaytjänster. De virtuella datorerna och gatewayundernätet liknar en förstärkt nätverksenhet. Du behöver inte konfigurera de här virtuella datorerna direkt och du bör inte distribuera några ytterligare resurser till gatewayundernätet.

Att skapa en virtuell nätverksgateway kan ta lite tid, så det är viktigt att planera på ett lämpligt sätt. När du skapar en virtuell nätverksgateway genererar etableringsprocessen de virtuella gatewaydatorerna och distribuerar dem till gatewayundernätet. De virtuella datorerna har de inställningar som du konfigurerar på gatewayen.

En viktig inställning är **_gatewaytypen_**, som för en VPN-gateway ska vara av typen ”vpn”. Alternativ för VPN-gatewayer är:

- Nätverk-till-nätverk-anslutningar via IPsec/IKE VPN-tunnel, som länkar VPN-gatewayer till andra VPN-gatewayer.

- Tunnlar mellan IPsec/IKE VPN på olika platser, för anslutning av lokala nätverk till Azure via dedikerade VPN-enheter för att skapa plats-till-plats-anslutningar.

- Punkt till plats-anslutningar via IKEv2 eller SSTP för att länka klientdatorer till resurser i Azure.

Nu ska vi titta på de faktorer som du behöver tänka på för att planera din VPN-gateway.

## <a name="plan-a-vpn-gateway"></a>Planera en VPN-gateway

När du planerar en VPN-gateway finns det tre arkitekturer att överväga:

- Punkt till plats via internet
- Plats till plats via internet
- Plats till plats via ett dedikerat nätverk, till exempel Azure ExpressRoute

### <a name="planning-factors"></a>Planeringsfaktorer

Faktorer som du behöver ta med under planeringsprocessen är exempelvis:

- Dataflöde – Mbit/s eller Gbit/s
- Stamnät – internet eller privat?
- Tillgängligheten för en offentlig IP-adress (statisk)
- Kompatibilitet med VPN-enhet
- Flera klientanslutningar eller länk för plats-till-plats?
- Typ av VPN-gateway
- SKU för Azure VPN Gateway

I följande tabell sammanfattas några av dessa planeringsproblem. Övriga frågor diskuteras senare.

|                           |  Punkt-till-plats            | Plats-till-plats                          |  ExpressRoute                 |
| -------------             | -------------             | -------------                         | ---------                     |
| Azure-tjänster som stöds  | Cloud Services och virtuella datorer    | Cloud Services och virtuella datorer                | Alla tjänster som stöds        |
| Vanliga bandbredder         | Beror på SKU för VPN Gateway    | Upp till 1 Gbit/s med aggregering         | Från 50 Mbit/s till 10 Gbit/s       |
| Protokoll som stöds       | SSTP och IPsec            | IPsec                                 | Direktanslutning, VLAN      |
| Routning                   | RouteBased (dynamisk)      | PolicyBased (statisk) och RouteBased   | BGP                           |
| Anslutningsåterhämtning     | Aktiv-passiv            | Aktiv-passiv eller aktiv-aktiv       | Aktiv-aktiv                 |
| Användningsfall                  | Testning och prototyper   | Utveckling, testning och småskalig produktion  | Enterprise-/verksamhetskritiskt   |

### <a name="gateway-skus"></a>Gateway-SKU:er

Azure erbjuder följande SKU:er för gatewaytjänster:

| SKU              |  S2S/nätverk-till-nätverk-tunnlar | P2S-anslutningar  |  Prestandamått för aggregerat dataflöde   | Använd för                         |
| -------------    | -------------             | -------------    | ---------                         | ---------                       |
| Basic            | Max 10                    | Max 128          | 100 Mbit/s                          | Dev/test/POC                    |
| VpnGw1           | Max 30                    | Max 128          | 650 Mbit/s                          | Produktion/kritiska arbetsbelastningar   |
| VpnGw2           | Max 30                    | Max 128          | 1 Gbit/s                            | Produktion/kritiska arbetsbelastningar   |
| VpnGw3           | Max 30                    | Max 128          | 1,25 Gbit/s                          | Produktion/kritiska arbetsbelastningar   |

> [!Note]
> Det är viktigt att du väljer rätt SKU. Om du har konfigurerat din VPN-gateway med fel SKU måste du ta bort gatewayen och återskapa den, vilket kan vara tidskrävande.

## <a name="workflow"></a>Arbetsflöde

När du utformar en strategi för molnanslutning med ett virtuellt privat nätverk i Azure bör du använda följande arbetsflöde:

1. Utforma din anslutningstopologi och skapa en lista över adressutrymmena för alla anslutningsnätverk.

1. Skapa ett virtuellt Azure-nätverk.

1. Skapa en VPN-gateway för det virtuella nätverket.

1. Skapa och konfigurera anslutningar till lokala nätverk eller andra virtuella nätverk som krävs.

1. Om det behövs skapar och konfigurerar du en punkt till plats-anslutning för din Azure VPN-gateway.

### <a name="design-considerations"></a>Designöverväganden

När du utformar dina VPN-gatewayer för att ansluta virtuella nätverk behöver du tänka på följande faktorer:

- Undernät får inte överlappa varandra

    Det är mycket viktigt att ett undernät på en plats inte innehåller samma adressutrymme som på en annan plats.

- IP-adresser måste vara unika

    Du kan inte ha två värdar med samma IP-adress på olika platser eftersom det blir omöjligt att dirigera trafik mellan dessa två värdar, och nätverk-till-nätverk-anslutningen kan inte upprättas.

- VPN-gatewayer behöver ett gatewayundernät med namnet **GatewaySubnet**

    Undernätet måste ha det namnet för att gatewayen ska fungera, och det får inte innehålla några andra resurser.

### <a name="create-an-azure-virtual-network"></a>Skapa ett virtuellt Azure-nätverk

Innan du skapar en VPN-gateway måste du skapa det virtuella Azure-nätverket.

### <a name="create-a-vpn-gateway"></a>Skapa en VPN-gateway

Typen av VPN-gateway som du skapar beror på din arkitektur. Alternativen är:

- Routningsbaserad

    Routningsbaserade VPN-enheter använder trafikväljare för ”any-to-any” (joker) och låter tabeller för routning/vidarebefordran dirigera trafik till olika IPsec-tunnlar. Routningsbaserade anslutningar bygger vanligtvis på routeplattformar där varje IPsec-tunnel modelleras som ett nätverksgränssnitt eller ett VTI (virtuellt tunnelgränssnitt).

- Principbaserad

    Principbaserade VPN-enheter använder kombinationerna av prefix från båda nätverken för att definiera hur trafik krypteras/avkrypteras via IPsec-tunnlar. En principbaserad anslutning bygger vanligtvis på brandväggsenheter som utför paketfiltrering. Kryptering och avkryptering av IPsec-tunnlar läggs till i motorn för paketfiltrering och bearbetning.

## <a name="set-up-a-vpn-gateway"></a>Konfigurera en VPN-gateway

Vilka åtgärder du behöver vidta beror på vilken typ av VPN-gateway du installerar. För att skapa en punkt till plats-VPN-gateway med hjälp av Azure Portal skulle du exempelvis göra följande:

1. Skapa ett virtuellt nätverk

2. Lägga till ett gatewayundernät

3. Ange en DNS-server (valfritt)

4. Skapa en virtuell nätverksgateway

5. Generera certifikat

6. Lägga till klientadresspoolen

7. Konfigurera tunneltyp

8. Konfigurera autentiseringstyp

9. Ladda upp offentliga certifikatdata från rotcertifikatet

10. Installera ett exporterat klientcertifikat

11. Skapa och installera VPN-klientkonfigurationspaketet

12. Ansluta till Azure

Eftersom det finns flera konfigurationsvägar med Azure VPN-gatewayer, där alla har flera alternativ, går det inte att ta upp alla konfigurationer i den här kursen. Mer information finns i avsnittet Ytterligare resurser.

## <a name="configure-the-gateway"></a>Konfigurera gatewayen

När du har skapat din gateway behöver du konfigurera den.  Det finns flera konfigurationsinställningar du behöver ange, till exempel namn, plats, DNS-server osv. Vi går igenom dessa i detalj i övningen.

## <a name="summary"></a>Sammanfattning

En Azure VPN-gateway är en komponent i virtuella Azure-nätverk som gör det möjligt att aktivera punkt till plats-, plats till plats- eller nätverk till nätverk-anslutningar. Med Azure VPN-gatewayer är det möjligt att aktivera enskilda klientdatorer för att ansluta till resurser i Azure, utöka lokala nätverk till Azure eller underlätta anslutningar mellan virtuella nätverk i olika regioner och prenumerationer.
