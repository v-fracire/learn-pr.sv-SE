<span data-ttu-id="4d289-101">Ditt utvecklingsteam för sportstatistik har kommit fram till att cachelagring avsevärt skulle kunna förbättra prestanda för nyligen begärda data.</span><span class="sxs-lookup"><span data-stu-id="4d289-101">Your sports statistics development team has decided that caching could dramatically improve performance for recently requested data.</span></span> <span data-ttu-id="4d289-102">Nästa steg är att skapa och konfigurera en Azure Redis Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="4d289-102">Your next step is to create and configure an Azure Redis Cache instance.</span></span>

## <a name="create-and-configure-the-azure-redis-cache-instance"></a><span data-ttu-id="4d289-103">Skapa och konfigurera Azure Redis Cache-instansen</span><span class="sxs-lookup"><span data-stu-id="4d289-103">Create and configure the Azure Redis Cache instance</span></span>

1. <span data-ttu-id="4d289-104">Öppna [Azure-portalen](https://portal.azure.com/?azure-portal=true) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4d289-104">Open the [Azure Portal](https://portal.azure.com/?azure-portal=true) in your browser.</span></span>

1. <span data-ttu-id="4d289-105">Klicka på **Skapa en resurs**.</span><span class="sxs-lookup"><span data-stu-id="4d289-105">Click on **Create a resource**.</span></span>

1. <span data-ttu-id="4d289-106">Sök efter **Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="4d289-106">Search for **Redis Cache**.</span></span>

1. <span data-ttu-id="4d289-107">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d289-107">Click **Create**.</span></span>

1. <span data-ttu-id="4d289-108">Du kan lägga till en Azure Redis Cache-instans från databasresurser i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="4d289-108">You can add an Azure Redis Cache instance from the database resources in the Azure portal.</span></span>

1. <span data-ttu-id="4d289-109">Ange ett globalt unikt namn.</span><span class="sxs-lookup"><span data-stu-id="4d289-109">Provide a globally unique name.</span></span> <span data-ttu-id="4d289-110">Namnet måste vara en sträng mellan 1 och 63 tecken och får endast innehålla siffror, bokstäver och tecknet ”-”.</span><span class="sxs-lookup"><span data-stu-id="4d289-110">The name must be a string between 1 and 63 characters and include only numbers, letters, and the '-' character.</span></span> <span data-ttu-id="4d289-111">Cachenamnet får inte inledas eller avslutas med tecknet ”-” eller ha flera ”-” i följd.</span><span class="sxs-lookup"><span data-stu-id="4d289-111">The cache name can't start or end with the '-' character, and consecutive '-' characters aren't valid.</span></span>

1. <span data-ttu-id="4d289-112">Ange prenumerationen där den nya Azure Redis Cache-instansen ska skapas.</span><span class="sxs-lookup"><span data-stu-id="4d289-112">Provide the subscription under which this new Azure Redis Cache instance is created.</span></span>

1. <span data-ttu-id="4d289-113">Namnge den resursgrupp där du vill skapa din cache.</span><span class="sxs-lookup"><span data-stu-id="4d289-113">Name the resource group in which to create your cache.</span></span> <span data-ttu-id="4d289-114">Genom att samla alla resurser för en app i en grupp kan du hantera dem tillsammans.</span><span class="sxs-lookup"><span data-stu-id="4d289-114">By putting all the resources for an app in a group, you can manage them together.</span></span>

1. <span data-ttu-id="4d289-115">Placera alltid din cacheinstans och ditt program i samma region/plats.</span><span class="sxs-lookup"><span data-stu-id="4d289-115">Always place your cache instance and your application in the same region / location.</span></span> <span data-ttu-id="4d289-116">Om du ansluter till en cache i en annan region kan latensen öka och tillförlitligheten minska avsevärt.</span><span class="sxs-lookup"><span data-stu-id="4d289-116">Connecting to a cache in a different region can significantly increase latency and reduce reliability.</span></span> <span data-ttu-id="4d289-117">Om du ansluter till cachen utanför Azure ska du välja en plats som ligger nära platsen där programmet som förbrukar data körs.</span><span class="sxs-lookup"><span data-stu-id="4d289-117">If you are connecting to the cache outside of Azure, then select a location close to where the application consuming the data is running.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4d289-118">Det är mycket viktigare att cachen är nära datakonsumenten än datalagret.</span><span class="sxs-lookup"><span data-stu-id="4d289-118">It's far more important that the cache is close to the data consumer than the data store.</span></span>

1. <span data-ttu-id="4d289-119">Välj prisnivå.</span><span class="sxs-lookup"><span data-stu-id="4d289-119">Select the pricing tier.</span></span> 
    - <span data-ttu-id="4d289-120">Vi rekommenderar att du alltid använder Standard eller Premium-nivån för produktionssystem.</span><span class="sxs-lookup"><span data-stu-id="4d289-120">We recommend you always use Standard or Premium Tier for production systems.</span></span> <span data-ttu-id="4d289-121">Basic-nivån är ett system med en nod utan datareplikering och serviceavtal.</span><span class="sxs-lookup"><span data-stu-id="4d289-121">The Basic Tier is a single node system with no data replication and no SLA.</span></span> <span data-ttu-id="4d289-122">Använd också minst ett C1-cacheminne.</span><span class="sxs-lookup"><span data-stu-id="4d289-122">Also, use at least a C1 cache.</span></span> <span data-ttu-id="4d289-123">C0-cacheminnen är avsedda för enkla scenarier för utveckling/testning eftersom de har en delad processorkärna och mycket lite minne.</span><span class="sxs-lookup"><span data-stu-id="4d289-123">C0 caches are really meant for simple dev/test scenarios since they have a shared CPU core and very little memory.</span></span>

    - <span data-ttu-id="4d289-124">Med Premium-nivån kan du bevara data på två sätt för att tillhandahålla katastrofåterställning:</span><span class="sxs-lookup"><span data-stu-id="4d289-124">The Premium tier allows you to persist data in two ways to provide disaster recovery:</span></span>

        - <span data-ttu-id="4d289-125">RDB-persistens tar en periodisk ögonblicksbild och kan återskapa cachen med hjälp av ögonblicksbilden om fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="4d289-125">RDB persistence takes a periodic snapshot and can rebuild the cache using the snapshot in case of failure.</span></span>

            ![Skärmbild av Azure Portal med alternativen för RDB-persistens på en ny Redis Cache-instans.](../media/3-redis-persistence-1.png)

        - <span data-ttu-id="4d289-127">AOF-persistens sparar alla skrivåtgärder till en logg som sparas minst en gång per sekund.</span><span class="sxs-lookup"><span data-stu-id="4d289-127">AOF persistence saves every write operation to a log that is saved at least once per second.</span></span> <span data-ttu-id="4d289-128">Detta skapar större filer än RDB men har mindre dataförlust.</span><span class="sxs-lookup"><span data-stu-id="4d289-128">This creates bigger files than RDB but has less data loss.</span></span>

            ![Skärmbild av Azure Portal med alternativen för AOF-persistens på en ny Redis Cache-instans.](../media/3-redis-persistence-2.png)

    - <span data-ttu-id="4d289-130">Skydda din cache med ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d289-130">Secure your cache with a virtual network.</span></span>
      <span data-ttu-id="4d289-131">Om du har en Redis Cache på Premium-nivå kan du distribuera den till ett virtuellt nätverk i molnet.</span><span class="sxs-lookup"><span data-stu-id="4d289-131">If you have a premium tier Redis cache, you can deploy it to a virtual network in the cloud.</span></span> <span data-ttu-id="4d289-132">Cachen blir bara tillgänglig för andra virtuella datorer och program i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d289-132">Your cache will be available to only other virtual machines and applications in the same virtual network.</span></span>

    - <span data-ttu-id="4d289-133">Distribuera din cache med klustring.</span><span class="sxs-lookup"><span data-stu-id="4d289-133">Distribute your cache with clustering.</span></span>
      <span data-ttu-id="4d289-134">Med en Redis Cache på Premium-nivå kan du implementera klustring för att automatiskt fördela din datamängd mellan flera noder.</span><span class="sxs-lookup"><span data-stu-id="4d289-134">With a premium tier Redis cache, you can implement clustering to automatically split your dataset among multiple nodes.</span></span> <span data-ttu-id="4d289-135">Implementera klustring genom att ange antalet shards till högst 10.</span><span class="sxs-lookup"><span data-stu-id="4d289-135">To implement clustering, you specify the number of shards to a maximum of 10.</span></span> <span data-ttu-id="4d289-136">Kostnaden som uppstår är kostnaden för den ursprungliga noden multiplicerat med antalet shards.</span><span class="sxs-lookup"><span data-stu-id="4d289-136">The cost incurred is the cost of the original node, multiplied by the number of shards.</span></span>

    - <span data-ttu-id="4d289-137">Migrera från Azure Managed Cache Service.</span><span class="sxs-lookup"><span data-stu-id="4d289-137">Migrate from Azure Managed Cache Service.</span></span>
      <span data-ttu-id="4d289-138">Beroende på vilka Managed Cache Service-funktioner du använder går det att migrera program som använder Azure Managed Cache Service till Azure Redis Cache med minimala ändringar i ditt program.</span><span class="sxs-lookup"><span data-stu-id="4d289-138">Depending on the Managed Cache Service features you use, migrating applications that use Azure Managed Cache Service to Azure Redis Cache can be accomplished with minimal changes to your application.</span></span> <span data-ttu-id="4d289-139">API:erna inte är exakt samma, men de liknar varandra.</span><span class="sxs-lookup"><span data-stu-id="4d289-139">While the APIs aren't exactly the same, they're similar.</span></span> <span data-ttu-id="4d289-140">Mycket av din befintliga kod som använder cachen via Managed Cache Service kan återanvändas med minimala ändringar.</span><span class="sxs-lookup"><span data-stu-id="4d289-140">Much of your existing code that accesses the cache with Managed Cache Service can be reused with minimal changes.</span></span>

## <a name="accessing-the-redis-instance"></a><span data-ttu-id="4d289-141">Åtkomst till Redis-instans</span><span class="sxs-lookup"><span data-stu-id="4d289-141">Accessing the Redis instance</span></span>

<span data-ttu-id="4d289-142">Redis stöder en uppsättning av [kända kommandon](https://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="4d289-142">Redis supports a set of [known commands](https://redis.io/commands).</span></span> <span data-ttu-id="4d289-143">Ett kommando utfärdas vanligen som `COMMAND parameter1 parameter2 parameter3`.</span><span class="sxs-lookup"><span data-stu-id="4d289-143">A command is typically issued as `COMMAND parameter1 parameter2 parameter3`.</span></span>

<span data-ttu-id="4d289-144">Här följer några vanliga kommandon som du kan använda:</span><span class="sxs-lookup"><span data-stu-id="4d289-144">Here are some common commands you can use:</span></span>

| <span data-ttu-id="4d289-145">Kommando</span><span class="sxs-lookup"><span data-stu-id="4d289-145">Command</span></span> | <span data-ttu-id="4d289-146">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4d289-146">Description</span></span> |
|---------|-------------|
| `ping` | <span data-ttu-id="4d289-147">Pinga servern.</span><span class="sxs-lookup"><span data-stu-id="4d289-147">Ping the server.</span></span> <span data-ttu-id="4d289-148">Returnerar ”PONG”.</span><span class="sxs-lookup"><span data-stu-id="4d289-148">Returns "PONG".</span></span> |
| `set [key] [value]` | <span data-ttu-id="4d289-149">Ställer in en nyckel/värde i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="4d289-149">Sets a key/value in the cache.</span></span> <span data-ttu-id="4d289-150">Returnerar ”OK” om åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="4d289-150">Returns "OK" on success.</span></span> |
| `get [key]` | <span data-ttu-id="4d289-151">Hämtar ett värde från cachen.</span><span class="sxs-lookup"><span data-stu-id="4d289-151">Gets a value from the cache.</span></span> |
| `exists [key]` | <span data-ttu-id="4d289-152">Returnerar ”1” om **nyckeln** finns i cacheminnet och ”0” om det inte gör det.</span><span class="sxs-lookup"><span data-stu-id="4d289-152">Returns '1' if the **key** exists in the cache, '0' if it doesn't.</span></span> |
| `type [key]` | <span data-ttu-id="4d289-153">Returnerar typen som är associerad till värdet för den angivna **nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="4d289-153">Returns the type associated to the value for the given **key**.</span></span> |
| `incr [key]` | <span data-ttu-id="4d289-154">Öka det angivna värdet som är associerat med **nyckeln** med ”1”.</span><span class="sxs-lookup"><span data-stu-id="4d289-154">Increment the given value associated with **key** by \`1'.</span></span> <span data-ttu-id="4d289-155">Värdet måste vara ett heltal eller ett double-värde.</span><span class="sxs-lookup"><span data-stu-id="4d289-155">The value must be an integer or double value.</span></span> <span data-ttu-id="4d289-156">Det här returnerar det nya värdet.</span><span class="sxs-lookup"><span data-stu-id="4d289-156">This returns the new value.</span></span> |
| `incrby [key] [amount]` | <span data-ttu-id="4d289-157">Öka det angivna värdet som är associerat med **nyckeln** med den angivna mängden.</span><span class="sxs-lookup"><span data-stu-id="4d289-157">Increment the given value associated with **key** by the specified amount.</span></span> <span data-ttu-id="4d289-158">Värdet måste vara ett heltal eller ett double-värde.</span><span class="sxs-lookup"><span data-stu-id="4d289-158">The value must be an integer or double value.</span></span> <span data-ttu-id="4d289-159">Det här returnerar det nya värdet.</span><span class="sxs-lookup"><span data-stu-id="4d289-159">This returns the new value.</span></span> |
| `del [key]` | <span data-ttu-id="4d289-160">Tar bort värdet som är associerat med **nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="4d289-160">Deletes the value associated with the **key**.</span></span> |
| `flushdb` | <span data-ttu-id="4d289-161">Ta bort _alla_ nycklar och värden i databasen.</span><span class="sxs-lookup"><span data-stu-id="4d289-161">Delete _all_ keys and values in the database.</span></span> |

<span data-ttu-id="4d289-162">Redis har ett kommandoradsverktyg (**redis-cli**) som du kan använda för att experimentera direkt med dessa kommandon.</span><span class="sxs-lookup"><span data-stu-id="4d289-162">Redis has a command-line tool (**redis-cli**) you can use to experiment directly with these commands.</span></span> <span data-ttu-id="4d289-163">Här följer några exempel.</span><span class="sxs-lookup"><span data-stu-id="4d289-163">Here are some examples.</span></span>

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

<span data-ttu-id="4d289-164">Här är ett exempel på hur du arbetar med kommandot `INCR`.</span><span class="sxs-lookup"><span data-stu-id="4d289-164">Here's an example of working with the `INCR` commands.</span></span> <span data-ttu-id="4d289-165">De är praktiska eftersom de erbjuder atomiska steg _över flera program_ som använder cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="4d289-165">These are convenient because they provide atomic increments _across multiple applications_ that are using the cache.</span></span>

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

### <a name="adding-an-expiration-time-to-values"></a><span data-ttu-id="4d289-166">Lägga till en förfallotid för värden</span><span class="sxs-lookup"><span data-stu-id="4d289-166">Adding an expiration time to values</span></span>

<span data-ttu-id="4d289-167">Cachelagring är viktig eftersom den gör att vi kan lagra vanliga värden i minnet.</span><span class="sxs-lookup"><span data-stu-id="4d289-167">Caching is important because it allows us to store commonly used values in memory.</span></span> <span data-ttu-id="4d289-168">Men vi behöver också ett sätt att se till att värden förfaller när de är inaktuella.</span><span class="sxs-lookup"><span data-stu-id="4d289-168">However, we also need a way to expire values when they are stale.</span></span> <span data-ttu-id="4d289-169">I Redis gör du det genom att tillämpa TTL (_time to live_) på en nyckel.</span><span class="sxs-lookup"><span data-stu-id="4d289-169">In Redis this is done by applying a _time to live_ (TTL) to a key.</span></span>

<span data-ttu-id="4d289-170">När TTL går ut tas nyckeln bort automatiskt, precis som om kommandot DEL skulle användas.</span><span class="sxs-lookup"><span data-stu-id="4d289-170">When the TTL elapses, the key is automatically deleted, exactly as if the DEL command were issued.</span></span> <span data-ttu-id="4d289-171">Här är några anteckningar om TTL-förfallotider.</span><span class="sxs-lookup"><span data-stu-id="4d289-171">Here are some notes on TTL expirations.</span></span>

- <span data-ttu-id="4d289-172">Förfallotider kan ställas in med sekund- eller millisekundprecision.</span><span class="sxs-lookup"><span data-stu-id="4d289-172">Expirations can be set using seconds or milliseconds precision.</span></span>
- <span data-ttu-id="4d289-173">Upplösningen för förfallotid är alltid 1 millisekund.</span><span class="sxs-lookup"><span data-stu-id="4d289-173">The expire time resolution is always 1 millisecond.</span></span>
- <span data-ttu-id="4d289-174">Information om förfallotid replikeras och sparas på disk, och tiden passerar praktiskt taget när Redis-servern förblir stoppad (det innebär att Redis sparar det datum då en nyckel upphör att gälla).</span><span class="sxs-lookup"><span data-stu-id="4d289-174">Information about expires are replicated and persisted on disk, the time virtually passes when your Redis server remains stopped (this means that Redis saves the date at which a key will expire).</span></span>

<span data-ttu-id="4d289-175">Här är ett exempel på en förfallotid:</span><span class="sxs-lookup"><span data-stu-id="4d289-175">Here is an example of an expiration:</span></span>

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

## <a name="accessing-a-redis-cache-from-a-client"></a><span data-ttu-id="4d289-176">Få åtkomst till Redis Cache från en klient</span><span class="sxs-lookup"><span data-stu-id="4d289-176">Accessing a Redis cache from a client</span></span>

<span data-ttu-id="4d289-177">När du ansluter till en Azure Redis Cache-instans behöver klienter värdnamnet, portarna och en åtkomstnyckel för cachen.</span><span class="sxs-lookup"><span data-stu-id="4d289-177">When connecting to an Azure Redis Cache instance, clients need the host name, port, and an access key for the cache.</span></span> <span data-ttu-id="4d289-178">Du kan hämta den här informationen i Azure-portalen via sidan **Inställningar > Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="4d289-178">You can retrieve this information in the Azure portal through the **Settings > Access Keys** page.</span></span> 

- <span data-ttu-id="4d289-179">Värdnamnet är den offentliga Internet-adressen för cacheminnet, som skapades med cachenamnet.</span><span class="sxs-lookup"><span data-stu-id="4d289-179">The host name is the public Internet address of your cache, which was created using the name of the cache.</span></span> <span data-ttu-id="4d289-180">Till exempel `sportsresults.redis.cache.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="4d289-180">For example `sportsresults.redis.cache.windows.net`.</span></span>

- <span data-ttu-id="4d289-181">Åtkomstnyckeln fungerar som lösenord för cachen.</span><span class="sxs-lookup"><span data-stu-id="4d289-181">The access key acts as a password for your cache.</span></span> <span data-ttu-id="4d289-182">Två nycklar har skapats: primär och sekundär.</span><span class="sxs-lookup"><span data-stu-id="4d289-182">There are two keys created: primary and secondary.</span></span> <span data-ttu-id="4d289-183">Du kan använda endera av nycklarna, men det tillhandahålls två ifall du behöver ändra den primära nyckeln.</span><span class="sxs-lookup"><span data-stu-id="4d289-183">You can use either key, two are provided in case you need to change the primary key.</span></span> <span data-ttu-id="4d289-184">Du kan växla alla dina klienter till den sekundära nyckeln och återskapa primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="4d289-184">You can switch all of your clients to the secondary key, and regenerate the primary key.</span></span> <span data-ttu-id="4d289-185">Det skulle blockera de program som använder den ursprungliga primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="4d289-185">This would block any applications using the original primary key.</span></span> <span data-ttu-id="4d289-186">Microsoft rekommenderar att du regelbundet återskapar nycklar – precis som du bör göra med dina personliga lösenord.</span><span class="sxs-lookup"><span data-stu-id="4d289-186">Microsoft recommends periodically regenerating the keys - much like you would your personal passwords.</span></span>

> [!WARNING]
> <span data-ttu-id="4d289-187">Dina åtkomstnycklar ska betraktas som konfidentiell information, så behandla dem som du behandlar lösenord.</span><span class="sxs-lookup"><span data-stu-id="4d289-187">Your access keys should be considered confidential information, treat them like you would a password.</span></span> <span data-ttu-id="4d289-188">Vem som helst som har en åtkomstnyckel kan utföra vilka åtgärder som helst i cacheminnet!</span><span class="sxs-lookup"><span data-stu-id="4d289-188">Anyone who has an access key can perform any operation on your cache!</span></span>
