<span data-ttu-id="13c56-101">Klientbiblioteket i Azure Storage har en objektmodell som används till att interagera med Azure Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="13c56-101">The Azure Storage client library provides an object model that is used to interact with Azure storage accounts.</span></span> <span data-ttu-id="13c56-102">Den används för att snabbt kunna ansluta till ett Azure Storage-konto och använda API:erna för Azure Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="13c56-102">It's used to quickly connect to an Azure storage account and use the Azure Storage service APIs.</span></span> 

## <a name="azure-storage-client-library-object-model"></a><span data-ttu-id="13c56-103">Objektmodell för klientbiblioteket i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="13c56-103">Azure Storage client library object model</span></span>

<span data-ttu-id="13c56-104">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="13c56-104">::: zone pivot="csharp"</span></span>

<span data-ttu-id="13c56-105">Grunden till objektmodellen för lagringskonton i klientbiblioteket .NET Core är klassen `CloudStorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="13c56-105">The foundation of the storage account object model in the .NET Core client library is the class `CloudStorageAccount`.</span></span> <span data-ttu-id="13c56-106">Det enklaste sättet att initiera objektmodellen är att använda `CloudStorageAccount.Parse` eller `CloudStorageAccount.TryParse` till att parsa anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="13c56-106">The simplest way to initialize the object model is to use `CloudStorageAccount.Parse` or `CloudStorageAccount.TryParse` to parse the connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="13c56-107">Klientbiblioteket försöker inte ansluta förrän en åtgärd som efterfrågar detta anropas.</span><span class="sxs-lookup"><span data-stu-id="13c56-107">The client library will not attempt to connect until an operation is invoked that requires it.</span></span> <span data-ttu-id="13c56-108">`Parse()` och `TryParse()` säkerställer endast att anslutningssträngen är välformad. Det görs ingen verifiering av att kontot finns eller att nyckeln är korrekt.</span><span class="sxs-lookup"><span data-stu-id="13c56-108">`Parse()` and `TryParse()` only guarantee that the connection string is well-formatted; they don't verify that the account exists or that the key is correct.</span></span> 

<span data-ttu-id="13c56-109">Den `CloudStorageAccount`-instans som returneras av metodanropen `Parse()` och `TryParse()` exponerar metoder som används för att skapa ett klientobjekt för åtkomst till lagringstjänsterna Azure Blob, Files, Queue och Table.</span><span class="sxs-lookup"><span data-stu-id="13c56-109">The resulting `CloudStorageAccount` instance returned from the `Parse()` or `TryParse()` method call exposes methods to create a client objects to access the Azure Blob, Files, Queue and Table storage services.</span></span> 

<span data-ttu-id="13c56-110">Kodfragmentet nedan är ett exempel på hur du kan skapa en klient för användning med bloblagring:</span><span class="sxs-lookup"><span data-stu-id="13c56-110">The code snippet below shows an example of creating a client to use for blob storage:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

<span data-ttu-id="13c56-111">`CloudStorageAccount` och klientobjekten är enkla och kan skapas på begäran eller i förväg för delning i ditt program.</span><span class="sxs-lookup"><span data-stu-id="13c56-111">`CloudStorageAccount` and the client objects are lightweight and can be created on demand or created up front to be shared within your application.</span></span> <span data-ttu-id="13c56-112">En vanlig metod är att använda `CloudStorageAccount.TryParse()` nära startpunkten för programmet till att skapa en instans och göra den tillgänglig för att skapa klientinstanser i programmet.</span><span class="sxs-lookup"><span data-stu-id="13c56-112">A standard approach is to use `CloudStorageAccount.TryParse()` near the entry point of your application to create an instance, and make it available within your application for creating client instances.</span></span>

<span data-ttu-id="13c56-113">När du har ett klientobjekt för en viss lagringstyp kan du använda metoder för att utföra det faktiska arbetet.</span><span class="sxs-lookup"><span data-stu-id="13c56-113">Once you have a client object to a specific storage type, you can use methods to perform actual work.</span></span> <span data-ttu-id="13c56-114">Metoder som gör nätverksanrop är avsiktligt asynkrona &mdash; i .NET använder vi nyckelorden `async` och `await` för att kunna arbeta effektivt med sådana metoder.</span><span class="sxs-lookup"><span data-stu-id="13c56-114">Methods which make network calls are intentionally asynchronous &mdash; in .NET we use the `async` and `await` keywords to work with these methods efficiently.</span></span>

<span data-ttu-id="13c56-115">Vi kan till exempel använda `CloudBlobClient` till att skapa en _blobcontainer_ och ladda upp en fil till blobblagring.</span><span class="sxs-lookup"><span data-stu-id="13c56-115">As an example, we can use the `CloudBlobClient` to create a _blob container_ and upload a file to blob storage.</span></span>

```csharp
// Create a local CloudBlobContainer object. No network call.
string containerName = "myblobcontainer";
CloudBlobContainer blobContainer = blobClient.GetContainerReference(containerName);

// This makes an actual service call to the Azure Storage service. 
// Unless this call fails, the container will have been created.
await blobContainer.CreateAsync();

// Create a local object to represent our blob. No network call.
string blobName = "myphoto";
CloudBlockBlob blob = blobContainer.GetBlockBlobReference(blobName);

// This transfers data in the file to the blob on the service.
string filename = "photo.png";
await blob.UploadFromFileAsync(fileName);
```

<span data-ttu-id="13c56-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="13c56-116">::: zone-end</span></span>

<span data-ttu-id="13c56-117">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="13c56-117">::: zone pivot="javascript"</span></span>

<span data-ttu-id="13c56-118">Grunden till objektmodellen för lagringskonton i **Microsoft Azure Storage-klientbiblioteket för Node.js och JavaScript** är objektet `azurestorage`.</span><span class="sxs-lookup"><span data-stu-id="13c56-118">The foundation of the storage account object model in the **Microsoft Azure Storage Client Library for Node.js and JavaScript** is the `azurestorage` object.</span></span> <span data-ttu-id="13c56-119">Det här skapas genom att du lägger till modulen **azure-storage** i din app med en `require`-instruktion.</span><span class="sxs-lookup"><span data-stu-id="13c56-119">This is created by adding the **azure-storage** module to your app through a `require` statement.</span></span>

```javascript
const storage = require('azure-storage');
```

<span data-ttu-id="13c56-120">Det här objektet har en uppsättning _förkonfigurerade_ metoder som skapar specifika objekt för arbete med de olika delarna i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="13c56-120">This object provides a series of _factory_ methods that create specific objects to work with each facet of Azure storage.</span></span> <span data-ttu-id="13c56-121">Du anropar metoderna `createXXX` för att skapa de olika objekten.</span><span class="sxs-lookup"><span data-stu-id="13c56-121">You call `createXXX` methods to create each object.</span></span>

| <span data-ttu-id="13c56-122">Typ</span><span class="sxs-lookup"><span data-stu-id="13c56-122">Type</span></span> | <span data-ttu-id="13c56-123">Metod</span><span class="sxs-lookup"><span data-stu-id="13c56-123">Method</span></span> | <span data-ttu-id="13c56-124">Returnerar</span><span class="sxs-lookup"><span data-stu-id="13c56-124">Returns</span></span> |
|--------|---------|-------------|
| <span data-ttu-id="13c56-125">**Blob**</span><span class="sxs-lookup"><span data-stu-id="13c56-125">**Blob**</span></span> | `createBlobService` | `BlobService` |
| <span data-ttu-id="13c56-126">**Tabell**</span><span class="sxs-lookup"><span data-stu-id="13c56-126">**Table**</span></span> | `createTableService` | `TableService` |
| <span data-ttu-id="13c56-127">**Kö**</span><span class="sxs-lookup"><span data-stu-id="13c56-127">**Queue**</span></span> | `createQueueService` | `QueueService` |
| <span data-ttu-id="13c56-128">**Fil**</span><span class="sxs-lookup"><span data-stu-id="13c56-128">**File**</span></span> | `createFileService` | `FileService` |

> [!NOTE]
> <span data-ttu-id="13c56-129">Klientbiblioteket försöker inte ansluta förrän en åtgärd som efterfrågar detta anropas.</span><span class="sxs-lookup"><span data-stu-id="13c56-129">The client library will not attempt to connect until an operation is invoked that requires it.</span></span> <span data-ttu-id="13c56-130">Var och en av dessa `create`-metoder returnerar ett enkelt objekt som representerar åtkomst till lagringstypen &mdash; de validerar inte anslutningen eller åtkomstnyckeln som används.</span><span class="sxs-lookup"><span data-stu-id="13c56-130">Each of these `create` methods return a lightweight object representing access to the storage type&mdash;it does not validate the connection or the access key being used.</span></span>

<span data-ttu-id="13c56-131">När du har ett tjänstobjekt för en viss lagringstyp kan du använda metoder för att utföra det faktiska arbetet.</span><span class="sxs-lookup"><span data-stu-id="13c56-131">Once you have a service object to a specific storage type, you can use methods to perform actual work.</span></span> <span data-ttu-id="13c56-132">Metoder som gör nätverksanrop är avsiktligt asynkrona.</span><span class="sxs-lookup"><span data-stu-id="13c56-132">Methods that make network calls are intentionally asynchronous.</span></span> <span data-ttu-id="13c56-133">Biblioteket har för närvarande stöd för _återanrop_ som returnerar asynkrona resultat.</span><span class="sxs-lookup"><span data-stu-id="13c56-133">The library currently supports _callbacks_ to return asynchronous results.</span></span> <span data-ttu-id="13c56-134">Här är till exempel kod som skapar en blobcontainer.</span><span class="sxs-lookup"><span data-stu-id="13c56-134">For example, here is code that creates a blob container.</span></span>

```javascript
var azure = require('azure-storage');
var blobService = azure.createBlobService();

blobService.createContainerIfNotExists('myblobcontainer', function(err, result, response) {
  if (!err) {
    // if result.created = true, container was created.
    // if result.created = false, container already existed.
  }
});
```

<span data-ttu-id="13c56-135">Den här metoden fungerar bra, men den brukar leda till att mycket kod läggs till i återanropen vilket kan bli besvärligt att hantera.</span><span class="sxs-lookup"><span data-stu-id="13c56-135">This approach works fine, but it tends to lead to a lot of code being added into the callbacks, which can get unmanageable.</span></span> <span data-ttu-id="13c56-136">Ett bättre sätt i JavaScript är att använda _promises_ när du arbetar med de här metoderna.</span><span class="sxs-lookup"><span data-stu-id="13c56-136">A better approach in JavaScript is to use _promises_ to work with these methods.</span></span> <span data-ttu-id="13c56-137">Det finns flera bra bibliotek som konverterar metoder av återanropstyp till promises &mdash; du kan välja det du föredrar.</span><span class="sxs-lookup"><span data-stu-id="13c56-137">There are several great libraries which will convert callback-style methods into promises&mdash;you can pick the one you prefer.</span></span>

<span data-ttu-id="13c56-138">Här använder vi funktionen `util.promisify` från Node och `BlobService` till att skapa containern och ladda upp en fil till blobblagring.</span><span class="sxs-lookup"><span data-stu-id="13c56-138">Here, we'll use the `util.promisify` feature from Node and use the `BlobService` to create the container and upload a file to blob storage.</span></span> <span data-ttu-id="13c56-139">Dessutom använder vi nyckelorden `async` och `await` så att vi kan arbeta med löftena lite mer naturligt.</span><span class="sxs-lookup"><span data-stu-id="13c56-139">In addition, we'll use the `async` and `await` keywords to work with the promises a bit more naturally.</span></span>

```javascript
const util = require('util');
const storage = require('azure-storage');

const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);
const uploadBlobAsync = util.promisify(blobService.createBlockBlobFromLocalFile).bind(blobService);

async function main() {
    try {
        // This makes an actual service call to the Azure Storage service. 
        // Unless this call fails, the container will have been created.
        await createContainerAsync(containerName);

        // This transfers data in the file to the blob on the service.
        var uploadResult = await uploadBlobAsync(containerName, "myphoto", "photo.png");
        if (uploadResult) {
            console.log("blob uploaded");
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

main();
```
<span data-ttu-id="13c56-140">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="13c56-140">::: zone-end</span></span>

<span data-ttu-id="13c56-141">Nu ska vi prova det här i vår app.</span><span class="sxs-lookup"><span data-stu-id="13c56-141">Let's try this in our app.</span></span>