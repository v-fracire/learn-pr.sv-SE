<span data-ttu-id="ccc66-101">Att arbeta med en enskild blob i Azure Storage SDK för .NET Core kräver en *blob-referens* och &mdash; en instans av ett `ICloudBlob`-objekt.</span><span class="sxs-lookup"><span data-stu-id="ccc66-101">Working with an individual blob in the Azure Storage SDK for .NET Core requires a *blob reference* &mdash; an instance of an `ICloudBlob` object.</span></span>

<span data-ttu-id="ccc66-102">Du kan få en `ICloudBlob` genom att begära den med blobens namn eller välja den från en lista över blobar i behållaren.</span><span class="sxs-lookup"><span data-stu-id="ccc66-102">You can get an `ICloudBlob` by requesting it with the blob's name or selecting it from a list of blobs in the container.</span></span> <span data-ttu-id="ccc66-103">Båda kräver en `CloudBlobContainer`, som du såg hur du skaffar i den senaste delen.</span><span class="sxs-lookup"><span data-stu-id="ccc66-103">Both require a `CloudBlobContainer`, which we saw how to get in the last unit.</span></span>

## <a name="getting-blobs-by-name"></a><span data-ttu-id="ccc66-104">Hämta blobar efter namn</span><span class="sxs-lookup"><span data-stu-id="ccc66-104">Getting blobs by name</span></span>

<span data-ttu-id="ccc66-105">Anropa en av metoderna `GetXXXReference` på en `CloudBlobContainer` för att hämta en `ICloudBlob` efter namn.</span><span class="sxs-lookup"><span data-stu-id="ccc66-105">Call one of the `GetXXXReference` methods on a `CloudBlobContainer` to get an `ICloudBlob` by name.</span></span> <span data-ttu-id="ccc66-106">Om du vet vilken typ av blob du ska hämta kanske du föredrar att använda någon av mer specifika metoderna (`GetBlockBlobReference`, `GetAppendBlobReference`, eller `GetPageBlobReference`).</span><span class="sxs-lookup"><span data-stu-id="ccc66-106">If you know the type of the blob you are retrieving, prefer using one of the more specific methods (`GetBlockBlobReference`, `GetAppendBlobReference`, or `GetPageBlobReference`).</span></span>

<span data-ttu-id="ccc66-107">Ingen av dessa metoder utför ett nätverksanrop och de bekräftar inte heller huruvida bloben finns eller ej.</span><span class="sxs-lookup"><span data-stu-id="ccc66-107">None of these methods make a network call, nor do they confirm whether or not the blob actually exists.</span></span> <span data-ttu-id="ccc66-108">En annan metod `GetBlobReferenceFromServerAsync`, anropar API:et för Blob Storage och genererar ett undantagsfel om bloben inte finns.</span><span class="sxs-lookup"><span data-stu-id="ccc66-108">A separate method, `GetBlobReferenceFromServerAsync`, does call the Blob storage API and will throw an exception if the blob doesn't already exist.</span></span>

## <a name="listing-blobs-in-a-container"></a><span data-ttu-id="ccc66-109">Visa blobar i en behållare</span><span class="sxs-lookup"><span data-stu-id="ccc66-109">Listing blobs in a container</span></span>

<span data-ttu-id="ccc66-110">Du kan hämta en lista över blobar i en behållare med hjälp av `CloudBlobContainer`s metod `ListBlobsSegmentedAsync`.</span><span class="sxs-lookup"><span data-stu-id="ccc66-110">You can get a list of the blobs in a container using `CloudBlobContainer`'s `ListBlobsSegmentedAsync` method.</span></span> <span data-ttu-id="ccc66-111">*Segmenterad* syftar till de separata sidor med resultat som returnerades och &mdash; ett enda anrop till `ListBlobsSegmentedAsync` kan aldrig garantera att alla resultat returneras på en enda sida.</span><span class="sxs-lookup"><span data-stu-id="ccc66-111">*Segmented* refers to the separate pages of results returned &mdash; a single call to `ListBlobsSegmentedAsync` is never guaranteed to return all the results in a single page.</span></span> <span data-ttu-id="ccc66-112">Vi kan behöva anropa den flera gånger med hjälp av `ContinuationToken` som den returnerar för att arbeta igenom sidorna.</span><span class="sxs-lookup"><span data-stu-id="ccc66-112">We may need to call it repeatedly using the `ContinuationToken` it returns to work our way through the pages.</span></span> <span data-ttu-id="ccc66-113">Det här gör att koden för att lista blobar blir lite mer komplicerad än koden för att ladda upp eller ned, men det finns ett standardmönster som du kan använda för att hämta alla blobar i en behållare:</span><span class="sxs-lookup"><span data-stu-id="ccc66-113">This makes the code for listing blobs a little more complex than the code for uploading or downloading, but there's a standard pattern you can use to get every blob in a container:</span></span>

```csharp
BlobContinuationToken continuationToken = null;
BlobResultSegment resultSegment = null; 

do
{
    resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

    // Do work here on resultSegment.Results

    continuationToken = resultSegment.ContinuationToken;
} while (continuationToken != null);
```

<span data-ttu-id="ccc66-114">Detta anropar `ListBlobsSegmentedAsync` upprepade gånger till `continuationToken` är `null`, vilket signalerar slutet av resultaten.</span><span class="sxs-lookup"><span data-stu-id="ccc66-114">This will call `ListBlobsSegmentedAsync` repeatedly until `continuationToken` is `null`, which signals the end of the results.</span></span>

### <a name="processing-list-results"></a><span data-ttu-id="ccc66-115">Bearbetning av listresultat</span><span class="sxs-lookup"><span data-stu-id="ccc66-115">Processing list results</span></span>

<span data-ttu-id="ccc66-116">Objektet du får tillbaka från `ListBlobsSegmentedAsync` innehåller en `Results`-egenskap av typen `IEnumerable<IListBlobItem>`.</span><span class="sxs-lookup"><span data-stu-id="ccc66-116">The object you'll get back from `ListBlobsSegmentedAsync` contains a `Results` property of type `IEnumerable<IListBlobItem>`.</span></span> <span data-ttu-id="ccc66-117">`IListBlobItem` innehåller en handfull egenskaper om blobens behållare och webbadress, men inga metoder för att ladda upp eller ned.</span><span class="sxs-lookup"><span data-stu-id="ccc66-117">`IListBlobItem`s contain a handful of properties about the blob's container and URL, but no upload or download methods.</span></span> <span data-ttu-id="ccc66-118">Det beror på att vissa av resultatobjekten kan vara `CloudBlobDirectory`-objekt som representerar virtuella kataloger i stället för enskilda blobar.</span><span class="sxs-lookup"><span data-stu-id="ccc66-118">This is because some of the result objects may be `CloudBlobDirectory` objects that represent virtual directories rather than individual blobs.</span></span>

<span data-ttu-id="ccc66-119">Om du bara är intresserad av enskilda blobar kan du använda metoden `OfType<>` för att filtrera resultaten.</span><span class="sxs-lookup"><span data-stu-id="ccc66-119">If you are only interested in individual blobs, you can use the `OfType<>` method to filter the results.</span></span> <span data-ttu-id="ccc66-120">Några exempel:</span><span class="sxs-lookup"><span data-stu-id="ccc66-120">Here are a few examples:</span></span>

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

<span data-ttu-id="ccc66-121">Om du vill använda `OfType<>` krävs en referens till namnområdet `System.Linq` (`using System.Linq;`).</span><span class="sxs-lookup"><span data-stu-id="ccc66-121">Using `OfType<>` will require a reference to the `System.Linq` namespace (`using System.Linq;`).</span></span>

## <a name="exercise"></a><span data-ttu-id="ccc66-122">Övning</span><span class="sxs-lookup"><span data-stu-id="ccc66-122">Exercise</span></span>

<span data-ttu-id="ccc66-123">En av funktionerna i vår app kräver att du hämtar en lista över blobar från API:et.</span><span class="sxs-lookup"><span data-stu-id="ccc66-123">One of the features in our app requires getting a list of blobs from the API.</span></span> <span data-ttu-id="ccc66-124">Vi använder mönstret som visas ovan för att visa alla blobar i vår behållare.</span><span class="sxs-lookup"><span data-stu-id="ccc66-124">We'll use the pattern shown above to list all the blobs in our container.</span></span> <span data-ttu-id="ccc66-125">Vi hittar namnet för varje blob när vi bearbetar listan.</span><span class="sxs-lookup"><span data-stu-id="ccc66-125">As we process the list, we get the name of each blob.</span></span>

<span data-ttu-id="ccc66-126">Öppna `BlobStorage.cs` i redigeraren och fyll i `GetNames` med följande kod:</span><span class="sxs-lookup"><span data-stu-id="ccc66-126">Open `BlobStorage.cs` in the editor and fill in `GetNames` with the following code:</span></span>

```csharp
public async Task<IEnumerable<string>> GetNames()
{
    List<string> names = new List<string>();

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);

    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    do
    {
        resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

        // Get the name of each blob.
        names.AddRange(resultSegment.Results.OfType<ICloudBlob>().Select(b => b.Name));

        continuationToken = resultSegment.ContinuationToken;
    } while (continuationToken != null);

    return names;
}
```

<span data-ttu-id="ccc66-127">De namn som returneras av den här metoden bearbetas av `FilesController` för att omvandla dem till webbadresser.</span><span class="sxs-lookup"><span data-stu-id="ccc66-127">The names returned by this method are processed by `FilesController` to turn them into URLs.</span></span> <span data-ttu-id="ccc66-128">När de returneras till klienten renderas de som hyperlänkar på sidan.</span><span class="sxs-lookup"><span data-stu-id="ccc66-128">When they are returned to the client, they are rendered as hyperlinks on the page.</span></span>