Såsom nämnts tidigare är Redis en InMemory-NoSQL-databas som kan replikeras över flera servrar. Det används ofta som en cache, men kan användas som en formell databas eller även meddelandekö. 

Det kan lagra olika datatyper och strukturer och stöder en mängd olika kommandon kan du utfärda för att hämta cachelagrade data eller fråga information om cache själva. De data du arbetar med lagras alltid som nyckel/värde-par.

## <a name="executing-commands-on-the-redis-cache"></a>Köra kommandon på Redis-cache

Vanligtvis ett klientprogram använder en _klientbiblioteket_ att formuläret begäranden och köra kommandon på en Redis-cache. Du kan hämta en lista över klientbibliotek direkt från den [Redis-klientsida](https://redis.io/clients). **StackExchange.Redis** är en populär Redis-klient för höga prestanda för språket .NET. Paketet är tillgänglig via NuGet och kan läggas till i din .NET-kod med hjälp av kommandoraden eller IDE.

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a>Ansluta till din Redis cache med StackExchange.Redis

Kom ihåg att vi använder värdadressen, portnumret och en åtkomstnyckel för att ansluta till en Redis-server. Azure erbjuder också en _anslutningssträngen_ för vissa Redis-klienter som kombinerar dessa data till en enda sträng.

### <a name="what-is-a-connection-string"></a>Vad är en anslutningssträng?

En anslutningssträng är en enskild rad med text som innehåller alla uppgifter som krävs för att ansluta och autentisera till en Redis-cache i Azure. Den ser ut ungefär så här (med den **cachenamn** och **lösenord här** fylls fälten i verkliga värden):

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> Strängen som ska skyddas i ditt program. Om programmet finns på Azure kan du använda ett Azure Key Vault för att lagra värdet.

Du kan skicka den här strängen ska **StackExchange.Redis** att skapa en anslutning till servern. 

Observera att det finns två ytterligare parametrar i slutet: 

- **SSL** -ser till att kommunikationen är krypterad.
- **abortConnection** -tillåter anslutning skapas även om servern inte är tillgänglig då.

Det finns flera andra [valfria parametrar](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) du lägga till strängen som du konfigurerar klientbiblioteket.

### <a name="creating-a-connection"></a>Skapa en anslutning

Det huvudsakliga anslutningsobjektet i **StackExchange.Redis** är den `StackExchange.Redis.ConnectionMultiplexer` klass. Det här objektet avlägsnar processen för att ansluta till en Redis-server (eller en grupp servrar). Den har optimerats för att effektivt hantera anslutningar och ska hållas runt när du behöver åtkomst till cachen.

Du skapar en `ConnectionMultiplexer` instans med statiskhet `ConnectionMultiplexer.Connect` eller `ConnectionMultiplexer.ConnectAsync` metoden och skicka antingen en anslutningssträng eller en `ConfigurationOptions` objekt. 

Här är ett enkelt exempel:

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

När du har en `ConnectionMultiplexer`, det finns 3 primära saker som du kanske vill göra:

1. Åtkomst till en Redis-databas. Det här är vad vi fokuserar på här.
2. Kontrollera användningen av utgivare/nedsänkt text-funktioner i Redis. Det här är utanför omfånget för den här modulen.
3. Åtkomst till en enskild server för underhåll och övervakning.

### <a name="accessing-a-redis-database"></a>Åtkomst till en Redis-databas

Redis-databasen som representeras av den `IDatabase` typen. Du kan hämta en med den `GetDatabase()` metoden:

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> Det objekt som returneras från `GetDatabase` är ett lightweight-objekt och behöver inte ska lagras. Endast den `ConnectionMultiplexer` måste hållas alive.

När du har en `IDatabase` objekt kan du köra metoder för att interagera med cacheminnet. Alla metoder har synkrona och asynkrona versioner som returnera `Task` objekt för att göra dem kompatibla med den `async` och `await` nyckelord.

Här är ett exempel för att lagra nyckel/värde i cacheminnet:

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

Den `StringSet` metoden returnerar en `bool` som anger om värdet har angetts (`true`) eller inte (`false`). Vi kan sedan hämta värde med den `StringGet` metoden:

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a>Hämta och ange binärvärden

Kom ihåg att Redis nycklar och värden är _binär säker_. Dessa samma metoder kan användas för att lagra binär data. Det finns implicit konvertering operatörer att arbeta med `byte[]` skriver så att du kan arbeta med data naturligt:

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
> **StackExchange.Redis** representerar nycklar med den `RedisKey` typen. Den här klassen har implicita konverteringar till och från både `string` och `byte[]`, vilket gör att både text och binära nycklar som ska användas utan någon complication. Värden representeras av den `RedisValue` typen. Precis som med `RedisKey`, det finns implicita konverteringar på plats så att du kan skicka `string` eller `byte[]`.

#### <a name="other-common-operations"></a>Andra vanliga åtgärder

Den `IDatabase` gränssnittet innehåller flera andra metoder för att arbeta med Redis-cache. Det finns sätt att arbeta med hashvärden, listor, uppsättningar och sorterade uppsättningar.

Här är några av de vanligaste som fungerar med enkel nycklar, kan du [läsa källkoden](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) för gränssnittet för att se hela listan.

| Metod | Beskrivning |
|--------|-------------|
| `CreateBatch` | Skapar en _för operations_ som kommer att skickas till servern som en enda enhet, men inte nödvändigtvis behandlas som en enhet. |
| `CreateTransaction` | Skapar en grupp av åtgärder som kommer att skickas till servern som en enda enhet _och_ bearbetas på servern som en enda enhet. |
| `KeyDelete` | Ta bort nyckel/värde. |
| `KeyExists` | Returnerar om den angivna nyckeln finns i cacheminnet. |
| `KeyExpire` | Anger en time to live (TTL) upphör att gälla för en nyckel. |
| `KeyRename` | Byter namn på en nyckel. |
| `KeyTimeToLive` | Returnerar TTL-värde för en nyckel. |
| `KeyType` | Returnerar en strängrepresentation av typen för det värde som lagras i nyckeln. Olika typer som kan returneras: sträng, lista, Ställ in zset och -hash. |
       
### <a name="executing-other-commands"></a>Köra andra kommandon

Den `IDatabase` objekt har en `Execute` och `ExecuteAsync` metod som kan användas för att skicka kommandon för text till Redis-servern. Exempel:

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

Den `Execute` och `ExecuteAsync` metoderna returnerar en `RedisResult` -objektet som är en data-innehavare som innehåller två egenskaper:

- `Type` vilken returnerar en `string` som anger vilken typ av resultatet - ”STRING”, ”heltal” osv.
- `IsNull` ett true/false-värde för att upptäcka när resultatet är `null`.

Du kan sedan använda `ToString()` på den `RedisResult` för att hämta den faktiska returvärde.

Du kan använda `Execute` att utföra vilka kommandon som stöds – till exempel får vi alla klienter som är anslutna till cachen (”klienten lista”):

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

Detta skulle mata ut alla anslutna klienter:

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a>Lagra mer komplexa värden
Redis är enhetsinriktade runt binära säker strängar, men du kan cachelagra av objektdiagram genom att serialisera dem till ett textformat – vanligtvis XML eller JSON. Exempelvis kanske för vår statistik, har vi en `GameStats` objekt som ser ut så:

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

Vi kan använda den **Newtonsoft.Json** -biblioteket för att aktivera en instans av det här objektet till en sträng:

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

Vi kan hämta den och omvandla dem tillbaka till ett objekt med omvänd processen:

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a>Rensa anslutningen
När du är klar med Redis-anslutning, kan du **ta bort** den `ConnectionMultiplexer`. Detta kommer att stänga alla anslutningar och Stäng av kommunikation till servern.

```csharp
redisConnection.Dispose();
redisConnection = null;
```

Nu ska vi skapa ett program och utföra vissa enkel arbete med våra Redis-cache.