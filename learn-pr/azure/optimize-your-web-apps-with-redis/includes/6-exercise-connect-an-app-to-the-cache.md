<span data-ttu-id="854f3-101">Nu när vi har en Redis-cache som skapats i Azure, kan vi skapa ett program att använda den i.</span><span class="sxs-lookup"><span data-stu-id="854f3-101">Now that we have a Redis cache created in Azure, let's create an application to use it.</span></span> <span data-ttu-id="854f3-102">Kontrollera att du har information om din anslutningssträng från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="854f3-102">Make sure you have your connection string information from the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="854f3-103">Integrerad Cloud Shell är tillgängligt till höger.</span><span class="sxs-lookup"><span data-stu-id="854f3-103">The integrated Cloud Shell is available on the right.</span></span> <span data-ttu-id="854f3-104">Du kan använda den kommandotolken för att skapa och köra exempelkoden vi skapar här, eller utföra de här stegen lokalt om du har en .NET Core utvecklingsmiljökonfiguration.</span><span class="sxs-lookup"><span data-stu-id="854f3-104">You can use that command prompt to create and run the example code we are building here, or perform these steps locally if you have a .NET Core development environment setup.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="854f3-105">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="854f3-105">Create a Console Application</span></span>

<span data-ttu-id="854f3-106">Vi använder ett enkelt konsolprogram så att vi kan fokusera på Redis-implementeringen.</span><span class="sxs-lookup"><span data-stu-id="854f3-106">We'll use a simple Console Application so we can focus on the Redis implementation.</span></span>

1. <span data-ttu-id="854f3-107">Skapa ett nytt .NET Core-konsolprogram i Cloud Shell och ge det namnet ”SportsStatsTracker”</span><span class="sxs-lookup"><span data-stu-id="854f3-107">In the Cloud Shell, create a new .NET Core Console Application, name it "SportsStatsTracker"</span></span>

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. <span data-ttu-id="854f3-108">Detta skapar en mapp för projektet. Du kan fortsätta med att ändra den aktuella katalogen.</span><span class="sxs-lookup"><span data-stu-id="854f3-108">This will create a folder for the project, go ahead and change the current directory.</span></span>

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a><span data-ttu-id="854f3-109">Lägga till anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="854f3-109">Add the connection string</span></span>

<span data-ttu-id="854f3-110">Vi lägger till den anslutningssträng som vi fick från Azure Portal i koden.</span><span class="sxs-lookup"><span data-stu-id="854f3-110">Let's add the connection string we got from the Azure portal into the code.</span></span> <span data-ttu-id="854f3-111">Lagra aldrig autentiseringsuppgifterna så här i källkoden.</span><span class="sxs-lookup"><span data-stu-id="854f3-111">Never store credentials like this in your source code.</span></span> <span data-ttu-id="854f3-112">För att det här exemplet ska vara enkelt ska vi använda en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="854f3-112">To keep this sample simple, we're going to use a configuration file.</span></span> <span data-ttu-id="854f3-113">En bättre metod för ett program på serversidan i Azure är att använda Azure Key Vault med certifikat.</span><span class="sxs-lookup"><span data-stu-id="854f3-113">A better approach for a server-side application in Azure would be to use Azure Key Vault with certificates.</span></span>

1. <span data-ttu-id="854f3-114">Skapa en ny **appsettings.json**-fil som ska läggas till i projektet.</span><span class="sxs-lookup"><span data-stu-id="854f3-114">Create a new **appsettings.json** file to add to the project.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="854f3-115">Öppna kodredigeraren genom att skriva `code .` i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="854f3-115">Open the code editor by typing `code .` in the project folder.</span></span> <span data-ttu-id="854f3-116">Om du arbetar lokalt bör du använda **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="854f3-116">If you are working locally, we recommend using **Visual Studio Code**.</span></span> <span data-ttu-id="854f3-117">De här stegen anpassas huvudsakligen med dess användning.</span><span class="sxs-lookup"><span data-stu-id="854f3-117">The steps here will mostly align with it's usage.</span></span>

1. <span data-ttu-id="854f3-118">Välj filen **appsettings.json** i redigeraren och lägg till följande text.</span><span class="sxs-lookup"><span data-stu-id="854f3-118">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="854f3-119">Klistra in anslutningssträngen i **värdet** för inställningen.</span><span class="sxs-lookup"><span data-stu-id="854f3-119">Paste your connection string into the **value** of the setting.</span></span>

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. <span data-ttu-id="854f3-120">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="854f3-120">Save the changes.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="854f3-121">När du klistrar in eller ändrar koden i en fil i redigeraren, se till att spara efteråt via menyn ”...” eller använd snabbtangenten (<kbd>Ctrl + S</kbd> på Windows och Linux, <kbd>Cmd + S</kbd> på macOS).</span><span class="sxs-lookup"><span data-stu-id="854f3-121">Whenever you paste or change code into a file in the editor, make sure to save afterwards using the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="854f3-122">Klicka på filen **SportsStatsTracker.csproj** i redigeraren för att öppna den.</span><span class="sxs-lookup"><span data-stu-id="854f3-122">Click on the **SportsStatsTracker.csproj** file in the editor to open it.</span></span>

1. <span data-ttu-id="854f3-123">Lägg till följande `<ItemGroup>` konfigurationsblock i rot `<Project>`-elementet för att inkludera den nya filen i projektet och kopiera den till utdatamappen.</span><span class="sxs-lookup"><span data-stu-id="854f3-123">Add the following `<ItemGroup>` configuration block into the root `<Project>` element to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="854f3-124">Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.</span><span class="sxs-lookup"><span data-stu-id="854f3-124">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

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

1. <span data-ttu-id="854f3-125">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="854f3-125">Save the file.</span></span> <span data-ttu-id="854f3-126">(Var noga med att göra det här, annars förlorar du dina ändringar när du lägger till paketet nedan!)</span><span class="sxs-lookup"><span data-stu-id="854f3-126">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="854f3-127">Lägga till stöd för läsning från en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="854f3-127">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="854f3-128">I .NET Core-program behövs fler NuGet-paket för att kunna läsa en JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="854f3-128">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="854f3-129">Lägg till en referens till NuGet-paketet **Microsoft.Extensions.Configuration.Json** i kommandotolken i fönstret.</span><span class="sxs-lookup"><span data-stu-id="854f3-129">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="854f3-130">Lägga till kod för att läsa konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="854f3-130">Add code to read the configuration file</span></span>

<span data-ttu-id="854f3-131">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="854f3-131">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="854f3-132">Välj **Program.cs** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="854f3-132">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="854f3-133">Längst upp i filen ser du raden `using System`.</span><span class="sxs-lookup"><span data-stu-id="854f3-133">At the top of the file, a `using System` line is present.</span></span> <span data-ttu-id="854f3-134">Lägg till följande kodrader under den här raden:</span><span class="sxs-lookup"><span data-stu-id="854f3-134">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="854f3-135">Ersätt innehållet i metoden **Main** med följande kod.</span><span class="sxs-lookup"><span data-stu-id="854f3-135">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="854f3-136">Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="854f3-136">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var config = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json")
        .Build();
    ```

<span data-ttu-id="854f3-137">Filen **Program.cs** bör se ut så här nu:</span><span class="sxs-lookup"><span data-stu-id="854f3-137">Your **Program.cs** file should now look like the following:</span></span>

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

## <a name="get-the-connection-string-from-configuration"></a><span data-ttu-id="854f3-138">Hämta anslutningssträngen från konfigurationen</span><span class="sxs-lookup"><span data-stu-id="854f3-138">Get the connection string from configuration</span></span>

1. <span data-ttu-id="854f3-139">I **Program.cs**, i slutet av **Main**-metoden, använder du den nya **config**-variabeln till att hämta anslutningssträngen och lagra den i en ny variabel med namnet  **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="854f3-139">In **Program.cs**, at the end of the **Main** method, use the new **config** variable to retrieve the connection string and store it in a new variable named **connectionString**.</span></span>
    - <span data-ttu-id="854f3-140">Variabeln **config** innehåller en indexerare där du kan skicka en sträng som hämtas från din **appSettings.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="854f3-140">The **config** variable has an indexer where you can pass in a string to retrieve from your **appSettings.json** file.</span></span>

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a><span data-ttu-id="854f3-141">Lägga till stöd för Redis-cachens .NET-klient</span><span class="sxs-lookup"><span data-stu-id="854f3-141">Add support for the Redis cache .NET client</span></span>

<span data-ttu-id="854f3-142">Nu ska vi konfigurera konsolprogrammet till att använda **StackExchange.Redis**-klienten för .NET.</span><span class="sxs-lookup"><span data-stu-id="854f3-142">Next, let's configure the console application to use the **StackExchange.Redis** client for .NET.</span></span>

1. <span data-ttu-id="854f3-143">Lägg till NuGet-paketet **StackExchange.Redis** i projektet med kommandotolken längst ned i Cloud Shell-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="854f3-143">Add the **StackExchange.Redis** NuGet package to the project using the command prompt at the bottom of the Cloud Shell editor.</span></span>

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. <span data-ttu-id="854f3-144">Välj **Program.cs** i redigeraren och lägg till en `using` för namnområdet **StackExchange.Redis**</span><span class="sxs-lookup"><span data-stu-id="854f3-144">Select **Program.cs** in the editor and add a `using` for the namespace **StackExchange.Redis**</span></span>

    ```csharp
    using StackExchange.Redis;
    ```
    
<span data-ttu-id="854f3-145">När installationen är klar är Redis-cacheklienten redo för användning med ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="854f3-145">Once the installation is completed, the Redis cache client is available to use with your project.</span></span>

## <a name="connect-to-the-cache"></a><span data-ttu-id="854f3-146">Ansluta till cachen</span><span class="sxs-lookup"><span data-stu-id="854f3-146">Connect to the cache</span></span>

<span data-ttu-id="854f3-147">Nu ska vi lägga till koden för att ansluta till cachen.</span><span class="sxs-lookup"><span data-stu-id="854f3-147">Let's add the code to connect to the cache.</span></span>

1. <span data-ttu-id="854f3-148">Välj **Program.cs** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="854f3-148">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="854f3-149">Skapa en `ConnectionMultiplexer` med `ConnectionMultiplexer.Connect` genom att skicka den till din anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="854f3-149">Create a `ConnectionMultiplexer` using `ConnectionMultiplexer.Connect` by passing it your connection string.</span></span> <span data-ttu-id="854f3-150">Ge det returnerade värdet namnet **cache**.</span><span class="sxs-lookup"><span data-stu-id="854f3-150">Name the returned value **cache**.</span></span>

1. <span data-ttu-id="854f3-151">Eftersom anslutningen är _kasserbar_ omsluter du den i ett `using`-block.</span><span class="sxs-lookup"><span data-stu-id="854f3-151">Since the created connection is _disposable_, wrap it in a `using` block.</span></span> <span data-ttu-id="854f3-152">Koden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="854f3-152">Your code should look something like:</span></span>

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> <span data-ttu-id="854f3-153">Anslutningen till Azure Redis Cache hanteras av `ConnectionMultiplexer`-klassen.</span><span class="sxs-lookup"><span data-stu-id="854f3-153">The connection to Azure Redis Cache is managed by the `ConnectionMultiplexer` class.</span></span> <span data-ttu-id="854f3-154">Den här klassen ska delas och återanvändas i hela ditt klientprogram.</span><span class="sxs-lookup"><span data-stu-id="854f3-154">This class should be shared and reused throughout your client application.</span></span> <span data-ttu-id="854f3-155">Vi vill _inte_ skapa någon ny anslutning för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="854f3-155">We do _not_ want to create a new connection for each operation.</span></span> <span data-ttu-id="854f3-156">I stället vill vi lagra den utanför som ett fält i vår klass och återanvända den för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="854f3-156">Instead, we want to store it off as a field in our class and reuse it for each operation.</span></span> <span data-ttu-id="854f3-157">Här ska vi bara använda den i **Main**-metoden, men i ett produktionsprogram ska den lagras i en klassfält eller en enkel databas.</span><span class="sxs-lookup"><span data-stu-id="854f3-157">Here we are only going to use it in the **Main** method, but in a production application, it should be stored in a class field, or a singleton.</span></span>

## <a name="add-a-value-to-the-cache"></a><span data-ttu-id="854f3-158">Lägga till ett värde i cachen</span><span class="sxs-lookup"><span data-stu-id="854f3-158">Add a value to the cache</span></span>

<span data-ttu-id="854f3-159">Nu när vi har anslutningen ska vi lägga till ett värde i cachen.</span><span class="sxs-lookup"><span data-stu-id="854f3-159">Now that we have the connection, let's add a value to the cache.</span></span>

1. <span data-ttu-id="854f3-160">I `using`-blocket när anslutningen har skapats använder du `GetDatabase`-metoden till att hämta en `IDatabase`-instans.</span><span class="sxs-lookup"><span data-stu-id="854f3-160">Inside the `using` block after the connection has been created, use the `GetDatabase` method to retrieve an `IDatabase` instance.</span></span>

1. <span data-ttu-id="854f3-161">Anropa `StringSet` i `IDatabase`-objektet för att ändra nyckeln ”test:key” till värdet ”some value”.</span><span class="sxs-lookup"><span data-stu-id="854f3-161">Call `StringSet` on the `IDatabase` object to set the key "test:key" to the value "some value".</span></span>
    - <span data-ttu-id="854f3-162">Returvärdet från `StringSet` är en `bool` som visar om nyckeln har lagts till.</span><span class="sxs-lookup"><span data-stu-id="854f3-162">the return value from `StringSet` is a `bool` indicating whether the key was added.</span></span>

1. <span data-ttu-id="854f3-163">Visa returvärdet från `StringSet` i konsolen.</span><span class="sxs-lookup"><span data-stu-id="854f3-163">Display the return value from `StringSet` onto the console.</span></span>

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a><span data-ttu-id="854f3-164">Hämta ett värde från cachen</span><span class="sxs-lookup"><span data-stu-id="854f3-164">Get a value from the cache</span></span>

1. <span data-ttu-id="854f3-165">Sedan hämtar vi värdet med hjälp av `StringGet`.</span><span class="sxs-lookup"><span data-stu-id="854f3-165">Next, retrieve the value using `StringGet`.</span></span> <span data-ttu-id="854f3-166">Detta kräver att nyckeln används till att hämta och returnera värdet.</span><span class="sxs-lookup"><span data-stu-id="854f3-166">This takes the key to retrieve and returns the value.</span></span>

1. <span data-ttu-id="854f3-167">Visa det returnerade värdet.</span><span class="sxs-lookup"><span data-stu-id="854f3-167">Output the returned value.</span></span>

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. <span data-ttu-id="854f3-168">Koden bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="854f3-168">Your code should look like this:</span></span>

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
    
1. <span data-ttu-id="854f3-169">Kör programmet för att se resultatet.</span><span class="sxs-lookup"><span data-stu-id="854f3-169">Run the application to see the result.</span></span> <span data-ttu-id="854f3-170">Skriv `dotnet run` i terminalfönstret nedanför redigeraren.</span><span class="sxs-lookup"><span data-stu-id="854f3-170">Type `dotnet run` into the terminal window below the editor.</span></span> <span data-ttu-id="854f3-171">Kontrollera att du är i projektmappen, annars hittar den inte din kod som ska skapas och köras.</span><span class="sxs-lookup"><span data-stu-id="854f3-171">Make sure you are in the project folder or it won't find your code to build and run.</span></span>
    
    ```bash
    dotnet run
    ```
    
> [!TIP] 
> <span data-ttu-id="854f3-172">Om programmet inte gör det du förväntar dig utan kompilerar, kanske du inte har sparat ändringarna i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="854f3-172">If the program doesn't do what you expect, but compiles, it may be because you have not saved changes in the editor.</span></span> <span data-ttu-id="854f3-173">Kom alltid ihåg att spara ändringar när du växlar mellan terminalen och redigerarfönstren</span><span class="sxs-lookup"><span data-stu-id="854f3-173">Always remember to save changes as you switch between the terminal and the editor windows</span></span> 

## <a name="use-the-async-versions-of-the-methods"></a><span data-ttu-id="854f3-174">Använda de asynkrona versionerna av metoderna</span><span class="sxs-lookup"><span data-stu-id="854f3-174">Use the async versions of the methods</span></span>

<span data-ttu-id="854f3-175">Vi har kunnat hämta och ange värden från cachen, men vi har använt de äldre synkrona versionerna.</span><span class="sxs-lookup"><span data-stu-id="854f3-175">We have been able to get and set values from the cache, but we are using the older synchronous versions.</span></span> <span data-ttu-id="854f3-176">I program på serversidan är det här inte en effektiv användning av våra trådar.</span><span class="sxs-lookup"><span data-stu-id="854f3-176">In server-side applications, these are not an efficient use of our threads.</span></span> <span data-ttu-id="854f3-177">I stället ska vi använda _asynkrona_ versioner av metoderna.</span><span class="sxs-lookup"><span data-stu-id="854f3-177">Instead, we want to use the _asynchronous_ versions of the methods.</span></span> <span data-ttu-id="854f3-178">Det är enkelt att hitta dem – alla har filändelsen **Async**.</span><span class="sxs-lookup"><span data-stu-id="854f3-178">You can easily spot them - they all end in **Async**.</span></span>

<span data-ttu-id="854f3-179">För att göra dessa metoder enkla att arbeta med använder vi `async` i C# och `await`-nyckelord.</span><span class="sxs-lookup"><span data-stu-id="854f3-179">To make these methods easy to work with, we can use the C# `async` and `await` keywords.</span></span> <span data-ttu-id="854f3-180">Men vi kommer att behöva använda _minst_ C# 7.1 för att dessa nyckelord ska kunna tillämpas i vår **Main**-metod.</span><span class="sxs-lookup"><span data-stu-id="854f3-180">However, we will need to be using _at least_ C# 7.1 to be able to apply these keywords to our **Main** method.</span></span>

### <a name="switch-to-c-71"></a><span data-ttu-id="854f3-181">Växla till C# 7.1</span><span class="sxs-lookup"><span data-stu-id="854f3-181">Switch to C# 7.1</span></span>

<span data-ttu-id="854f3-182">C#-`async` och `await`-nyckelord var inte giltiga nyckelord i **Main**-metoderna före C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="854f3-182">C#'s `async` and `await` keywords were not valid keywords in **Main** methods until C# 7.1.</span></span> <span data-ttu-id="854f3-183">Vi kan enkelt växla till den kompilatorn via en flagga i **.csproj**-filen.</span><span class="sxs-lookup"><span data-stu-id="854f3-183">We can easily switch to that compiler through a flag in the **.csproj** file.</span></span>

1. <span data-ttu-id="854f3-184">Öppna filen **SportsStatsTracker.csproj** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="854f3-184">Open the **SportsStatsTracker.csproj** file in the editor.</span></span>

1. <span data-ttu-id="854f3-185">Lägg till `<LangVersion>7.1</LangVersion>` i den första `PropertyGroup` i versionsfilen.</span><span class="sxs-lookup"><span data-stu-id="854f3-185">Add `<LangVersion>7.1</LangVersion>` into the first `PropertyGroup` in the build file.</span></span> <span data-ttu-id="854f3-186">Den bör se ut enligt nedan när du är klar.</span><span class="sxs-lookup"><span data-stu-id="854f3-186">It should look like the following when you are finished.</span></span>
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a><span data-ttu-id="854f3-187">Använda async-nyckelordet</span><span class="sxs-lookup"><span data-stu-id="854f3-187">Apply the async keyword</span></span>

<span data-ttu-id="854f3-188">Tillämpa sedan `async`-nyckelordet på **Main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="854f3-188">Next, apply the `async` keyword to the **Main** method.</span></span> <span data-ttu-id="854f3-189">Vi måste göra tre saker.</span><span class="sxs-lookup"><span data-stu-id="854f3-189">We will have to do three things.</span></span>

1. <span data-ttu-id="854f3-190">Lägga till `async`-nyckelordet i **Main**-metodens signatur.</span><span class="sxs-lookup"><span data-stu-id="854f3-190">Add the `async` keyword onto the **Main** method signature.</span></span>
1. <span data-ttu-id="854f3-191">Ändra returtypen från `void` till `Task`.</span><span class="sxs-lookup"><span data-stu-id="854f3-191">Change the return type from `void` to `Task`.</span></span>
1. <span data-ttu-id="854f3-192">Lägga till en `using`-instruktion som inkluderar `System.Threading.Tasks`.</span><span class="sxs-lookup"><span data-stu-id="854f3-192">Add a `using` statement to include `System.Threading.Tasks`.</span></span>

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

### <a name="get-and-set-values-asynchronously"></a><span data-ttu-id="854f3-193">Hämta och ange värden asynkront</span><span class="sxs-lookup"><span data-stu-id="854f3-193">Get and set values asynchronously</span></span>

<span data-ttu-id="854f3-194">Vi kan lämna de synkrona metoderna på plats och lägga till ytterligare ett värde till cacheminnet med ett anrop till metoderna `StringSetAsync` och `StringGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="854f3-194">We can leave the synchronous methods in place, let's add a call to the `StringSetAsync` and `StringGetAsync` methods to add another value to the cache.</span></span> <span data-ttu-id="854f3-195">Ge ”counter” värdet ”100”.</span><span class="sxs-lookup"><span data-stu-id="854f3-195">Set "counter" to the value "100".</span></span>  

1. <span data-ttu-id="854f3-196">Konfigurera och hämta en nyckel med namnet ”counter” med metoderna `StringSetAsync` och `StringGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="854f3-196">Use the `StringSetAsync` and `StringGetAsync` methods to set and retrieve a key named "counter".</span></span> <span data-ttu-id="854f3-197">Ange värdet till ”100”.</span><span class="sxs-lookup"><span data-stu-id="854f3-197">Set the value to "100".</span></span>

1. <span data-ttu-id="854f3-198">Tillämpa `await`-nyckelordet för att få resultat från varje metod.</span><span class="sxs-lookup"><span data-stu-id="854f3-198">Apply the `await` keyword to get the results from each method.</span></span>

1. <span data-ttu-id="854f3-199">Överför resultatet till konsolfönstret – precis som du gjorde med de synkrona versionerna.</span><span class="sxs-lookup"><span data-stu-id="854f3-199">Output the results to the console window - just as you did with the synchronous versions.</span></span>

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. <span data-ttu-id="854f3-200">Kör programmet igen – det bör fortfarande fungera och nu ha två värden.</span><span class="sxs-lookup"><span data-stu-id="854f3-200">Run the application again - it should still work and now have two values.</span></span>

#### <a name="increment-the-value"></a><span data-ttu-id="854f3-201">Öka värdet</span><span class="sxs-lookup"><span data-stu-id="854f3-201">Increment the value</span></span>

1. <span data-ttu-id="854f3-202">Använd `StringIncrementAsync`-metoden till att öka ditt värde i **räknaren**.</span><span class="sxs-lookup"><span data-stu-id="854f3-202">Use the `StringIncrementAsync` method to increment your **counter** value.</span></span> <span data-ttu-id="854f3-203">Skicka talet **50** som ska läggas till i räknaren.</span><span class="sxs-lookup"><span data-stu-id="854f3-203">Pass the number **50** to add to the counter.</span></span>
    - <span data-ttu-id="854f3-204">Observera att metoden tar nyckeln _och_ antingen en `long` eller `double`.</span><span class="sxs-lookup"><span data-stu-id="854f3-204">Notice that the method takes the key _and_ either a `long` or `double`.</span></span>
    - <span data-ttu-id="854f3-205">Beroende på vilka parametrar som skickades returnerar den antingen en `long` eller `double`.</span><span class="sxs-lookup"><span data-stu-id="854f3-205">Depending on the parameters passed, it either returns a `long` or `double`.</span></span>

1. <span data-ttu-id="854f3-206">Visa resultaten från metoden i konsolen.</span><span class="sxs-lookup"><span data-stu-id="854f3-206">Output the results of the method to the console.</span></span>

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a><span data-ttu-id="854f3-207">Andra åtgärder</span><span class="sxs-lookup"><span data-stu-id="854f3-207">Other operations</span></span>

<span data-ttu-id="854f3-208">Slutligen ska vi prova att köra några fler metoder med `ExecuteAsync`-stödet.</span><span class="sxs-lookup"><span data-stu-id="854f3-208">Finally, let's try executing a few additional methods with the `ExecuteAsync` support.</span></span>

1. <span data-ttu-id="854f3-209">Kör ”PING” om du vill testa serveranslutningen.</span><span class="sxs-lookup"><span data-stu-id="854f3-209">Execute "PING" to test the server connection.</span></span> <span data-ttu-id="854f3-210">Den bör svara med ”PONG”.</span><span class="sxs-lookup"><span data-stu-id="854f3-210">It should respond with "PONG".</span></span>
1. <span data-ttu-id="854f3-211">Kör ”FLUSHDB” för att rensa databasvärdena.</span><span class="sxs-lookup"><span data-stu-id="854f3-211">Execute "FLUSHDB" to clear the database values.</span></span> <span data-ttu-id="854f3-212">Den bör svara med ”OK”.</span><span class="sxs-lookup"><span data-stu-id="854f3-212">It should respond with "OK".</span></span>

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

<span data-ttu-id="854f3-213">Den slutliga koden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="854f3-213">The final code should look something like:</span></span>

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

## <a name="challenge"></a><span data-ttu-id="854f3-214">Utmaning</span><span class="sxs-lookup"><span data-stu-id="854f3-214">Challenge</span></span>

<span data-ttu-id="854f3-215">Som en utmaning kan du försöka att serialisera en objekttyp till cachen.</span><span class="sxs-lookup"><span data-stu-id="854f3-215">As a challenge, try serializing an object type to the cache.</span></span> <span data-ttu-id="854f3-216">Här är de grundläggande stegen.</span><span class="sxs-lookup"><span data-stu-id="854f3-216">Here are the basic steps.</span></span>

1. <span data-ttu-id="854f3-217">Skapa en ny `class` med några offentliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="854f3-217">Create a new `class` with some public properties.</span></span> <span data-ttu-id="854f3-218">Du kan göra en egen (”Person” eller ”Bil” är populära), eller använda exemplet ”GameStats” i den föregående kursdelen.</span><span class="sxs-lookup"><span data-stu-id="854f3-218">You can invent one of your own ("Person" or "Car" are popular), or use the "GameStats" example given in the previous unit.</span></span>

1. <span data-ttu-id="854f3-219">Lägg till stöd för NuGet-paketet **Newtonsoft.Json** med hjälp av `dotnet add package`.</span><span class="sxs-lookup"><span data-stu-id="854f3-219">Add support for the **Newtonsoft.Json** NuGet package using `dotnet add package`.</span></span>

1. <span data-ttu-id="854f3-220">Lägg till en `using` för `Newtonsoft.Json`-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="854f3-220">Add a `using` for the `Newtonsoft.Json` namespace.</span></span>

1. <span data-ttu-id="854f3-221">Skapa ett av objekten.</span><span class="sxs-lookup"><span data-stu-id="854f3-221">Create one of your objects.</span></span>

1. <span data-ttu-id="854f3-222">Serialisera det med `JsonConvert.SerializeObject` och använd `StringSetAsync` för att skicka det till cachen.</span><span class="sxs-lookup"><span data-stu-id="854f3-222">Serialize it with `JsonConvert.SerializeObject` and use `StringSetAsync` to push it into the cache.</span></span>

1. <span data-ttu-id="854f3-223">Hämta tillbaka det från cachen med `StringGetAsync` och deserialisera det sedan med `JsonConvert.DeserializeObject<T>`.</span><span class="sxs-lookup"><span data-stu-id="854f3-223">Get it back from the cache with `StringGetAsync` and then deserialize it with `JsonConvert.DeserializeObject<T>`.</span></span>

