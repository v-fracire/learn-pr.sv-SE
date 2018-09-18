<span data-ttu-id="0c27d-101">::: zone pivot="csharp" Vi ska nu lägga till kod för att hämta anslutningssträngen för Azure Storage-kontot från konfigurationen och använda den för att ansluta till Azure Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="0c27d-101">::: zone pivot="csharp" Let's add code to retrieve the connection string from configuration and use it to connect to the Azure storage account.</span></span>

## <a name="retrieve-the-connection-string"></a><span data-ttu-id="0c27d-102">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="0c27d-102">Retrieve the connection string</span></span>

1. <span data-ttu-id="0c27d-103">Välj **Program.cs** för att öppna den i kodredigeraren.</span><span class="sxs-lookup"><span data-stu-id="0c27d-103">Select **Program.cs** to open it in the code editor.</span></span>

1. <span data-ttu-id="0c27d-104">Lägg till en `using`-instruktion överst i filen för att referera till `Microsoft.WindowsAzure.Storage`-namnområdet:</span><span class="sxs-lookup"><span data-stu-id="0c27d-104">Add a `using` statement at the top of the file to reference the `Microsoft.WindowsAzure.Storage` namespace:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. <span data-ttu-id="0c27d-105">I slutet av `Main`-metoden lägger du till följande rad för att hämta anslutningssträngen för Azure Storage-kontot från konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="0c27d-105">At the end of the `Main` method, add the following line to retrieve the Azure storage account connection string from the configuration file.</span></span> <span data-ttu-id="0c27d-106">Den överförda _nyckeln_ måste matcha namnet som används i din **appsettings.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="0c27d-106">The passed _key_ must match the name used in your **appsettings.json** file.</span></span>

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a><span data-ttu-id="0c27d-107">Skapa en blobbklient</span><span class="sxs-lookup"><span data-stu-id="0c27d-107">Create a blob client</span></span>

1. <span data-ttu-id="0c27d-108">Använd den statiska `CloudStorageAccount.TryParse`-metoden till att skapa ett `CloudStorageAccount`-objekt. Den använder anslutningssträngen och en `out`-parameter för att returnera objektet.</span><span class="sxs-lookup"><span data-stu-id="0c27d-108">Use the static `CloudStorageAccount.TryParse` method to create a `CloudStorageAccount` object - it takes the connection string and an `out` parameter to return the created object.</span></span> <span data-ttu-id="0c27d-109">Returnerar ett `bool`-värde som visar om strängen har parsats.</span><span class="sxs-lookup"><span data-stu-id="0c27d-109">It returns a `bool` value indicating whether it successfully parsed the string.</span></span>
    - <span data-ttu-id="0c27d-110">Om det misslyckas skickas ett meddelande till konsolen och returneras från metoden.</span><span class="sxs-lookup"><span data-stu-id="0c27d-110">If it fails, output a message to the console and return from the method.</span></span>

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString, 
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. <span data-ttu-id="0c27d-111">Använd det returnerade `CloudStorageAccount`-objektet för att skapa en blobbklient.</span><span class="sxs-lookup"><span data-stu-id="0c27d-111">Use the returned `CloudStorageAccount` object to create a blob client.</span></span>

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="0c27d-112">Använd sedan blobbklienten för att hämta en referens till en container med namnet ”photoblobs”.</span><span class="sxs-lookup"><span data-stu-id="0c27d-112">Next, use the blob client to retrieve a reference to a container named "photoblobs".</span></span> <span data-ttu-id="0c27d-113">Precis som med kontonamn måste blobbcontainerns namn vara gemener och bestå av bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="0c27d-113">Much like the account names, the Blob container names must be lowercase and composed of letters and numbers.</span></span>

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. <span data-ttu-id="0c27d-114">Använd `CreateIfNotExistsAsync`-metoden till att skapa containern.</span><span class="sxs-lookup"><span data-stu-id="0c27d-114">Use the `CreateIfNotExistsAsync` method to create the container.</span></span> <span data-ttu-id="0c27d-115">Detta returnerar en `bool` som visar om containern har skapats.</span><span class="sxs-lookup"><span data-stu-id="0c27d-115">This returns a `bool` indicating whether the container was created.</span></span> <span data-ttu-id="0c27d-116">Lagra detta i en variabel med namnet `created`.</span><span class="sxs-lookup"><span data-stu-id="0c27d-116">Store this in a variable named `created`.</span></span>
    - <span data-ttu-id="0c27d-117">Observera att detta är en **async**-metod, vilket innebär att det utförs ett faktiskt nätverksanrop.</span><span class="sxs-lookup"><span data-stu-id="0c27d-117">Notice that this is an **async** method - that means it will perform an actual network call.</span></span>
    - <span data-ttu-id="0c27d-118">Du måste använda `await`-nyckelorden för att kunna hämta `bool`-resultatet.</span><span class="sxs-lookup"><span data-stu-id="0c27d-118">You will need to use the `await` keywords to get the `bool` result.</span></span>

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. <span data-ttu-id="0c27d-119">Eftersom vi använder `await`-nyckelordet, går vi vidare och ändrar signaturen för `Main`-metoden till `async` och returnerar en `Task`.</span><span class="sxs-lookup"><span data-stu-id="0c27d-119">Because we are using the `await` keyword, go ahead and change the signature for the `Main` method to be `async` and return a `Task`.</span></span>

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

1. <span data-ttu-id="0c27d-120">Slutligen ser vi om vi har skapat blobbcontainern.</span><span class="sxs-lookup"><span data-stu-id="0c27d-120">Finally, output whether we created the Blob container.</span></span>

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. <span data-ttu-id="0c27d-121">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="0c27d-121">Save the file.</span></span>

<span data-ttu-id="0c27d-122">Den slutgiltiga filen bör se ut så här om du vill kontrollera ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="0c27d-122">The final file should look like this if you'd like to check your work.</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;

namespace PhotoSharingApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
            var connectionString = configuration["StorageAccountConnectionString"];

            if (!CloudStorageAccount.TryParse(connectionString, out CloudStorageAccount storageAccount))
            {
                Console.WriteLine("Unable to parse connection string");
                return;
            }

            var blobClient = storageAccount.CreateCloudBlobClient();
            var blobContainer = blobClient.GetContainerReference("photoblobs");
            bool created = await blobContainer.CreateIfNotExistsAsync();

            Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
        }
    }
}
```

## <a name="use-c-71-to-build-our-app"></a><span data-ttu-id="0c27d-123">Använda C# 7.1 till att skapa vår app</span><span class="sxs-lookup"><span data-stu-id="0c27d-123">Use C# 7.1 to build our app</span></span>

<span data-ttu-id="0c27d-124">Stöd för `async` och `await` i `Main`-metoderna har lagts till i C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="0c27d-124">Support for `async` and `await` on `Main` methods was added to C# 7.1.</span></span> <span data-ttu-id="0c27d-125">Detta kanske inte är standardversionen av kompilatorn som du använder.</span><span class="sxs-lookup"><span data-stu-id="0c27d-125">This might not be the default version of the compiler you are using.</span></span> <span data-ttu-id="0c27d-126">Låt oss kontrollera att vi använder den versionen av kompilatorn genom att konfigurera den i vår konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="0c27d-126">Let's make sure we use that version of the compiler by setting it in our configuration file.</span></span>

1. <span data-ttu-id="0c27d-127">Öppna `PhotoSharingApp.csproj` och lägg till `<LangVersion>7.1</LangVersion>` i `PropertyGroup` som anger `TargetFramework`.</span><span class="sxs-lookup"><span data-stu-id="0c27d-127">Open the `PhotoSharingApp.csproj` and add `<LangVersion>7.1</LangVersion>` to the `PropertyGroup` that specifies the `TargetFramework`.</span></span> <span data-ttu-id="0c27d-128">Det bör se ut så här när du är klar:</span><span class="sxs-lookup"><span data-stu-id="0c27d-128">It should look like this when you're finished:</span></span>

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <LangVersion>7.1</LangVersion> 
        <TargetFramework>netcoreapp2.0</TargetFramework>
      </PropertyGroup>
      ...
    </Project>
    ```

1. <span data-ttu-id="0c27d-129">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="0c27d-129">Save the file.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0c27d-130">Kör appen</span><span class="sxs-lookup"><span data-stu-id="0c27d-130">Run the app</span></span>

1. <span data-ttu-id="0c27d-131">Skapa och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="0c27d-131">Build and run the application.</span></span> <span data-ttu-id="0c27d-132">**Obs!** Se till att du befinner dig i rätt arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="0c27d-132">**Note:** make sure you're in the correct working directory.</span></span>

    ```bash
    dotnet run
    ```

<span data-ttu-id="0c27d-133">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0c27d-133">::: zone-end</span></span>

<span data-ttu-id="0c27d-134">::: zone-pivot="javascript" Nu ska vi lägga till kod för att ansluta till Azure-lagringskontot med hjälp av vår lagrade anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="0c27d-134">::: zone-pivot="javascript" Let's add code to connect to the Azure storage account using our stored connection string.</span></span> <span data-ttu-id="0c27d-135">Azure-klientbiblioteket använder automatiskt miljövariabeln **AZURE_STORAGE_CONNECTION_STRING** till att hämta anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="0c27d-135">The Azure client library will automatically use the **AZURE_STORAGE_CONNECTION_STRING** environment variable to get the connection string.</span></span>

## <a name="create-a-blob-client"></a><span data-ttu-id="0c27d-136">Skapa en blobbklient</span><span class="sxs-lookup"><span data-stu-id="0c27d-136">Create a blob client</span></span>

1. <span data-ttu-id="0c27d-137">Öppna **index.js** i kodredigeraren.</span><span class="sxs-lookup"><span data-stu-id="0c27d-137">Open **index.js** in the code editor.</span></span>

1. <span data-ttu-id="0c27d-138">Starta genom att inkludera modulen **azure-storage**.</span><span class="sxs-lookup"><span data-stu-id="0c27d-138">Start by including the **azure-storage** module.</span></span> <span data-ttu-id="0c27d-139">Lagra modulen i en variabel med namnet **storage**.</span><span class="sxs-lookup"><span data-stu-id="0c27d-139">Store the module in a variable named **storage**.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();
    
    const storage = require('azure-storage');
    ```

1. <span data-ttu-id="0c27d-140">Direkt efteråt använder du **storage**-objektet till att skapa `BlobService`-objektet och lagra det globalt med namnet **blobService**.</span><span class="sxs-lookup"><span data-stu-id="0c27d-140">Next, right after that, use the **storage** object to create the `BlobService` object and store it in a global named **blobService**.</span></span> <span data-ttu-id="0c27d-141">Kom ihåg att detta är förenklade objekt som motsvarar åtkomst till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="0c27d-141">Remember, these are light-weight objects representing access to the storage account.</span></span>

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. <span data-ttu-id="0c27d-142">Lägg till en konstant som motsvarar den container som vi vill skapa.</span><span class="sxs-lookup"><span data-stu-id="0c27d-142">Add a constant to represent the container we want to create.</span></span> <span data-ttu-id="0c27d-143">Vi ger containern namnet ”photoblobs”.</span><span class="sxs-lookup"><span data-stu-id="0c27d-143">We'll name the container "photoblobs".</span></span>

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a><span data-ttu-id="0c27d-144">Skapa en container</span><span class="sxs-lookup"><span data-stu-id="0c27d-144">Create a container</span></span>

<span data-ttu-id="0c27d-145">Vi kan använda `BlobService`-objektet för att arbeta med blobb-API: er i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0c27d-145">We can use the `BlobService` object to work with blob APIs in Azure storage.</span></span> <span data-ttu-id="0c27d-146">Som tidigare nämnts är alla API:er som utför nätverksanrop asynkrona för att appen ska vara responsiv.</span><span class="sxs-lookup"><span data-stu-id="0c27d-146">As mentioned before, all the APIs that make network calls are asynchronous to keep the app responsive.</span></span> <span data-ttu-id="0c27d-147">`createContainerIfNotExists`-metoden är en sådan metod.</span><span class="sxs-lookup"><span data-stu-id="0c27d-147">The `createContainerIfNotExists` method is one such method.</span></span> <span data-ttu-id="0c27d-148">Vi använder _promises_ till att hantera återanrop som innehåller svaret.</span><span class="sxs-lookup"><span data-stu-id="0c27d-148">We'll use _promises_ to handle the callback which contains the response.</span></span>

1. <span data-ttu-id="0c27d-149">Använd `util.promisify` för återanropsversionen av `createContainerIfNotExists` och omvandla den till en metod som returnerar ett ”promise”.</span><span class="sxs-lookup"><span data-stu-id="0c27d-149">Use `util.promisify` to take the callback version of `createContainerIfNotExists` and turn it into a promise-returning method.</span></span>
    - <span data-ttu-id="0c27d-150">Eftersom återanropsmetoden finns i ett objekt måste du lägga till ett `bind`-anrop i slutet för att ansluta det till denna kontext.</span><span class="sxs-lookup"><span data-stu-id="0c27d-150">Since the callback method is on an object, make sure to add a `bind` call at the end to connect it to that context.</span></span>
    - <span data-ttu-id="0c27d-151">Tilldela det returnerade värdet till en konstant överst i filen som heter `createContainerAsync` enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="0c27d-151">Assign the return value to a constant at the top of the file named `createContainerAsync` as shown below.</span></span>

```javascript
const storage = require('azure-storage');
const blobService = storage.createBlobService();

const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';
```

1. <span data-ttu-id="0c27d-152">Ta bort ”Hello World!”</span><span class="sxs-lookup"><span data-stu-id="0c27d-152">Remove the "Hello, World!"</span></span> <span data-ttu-id="0c27d-153">utdata från `main()`.</span><span class="sxs-lookup"><span data-stu-id="0c27d-153">output from `main()`.</span></span>

1. <span data-ttu-id="0c27d-154">Anropa ditt nya `createContainerAsync`-promise.</span><span class="sxs-lookup"><span data-stu-id="0c27d-154">Call your new `createContainerAsync` promise.</span></span>
    - <span data-ttu-id="0c27d-155">Skicka det till konstanten **containerName**.</span><span class="sxs-lookup"><span data-stu-id="0c27d-155">Pass it the **containerName** constant.</span></span>
    - <span data-ttu-id="0c27d-156">Tillämpa sedan `await`-nyckelordet på anropet.</span><span class="sxs-lookup"><span data-stu-id="0c27d-156">Apply the `await` keyword to the call.</span></span>
    - <span data-ttu-id="0c27d-157">Omslut anropet i en `try` / `catch`-konstruktion och visa eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="0c27d-157">Wrap the call in a `try` / `catch` construct and output any error.</span></span>

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```
    
1. <span data-ttu-id="0c27d-158">`createContainerAsync`-promise returnerar det första värdet från den underliggande `createContainerIfNotExists`, vilket är resultatet för containern.</span><span class="sxs-lookup"><span data-stu-id="0c27d-158">The `createContainerAsync` promise returns the first value from the underlying `createContainerIfNotExists` which is the container result.</span></span> <span data-ttu-id="0c27d-159">Detta inkluderar information om containern har skapats eller inte.</span><span class="sxs-lookup"><span data-stu-id="0c27d-159">This includes information on whether the container was created or not.</span></span> <span data-ttu-id="0c27d-160">Samla in resultatet i en variabel och visa om containern har skapats baserat på `result.created`-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0c27d-160">Capture the result in a variable and output whether the container was created based on the `result.created` property.</span></span>

    ```javascript
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. <span data-ttu-id="0c27d-161">Slutligen kan du anpassa `main`-funktionen med `async`-nyckelordet.</span><span class="sxs-lookup"><span data-stu-id="0c27d-161">Finally, decorate the `main` function with the `async` keyword.</span></span>
        
1. <span data-ttu-id="0c27d-162">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="0c27d-162">Save the file.</span></span>

<span data-ttu-id="0c27d-163">Den slutgiltiga filen bör se ut så här om du vill kontrollera ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="0c27d-163">The final file should look like this if you'd like to check your work.</span></span>

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function run() {
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

run();
```

## <a name="run-the-app"></a><span data-ttu-id="0c27d-164">Kör appen</span><span class="sxs-lookup"><span data-stu-id="0c27d-164">Run the app</span></span>

1. <span data-ttu-id="0c27d-165">Skapa och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="0c27d-165">Build and run the application.</span></span> <span data-ttu-id="0c27d-166">**Obs!** Se till att du befinner dig i rätt arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="0c27d-166">**Note:** make sure you're in the correct working directory.</span></span>

    ```bash
    node index.js
    ```

<span data-ttu-id="0c27d-167">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0c27d-167">::: zone-end</span></span>

<span data-ttu-id="0c27d-168">Den bör rapportera att blobbcontainern har skapats.</span><span class="sxs-lookup"><span data-stu-id="0c27d-168">It should report that the blob container was created.</span></span> <span data-ttu-id="0c27d-169">Om du kör den en gång till, bör den informera dig om att den redan finns.</span><span class="sxs-lookup"><span data-stu-id="0c27d-169">If you run it a second time, it should tell you it already exists.</span></span>

<span data-ttu-id="0c27d-170">Verifiera containern:</span><span class="sxs-lookup"><span data-stu-id="0c27d-170">To verify the container:</span></span>

1. <span data-ttu-id="0c27d-171">Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="0c27d-171">Sign in to the [Azure Portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="0c27d-172">Gå till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0c27d-172">Navigate to your storage account.</span></span> <span data-ttu-id="0c27d-173">Du kan använda avsnittet **Alla resurser** för att hitta lagringskontot, eller söka efter namnet i _sökrutan_ överst i portalfönstret.</span><span class="sxs-lookup"><span data-stu-id="0c27d-173">You can use the **All Resources** section to find the storage account, or search by name from the _search box_ at the top of the portal window.</span></span> 

1. <span data-ttu-id="0c27d-174">Välj posten **Blobbar** i lagringskontot i avsnittet **Blobbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="0c27d-174">Select the **Blobs** entry of the storage account in the **Blob services** section.</span></span>

1. <span data-ttu-id="0c27d-175">Du bör se din container **photoblobs** i blobbpanelen.</span><span class="sxs-lookup"><span data-stu-id="0c27d-175">You should see your **photoblobs** container in the Blobs panel.</span></span> <span data-ttu-id="0c27d-176">Du kan ta bort containern via menyn ”...” på höger sida av posten och försöka återskapa den med din app.</span><span class="sxs-lookup"><span data-stu-id="0c27d-176">You can delete the container through the "..." menu on the right hand side of the entry to try re-creating it with your app.</span></span>

> [!NOTE]
> <span data-ttu-id="0c27d-177">Containern försvinner från portalen mycket snabbt, men det tar några minuter innan den egentligen är borta.</span><span class="sxs-lookup"><span data-stu-id="0c27d-177">The container will disappear from the portal very quickly, but it takes a few minutes to actually delete.</span></span> <span data-ttu-id="0c27d-178">Du får ett felsvar från Azure om du försöker återskapa den under tiden som den tas bort.</span><span class="sxs-lookup"><span data-stu-id="0c27d-178">You will get an error response from Azure while it is being deleted if you attempt to recreate it.</span></span>

## <a name="delete-the-app"></a><span data-ttu-id="0c27d-179">Ta bort appen</span><span class="sxs-lookup"><span data-stu-id="0c27d-179">Delete the app</span></span>
<span data-ttu-id="0c27d-180">Om du inte vill behålla programmets källkod i Cloud Shell-miljön, kan du använda följande kommando för att ta bort mappen och allt innehåll.</span><span class="sxs-lookup"><span data-stu-id="0c27d-180">If you decide you don't want to keep the application source code in your Cloud Shell environment, you can use the following command to remove the folder and all the contents.</span></span>

```bash
rm -r PhotoSharingApp/
```
