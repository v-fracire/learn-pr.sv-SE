<span data-ttu-id="267bc-101">Här följer det vanliga arbetsflödet för appar som använder Azure Blob Storage:</span><span class="sxs-lookup"><span data-stu-id="267bc-101">The following is the typical workflow for apps that use Azure Blob storage:</span></span>

1. <span data-ttu-id="267bc-102">**Hämta konfiguration**: Läs in konfigurationen, till exempel anslutningssträngen med kontonyckeln vid start.</span><span class="sxs-lookup"><span data-stu-id="267bc-102">**Retrieve configuration**: At startup, load the configuration, such as the connection string with the account key.</span></span> <span data-ttu-id="267bc-103">Detta krävs för att autentisera API-anrop.</span><span class="sxs-lookup"><span data-stu-id="267bc-103">This is needed to authenticate API calls.</span></span>
1. <span data-ttu-id="267bc-104">**Initiera klienten**: Använd anslutningssträngen för att initiera Azure Storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="267bc-104">**Initialize client**: Use the connection string to initialize the Azure Storage client library.</span></span> <span data-ttu-id="267bc-105">Det skapar de objekt som appen kommer att använda med Blob Storages API.</span><span class="sxs-lookup"><span data-stu-id="267bc-105">This creates the objects the app will use to work with the Blob storage API.</span></span>
1. <span data-ttu-id="267bc-106">**Använd**: Utför API-anrop med klientbiblioteket för att arbeta med behållare och blobar.</span><span class="sxs-lookup"><span data-stu-id="267bc-106">**Use**: Make API calls with the client library to operate on containers and blobs.</span></span>

## <a name="configure-your-connection-string"></a><span data-ttu-id="267bc-107">Konfigurera anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="267bc-107">Configure your connection string</span></span>

<span data-ttu-id="267bc-108">Innan du skriver någon kod behöver du anslutningssträngen för lagringskontot som du ska använda.</span><span class="sxs-lookup"><span data-stu-id="267bc-108">Before writing any code, you'll need the connection string for the storage account you will use.</span></span> 

<span data-ttu-id="267bc-109">Anslutningssträngen innehåller din kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="267bc-109">The connection string includes your account key.</span></span> <span data-ttu-id="267bc-110">Kontonyckeln anses vara en hemlighet och bör lagras på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="267bc-110">The account key is considered a secret and should be stored securely.</span></span> <span data-ttu-id="267bc-111">Vi lagrar anslutningssträngen i en programinställning för App Service.</span><span class="sxs-lookup"><span data-stu-id="267bc-111">We will store the connection string in an App Service application setting.</span></span> <span data-ttu-id="267bc-112">En programinställning är en säker plats för programhemligheter, men den har inte stöd för lokal utveckling och inte är en robust lösning från slutpunkt till slutpunkt på egen hand.</span><span class="sxs-lookup"><span data-stu-id="267bc-112">An application setting is a secure place for application secrets, but it does not support local development and is not a robust, end-to-end solution on its own.</span></span>

> [!WARNING]
> <span data-ttu-id="267bc-113">**Placera inte lagringskontonycklar i kod eller i oskyddade konfigurationsfiler.**</span><span class="sxs-lookup"><span data-stu-id="267bc-113">**Do not place storage account keys in code or in unprotected configuration files.**</span></span> <span data-ttu-id="267bc-114">Lagringskontonycklar gör det möjligt att få fullständig åtkomst till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="267bc-114">Storage account keys enable full access to your storage account.</span></span> <span data-ttu-id="267bc-115">Läcker en nyckel kan det resultera i oåterkalleliga skador och dyra fakturor.</span><span class="sxs-lookup"><span data-stu-id="267bc-115">Leaking a key can result in unrecoverable damage and large bills.</span></span> <span data-ttu-id="267bc-116">Lagringsguidning och tips om hur du återställer efter att en nyckel har läckts finns i avsnittet Fler resurser i slutet av den här modulen.</span><span class="sxs-lookup"><span data-stu-id="267bc-116">See the Additional Resources section at the end of this module for storage guidance and advice about how to recover from a leaked key.</span></span>

## <a name="initialize-the-blob-storage-object-model"></a><span data-ttu-id="267bc-117">Initiera objektmodellen för Blob Storage</span><span class="sxs-lookup"><span data-stu-id="267bc-117">Initialize the Blob storage object model</span></span>

<span data-ttu-id="267bc-118">I Azure Storage SDK för .NET Core består standardmönstret för att använda Blob Storage av följande steg:</span><span class="sxs-lookup"><span data-stu-id="267bc-118">In the Azure Storage SDK for .NET Core, the standard pattern for using Blob storage consists of the following steps:</span></span>

1. <span data-ttu-id="267bc-119">Anropa `CloudStorageAccount.Parse` (eller `TryParse`) med anslutningssträngen för att hämta en `CloudStorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="267bc-119">Call `CloudStorageAccount.Parse` (or `TryParse`) with your connection string to get a `CloudStorageAccount`.</span></span>
1. <span data-ttu-id="267bc-120">Anropa `CreateCloudBlobClient` på `CloudStorageAccount` för att hämta en `CloudBlobClient`.</span><span class="sxs-lookup"><span data-stu-id="267bc-120">Call `CreateCloudBlobClient` on the `CloudStorageAccount` to get a `CloudBlobClient`.</span></span>
1. <span data-ttu-id="267bc-121">Anropa `GetContainerReference` på `CloudBlobClient` för att hämta en `CloudBlobContainer`.</span><span class="sxs-lookup"><span data-stu-id="267bc-121">Call `GetContainerReference` on the `CloudBlobClient` to get a `CloudBlobContainer`.</span></span>
1. <span data-ttu-id="267bc-122">Använd metoder för att behållaren ska hämta en lista över blobar och/eller hämta referenser till enskilda blobar och ladda upp och ned data.</span><span class="sxs-lookup"><span data-stu-id="267bc-122">Use methods on the container to get a list of blobs and/or get references to individual blobs to upload and download data.</span></span>

<span data-ttu-id="267bc-123">I kod ser steg 1&ndash;3 ut så här:</span><span class="sxs-lookup"><span data-stu-id="267bc-123">In code, steps 1&ndash;3 look like this:</span></span>

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

<span data-ttu-id="267bc-124">Den här initieringskoden utför inga anrop via nätverket.</span><span class="sxs-lookup"><span data-stu-id="267bc-124">None of this initialization code makes calls over the network.</span></span> <span data-ttu-id="267bc-125">Det innebär att undantag som uppstår från felaktig information inte visas förrän senare. Till exempel kommer anropet till `GetContainerReference` lyckas oavsett om behållaren faktiskt finns i kontot eller ej.</span><span class="sxs-lookup"><span data-stu-id="267bc-125">This means exceptions that occur from incorrect information won't be thrown until later; for example, the call to `GetContainerReference` will succeed whether or not the container actually exists in the account.</span></span>

## <a name="create-containers-at-startup"></a><span data-ttu-id="267bc-126">Skapa behållare vid start</span><span class="sxs-lookup"><span data-stu-id="267bc-126">Create containers at startup</span></span>

<span data-ttu-id="267bc-127">Det är vanligt för program att skapa nödvändiga behållare i kod, även när vi vet vad behållarnas startkostnader är.</span><span class="sxs-lookup"><span data-stu-id="267bc-127">Common practice is for applications to create needed containers in code, even when we know what those containers will be up-front.</span></span> <span data-ttu-id="267bc-128">Att anropa `CreateIfNotExistsAsync` på en `CloudBlobContainer` är det bästa sättet att göra detta och vi bör använda det för att skapa varje behållare som vi vet att vi kommer att behöva innan vi använder dem.</span><span class="sxs-lookup"><span data-stu-id="267bc-128">Calling `CreateIfNotExistsAsync` on a `CloudBlobContainer` is the best way to do this, and we should use it to create each container we know we'll need before we use them.</span></span>

<span data-ttu-id="267bc-129">`CreateIfNotExistsAsync` *utför* ett nätverksanrop till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="267bc-129">`CreateIfNotExistsAsync` *does* make a network call to Azure Storage.</span></span> <span data-ttu-id="267bc-130">Det är bäst att utföra anropet en gång vid start och inte varje gång vi använder en behållare.</span><span class="sxs-lookup"><span data-stu-id="267bc-130">Best practice is to call it once at startup and not every time we access a container.</span></span>

## <a name="exercise"></a><span data-ttu-id="267bc-131">Övning</span><span class="sxs-lookup"><span data-stu-id="267bc-131">Exercise</span></span>

### <a name="clone-and-explore-the-unfinished-app"></a><span data-ttu-id="267bc-132">Klona och utforska den ofärdiga appen</span><span class="sxs-lookup"><span data-stu-id="267bc-132">Clone and explore the unfinished app</span></span>

<span data-ttu-id="267bc-133">Vi börjar med att klona startappen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="267bc-133">First, let's clone the starter app from GitHub.</span></span> <span data-ttu-id="267bc-134">I Cloud Shell-terminalen kör du följande för att hämta en kopia av källkoden och öppna den i redigeringsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="267bc-134">In the Cloud Shell terminal, run the following command to get a copy of the source code and open it in the editor:</span></span>

```console
git clone TODO
cd TODO
code .
```

<span data-ttu-id="267bc-135">Öppna filen `Controllers/FilesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="267bc-135">Open the file `Controllers/FilesController.cs`.</span></span>

<span data-ttu-id="267bc-136">Den här styrenheten implementerar ett API med tre åtgärder:</span><span class="sxs-lookup"><span data-stu-id="267bc-136">This controller implements an API with three actions:</span></span>

* <span data-ttu-id="267bc-137">**Index** (GET-/api/Files) returnerar en lista med webbadresser, en för varje fil som har laddats upp.</span><span class="sxs-lookup"><span data-stu-id="267bc-137">**Index** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded.</span></span> <span data-ttu-id="267bc-138">Appens front end anropar den här metoden för att skapa en lista med hyperlänkar till de uppladdade filerna.</span><span class="sxs-lookup"><span data-stu-id="267bc-138">The app front end calls this method to build a list of hyperlinks to the uploaded files.</span></span>
* <span data-ttu-id="267bc-139">**Ladda upp** (POST /api/Files) tar emot en uppladdad fil och sparar den.</span><span class="sxs-lookup"><span data-stu-id="267bc-139">**Upload** (POST /api/Files) receives an uploaded file and saves it.</span></span>
* <span data-ttu-id="267bc-140">**Ladda ned** (GET /api/Files/{file-name}) laddar ned en enskild fil efter dess namn.</span><span class="sxs-lookup"><span data-stu-id="267bc-140">**Download** (GET /api/Files/{file-name}) downloads an individual file by its name.</span></span>

<span data-ttu-id="267bc-141">Varje metod använder en `IStorage`-instans som heter `storage` för att utföra sin funktion.</span><span class="sxs-lookup"><span data-stu-id="267bc-141">Each method uses an `IStorage` instance called `storage` to do its work.</span></span> <span data-ttu-id="267bc-142">Det finns en ofullständig implementering av `IStorage` i `Models/BlobStorage.cs`.</span><span class="sxs-lookup"><span data-stu-id="267bc-142">There is an incomplete implementation of `IStorage` in  `Models/BlobStorage.cs`.</span></span>

### <a name="add-the-nuget-package"></a><span data-ttu-id="267bc-143">Lägg till NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="267bc-143">Add the NuGet package</span></span>

<span data-ttu-id="267bc-144">Lägg först till en referens till Azure Storage SDK.</span><span class="sxs-lookup"><span data-stu-id="267bc-144">First, add a reference to the Azure Storage SDK.</span></span> <span data-ttu-id="267bc-145">Kör följande i terminalen:</span><span class="sxs-lookup"><span data-stu-id="267bc-145">In the terminal, run the following:</span></span>

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

<span data-ttu-id="267bc-146">Det ser till att vi använder den senaste versionen av Blob Storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="267bc-146">This will make sure we're using the newest version of the Blob storage client library.</span></span>

### <a name="configure"></a><span data-ttu-id="267bc-147">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="267bc-147">Configure</span></span>

<span data-ttu-id="267bc-148">Vår startapp innehåller redan grunderna för konfigurationerna vi behöver.</span><span class="sxs-lookup"><span data-stu-id="267bc-148">Our starter app already includes the configuration plumbing we need.</span></span> <span data-ttu-id="267bc-149">Konstruktorparametern `IOptions<AzureStorageConfig>` i `BlobStorage` har två egenskaper: anslutningssträngen för lagringskontot och namnet på behållaren som vår app använder för att lagra blobar.</span><span class="sxs-lookup"><span data-stu-id="267bc-149">The `IOptions<AzureStorageConfig>` constructor parameter in `BlobStorage` has two properties: the storage account connection string and the name of the container our app will store blobs in.</span></span> <span data-ttu-id="267bc-150">Det finns kod i metoden `ConfigureServices` för `Startup.cs` som läser in värdena från konfigurationen när appen startar.</span><span class="sxs-lookup"><span data-stu-id="267bc-150">There is code in the `ConfigureServices` method of `Startup.cs` that loads the values from configuration when the app starts.</span></span>

<span data-ttu-id="267bc-151">I den här övningen kommer vi att köra appen i Azure App Service, så vi lägger till konfigurationsvärdena till App Service-programinställningarna senare.</span><span class="sxs-lookup"><span data-stu-id="267bc-151">In this exercise, we will run the app in Azure App Service, so we will add the configuration values to the App Service application settings later.</span></span> <span data-ttu-id="267bc-152">För tillfället behöver vi inte gör något arbete som rör konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="267bc-152">For now, we don't need to do any work related to configuration.</span></span>

### <a name="initialize"></a><span data-ttu-id="267bc-153">Initiera</span><span class="sxs-lookup"><span data-stu-id="267bc-153">Initialize</span></span>

<span data-ttu-id="267bc-154">Öppna `BlobStorage.cs`.</span><span class="sxs-lookup"><span data-stu-id="267bc-154">Open `BlobStorage.cs`.</span></span>

<span data-ttu-id="267bc-155">Platsen för metoden `Initialize`.</span><span class="sxs-lookup"><span data-stu-id="267bc-155">Location the `Initialize` method.</span></span> <span data-ttu-id="267bc-156">Vår app anropar den här metoden när den används för första gången.</span><span class="sxs-lookup"><span data-stu-id="267bc-156">Our app will call this method when it's first used.</span></span> <span data-ttu-id="267bc-157">Om du är nyfiken kan du titta på `ConfigureServices` i `Startup.cs` att se hur detta utförs.</span><span class="sxs-lookup"><span data-stu-id="267bc-157">If you're curious, you can look at `ConfigureServices` in `Startup.cs` to see how this is done.</span></span> 

<span data-ttu-id="267bc-158">`Initialize` är platsen där vi vill skapa vår behållare om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="267bc-158">`Initialize` is where we want to create our container if it doesn't already exist.</span></span> <span data-ttu-id="267bc-159">Fyll i `Initialize` med följande kod och spara ditt arbete:</span><span class="sxs-lookup"><span data-stu-id="267bc-159">Fill in `Initialize` with the following code and save your work:</span></span>

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```