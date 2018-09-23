Nu när vi har en Redis-cache som skapats i Azure, kan vi skapa ett program att använda den i. Kontrollera att du har information om din anslutningssträng från Azure Portal.

> [!NOTE]
> Integrerad Cloud Shell är tillgängligt till höger. Du kan använda den kommandotolken för att skapa och köra exempelkoden vi skapar här, eller utföra de här stegen lokalt om du har en .NET Core utvecklingsmiljökonfiguration.

## <a name="create-a-console-application"></a>Skapa ett konsolprogram

Vi använder ett enkelt konsolprogram så att vi kan fokusera på Redis-implementeringen.

1. Skapa ett nytt .NET Core-konsolprogram i Cloud Shell och ge det namnet ”SportsStatsTracker”

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. Detta skapar en mapp för projektet. Du kan fortsätta med att ändra den aktuella katalogen.

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a>Lägga till anslutningssträngen

Vi lägger till den anslutningssträng som vi fick från Azure Portal i koden. Lagra aldrig autentiseringsuppgifterna så här i källkoden. För att det här exemplet ska vara enkelt ska vi använda en konfigurationsfil. En bättre metod för ett program på serversidan i Azure är att använda Azure Key Vault med certifikat.

1. Skapa en ny **appsettings.json**-fil som ska läggas till i projektet.

    ```bash
    touch appsettings.json
    ```

1. Öppna kodredigeraren genom att skriva `code .` i projektmappen. Om du arbetar lokalt bör du använda **Visual Studio Code**. De här stegen anpassas huvudsakligen med dess användning.

1. Välj filen **appsettings.json** i redigeraren och lägg till följande text. Klistra in anslutningssträngen i **värdet** för inställningen.

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. Spara ändringarna.

    > [!IMPORTANT]
    > När du klistrar in eller ändrar koden i en fil i redigeraren, se till att spara efteråt via menyn ”...” eller använd snabbtangenten (<kbd>Ctrl + S</kbd> på Windows och Linux, <kbd>Cmd + S</kbd> på macOS).

1. Klicka på filen **SportsStatsTracker.csproj** i redigeraren för att öppna den.

1. Lägg till följande `<ItemGroup>` konfigurationsblock i rot `<Project>`-elementet för att inkludera den nya filen i projektet och kopiera den till utdatamappen. Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.

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

1. Spara filen. (Var noga med att göra det här, annars förlorar du dina ändringar när du lägger till paketet nedan!)

## <a name="add-support-to-read-a-json-configuration-file"></a>Lägga till stöd för läsning från en JSON-konfigurationsfil

I .NET Core-program behövs fler NuGet-paket för att kunna läsa en JSON-konfigurationsfil.

1. Lägg till en referens till NuGet-paketet **Microsoft.Extensions.Configuration.Json** i kommandotolken i fönstret.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Lägga till kod för att läsa konfigurationsfilen

Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.

1. Välj **Program.cs** i redigeraren.

1. Längst upp i filen ser du raden `using System`. Lägg till följande kodrader under den här raden:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Ersätt innehållet i metoden **Main** med följande kod. Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.

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

## <a name="get-the-connection-string-from-configuration"></a>Hämta anslutningssträngen från konfigurationen

1. I **Program.cs**, i slutet av **Main**-metoden, använder du den nya **config**-variabeln till att hämta anslutningssträngen och lagra den i en ny variabel med namnet  **connectionString**.
    - Variabeln **config** innehåller en indexerare där du kan skicka en sträng som hämtas från din **appSettings.json**-fil.

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a>Lägga till stöd för Redis-cachens .NET-klient

Nu ska vi konfigurera konsolprogrammet till att använda **StackExchange.Redis**-klienten för .NET.

1. Lägg till NuGet-paketet **StackExchange.Redis** i projektet med kommandotolken längst ned i Cloud Shell-redigeraren.

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. Välj **Program.cs** i redigeraren och lägg till en `using` för namnområdet **StackExchange.Redis**

    ```csharp
    using StackExchange.Redis;
    ```
    
När installationen är klar är Redis-cacheklienten redo för användning med ditt projekt.

## <a name="connect-to-the-cache"></a>Ansluta till cachen

Nu ska vi lägga till koden för att ansluta till cachen.

1. Välj **Program.cs** i redigeraren.

1. Skapa en `ConnectionMultiplexer` med `ConnectionMultiplexer.Connect` genom att skicka den till din anslutningssträng. Ge det returnerade värdet namnet **cache**.

1. Eftersom anslutningen är _kasserbar_ omsluter du den i ett `using`-block. Koden bör se ut ungefär så här:

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> Anslutningen till Azure Redis Cache hanteras av `ConnectionMultiplexer`-klassen. Den här klassen ska delas och återanvändas i hela ditt klientprogram. Vi vill _inte_ skapa någon ny anslutning för varje åtgärd. I stället vill vi lagra den utanför som ett fält i vår klass och återanvända den för varje åtgärd. Här ska vi bara använda den i **Main**-metoden, men i ett produktionsprogram ska den lagras i en klassfält eller en enkel databas.

## <a name="add-a-value-to-the-cache"></a>Lägga till ett värde i cachen

Nu när vi har anslutningen ska vi lägga till ett värde i cachen.

1. I `using`-blocket när anslutningen har skapats använder du `GetDatabase`-metoden till att hämta en `IDatabase`-instans.

1. Anropa `StringSet` i `IDatabase`-objektet för att ändra nyckeln ”test:key” till värdet ”some value”.
    - Returvärdet från `StringSet` är en `bool` som visar om nyckeln har lagts till.

1. Visa returvärdet från `StringSet` i konsolen.

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a>Hämta ett värde från cachen

1. Sedan hämtar vi värdet med hjälp av `StringGet`. Detta kräver att nyckeln används till att hämta och returnera värdet.

1. Visa det returnerade värdet.

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
    
1. Kör programmet för att se resultatet. Skriv `dotnet run` i terminalfönstret nedanför redigeraren. Kontrollera att du är i projektmappen, annars hittar den inte din kod som ska skapas och köras.
    
    ```bash
    dotnet run
    ```
    
> [!TIP] 
> Om programmet inte gör det du förväntar dig utan kompilerar, kanske du inte har sparat ändringarna i redigeraren. Kom alltid ihåg att spara ändringar när du växlar mellan terminalen och redigerarfönstren 

## <a name="use-the-async-versions-of-the-methods"></a>Använda de asynkrona versionerna av metoderna

Vi har kunnat hämta och ange värden från cachen, men vi har använt de äldre synkrona versionerna. I program på serversidan är det här inte en effektiv användning av våra trådar. I stället ska vi använda _asynkrona_ versioner av metoderna. Det är enkelt att hitta dem – alla har filändelsen **Async**.

För att göra dessa metoder enkla att arbeta med använder vi `async` i C# och `await`-nyckelord. Men vi kommer att behöva använda _minst_ C# 7.1 för att dessa nyckelord ska kunna tillämpas i vår **Main**-metod.

### <a name="switch-to-c-71"></a>Växla till C# 7.1

C#-`async` och `await`-nyckelord var inte giltiga nyckelord i **Main**-metoderna före C# 7.1. Vi kan enkelt växla till den kompilatorn via en flagga i **.csproj**-filen.

1. Öppna filen **SportsStatsTracker.csproj** i redigeraren.

1. Lägg till `<LangVersion>7.1</LangVersion>` i den första `PropertyGroup` i versionsfilen. Den bör se ut enligt nedan när du är klar.
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>Använda async-nyckelordet

Tillämpa sedan `async`-nyckelordet på **Main**-metoden. Vi måste göra tre saker.

1. Lägga till `async`-nyckelordet i **Main**-metodens signatur.
1. Ändra returtypen från `void` till `Task`.
1. Lägga till en `using`-instruktion som inkluderar `System.Threading.Tasks`.

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

Vi kan lämna de synkrona metoderna på plats och lägga till ytterligare ett värde till cacheminnet med ett anrop till metoderna `StringSetAsync` och `StringGetAsync`. Ge ”counter” värdet ”100”.  

1. Konfigurera och hämta en nyckel med namnet ”counter” med metoderna `StringSetAsync` och `StringGetAsync`. Ange värdet till ”100”.

1. Tillämpa `await`-nyckelordet för att få resultat från varje metod.

1. Överför resultatet till konsolfönstret – precis som du gjorde med de synkrona versionerna.

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. Kör programmet igen – det bör fortfarande fungera och nu ha två värden.

#### <a name="increment-the-value"></a>Öka värdet

1. Använd `StringIncrementAsync`-metoden till att öka ditt värde i **räknaren**. Skicka talet **50** som ska läggas till i räknaren.
    - Observera att metoden tar nyckeln _och_ antingen en `long` eller `double`.
    - Beroende på vilka parametrar som skickades returnerar den antingen en `long` eller `double`.

1. Visa resultaten från metoden i konsolen.

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a>Andra åtgärder

Slutligen ska vi prova att köra några fler metoder med `ExecuteAsync`-stödet.

1. Kör ”PING” om du vill testa serveranslutningen. Den bör svara med ”PONG”.
1. Kör ”FLUSHDB” för att rensa databasvärdena. Den bör svara med ”OK”.

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

Den slutliga koden bör se ut ungefär så här:

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

                setValue = await db.StringSetAsync("test", "100");
                Console.WriteLine($"SET: {setValue}");

                getValue = await db.StringGetAsync("test");
                Console.WriteLine($"GET: {getValue}");

                var result = await db.ExecuteAsync("ping");
                Console.WriteLine($"PING = {result.Type} : {result}");
                
                result = await db.ExecuteAsync("flushdb");
                Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
            }
        }
    }
}
```

## <a name="challenge"></a>Utmaning

Som en utmaning kan du försöka att serialisera en objekttyp till cachen. Här är de grundläggande stegen.

1. Skapa en ny `class` med några offentliga egenskaper. Du kan göra en egen (”Person” eller ”Bil” är populära), eller använda exemplet ”GameStats” i den föregående kursdelen.

1. Lägg till stöd för NuGet-paketet **Newtonsoft.Json** med hjälp av `dotnet add package`.

1. Lägg till en `using` för `Newtonsoft.Json`-namnområdet.

1. Skapa ett av objekten.

1. Serialisera det med `JsonConvert.SerializeObject` och använd `StringSetAsync` för att skicka det till cachen.

1. Hämta tillbaka det från cachen med `StringGetAsync` och deserialisera det sedan med `JsonConvert.DeserializeObject<T>`.

