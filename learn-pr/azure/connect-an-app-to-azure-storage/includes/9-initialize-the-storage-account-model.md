Azure Storages klientbibliotek tillhandahåller en objektmodell som används för att interagera med Azure Storage-kontona. Den används för att snabbt ansluta till ett Azure Storage-konto och använda Azure Storages tjänst-API:er. 

## <a name="azure-storage-client-library-object-model"></a>Objektmodell från Azure Storages klientbibliotek

::: zone pivot="csharp"

Grunden för storage-konto: objektmodell i klientbiblioteket för .NET Core är klassen `CloudStorageAccount`. Det enklaste sättet att initiera objektmodellen är att använda `CloudStorageAccount.Parse` eller `CloudStorageAccount.TryParse` för att parsa anslutningssträngen.

> [!NOTE]
> Klientbiblioteket försöker inte ansluta förrän en åtgärd som efterfrågar detta anropas. `Parse()` och `TryParse()` säkerställer endast att anslutningssträngen är välformad: de kan de inte verifiera att kontot finns eller att nyckeln är korrekt. 

Den resulterande `CloudStorageAccount` instans som returnerades från den `Parse()` eller `TryParse()` metodanrop exponerar metoder för att skapa ett klientobjekt för att få åtkomst till Azure Blob, filer, Queue och Table storage-tjänsterna. 

Kodfragmentet nedan visar ett exempel på hur du skapar en klient för användning med bloblagring:

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` och klientobjekten är enkla och kan skapas på begäran eller i förväg för delning i ditt program. En vanlig metod är att använda `CloudStorageAccount.TryParse()` nära startpunkten för programmet för att skapa en instans och göra den tillgänglig i ditt program när du vill skapa klientinstanser.

När du har ett klientobjekt till en viss lagringstyp kan använda du metoder för att utföra verkligt arbete. För att nätverksanrop är avsiktligt asynkrona – i .NET använder vi den `async` och `await` nyckelord att arbeta med dessa metoder effektivt.

Exempelvis kan vi använda den `CloudBlobClient` att skapa en _blobbehållare_ och överföra en fil till blob storage.

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

Grunden för objektmodell för storage-konto i den **Microsoft Azure Storage-klientbibliotek för Node.js och JavaScript** är den `azurestorage` objekt. Det här skapas genom att lägga till den **azure-storage** modul till din app via en `require` instruktionen.

```javascript
const storage = require('azure-storage');
```

Det här objektet innehåller en serie _factory_ metoder som skapar specifika objekt att fungera med alla aspekter av Azure storage. Du anropar `createXXX` metoder för att skapa varje objekt.

| Typ | Metod | Returnerar |
|--------|---------|-------------|
| **Blob** | `createBlobService` | `BlobService` |
| **Tabell** | `createTableService` | `TableService` |
| **Kö** | `createQueueService` | `QueueService` |
| **Fil** | `createFileService` | `FileService` |

> [!NOTE]
> Klientbiblioteket försöker inte ansluta förrän en åtgärd som efterfrågar detta anropas. Var och en av dessa `create` metoder returnerar ett lightweight-objekt som representerar åtkomst för lagringstypen&mdash;validerar inte anslutningen eller åtkomstnyckeln som används.

När du har ett serviceobjekt till en viss lagringstyp kan använda du metoder för att utföra verkligt arbete. Metoder som gör nätverksanrop är avsiktligt asynkrona. Biblioteket stöder för närvarande _återanrop_ att returnera asynkront resultat. Här är till exempel kod som skapar en blob-behållare.

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

Den här metoden fungerar bra, men det ofta leda till en massa kod som läggs till i återanrop som kan hämta svårhanterligt. En bättre metod i JavaScript är att använda _lovar_ att arbeta med dessa metoder. Det finns flera bra bibliotek som konverterar återanrop-style metoder till löften&mdash;du kan välja den som du föredrar.

Här kan vi använder den `util.promisify` funktionen från noden och använder den `BlobService` att skapa behållaren och ladda upp en fil till blob storage. Dessutom använder vi den `async` och `await` nyckelord att arbeta med sina löften lite mer naturligt.

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

Nu ska vi prova detta i vår app.