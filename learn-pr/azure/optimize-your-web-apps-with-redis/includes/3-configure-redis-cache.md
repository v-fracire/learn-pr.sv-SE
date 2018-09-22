<span data-ttu-id="b6fbc-101">Ditt utvecklingsteam för sportstatistik har kommit fram till att cachelagring avsevärt skulle kunna förbättra prestanda för nyligen begärda data.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-101">Your sports statistics development team has decided that caching could dramatically improve performance for recently requested data.</span></span> <span data-ttu-id="b6fbc-102">Nästa steg är att skapa och konfigurera en Azure Redis Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-102">Your next step is to create and configure an Azure Redis Cache instance.</span></span>

## <a name="create-and-configure-the-azure-redis-cache-instance"></a><span data-ttu-id="b6fbc-103">Skapa och konfigurera Azure Redis Cache-instansen</span><span class="sxs-lookup"><span data-stu-id="b6fbc-103">Create and configure the Azure Redis Cache instance</span></span>

<span data-ttu-id="b6fbc-104">Du kan skapa en Redis-cache med Azure-portalen, Azure CLI eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-104">You can create a Redis cache using the Azure portal, the Azure CLI, or Azure PowerShell.</span></span> <span data-ttu-id="b6fbc-105">Det finns flera parametrar som du måste bestämma för att konfigurera cachen korrekt för dina syften.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-105">There are several parameters you will need to decide in order to configure the cache properly for your purposes.</span></span>

### <a name="name"></a><span data-ttu-id="b6fbc-106">Namn</span><span class="sxs-lookup"><span data-stu-id="b6fbc-106">Name</span></span>

<span data-ttu-id="b6fbc-107">Redis-cachen behöver ett globalt unikt namn.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-107">The Redis cache will need a globally unique name.</span></span> <span data-ttu-id="b6fbc-108">Namnet måste vara unikt i Azure eftersom den används för att skapa en offentlig URL för att ansluta och kommunicera med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-108">The name has to be unique within Azure because it is used to generate a public-facing URL to connect and communicate with the service.</span></span>

<span data-ttu-id="b6fbc-109">Namnet måste vara mellan 1 och 63 tecken, innehålla siffror, bokstäver, och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-109">The name must be between 1 and 63 characters, composed of numbers, letters, and the '-' character.</span></span> <span data-ttu-id="b6fbc-110">Cachenamnet får inte inledas eller avslutas med tecknet - eller ha flera - i följd.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-110">The cache name can't start or end with the '-' character, and consecutive '-' characters aren't valid.</span></span>

### <a name="resource-group"></a><span data-ttu-id="b6fbc-111">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b6fbc-111">Resource Group</span></span>

<span data-ttu-id="b6fbc-112">Azure Redis Cache är en hanterad resurs och behöver en _resursgrupp_-ägare.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-112">The Azure Redis Cache is a managed resource and needs a _resource group_ owner.</span></span> <span data-ttu-id="b6fbc-113">Du kan antingen skapa en ny resursgrupp eller använda en befintlig en på en prenumeration du är del av.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-113">You can either create a new resource group, or use an existing one in a subscription you are part of.</span></span>

### <a name="location"></a><span data-ttu-id="b6fbc-114">Plats</span><span class="sxs-lookup"><span data-stu-id="b6fbc-114">Location</span></span>

<span data-ttu-id="b6fbc-115">Du måste bestämma var din Redis-cache kommer att finnas fysiskt genom att välja en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-115">You will need to decide where the Redis cache will be physically located by selecting an Azure region.</span></span> <span data-ttu-id="b6fbc-116">Du ska alltid placera din cacheinstans och ditt program i samma region.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-116">You should always place your cache instance and your application in the same region.</span></span> <span data-ttu-id="b6fbc-117">Om du ansluter till en cache i en annan region kan latensen öka och tillförlitligheten minska avsevärt.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-117">Connecting to a cache in a different region can significantly increase latency and reduce reliability.</span></span> <span data-ttu-id="b6fbc-118">Om du ansluter till cachen utanför Azure ska du välja en plats som ligger nära platsen där programmet som förbrukar data körs.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-118">If you are connecting to the cache outside of Azure, then select a location close to where the application consuming the data is running.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b6fbc-119">Placera din Redis-cache så nära data_konsumenten_ som möjligt.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-119">Put the Redis cache as close to the data _consumer_ as you can.</span></span>

### <a name="pricing-tier"></a><span data-ttu-id="b6fbc-120">Prisnivå</span><span class="sxs-lookup"><span data-stu-id="b6fbc-120">Pricing tier</span></span>

<span data-ttu-id="b6fbc-121">Som vi nämnde i den senaste enheten, finns det tre olika prisnivåer för Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-121">As mentioned in the last unit, there are three pricing tiers available for an Azure Redis Cache.</span></span>

- <span data-ttu-id="b6fbc-122">**Grundläggande**: grundläggande cachelagring är perfekt för utveckling/testning.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-122">**Basic**: Basic cache ideal for development/testing.</span></span> <span data-ttu-id="b6fbc-123">Är begränsad till en enda server, 53 GB minne och 20 000 anslutningar.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-123">Is limited to a single server, 53 GB of memory, and 20,000 connections.</span></span> <span data-ttu-id="b6fbc-124">Det finns inget serviceavtal för den här tjänstnivån.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-124">There is no SLA for this service tier.</span></span>
- <span data-ttu-id="b6fbc-125">**Standard**: cachelagring för produktion som stöder replikering och innehåller ett serviceavtal på 99,99 %.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-125">**Standard**: Production cache which supports replication and includes an 99.99% SLA.</span></span> <span data-ttu-id="b6fbc-126">Den stöder två servrar (över-/underordnad) och har samma minnes-/anslutningsbegränsningar som Basic-nivån.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-126">It supports two servers (master/slave), and has the same memory/connection limits as the Basic tier.</span></span>
- <span data-ttu-id="b6fbc-127">**Premium**: Enterprise-nivå som bygger på nivån Standard och har stöd för persistence, klustring och skala ut cache.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-127">**Premium**: Enterprise tier which builds on the Standard tier and includes persistence, clustering, and scale-out cache support.</span></span> <span data-ttu-id="b6fbc-128">Det här är den högsta prestandanivån med upp till 530 GB minne och 40 000 samtidiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-128">This is the highest performing tier with up to 530 GB of memory and 40,000 simultaneous connections.</span></span>

<span data-ttu-id="b6fbc-129">Du kan styra mängden tillgängligt cacheminne på varje nivå – det väljs genom att välja en cachenivå från C0-C6 för Basic/Standard och P0-P4 för Premium.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-129">You can control the amount of cache memory available on each tier - this is selected by choosing a cache level from C0-C6 for Basic/Standard and P0-P4 for Premium.</span></span> <span data-ttu-id="b6fbc-130">Mer information finns på [prissättningssidan](https://azure.microsoft.com/pricing/details/cache/).</span><span class="sxs-lookup"><span data-stu-id="b6fbc-130">Check the [pricing page](https://azure.microsoft.com/pricing/details/cache/) for full details.</span></span>

> [!TIP]
> <span data-ttu-id="b6fbc-131">Microsoft rekommenderar alltid att du använder Standard- eller Premium-nivån för produktionssystem.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-131">Microsoft recommends you always use Standard or Premium Tier for production systems.</span></span> <span data-ttu-id="b6fbc-132">Basic-nivån är ett system med en nod utan datareplikering och serviceavtal.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-132">The Basic Tier is a single node system with no data replication and no SLA.</span></span> <span data-ttu-id="b6fbc-133">Använd också minst ett C1-cacheminne.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-133">Also, use at least a C1 cache.</span></span> <span data-ttu-id="b6fbc-134">C0-cacheminnen är avsedda för enkla scenarier för utveckling/testning eftersom de har en delad processorkärna och mycket lite minne.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-134">C0 caches are really meant for simple dev/test scenarios since they have a shared CPU core and very little memory.</span></span>

<span data-ttu-id="b6fbc-135">Med Premium-nivån kan du bevara data på två sätt för att tillhandahålla katastrofåterställning:</span><span class="sxs-lookup"><span data-stu-id="b6fbc-135">The Premium tier allows you to persist data in two ways to provide disaster recovery:</span></span>

1. <span data-ttu-id="b6fbc-136">RDB-persistens tar en periodisk ögonblicksbild och kan återskapa cachen med hjälp av ögonblicksbilden om fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-136">RDB persistence takes a periodic snapshot and can rebuild the cache using the snapshot in case of failure.</span></span>

    ![Skärmbild av Azure Portal med alternativen för RDB-persistens på en ny Redis Cache-instans.](../media/3-redis-persistence-1.png)

2. <span data-ttu-id="b6fbc-138">AOF-persistens sparar varje skrivåtgärd till en logg som sparas minst en gång per sekund.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-138">AOF persistence saves every write operation to a log that is saved at least once per second.</span></span> <span data-ttu-id="b6fbc-139">Detta skapar större filer än RDB men har mindre dataförlust.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-139">This creates bigger files than RDB but has less data loss.</span></span>

    ![Skärmbild av Azure-portalen med alternativen för AOF-persistens på en ny Redis Cache-instans.](../media/3-redis-persistence-2.png)

<span data-ttu-id="b6fbc-141">Det finns flera andra inställningar som bara är tillgängliga för **Premium**-nivån.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-141">There are several other settings which are only available to the **Premium** tier.</span></span>

### <a name="virtual-network-support"></a><span data-ttu-id="b6fbc-142">Stöd för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="b6fbc-142">Virtual Network support</span></span>

<span data-ttu-id="b6fbc-143">Om du skapar en Redis Cache på Premium-nivå kan du distribuera den till ett virtuellt nätverk i molnet.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-143">If you create a premium tier Redis cache, you can deploy it to a virtual network in the cloud.</span></span> <span data-ttu-id="b6fbc-144">Din cache blir bara tillgänglig för andra virtuella datorer och program i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-144">Your cache will be available to only other virtual machines and applications in the same virtual network.</span></span> <span data-ttu-id="b6fbc-145">Det ger en högre säkerhetsnivå när din tjänst och cache finns i både Azure och är anslutna via ett virtuellt privat Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-145">This provides a higher level of security when your service and cache are both hosted in Azure, or are connected through an Azure virtual network VPN.</span></span>

### <a name="clustering-support"></a><span data-ttu-id="b6fbc-146">Klusterstöd</span><span class="sxs-lookup"><span data-stu-id="b6fbc-146">Clustering support</span></span>

<span data-ttu-id="b6fbc-147">Med en Redis Cache på Premium-nivå kan du implementera klustring för att automatiskt fördela din datamängd mellan flera noder.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-147">With a premium tier Redis cache, you can implement clustering to automatically split your dataset among multiple nodes.</span></span> <span data-ttu-id="b6fbc-148">Implementera klustring genom att ange antalet shards till högst 10.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-148">To implement clustering, you specify the number of shards to a maximum of 10.</span></span> <span data-ttu-id="b6fbc-149">Kostnaden som uppstår är kostnaden för den ursprungliga noden multiplicerat med antalet shards.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-149">The cost incurred is the cost of the original node, multiplied by the number of shards.</span></span>

## <a name="accessing-the-redis-instance"></a><span data-ttu-id="b6fbc-150">Åtkomst till Redis-instans</span><span class="sxs-lookup"><span data-stu-id="b6fbc-150">Accessing the Redis instance</span></span>

<span data-ttu-id="b6fbc-151">Redis stöder en uppsättning av [kända kommandon](https://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="b6fbc-151">Redis supports a set of [known commands](https://redis.io/commands).</span></span> <span data-ttu-id="b6fbc-152">Ett kommando utfärdas vanligen som `COMMAND parameter1 parameter2 parameter3`.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-152">A command is typically issued as `COMMAND parameter1 parameter2 parameter3`.</span></span>

<span data-ttu-id="b6fbc-153">Här följer några vanliga kommandon som du kan använda:</span><span class="sxs-lookup"><span data-stu-id="b6fbc-153">Here are some common commands you can use:</span></span>

| <span data-ttu-id="b6fbc-154">Kommando</span><span class="sxs-lookup"><span data-stu-id="b6fbc-154">Command</span></span> | <span data-ttu-id="b6fbc-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b6fbc-155">Description</span></span> |
|---------|-------------|
| `ping` | <span data-ttu-id="b6fbc-156">Pinga servern.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-156">Ping the server.</span></span> <span data-ttu-id="b6fbc-157">Returnerar ”PONG”.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-157">Returns "PONG".</span></span> |
| `set [key] [value]` | <span data-ttu-id="b6fbc-158">Ställer in en nyckel/värde i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-158">Sets a key/value in the cache.</span></span> <span data-ttu-id="b6fbc-159">Returnerar ”OK” om åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-159">Returns "OK" on success.</span></span> |
| `get [key]` | <span data-ttu-id="b6fbc-160">Hämtar ett värde från cachen.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-160">Gets a value from the cache.</span></span> |
| `exists [key]` | <span data-ttu-id="b6fbc-161">Returnerar ”1” om **nyckeln** finns i cacheminnet och ”0” om det inte gör det.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-161">Returns '1' if the **key** exists in the cache, '0' if it doesn't.</span></span> |
| `type [key]` | <span data-ttu-id="b6fbc-162">Returnerar typen som är associerad till värdet för den angivna **nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-162">Returns the type associated to the value for the given **key**.</span></span> |
| `incr [key]` | <span data-ttu-id="b6fbc-163">Öka det angivna värdet som är associerat med **nyckeln** med 1.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-163">Increment the given value associated with **key** by '1'.</span></span> <span data-ttu-id="b6fbc-164">Värdet måste vara ett heltal eller ett double-värde.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-164">The value must be an integer or double value.</span></span> <span data-ttu-id="b6fbc-165">Det här returnerar det nya värdet.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-165">This returns the new value.</span></span> |
| `incrby [key] [amount]` | <span data-ttu-id="b6fbc-166">Öka det angivna värdet som är associerat med **nyckeln** med den angivna mängden.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-166">Increment the given value associated with **key** by the specified amount.</span></span> <span data-ttu-id="b6fbc-167">Värdet måste vara ett heltal eller ett double-värde.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-167">The value must be an integer or double value.</span></span> <span data-ttu-id="b6fbc-168">Det här returnerar det nya värdet.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-168">This returns the new value.</span></span> |
| `del [key]` | <span data-ttu-id="b6fbc-169">Tar bort värdet som är associerat med **nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-169">Deletes the value associated with the **key**.</span></span> |
| `flushdb` | <span data-ttu-id="b6fbc-170">Ta bort _alla_ nycklar och värden i databasen.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-170">Delete _all_ keys and values in the database.</span></span> |

<span data-ttu-id="b6fbc-171">Redis har ett kommandoradsverktyg (**redis-cli**) som du kan använda för att experimentera direkt med dessa kommandon.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-171">Redis has a command-line tool (**redis-cli**) you can use to experiment directly with these commands.</span></span> <span data-ttu-id="b6fbc-172">Här följer några exempel.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-172">Here are some examples.</span></span>

```output
> set somekey somevalue
OK
> get somekey
"somevalue"
> exists somekey
(string) 1
> del somekey
(string) 1
> exists somekey
(string) 0
```

<span data-ttu-id="b6fbc-173">Här är ett exempel på hur du arbetar med kommandot `INCR`.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-173">Here's an example of working with the `INCR` commands.</span></span> <span data-ttu-id="b6fbc-174">De är praktiska eftersom de erbjuder atomiska steg _över flera program_ som använder cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-174">These are convenient because they provide atomic increments _across multiple applications_ that are using the cache.</span></span>

```output
> set counter 100
OK
> incr counter
(integer) 101
> incrby counter 50
(integer) 151
> type counter
(integer)
```

### <a name="adding-an-expiration-time-to-values"></a><span data-ttu-id="b6fbc-175">Lägga till en förfallotid för värden</span><span class="sxs-lookup"><span data-stu-id="b6fbc-175">Adding an expiration time to values</span></span>

<span data-ttu-id="b6fbc-176">Cachelagring är viktig eftersom den gör att vi kan lagra vanliga värden i minnet.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-176">Caching is important because it allows us to store commonly used values in memory.</span></span> <span data-ttu-id="b6fbc-177">Men vi behöver också ett sätt att se till att värden förfaller när de är inaktuella.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-177">However, we also need a way to expire values when they are stale.</span></span> <span data-ttu-id="b6fbc-178">I Redis gör du det genom att tillämpa TTL (_time to live_) på en nyckel.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-178">In Redis this is done by applying a _time to live_ (TTL) to a key.</span></span>

<span data-ttu-id="b6fbc-179">När TTL går ut tas nyckeln bort automatiskt, precis som om kommandot DEL skulle användas.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-179">When the TTL elapses, the key is automatically deleted, exactly as if the DEL command were issued.</span></span> <span data-ttu-id="b6fbc-180">Här är några anteckningar om TTL-förfallotider.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-180">Here are some notes on TTL expirations.</span></span>

- <span data-ttu-id="b6fbc-181">Förfallotider kan ställas in med sekund- eller millisekundprecision.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-181">Expirations can be set using seconds or milliseconds precision.</span></span>
- <span data-ttu-id="b6fbc-182">Upplösningen för förfallotid är alltid 1 millisekund.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-182">The expire time resolution is always 1 millisecond.</span></span>
- <span data-ttu-id="b6fbc-183">Information om förfallotid replikeras och sparas på disk, och tiden passerar praktiskt taget när Redis-servern förblir stoppad (det innebär att Redis sparar det datum då en nyckel upphör att gälla).</span><span class="sxs-lookup"><span data-stu-id="b6fbc-183">Information about expires are replicated and persisted on disk, the time virtually passes when your Redis server remains stopped (this means that Redis saves the date at which a key will expire).</span></span>

<span data-ttu-id="b6fbc-184">Här är ett exempel på en förfallotid:</span><span class="sxs-lookup"><span data-stu-id="b6fbc-184">Here is an example of an expiration:</span></span>

```output
> set counter 100
OK
> expire counter 5
(integer) 1
> get counter
100
... wait ...
> get counter
(nil)
```

## <a name="accessing-a-redis-cache-from-a-client"></a><span data-ttu-id="b6fbc-185">Få åtkomst till Redis Cache från en klient</span><span class="sxs-lookup"><span data-stu-id="b6fbc-185">Accessing a Redis cache from a client</span></span>

<span data-ttu-id="b6fbc-186">Om du vill ansluta till en Azure Redis Cache-instans så behöver du flera olika typer av information.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-186">To connect to an Azure Redis Cache instance, you'll need several pieces of information.</span></span> <span data-ttu-id="b6fbc-187">Klienter behöver värdnamn, port och en åtkomstnyckel för cachen.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-187">Clients need the host name, port, and an access key for the cache.</span></span> <span data-ttu-id="b6fbc-188">Du kan hämta den här informationen i Azure-portalen via sidan **Inställningar > Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-188">You can retrieve this information in the Azure portal through the **Settings > Access Keys** page.</span></span> 

- <span data-ttu-id="b6fbc-189">Värdnamnet är den offentliga Internet-adressen för cacheminnet, som skapades med cachenamnet.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-189">The host name is the public Internet address of your cache, which was created using the name of the cache.</span></span> <span data-ttu-id="b6fbc-190">Till exempel `sportsresults.redis.cache.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-190">For example `sportsresults.redis.cache.windows.net`.</span></span>

- <span data-ttu-id="b6fbc-191">Åtkomstnyckeln fungerar som lösenord för cachen.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-191">The access key acts as a password for your cache.</span></span> <span data-ttu-id="b6fbc-192">Två nycklar har skapats: primär och sekundär.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-192">There are two keys created: primary and secondary.</span></span> <span data-ttu-id="b6fbc-193">Du kan använda endera av nycklarna, men det tillhandahålls två ifall du behöver ändra den primära nyckeln.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-193">You can use either key, two are provided in case you need to change the primary key.</span></span> <span data-ttu-id="b6fbc-194">Du kan växla alla dina klienter till den sekundära nyckeln och återskapa primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-194">You can switch all of your clients to the secondary key, and regenerate the primary key.</span></span> <span data-ttu-id="b6fbc-195">Det skulle blockera de program som använder den ursprungliga primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-195">This would block any applications using the original primary key.</span></span> <span data-ttu-id="b6fbc-196">Microsoft rekommenderar att du regelbundet återskapar nycklar – precis som du bör göra med dina personliga lösenord.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-196">Microsoft recommends periodically regenerating the keys - much like you would your personal passwords.</span></span>

> [!WARNING]
> <span data-ttu-id="b6fbc-197">Dina åtkomstnycklar ska betraktas som konfidentiell information, så behandla dem som du behandlar lösenord.</span><span class="sxs-lookup"><span data-stu-id="b6fbc-197">Your access keys should be considered confidential information, treat them like you would a password.</span></span> <span data-ttu-id="b6fbc-198">Vem som helst som har en åtkomstnyckel kan utföra vilka åtgärder som helst i cacheminnet!</span><span class="sxs-lookup"><span data-stu-id="b6fbc-198">Anyone who has an access key can perform any operation on your cache!</span></span>
