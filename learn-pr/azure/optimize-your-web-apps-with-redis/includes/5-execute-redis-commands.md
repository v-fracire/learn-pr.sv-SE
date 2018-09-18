Såsom nämnts tidigare är Redis en minnesintern NoSQL-databas som kan replikeras över flera servrar. Den används ofta som en cache, men kan även användas som en formell databas eller en asynkron meddelandekö. 

Den kan lagra olika datatyper och strukturer samt stöder en mängd olika kommandon som du kan utfärda för att hämta cachelagrade data eller fråga efter information om själva cachen. De data du arbetar med lagras alltid som nyckel/värde-par.

## <a name="executing-commands-on-the-redis-cache"></a>Köra kommandon på Redis-cachen

Vanligtvis använder ett klientprogram ett _klientbibliotek_ till att skapa begäranden och köra kommandon på en Redis-cache. Du kan hämta en lista med klientbibliotek direkt från [Redis klientsida](https://redis.io/clients). **StackExchange.Redis** är en populär Redis-klient med höga prestanda för språket .NET. Paketet är tillgängligt via NuGet och kan läggas till i din .NET-kod med hjälp av kommandoraden eller IDE.

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a>Ansluta till din Redis-cache med StackExchange.Redis

Kom ihåg att vi använder värdadressen, portnumret och en åtkomstnyckel för att ansluta till en Redis-server. Azure erbjuder också en _anslutningssträng_ för vissa Redis-klienter som kombinerar dessa data till en enda sträng.

### <a name="what-is-a-connection-string"></a>Vad är en anslutningssträng?

En anslutningssträng är en enskild textrad som innehåller alla uppgifter som krävs för att ansluta och autentisera till en Redis-cache i Azure. Den ser ut ungefär så här (med fälten **cache-name** och **password-here** ifyllda med verkliga värden):

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> Anslutningssträngen ska skyddas i ditt program. Om programmet finns på Azure kan du använda ett Azure Key Vault för att lagra värdet.

Du kan skicka den här strängen till **StackExchange.Redis** för att skapa en anslutning till servern. 

Observera att det finns ytterligare två parametrar i slutet: 

- **SSL** – Ser till att kommunikationen är krypterad.
- **abortConnection** – Tillåter att en anslutning skapas även om servern inte är tillgänglig just då.

Det finns flera andra [valfria parametrar](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) som du kan lägga till i strängen när du konfigurerar klientbiblioteket.

### <a name="creating-a-connection"></a>Skapa en anslutning

Det huvudsakliga anslutningsobjektet i **StackExchange.Redis** är klassen `StackExchange.Redis.ConnectionMultiplexer`. Det här objektet eliminerar processen för att ansluta till en Redis-server (eller en grupp med servrar). Det har optimerats för att effektivt hantera anslutningar och kan användas varje gång du behöver åtkomst till cachen.

Du skapar en `ConnectionMultiplexer`-instans med statisk `ConnectionMultiplexer.Connect` eller `ConnectionMultiplexer.ConnectAsync`-metoden och skickar antingen en anslutningssträng eller ett `ConfigurationOptions`-objekt. 

Här är ett enkelt exempel:

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

När du har en `ConnectionMultiplexer` finns det tre primära saker som du kanske vill göra:

1. Få åtkomst till en Redis-databas. Det är vad vi kommer att fokusera på här.
2. Använda utgivar-/prenumerationsfunktioner i Redis. Det här ligger utanför innehållet för den här modulen.
3. Få åtkomst till en enskild server för underhåll eller övervakning.

### <a name="accessing-a-redis-database"></a>Åtkomst till en Redis-databas

Redis-databasen representeras av `IDatabase`-typen. Du kan hämta en med `GetDatabase()`-metoden:

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> Det objekt som returneras från `GetDatabase` är ett förenklat objekt som inte behöver lagras. Det är bara `ConnectionMultiplexer` som måste vara aktiv.

När du har ett `IDatabase`-objekt kan du köra metoder för att interagera med cacheminnet. Alla metoder har synkrona och asynkrona versioner som returnerar `Task`-objekt för att göra dem kompatibla med nyckelorden `async` och `await`.

Här är ett exempel på att lagra nyckel/värde i cacheminnet:

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

`StringSet`-metoden returnerar en `bool` som visar ifall värdet har angetts (`true`) eller inte (`false`). Vi kan sedan hämta värdet med `StringGet`-metoden:

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a>Hämta och ange binära värden

Kom ihåg att Redis-nycklar och värden är _binärt säkra_. Samma metoder kan användas för att lagra binära data. Det finns implicita konverteringsoperatörer som använder `byte[]`-typer, så du kan arbeta med data naturligt:

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
> **StackExchange.Redis** representerar nycklar med `RedisKey`-typen. Den här klassen har implicita konverteringar till och från både `string` och `byte[]`, vilket gör att både text och binära nycklar kan användas utan problem. Värden representeras av `RedisValue`-typen. Precis som med `RedisKey`, finns det implicita konverteringar på plats så att du kan skicka `string` eller `byte[]`.

#### <a name="other-common-operations"></a>Andra vanliga åtgärder

`IDatabase`-gränssnittet innehåller flera andra metoder för att arbeta med Redis-cache. Det finns metoder för att arbeta med hashvärden, listor, uppsättningar och ordnade uppsättningar.

Här är några av de vanligaste metoderna som fungerar med enkla nycklar. Du kan [läsa källkoden](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) för gränssnittet om du vill se hela listan.

| Metod | Beskrivning |
|--------|-------------|
| `CreateBatch` | Skapar en _åtgärdsgrupp_ som kommer att skickas till servern som en enda enhet, men inte nödvändigtvis behandlas som en enhet. |
| `CreateTransaction` | Skapar en åtgärdsgrupp som kommer att skickas till servern som en enda enhet _och_ behandlas på servern som en enda enhet. |
| `KeyDelete` | Ta bort nyckel/värde. |
| `KeyExists` | Returneras om den angivna nyckeln finns i cacheminnet. |
| `KeyExpire` | Anger en tidsgräns för TTL-värdet (Time to live) för en nyckel. |
| `KeyRename` | Byter namn på en nyckel. |
| `KeyTimeToLive` | Returnerar TTL-värdet för en nyckel. |
| `KeyType` | Returnerar en strängrepresentation av typen på det värde som lagras i nyckeln. Olika typer som kan returneras är: sträng, lista, uppsättning, zset och hash. |
       
### <a name="executing-other-commands"></a>Köra andra kommandon

`IDatabase`-objektet har en `Execute`- och `ExecuteAsync`-metod som kan användas till att skicka textkommandon till Redis-servern. Exempel:

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

Metoderna `Execute` och `ExecuteAsync` returnerar ett `RedisResult`-objekt som är en datainnehavare med två egenskaper:

- `Type` returnerar en `string` som anger resultattypen – ”STRING”, ”INTEGER” osv.
- `IsNull` är ett true/false-värde som visar om resultatet är `null`.

Du kan sedan använda `ToString()` på `RedisResult` för att hämta det faktiska returvärdet.

Du kan använda `Execute` till att utföra de kommandon som stöds – till exempel kan vi hämta alla klienter som är anslutna till cachen (”CLIENT LIST”):

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

Detta visar alla anslutna klienter:

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a>Lagra mer komplexa värden
Redis är inriktade på binära säkra strängar, men du kan cachelagra objektdiagram genom att serialisera dem till ett textformat – vanligtvis XML eller JSON. För vår statistik kan vi vilja ha ett `GameStats`-objekt som ser ut så här:

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

Vi kan använda biblioteket **Newtonsoft.Json** till att omvandla en instans av det här objektet till en sträng:

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

Vi kan hämta den och omvandla den tillbaka till ett objekt genom en omvänd process:

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a>Rensa anslutningen
När du är klar med Redis-anslutningen kan du **ta bort** `ConnectionMultiplexer`. Detta kommer att stänga alla anslutningar och avsluta kommunikationen till servern.

```csharp
redisConnection.Dispose();
redisConnection = null;
```

Nu ska vi skapa ett program och utföra ett enkelt arbete med vår Redis-cache.