<span data-ttu-id="a173d-101">Nu när vi har en Redis-cache som skapats i Azure, kan vi skapa ett program att använda den i.</span><span class="sxs-lookup"><span data-stu-id="a173d-101">Now that we have a Redis cache created in Azure, let's create an application to use it.</span></span> <span data-ttu-id="a173d-102">Kontrollera att du har information om din anslutningssträng från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a173d-102">Make sure you have your connection string information from the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="a173d-103">Integrerad Cloud Shell är tillgängligt på höger sida.</span><span class="sxs-lookup"><span data-stu-id="a173d-103">The integrated Cloud Shell is available on the right.</span></span> <span data-ttu-id="a173d-104">Du kan använda kommandotolken till att skapa och köra exempelkoden vi bygger här, eller utföra de här stegen om du har en utvecklingsmiljökonfiguration.</span><span class="sxs-lookup"><span data-stu-id="a173d-104">You can use that command prompt to create and run the example code we are building here, or perform these steps locally if you have a development environment setup.</span></span> <span data-ttu-id="a173d-105">Om du väljer att använda Cloud Shell bör du välja Bash-gränssnittet om du uppmanas att välja.</span><span class="sxs-lookup"><span data-stu-id="a173d-105">If you decide to use the Cloud Shell, please select the Bash shell if you are prompted for a selection.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="a173d-106">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="a173d-106">Create a Console Application</span></span>

<span data-ttu-id="a173d-107">Vi använder ett enkelt konsolprogram så att vi kan fokusera på Redis-implementeringen.</span><span class="sxs-lookup"><span data-stu-id="a173d-107">We'll use a simple Console Application so we can focus on the Redis implementation.</span></span>

1. <span data-ttu-id="a173d-108">Skapa ett nytt .NET Core-konsolprogram i Cloud Shell och ge det namnet ”SportsStatsTracker”</span><span class="sxs-lookup"><span data-stu-id="a173d-108">In the Cloud Shell, create a new .NET Core Console Application, name it "SportsStatsTracker"</span></span>

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. <span data-ttu-id="a173d-109">Detta skapar en mapp för projektet. Du kan fortsätta med att ändra den aktuella katalogen.</span><span class="sxs-lookup"><span data-stu-id="a173d-109">This will create a folder for the project, go ahead and change the current directory.</span></span>

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a><span data-ttu-id="a173d-110">Lägga till anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="a173d-110">Add the connection string</span></span>

<span data-ttu-id="a173d-111">Vi lägger till den anslutningssträng som vi fick från Azure Portal i koden.</span><span class="sxs-lookup"><span data-stu-id="a173d-111">Let's add the connection string we got from the Azure portal into the code.</span></span> <span data-ttu-id="a173d-112">Lagra aldrig autentiseringsuppgifterna så här i källkoden.</span><span class="sxs-lookup"><span data-stu-id="a173d-112">Never store credentials like this in your source code.</span></span> <span data-ttu-id="a173d-113">För att det här exemplet ska vara enkelt ska vi använda en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="a173d-113">To keep this sample simple, we're going to use a configuration file.</span></span> <span data-ttu-id="a173d-114">En bättre metod för ett program på serversidan i Azure är att använda Azure Key Vault med certifikat.</span><span class="sxs-lookup"><span data-stu-id="a173d-114">A better approach for a server-side application in Azure would be to use Azure Key Vault with certificates.</span></span>

1. <span data-ttu-id="a173d-115">Skapa en ny **appsettings.json**-fil som ska läggas till i projektet.</span><span class="sxs-lookup"><span data-stu-id="a173d-115">Create a new **appsettings.json** file to add to the project.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="a173d-116">Öppna kodredigeraren genom att skriva `code .` i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="a173d-116">Open the code editor by typing `code .` in the project folder.</span></span> <span data-ttu-id="a173d-117">Om du arbetar lokalt bör du använda **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="a173d-117">If you are working locally, we recommend using **Visual Studio Code**.</span></span> <span data-ttu-id="a173d-118">De här stegen anpassas huvudsakligen med dess användning.</span><span class="sxs-lookup"><span data-stu-id="a173d-118">The steps here will mostly align with it's usage.</span></span>

1. <span data-ttu-id="a173d-119">Välj filen **appsettings.json** i redigeraren och lägg till följande text.</span><span class="sxs-lookup"><span data-stu-id="a173d-119">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="a173d-120">Klistra in anslutningssträngen i **värdet** för inställningen.</span><span class="sxs-lookup"><span data-stu-id="a173d-120">Paste your connection string into the **value** of the setting.</span></span>

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. <span data-ttu-id="a173d-121">Spara filen. I onlineredigeraren finns det en meny i det övre högra hörnet med vanliga filåtgärder.</span><span class="sxs-lookup"><span data-stu-id="a173d-121">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

1. <span data-ttu-id="a173d-122">Lägg till följande konfigurationsblock för att inkludera den nya filen i projektet och kopiera den till utdatamappen.</span><span class="sxs-lookup"><span data-stu-id="a173d-122">Add the following configuration block to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="a173d-123">Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.</span><span class="sxs-lookup"><span data-stu-id="a173d-123">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

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

1. <span data-ttu-id="a173d-124">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="a173d-124">Save the file.</span></span> <span data-ttu-id="a173d-125">(Kontrollera att detta görs, annars förlorar du ändringen när du lägger till paketet nedan!)</span><span class="sxs-lookup"><span data-stu-id="a173d-125">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="a173d-126">Lägga till stöd för läsning av en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="a173d-126">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="a173d-127">Ett .NET Core-program kräver fler NuGet-paket för att kunna läsa en JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="a173d-127">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="a173d-128">I kommandotolken i fönstret lägger du till en referens till NuGet-paketet **Microsoft.Extensions.Configuration.Json**.</span><span class="sxs-lookup"><span data-stu-id="a173d-128">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="a173d-129">Lägga till kod för att läsa konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="a173d-129">Add code to read the configuration file</span></span>

<span data-ttu-id="a173d-130">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen, måste vi aktivera funktionerna i vårt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="a173d-130">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="a173d-131">Välj **Program.cs** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a173d-131">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="a173d-132">Överst i filen visas raden **using System;**.</span><span class="sxs-lookup"><span data-stu-id="a173d-132">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="a173d-133">Lägg till följande rader med kod under den raden:</span><span class="sxs-lookup"><span data-stu-id="a173d-133">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="a173d-134">Ersätt innehållet i **Main**metoden med följande kod.</span><span class="sxs-lookup"><span data-stu-id="a173d-134">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="a173d-135">Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="a173d-135">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var config = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json")
        .Build();
    ```

<span data-ttu-id="a173d-136">Filen **Program.cs** bör se ut så här nu:</span><span class="sxs-lookup"><span data-stu-id="a173d-136">Your **Program.cs** file should now look like the following:</span></span>

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

## <a name="get-the-connection-string-from-configuration"></a><span data-ttu-id="a173d-137">Hämta anslutningssträngen från konfigurationen</span><span class="sxs-lookup"><span data-stu-id="a173d-137">Get the connection string from configuration</span></span>

1. <span data-ttu-id="a173d-138">I **Program.cs**, i slutet av **Main**-metoden, använder du den nya **config**-variabeln till att hämta anslutningssträngen och lagra den i en ny variabel med namnet  **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="a173d-138">In **Program.cs**, at the end of the **Main** method, use the new **config** variable to retrieve the connection string and store it in a new variable named **connectionString**.</span></span>
    - <span data-ttu-id="a173d-139">Variabeln **config** innehåller en indexerare där du kan skicka en sträng som hämtas från din **appSettings.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="a173d-139">The **config** variable has an indexer where you can pass in a string to retrieve from your **appSettings.json** file.</span></span>

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a><span data-ttu-id="a173d-140">Lägga till stöd för Redis-cachens .NET-klient</span><span class="sxs-lookup"><span data-stu-id="a173d-140">Add support for the Redis cache .NET client</span></span>

<span data-ttu-id="a173d-141">Nu ska vi konfigurera konsolprogrammet till att använda **StackExchange.Redis**-klienten för .NET.</span><span class="sxs-lookup"><span data-stu-id="a173d-141">Next, let's configure the console application to use the **StackExchange.Redis** client for .NET.</span></span>

1. <span data-ttu-id="a173d-142">Lägg till NuGet-paketet **StackExchange.Redis** i projektet med kommandotolken längst ned i Cloud Shell-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a173d-142">Add the **StackExchange.Redis** NuGet package to the project using the command prompt at the bottom of the Cloud Shell editor.</span></span>

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. <span data-ttu-id="a173d-143">Välj **Program.cs** i redigeraren och lägg till en `using` för namnområdet **StackExchange.Redis**</span><span class="sxs-lookup"><span data-stu-id="a173d-143">Select **Program.cs** in the editor and add a `using` for the namespace **StackExchange.Redis**</span></span>

    ```csharp
    using StackExchange.Redis;
    ```
    
<span data-ttu-id="a173d-144">När installationen är klar är Redis-cacheklienten redo för användning med ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="a173d-144">Once the installation is completed, the Redis cache client is available to use with your project.</span></span>

## <a name="connect-to-the-cache"></a><span data-ttu-id="a173d-145">Ansluta till cachen</span><span class="sxs-lookup"><span data-stu-id="a173d-145">Connect to the cache</span></span>

<span data-ttu-id="a173d-146">Nu ska vi lägga till koden för att ansluta till cachen.</span><span class="sxs-lookup"><span data-stu-id="a173d-146">Let's add the code to connect to the cache.</span></span>

1. <span data-ttu-id="a173d-147">Välj **Program.cs** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a173d-147">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="a173d-148">Skapa en `ConnectionMultiplexer` med `ConnectionMultiplexer.Connect` genom att skicka den till din anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="a173d-148">Create a `ConnectionMultiplexer` using `ConnectionMultiplexer.Connect` by passing it your connection string.</span></span> <span data-ttu-id="a173d-149">Ge det returnerade värdet namnet **cache**.</span><span class="sxs-lookup"><span data-stu-id="a173d-149">Name the returned value **cache**.</span></span>

1. <span data-ttu-id="a173d-150">Eftersom anslutningen är _kasserbar_ omsluter du den i ett `using`-block.</span><span class="sxs-lookup"><span data-stu-id="a173d-150">Since the created connection is _disposable_, wrap it in a `using` block.</span></span> <span data-ttu-id="a173d-151">Koden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="a173d-151">Your code should look something like:</span></span>

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> <span data-ttu-id="a173d-152">Anslutningen till Azure Redis Cache hanteras av `ConnectionMultiplexer`-klassen.</span><span class="sxs-lookup"><span data-stu-id="a173d-152">The connection to Azure Redis Cache is managed by the `ConnectionMultiplexer` class.</span></span> <span data-ttu-id="a173d-153">Den här klassen ska delas och återanvändas i hela ditt klientprogram.</span><span class="sxs-lookup"><span data-stu-id="a173d-153">This class should be shared and reused throughout your client application.</span></span> <span data-ttu-id="a173d-154">Vi vill _inte_ skapa någon ny anslutning för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a173d-154">We do _not_ want to create a new connection for each operation.</span></span> <span data-ttu-id="a173d-155">I stället vill vi lagra den utanför som ett fält i vår klass och återanvända den för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a173d-155">Instead, we want to store it off as a field in our class and reuse it for each operation.</span></span> <span data-ttu-id="a173d-156">Här ska vi bara använda den i **Main**-metoden, men i ett produktionsprogram ska den lagras i en klassfält eller en enkel databas.</span><span class="sxs-lookup"><span data-stu-id="a173d-156">Here we are only going to use it in the **Main** method, but in a production application, it should be stored in a class field, or a singleton.</span></span>

## <a name="add-a-value-to-the-cache"></a><span data-ttu-id="a173d-157">Lägga till ett värde i cachen</span><span class="sxs-lookup"><span data-stu-id="a173d-157">Add a value to the cache</span></span>

<span data-ttu-id="a173d-158">Nu när vi har anslutningen ska vi lägga till ett värde i cachen.</span><span class="sxs-lookup"><span data-stu-id="a173d-158">Now that we have the connection, let's add a value to the cache.</span></span>

1. <span data-ttu-id="a173d-159">I `using`-blocket när anslutningen har skapats använder du `GetDatabase`-metoden till att hämta en `IDatabase`-instans.</span><span class="sxs-lookup"><span data-stu-id="a173d-159">Inside the `using` block after the connection has been created, use the `GetDatabase` method to retrieve an `IDatabase` instance.</span></span>

1. <span data-ttu-id="a173d-160">Anropa `StringSet` i `IDatabase`-objektet för att ändra nyckeln ”test:key” till värdet ”some value”.</span><span class="sxs-lookup"><span data-stu-id="a173d-160">Call `StringSet` on the `IDatabase` object to set the key "test:key" to the value "some value".</span></span>
    - <span data-ttu-id="a173d-161">Returvärdet från `StringSet` är en `bool` som visar om nyckeln har lagts till.</span><span class="sxs-lookup"><span data-stu-id="a173d-161">the return value from `StringSet` is a `bool` indicating whether the key was added.</span></span>

1. <span data-ttu-id="a173d-162">Visa returvärdet från `StringSet` i konsolen.</span><span class="sxs-lookup"><span data-stu-id="a173d-162">Display the return value from `StringSet` onto the console.</span></span>

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a><span data-ttu-id="a173d-163">Hämta ett värde från cachen</span><span class="sxs-lookup"><span data-stu-id="a173d-163">Get a value from the cache</span></span>

1. <span data-ttu-id="a173d-164">Sedan hämtar vi värdet med hjälp av `StringGet`.</span><span class="sxs-lookup"><span data-stu-id="a173d-164">Next, retrieve the value using `StringGet`.</span></span> <span data-ttu-id="a173d-165">Detta kräver att nyckeln används till att hämta och returnera värdet.</span><span class="sxs-lookup"><span data-stu-id="a173d-165">This takes the key to retrieve and returns the value.</span></span>

1. <span data-ttu-id="a173d-166">Visa det returnerade värdet.</span><span class="sxs-lookup"><span data-stu-id="a173d-166">Output the returned value.</span></span>

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. <span data-ttu-id="a173d-167">Koden bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="a173d-167">Your code should look like this:</span></span>

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
    
1. <span data-ttu-id="a173d-168">Kör programmet för att se resultatet.</span><span class="sxs-lookup"><span data-stu-id="a173d-168">Run the application to see the result.</span></span> <span data-ttu-id="a173d-169">Skriv `dotnet run` i terminalfönstret nedanför redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a173d-169">Type `dotnet run` into the terminal window below the editor.</span></span> <span data-ttu-id="a173d-170">Kontrollera att du är i projektmappen, annars hittar den inte din kod som ska skapas och köras.</span><span class="sxs-lookup"><span data-stu-id="a173d-170">Make sure you are in the project folder or it won't find your code to build and run.</span></span>
    
    ```bash
    dotnet run
    ```
    
## <a name="use-the-async-versions-of-the-methods"></a><span data-ttu-id="a173d-171">Använda asynkrona versioner av metoderna</span><span class="sxs-lookup"><span data-stu-id="a173d-171">Use the async versions of the methods</span></span>

<span data-ttu-id="a173d-172">Vi har kunnat hämta och ange värden från cachen, men vi har använt de äldre synkrona versionerna.</span><span class="sxs-lookup"><span data-stu-id="a173d-172">We have been able to get and set values from the cache, but we are using the older synchronous versions.</span></span> <span data-ttu-id="a173d-173">I program på serversidan är det här inte en effektiv användning av våra trådar.</span><span class="sxs-lookup"><span data-stu-id="a173d-173">In server-side applications, these are not an efficient use of our threads.</span></span> <span data-ttu-id="a173d-174">I stället ska vi använda _asynkrona_ versioner av metoderna.</span><span class="sxs-lookup"><span data-stu-id="a173d-174">Instead, we want to use the _asynchronous_ versions of the methods.</span></span> <span data-ttu-id="a173d-175">Det är enkelt att hitta dem – alla har filändelsen **Async**.</span><span class="sxs-lookup"><span data-stu-id="a173d-175">You can easily spot them - they all end in **Async**.</span></span>

<span data-ttu-id="a173d-176">För att göra dessa metoder enkla att arbeta med använder vi `async` i C# och `await`-nyckelord.</span><span class="sxs-lookup"><span data-stu-id="a173d-176">To make these methods easy to work with, we can use the C# `async` and `await` keywords.</span></span> <span data-ttu-id="a173d-177">Men vi kommer att behöva använda _minst_ C# 7.1 för att dessa nyckelord ska kunna tillämpas i vår **Main**-metod.</span><span class="sxs-lookup"><span data-stu-id="a173d-177">However, we will need to be using _at least_ C# 7.1 to be able to apply these keywords to our **Main** method.</span></span>

### <a name="switch-to-c-71"></a><span data-ttu-id="a173d-178">Växla till C# 7.1</span><span class="sxs-lookup"><span data-stu-id="a173d-178">Switch to C# 7.1</span></span>

<span data-ttu-id="a173d-179">C#-`async` och `await`-nyckelord var inte giltiga nyckelord i **Main**-metoderna före C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="a173d-179">C#'s `async` and `await` keywords were not valid keywords in **Main** methods until C# 7.1.</span></span> <span data-ttu-id="a173d-180">Vi kan enkelt växla till den kompilatorn via en flagga i **.csproj**-filen.</span><span class="sxs-lookup"><span data-stu-id="a173d-180">We can easily switch to that compiler through a flag in the **.csproj** file.</span></span>

1. <span data-ttu-id="a173d-181">Öppna filen **SportsStatsTracker.csproj** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a173d-181">Open the **SportsStatsTracker.csproj** file in the editor.</span></span>

1. <span data-ttu-id="a173d-182">Lägg till `<LangVersion>7.1</LangVersion>` i den första `PropertyGroup` i versionsfilen.</span><span class="sxs-lookup"><span data-stu-id="a173d-182">Add `<LangVersion>7.1</LangVersion>` into the first `PropertyGroup` in the build file.</span></span> <span data-ttu-id="a173d-183">Den bör se ut enligt nedan när du är klar.</span><span class="sxs-lookup"><span data-stu-id="a173d-183">It should look like the following when you are finished.</span></span>
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a><span data-ttu-id="a173d-184">Använda async-nyckelordet</span><span class="sxs-lookup"><span data-stu-id="a173d-184">Apply the async keyword</span></span>

<span data-ttu-id="a173d-185">Tillämpa sedan `async`-nyckelordet på **Main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="a173d-185">Next, apply the `async` keyword to the **Main** method.</span></span> <span data-ttu-id="a173d-186">Vi måste göra tre saker.</span><span class="sxs-lookup"><span data-stu-id="a173d-186">We will have to do three things.</span></span>

1. <span data-ttu-id="a173d-187">Lägga till `async`-nyckelordet i **Main**-metodens signatur.</span><span class="sxs-lookup"><span data-stu-id="a173d-187">Add the `async` keyword onto the **Main** method signature.</span></span>
1. <span data-ttu-id="a173d-188">Ändra returtypen från `void` till `Task`.</span><span class="sxs-lookup"><span data-stu-id="a173d-188">Change the return type from `void` to `Task`.</span></span>
1. <span data-ttu-id="a173d-189">Lägga till en `using`-instruktion som inkluderar `System.Threading.Tasks`.</span><span class="sxs-lookup"><span data-stu-id="a173d-189">Add a `using` statement to include `System.Threading.Tasks`.</span></span>

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

### <a name="get-and-set-values-asynchronously"></a><span data-ttu-id="a173d-190">Hämta och ange värden asynkront</span><span class="sxs-lookup"><span data-stu-id="a173d-190">Get and set values asynchronously</span></span>

1. <span data-ttu-id="a173d-191">Använd metoderna `StringSetAsync` och `StringGetAsync` för att ange och hämta en nyckel med namnet ”counter”.</span><span class="sxs-lookup"><span data-stu-id="a173d-191">Use the `StringSetAsync` and `StringGetAsync` methods to set and retrieve a key named "counter".</span></span> <span data-ttu-id="a173d-192">Ange värdet till ”100”.</span><span class="sxs-lookup"><span data-stu-id="a173d-192">Set the value to "100".</span></span>

1. <span data-ttu-id="a173d-193">Tillämpa `await`-nyckelordet för att få resultat från varje metod.</span><span class="sxs-lookup"><span data-stu-id="a173d-193">Apply the `await` keyword to get the results from each method.</span></span>

1. <span data-ttu-id="a173d-194">Överför resultatet till konsolfönstret – precis som du gjorde med de synkrona versionerna.</span><span class="sxs-lookup"><span data-stu-id="a173d-194">Output the results to the console window - just as you did with the synchronous versions.</span></span>

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. <span data-ttu-id="a173d-195">Kör programmet igen – det bör fortfarande fungera och nu ha två värden.</span><span class="sxs-lookup"><span data-stu-id="a173d-195">Run the application again - it should still work and now have two values.</span></span>

#### <a name="increment-the-value"></a><span data-ttu-id="a173d-196">Öka värdet</span><span class="sxs-lookup"><span data-stu-id="a173d-196">Increment the value</span></span>

1. <span data-ttu-id="a173d-197">Använd `StringIncrementAsync`-metoden till att öka ditt värde i **räknaren**.</span><span class="sxs-lookup"><span data-stu-id="a173d-197">Use the `StringIncrementAsync` method to increment your **counter** value.</span></span> <span data-ttu-id="a173d-198">Skicka talet **50** som ska läggas till i räknaren.</span><span class="sxs-lookup"><span data-stu-id="a173d-198">Pass the number **50** to add to the counter.</span></span>
    - <span data-ttu-id="a173d-199">Observera att metoden tar nyckeln _och_ antingen en `long` eller `double`.</span><span class="sxs-lookup"><span data-stu-id="a173d-199">Notice that the method takes the key _and_ either a `long` or `double`.</span></span>
    - <span data-ttu-id="a173d-200">Beroende på vilka parametrar som skickades returnerar den antingen en `long` eller `double`.</span><span class="sxs-lookup"><span data-stu-id="a173d-200">Depending on the parameters passed, it either returns a `long` or `double`.</span></span>

1. <span data-ttu-id="a173d-201">Visa resultaten från metoden i konsolen.</span><span class="sxs-lookup"><span data-stu-id="a173d-201">Output the results of the method to the console.</span></span>

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a><span data-ttu-id="a173d-202">Andra åtgärder</span><span class="sxs-lookup"><span data-stu-id="a173d-202">Other operations</span></span>

<span data-ttu-id="a173d-203">Slutligen ska vi prova att köra några fler metoder med `ExecuteAsync`-stödet.</span><span class="sxs-lookup"><span data-stu-id="a173d-203">Finally, let's try executing a few additional methods with the `ExecuteAsync` support.</span></span>

1. <span data-ttu-id="a173d-204">Kör ”PING” om du vill testa serveranslutningen.</span><span class="sxs-lookup"><span data-stu-id="a173d-204">Execute "PING" to test the server connection.</span></span> <span data-ttu-id="a173d-205">Den bör svara med ”PONG”.</span><span class="sxs-lookup"><span data-stu-id="a173d-205">It should respond with "PONG".</span></span>
1. <span data-ttu-id="a173d-206">Kör ”FLUSHDB” för att rensa databasvärdena.</span><span class="sxs-lookup"><span data-stu-id="a173d-206">Execute "FLUSHDB" to clear the database values.</span></span> <span data-ttu-id="a173d-207">Den bör svara med ”OK”.</span><span class="sxs-lookup"><span data-stu-id="a173d-207">It should respond with "OK".</span></span>

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

## <a name="challenge"></a><span data-ttu-id="a173d-208">Utmaning</span><span class="sxs-lookup"><span data-stu-id="a173d-208">Challenge</span></span>

<span data-ttu-id="a173d-209">Som en utmaning kan du försöka att serialisera en objekttyp till cachen.</span><span class="sxs-lookup"><span data-stu-id="a173d-209">As a challenge, try serializing an object type to the cache.</span></span> <span data-ttu-id="a173d-210">Här är de grundläggande stegen.</span><span class="sxs-lookup"><span data-stu-id="a173d-210">Here are the basic steps.</span></span>

1. <span data-ttu-id="a173d-211">Skapa en ny `class` med några offentliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a173d-211">Create a new `class` with some public properties.</span></span> <span data-ttu-id="a173d-212">Du kan göra en egen (”Person” eller ”Bil” är populära), eller använda exemplet ”GameStats” i den föregående kursdelen.</span><span class="sxs-lookup"><span data-stu-id="a173d-212">You can invent one of your own ("Person" or "Car" are popular), or use the "GameStats" example given in the previous unit.</span></span>
1. <span data-ttu-id="a173d-213">Lägg till stöd för NuGet-paketet **Newtonsoft.Json** med hjälp av `dotnet add package`.</span><span class="sxs-lookup"><span data-stu-id="a173d-213">Add support for the **Newtonsoft.Json** NuGet package using `dotnet add package`.</span></span>
1. <span data-ttu-id="a173d-214">Lägg till en `using` för `Newtonsoft.Json`-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="a173d-214">Add a `using` for the `Newtonsoft.Json` namespace.</span></span>
1. <span data-ttu-id="a173d-215">Skapa ett av objekten.</span><span class="sxs-lookup"><span data-stu-id="a173d-215">Create one of your objects.</span></span>
1. <span data-ttu-id="a173d-216">Serialisera det med `JsonConvert.SerializeObject` och använd `StringSetAsync` för att skicka det till cachen.</span><span class="sxs-lookup"><span data-stu-id="a173d-216">Serialize it with `JsonConvert.SerializeObject` and use `StringSetAsync` to push it into the cache.</span></span>
1. <span data-ttu-id="a173d-217">Hämta tillbaka det från cachen med `StringGetAsync` och deserialisera det sedan med `JsonConvert.DeserializeObject<T>`.</span><span class="sxs-lookup"><span data-stu-id="a173d-217">Get it back from the cache with `StringGetAsync` and then deserialize it with `JsonConvert.DeserializeObject<T>`.</span></span>

