<span data-ttu-id="83cd5-101">När du skapar ett program vill du ge en bättre användarupplevelse och en del av detta är snabb datahämtning.</span><span class="sxs-lookup"><span data-stu-id="83cd5-101">When building an application, you want to provide a great user experience, and a part of that is quick data retrieval.</span></span> <span data-ttu-id="83cd5-102">Att hämta data från en databas tar vanligtvis lång tid och om dessa data läses ofta kan detta göra att användarupplevelsen försämras.</span><span class="sxs-lookup"><span data-stu-id="83cd5-102">Retrieving data from a database is typically a slow process, and if this data is read often, this could provide a poor user experience.</span></span> <span data-ttu-id="83cd5-103">Cache-aside-mönstret beskriver hur du kan implementera ett cacheminne tillsammans med en databas för att returnera de data som oftast används så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="83cd5-103">The cache-aside pattern describes how you can implement a cache in conjunction with a database, to return the most commonly accessed data as quickly as possible.</span></span>

<span data-ttu-id="83cd5-104">Här kan lära du dig hur cache-aside-mönstret kan användas för att säkerställa att du snabbt kan komma åt dina viktiga data.</span><span class="sxs-lookup"><span data-stu-id="83cd5-104">Here, you'll learn how the cache-aside pattern can be used to ensure your important data is quickly accessible.</span></span>

## <a name="what-is-the-cache-aside-pattern"></a><span data-ttu-id="83cd5-105">Vad är ett cache aside-mönster?</span><span class="sxs-lookup"><span data-stu-id="83cd5-105">What is the cache-aside pattern?</span></span>

<span data-ttu-id="83cd5-106">Cache-aside-mönstret dikterar att när man behöver hämta data från en datakälla, exempelvis en relationsdatabas, bör man först kontrollera om det finns data i det egna cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="83cd5-106">The cache-aside pattern dictates that when you need to retrieve data from a data source, like a relational database, you should first check for the data in your cache.</span></span> <span data-ttu-id="83cd5-107">Använd de eventuella data som finns i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="83cd5-107">If the data is in your cache, use it.</span></span> <span data-ttu-id="83cd5-108">Om data inte finns i ditt cacheminne frågar du databasen och när du returnerar data tillbaka till användaren lägger du till dem i ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="83cd5-108">If the data is not in your cache, then query the database, and when you're returning the data back to the user, add it to your cache.</span></span> <span data-ttu-id="83cd5-109">Detta gör sedan att du kan komma åt data från ditt cacheminne nästa gång de behövs.</span><span class="sxs-lookup"><span data-stu-id="83cd5-109">This will then allow you to access the data from your cache the next time it's needed.</span></span>

## <a name="when-to-implement-the-cache-aside-pattern"></a><span data-ttu-id="83cd5-110">När ska du implementera cache-aside-mönstret?</span><span class="sxs-lookup"><span data-stu-id="83cd5-110">When to implement the cache-aside pattern?</span></span>

<span data-ttu-id="83cd5-111">Läsning av data från en databas tar vanligtvis lång tid.</span><span class="sxs-lookup"><span data-stu-id="83cd5-111">Reading data from a database is usually a slow process.</span></span> <span data-ttu-id="83cd5-112">Det omfattar kompilering av en komplex fråga, förberedelse av en plan för frågekörning, körning av frågan och sedan förberedelse av resultatet.</span><span class="sxs-lookup"><span data-stu-id="83cd5-112">It involves compilation of a complex query, preparation of a query execution plan, execution of the query, and then preparation of the result.</span></span> <span data-ttu-id="83cd5-113">I vissa fall kan den här processen även läsa data från disken.</span><span class="sxs-lookup"><span data-stu-id="83cd5-113">In some cases, this process may read data from the disk as well.</span></span> <span data-ttu-id="83cd5-114">Det finns optimeringar som kan göras på databasnivå, exempelvis att sammanställa frågorna i förväg, och läsa in en del data i minnet.</span><span class="sxs-lookup"><span data-stu-id="83cd5-114">There are optimizations that can be done on the database level, like pre-compiling the queries, and loading some of the data in memory.</span></span> <span data-ttu-id="83cd5-115">Körningen av frågan och förberedelsen av resultatet inträffar emellertid alltid när data hämtas från en databas.</span><span class="sxs-lookup"><span data-stu-id="83cd5-115">However, execution of the query and preparation of the result will always happen when retrieving data from a database.</span></span>

<span data-ttu-id="83cd5-116">Vi kan lösa problemet med hjälp av cache-aside-mönstret.</span><span class="sxs-lookup"><span data-stu-id="83cd5-116">We can solve this problem by using the cache-aside pattern.</span></span> <span data-ttu-id="83cd5-117">Vi har fortfarande programmet och databasen i cache-aside-mönstret, men nu har vi även en cache.</span><span class="sxs-lookup"><span data-stu-id="83cd5-117">In the cache-aside pattern, we still have the application and the database, but now we also have a cache.</span></span> <span data-ttu-id="83cd5-118">En cache lagrar data i minnet så att den inte behöver interagera med filsystemet.</span><span class="sxs-lookup"><span data-stu-id="83cd5-118">A cache stores its data in memory, so it doesn't have to interact with the file system.</span></span> <span data-ttu-id="83cd5-119">Cacheminnen lagrar också data i mycket enkla datastrukturer, som nyckelvärdespar, så att de inte behöver köra komplexa frågor för att samla in data eller underhålla index när du skriver data.</span><span class="sxs-lookup"><span data-stu-id="83cd5-119">Caches also store data in very simple data structures, like key value pairs, so they don't have to execute complex queries to gather data or maintain indexes when writing data.</span></span> <span data-ttu-id="83cd5-120">Ett cacheminne är därför vanligtvis bättre än en databas.</span><span class="sxs-lookup"><span data-stu-id="83cd5-120">Because of this, a cache is typically more performant than a database.</span></span> <span data-ttu-id="83cd5-121">När du använder ett program görs ett försök att läsa data från cachen först.</span><span class="sxs-lookup"><span data-stu-id="83cd5-121">When you use an application, it will try to read data from the cache first.</span></span> <span data-ttu-id="83cd5-122">Om begärda data inte finns i cacheminnet hämtar programmet den från databasen, som den alltid har gjort.</span><span class="sxs-lookup"><span data-stu-id="83cd5-122">If the requested data is not in the cache, the application will retrieve it from the database, like it always has done.</span></span> <span data-ttu-id="83cd5-123">Därefter lagras dock data i cacheminnet för efterföljande begäranden.</span><span class="sxs-lookup"><span data-stu-id="83cd5-123">However, then it stores the data in the cache for subsequent requests.</span></span> <span data-ttu-id="83cd5-124">Nästa gång en användare begär data returneras de från cachen direkt.</span><span class="sxs-lookup"><span data-stu-id="83cd5-124">Next time when any user requests the data, it will return it from the cache directly.</span></span>

![Diagram för att läsa in data som ska cachelagras](../media/8-cache-aside-set-cache.png)

### <a name="how-to-manage-updating-data"></a><span data-ttu-id="83cd5-126">Så här hanterar du datauppdateringar</span><span class="sxs-lookup"><span data-stu-id="83cd5-126">How to manage updating data</span></span>

<span data-ttu-id="83cd5-127">När du implementerar mönstret cache-aside uppstår ett litet problem.</span><span class="sxs-lookup"><span data-stu-id="83cd5-127">When you implement the cache-aside pattern, you introduce a small problem.</span></span> <span data-ttu-id="83cd5-128">Eftersom dina data nu lagras nu i ett cacheminne och ett datalager kan du stöta på problem när du försöker göra en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="83cd5-128">Since your data is now stored in a cache and a data store, you can run into problems when you try to make an update.</span></span> <span data-ttu-id="83cd5-129">Om du vill uppdatera dina data, skulle du exempelvis behöva uppdatera både cacheminnet och datalagret.</span><span class="sxs-lookup"><span data-stu-id="83cd5-129">For example, to update your data, you would need to update both the cache and the data store.</span></span>

<span data-ttu-id="83cd5-130">Lösning på problemet i cache-aside-mönstret är att ogiltigförklara data i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="83cd5-130">The solution to this problem in the cache-aside pattern is to invalidate the data in the cache.</span></span> <span data-ttu-id="83cd5-131">När du uppdaterar data i ditt program bör du först ta bort data i cacheminnet och sedan göra ändringarna i datakällan direkt.</span><span class="sxs-lookup"><span data-stu-id="83cd5-131">When you update data in your application, you should first delete the data in the cache and then make the changes to the data source directly.</span></span> <span data-ttu-id="83cd5-132">Om du gör det kommer den inte att finnas i cacheminnet nästa gång data begärs, och processen upprepar sig.</span><span class="sxs-lookup"><span data-stu-id="83cd5-132">By doing this, next time the data is requested, it won't be present in the cache, and the process will repeat.</span></span> 

![Diagram över att ogiltigförklara cachelagrade data](../media/8-cache-aside-invalidate.png)

## <a name="considerations-for-using-the-cache-aside-pattern"></a><span data-ttu-id="83cd5-134">Att tänka på när cache-aside-mönstret används</span><span class="sxs-lookup"><span data-stu-id="83cd5-134">Considerations for using the cache-aside pattern</span></span>

<span data-ttu-id="83cd5-135">Överväg noga vilka data som vi ska placera i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="83cd5-135">Carefully consider which data to put in the cache.</span></span> <span data-ttu-id="83cd5-136">Alla data lämpar sig inte för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="83cd5-136">Not all data is suitable to be cached.</span></span>

- <span data-ttu-id="83cd5-137">**Omfång**: För att cache-aside ska vara effektivt måste du kontrollera att förfallopolicyn matchar åtkomstfrekvensen för data.</span><span class="sxs-lookup"><span data-stu-id="83cd5-137">**Lifetime**: For cache-aside to be effective, make sure that the expiration policy matches the access frequency of the data.</span></span> <span data-ttu-id="83cd5-138">Gör inte förfalloperioden för kort, då det kan leda till att programmen hämtar data regelbundet från datalagret och lägger till dem i cachen.</span><span class="sxs-lookup"><span data-stu-id="83cd5-138">Making the expiration period too short can cause applications to continually retrieve data from the data store and add it to the cache.</span></span>

- <span data-ttu-id="83cd5-139">**Borttagning**: Cacheminnen har en begränsad storlek jämfört med typiska datalager och de tar bort data när det behövs.</span><span class="sxs-lookup"><span data-stu-id="83cd5-139">**Evicting**: Caches have a limited size compared to typical data stores, and they'll evict data if necessary.</span></span> <span data-ttu-id="83cd5-140">Se till att välja en lämplig borttagningsprincip för dina data.</span><span class="sxs-lookup"><span data-stu-id="83cd5-140">Make sure you choose an appropriate eviction policy for your data.</span></span>

- <span data-ttu-id="83cd5-141">**Inställningar**: I syfte att göra cache aside-mönstret effektivt kommer många lösningar att fylla i cachen med data som bedöms kommer till användning ofta.</span><span class="sxs-lookup"><span data-stu-id="83cd5-141">**Priming**: To make the cache-aside pattern effective, many solutions will prepopulate the cache with data that they think will be accessed often.</span></span>

- <span data-ttu-id="83cd5-142">**Konsekvens**: Konsekvens mellan datalagret och cachen garanteras inte av att cache-aside-mönstret implementeras.</span><span class="sxs-lookup"><span data-stu-id="83cd5-142">**Consistency**: Implementing the cache-aside pattern doesn't guarantee consistency between the data store and the cache.</span></span> <span data-ttu-id="83cd5-143">Data i ett datalager kan ändras utan att cachen meddelas.</span><span class="sxs-lookup"><span data-stu-id="83cd5-143">Data in a data store can be changed without notifying the cache.</span></span> <span data-ttu-id="83cd5-144">Detta kan leda till allvarliga synkroniseringsproblem.</span><span class="sxs-lookup"><span data-stu-id="83cd5-144">This can lead to serious synchronization issues.</span></span>

<span data-ttu-id="83cd5-145">Cache-aside-mönstret är användbart när man måste komma åt data ofta från en datakälla som använder en disk.</span><span class="sxs-lookup"><span data-stu-id="83cd5-145">The cache-aside pattern is useful when you're required to access data frequently from a data source that uses a disk.</span></span> <span data-ttu-id="83cd5-146">Med hjälp av cache-aside-mönstret kommer du att lagra viktiga data i ett cacheminne för att öka hämtningshastigheten.</span><span class="sxs-lookup"><span data-stu-id="83cd5-146">Using the cache-aside pattern, you'll store important data in a cache to help increase the speed of retrieving it.</span></span> 