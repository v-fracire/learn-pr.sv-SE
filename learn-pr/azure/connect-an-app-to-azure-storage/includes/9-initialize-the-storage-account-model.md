Klientbiblioteket i Azure Storage har en objektmodell som används till att interagera med Azure Storage-konton. Den används för att snabbt kunna ansluta till ett Azure Storage-konto och använda API:erna för Azure Storage-tjänsten. 

## <a name="azure-storage-client-library-object-model"></a>Objektmodell för klientbiblioteket i Azure Storage

::: zone pivot="csharp"

Grunden till objektmodellen för lagringskonton i klientbiblioteket .NET Core är klassen `CloudStorageAccount`. Det enklaste sättet att initiera objektmodellen är att använda `CloudStorageAccount.Parse` eller `CloudStorageAccount.TryParse` till att parsa anslutningssträngen.

> [!NOTE]
> Klientbiblioteket försöker inte ansluta förrän en åtgärd som efterfrågar detta anropas. `Parse()` och `TryParse()` säkerställer endast att anslutningssträngen är välformad. Det görs ingen verifiering av att kontot finns eller att nyckeln är korrekt. 

Den `CloudStorageAccount`-instans som returneras av metodanropen `Parse()` och `TryParse()` exponerar metoder som används för att skapa ett klientobjekt för åtkomst till lagringstjänsterna Azure Blob, Files, Queue och Table. 

Kodfragmentet nedan är ett exempel på hur du kan skapa en klient för användning med bloblagring:

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` och klientobjekten är enkla och kan skapas på begäran eller i förväg för delning i ditt program. En vanlig metod är att använda `CloudStorageAccount.TryParse()` nära startpunkten för programmet till att skapa en instans och göra den tillgänglig för att skapa klientinstanser i programmet.

När du har ett klientobjekt för en viss lagringstyp kan du använda metoder för att utföra det faktiska arbetet. Metoder som gör nätverksanrop är avsiktligt asynkrona &mdash; i .NET använder vi nyckelorden `async` och `await` för att kunna arbeta effektivt med sådana metoder.

Vi kan till exempel använda `CloudBlobClient` till att skapa en _blobcontainer_ och ladda upp en fil till blobblagring.

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

::: zone-end

::: zone pivot="javascript"

Grunden till objektmodellen för lagringskonton i **Microsoft Azure Storage-klientbiblioteket för Node.js och JavaScript** är objektet `azurestorage`. Det här skapas genom att du lägger till modulen **azure-storage** i din app med en `require`-instruktion.

```javascript
const storage = require('azure-storage');
```

Det här objektet har en uppsättning _förkonfigurerade_ metoder som skapar specifika objekt för arbete med de olika delarna i Azure Storage. Du anropar metoderna `createXXX` för att skapa de olika objekten.

| Typ | Metod | Returnerar |
|--------|---------|-------------|
| **Blob** | `createBlobService` | `BlobService` |
| **Tabell** | `createTableService` | `TableService` |
| **Kö** | `createQueueService` | `QueueService` |
| **Fil** | `createFileService` | `FileService` |

> [!NOTE]
> Klientbiblioteket försöker inte ansluta förrän en åtgärd som efterfrågar detta anropas. Var och en av dessa `create`-metoder returnerar ett enkelt objekt som representerar åtkomst till lagringstypen &mdash; de validerar inte anslutningen eller åtkomstnyckeln som används.

När du har ett tjänstobjekt för en viss lagringstyp kan du använda metoder för att utföra det faktiska arbetet. Metoder som gör nätverksanrop är avsiktligt asynkrona. Biblioteket har för närvarande stöd för _återanrop_ som returnerar asynkrona resultat. Här är till exempel kod som skapar en blobcontainer.

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

Den här metoden fungerar bra, men den brukar leda till att mycket kod läggs till i återanropen vilket kan bli besvärligt att hantera. Ett bättre sätt i JavaScript är att använda _promises_ när du arbetar med de här metoderna. Det finns flera bra bibliotek som konverterar metoder av återanropstyp till promises &mdash; du kan välja det du föredrar.

Här använder vi funktionen `util.promisify` från Node och `BlobService` till att skapa containern och ladda upp en fil till blobblagring. Dessutom använder vi nyckelorden `async` och `await` så att vi kan arbeta med löftena lite mer naturligt.

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
::: zone-end

Nu ska vi prova det här i vår app.