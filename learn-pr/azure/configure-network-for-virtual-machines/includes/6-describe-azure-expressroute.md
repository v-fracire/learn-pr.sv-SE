Eftersom ditt företag hanterar mycket känsliga data och har stora mängder information som ska lagras i Azure finns det viss oro över säkerheten och tillförlitligheten för anslutningar via det offentliga internet. Företaget är inte villigt att migrera helt till Azure, såvida lösningen inte kan uppvisa en högre anslutningsnivå, högre säkerhet och större tillförlitlighet.

Här går vi längre än anslutningar som körs via det offentliga Internet till dedikerade anslutningar direkt till Azures datacenter.

## <a name="azure-expressroute"></a>Azure ExpressRoute

Microsoft Azure ExpressRoute låter organisationer utöka sina lokala nätverk till Microsoft Cloud över en privat anslutning som implementerats av en anslutningsleverantör. Det här arrangemanget innebär att anslutningen till Azures datacenter inte går via Internet utan över en dedikerad länk. ExpressRoute effektiviserar även anslutningar med andra molnbaserade tjänster från Microsoft, till exempel Office 365 och Dynamics 365.

Fördelarna med ExpressRoute är bland annat:

- Snabbare hastighet, från 50 Mbit/s till 10 Gbit/s, med dynamisk bandbreddsskalning

- Kortare svarstider

- Större tillförlitlighet trots inbyggd peering

- Hög säkerhet

ExpressRoute har ytterligare ett antal fördelar, till exempel:

- Anslutning till alla Azure tjänster som stöds

- Global anslutning till alla regioner (kräver premium-tillägg)

- Dynamisk routning över Border Gateway Protocol

- Serviceavtal (SLA) för oavbruten anslutningstid

- Tjänstkvalitet (QoS) för Skype för företag

Dessutom finns också premiumtillägget ExpressRoute, som ger fördelar som ökade routningsgränser, global tjänstanslutning och ett större antal vNet-länkar per krets.

## <a name="expressroute-connectivity-models"></a>ExpressRoute-anslutningsmodeller

Anslutningar till ExpressRoute kan ske genom följande metoder:

- P VPN-nätverk (”any-to-any”)

- Virtuell korsanslutning via ett Ethernet-utbyte

- Ethernet-anslutning av punkt till punkt-typ

 Alla ExpressRoute-funktioner och egenskaper är identiska i de ovanstående anslutningsmodellerna.

### <a name="what-is-layer-3-connectivity"></a>Vad är Layer 3-anslutning?

Microsoft använder ett branschstandardprotokoll för dynamisk routning (BGP) för att växla vägar mellan det lokala nätverket, dina instanser i Azure och Microsofts offentliga adresser. Vi upprättar flera BGP-sessioner med ditt nätverk för olika trafikprofiler.

### <a name="any-to-any-ipvpn-networks"></a>”Any-to-any”-nätverk (IPVPN)

IPVPN-leverantörer tillhandahåller vanligtvis anslutning mellan olika avdelningskontor och dina företagsdatacenter över hanterade Layer 3-anslutningar. Med ExpressRoute visas Azures datacenter som om de vore separata avdelningskontor.

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a>Virtuell korsanslutning via ett Ethernet-utbyte

Om din organisation är samordnad med en molnutbytesfunktion begär du korsanslutningar till Microsoft Cloud genom din leverantörs Ethernet-utbyte. Korsanslutningarna till Microsoft Cloud kan antingen köras med hanterade Layer 2- eller Layer 3-anslutningar, som i OSI-nätverksmodellen.

### <a name="point-to-point-ethernet-connection"></a>Ethernet-anslutning av punkt till punkt-typ

Ethernet-länkar av punkt till punkt-typ kan ge hanterade Layer 2- eller Layer 3-anslutningar mellan dina lokala datacenter eller kontor till Microsoft Cloud.

## <a name="how-expressroute-works"></a>Så här fungerar ExpressRoute

Azure ExpressRoute använder en kombination av ExpressRoute-kretsar och routningsdomäner för snabba anslutningar till Microsoft Cloud.

### <a name="what-are-expressroute-circuits"></a>Vad är ExpressRoute-kretsar?

En ExpressRoute-krets är den logiska anslutningen mellan din lokala infrastruktur och Microsoft Cloud. En anslutningsleverantör implementerar anslutningen, även om vissa organisationer använder flera anslutningsleverantörer av redundansskäl. Varje krets har en fast bandbredd på 50, 100, 200 eller 500 Mbit/s alternativt 1 eller 10 Gbit/s och var och en av dessa kretsar mappas till en anslutningsleverantör och en peering-plats. Dessutom har varje ExpressRoute-krets standardkvoter och begränsningar.

En ExpressRoute-krets motsvarar inte en nätverksanslutning eller en nätverksenhet. Varje krets definieras av ett GUID som kallas en _tjänst_- eller _s-nyckel_. Den här s-nyckeln tillhandahåller anslutningslänken mellan Microsoft, din anslutningsleverantör och organisation – det är inte en kryptografisk hemlighet. Varje s-nyckel har en en-till-en-mappning till en Azure ExpressRoute-krets.

Varje krets kan ha upp till tre peering-sessioner, som är ett par BGP-sessioner som har konfigurerats för redundans. De är:

- Azure, privat
- Azure, offentlig
- Microsoft

### <a name="routing-domains"></a>Routningsdomäner

ExpressRoute-kretsarna mappar sedan till routningsdomäner, där varje ExpressRoute-krets har flera routningsdomäner. Dessa domäner är desamma som de tre peering-sessionerna som anges ovan. I en aktiv-aktiv-konfiguration skulle varje routerpar ha en identiskt konfigurerad routningsdomän, vilket ger hög tillgänglighet. De offentliga och privata Azure-peering-namnen representerar IP-adresscheman.

#### <a name="azure-private-peering"></a>Privat peering i Azure

Azures privata peering ansluter till Azures beräkningstjänster, till exempel virtuella datorer och molntjänster som distribueras med ett virtuellt nätverk. När det handlar om säkerhet är den privata peeringdomänen helt enkelt en förlängning av ditt lokala nätverk till Azure. Du aktiverar sedan dubbelriktad anslutning mellan nätverket och valfritt virtuellt Azure-nätverk, vilket gör att Azure VM-IP-adresserna blir synliga i ditt interna nätverk.

> [!NOTE]
> Du kan bara ansluta ett enda virtuellt nätverk till den privata peering-domänen.

#### <a name="azure-public-peering"></a>Offentlig peering i Azure

Azures offentliga peering gör det möjligt att ha privata anslutningar till tjänster som är tillgängliga via offentliga IP-adresser, som Azure Storage, Azure SQL-databaser och webbtjänster i Azure. Med offentlig peering kan du ansluta till de offentliga IP-adresserna för tjänsten utan att trafiken dirigeras via Internet. Anslutningen är alltid från ditt WAN-nätverk till Azure, inte tvärtom. Detta är också en allt-eller-inget-metod eftersom du inte kan välja vilka tjänster som du vill aktivera offentlig peering för.

> [!NOTE]
> För Azure PaaS-tjänster rekommenderar vi Microsoft-peering i stället för offentlig peering.

#### <a name="microsoft-peering"></a>Microsoft-peering

Microsoft-peering har stöd för anslutningar till molnbaserade SaaS-erbjudanden, som Office 365 och Dynamics 365. Det här peeringalternativet ger dubbelriktad anslutning mellan ditt företags WAN-nätverk och Microsofts molntjänster.

### <a name="expressroute-health"></a>ExpressRoute-hälsa

Precis som med de flesta funktioner i Microsoft Azure kan du övervaka ExpressRoute-anslutningar för att säkerställa att de fungerar på ett tillfredsställande sätt. Övervakning innefattar täckning av följande områden:

- Tillgänglighet
- Anslutning till virtuella nätverk
- Nyttjande av bandbredd

Det viktigaste verktyget för den här övervakningsaktiviteten är Övervakare av nätverksprestanda, i synnerhet övervakaren för ExpressRoute.

## <a name="summary"></a>Sammanfattning

Med Azure ExpressRoute kan du skapa privata anslutningar mellan Azures datacenter och infrastruktur som finns lokalt eller i en samplaceringsmiljö. ExpressRoute-anslutningar upprättas inte via offentligt internet, och de är tillförlitligare, snabbare och har kortare svarstider och högre säkerhet än vanliga anslutningar över internet.
