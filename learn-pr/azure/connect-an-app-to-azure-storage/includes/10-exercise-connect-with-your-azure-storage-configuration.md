<span data-ttu-id="eb9b9-101">::: zone pivot="csharp" Vi ska nu lägga till kod för att hämta anslutningssträngen för Azure Storage-kontot från konfigurationen och använda den för att ansluta till Azure Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-101">::: zone pivot="csharp" Let's add code to retrieve the connection string from configuration and use it to connect to the Azure storage account.</span></span>

## <a name="retrieve-the-connection-string"></a><span data-ttu-id="eb9b9-102">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="eb9b9-102">Retrieve the connection string</span></span>

1. <span data-ttu-id="eb9b9-103">Öppna **Program.cs** i kodredigeraren.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-103">Open **Program.cs** in the code editor.</span></span>

1. <span data-ttu-id="eb9b9-104">Lägg till en `using`-instruktion överst i filen för att referera till `Microsoft.WindowsAzure.Storage`-namnområdet:</span><span class="sxs-lookup"><span data-stu-id="eb9b9-104">Add a `using` statement at the top of the file to reference the `Microsoft.WindowsAzure.Storage` namespace:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. <span data-ttu-id="eb9b9-105">I slutet av `Main`-metoden lägger du till följande rad för att hämta anslutningssträngen för Azure Storage-kontot från konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-105">At the end of the `Main` method, add the following line to retrieve the Azure storage account connection string from the configuration file.</span></span> <span data-ttu-id="eb9b9-106">Den överförda _nyckeln_ måste matcha namnet som används i din **appsettings.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-106">The passed _key_ must match the name used in your **appsettings.json** file.</span></span>

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a><span data-ttu-id="eb9b9-107">Skapa en blobbklient</span><span class="sxs-lookup"><span data-stu-id="eb9b9-107">Create a blob client</span></span>

1. <span data-ttu-id="eb9b9-108">Använd den statiska `CloudStorageAccount.TryParse`-metoden till att skapa ett `CloudStorageAccount`-objekt. Den använder anslutningssträngen och en `out`-parameter för att returnera objektet.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-108">Use the static `CloudStorageAccount.TryParse` method to create a `CloudStorageAccount` object - it takes the connection string and an `out` parameter to return the created object.</span></span> <span data-ttu-id="eb9b9-109">Returnerar ett `bool`-värde som visar om strängen har parsats.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-109">It returns a `bool` value indicating whether it successfully parsed the string.</span></span>
    - <span data-ttu-id="eb9b9-110">Om det misslyckas skickas ett meddelande till konsolen och returneras från metoden.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-110">If it fails, output a message to the console and return from the method.</span></span>

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString,
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. <span data-ttu-id="eb9b9-111">Använd det returnerade `CloudStorageAccount`-objektet för att skapa en blobbklient.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-111">Use the returned `CloudStorageAccount` object to create a blob client.</span></span>

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="eb9b9-112">Använd sedan blobbklienten för att hämta en referens till en container med namnet ”photoblobs”.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-112">Next, use the blob client to retrieve a reference to a container named "photoblobs".</span></span> <span data-ttu-id="eb9b9-113">Precis som med kontonamn måste blobbcontainerns namn vara gemener och bestå av bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-113">Much like the account names, the Blob container names must be lowercase and composed of letters and numbers.</span></span>

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. <span data-ttu-id="eb9b9-114">Använd `CreateIfNotExistsAsync`-metoden till att skapa containern.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-114">Use the `CreateIfNotExistsAsync` method to create the container.</span></span> <span data-ttu-id="eb9b9-115">Detta returnerar en `bool` som visar om containern har skapats.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-115">This returns a `bool` indicating whether the container was created.</span></span> <span data-ttu-id="eb9b9-116">Lagra detta i en variabel med namnet `created`.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-116">Store this in a variable named `created`.</span></span>
    - <span data-ttu-id="eb9b9-117">Observera att detta är en **async**-metod, vilket innebär att det utförs ett faktiskt nätverksanrop.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-117">Notice that this is an **async** method - that means it will perform an actual network call.</span></span>
    - <span data-ttu-id="eb9b9-118">Du måste använda `await`-nyckelorden för att kunna hämta `bool`-resultatet.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-118">You will need to use the `await` keywords to get the `bool` result.</span></span>

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. <span data-ttu-id="eb9b9-119">Eftersom vi använder `await`-nyckelordet, går vi vidare och ändrar signaturen för `Main`-metoden till `async` och returnerar en `Task`.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-119">Because we are using the `await` keyword, go ahead and change the signature for the `Main` method to be `async` and return a `Task`.</span></span>

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

    <span data-ttu-id="eb9b9-120">Du måste också lägga till en ny `using`-instruktion överst:</span><span class="sxs-lookup"><span data-stu-id="eb9b9-120">You'll also need to add a new `using` statement at the top:</span></span>

    ```csharp
    using System.Threading.Tasks;
    ```

1. <span data-ttu-id="eb9b9-121">Slutligen ser vi om vi har skapat blobbcontainern.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-121">Finally, output whether we created the Blob container.</span></span>

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. <span data-ttu-id="eb9b9-122">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-122">Save the file.</span></span>

<span data-ttu-id="eb9b9-123">Den slutgiltiga filen bör se ut så här om du vill kontrollera ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-123">The final file should look like this if you'd like to check your work.</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using Microsoft.WindowsAzure.Storage;
using System.Threading.Tasks;

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

## <a name="use-c-71-to-build-our-app"></a><span data-ttu-id="eb9b9-124">Använda C# 7.1 till att skapa vår app</span><span class="sxs-lookup"><span data-stu-id="eb9b9-124">Use C# 7.1 to build our app</span></span>

<span data-ttu-id="eb9b9-125">Stöd för `async` och `await` i `Main`-metoderna har lagts till i C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-125">Support for `async` and `await` on `Main` methods was added to C# 7.1.</span></span> <span data-ttu-id="eb9b9-126">Detta kanske inte är standardversionen av kompilatorn som du använder.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-126">This might not be the default version of the compiler you are using.</span></span> <span data-ttu-id="eb9b9-127">Låt oss kontrollera att vi använder den versionen av kompilatorn genom att konfigurera den i vår konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-127">Let's make sure we use that version of the compiler by setting it in our configuration file.</span></span>

1. <span data-ttu-id="eb9b9-128">Öppna `PhotoSharingApp.csproj` och lägg till `<LangVersion>7.1</LangVersion>` i `PropertyGroup` som anger `TargetFramework`.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-128">Open the `PhotoSharingApp.csproj` and add `<LangVersion>7.1</LangVersion>` to the `PropertyGroup` that specifies the `TargetFramework`.</span></span> <span data-ttu-id="eb9b9-129">Det bör se ut så här när du är klar:</span><span class="sxs-lookup"><span data-stu-id="eb9b9-129">It should look like this when you're finished:</span></span>

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

1. <span data-ttu-id="eb9b9-130">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-130">Save the file.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="eb9b9-131">Kör appen</span><span class="sxs-lookup"><span data-stu-id="eb9b9-131">Run the app</span></span>

1. <span data-ttu-id="eb9b9-132">Skapa och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-132">Build and run the application.</span></span> <span data-ttu-id="eb9b9-133">**Obs!** Se till att du befinner dig i rätt arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-133">**Note:** make sure you're in the correct working directory.</span></span>

    ```bash
    dotnet run
    ```

<span data-ttu-id="eb9b9-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb9b9-134">::: zone-end</span></span>

<span data-ttu-id="eb9b9-135">::: zone pivot="javascript" Nu ska vi lägga till kod för att ansluta till Azure-lagringskontot med hjälp av vår lagrade anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-135">::: zone pivot="javascript" Let's add code to connect to the Azure storage account using our stored connection string.</span></span> <span data-ttu-id="eb9b9-136">Azure-klientbiblioteket använder automatiskt miljövariabeln **AZURE_STORAGE_CONNECTION_STRING** till att hämta anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-136">The Azure client library will automatically use the **AZURE_STORAGE_CONNECTION_STRING** environment variable to get the connection string.</span></span>

## <a name="create-a-blob-client"></a><span data-ttu-id="eb9b9-137">Skapa en blobbklient</span><span class="sxs-lookup"><span data-stu-id="eb9b9-137">Create a blob client</span></span>

1. <span data-ttu-id="eb9b9-138">Öppna **index.js** i kodredigeraren.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-138">Open **index.js** in the code editor.</span></span>

1. <span data-ttu-id="eb9b9-139">Starta genom att inkludera modulen **azure-storage**.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-139">Start by including the **azure-storage** module.</span></span> <span data-ttu-id="eb9b9-140">Lagra modulen i en variabel med namnet **storage**.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-140">Store the module in a variable named **storage**.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    const storage = require('azure-storage');
    ```

1. <span data-ttu-id="eb9b9-141">Direkt efteråt använder du **storage**-objektet till att skapa `BlobService`-objektet och lagra det globalt med namnet **blobService**.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-141">Next, right after that, use the **storage** object to create the `BlobService` object and store it in a global named **blobService**.</span></span> <span data-ttu-id="eb9b9-142">Kom ihåg att detta är förenklade objekt som motsvarar åtkomst till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-142">Remember, these are lightweight objects representing access to the storage account.</span></span>

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. <span data-ttu-id="eb9b9-143">Lägg till en konstant som motsvarar den container som vi vill skapa.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-143">Add a constant to represent the container we want to create.</span></span> <span data-ttu-id="eb9b9-144">Vi ger containern namnet ”photoblobs”.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-144">We'll name the container "photoblobs".</span></span>

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a><span data-ttu-id="eb9b9-145">Skapa en container</span><span class="sxs-lookup"><span data-stu-id="eb9b9-145">Create a container</span></span>

<span data-ttu-id="eb9b9-146">Vi kan använda `BlobService`-objektet för att arbeta med blobb-API: er i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-146">We can use the `BlobService` object to work with blob APIs in Azure storage.</span></span> <span data-ttu-id="eb9b9-147">Som tidigare nämnts är alla API:er som utför nätverksanrop asynkrona för att appen ska vara responsiv.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-147">As mentioned before, all the APIs that make network calls are asynchronous to keep the app responsive.</span></span> <span data-ttu-id="eb9b9-148">`createContainerIfNotExists`-metoden är en sådan metod.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-148">The `createContainerIfNotExists` method is one such method.</span></span> <span data-ttu-id="eb9b9-149">Vi använder _promises_ till att hantera återanrop som innehåller svaret.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-149">We'll use _promises_ to handle the callback that contains the response.</span></span>

1. <span data-ttu-id="eb9b9-150">Använd `util.promisify` för återanropsversionen av `createContainerIfNotExists` och omvandla den till en metod som returnerar ett ”promise”.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-150">Use `util.promisify` to take the callback version of `createContainerIfNotExists` and turn it into a promise-returning method.</span></span>
    - <span data-ttu-id="eb9b9-151">Eftersom återanropsmetoden finns i ett objekt måste du lägga till ett `bind`-anrop i slutet för att ansluta det till denna kontext.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-151">Since the callback method is on an object, make sure to add a `bind` call at the end to connect it to that context.</span></span>
    - <span data-ttu-id="eb9b9-152">Tilldela det returnerade värdet till en konstant överst i filen som heter `createContainerAsync` enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-152">Assign the return value to a constant at the top of the file named `createContainerAsync`, as shown below.</span></span>
    - <span data-ttu-id="eb9b9-153">Du behöver också en `require` för `util`-modulen längst upp i filen.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-153">You'll also need a `require` for the `util` module near the top of the file.</span></span>

    ```javascript
    const util = require('util');
    const storage = require('azure-storage');
    const blobService = storage.createBlobService();

    const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

    const containerName = 'photoblobs';
    ```

2. <span data-ttu-id="eb9b9-154">Ta bort ”Hello World!”</span><span class="sxs-lookup"><span data-stu-id="eb9b9-154">Remove the "Hello, World!"</span></span> <span data-ttu-id="eb9b9-155">utdata från `main()`.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-155">output from `main()`.</span></span>

3. <span data-ttu-id="eb9b9-156">Anropa ditt nya `createContainerAsync`-promise.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-156">Call your new `createContainerAsync` promise.</span></span>
    - <span data-ttu-id="eb9b9-157">Skicka det till konstanten **containerName**.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-157">Pass it the **containerName** constant.</span></span>
    - <span data-ttu-id="eb9b9-158">Tillämpa sedan `await`-nyckelordet på anropet.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-158">Apply the `await` keyword to the call.</span></span>
    - <span data-ttu-id="eb9b9-159">Omslut anropet i en `try` / `catch`-konstruktion och visa eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-159">Wrap the call in a `try` / `catch` construct and output any error.</span></span>

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. <span data-ttu-id="eb9b9-160">`createContainerAsync`-promise returnerar det första värdet från den underliggande `createContainerIfNotExists`, vilket är resultatet för containern.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-160">The `createContainerAsync` promise returns the first value from the underlying `createContainerIfNotExists`, which is the container result.</span></span> <span data-ttu-id="eb9b9-161">Detta inkluderar information om containern har skapats eller inte.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-161">This includes information on whether the container was created or not.</span></span> <span data-ttu-id="eb9b9-162">Samla in resultatet i en variabel och visa om containern har skapats baserat på `result.created`-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-162">Capture the result in a variable, and output whether the container was created based on the `result.created` property.</span></span>

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

1. <span data-ttu-id="eb9b9-163">Slutligen kan du anpassa `main`-funktionen med `async`-nyckelordet.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-163">Finally, decorate the `main` function with the `async` keyword.</span></span>

1. <span data-ttu-id="eb9b9-164">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-164">Save the file.</span></span>

<span data-ttu-id="eb9b9-165">Den slutgiltiga filen bör se ut så här om du vill kontrollera ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-165">The final file should look like this, if you'd like to check your work.</span></span>

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function main() {
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

main();
```

## <a name="run-the-app"></a><span data-ttu-id="eb9b9-166">Kör appen</span><span class="sxs-lookup"><span data-stu-id="eb9b9-166">Run the app</span></span>

1. <span data-ttu-id="eb9b9-167">Skapa och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-167">Build and run the application.</span></span> <span data-ttu-id="eb9b9-168">**Obs!** Se till att du befinner dig i katalogen PhotoSharingApp.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-168">**Note:** make sure you're in the PhotoSharingApp directory.</span></span>

    ```bash
    node index.js
    ```

<span data-ttu-id="eb9b9-169">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb9b9-169">::: zone-end</span></span>

<span data-ttu-id="eb9b9-170">Den bör rapportera att blobbcontainern har skapats.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-170">It should report that the blob container was created.</span></span> <span data-ttu-id="eb9b9-171">Om du kör den en gång till, bör den informera dig om att den redan finns.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-171">If you run it a second time, it should tell you it already exists.</span></span>

<span data-ttu-id="eb9b9-172">Verifiera containern:</span><span class="sxs-lookup"><span data-stu-id="eb9b9-172">To verify the container:</span></span>

1. <span data-ttu-id="eb9b9-173">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-173">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="eb9b9-174">Navigera till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-174">Navigate to your storage account.</span></span> <span data-ttu-id="eb9b9-175">Du kan leta rätt på lagringskontot under **Alla resurser** eller söka efter namnet i _sökrutan_ längst upp i portalfönstret.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-175">You can use the **All Resources** section to find the storage account or search by name from the _search box_ at the top of the portal window.</span></span>

1. <span data-ttu-id="eb9b9-176">Välj posten **Blobbar** i lagringskontot i avsnittet **Blobbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-176">Select the **Blobs** entry of the storage account in the **Blob services** section.</span></span>

1. <span data-ttu-id="eb9b9-177">Du bör se din container **photoblobs** i blobbpanelen.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-177">You should see your **photoblobs** container in the Blobs panel.</span></span> <span data-ttu-id="eb9b9-178">Du kan ta bort containern via menyn ”...” på höger sida av posten och försöka återskapa den med din app.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-178">You can delete the container through the "..." menu on the right-hand side of the entry to try recreating it with your app.</span></span>

> [!NOTE]
> <span data-ttu-id="eb9b9-179">Containern försvinner från portalen mycket snabbt, men det tar några minuter innan den egentligen är borta.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-179">The container will disappear from the portal very quickly, but it takes a few minutes to actually delete.</span></span> <span data-ttu-id="eb9b9-180">Du får ett felsvar från Azure om du försöker återskapa den under tiden som den tas bort.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-180">You will get an error response from Azure while it is being deleted if you attempt to recreate it.</span></span>

## <a name="delete-the-app"></a><span data-ttu-id="eb9b9-181">Ta bort appen</span><span class="sxs-lookup"><span data-stu-id="eb9b9-181">Delete the app</span></span>
<span data-ttu-id="eb9b9-182">Om du inte vill behålla programmets källkod i Cloud Shell-miljön, kan du använda följande kommando för att ta bort mappen och allt innehåll.</span><span class="sxs-lookup"><span data-stu-id="eb9b9-182">If you decide you don't want to keep the application source code in your Cloud Shell environment, you can use the following command to remove the folder and all the contents.</span></span>

```bash
rm -r PhotoSharingApp/
```
