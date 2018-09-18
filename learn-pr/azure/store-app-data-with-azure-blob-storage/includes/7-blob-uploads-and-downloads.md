<span data-ttu-id="f5d0b-101">När vi har en referens till en blob kan vi överföra och hämta data.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-101">Once we have a reference to a blob, we can upload and download data.</span></span> <span data-ttu-id="f5d0b-102">`ICloudBlob`-objekt har `Upload`- och `Download`-metoder som stöder bytematriser, strömmar och filer som källor och mål.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-102">`ICloudBlob` objects have `Upload` and `Download` methods that support byte arrays, streams, and files as sources and targets.</span></span> <span data-ttu-id="f5d0b-103">Vissa typer har ytterligare metoder för att underlätta &mdash; exempelvis har `CloudBlockBlob` stöd för att överföra och hämta strängar med `UploadTextAsync` och `DownloadTextAsync`.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-103">Specific types have additional methods for convenience &mdash; for example, `CloudBlockBlob` supports uploading and downloading strings with `UploadTextAsync` and `DownloadTextAsync`.</span></span>

## <a name="creating-new-blobs"></a><span data-ttu-id="f5d0b-104">Skapa nya blobbar</span><span class="sxs-lookup"><span data-stu-id="f5d0b-104">Creating new blobs</span></span>

<span data-ttu-id="f5d0b-105">Om du vill skapa en ny blob anropar du en av `Upload`-metoderna för en referens till en blob som inte finns i lagring.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-105">To create a new blob, you call one of the `Upload` methods on a reference to a blob that doesn't exist in storage.</span></span> <span data-ttu-id="f5d0b-106">Två saker händer: Blobben skapas i lagring och data laddas upp.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-106">This does two things: creates the blob in storage and uploads the data.</span></span>

## <a name="moving-data-to-and-from-blobs"></a><span data-ttu-id="f5d0b-107">Flytta data till och från blobbar</span><span class="sxs-lookup"><span data-stu-id="f5d0b-107">Moving data to and from blobs</span></span>

<span data-ttu-id="f5d0b-108">Att flytta data till och från en blob är en nätverksåtgärd som tar tid.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-108">Moving data to and from a blob is a network operation that takes time.</span></span> <span data-ttu-id="f5d0b-109">I Azure Storage SDK för .NET Core kommer alla metoder som kräver nätverksaktivitet att returnera `Task`, så se till att du använder `await` i dina kontrollantmetoder på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-109">In the Azure Storage SDK for .NET Core, all methods that require network activity return `Task`s, so make sure you use `await` in your controller methods appropriately.</span></span>

<span data-ttu-id="f5d0b-110">En vanlig rekommendation när du arbetar med stora dataobjekt är att använda strömmar i stället för minnesinterna strukturer som t.ex. bytematriser eller strängar.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-110">A common recommendation when working with large data objects is to use streams instead of in-memory structures like byte arrays or strings.</span></span> <span data-ttu-id="f5d0b-111">Detta förhindrar att hela innehållet buffras i minnet innan det skickas till målet.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-111">This avoids buffering the full content in memory before sending it to the target.</span></span> <span data-ttu-id="f5d0b-112">ASP.NET Core har stöd för läsning och skrivning av strömmar från begäranden och svar.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-112">ASP.NET Core supports reading and writing streams from requests and responses.</span></span>

## <a name="concurrent-access"></a><span data-ttu-id="f5d0b-113">Samtidig åtkomst</span><span class="sxs-lookup"><span data-stu-id="f5d0b-113">Concurrent access</span></span>

<span data-ttu-id="f5d0b-114">Andra processer kan lägga till, ändra eller ta bort blobbar samtidigt som din app använder dem.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-114">Other processes may be adding, changing, or deleting blobs as your app is using them.</span></span> <span data-ttu-id="f5d0b-115">Använd alltid kod på ett defensivt sätt och förutsäg problem som kan orsakas av samtidighet, t.ex att blobbar tas bort samtidigt som du försöker ladda ned från dem eller blobbar vars innehåll ändras när du inte förväntar dig det.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-115">Always code defensively and think about problems caused by concurrency, such as blobs that are deleted right as you try to download from them, or blobs whose contents change when you don't expect them to.</span></span> <span data-ttu-id="f5d0b-116">Se avsnittet Ytterligare läsning i slutet av den här modulen för information om hur du använder AccessConditions och blobblån för att hantera samtidig blobbåtkomst.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-116">See the Further Reading section at the end of this module for information about using AccessConditions and blob leases to manage concurrent blob access.</span></span>

## <a name="exercise"></a><span data-ttu-id="f5d0b-117">Övning</span><span class="sxs-lookup"><span data-stu-id="f5d0b-117">Exercise</span></span>

<span data-ttu-id="f5d0b-118">Nu ska vi slutföra vår app genom att lägga till uppladdnings- och nedladdningskod, och sedan distribuera den till Azure App Service för att testa den.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-118">Let's finish our app by adding upload and download code, then deploy it to Azure App Service for testing.</span></span>

### <a name="upload"></a><span data-ttu-id="f5d0b-119">Ladda upp</span><span class="sxs-lookup"><span data-stu-id="f5d0b-119">Upload</span></span>

<span data-ttu-id="f5d0b-120">Vi laddar upp en blob genom att implementera metoden `BlobStorage.Save` med hjälp av `GetBlockBlobReference` för att få en `CloudBlockBlob` från behållaren.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-120">To upload a blob, we'll implement the `BlobStorage.Save` method using `GetBlockBlobReference` to get a `CloudBlockBlob` from the container.</span></span> <span data-ttu-id="f5d0b-121">`FilesController.Upload` skickar filströmmen till `Save`, så att vi kan använda `UploadFromStreamAsync` till att genomföra uppladdningen för maximal effektivitet.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-121">`FilesController.Upload` passes the file stream to `Save`, so we can use `UploadFromStreamAsync` to perform the upload for maximum efficiency.</span></span>

<span data-ttu-id="f5d0b-122">I redigeraren ersätter du `Save` i `BlobStorage.cs` med följande kod:</span><span class="sxs-lookup"><span data-stu-id="f5d0b-122">In the editor, replace `Save` in `BlobStorage.cs` with the following code:</span></span>

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
> <span data-ttu-id="f5d0b-123">Den strömbaserade uppladdningskod som visas här är effektivare än att läsa filen till en bytematris innan den skickas till Azures bloblagring.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-123">The stream-based upload code shown here is more efficient than reading the file into a byte array before sending it to Azure Blob storage.</span></span> <span data-ttu-id="f5d0b-124">Men den ASP.NET Core `IFormFile`-teknik som vi använder för att hämta filen från klienten är inte en verklig strömmande implementering från slutpunkt till slutpunkt och passar bara till att hantera uppladdningar av små filer.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-124">However, the ASP.NET Core `IFormFile` technique we use to get the file from the client is not a true end-to-end streaming implementation and is only appropriate for handling uploads of small files.</span></span> <span data-ttu-id="f5d0b-125">Se avsnittet Ytterligare läsning i slutet av den här modulen för mer information om helt strömmade filuppladdningar.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-125">See the Further Reading section at the end of this module for information about fully streamed file uploads.</span></span>

### <a name="download"></a><span data-ttu-id="f5d0b-126">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="f5d0b-126">Download</span></span>

<span data-ttu-id="f5d0b-127">`BlobStorage.Load` returnerar en `Stream`, vilket innebär att vår kod inte behöver flytta byte fysiskt från blobblagringen alls &mdash; vi behöver bara returnera en referens till blobbströmmen.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-127">`BlobStorage.Load` returns a `Stream`, meaning that our code doesn't need to physically move the bytes from Blob storage at all &mdash; we just need to return a reference to the blob stream.</span></span> <span data-ttu-id="f5d0b-128">Vi kan göra det med `OpenReadAsync`.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-128">We can do that with `OpenReadAsync`.</span></span> <span data-ttu-id="f5d0b-129">ASP.NET Core hanterar läsningen och stänger dataströmmen när den skapar klientsvaret.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-129">ASP.NET Core will handle reading and closing the stream when it builds the client response.</span></span>

<span data-ttu-id="f5d0b-130">Ersätt `Load` med den här koden och spara ditt arbete:</span><span class="sxs-lookup"><span data-stu-id="f5d0b-130">Replace `Load` with this code and save your work:</span></span>

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a><span data-ttu-id="f5d0b-131">Distribuera och köra i Azure</span><span class="sxs-lookup"><span data-stu-id="f5d0b-131">Deploy and run in Azure</span></span>

<span data-ttu-id="f5d0b-132">Vår app är klar &mdash; nu ska vi distribuera den och se hur den fungerar.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-132">Our app is finished &mdash; let's deploy it and see it work.</span></span> <span data-ttu-id="f5d0b-133">Skapa en App Service-app och konfigurera den med programinställningar för vårt lagringskontos anslutningssträng och containernamn.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-133">Create an App Service app and configure it with application settings for our storage account connection string and container name.</span></span> <span data-ttu-id="f5d0b-134">Hämta lagringskontots anslutningssträng med `az storage account show-connection-string` och ange `files` som namn på containern.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-134">Get the storage account's connection string with `az storage account show-connection-string` and set the name of the container to be `files`.</span></span>

<span data-ttu-id="f5d0b-135">Appnamnet måste vara globalt unikt, så du måste välja ett eget namn att fylla i `<your-unique-app-name>`.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-135">The app name needs to be globally unique, so you'll need to choose your own name to fill in `<your-unique-app-name>`.</span></span>

```azurecli
az appservice plan create --name blob-exercise-plan --resource-group blob-exercise-group
az webapp create --name <your-unique-app-name> --plan blob-exercise-plan --resource-group blob-exercise-group
CONNECTIONSTRING=$(az storage account show-connection-string --name <your-unique-storage-account-name> --output tsv)
az webapp config appsettings set --name <your-unique-app-name> --resource-group blob-exercise-group --settings AzureStorageConfig:ConnectionString=$CONNECTIONSTRING AzureStorageConfig:FileContainerName=files
```

<span data-ttu-id="f5d0b-136">Nu ska vi distribuera appen.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-136">Now we'll deploy our app.</span></span> <span data-ttu-id="f5d0b-137">Nedanstående kommandon publicerar webbplatsen till mappen `pub`, zippar upp den till `site.zip` och distribuerar zip-filen till App Service.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-137">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="f5d0b-138">Se till att gränssnittet fortfarande körs i katalogen `mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start` innan du kör följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-138">Make sure your shell is still in the `mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start` directory before running the following commands.</span></span>

```azurecli
dotnet publish -o pub
cd pub
zip -r ../site.zip *
az webapp deployment source config-zip --src ../site.zip --name <your-unique-app-name> --resource-group blob-exercise-group
```

<span data-ttu-id="f5d0b-139">Öppna `https://<your-unique-app-name>.azurewebsites.net` i en webbläsare om du vill se appen som körs.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-139">Open `https://<your-unique-app-name>.azurewebsites.net` in a browser to see the running app.</span></span> <span data-ttu-id="f5d0b-140">Det bör se ut som på bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-140">It should look like the image below.</span></span>

![Skärmbild av webbappen FileUploader](../media/7-fileuploader-empty.PNG)

<span data-ttu-id="f5d0b-142">Prova att ladda upp och ladda ned några filer om du vill testa appen.</span><span class="sxs-lookup"><span data-stu-id="f5d0b-142">Try uploading and downloading some files to test the app.</span></span> <span data-ttu-id="f5d0b-143">När du har laddat upp några filer kör du följande i gränssnittet för att se vilka blobbar som har laddats upp till containern:</span><span class="sxs-lookup"><span data-stu-id="f5d0b-143">After you've uploaded a few files, run the following in the shell to see the blobs that have been uploaded to the container:</span></span>

```console
az storage blob list --account-name <your-unique-storage-account-name> --container-name files --query [].{Name:name} --output table
```