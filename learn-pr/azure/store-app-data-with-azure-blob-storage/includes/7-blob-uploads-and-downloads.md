<span data-ttu-id="399b9-101">När vi har en referens till en blob kan vi överföra och hämta data.</span><span class="sxs-lookup"><span data-stu-id="399b9-101">Once we have a reference to a blob, we can upload and download data.</span></span> <span data-ttu-id="399b9-102">`ICloudBlob`-objekt har `Upload`- och `Download`-metoder som stöder bytematriser, strömmar och filer som källor och mål.</span><span class="sxs-lookup"><span data-stu-id="399b9-102">`ICloudBlob` objects have `Upload` and `Download` methods that support byte arrays, streams, and files as sources and targets.</span></span> <span data-ttu-id="399b9-103">Vissa typer har ytterligare metoder för att underlätta &mdash; exempelvis har `CloudBlockBlob` stöd för att överföra och hämta strängar med `UploadTextAsync` och `DownloadTextAsync`.</span><span class="sxs-lookup"><span data-stu-id="399b9-103">Specific types have additional methods for convenience &mdash; for example, `CloudBlockBlob` supports uploading and downloading strings with `UploadTextAsync` and `DownloadTextAsync`.</span></span>

## <a name="creating-new-blobs"></a><span data-ttu-id="399b9-104">Skapa nya blobbar</span><span class="sxs-lookup"><span data-stu-id="399b9-104">Creating new blobs</span></span>

<span data-ttu-id="399b9-105">Om du vill skapa en ny blob anropar du en av `Upload`-metoderna för en blob som inte finns.</span><span class="sxs-lookup"><span data-stu-id="399b9-105">To create a new blob, you call one of the `Upload` methods on a blob that doesn't exist.</span></span> <span data-ttu-id="399b9-106">Två saker händer: Blobben skapas och data överförs.</span><span class="sxs-lookup"><span data-stu-id="399b9-106">This does two things: creates the blob and uploads the data.</span></span> 

## <a name="moving-data-to-and-from-blobs"></a><span data-ttu-id="399b9-107">Flytta data till och från blobbar</span><span class="sxs-lookup"><span data-stu-id="399b9-107">Moving data to and from blobs</span></span>

<span data-ttu-id="399b9-108">Att flytta data till och från en blob är en nätverksåtgärd som tar tid.</span><span class="sxs-lookup"><span data-stu-id="399b9-108">Moving data to and from a blob is a network operation that takes time.</span></span> <span data-ttu-id="399b9-109">I Azure Storage SDK för .NET Core kommer alla metoder som kräver nätverksaktivitet att returnera `Task`, så se till att dina kontrollantmetoder är `async` med relevanta värden och att du använder `await` för metodanrop i stället för `Wait`.</span><span class="sxs-lookup"><span data-stu-id="399b9-109">In the Azure Storage SDK for .NET Core, all methods that require network activity return `Task`s, so make sure your controller methods are `async` as appropriate and that you are `await`ing method calls and not `Wait`ing on them.</span></span>

<span data-ttu-id="399b9-110">En vanlig rekommendation när du arbetar med stora dataobjekt är att använda strömmar i stället för minnesinterna strukturer som t.ex. bytematriser eller strängar.</span><span class="sxs-lookup"><span data-stu-id="399b9-110">A common recommendation when working with large data objects is to use streams instead of in-memory structures like byte arrays or strings.</span></span> <span data-ttu-id="399b9-111">Detta förhindrar att hela innehållet buffras i minnet innan det skickas till målet.</span><span class="sxs-lookup"><span data-stu-id="399b9-111">This avoids buffering the full content in memory before sending it to the target.</span></span> <span data-ttu-id="399b9-112">ASP.NET Core har stöd för läsning och skrivning av strömmar från begäranden och svar.</span><span class="sxs-lookup"><span data-stu-id="399b9-112">ASP.NET Core supports reading and writing streams from requests and responses.</span></span>

## <a name="concurrent-access"></a><span data-ttu-id="399b9-113">Samtidig åtkomst</span><span class="sxs-lookup"><span data-stu-id="399b9-113">Concurrent access</span></span>

<span data-ttu-id="399b9-114">Det kan hända att andra processer kan lägga till, ändra eller ta bort blobbar samtidigt som din app använder dem.</span><span class="sxs-lookup"><span data-stu-id="399b9-114">It is possible that other processes may be adding, changing, or deleting blobs as your app is using them.</span></span> <span data-ttu-id="399b9-115">Använd alltid kod på ett defensivt sätt och förutsäg problem som kan orsakas av samtidighet, t.ex att blobbar tas bort samtidigt som du försöker ladda ned från dem eller blobbar vars innehåll ändras när du inte förväntar dig det.</span><span class="sxs-lookup"><span data-stu-id="399b9-115">Always code defensively and think about problems caused by concurrency, such as blobs that are deleted right as you try to download from them, or blobs whose contents change when you don't expect them to.</span></span> <span data-ttu-id="399b9-116">Se avsnittet Ytterligare resurser i slutet av den här modulen för information om hur du använder AccessConditions och blobblån för att hantera samtidig blobbåtkomst.</span><span class="sxs-lookup"><span data-stu-id="399b9-116">See the Additional Resources section at the end of this module for information about using AccessConditions and blob leases to manage concurrent blob access.</span></span>

## <a name="exercise"></a><span data-ttu-id="399b9-117">Övning</span><span class="sxs-lookup"><span data-stu-id="399b9-117">Exercise</span></span>

<span data-ttu-id="399b9-118">Nu ska vi slutföra vår app genom att lägga till uppladdnings- och nedladdningskod, och sedan distribuera den till Azure App Service för att testa den.</span><span class="sxs-lookup"><span data-stu-id="399b9-118">Let's finish our app by adding upload and download code, then deploy it to Azure App Service for testing.</span></span>

### <a name="upload"></a><span data-ttu-id="399b9-119">Ladda upp</span><span class="sxs-lookup"><span data-stu-id="399b9-119">Upload</span></span>

<span data-ttu-id="399b9-120">Vi laddar upp en blob genom att implementera metoden `BlobStorage.Save` med hjälp av `GetBlockBlobReference` för att få en `CloudBlockBlob` från behållaren.</span><span class="sxs-lookup"><span data-stu-id="399b9-120">To upload a blob, we'll implement the `BlobStorage.Save` method using `GetBlockBlobReference` to get a `CloudBlockBlob` from the container.</span></span> <span data-ttu-id="399b9-121">`FilesController.Upload` skickar filströmmen till `Save`, så att vi kan använda `UploadFromStreamAsync` till att genomföra överföringen för maximal effektivitet.</span><span class="sxs-lookup"><span data-stu-id="399b9-121">`FilesController.Upload` passes the file stream to `Save`, so we can use `UploadFromStreamAsync` to perform the upload for maximum efficiency.</span></span>

<span data-ttu-id="399b9-122">Öppna `BlobStorage.cs` i redigeringsprogrammet och fyll i `Save`-implementeringen med följande kod:</span><span class="sxs-lookup"><span data-stu-id="399b9-122">Open `BlobStorage.cs` in the editor and fill in the `Save` implementation with the following code:</span></span>

```csharp
public Task Save(Stream fileStream, string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(name);
    return blockBlob.UploadFromStreamAsync(fileStream);
}
```

> [!NOTE]
> <span data-ttu-id="399b9-123">Den strömbaserade uppladdningskod som visas här är effektivare än att läsa filen till en bytematris innan den skickas till Azures blobblagring.</span><span class="sxs-lookup"><span data-stu-id="399b9-123">The stream-based upload code shown here is more efficient than reading the file into a byte array before sending it to Azure Blob storage.</span></span> <span data-ttu-id="399b9-124">Men den `IFormFile`-teknik som vi använder för att hämta filen från klienten är inte en verklig strömmande implementering från slutpunkt till slutpunkt och passar bara till att hantera uppladdningar av små filer.</span><span class="sxs-lookup"><span data-stu-id="399b9-124">However, the `IFormFile` technique we use to get the file from the client is not a true end-to-end streaming implementation and is only appropriate for handling uploads of small files.</span></span> <span data-ttu-id="399b9-125">Se avsnittet Ytterligare resurser i slutet av den här modulen för mer information om helt strömmade filöverföringar.</span><span class="sxs-lookup"><span data-stu-id="399b9-125">See the Additional Resources section at the end of this module for information about fully streamed file uploads.</span></span>

### <a name="download"></a><span data-ttu-id="399b9-126">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="399b9-126">Download</span></span>

<span data-ttu-id="399b9-127">`BlobStorage.Load` returnerar en `Stream`, vilket innebär att vår kod inte behöver flytta byte fysiskt från blobblagringen alls &mdash; vi behöver bara returnera en referens till blobbströmmen.</span><span class="sxs-lookup"><span data-stu-id="399b9-127">`BlobStorage.Load` returns a `Stream`, meaning that our code doesn't need to physically move the bytes from Blob storage at all &mdash; we just need to return a reference to the blob stream.</span></span> <span data-ttu-id="399b9-128">Vi kan göra det med `OpenReadAsync`.</span><span class="sxs-lookup"><span data-stu-id="399b9-128">We can do that with `OpenReadAsync`.</span></span> <span data-ttu-id="399b9-129">ASP.NET Core hanterar läsningen och stänger dataströmmen när den skapar klientsvaret.</span><span class="sxs-lookup"><span data-stu-id="399b9-129">ASP.NET Core will handle reading and closing the stream when it builds the client response.</span></span>

<span data-ttu-id="399b9-130">Fyll i `Load` med den här koden:</span><span class="sxs-lookup"><span data-stu-id="399b9-130">Fill in `Load` with this code:</span></span>

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a><span data-ttu-id="399b9-131">Publicera och köra i Azure</span><span class="sxs-lookup"><span data-stu-id="399b9-131">Deploy and run in Azure</span></span>

<span data-ttu-id="399b9-132">Vår app är klar &mdash; nu ska vi distribuera den och se hur den fungerar.</span><span class="sxs-lookup"><span data-stu-id="399b9-132">Our app is finished &mdash; let's deploy it and see it work.</span></span> <span data-ttu-id="399b9-133">Kör följande kod i Azure Cloud Shell-terminalen för att skapa koden och distribuera den till en ny App Service-instans.</span><span class="sxs-lookup"><span data-stu-id="399b9-133">Run the following code in the Azure Cloud Shell terminal to build our code and deploy it to a new App Service instance.</span></span> <span data-ttu-id="399b9-134">Vi ska också lägga till lagringskontots anslutningssträng till konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="399b9-134">We also add our storage account connection string to configuration.</span></span>

```console

```

<span data-ttu-id="399b9-135">...</span><span class="sxs-lookup"><span data-stu-id="399b9-135">...</span></span>

<span data-ttu-id="399b9-136">Överför och hämta några filer om du vill testa appen och kör sedan följande i gränssnittet för att se vilka blobbar som har överförts:</span><span class="sxs-lookup"><span data-stu-id="399b9-136">Upload and download some files to test the app, then run the following in the shell to see the blobs that have been uploaded:</span></span>

```console

```

<span data-ttu-id="399b9-137">**TODO-bild från portalen**</span><span class="sxs-lookup"><span data-stu-id="399b9-137">**TODO pic from portal**</span></span>