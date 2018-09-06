Grattis, du har slutfört modulen om prestanda och skalbarhet. Vi tar och sammanfattar vad du har lärt dig:

## <a name="scaling-up-and-scaling-out"></a>Att skala upp och skala ut

* Skillnaden mellan att skala upp/ned (nivån på resursen som etableras på en instans) och skala in/ut (öka eller minska antalet tillgängliga instanser). Vi har även gått igenom exempel i Azure för varje alternativ
* Saker att tänka på vid in- och utskalning för tillståndshantering och programstarttider
* Autoskalning, och hur det kan hjälpa dig att skala antalet instanser utifrån ett schema eller ett programs behov
* Vi har gått igenom alternativa tekniker som kan underlätta skalningen, till exempel serverlösa tekniker och containrar.

## <a name="optimize-network-performance"></a>Optimera nätverksprestanda

* Nätverksfördröjning är ett mått som mäter fördröjning på data som skickas från en avsändare till en mottagare.
* I en molnmiljö kan ett program på chattnivå prestera sämre än i en lokal miljö eftersom resurserna inte längre samordnas omedelbart.
* Genom att samordna API:er nära en skrivningsslutpunkt för en databas kan du tillhandahålla prestandafördelar som minskar nätverksfördröjningen mellan de två resurserna
* Med Azure Traffic Manager kan du dirigera användare till den distribuerade instansen med lägst nätverksfördröjning.
* Azure Content Delivery nätverk (CDN) kan användas för att avlasta beräkning från huvudprogrammet och påskynda inläsningen av program genom cachelagring av innehåll på en CDN-gränsnod nära en användare.

## <a name="optimize-storage-performance"></a>Optimera lagringsprestanda

* Det finns tre typer av diskar som är tillgängliga för IaaS-distributioner (Infrastruktur som en tjänst). Standard HDD-diskar (inkonsekventa svarstider och lägre dataflödesnivåer), Standard SSD-diskar (konsekventa svarstider och lägre dataflödesnivåer) och Premium SSD-diskar (konsekventa svarstider och höga dataflödesnivåer).
* Cachelagring kan användas i programlagret för att förbättra ett programs inläsningstider. Information som begärs ofta kan lagras i ett cacheminne framför databasen, som i sin tur kan optimera för datainläsning av den mest efterfrågade informationen.
* Du bör tänka på att använda korrekt datalager för jobbet i serverdelen (flerspråkig persistens) när du skapar din lösning.

## <a name="identify-performance-bottlenecks"></a>Identifiera prestandaflaskhalsar

* Det är viktigt att förstå förväntningarna på programmet innan du utformar arkitekturen eller skapar åtgärder
* Förstå hur effektiva DevOps-strategier kan hjälpa dig att bygga mer robusta och högpresterande program
* Vi har även sammanfattat de övervakningsalternativ som är tillgängliga i Azure, inklusive Azure Monitor (ett fönster för övervakning i Azure), Azure Log Analytics (datainmatning för loggar och IaaS-övervakning) och Application Insights (prestandaövervakning av program, inklusive tillgänglighet, prestanda och information om undantagshändelser).