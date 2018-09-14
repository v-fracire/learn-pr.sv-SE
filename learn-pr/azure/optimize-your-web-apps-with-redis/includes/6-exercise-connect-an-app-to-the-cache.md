Nu när vi har en Redis-cache som skapats i Azure kan vi skapa ett program att använda den. Kontrollera att du har information om din anslutningssträng från Azure-portalen.

> [!NOTE]
> Integrerad Cloud Shell är tillgängligt på höger sida. Du kan använda den Kommandotolken du skapar och kör exempelkoden vi bygger här eller utföra de här stegen om du har en installation av utvecklingsmiljö. Om du väljer att använda Cloud Shell, Välj Bash-gränssnittet om du uppmanas för ett val.

## <a name="create-a-console-application"></a>Skapa ett konsolprogram

Vi använder ett enkelt konsolprogram så att vi kan fokusera på Redis-implementering.

1. Skapa en ny .NET Core-konsolprogram i Cloud Shell, ge den namnet ”SportsStatsTracker”

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. Detta skapar en mapp för projektet, gå vidare och ändra den aktuella katalogen.

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a>Lägg till anslutningssträngen

Vi lägger till den anslutningssträng som vi fick från Azure-portalen i koden. Lagra aldrig autentiseringsuppgifterna så här i källkoden. För att det här exemplet enkelt ska vi använda en konfigurationsfil. En bättre metod för ett program för serversidan i Azure är att använda Azure Key Vault med certifikat.

1. Skapa en ny **appsettings.json** ska läggas till i projektet.

    ```bash
    touch appsettings.json
    ```

1. Öppna kodredigeraren genom att skriva `code .` i projektmappen. Om du arbetar lokalt bör du använda **Visual Studio Code**. De här stegen anpassas huvudsakligen med dess användning.

1. Välj den **appsettings.json** i redigeraren och Lägg till följande text. Klistra in anslutningssträngen i den **värdet** för inställningen.

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. Sparar du filen - redigeraren online finns det en meny i det övre högra hörnet som har gemensamma filåtgärder.

1. Lägg till följande konfiguration block för att inkludera den nya filen i projektet och kopiera den till utdatamappen. Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. Spara filen. (Kontrollera att du gör detta eller förlorar du ändringen när du lägger till paketet nedan!)

## <a name="add-support-to-read-a-json-configuration-file"></a>Lägga till stöd för läsning från en JSON-konfigurationsfil

Ett .NET Core-program kräver ytterligare NuGet-paket för att läsa en JSON-konfigurationsfil.

1. I avsnittet kommandotolk i fönstret, lägger du till en referens till den **Microsoft.Extensions.Configuration.Json** NuGet-paketet.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Lägg till kod för att läsa konfigurationsfilen

Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.

1. Välj **Program.cs** i redigeraren.

1. Överst i filen visas raden **using System;**. Lägg till följande rader med kod under den raden:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Ersätt innehållet i den **Main** metoden med följande kod. Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.

    ```csharp
    var config = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json")
        .Build();
    ```

Filen **Program.cs** bör se ut så här nu:

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace SportsStatsTracker
{
    class Program
    {
        static void Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();
        }
    }
}
```

## <a name="get-the-connection-string-from-configuration"></a>Hämta anslutningssträngen från konfiguration

1. I **Program.cs**, i slutet av den **Main** metod, använda den nya **config** variabeln för att hämta anslutningssträngen och lagra den i en ny variabel med namnet  **connectionString**.
    - Den **config** variabeln har en indexerare där du kan skicka en sträng att hämta från dina **appSettings.json** fil.

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a>Lägg till stöd för .NET-klient för Redis cache

Därefter ska vi konfigurera konsolprogram för att använda den **StackExchange.Redis** klienten för .NET.

1. Lägg till den **StackExchange.Redis** NuGet-paket i projektet med Kommandotolken längst ned i Cloud Shell-redigeraren.

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. Välj **Program.cs** i redigeraren och Lägg till en `using` för namnområdet **StackExchange.Redis**

    ```csharp
    using StackExchange.Redis;
    ```
    
När installationen är klar är klient för Redis-cache kan användas med ditt projekt.

## <a name="connect-to-the-cache"></a>Ansluta till cachen

Vi lägger du till koden för att ansluta till cachen.

1. Välj **Program.cs** i redigeraren.

1. Skapa en `ConnectionMultiplexer` med `ConnectionMultiplexer.Connect` genom att skicka den till din anslutningssträng. Namnge det returnerade värdet **cache**.

1. Eftersom anslutningen är _disponibla_, skriva det i en `using` block. Koden bör se ut ungefär så här:

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> Anslutningen till Azure Redis Cache hanteras av den `ConnectionMultiplexer` klass. Den här klassen ska delas och återanvändas i hela ditt klientprogram. Vi gör _inte_ vill skapa en ny anslutning för varje åtgärd. I stället vill vi lagra den ut som ett fält i vår klass och återanvända den för varje åtgärd. Här vi bara ska använda den i den **Main** metoden, men i ett produktionsprogram ska den lagras i en klass fält eller en singleton.

## <a name="add-a-value-to-the-cache"></a>Lägga till ett värde i cacheminnet

Nu när vi har anslutningen kan du nu ska vi lägga till ett värde till cachen.

1. I den `using` block när anslutningen har skapats, Använd den `GetDatabase` metod för att hämta en `IDatabase` instans.

1. Anropa `StringSet` på den `IDatabase` objektet för att ändra nyckeln ”test: key” till värdet ”value”.
    - Returvärdet från `StringSet` är en `bool` som anger om nyckeln har lagts till.

1. Visa returvärdet från `StringSet` till konsolen.

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a>Hämta ett värde från cachen

1. Sedan hämtar den värdet med hjälp av `StringGet`. Detta tar nyckeln som ska hämtas och returnerar värdet.

1. Utdata värdet som returneras.

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. Koden bör se ut så här:

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;
    using StackExchange.Redis;
    
    namespace SportsStatsTracker
    {
        class Program
        {
            static void Main(string[] args)
            {
                var config = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile("appsettings.json")
                    .Build();
    
                string connectionString = config["CacheConnection"];
    
                using (var cache = ConnectionMultiplexer.Connect(connectionString))
                {
                    var db = cache.GetDatabase();
    
                    bool setValue = db.StringSet("test:key", "some value");
                    Console.WriteLine($"SET: {setValue}");
    
                    string getValue = db.StringGet("test:key");
                    Console.WriteLine($"GET: {getValue}");
                }
            }
        }
    }
    ```
    
1. Kör programmet för att se resultatet. Typ `dotnet run` i terminalfönstret nedan redigeraren. Kontrollera att du är i projektmappen eller din kod för att skapa och köra hittas inte.
    
    ```bash
    dotnet run
    ```
    
## <a name="use-the-async-versions-of-the-methods"></a>Använd de asynkron versionerna av metoder

Vi har kunnat få och ange värden från cachen, men vi använder de äldre synkron versionerna. I program på serversidan, det här är inte en effektiv användning av våra trådar. I stället ska vi använda den _asynkrona_ versioner av metoderna. Du enkelt se dem – alla filändelsen **Async**.

Vi kan använda för att göra dessa metoder ska vara enkelt att arbeta med C# `async` och `await` nyckelord. Men vi kommer att behöva använda _minst_ 7.1 C# för att kunna använda dessa nyckelord till vår **Main** metod.

### <a name="switch-to-c-71"></a>Växla till C# 7.1

C# 's `async` och `await` nyckelord var inte giltig nyckelord i **Main** metoder fram till C# 7.1. Vi kan enkelt växla till den kompilatorn via en flagga i den **.csproj** fil.

1. Öppna den **SportsStatsTracker.csproj** filen i redigeraren.

1. Lägg till `<LangVersion>7.1</LangVersion>` första `PropertyGroup` i build-filen. Det bör se ut så här när du är klar.
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>Använda async-nyckelord

Tillämpa sedan det `async` nyckelord till den **Main** metod. Vi måste göra tre saker.

1. Lägg till den `async` nyckelord till den **Main** signaturen för metoden.
1. Ändra returtypen från `void` till `Task`.
1. Lägg till en `using` instruktion att inkludera `System.Threading.Tasks`.

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
        ...
```

### <a name="get-and-set-values-asynchronously"></a>Hämta och ange värden asynkront

1. Använd den `StringSetAsync` och `StringGetAsync` metoder för att ange och hämta en nyckel med namnet ”prestandaräknaren”. Ange värdet till ”100”.

1. Tillämpa den `await` nyckelord att få resultat från varje metod.

1. Överför resultatet till konsolfönstret - precis som du gjorde med synkron versioner.

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. Kör programmet igen - bör den fortfarande fungerar och nu ha två värden.

#### <a name="increment-the-value"></a>Öka värdet

1. Använd den `StringIncrementAsync` metod för att öka din **räknaren** värde. Skickar tal **50** att lägga till räknaren.
    - Observera att metoden tar nyckeln _och_ antingen en `long` eller `double`.
    - Beroende på parametrarna som skickades den antingen returnerar en `long` eller `double`.

1. Matar ut resultaten av metoden i konsolen.

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a>Andra åtgärder

Slutligen ska vi prova köra några ytterligare metoder med den `ExecuteAsync` stöd.

1. Kör ”PINGA” om du vill testa serveranslutning. Den bör svara med ”PONG”.
1. Kör ”FLUSHDB” att rensa databasvärdena. Den bör svara med ”OK”.

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

## <a name="challenge"></a>Utmaningen

Som en utmaning och försök att serialisera en objekttyp till cachen. Här är de grundläggande stegen.

1. Skapa en ny `class` med vissa offentliga egenskaper. Du kan göra en egen (”Person” eller ”bil” är populära) eller Använd exemplet ”GameStats” i den föregående enheten.
1. Lägg till stöd för den **Newtonsoft.Json** NuGet med `dotnet add package`.
1. Lägg till en `using` för den `Newtonsoft.Json` namnområde.
1. Skapa ett av objekten.
1. Serialisera med `JsonConvert.SerializeObject` och använda `StringSetAsync` att skicka den till cachen.
1. Få tillbaka den från cachen med `StringGetAsync` och sedan deserialisera det med `JsonConvert.DeserializeObject<T>`.

