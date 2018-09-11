Hög tillgänglighet (HA) garanterar att din arkitektur kan hantera fel. Anta att du är ansvarig för ett system som måste alltid vara helt fungerande. Fel kan och kommer att inträffa, så hur garanterar du att systemet kan vara kvar online när något går fel? Hur hanterar du underhållshändelser? 

Här får du lära dig behovet av hög tillgänglighet, utvärdera krav på hög tillgänglighet för program och hur Azure-plattformen hanterar och erbjuder lösningar för att uppfylla dina tillgänglighetsmål.

## <a name="what-is-high-availability"></a>Vad är hög tillgänglighet?

En tjänst med hög tillgänglighet är ett program som absorberar variationer i tillgänglighet, belastning och tillfälliga fel i beroende tjänster och maskinvara. Programmet är online och tillgängligt hela tiden (eller ser till att det ser ut som det) samtidigt som det fungerar acceptabelt. Tillgänglighet definieras ofta av affärskrav eller programmets serviceavtal.

Hög tillgänglighet handlar främst om förmågan att hantera förlust av eller allvarlig försämring av en komponent i ett system. Det kan bero på att en virtuell dator som är värd för ett program försätts i offlineläge eftersom värden misslyckades. Det kan vara på grund av planerat underhåll av en systemuppgradering. Det kan även bero på fel i en tjänst i molnet. Om du identifierar de platser där ditt system kan misslyckas och skapar funktioner för att hantera sådana fel kan tjänsterna du erbjuder dina kunder förbli online.

Hög tillgänglighet för en tjänst kräver normalt hög tillgänglighet för de komponenter som utgör tjänsten. Tänk på en webbplats som erbjuder en onlinemarknadsplats för inköp av objekt. Tjänsten som erbjuds till dina kunder är möjligheten att lista, köpa och sälja saker online. För att tillhandahålla den här tjänsten har du flera komponenter: en databas, webbservrar, programservrar och så vidare. Var och en av dessa komponenter kan misslyckas, så du behöver identifiera hur och var dina felpunkter är och bestämma hur du ska åtgärda dessa felpunkter i arkitekturen.

## <a name="evaluate-high-availability-for-your-architecture"></a>Utvärdera hög tillgänglighet för din arkitektur

Utvärderingen av ett program för hög tillgänglighet sker i tre steg: 

1. Fastställa serviceavtalet för ditt program
1. Utvärdera programmets HA-funktion
1. Utvärdera beroende programs HA-funktion

Vi ska titta lite närmare på stegen.

### <a name="determine-the-service-level-agreement-of-your-application"></a>Fastställa serviceavtalet för ditt program

Ett serviceavtal (SLA) är ett avtal mellan en tjänsteleverantör och en tjänstanvändare där tjänstleverantören förbinder sig till en tjänststandard baserat på mätbara mått och angivna ansvarsområden. Serviceavtal kan vara strikta, juridiskt bundna, avtalsenliga avtal eller förutsatta förväntningar på tillgänglighet av kunder. Tjänstmått fokuserar vanligen på tjänstens dataflöde, kapacitet och tillgänglighet, vilket alltihop kan mätas på olika sätt. Oavsett de specifika mätvärdena som utgör serviceavtalet kan underlåtenhet att uppfylla serviceavtalet få allvarliga ekonomiska påföljder för tjänstleverantören. En vanlig komponent av serviceavtal är garanterad ekonomisk återbetalning för serviceavtal som inte uppfylls.

![SLA-handskakning](../media-draft/SLAHandshake.png)

Att identifiera serviceavtal är ett viktigt första steg vid beslut om vilka funktioner med hög tillgänglighet din arkitektur kräver. De hjälper till att utforma metoderna du ska använda för att göra programmet mer tillgängligt.

### <a name="evaluate-the-ha-capabilities-of-the-application"></a>Utvärdera programmets HA-funktion

Om du vill utvärdera funktionerna för hög tillgänglighet i ditt program utför du en felanalys. Fokusera på felkritiska systemdelar och kritiska komponenter som kan ha en stor inverkan på programmet om de inte nås, är felkonfigurerade eller börjar bete sig oväntat. För områden som har redundans ska du fastställa om programmet kan identifiera feltillstånd och självåterställning.

Du måste noggrant utvärdera alla komponenter i programmet, däribland delarna som är utformade för att tillhandahålla funktioner för hög tillgänglighet, som lastbalanserare. Felkritiska systemdelar måste antingen ändras för att få inbyggda funktioner för hög tillgänglighet eller ersättas med tjänster som kan erbjuda detta.

### <a name="evaluate-the-ha-capabilities-of-dependent-applications"></a>Utvärdera beroende programs HA-funktion

Du måste inte enbart förstå ditt programs serviceavtalskrav på konsumenten, utan även serviceavtalen som tillhandahålls av en resurs ditt program förlitar sig på. Om du lovar kunderna en drifttid på 99,.9 %, men en tjänst som ditt program är beroende av endast har ett SLA på 99 % kan det innebära att du riskerar att inte uppfylla ditt SLA till kunderna. Om en beroende tjänst inte har tillräckligt med serviceavtal kan du behöva ändra dina egna serviceavtal, ersätta beroendet med ett alternativ eller hitta sätt att uppfylla serviceavtalet medan beroendet är otillgängligt. Beroende på scenariot och typ av beroende kan beroenden som misslyckas lösas tillfälligt med lösningar som cacheminnen och arbetsköer.

## <a name="azures-highly-available-platform"></a>Azures plattform med hög tillgänglighet

Azure-molnplattformen har utformats för att tillhandahålla hög tillgänglighet i alla tjänster. Programmen kan påverkas av händelser för både maskinvaru- och programvaruplattformar precis som i alla andra system. Behovet av att utforma din programarkitektur för att hantera fel är viktigt, och Azure-molnplattformen ger dig verktyg och funktioner som ger ditt program hög tillgänglighet. Det finns flera grundläggande begrepp när du överväger hög tillgänglighet för din arkitektur på Azure:

* Tillgänglighetsuppsättningar
* Tillgänglighetszoner
* Belastningsutjämning
* PaaS HA-funktioner

### <a name="availability-sets"></a>Tillgänglighetsuppsättningar

Tillgänglighetsuppsättningar är ett sätt för dig att informera virtuella Azure-datorer om att virtuella datorer som tillhör samma programbelastning ska distribueras för att förhindra samtidig påverkan från maskinvarufel och schemalagt underhåll. Tillgänglighetsuppsättningar består av *uppdateringsdomäner* och *feldomäner*.

![tillgänglighetsuppsättningar](../media-draft/AzAvailSets.png)

Uppdateringsdomäner ser till att en delmängd av programmets servrar alltid körs när den virtuella datorn finns i ett Azure-datacenter som kräver driftstopp vid underhåll. De flesta uppdateringarna kan utföras utan att påverka till de virtuella datorerna som körs på dem, men det finns tillfällen när det inte är möjligt. För att se till att uppdateringar inte sker för ett helt datacenter i taget delas Azure-datacentret upp logiskt i uppdateringsdomäner (UD). När en underhållshändelse som en prestandauppdatering sker och en viktig säkerhetskorrigering krävs som ska tillämpas på värden sekventeras uppdateringen via uppdateringsdomäner. Användningen av sekvensering av uppdateringar med uppdateringsdomäner ser till att hela datacenter inte blir otillgängligt under plattformsuppdateringar och -korrigeringar.

Medan uppdateringsdomäner representerar en logisk del av datacentret representerar feldomäner (FD) fysiska avsnitt av datacentret, vilket garanterar en mångfald av rack för servrar i en tillgänglighetsuppsättning. Feldomäner justeras till den fysiska separationen av delad maskinvara i datacentret. Det inkluderar kraft, kylning och nätverksmaskinvara som stöder fysiska servrar som finns i serverrack. Om maskinvara som stöder ett serverrack har blivit otillgänglig påverkas endast det serverracket av felet.

Med tillgänglighetsuppsättningar kan du se till att programmet fortsätter att vara online om en händelse med stor inverkan krävs eller om ett maskinvarufel uppstår.

### <a name="availability-zones"></a>Tillgänglighetszoner

Tillgänglighetszoner är oberoende fysiska datacenterplatser inom en region som har egen kraft, kylning och nätverkstjänster. Om du tar hänsyn till tillgänglighetszoner när du distribuerar resurser kan du skydda arbetsbelastningar mot datacenteravbrott och behålla närvaron i en viss region. Tjänster som virtuella datorer är *zonindelade*, vilket gör att du kan distribuera dem till to specifika zoner inom en region. Andra tjänster är *zonredundanta* och replikeras mellan tillgänglighetszoner i den specifika Azure-regionen. Båda typerna ser till att det inte finns några felkritiska systemdelar inom en Azure-region.

![tillgänglighetszoner](../media-draft/AzAvailZones.png)

Regioner som stöds innehåller minst tre tillgänglighetszoner. När du skapar zonindelade resurser i de här regionerna har du möjlighet att välja zonen där resursen ska skapas. På så sätt kan du utforma ditt program så att det kan hantera ett zonindelat avbrott och fortsätta att fungera i en Azure-region innan du måste evakuera programmet till en annan Azure-region.

Tillgänglighetszoner är en nyare konfigurationstjänst för hög tillgänglighet för Azure-regioner och är tillgängliga för vissa regioner. Det är viktigt att kontrollera tillgängligheten för den här tjänsten i regionen där du tänker distribuera ditt program om du vill överväga den här funktionen. Tillgänglighetszoner stöds när du använder virtuella datorer och flera PaaS-tjänster. Tillgänglighetszoner ersätter tillgänglighetsuppsättningar i regioner som stöds.

### <a name="load-balancing"></a>Belastningsutjämning

Belastningsutjämnare hanterar hur nätverkstrafiken distribueras i ett program. Lastbalanserare är viktiga för att hålla programmet motståndskraftigt mot enskilda komponentfel och att programmet är tillgängligt för att bearbeta begäranden. För program som inte har inbyggd tjänstidentifiering krävs belastningsutjämning för både tillgänglighetsuppsättningar och tillgänglighetszoner.

Azure har tre tjänster för belastningsutjämning som skiljer sig åt i hur de dirigerar nätverkstrafik:

* **Azure Traffic Manager** erbjuder global DNS-belastningsutjämning. Du bör överväga att använda Traffic Manager för att erbjuda belastningsutjämning av DNS-slutpunkter i eller mellan Azure-regioner.
* **Azure Application Gateway** tillhandahåller layer 7-belastningsutjämningsfunktioner, till exempel resursallokeringsdistribution av inkommande trafik, cookiebaserad sessionstillhörighet, URL-sökvägsbaserad routning och möjligheten att vara värd för flera webbplatser bakom en enda Application Gateway.
* **Azure Load Balancer** är en layer 4-lastbalanserare. Du kan konfigurera offentliga och interna belastningsutjämnade slutpunkter och definiera regler för att mappa inkommande anslutningar till serverdelspooldestinationer med TCP- och HTTP-avsökningsalternativ för att hantera tjänstens tillgänglighet.

Med en eller en kombination av alla tre teknikerna för Azure-belastningsutjämning får du de tillgängliga alternativ som krävs för att skapa en lösning med hög tillgänglighet för att dirigera nätverkstrafik i programmet.

![Skärmbild](../media-draft/AzLBOptions.png)

### <a name="paas-ha-capabilities"></a>PaaS HA-funktioner

PaaS-tjänster levereras med inbyggd hög tillgänglighet. Tjänster som Azure SQL Database, Azure App Service och Azure Service Bus innefattar funktioner för hög tillgänglighet, vilket garanterar att fel på en enskild komponent av tjänsten inte blir märkbar i programmet. Ett av de bästa sätten att se till att din arkitektur har hög tillgänglighet är att använda PaaS-tjänster.

När du utformar för hög tillgänglighet kommer du att förstå serviceavtalet du checkar in till dina kunder. Utvärdera sedan båda HA-funktionerna som programmet har, och HA-funktionerna och serviceavtalen för beroende system. När de har upprättats kan du använda Azure-funktioner som tillgänglighetsuppsättningar, tillgänglighetszoner och olika tekniker för belastningsutjämning för att lägga till HA-funktioner i ditt program. Oavsett vilka PaaS-tjänster du väljer att använda har de inbyggda HA-funktioner.
