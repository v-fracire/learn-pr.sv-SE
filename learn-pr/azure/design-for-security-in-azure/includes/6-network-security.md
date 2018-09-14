Att skydda ditt nätverk från attacker och obehörig åtkomst är en viktig del av alla arkitekturer. Som en del i att planera för sina molnmigrering, tog Lamna Healthcare sig tid att planera sina nätverksinfrastruktur för att säkerställa att de hade säkerhetskontrollerna som rätt nätverk på plats för att skydda sina nätverksinfrastruktur mot angrepp. Här tittar vi på hur nätverkssäkerhet ser ut, hur du integrerar en metod med flera lager i arkitekturen samt hur Azure kan hjälpa dig att få nätverkssäkerhet för din miljö.

## <a name="what-is-network-security"></a>Vad är nätverkssäkerhet

Nätverkssäkerhet är att skydda kommunikationen för resurser i och utanför ditt nätverk. Målet är att begränsa exponeringen på nätverkslagret över dina tjänster och system. Genom att begränsa exponeringen minskar du sannolikheten att dina resurser kan angripas. I fokus på nätverkssäkerhet kan arbetet fokusera på följande områden:

- Skydda trafikflödet mellan program och Internet
- Skydda trafikflödet mellan program
- Skydda trafikflödet mellan användare och programmet

Skydd av trafikflödet mellan program och Internet fokuserar på att begränsa exponeringen utanför nätverket. Nätverksattacker startar oftast utanför nätverket. Genom att begränsa exponeringen mot Internet och skydda perimetern kan du därför minska risken för angrepp.

Skydd av trafikflödet mellan program fokuserar på data mellan program, deras nivåer, mellan olika miljöer och andra tjänster i nätverket. Genom att begränsa exponering mellan dessa resurser minskar du inverkan som en komprometterad resurs kan få. Detta kan ytterligare minska spridningen inom ett nätverk.

Skydd av trafikflödet mellan användare och programmet fokuserar på att skydda nätverksflödet för dina slutanvändare. Detta begränsar resursernas exponering för attacker utifrån och ger ett säkert sätt för användare att utnyttja dina resurser. 

## <a name="a-layered-approach-to-network-security"></a>En metod för nätverkssäkerhet med flera lager

En gemensam tråd i den här modulen har varit att använda en säkerhetsmetod med flera lager, och den metoden fungerar på samma sätt vid nätverkslagret. Det räcker inte att bara fokusera på säkra perimeternätverket eller på nätverkssäkerheten mellan tjänster i ett nätverk. En metod med flera lager ger flera nivåer av skydd, så om en angripare tar sig igenom ett lager finns det ytterligare skydd på plats för att begränsa angreppet.

Vi går igenom hur Azure kan ge verktyg för en metod med flera lager för att säkra ditt nätverksfotavtryck.

### <a name="internet-protection"></a>Internetskydd

Om vi startar i utkanten av nätverket fokuserar vi på att begränsa och eliminera attacker från Internet. Ett bra område att börja med är att utvärdera de resurser som är riktade mot Internet och endast tillåta inkommande och utgående kommunikation där det finns behov. Identifiera alla resurser som tillåter inkommande nätverkstrafik oavsett typ och se till att de är nödvändiga och begränsade till endast de portar/protokoll som krävs. Azure Security Center är ett bra ställe att leta efter den här informationen eftersom det identifierar Internetinriktade resurser som inte har nätverkssäkerhetsgrupper (NSG) associerade med dem samt resurser som inte skyddas bakom en brandvägg.

Det finns flera sätt att ge inkommande skydd vid gränsen. Application Gateway är en Layer 7-lastbalanserar som även innehåller en brandvägg för webbaserade program (WAF) för att tillhandahålla avancerad säkerhet för dina HTTP-baserade tjänster. WAF är baserat på regler från OWASP-3.0 eller 2.2.9 core regeluppsättningar och skyddar mot vanliga kända säkerhetsproblem, till exempel cross site scripting och SQL-inmatning.

![Application Gateway med WAF](../media-draft/appgw-waf.png)

Virtuella nätverksinstallationer (NVA) kan användas för att skydda dina nätverksresurser för skydd av icke-HTTP-baserade tjänster eller för ökad anpassning. NVA:er liknar de brandväggsenheter som du kan hitta i lokala nätverk och är tillgängliga från många av de mest populära leverantörerna för nätverkssäkerhet. NVA:er kan ge större anpassning av säkerhet för program som kräver det, men kan innebära ökad komplexitet, så noggrant beaktande av kraven rekommenderas.

Alla resurser som är exponerade mot internet är risk för angrepp av en denial of service-attack. Dessa typer av attacker försöker överbelasta en nätverksresurs genom att skicka så många begäranden att resursen blir långsam eller slutar svarar. För att lösa dessa attacker, ger Azure DDoS grundläggande skydd över alla Azure-tjänster och förbättrat skydd för ytterligare anpassning för dina resurser. DDoS-skydd blockerar angreppstrafik och vidarebefordrar återstående trafik till dess avsedda destination. Inom några minuter efter att ett angrepp har identifierats meddelas du via Azure Monitor-mått.

![DDoS](../media-draft/ddos.png)

### <a name="virtual-network-security"></a>Säkerhet för virtuella nätverk

I ett virtuellt nätverk (VNet) är det viktigt att begränsa kommunikationen mellan resurser till det som krävs.

För kommunikation mellan virtuella datorer är nätverkssäkerhetsgrupper (NSG) en viktig del av att begränsa onödig kommunikation. NSG:er fungerar på lager 3 och 4 och ger en lista över tillåten respektive nekad kommunikation till och från nätverksgränssnitt och undernät. NSG:er är helt anpassningsbar och ger dig möjlighet att fullständigt låsa nätverkskommunikation till och från virtuella datorer. Med hjälp av NSG:er kan du isolera program mellan miljöer, nivåer och tjänster.

![Azure-nätverkssäkerhetsgrupper](../media-draft/azure-network-security.png)

Använd tjänstslutpunkter för virtuella nätverk för att isolera Azure-tjänster så att endast kommunikation från virtuella nätverk tillåts. Med tjänstslutpunkter kan Azure-tjänstresurser skyddas på ditt virtuella nätverk. Skydda tjänstresurser i ett virtuellt nätverk ger förbättrad säkerhet genom att helt ta bort den offentliga Internetåtkomsten till resurser, så att endast trafik från ditt virtuella nätverk. Detta minskar risken för angrepp för din miljö, minskar den administration som krävs för att begränsa kommunikationen mellan virtuella nätverk och Azure-tjänster samt tillhandahåller optimal routning för den här kommunikationen.

### <a name="network-integration"></a>Nätverksintegrering

Det är vanligt att ha en befintlig nätverksinfrastruktur som behöver integreras för att tillhandahålla kommunikation från lokala nätverk eller för att tillhandahålla förbättrad kommunikation mellan tjänster i Azure. Det finns några viktiga sätt att hantera den här integreringen och förbättra säkerheten för nätverket.

VPN-anslutningar (virtuellt privat nätverk) är ett vanligt sätt att upprätta säkra kommunikationskanaler mellan nätverk, och detta fungerar på samma när du arbetar med virtuella nätverk på Azure. Anslutning mellan virtuella Azure-nätverk och en lokal VPN-enhet är ett bra sätt att tillhandahålla säker kommunikation mellan ditt nätverk och dina virtuella datorer på Azure.

Du kan använda ExpressRoute för att tillhandahålla en dedikerad privat anslutning mellan ditt nätverk och Azure. Med ExpressRoute kan du utöka ditt lokala nätverk till Microsoft-molnet över en privat anslutning som tillhandahålls av en anslutningsprovider. Med ExpressRoute kan du upprätta anslutningar till Microsofts molntjänster, till exempel Microsoft Azure, Office 365 och Dynamics 365. Detta förbättrar säkerheten för din lokala kommunikation genom att skicka den här trafiken över den privata anslutningen i stället för via Internet. Du behöver inte tillåta åtkomst till dessa tjänster för dina slutanvändare via Internet och kan skicka den här trafiken via installation för ytterligare trafikkontroll.

![ExpressRoute](../media-draft/expressroute-connection-overview.png)

För att enkelt integrera flera virtuella nätverk i Azure etablerar VNet-peering en direkt anslutning mellan angivna virtuella nätverk. När detta har upprättats kan du använda NSG:er för att tillhandahålla isolering mellan resurser på samma sätt som du skyddar resurser inom ett virtuellt nätverk. Den här integreringen ger dig möjligheten att tillhandahålla samma grundläggande säkerhetslager över alla peerkopplade virtuella nätverk. Kommunikation tillåts endast mellan direktanslutna virtuella nätverk.

## <a name="network-security-at-lamna-healthcare"></a>Nätverkssäkerhet på Lamna Healthcare

Lamna Healthcare har använt många av dessa tjänster för att bygga ut en säker nätverksinfrastruktur. Kommunikation mellan resurser nekas som standard och tillåts endast när det krävs. Inkommande anslutningar från internet är bara aktiverat för tjänster som kräver det. RDP och SSH tillåts inte från internet-slutpunkter, endast från betrodda interna resurser.

För att skydda sina Internetinriktade webbtjänster placerar de dem bakom Application Gateways med WAF aktiverat. Detta gäller både för tjänster som körs på virtuella datorer samt på App Service. Genom att använda Application Gateways får de skydd mot många av de vanligaste sårbarheterna.

De har DDoS-standarden aktiverad för att skydda sina Internetinriktade slutpunkter från överbelastningsattacker.

Genom att använda NSG:er kan de helt isolera kommunikationen mellan programtjänster och mellan miljöer. De tillåter endast nödvändig kommunikation mellan tjänster i en miljö, och ingen åtkomst tillåts mellan produktions- och icke-produktionsmiljöer.

De har etablerat en ExpressRoute-krets med anslutning till sina lokala nätverk för att tillhandahålla dedikerade anslutningar mellan sina slutanvändare och program i Azure. Detta håller trafiken till Azure borta från Internet, och en privat anslutning till tjänsterna i Azure upprätthålls för att kommunicera med system som förblir lokala.

Med den här metoden har Lamna Healthcare dragit nytta av Azure-tjänster för att ge säkerhet på flera nivåer i sin nätverksinfrastruktur.

## <a name="summary"></a>Sammanfattning

En metod med flera lager för nätverkssäkerhet bidrar till att minska risken för exponering via nätverksbaserade attacker. Azure tillhandahåller flera tjänster och funktioner för att skydda dina Internetinriktade resurser, interna resurser och kommunikationen mellan lokala nätverk. De här funktionerna gör det möjligt att skapa säkra lösningar på Azure.