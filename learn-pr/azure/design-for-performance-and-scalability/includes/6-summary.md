<span data-ttu-id="37dcf-101">Grattis, du har slutfört modulen om prestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="37dcf-101">Congratulations, you have completed the module on Performance and Scalability.</span></span> <span data-ttu-id="37dcf-102">Vi tar och sammanfattar vad du har lärt dig:</span><span class="sxs-lookup"><span data-stu-id="37dcf-102">Let's recap what we have covered:</span></span>

## <a name="scaling-up-and-scaling-out"></a><span data-ttu-id="37dcf-103">Att skala upp och skala ut</span><span class="sxs-lookup"><span data-stu-id="37dcf-103">Scaling up and scaling out</span></span>

- <span data-ttu-id="37dcf-104">Skillnaden mellan att skala upp/ned (nivån på resursen som etableras på en instans) och skala in/ut (öka eller minska antalet tillgängliga instanser).</span><span class="sxs-lookup"><span data-stu-id="37dcf-104">The difference between scaling up/down (the level of resource provisioned on an instance) and scaling in/out (increasing or decreasing the number of available instances).</span></span> <span data-ttu-id="37dcf-105">Vi har även gått igenom exempel i Azure för varje alternativ.</span><span class="sxs-lookup"><span data-stu-id="37dcf-105">We also discussed examples in Azure of each.</span></span>
- <span data-ttu-id="37dcf-106">Saker att tänka på vid in- och utskalning för tillståndshantering och programstarttider.</span><span class="sxs-lookup"><span data-stu-id="37dcf-106">The considerations to be made when scaling in/out around state management and application startup times.</span></span>
- <span data-ttu-id="37dcf-107">Autoskalning, och hur det kan hjälpa dig att skala antalet instanser utifrån ett schema eller ett programs behov.</span><span class="sxs-lookup"><span data-stu-id="37dcf-107">Autoscale, and how it can help you scale the number of instances based on a schedule or the demand of an application.</span></span>
- <span data-ttu-id="37dcf-108">Vi har gått igenom alternativa tekniker som kan underlätta skalningen, till exempel serverlösa tekniker och containrar.</span><span class="sxs-lookup"><span data-stu-id="37dcf-108">Discussed alternate technologies that may help with scalability, including serverless and containers.</span></span>

## <a name="optimize-network-performance"></a><span data-ttu-id="37dcf-109">Optimera nätverksprestanda</span><span class="sxs-lookup"><span data-stu-id="37dcf-109">Optimize network performance</span></span>

- <span data-ttu-id="37dcf-110">Nätverksfördröjning är ett mått som mäter fördröjning på data som skickas från en avsändare till en mottagare.</span><span class="sxs-lookup"><span data-stu-id="37dcf-110">Network latency is a measure in delay of data being sent from a sender to a receiver.</span></span>
- <span data-ttu-id="37dcf-111">I en molnmiljö kan ett program på chattnivå prestera sämre än i en lokal miljö eftersom resurserna inte längre samordnas omedelbart.</span><span class="sxs-lookup"><span data-stu-id="37dcf-111">In a cloud environment, chattier applications may see a performance impact compared to on-premises as resources are no longer immediately co-located.</span></span>
- <span data-ttu-id="37dcf-112">Genom att samordna API:er nära en skrivningsslutpunkt för en databas kan du tillhandahålla prestandafördelar som minskar nätverksfördröjningen mellan de två resurserna.</span><span class="sxs-lookup"><span data-stu-id="37dcf-112">Co-locating APIs near to a database write endpoint could provide a performance benefit, as we are reducing the network latency between the two resources.</span></span>
- <span data-ttu-id="37dcf-113">Med Azure Traffic Manager kan du dirigera användare till den distribuerade instansen med lägst nätverksfördröjning.</span><span class="sxs-lookup"><span data-stu-id="37dcf-113">Azure Traffic Manager could be used to route users to the deployed instance with the lowest network latency.</span></span>
- <span data-ttu-id="37dcf-114">Azure Content Delivery nätverk (CDN) kan användas för att avlasta beräkning från huvudprogrammet och påskynda inläsningen av program genom cachelagring av innehåll på en CDN-gränsnod nära en användare.</span><span class="sxs-lookup"><span data-stu-id="37dcf-114">Azure Content Delivery Networks (CDN) could be used to offload compute from the main application and speed up application load times by caching content on a CDN edge node near to a user.</span></span>

## <a name="optimize-storage-performance"></a><span data-ttu-id="37dcf-115">Optimera lagringsprestanda</span><span class="sxs-lookup"><span data-stu-id="37dcf-115">Optimize storage performance</span></span>

- <span data-ttu-id="37dcf-116">Det finns tre typer av diskar som är tillgängliga för IaaS-distributioner (Infrastruktur som en tjänst).</span><span class="sxs-lookup"><span data-stu-id="37dcf-116">There are three main types of disks available for infrastructure as a service (IaaS) deployments.</span></span> <span data-ttu-id="37dcf-117">Standard HDD-diskar (inkonsekventa svarstider och lägre dataflödesnivåer), Standard SSD-diskar (konsekventa svarstider och lägre dataflödesnivåer) och Premium SSD-diskar (konsekventa svarstider och höga dataflödesnivåer).</span><span class="sxs-lookup"><span data-stu-id="37dcf-117">Standard HDD disks (inconsistent latency and lower levels of throughput), Standard SSD disks (consistent latency and lower levels of throughput), and Premium SSD disks (consistent latency and high levels of throughput).</span></span>
- <span data-ttu-id="37dcf-118">Cachelagring kan användas i programlagret för att förbättra ett programs inläsningstider.</span><span class="sxs-lookup"><span data-stu-id="37dcf-118">Caching could be used in the application layer to improve the load times of an application.</span></span> <span data-ttu-id="37dcf-119">Information som begärs ofta kan lagras i ett cacheminne framför databasen, som i sin tur kan optimera för datainläsning av den mest efterfrågade informationen.</span><span class="sxs-lookup"><span data-stu-id="37dcf-119">Frequently requested information could be stored in a cache in front of the database, which could then optimize for data load times of the most requested information.</span></span>
- <span data-ttu-id="37dcf-120">Du bör tänka på att använda korrekt datalager för jobbet i serverdelen (flerspråkig persistens) när du skapar din lösning.</span><span class="sxs-lookup"><span data-stu-id="37dcf-120">Using the appropriate back-end data store for the job (polyglot persistence) should be considered when building out your solution.</span></span>

## <a name="identify-performance-bottlenecks"></a><span data-ttu-id="37dcf-121">Identifiera prestandaflaskhalsar</span><span class="sxs-lookup"><span data-stu-id="37dcf-121">Identify performance bottlenecks</span></span>

- <span data-ttu-id="37dcf-122">Det är viktigt att förstå förväntningarna på programmet innan du utformar arkitekturen eller skapar åtgärder.</span><span class="sxs-lookup"><span data-stu-id="37dcf-122">Importance of understanding the expectations of the application before architecting or building any operations.</span></span>
- <span data-ttu-id="37dcf-123">Förstå hur effektiva DevOps-strategier kan hjälpa dig att bygga mer robusta och högpresterande program.</span><span class="sxs-lookup"><span data-stu-id="37dcf-123">Understanding how effective DevOps strategies can help build more robust and well-performing applications.</span></span>
- <span data-ttu-id="37dcf-124">Vi har även sammanfattat de övervakningsalternativ som är tillgängliga i Azure, inklusive Azure Monitor (ett fönster för övervakning i Azure), Azure Log Analytics (datainmatning för loggar och IaaS-övervakning) och Application Insights (prestandaövervakning av program, inklusive tillgänglighet, prestanda och information om undantagshändelser).</span><span class="sxs-lookup"><span data-stu-id="37dcf-124">Summarized the monitoring options available in Azure, including Azure Monitor (pane of glass for monitoring on Azure), Azure Log Analytics (log ingestion and IaaS monitoring), and Application Insights (application performance monitoring including availability, performance, and exception information).</span></span>