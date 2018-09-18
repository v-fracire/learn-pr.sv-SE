<span data-ttu-id="91a09-101">Såsom nämnts tidigare är Redis en minnesintern NoSQL-databas som kan replikeras över flera servrar.</span><span class="sxs-lookup"><span data-stu-id="91a09-101">As mentioned earlier, Redis is an in-memory NoSQL database which can be replicated across multiple servers.</span></span> <span data-ttu-id="91a09-102">Den används ofta som en cache, men kan även användas som en formell databas eller en asynkron meddelandekö.</span><span class="sxs-lookup"><span data-stu-id="91a09-102">It is often used as a cache, but can be used as a formal database or even message-broker.</span></span> 

<span data-ttu-id="91a09-103">Den kan lagra olika datatyper och strukturer samt stöder en mängd olika kommandon som du kan utfärda för att hämta cachelagrade data eller fråga efter information om själva cachen.</span><span class="sxs-lookup"><span data-stu-id="91a09-103">It can store a variety of data types and structures and supports a variety of commands you can issue to retrieve cached data or query information about the cache itself.</span></span> <span data-ttu-id="91a09-104">De data du arbetar med lagras alltid som nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="91a09-104">The data you work with is always stored as key/value pairs.</span></span>

## <a name="executing-commands-on-the-redis-cache"></a><span data-ttu-id="91a09-105">Köra kommandon på Redis-cachen</span><span class="sxs-lookup"><span data-stu-id="91a09-105">Executing commands on the Redis cache</span></span>

<span data-ttu-id="91a09-106">Vanligtvis använder ett klientprogram ett _klientbibliotek_ till att skapa begäranden och köra kommandon på en Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="91a09-106">Typically, a client application will use a _client library_ to form requests and execute commands on a Redis cache.</span></span> <span data-ttu-id="91a09-107">Du kan hämta en lista med klientbibliotek direkt från [Redis klientsida](https://redis.io/clients).</span><span class="sxs-lookup"><span data-stu-id="91a09-107">You can get a list of client libraries directly from the [Redis clients page](https://redis.io/clients).</span></span> <span data-ttu-id="91a09-108">**StackExchange.Redis** är en populär Redis-klient med höga prestanda för språket .NET.</span><span class="sxs-lookup"><span data-stu-id="91a09-108">A popular high-performance Redis client for the .NET language is **StackExchange.Redis**.</span></span> <span data-ttu-id="91a09-109">Paketet är tillgängligt via NuGet och kan läggas till i din .NET-kod med hjälp av kommandoraden eller IDE.</span><span class="sxs-lookup"><span data-stu-id="91a09-109">The package is available through NuGet and can be added to your .NET code using the command line or IDE.</span></span>

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a><span data-ttu-id="91a09-110">Ansluta till din Redis-cache med StackExchange.Redis</span><span class="sxs-lookup"><span data-stu-id="91a09-110">Connecting to your Redis cache with StackExchange.Redis</span></span>

<span data-ttu-id="91a09-111">Kom ihåg att vi använder värdadressen, portnumret och en åtkomstnyckel för att ansluta till en Redis-server.</span><span class="sxs-lookup"><span data-stu-id="91a09-111">Recall that we use the host address, port number, and an access key to connect to a Redis server.</span></span> <span data-ttu-id="91a09-112">Azure erbjuder också en _anslutningssträng_ för vissa Redis-klienter som kombinerar dessa data till en enda sträng.</span><span class="sxs-lookup"><span data-stu-id="91a09-112">Azure also offers a _connection string_ for some Redis clients which bundles this data together into a single string.</span></span>

### <a name="what-is-a-connection-string"></a><span data-ttu-id="91a09-113">Vad är en anslutningssträng?</span><span class="sxs-lookup"><span data-stu-id="91a09-113">What is a connection string?</span></span>

<span data-ttu-id="91a09-114">En anslutningssträng är en enskild textrad som innehåller alla uppgifter som krävs för att ansluta och autentisera till en Redis-cache i Azure.</span><span class="sxs-lookup"><span data-stu-id="91a09-114">A connection string is a single line of text that includes all the required pieces of information to connect and authenticate to a Redis cache in Azure.</span></span> <span data-ttu-id="91a09-115">Den ser ut ungefär så här (med fälten **cache-name** och **password-here** ifyllda med verkliga värden):</span><span class="sxs-lookup"><span data-stu-id="91a09-115">It will look something like the following (with the **cache-name** and **password-here** fields filled in with real values):</span></span>

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> <span data-ttu-id="91a09-116">Anslutningssträngen ska skyddas i ditt program.</span><span class="sxs-lookup"><span data-stu-id="91a09-116">The connection string should be protected in your application.</span></span> <span data-ttu-id="91a09-117">Om programmet finns på Azure kan du använda ett Azure Key Vault för att lagra värdet.</span><span class="sxs-lookup"><span data-stu-id="91a09-117">If the application is hosted on Azure, consider using an Azure Key Vault to store the value.</span></span>

<span data-ttu-id="91a09-118">Du kan skicka den här strängen till **StackExchange.Redis** för att skapa en anslutning till servern.</span><span class="sxs-lookup"><span data-stu-id="91a09-118">You can pass this string to **StackExchange.Redis** to create a connection the server.</span></span> 

<span data-ttu-id="91a09-119">Observera att det finns ytterligare två parametrar i slutet:</span><span class="sxs-lookup"><span data-stu-id="91a09-119">Notice that there are two additional parameters at the end:</span></span> 

- <span data-ttu-id="91a09-120">**SSL** – Ser till att kommunikationen är krypterad.</span><span class="sxs-lookup"><span data-stu-id="91a09-120">**ssl** - ensures that communication is encrypted.</span></span>
- <span data-ttu-id="91a09-121">**abortConnection** – Tillåter att en anslutning skapas även om servern inte är tillgänglig just då.</span><span class="sxs-lookup"><span data-stu-id="91a09-121">**abortConnection** - allows a connection to be created even if the server is unavailable at that moment.</span></span>

<span data-ttu-id="91a09-122">Det finns flera andra [valfria parametrar](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) som du kan lägga till i strängen när du konfigurerar klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="91a09-122">There are several other [optional parameters](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) you can append to the string to configure the client library.</span></span>

### <a name="creating-a-connection"></a><span data-ttu-id="91a09-123">Skapa en anslutning</span><span class="sxs-lookup"><span data-stu-id="91a09-123">Creating a connection</span></span>

<span data-ttu-id="91a09-124">Det huvudsakliga anslutningsobjektet i **StackExchange.Redis** är klassen `StackExchange.Redis.ConnectionMultiplexer`.</span><span class="sxs-lookup"><span data-stu-id="91a09-124">The main connection object in **StackExchange.Redis** is the `StackExchange.Redis.ConnectionMultiplexer` class.</span></span> <span data-ttu-id="91a09-125">Det här objektet eliminerar processen för att ansluta till en Redis-server (eller en grupp med servrar).</span><span class="sxs-lookup"><span data-stu-id="91a09-125">This object abstracts the process of connecting to a Redis server (or group of servers).</span></span> <span data-ttu-id="91a09-126">Det har optimerats för att effektivt hantera anslutningar och kan användas varje gång du behöver åtkomst till cachen.</span><span class="sxs-lookup"><span data-stu-id="91a09-126">It's optimized to manage connections efficiently and intended to be kept around while you need access to the cache.</span></span>

<span data-ttu-id="91a09-127">Du skapar en `ConnectionMultiplexer`-instans med statisk `ConnectionMultiplexer.Connect` eller `ConnectionMultiplexer.ConnectAsync`-metoden och skickar antingen en anslutningssträng eller ett `ConfigurationOptions`-objekt.</span><span class="sxs-lookup"><span data-stu-id="91a09-127">You create a `ConnectionMultiplexer` instance using the static `ConnectionMultiplexer.Connect` or `ConnectionMultiplexer.ConnectAsync` method, passing in either a connection string or a `ConfigurationOptions` object.</span></span> 

<span data-ttu-id="91a09-128">Här är ett enkelt exempel:</span><span class="sxs-lookup"><span data-stu-id="91a09-128">Here's a simple example:</span></span>

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

<span data-ttu-id="91a09-129">När du har en `ConnectionMultiplexer` finns det tre primära saker som du kanske vill göra:</span><span class="sxs-lookup"><span data-stu-id="91a09-129">Once you have a `ConnectionMultiplexer`, there are 3 primary things you might want to do:</span></span>

1. <span data-ttu-id="91a09-130">Få åtkomst till en Redis-databas.</span><span class="sxs-lookup"><span data-stu-id="91a09-130">Access a Redis Database.</span></span> <span data-ttu-id="91a09-131">Det är vad vi kommer att fokusera på här.</span><span class="sxs-lookup"><span data-stu-id="91a09-131">This is what we will focus on here.</span></span>
2. <span data-ttu-id="91a09-132">Använda utgivar-/prenumerationsfunktioner i Redis.</span><span class="sxs-lookup"><span data-stu-id="91a09-132">Make use of the publisher/subscript features of Redis.</span></span> <span data-ttu-id="91a09-133">Det här ligger utanför innehållet för den här modulen.</span><span class="sxs-lookup"><span data-stu-id="91a09-133">This is outside the scope of this module.</span></span>
3. <span data-ttu-id="91a09-134">Få åtkomst till en enskild server för underhåll eller övervakning.</span><span class="sxs-lookup"><span data-stu-id="91a09-134">Access an individual server for maintenance or monitoring purposes.</span></span>

### <a name="accessing-a-redis-database"></a><span data-ttu-id="91a09-135">Åtkomst till en Redis-databas</span><span class="sxs-lookup"><span data-stu-id="91a09-135">Accessing a Redis database</span></span>

<span data-ttu-id="91a09-136">Redis-databasen representeras av `IDatabase`-typen.</span><span class="sxs-lookup"><span data-stu-id="91a09-136">The Redis database is represented by the `IDatabase` type.</span></span> <span data-ttu-id="91a09-137">Du kan hämta en med `GetDatabase()`-metoden:</span><span class="sxs-lookup"><span data-stu-id="91a09-137">You can retrieve one using the `GetDatabase()` method:</span></span>

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> <span data-ttu-id="91a09-138">Det objekt som returneras från `GetDatabase` är ett förenklat objekt som inte behöver lagras.</span><span class="sxs-lookup"><span data-stu-id="91a09-138">The object returned from `GetDatabase` is a lightweight object, and does not need to be stored.</span></span> <span data-ttu-id="91a09-139">Det är bara `ConnectionMultiplexer` som måste vara aktiv.</span><span class="sxs-lookup"><span data-stu-id="91a09-139">Only the `ConnectionMultiplexer` needs to be kept alive.</span></span>

<span data-ttu-id="91a09-140">När du har ett `IDatabase`-objekt kan du köra metoder för att interagera med cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="91a09-140">Once you have a `IDatabase` object, you can execute methods to interact with the cache.</span></span> <span data-ttu-id="91a09-141">Alla metoder har synkrona och asynkrona versioner som returnerar `Task`-objekt för att göra dem kompatibla med nyckelorden `async` och `await`.</span><span class="sxs-lookup"><span data-stu-id="91a09-141">All methods have synchronous and asynchronous versions which return `Task` objects to make them compatible with the `async` and `await` keywords.</span></span>

<span data-ttu-id="91a09-142">Här är ett exempel på att lagra nyckel/värde i cacheminnet:</span><span class="sxs-lookup"><span data-stu-id="91a09-142">Here is an example of storing a key/value in the cache:</span></span>

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

<span data-ttu-id="91a09-143">`StringSet`-metoden returnerar en `bool` som visar ifall värdet har angetts (`true`) eller inte (`false`).</span><span class="sxs-lookup"><span data-stu-id="91a09-143">The `StringSet` method returns a `bool` indicating whether the value was set (`true`) or not (`false`).</span></span> <span data-ttu-id="91a09-144">Vi kan sedan hämta värdet med `StringGet`-metoden:</span><span class="sxs-lookup"><span data-stu-id="91a09-144">We can then retrieve the value with the `StringGet` method:</span></span>

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a><span data-ttu-id="91a09-145">Hämta och ange binära värden</span><span class="sxs-lookup"><span data-stu-id="91a09-145">Getting and Setting binary values</span></span>

<span data-ttu-id="91a09-146">Kom ihåg att Redis-nycklar och värden är _binärt säkra_.</span><span class="sxs-lookup"><span data-stu-id="91a09-146">Recall that Redis keys and values are _binary safe_.</span></span> <span data-ttu-id="91a09-147">Samma metoder kan användas för att lagra binära data.</span><span class="sxs-lookup"><span data-stu-id="91a09-147">These same methods can be used to store binary data.</span></span> <span data-ttu-id="91a09-148">Det finns implicita konverteringsoperatörer som använder `byte[]`-typer, så du kan arbeta med data naturligt:</span><span class="sxs-lookup"><span data-stu-id="91a09-148">There are implicit conversion operators to work with `byte[]` types so you can work with the data naturally:</span></span>

```csharp
byte[] key = ...;
byte[] value = ...;

db.StringSet(key, value);
```

```csharp
byte[] key = ...;
byte[] value = db.StringGet(key);
```

> [!TIP]
> <span data-ttu-id="91a09-149">**StackExchange.Redis** representerar nycklar med `RedisKey`-typen.</span><span class="sxs-lookup"><span data-stu-id="91a09-149">**StackExchange.Redis** represents keys using the `RedisKey` type.</span></span> <span data-ttu-id="91a09-150">Den här klassen har implicita konverteringar till och från både `string` och `byte[]`, vilket gör att både text och binära nycklar kan användas utan problem.</span><span class="sxs-lookup"><span data-stu-id="91a09-150">This class has implicit conversions to and from both `string` and `byte[]`, allowing both text and binary keys to be used without any complication.</span></span> <span data-ttu-id="91a09-151">Värden representeras av `RedisValue`-typen.</span><span class="sxs-lookup"><span data-stu-id="91a09-151">Values are represented by the `RedisValue` type.</span></span> <span data-ttu-id="91a09-152">Precis som med `RedisKey`, finns det implicita konverteringar på plats så att du kan skicka `string` eller `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="91a09-152">As with `RedisKey`, there are implicit conversions in place to allow you to pass `string` or `byte[]`.</span></span>

#### <a name="other-common-operations"></a><span data-ttu-id="91a09-153">Andra vanliga åtgärder</span><span class="sxs-lookup"><span data-stu-id="91a09-153">Other common operations</span></span>

<span data-ttu-id="91a09-154">`IDatabase`-gränssnittet innehåller flera andra metoder för att arbeta med Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="91a09-154">The `IDatabase` interface includes several other methods to work with the Redis cache.</span></span> <span data-ttu-id="91a09-155">Det finns metoder för att arbeta med hashvärden, listor, uppsättningar och ordnade uppsättningar.</span><span class="sxs-lookup"><span data-stu-id="91a09-155">There are methods to work with hashes, lists, sets, and ordered sets.</span></span>

<span data-ttu-id="91a09-156">Här är några av de vanligaste metoderna som fungerar med enkla nycklar. Du kan [läsa källkoden](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) för gränssnittet om du vill se hela listan.</span><span class="sxs-lookup"><span data-stu-id="91a09-156">Here are some of the more common ones that work with single keys, you can [read the source code](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) for the interface to see the full list.</span></span>

| <span data-ttu-id="91a09-157">Metod</span><span class="sxs-lookup"><span data-stu-id="91a09-157">Method</span></span> | <span data-ttu-id="91a09-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="91a09-158">Description</span></span> |
|--------|-------------|
| `CreateBatch` | <span data-ttu-id="91a09-159">Skapar en _åtgärdsgrupp_ som kommer att skickas till servern som en enda enhet, men inte nödvändigtvis behandlas som en enhet.</span><span class="sxs-lookup"><span data-stu-id="91a09-159">Creates a _group of operations_ that will be sent to the server as a single unit, but not necessarily processed as a unit.</span></span> |
| `CreateTransaction` | <span data-ttu-id="91a09-160">Skapar en åtgärdsgrupp som kommer att skickas till servern som en enda enhet _och_ behandlas på servern som en enda enhet.</span><span class="sxs-lookup"><span data-stu-id="91a09-160">Creates a group of operations that will be sent to the server as a single unit _and_ processed on the server as a single unit.</span></span> |
| `KeyDelete` | <span data-ttu-id="91a09-161">Ta bort nyckel/värde.</span><span class="sxs-lookup"><span data-stu-id="91a09-161">Delete the key/value.</span></span> |
| `KeyExists` | <span data-ttu-id="91a09-162">Returneras om den angivna nyckeln finns i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="91a09-162">Returns whether the given key exists in cache.</span></span> |
| `KeyExpire` | <span data-ttu-id="91a09-163">Anger en tidsgräns för TTL-värdet (Time to live) för en nyckel.</span><span class="sxs-lookup"><span data-stu-id="91a09-163">Sets a time-to-live (TTL) expiration on a key.</span></span> |
| `KeyRename` | <span data-ttu-id="91a09-164">Byter namn på en nyckel.</span><span class="sxs-lookup"><span data-stu-id="91a09-164">Renames a key.</span></span> |
| `KeyTimeToLive` | <span data-ttu-id="91a09-165">Returnerar TTL-värdet för en nyckel.</span><span class="sxs-lookup"><span data-stu-id="91a09-165">Returns the TTL for a key.</span></span> |
| `KeyType` | <span data-ttu-id="91a09-166">Returnerar en strängrepresentation av typen på det värde som lagras i nyckeln.</span><span class="sxs-lookup"><span data-stu-id="91a09-166">Returns the string representation of the type of the value stored at key.</span></span> <span data-ttu-id="91a09-167">Olika typer som kan returneras är: sträng, lista, uppsättning, zset och hash.</span><span class="sxs-lookup"><span data-stu-id="91a09-167">The different types that can be returned are: string, list, set, zset and hash.</span></span> |
       
### <a name="executing-other-commands"></a><span data-ttu-id="91a09-168">Köra andra kommandon</span><span class="sxs-lookup"><span data-stu-id="91a09-168">Executing other commands</span></span>

<span data-ttu-id="91a09-169">`IDatabase`-objektet har en `Execute`- och `ExecuteAsync`-metod som kan användas till att skicka textkommandon till Redis-servern.</span><span class="sxs-lookup"><span data-stu-id="91a09-169">The `IDatabase` object has an `Execute` and `ExecuteAsync` method which can be used to pass textual commands to the Redis server.</span></span> <span data-ttu-id="91a09-170">Exempel:</span><span class="sxs-lookup"><span data-stu-id="91a09-170">For example:</span></span>

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

<span data-ttu-id="91a09-171">Metoderna `Execute` och `ExecuteAsync` returnerar ett `RedisResult`-objekt som är en datainnehavare med två egenskaper:</span><span class="sxs-lookup"><span data-stu-id="91a09-171">The `Execute` and `ExecuteAsync` methods return a `RedisResult` object which is a data holder that includes two properties:</span></span>

- <span data-ttu-id="91a09-172">`Type` returnerar en `string` som anger resultattypen – ”STRING”, ”INTEGER” osv.</span><span class="sxs-lookup"><span data-stu-id="91a09-172">`Type` which returns a `string` indicating the type of the result - "STRING", "INTEGER", etc.</span></span>
- <span data-ttu-id="91a09-173">`IsNull` är ett true/false-värde som visar om resultatet är `null`.</span><span class="sxs-lookup"><span data-stu-id="91a09-173">`IsNull` a true/false value to detect when the result is `null`.</span></span>

<span data-ttu-id="91a09-174">Du kan sedan använda `ToString()` på `RedisResult` för att hämta det faktiska returvärdet.</span><span class="sxs-lookup"><span data-stu-id="91a09-174">You can then use `ToString()` on the `RedisResult` to get the actual return value.</span></span>

<span data-ttu-id="91a09-175">Du kan använda `Execute` till att utföra de kommandon som stöds – till exempel kan vi hämta alla klienter som är anslutna till cachen (”CLIENT LIST”):</span><span class="sxs-lookup"><span data-stu-id="91a09-175">You can use `Execute` to perform any supported commands - for example, we can get all the clients connected to the cache ("CLIENT LIST"):</span></span>

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

<span data-ttu-id="91a09-176">Detta visar alla anslutna klienter:</span><span class="sxs-lookup"><span data-stu-id="91a09-176">This would output all the connected clients:</span></span>

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a><span data-ttu-id="91a09-177">Lagra mer komplexa värden</span><span class="sxs-lookup"><span data-stu-id="91a09-177">Storing more complex values</span></span>
<span data-ttu-id="91a09-178">Redis är inriktade på binära säkra strängar, men du kan cachelagra objektdiagram genom att serialisera dem till ett textformat – vanligtvis XML eller JSON.</span><span class="sxs-lookup"><span data-stu-id="91a09-178">Redis is oriented around binary safe strings, but you can cache off object graphs by serializing them to a textual format - typically XML or JSON.</span></span> <span data-ttu-id="91a09-179">För vår statistik kan vi vilja ha ett `GameStats`-objekt som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="91a09-179">For example, perhaps for our statistics, we have a `GameStats` object which looks like:</span></span>

```csharp
public class GameStat
{
    public string Id { get; set; }
    public string Sport { get; set; }
    public DateTimeOffset DatePlayed { get; set; }
    public string Game { get; set; }
    public IReadOnlyList<string> Teams { get; set; }
    public IReadOnlyList<(string team, int score)> Results { get; set; }

    public GameStat(string sport, DateTimeOffset datePlayed, string game, string[] teams, IEnumerable<(string team, int score)> results)
    {
        Id = Guid.NewGuid().ToString();
        Sport = sport;
        DatePlayed = datePlayed;
        Game = game;
        Teams = teams.ToList();
        Results = results.ToList();
    }

    public override string ToString()
    {
        return $"{Sport} {Game} played on {DatePlayed.Date.ToShortDateString()} - " +
               $"{String.Join(',', Teams)}\r\n\t" + 
               $"{String.Join('\t', Results.Select(r => $"{r.team } - {r.score}\r\n"))}";
    }
}
```

<span data-ttu-id="91a09-180">Vi kan använda biblioteket **Newtonsoft.Json** till att omvandla en instans av det här objektet till en sträng:</span><span class="sxs-lookup"><span data-stu-id="91a09-180">We could use the **Newtonsoft.Json** library to turn an instance of this object into a string:</span></span>

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

<span data-ttu-id="91a09-181">Vi kan hämta den och omvandla den tillbaka till ett objekt genom en omvänd process:</span><span class="sxs-lookup"><span data-stu-id="91a09-181">We could retrieve it and turn it back into an object using the reverse process:</span></span>

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a><span data-ttu-id="91a09-182">Rensa anslutningen</span><span class="sxs-lookup"><span data-stu-id="91a09-182">Cleaning up the connection</span></span>
<span data-ttu-id="91a09-183">När du är klar med Redis-anslutningen kan du **ta bort** `ConnectionMultiplexer`.</span><span class="sxs-lookup"><span data-stu-id="91a09-183">Once you are done with the Redis connection, you can **Dispose** the `ConnectionMultiplexer`.</span></span> <span data-ttu-id="91a09-184">Detta kommer att stänga alla anslutningar och avsluta kommunikationen till servern.</span><span class="sxs-lookup"><span data-stu-id="91a09-184">This will close all connections and shutdown the communication to the server.</span></span>

```csharp
redisConnection.Dispose();
redisConnection = null;
```

<span data-ttu-id="91a09-185">Nu ska vi skapa ett program och utföra ett enkelt arbete med vår Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="91a09-185">Let's create an application and do some simple work with our Redis cache.</span></span>