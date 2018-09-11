<span data-ttu-id="9f0e5-101">Att arbeta med en enskild blob i Azure Storage SDK för .NET Core kräver en *blob-referens* och &mdash; en instans av ett `ICloudBlob`-objekt.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-101">Working with an individual blob in the Azure Storage SDK for .NET Core requires a *blob reference* &mdash; an instance of an `ICloudBlob` object.</span></span>

<span data-ttu-id="9f0e5-102">Du kan få en `ICloudBlob` genom att begära den med blobens namn eller välja den från en lista över blobar i containern.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-102">You can get an `ICloudBlob` by requesting it with the blob's name or selecting it from a list of blobs in the container.</span></span> <span data-ttu-id="9f0e5-103">Båda kräver en `CloudBlobContainer`, som du såg hur du skaffar i den senaste delen.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-103">Both require a `CloudBlobContainer`, which we saw how to get in the last unit.</span></span>

## <a name="getting-blobs-by-name"></a><span data-ttu-id="9f0e5-104">Hämta blobar efter namn</span><span class="sxs-lookup"><span data-stu-id="9f0e5-104">Getting blobs by name</span></span>

<span data-ttu-id="9f0e5-105">Anropa en av metoderna `GetXXXReference` på en `CloudBlobContainer` för att hämta en `ICloudBlob` efter namn.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-105">Call one of the `GetXXXReference` methods on a `CloudBlobContainer` to get an `ICloudBlob` by name.</span></span> <span data-ttu-id="9f0e5-106">Om du vet vilken typ av blob som du hämtar kan du använda en av de specifika metoderna (`GetBlockBlobReference`, `GetAppendBlobReference` eller `GetPageBlobReference`) för att hämta ett objekt som innehåller metoder och egenskaper som är skräddarsydda för den blobtypen.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-106">If you know the type of the blob you are retrieving, use one of the specific methods (`GetBlockBlobReference`, `GetAppendBlobReference`, or `GetPageBlobReference`) to get an object that includes methods and properties tailored for that blob type.</span></span>

<span data-ttu-id="9f0e5-107">Ingen av dessa metoder utför nätverksanrop och de bekräftar inte heller huruvida målbloben finns eller ej.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-107">None of these methods make network calls, nor do they confirm whether or not the targeted blob actually exists.</span></span> <span data-ttu-id="9f0e5-108">De skapar bara ett blobreferensobjekt lokalt, som sedan kan användas för att anropa metoder som *fungerar* via nätverket och interagerar med blobar i Storage.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-108">They only create a blob reference object locally, which can then be used to call methods that *do* operate over the network and interact with blobs in storage.</span></span> <span data-ttu-id="9f0e5-109">En annan metod, `GetBlobReferenceFromServerAsync`, anropar API:et för Blob Storage och genererar ett undantagsfel om bloben inte finns.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-109">A separate method, `GetBlobReferenceFromServerAsync`, does call the Blob storage API and will throw an exception if the blob doesn't already exist.</span></span>

## <a name="listing-blobs-in-a-container"></a><span data-ttu-id="9f0e5-110">Visa blobar i en container</span><span class="sxs-lookup"><span data-stu-id="9f0e5-110">Listing blobs in a container</span></span>

<span data-ttu-id="9f0e5-111">Du kan hämta en lista över blobar i en container med hjälp av `CloudBlobContainer`s metod `ListBlobsSegmentedAsync`.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-111">You can get a list of the blobs in a container using `CloudBlobContainer`'s `ListBlobsSegmentedAsync` method.</span></span> <span data-ttu-id="9f0e5-112">*Segmenterad* syftar till de separata sidor med resultat som returnerades och &mdash; ett enda anrop till `ListBlobsSegmentedAsync` kan aldrig garantera att alla resultat returneras på en enda sida.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-112">*Segmented* refers to the separate pages of results returned &mdash; a single call to `ListBlobsSegmentedAsync` is never guaranteed to return all the results in a single page.</span></span> <span data-ttu-id="9f0e5-113">Vi kan behöva anropa den flera gånger med hjälp av `ContinuationToken` som den returnerar för att arbeta igenom sidorna.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-113">We may need to call it repeatedly using the `ContinuationToken` it returns to work our way through the pages.</span></span> <span data-ttu-id="9f0e5-114">Det här gör att koden för att lista blobar blir lite mer komplicerad än koden för att ladda upp eller ned, men det finns ett standardmönster som du kan använda för att hämta alla blobar i en container:</span><span class="sxs-lookup"><span data-stu-id="9f0e5-114">This makes the code for listing blobs a little more complex than the code for uploading or downloading, but there's a standard pattern you can use to get every blob in a container:</span></span>

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

<span data-ttu-id="9f0e5-115">Detta anropar `ListBlobsSegmentedAsync` upprepade gånger till `continuationToken` är `null`, vilket signalerar slutet av resultaten.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-115">This will call `ListBlobsSegmentedAsync` repeatedly until `continuationToken` is `null`, which signals the end of the results.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f0e5-116">Anta aldrig resultatet av `ListBlobsSegmentedAsync` ska tas emot på en enda sida.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-116">Never assume that `ListBlobsSegmentedAsync` results will arrive in a single page.</span></span> <span data-ttu-id="9f0e5-117">Titta alltid efter en fortsättningstoken och använd den om den finns.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-117">Always check for a continuation token and use it if it's present.</span></span>

### <a name="processing-list-results"></a><span data-ttu-id="9f0e5-118">Bearbetning av listresultat</span><span class="sxs-lookup"><span data-stu-id="9f0e5-118">Processing list results</span></span>

<span data-ttu-id="9f0e5-119">Objektet du får tillbaka från `ListBlobsSegmentedAsync` innehåller en `Results`-egenskap av typen `IEnumerable<IListBlobItem>`.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-119">The object you'll get back from `ListBlobsSegmentedAsync` contains a `Results` property of type `IEnumerable<IListBlobItem>`.</span></span> <span data-ttu-id="9f0e5-120">Gränssnittet `IListBlobItem` innehåller endast ett fåtal egenskaper om blob-containern och webbadressen och är inte särskilt användbar på egen hand.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-120">The `IListBlobItem` interface includes only a handful of properties about the blob's container and URL, and isn't very useful by itself.</span></span>

<span data-ttu-id="9f0e5-121">Du kan få mer användbara blob-objekt från `Results` genom att använda metoden `OfType<>` för att filtrera och konvertera resultatet till mer specifika blob-objekttyper.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-121">To get useful blob objects out of `Results`, you can use the `OfType<>` method to filter and cast the results to more specific blob object types.</span></span> <span data-ttu-id="9f0e5-122">Några exempel:</span><span class="sxs-lookup"><span data-stu-id="9f0e5-122">Here are a few examples:</span></span>

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

> [!NOTE]
> <span data-ttu-id="9f0e5-123">Om du vill använda `OfType<>` krävs en referens till namnområdet `System.Linq` (`using System.Linq;`).</span><span class="sxs-lookup"><span data-stu-id="9f0e5-123">Using `OfType<>` requires a reference to the `System.Linq` namespace (`using System.Linq;`).</span></span>

## <a name="exercise"></a><span data-ttu-id="9f0e5-124">Övning</span><span class="sxs-lookup"><span data-stu-id="9f0e5-124">Exercise</span></span>

<span data-ttu-id="9f0e5-125">En av funktionerna i vår app kräver att du hämtar en lista över blobar från API:et.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-125">One of the features in our app requires getting a list of blobs from the API.</span></span> <span data-ttu-id="9f0e5-126">Vi använder mönstret som visas ovan för att visa alla blobar i vår container.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-126">We'll use the pattern shown above to list all the blobs in our container.</span></span> <span data-ttu-id="9f0e5-127">Vi hittar namnet för varje blob när vi bearbetar listan.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-127">As we process the list, we get the name of each blob.</span></span>

<span data-ttu-id="9f0e5-128">Öppna `BlobStorage.cs` i redigeraren, ersätt `GetNames` med följande kod och spara dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-128">Open `BlobStorage.cs` in the editor, replace `GetNames` with the following code and save your changes.</span></span>

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

> [!TIP]
> <span data-ttu-id="9f0e5-129">Observera att metodsignaturen nu måste ange `async`.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-129">Note that the method signature now needs to specify `async`.</span></span>

<span data-ttu-id="9f0e5-130">De namn som returneras av den här metoden bearbetas av `FilesController` som omvandlar dem till webbadresser.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-130">The names returned by this method are processed by `FilesController` to turn them into URLs.</span></span> <span data-ttu-id="9f0e5-131">När de returneras till klienten renderas de som hyperlänkar på sidan.</span><span class="sxs-lookup"><span data-stu-id="9f0e5-131">When they are returned to the client, they are rendered as hyperlinks on the page.</span></span>