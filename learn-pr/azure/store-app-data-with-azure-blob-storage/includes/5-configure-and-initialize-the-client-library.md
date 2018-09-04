<span data-ttu-id="35d2e-101">Här följer det vanliga arbetsflödet för appar som använder Azure Blob Storage:</span><span class="sxs-lookup"><span data-stu-id="35d2e-101">The following is the typical workflow for apps that use Azure Blob storage:</span></span>

1. <span data-ttu-id="35d2e-102">**Hämta konfigurationen**: Läs in konfigurationen för lagringskontot vid start.</span><span class="sxs-lookup"><span data-stu-id="35d2e-102">**Retrieve configuration**: At startup, load the storage account configuration.</span></span> <span data-ttu-id="35d2e-103">Detta är vanligtvis en anslutningssträng för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="35d2e-103">This is typically a storage account connection string.</span></span>

1. <span data-ttu-id="35d2e-104">**Initiera klienten**: Använd anslutningssträngen för att initiera Azure Storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="35d2e-104">**Initialize client**: Use the connection string to initialize the Azure Storage client library.</span></span> <span data-ttu-id="35d2e-105">Det skapar de objekt som appen kommer att använda med Blob Storages API.</span><span class="sxs-lookup"><span data-stu-id="35d2e-105">This creates the objects the app will use to work with the Blob storage API.</span></span>

1. <span data-ttu-id="35d2e-106">**Använd**: Utför API-anrop med klientbiblioteket för att arbeta med containrar och blobar.</span><span class="sxs-lookup"><span data-stu-id="35d2e-106">**Use**: Make API calls with the client library to operate on containers and blobs.</span></span>

## <a name="configure-your-connection-string"></a><span data-ttu-id="35d2e-107">Konfigurera anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="35d2e-107">Configure your connection string</span></span>

<span data-ttu-id="35d2e-108">Innan du skriver någon kod behöver du anslutningssträngen för lagringskontot som du ska använda.</span><span class="sxs-lookup"><span data-stu-id="35d2e-108">Before writing any code, you'll need the connection string for the storage account you will use.</span></span>

<span data-ttu-id="35d2e-109">Lagringskontons anslutningssträngar innehåller kontonyckeln.</span><span class="sxs-lookup"><span data-stu-id="35d2e-109">Storage account connection strings include the account key.</span></span> <span data-ttu-id="35d2e-110">Kontonyckeln anses vara en hemlighet och bör lagras på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="35d2e-110">The account key is considered a secret and should be stored securely.</span></span> <span data-ttu-id="35d2e-111">Här lagrar vi anslutningssträngen i en programinställning för App Service.</span><span class="sxs-lookup"><span data-stu-id="35d2e-111">Here, we will store the connection string in an App Service application setting.</span></span> <span data-ttu-id="35d2e-112">Programinställningar i App Service är en säker plats för programhemligheter, men den här modellen har inte stöd för lokal utveckling och är ingen robust lösning från slutpunkt till slutpunkt på egen hand.</span><span class="sxs-lookup"><span data-stu-id="35d2e-112">App Service application settings are a secure place for application secrets, but this design does not support local development and is not a robust, end-to-end solution on its own.</span></span>

> [!WARNING]
> <span data-ttu-id="35d2e-113">**Placera inte lagringskontonycklar i kod eller i oskyddade konfigurationsfiler.**</span><span class="sxs-lookup"><span data-stu-id="35d2e-113">**Do not place storage account keys in code or in unprotected configuration files.**</span></span> <span data-ttu-id="35d2e-114">Lagringskontonycklar gör det möjligt att få fullständig åtkomst till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="35d2e-114">Storage account keys enable full access to your storage account.</span></span> <span data-ttu-id="35d2e-115">Läcker en nyckel kan det resultera i oåterkalleliga skador och dyra fakturor.</span><span class="sxs-lookup"><span data-stu-id="35d2e-115">Leaking a key can result in unrecoverable damage and large bills.</span></span> <span data-ttu-id="35d2e-116">Lagringsguidning och tips om hur du återställer efter att en nyckel har läckts finns i avsnittet Ytterligare läsning i slutet av den här modulen.</span><span class="sxs-lookup"><span data-stu-id="35d2e-116">See the Further Reading section at the end of this module for storage guidance and advice about how to recover from a leaked key.</span></span>

## <a name="initialize-the-blob-storage-object-model"></a><span data-ttu-id="35d2e-117">Initiera objektmodellen för Blob Storage</span><span class="sxs-lookup"><span data-stu-id="35d2e-117">Initialize the Blob storage object model</span></span>

<span data-ttu-id="35d2e-118">I Azure Storage SDK för .NET Core består standardmönstret för att använda Blob Storage av följande steg:</span><span class="sxs-lookup"><span data-stu-id="35d2e-118">In the Azure Storage SDK for .NET Core, the standard pattern for using Blob storage consists of the following steps:</span></span>

1. <span data-ttu-id="35d2e-119">Anropa `CloudStorageAccount.Parse` (eller `TryParse`) med anslutningssträngen för att hämta en `CloudStorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="35d2e-119">Call `CloudStorageAccount.Parse` (or `TryParse`) with your connection string to get a `CloudStorageAccount`.</span></span>

1. <span data-ttu-id="35d2e-120">Anropa `CreateCloudBlobClient` på `CloudStorageAccount` för att hämta en `CloudBlobClient`.</span><span class="sxs-lookup"><span data-stu-id="35d2e-120">Call `CreateCloudBlobClient` on the `CloudStorageAccount` to get a `CloudBlobClient`.</span></span>

1. <span data-ttu-id="35d2e-121">Anropa `GetContainerReference` på `CloudBlobClient` för att hämta en `CloudBlobContainer`.</span><span class="sxs-lookup"><span data-stu-id="35d2e-121">Call `GetContainerReference` on the `CloudBlobClient` to get a `CloudBlobContainer`.</span></span>

1. <span data-ttu-id="35d2e-122">Använd metoder för att containern ska hämta en lista över blobar och/eller hämta referenser till enskilda blobar och ladda upp och ned data.</span><span class="sxs-lookup"><span data-stu-id="35d2e-122">Use methods on the container to get a list of blobs and/or get references to individual blobs to upload and download data.</span></span>

<span data-ttu-id="35d2e-123">I kod ser steg 1&ndash;3 ut så här:</span><span class="sxs-lookup"><span data-stu-id="35d2e-123">In code, steps 1&ndash;3 look like this:</span></span>

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

<span data-ttu-id="35d2e-124">Den här initieringskoden utför inga anrop via nätverket.</span><span class="sxs-lookup"><span data-stu-id="35d2e-124">None of this initialization code makes calls over the network.</span></span> <span data-ttu-id="35d2e-125">Det innebär att vissa undantag som uppstår på grund av felaktig information inte uppstår förrän senare.</span><span class="sxs-lookup"><span data-stu-id="35d2e-125">This means that some exceptions that occur because of incorrect information won't be thrown until later.</span></span> <span data-ttu-id="35d2e-126">Till exempel genererar anropet till `CloudStorageAccount.Parse` ett undantagsfel omedelbart om anslutningssträngen är felaktigt formaterad, men inget undantag genereras om lagringskontot som en anslutningssträng pekar på inte finns.</span><span class="sxs-lookup"><span data-stu-id="35d2e-126">For example, the call to `CloudStorageAccount.Parse` will throw an exception immediately if the connection string is formatted incorrectly, but no exception will be thrown if the storage account that a connection string points to doesn't exist.</span></span>

## <a name="create-containers-at-startup"></a><span data-ttu-id="35d2e-127">Skapa containrar vid start</span><span class="sxs-lookup"><span data-stu-id="35d2e-127">Create containers at startup</span></span>

<span data-ttu-id="35d2e-128">Att anropa `CreateIfNotExistsAsync` på en `CloudBlobContainer` är det bästa sättet att skapa en container när programmet startas eller när det för första gången försöker använda den.</span><span class="sxs-lookup"><span data-stu-id="35d2e-128">Calling `CreateIfNotExistsAsync` on a `CloudBlobContainer` is the best way to create a container when your application starts or when it first tries to use it.</span></span>

<span data-ttu-id="35d2e-129">`CreateIfNotExistsAsync` utlöser inget undantag om containern redan finns, men ett nätverksanrop görs till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="35d2e-129">`CreateIfNotExistsAsync` won't throw an exception if the container already exists, but it does make a network call to Azure Storage.</span></span> <span data-ttu-id="35d2e-130">Anropa det en gång under initieringen, inte varje gång du försöker använda en container.</span><span class="sxs-lookup"><span data-stu-id="35d2e-130">Call it once during initialization, not every time you try to use a container.</span></span>

## <a name="exercise"></a><span data-ttu-id="35d2e-131">Övning</span><span class="sxs-lookup"><span data-stu-id="35d2e-131">Exercise</span></span>

### <a name="clone-and-explore-the-unfinished-app"></a><span data-ttu-id="35d2e-132">Klona och utforska den ofärdiga appen</span><span class="sxs-lookup"><span data-stu-id="35d2e-132">Clone and explore the unfinished app</span></span>

<span data-ttu-id="35d2e-133">Vi börjar med att klona startappen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="35d2e-133">First, let's clone the starter app from GitHub.</span></span> <span data-ttu-id="35d2e-134">I Cloud Shell-terminalen kör du följande för att hämta en kopia av källkoden och öppna den i redigeringsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="35d2e-134">In the Cloud Shell terminal, run the following command to get a copy of the source code and open it in the editor:</span></span>

<span data-ttu-id="35d2e-135">**TODO-uppdatering av slutgiltig lagringsplats-URL**</span><span class="sxs-lookup"><span data-stu-id="35d2e-135">**TODO update to final repo URL**</span></span>

```console
git clone https://github.com/nickwalkmsft/FileUploader.git
cd FileUploader
code .
```

<span data-ttu-id="35d2e-136">Öppna filen `Controllers/FilesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="35d2e-136">Open the file `Controllers/FilesController.cs`.</span></span> <span data-ttu-id="35d2e-137">Inget arbete ska göras här, men vi ska ta en snabb titt på vad appen gör.</span><span class="sxs-lookup"><span data-stu-id="35d2e-137">There's no work to do here, but we're going to have a quick look at what the app does.</span></span>

<span data-ttu-id="35d2e-138">Den här styrenheten implementerar ett API med tre åtgärder:</span><span class="sxs-lookup"><span data-stu-id="35d2e-138">This controller implements an API with three actions:</span></span>

- <span data-ttu-id="35d2e-139">**Index** (GET-/api/Files) returnerar en lista med webbadresser, en för varje fil som har laddats upp.</span><span class="sxs-lookup"><span data-stu-id="35d2e-139">**Index** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded.</span></span> <span data-ttu-id="35d2e-140">Appens front end anropar den här metoden för att skapa en lista med hyperlänkar till de uppladdade filerna.</span><span class="sxs-lookup"><span data-stu-id="35d2e-140">The app front end calls this method to build a list of hyperlinks to the uploaded files.</span></span>
- <span data-ttu-id="35d2e-141">**Upload** (POST /api/Files) tar emot en uppladdad fil och sparar den.</span><span class="sxs-lookup"><span data-stu-id="35d2e-141">**Upload** (POST /api/Files) receives an uploaded file and saves it.</span></span>
- <span data-ttu-id="35d2e-142">**Download** (GET /api/Files/{filnamn}) laddar ned en enskild fil efter dess namn.</span><span class="sxs-lookup"><span data-stu-id="35d2e-142">**Download** (GET /api/Files/{filename}) downloads an individual file by its name.</span></span>

<span data-ttu-id="35d2e-143">Varje metod använder en `IStorage`-instans som heter `storage` för att utföra sin funktion.</span><span class="sxs-lookup"><span data-stu-id="35d2e-143">Each method uses an `IStorage` instance called `storage` to do its work.</span></span> <span data-ttu-id="35d2e-144">Det finns en ofullständig implementering av `IStorage` i `Models/BlobStorage.cs` som vi ska fylla i.</span><span class="sxs-lookup"><span data-stu-id="35d2e-144">There is an incomplete implementation of `IStorage` in `Models/BlobStorage.cs` that we're going to fill in.</span></span>

### <a name="add-the-nuget-package"></a><span data-ttu-id="35d2e-145">Lägga till NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="35d2e-145">Add the NuGet package</span></span>

<span data-ttu-id="35d2e-146">Lägg först till en referens till Azure Storage SDK.</span><span class="sxs-lookup"><span data-stu-id="35d2e-146">First, add a reference to the Azure Storage SDK.</span></span> <span data-ttu-id="35d2e-147">Kör följande i terminalen:</span><span class="sxs-lookup"><span data-stu-id="35d2e-147">In the terminal, run the following:</span></span>

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

<span data-ttu-id="35d2e-148">Det ser till att vi använder den senaste versionen av Blob Storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="35d2e-148">This will make sure we're using the newest version of the Blob storage client library.</span></span>

### <a name="configure"></a><span data-ttu-id="35d2e-149">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="35d2e-149">Configure</span></span>

<span data-ttu-id="35d2e-150">De konfigurationsvärden vi behöver för att köra appen är anslutningssträngen för lagringskonto och namnet på den container appen använder för att lagra filer.</span><span class="sxs-lookup"><span data-stu-id="35d2e-150">The configuration values we need to run the app are the storage account connection string and the name of the container the app will use to store files.</span></span> <span data-ttu-id="35d2e-151">I den här övningen ska vi bara köra appen i Azure App Service, så vi följer bästa praxis för App Service och lagrar värdena i programinställningarna för App Service.</span><span class="sxs-lookup"><span data-stu-id="35d2e-151">In this exercise, we're only going to run the app in Azure App Service, so we'll follow App Service best practice and store the values in App Service application settings.</span></span> <span data-ttu-id="35d2e-152">Vi gör det när vi skapar App Service-instansen, så det är inget vi behöver göra just nu.</span><span class="sxs-lookup"><span data-stu-id="35d2e-152">We'll do that when we create the App Service instance, so there's nothing we need to do at the moment.</span></span>

<span data-ttu-id="35d2e-153">När det handlar om att *använda* konfigurationen innehåller vår startapp redan de grunder vi behöver.</span><span class="sxs-lookup"><span data-stu-id="35d2e-153">When it comes to *using* the configuration, our starter app already includes the plumbing we need.</span></span> <span data-ttu-id="35d2e-154">Konstruktorparametern `IOptions<AzureStorageConfig>` i `BlobStorage` har två egenskaper: anslutningssträngen för lagringskontot och namnet på containern som vår app använder för att lagra blobar.</span><span class="sxs-lookup"><span data-stu-id="35d2e-154">The `IOptions<AzureStorageConfig>` constructor parameter in `BlobStorage` has two properties: the storage account connection string and the name of the container our app will store blobs in.</span></span> <span data-ttu-id="35d2e-155">Det finns kod i metoden `ConfigureServices` för `Startup.cs` som läser in värdena från konfigurationen när appen startar.</span><span class="sxs-lookup"><span data-stu-id="35d2e-155">There is code in the `ConfigureServices` method of `Startup.cs` that loads the values from configuration when the app starts.</span></span>

### <a name="initialize"></a><span data-ttu-id="35d2e-156">Initiera</span><span class="sxs-lookup"><span data-stu-id="35d2e-156">Initialize</span></span>

<span data-ttu-id="35d2e-157">Öppna `Models/BlobStorage.cs`.</span><span class="sxs-lookup"><span data-stu-id="35d2e-157">Open `Models/BlobStorage.cs`.</span></span> <span data-ttu-id="35d2e-158">Lägg till följande `using`-uttryck överst i filen för att förbereda den för den kod som du ska lägga till under den här övningen.</span><span class="sxs-lookup"><span data-stu-id="35d2e-158">Add the following `using` statements to the top of the file to prepare it for the code you're going to add during the exercise.</span></span>

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="35d2e-159">Leta upp metoden `Initialize`.</span><span class="sxs-lookup"><span data-stu-id="35d2e-159">Locate the `Initialize` method.</span></span> <span data-ttu-id="35d2e-160">Vår app anropar den här metoden när `BlobStorage` används för första gången.</span><span class="sxs-lookup"><span data-stu-id="35d2e-160">Our app will call this method when `BlobStorage` is used for the first time.</span></span> <span data-ttu-id="35d2e-161">Om du är nyfiken kan du titta på `ConfigureServices` i `Startup.cs` och se hur detta utförs.</span><span class="sxs-lookup"><span data-stu-id="35d2e-161">If you're curious, you can look at `ConfigureServices` in `Startup.cs` to see how this is done.</span></span>

<span data-ttu-id="35d2e-162">`Initialize` är platsen där vi vill skapa vår container om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="35d2e-162">`Initialize` is where we want to create our container if it doesn't already exist.</span></span> <span data-ttu-id="35d2e-163">Ersätt den aktuella implementationen av `Initialize` med följande kod och spara ditt arbete:</span><span class="sxs-lookup"><span data-stu-id="35d2e-163">Replace the current implementation of `Initialize` with the following code and save your work:</span></span>

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```