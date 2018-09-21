Hög tillgänglighet (HA) garanterar att din arkitektur kan hantera fel. Anta att du är ansvarig för ett system som måste alltid vara helt fungerande. Fel kan och kommer att inträffa, så hur garanterar du att systemet kan vara kvar online när något går fel? Hur använder jag underhåll utan tjänstavbrott? 

Här får du lära dig behovet av hög tillgänglighet, utvärdera krav på hög tillgänglighet för program och hur Azure-plattformen hanterar dina tillgänglighetsmål.

## <a name="what-is-high-availability"></a>Vad är hög tillgänglighet?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvc]

En tjänst med hög tillgänglighet är en tjänst som absorberar variationer i tillgänglighet, belastning och tillfälliga fel i beroende tjänster och maskinvara. Programmet är online och tillgängligt hela tiden (eller ser till att det ser ut som det) samtidigt som det fungerar acceptabelt. Tillgängligheten definieras ofta av affärskrav, servicenivåmål eller servicenivåavtal.

Hög tillgänglighet handlar främst om förmågan att hantera förlust eller allvarlig försämring av en komponent i ett system. Det kan bero på att en virtuell dator som är värd för ett program försätts i offlineläge eftersom värden misslyckades. Det kan vara på grund av planerat underhåll av en systemuppgradering. Det kan även bero på fel i en tjänst i molnet. Om du identifierar de platser där ditt system kan misslyckas och skapar funktioner för att hantera sådana fel kan tjänsterna du erbjuder dina kunder förbli online.

Hög tillgänglighet för en tjänst kräver normalt hög tillgänglighet för de komponenter som utgör tjänsten. Tänk på en webbplats som erbjuder en onlinemarknadsplats för inköp av objekt. Tjänsten som erbjuds till dina kunder är möjligheten att lista, köpa och sälja saker online. För att tillhandahålla den här tjänsten har du flera komponenter: en databas, webbservrar, programservrar och så vidare. Var och en av dessa komponenter kan misslyckas, så du behöver identifiera hur och var dina felpunkter är och bestämma hur du ska åtgärda dessa felpunkter i arkitekturen.

## <a name="evaluate-high-availability-for-your-architecture"></a>Utvärdera hög tillgänglighet för din arkitektur

Utvärderingen av ett program för hög tillgänglighet sker i tre steg:

1. Fastställa serviceavtalet för ditt program
1. Utvärdera programmets HA-funktion
1. Utvärdera beroende programs HA-funktion

Vi ska titta lite närmare på stegen.

### <a name="determine-the-service-level-agreement-of-your-application"></a>Fastställa serviceavtalet för ditt program

Ett serviceavtal (SLA) är ett avtal mellan en tjänsteleverantör och en tjänstanvändare där tjänstleverantören förbinder sig till en tjänststandard baserat på mätbara mått och angivna ansvarsområden. Serviceavtal kan vara strikta, juridiskt bundna, avtalsenliga avtal eller förutsatta förväntningar på tillgänglighet av kunder. Tjänstmått fokuserar vanligen på tjänstens dataflöde, kapacitet och tillgänglighet, vilket alltihop kan mätas på olika sätt. Oavsett de specifika mätvärdena som utgör serviceavtalet kan underlåtenhet att uppfylla serviceavtalet få allvarliga ekonomiska påföljder för tjänstleverantören. En vanlig komponent i serviceavtal är garanterad ekonomisk kompensation om serviceavtalet inte uppfylls.

Servicenivåmål är värdena för de målmått som används för att mäta prestanda, tillförlitlighet och tillgänglighet. Detta kan vara mått som definierar prestanda för behandling av begäranden i millisekunder, tillgängligheten för tjänster på bara några minuter per månad eller antalet begäranden som bearbetas per timme. Du kan definiera de godkända och oacceptabla intervall för dessa enkla utloggningar genom att utvärdera de mått som exponeras av ditt program och förstå vilka kunderna använder som ett mått på kvalitet. Genom att definiera dessa mål kan ange du tydliga mål och förväntningar för båda teamen så att de kan stödja de tjänster och kunder som förbrukar dessa tjänster. Dessa enkla utloggningar används för att fastställa om ditt övergripande serviceavtal är uppfyllt.

I följande tabell visas den potentiella totala avbrottstiden för olika nivåer i serviceavtalet. 

| SLA | Nedtid per vecka | Nedtid per månad | Nedtid per år |
| --- | --- | --- | --- |
| 99 % |1,68 timmar |7,2 timmar |3,65 dagar |
| 99,9 % |10,1 minuter |43,2 minuter |8,76 timmar |
| 99,95 % |5 minuter |21,6 minuter |4,38 timmar |
| 99,99 % |1,01 minuter |4,32 minuter |52,56 minuter |
| 99,999 % |6 sekunder |25,9 sekunder |5,26 minuter |

Naturligtvis är en högre tillgänglighet bättre om allt annat är lika. Men i jakten på fler 9:or så ökar kostnaden och komplexiteten med att uppnå den höga tillgängligheten. En drifttid på 99,99 % innebär cirka 5 minuter total nedtid per månad. Är det värt den extra komplexiteten och kostnaden för att nå fem 9:or? Svaret beror på affärskraven. 

Här följer några andra saker att tänka på när du definierar ett SLA:

* För att uppnå fyra 9:or (99,99 %) kan du förmodligen inte förlita dig på manuella åtgärder efter fel. Programmet måste själv kunna diagnosticera och återställa interna fel. 
* Bortom fyra 9:or är det svårt att identifiera avbrott tillräckligt snabbt för att uppfylla serviceavtalet.
* Tänk på vilket tidsfönster som serviceavtalet mäts mot. Ju mindre fönster desto snävare tolerans. Det är förmodligen inte så bra att definiera serviceavtalet i termer av drifttid per timme eller dag. 

Att identifiera serviceavtal är ett viktigt första steg vid beslut om vilka funktioner med hög tillgänglighet som din arkitektur kräver. De hjälper till att utforma metoderna du ska använda för att göra programmet mer tillgängligt.

### <a name="evaluate-the-ha-capabilities-of-the-application"></a>Utvärdera programmets HA-funktion

Om du vill utvärdera funktionerna för hög tillgänglighet i ditt program utför du en felanalys. Fokusera på felkritiska systemdelar och kritiska komponenter som kan ha en stor inverkan på programmet om de inte nås, är felkonfigurerade eller börjar bete sig oväntat. För områden som har redundans ska du fastställa om programmet kan identifiera feltillstånd och självåterställning.

Du måste noggrant utvärdera alla komponenter i programmet, däribland delarna som är utformade för att tillhandahålla funktioner för hög tillgänglighet, som lastbalanserare. Felkritiska systemdelar måste antingen ändras för att få inbyggda funktioner för hög tillgänglighet eller ersättas med tjänster som kan erbjuda detta.

### <a name="evaluate-the-ha-capabilities-of-dependent-applications"></a>Utvärdera beroende programs HA-funktion

Du måste inte enbart förstå ditt programs serviceavtalskrav på konsumenten, utan även serviceavtalen som tillhandahålls av en resurs som ditt program kan bygga på. Om du lovar kunderna en drifttid på 99,9 procent, men en tjänst som ditt program är beroende av endast har en upptidsöverenskommelse på 99 procent, kan det innebära att du riskerar att inte uppfylla ditt servicenivåavtal till kunderna. Om en beroende tjänst inte har ett tillräckligt serviceavtal kan du behöva ändra dina egna serviceavtal, ersätta beroendet med ett alternativ eller hitta sätt att uppfylla serviceavtalet medan beroendet är otillgängligt. Beroende på scenariot och typ av beroende kan beroenden som misslyckas lösas tillfälligt med lösningar som cacheminnen och arbetsköer.

## <a name="azures-highly-available-platform"></a>Azures plattform med hög tillgänglighet

Azure-molnplattformen har utformats för att tillhandahålla hög tillgänglighet i alla tjänster. Programmen kan påverkas av händelser för både maskinvaru- och programvaruplattformar precis som i alla andra system. Behovet av att utforma din programarkitektur för att hantera fel är viktigt, och Azure-molnplattformen ger dig verktyg och funktioner som ger ditt program hög tillgänglighet. Det finns flera grundläggande begrepp när du överväger hög tillgänglighet för din arkitektur på Azure:

* Tillgänglighetsuppsättningar
* Tillgänglighetszoner
* Belastningsutjämning
* PaaS HA-funktioner (Platform as a Service)

### <a name="availability-sets"></a>Tillgänglighetsuppsättningar

Tillgänglighetsuppsättningar är ett sätt för dig att informera virtuella Azure-datorer om att virtuella datorer som tillhör samma programbelastning ska distribueras för att förhindra samtidig påverkan från maskinvarufel och schemalagt underhåll. Tillgänglighetsuppsättningar består av *uppdateringsdomäner* och *feldomäner*.

![En bild som visar tre tillgänglighetsuppsättningar. Den första uppsättningen har en uppdateringsdomän, den andra har två uppdateringsdomäner och den tredje saknar uppdateringsdomän.](../media/AzAvailSets.png)

Uppdateringsdomäner ser till att en delmängd av programmets servrar alltid körs när den virtuella datorn finns i ett Azure-datacenter som kräver driftstopp vid underhåll. De flesta uppdateringarna kan utföras utan att påverka till de virtuella datorerna som körs på dem, men det finns tillfällen när det inte är möjligt. För att se till att uppdateringar inte sker för ett helt datacenter i taget delas Azure-datacentret upp logiskt i uppdateringsdomäner (UD). När en underhållshändelse som en prestandauppdatering sker och en viktig säkerhetskorrigering krävs som ska tillämpas på värden sekventeras uppdateringen via uppdateringsdomäner. Användningen av sekvensering av uppdateringar med uppdateringsdomäner ser till att hela datacentrat inte blir otillgängligt under plattformsuppdateringar och -korrigeringar.

Medan uppdateringsdomäner representerar en logisk del av datacentret representerar feldomäner (FD) fysiska avsnitt av datacentret och garanterar en mångfald av rack för servrar i en tillgänglighetsuppsättning. Feldomäner justeras till den fysiska separationen av delad maskinvara i datacentret. Det inkluderar kraft, kylning och nätverksmaskinvara som stöder fysiska servrar som finns i serverrack. Om maskinvara som stöder ett serverrack har blivit otillgänglig påverkas endast det här serverracket av felet. Genom att placera dina virtuella datorer i en tillgänglighetsuppsättning, sprids dina virtuella datorer automatiskt över flera feldomäner så att endast en del av de virtuella datorerna påverkas om ett maskinvarufel skulle uppstå, något som dock sällan inträffar.

Med tillgänglighetsuppsättningar kan du se till att programmet fortsätter att vara online om en händelse med stor inverkan krävs eller om ett maskinvarufel uppstår.

### <a name="availability-zones"></a>Tillgänglighetszoner

Tillgänglighetszoner är oberoende fysiska datacenterplatser inom en region som har egen kraft, kylning och nätverkstjänster. Om du tar hänsyn till tillgänglighetszoner när du distribuerar resurser kan du skydda arbetsbelastningar mot datacenteravbrott och behålla närvaron i en viss region. Tjänster som virtuella datorer är *zonindelade*, vilket gör att du kan distribuera dem till to specifika zoner inom en region. Andra tjänster är *zonredundanta* och replikeras mellan tillgänglighetszoner i den specifika Azure-regionen. Båda typerna ser till att det inte finns några felkritiska systemdelar inom en Azure-region.

![En bild som visar tre tillgänglighetszoner inom en Azure-region. Varje tillgänglighetszon har en egen uppsättning resurser och är anslutna till varandra för att skapa zonredundanta tjänster.](../media/AzAvailZones.png)

Regioner som stöds innehåller minst tre tillgänglighetszoner. När du skapar zonindelade resurser i de här regionerna har du möjlighet att välja zonen där resursen ska skapas. På så sätt kan du utforma ditt program så att det kan hantera ett zonindelat avbrott och fortsätta att fungera i en Azure-region innan du måste evakuera programmet till en annan Azure-region.

Tillgänglighetszoner är en nyare konfigurationstjänst för hög tillgänglighet för Azure-regioner och är tillgängliga för vissa regioner. Det är viktigt att kontrollera tillgängligheten för den här tjänsten i regionen där du tänker distribuera ditt program om du vill överväga den här funktionen. Tillgänglighetszoner stöds när man använder virtuella datorer och flera PaaS-tjänster. Tillgänglighetszoner är ömsesidigt uteslutande med tillgänglighetsuppsättningar. När du använder tillgänglighetszoner behöver du inte längre definiera någon tillgänglighetsuppsättning för dina system. Du kommer att ha mångfald på datacenternivå och uppdateringarna kommer aldrig att föras ut till flera tillgänglighetszoner på samma gång.

### <a name="load-balancing"></a>Belastningsutjämning

Belastningsutjämnare hanterar hur nätverkstrafiken distribueras i ett program. Lastbalanserare är viktiga för att hålla programmet motståndskraftigt mot enskilda komponentfel och se till att programmet är tillgängligt för att bearbeta begäranden. För program som inte har inbyggd tjänstidentifiering krävs belastningsutjämning för både tillgänglighetsuppsättningar och tillgänglighetszoner.

Azure har tre tjänster för belastningsutjämning som skiljer sig åt i hur de dirigerar nätverkstrafik:

* **Azure Traffic Manager** erbjuder global DNS-belastningsutjämning. Du bör överväga att använda Traffic Manager för att erbjuda belastningsutjämning av DNS-slutpunkter i eller mellan Azure-regioner. Trafikhanteraren kommer att distribuera begäranden till tillgängliga slutpunkter och använda slutpunktsövervakning för att identifiera och ta bort misslyckade slutpunkter från arbetsbelastningen.
* **Azure Application Gateway** tillhandahåller Layer 7-belastningsutjämningsfunktioner, till exempel resursallokeringsdistribution av inkommande trafik, cookiebaserad sessionstillhörighet, URL-sökvägsbaserad routning och möjligheten att vara värd för flera webbplatser bakom en enda Application Gateway. Som standard övervakar Application Gateway hälsotillståndet för alla resurser i dess serverdelpool och tar automatiskt bort alla resurser som anses felaktiga från poolen. Application Gateway fortsätter att övervaka de felaktiga instanserna och lägger till dem i den felfria serverdelpoolen när de är tillgängliga och svarar på hälsokontroller.
* **Azure Load Balancer** är en Layer 4-lastbalanserare. Du kan konfigurera offentliga och interna belastningsutjämnade slutpunkter och definiera regler för att mappa inkommande anslutningar till serverdelspooldestinationer med TCP- och HTTP-avsökningsalternativ för att hantera tjänstens tillgänglighet.

Med en eller en kombination av alla tre teknikerna för Azure-belastningsutjämning får du de tillgängliga alternativ som krävs för att skapa en lösning med hög tillgänglighet för att dirigera nätverkstrafik i programmet.

![Alternativ för belastningsutjämning i Azure. En illustration som visar de olika belastningsutjämningsteknikerna i Azure. Trafikhanteraren balanserar belastningen mellan två regioner. Det finns en Application Gateway i varje region som distribuerar belastningen mellan olika virtuella datorer på webbnivån baserat på typen av begäran. Alla begäranden om avbildningar går till avbildningsserverpoolen och andra begäranden skickas till standardserverpoolen. Ytterligare begäranden som kommer från standardserverpoolerna hanteras av Azure-belastningsutjämnaren som fördelar dem mellan de virtuella datorerna på databasnivån.](../media/AzLBOptions.png)

### <a name="paas-ha-capabilities"></a>PaaS HA-funktioner

PaaS-tjänster levereras med inbyggd hög tillgänglighet. Tjänster som Azure SQL Database, Azure App Service och Azure Service Bus innefattar funktioner för hög tillgänglighet och garanterar att fel på en enskild komponent av tjänsten inte blir märkbara i programmet. Ett av de bästa sätten att se till att din arkitektur har hög tillgänglighet är att använda PaaS-tjänster.

När du bygger för hög tillgänglighet behöver du förstå serviceavtalet du upprättar med dina kunder. Utvärdera sedan både HA-funktionerna som programmet har och HA-funktionerna och serviceavtalen för beroende system. När de har upprättats kan du använda Azure-funktioner som tillgänglighetsuppsättningar, tillgänglighetszoner och olika tekniker för belastningsutjämning för att lägga till HA-funktioner i ditt program. Oavsett vilka PaaS-tjänster du väljer att använda har de inbyggda HA-funktioner.
