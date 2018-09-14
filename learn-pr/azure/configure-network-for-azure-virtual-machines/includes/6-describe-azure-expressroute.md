Eftersom företaget hanterar mycket känsliga data och har stora mängder information som det lagras i Azure, finns det vissa orolig för säkerheten och pålitligheten för anslutningar via det offentliga Internet. Företaget är villigt att migrera grossist till Azure, såvida inte kan visa högre nivåer av anslutning, säkerhet och tillförlitlighet.

Här kan ska vi gå bortom anslutningar som kör via Internet till dedikerade linjer som är direkt i Azure-datacenter.

## <a name="azure-expressroute"></a>Azure ExpressRoute

Microsoft Azure ExpressRoute kan organisationer utöka sina lokala nätverk till Microsoft Cloud via en privat anslutning som implementeras av en anslutningsprovider. Den här ordningen innebär att anslutningen till Azure-Datacenter inte går via Internet men över en dedikerad länk. ExpressRoute underlättar också effektiva anslutningar med andra molnbaserade tjänster från Microsoft, till exempel Office 365 och Dynamics 365.

Fördelarna med ExpressRoute är bland annat:

- Snabbare hastighet, från 50 Mbit/s till 10 Gbit/s, med dynamisk bandbreddsskalning

- Kortare svarstider

- Större tillförlitlighet via inbyggd peering

- Hög säkerhet

ExpressRoute har ytterligare ett antal fördelar, till exempel:

- Anslutning till alla Azure tjänster som stöds

- Global anslutning till alla regioner (kräver premium-tillägg)

- Dynamisk routning över Border Gateway Protocol

- Servicenivåavtal (SLA) för anslutning upptid

- Tjänstkvalitet (QoS) för Skype för företag

Dessutom är ExpressRoute premium-tillägget, vilket ger fördelar som högre väggränser, global tjänst-anslutning och ökad vnet-länkar per krets.

## <a name="expressroute-connectivity-models"></a>ExpressRoute-anslutningsmodeller

Anslutningar till ExpressRoute kan ske genom följande metoder:

- P VPN-nätverk (”any-to-any”)

- Virtuell korsanslutning via ett Ethernet-utbyte

- Ethernet-anslutning av punkt till punkt-typ

 Alla ExpressRoute-funktioner och egenskaper är identiska i de ovanstående anslutningsmodellerna.

### <a name="what-is-layer-3-connectivity"></a>Vad är Layer 3-anslutning?

Microsoft använder en branschstandard dynamisk routning protocol (BGP) att utbyta routning mellan ditt lokala nätverk, dina instanser i Azure och Microsoft offentliga adresser. Vi upprättar flera BGP-sessioner med ditt nätverk för olika trafikprofiler.

### <a name="any-to-any-ipvpn-networks"></a>”Any-to-any”-nätverk (IPVPN)

IPVPN-leverantörer tillhandahåller vanligtvis anslutning mellan olika avdelningskontor och ditt företags datacenter över hanterade layer 3-anslutningar. Med ExpressRoute kan visas i Azure-datacenter som om de har ett annat filialkontor.

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a>Virtuell korsanslutning via en Ethernet-utbyte

Om din organisation är samordnad med en cloud exchange kan begära du-anslutningar till Microsoft Cloud dock din provider Ethernet-utbyte. Dessa-anslutningar till Microsoft Cloud kan användas med nivå 2- eller layer 3 hanterade anslutningar, som i nätverk OSI-modellen.

### <a name="point-to-point-ethernet-connection"></a>Ethernet-anslutning av punkt till punkt-typ

Mellan punkter med Ethernet-länkar kan ge layer 2- eller Hanterade layer 3-anslutningar mellan ditt lokala Datacenter eller kontor till Microsoft Cloud.

## <a name="how-expressroute-works"></a>Så här fungerar ExpressRoute

Med ExpressRoute i Azure använder en kombination av ExpressRoute-kretsar och routningsdomäner för att tillhandahålla snabba anslutningar till Microsoft Cloud.

### <a name="what-are-expressroute-circuits"></a>Vad är ExpressRoute-kretsar

En ExpressRoute-krets är logiska anslutningen mellan din lokala infrastruktur och Microsoft Cloud. En anslutningsleverantör implementerar anslutningen, även om vissa organisationer använder flera anslutningsleverantörer av redundansskäl. Varje krets har en fast bandbredd på 50, 100, 200 Mbit/s eller 500 Mbit/s eller 1 Gbit/s eller 10 Gbit/s och var och en av dessa kretsar mappas till en anslutningsprovider och en peering-plats. Dessutom har varje ExpressRoute-krets standardkvoter och begränsningar.

Det är en ExpressRoute-krets som inte motsvarar en nätverksanslutning eller en nätverksenhet. Varje krets definieras av ett GUID som kallas en _service_ eller _s-key_. Nyckelns s innehåller länken anslutning mellan Microsoft och anslutningsleverantören organisationen – det är inte en kryptografisk hemlighet. Varje s-nyckel har en en-till-en-mappning till en Azure ExpressRoute-krets.

Varje krets kan ha upp till tre peerings, som är ett par med BGP-sessioner som har konfigurerats för redundans. De är:

- Azure, privat
- Azures offentliga
- Microsoft

### <a name="routing-domains"></a>Routningsdomäner

ExpressRoute-kretsar mappa till routningsdomäner, med varje ExpressRoute-krets som har flera routningsdomäner. Dessa domäner är desamma som de tre peerings som anges ovan. I en aktiv-aktiv-konfiguration skulle varje routerpar ha en identiskt konfigurerad routningsdomän, vilket ger hög tillgänglighet. Azure offentliga och privata peering namnen representerar den IP-adresser scheman.

#### <a name="azure-private-peering"></a>Azures privata peering

Azures privata peering ansluter till Azure-Beräkningstjänster, till exempel virtuella datorer och molntjänster som distribueras med ett virtuellt nätverk. När det handlar om säkerhet är den privata peeringdomänen helt enkelt en förlängning av ditt lokala nätverk till Azure. Du aktiverar sedan dubbelriktad anslutning mellan nätverket och valfritt virtuellt Azure-nätverk, vilket gör att Azure VM-IP-adresserna blir synliga i ditt interna nätverk.

> [!NOTE]
> Du kan ansluta endast ett virtuellt nätverk till privat peering domänen.

#### <a name="azure-public-peering"></a>Azures offentliga peering

Azures offentliga peering ger privata anslutningar till tjänster som är tillgängliga på den offentliga IP-adresser som Azure Storage, Azure SQL-databaser och Azure-webbtjänster. Med offentlig peering, kan du ansluta till de offentliga IP-adresserna för tjänsten utan att trafiken dirigeras via Internet. Anslutning sker alltid från ditt WAN-nätverk till Azure, inte tvärtom. Det är också en alternativ metod som du inte kan välja de tjänster som du vill offentlig peering aktiverad.

> [!NOTE]
> För Azure PaaS-tjänster rekommenderar vi för att använda Microsoft-peering i stället för offentlig peering.

#### <a name="microsoft-peering"></a>Microsoft-peering

Microsoft-peering har stöd för anslutningar till molnbaserade SaaS-erbjudanden, som Office 365 och Dynamics 365. Det här peeringalternativet ger dubbelriktad anslutning mellan ditt företags WAN-nätverk och Microsofts molntjänster.

### <a name="expressroute-health"></a>ExpressRoute-hälsa

Precis som med de flesta funktioner i Microsoft Azure kan du övervaka ExpressRoute-anslutningar för att säkerställa att de fungerar på ett tillfredsställande sätt. Övervakning innefattar täckning av följande områden:

- Tillgänglighet
- Anslutning till virtuella nätverk
- Nyttjande av bandbredd

Det viktigaste verktyget för den här övervakningsaktiviteten är Övervakare av nätverksprestanda, i synnerhet övervakaren för ExpressRoute.

## <a name="summary"></a>Sammanfattning

Med Azure ExpressRoute kan du skapa privata anslutningar mellan Azures datacenter och infrastruktur som finns lokalt eller i en samplaceringsmiljö. ExpressRoute-anslutningar går inte via offentliga Internet och de erbjuder mer tillförlitlighet, snabbare hastigheter och kortare svarstider än vanliga Internetanslutningar.
