<span data-ttu-id="dccab-101">Här följer det vanliga arbetsflödet för appar som använder Azure Blob Storage:</span><span class="sxs-lookup"><span data-stu-id="dccab-101">The following is the typical workflow for apps that use Azure Blob storage:</span></span>

1. <span data-ttu-id="dccab-102">**Hämta konfigurationen**: Läs in konfigurationen för lagringskontot vid start.</span><span class="sxs-lookup"><span data-stu-id="dccab-102">**Retrieve configuration**: At startup, load the storage account configuration.</span></span> <span data-ttu-id="dccab-103">Detta är vanligtvis en anslutningssträng för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="dccab-103">This is typically a storage account connection string.</span></span>

1. <span data-ttu-id="dccab-104">**Initiera klienten**: Använd anslutningssträngen för att initiera Azure Storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="dccab-104">**Initialize client**: Use the connection string to initialize the Azure Storage client library.</span></span> <span data-ttu-id="dccab-105">Det skapar de objekt som appen kommer att använda med Blob Storages API.</span><span class="sxs-lookup"><span data-stu-id="dccab-105">This creates the objects the app will use to work with the Blob storage API.</span></span>

1. <span data-ttu-id="dccab-106">**Använd**: Utför API-anrop med klientbiblioteket för att arbeta med containrar och blobar.</span><span class="sxs-lookup"><span data-stu-id="dccab-106">**Use**: Make API calls with the client library to operate on containers and blobs.</span></span>

## <a name="configure-your-connection-string"></a><span data-ttu-id="dccab-107">Konfigurera anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="dccab-107">Configure your connection string</span></span>

<span data-ttu-id="dccab-108">Innan du skriver någon kod behöver du anslutningssträngen för det lagringskonto som du ska använda.</span><span class="sxs-lookup"><span data-stu-id="dccab-108">Before running your app, you'll need the connection string for the storage account you will use.</span></span> <span data-ttu-id="dccab-109">Du kan använda valfritt Azure-hanteringsgränssnitt för att hämta den, inklusive Microsoft Azure-portalen, Azure CLI eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dccab-109">You can use any Azure management interface to get it, including the Azure portal, the Azure CLI or Azure PowerShell.</span></span> <span data-ttu-id="dccab-110">När vi konfigurera webbappen för att köra vår kod mot slutet av den här modulen kommer vi att använda Azure CLI för att hämta anslutningssträngen för lagringskontot som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="dccab-110">When we set up the web app to run our code near the end of this module, we'll use the Azure CLI to get the connection string for the storage account you created earlier.</span></span>

<span data-ttu-id="dccab-111">Lagringskontons anslutningssträngar innehåller kontonyckeln.</span><span class="sxs-lookup"><span data-stu-id="dccab-111">Storage account connection strings include the account key.</span></span> <span data-ttu-id="dccab-112">Kontonyckeln anses vara en hemlighet och bör lagras på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="dccab-112">The account key is considered a secret and should be stored securely.</span></span> <span data-ttu-id="dccab-113">Här lagrar vi anslutningssträngen i en programinställning för App Service.</span><span class="sxs-lookup"><span data-stu-id="dccab-113">Here, we will store the connection string in an App Service application setting.</span></span> <span data-ttu-id="dccab-114">Programinställningar i App Service är en säker plats för programhemligheter, men den här modellen har inte stöd för lokal utveckling och är ingen robust lösning från slutpunkt till slutpunkt på egen hand.</span><span class="sxs-lookup"><span data-stu-id="dccab-114">App Service application settings are a secure place for application secrets, but this design does not support local development and is not a robust, end-to-end solution on its own.</span></span>

> [!WARNING]
> <span data-ttu-id="dccab-115">**Placera inte lagringskontonycklar i kod eller i oskyddade konfigurationsfiler.**</span><span class="sxs-lookup"><span data-stu-id="dccab-115">**Do not place storage account keys in code or in unprotected configuration files.**</span></span> <span data-ttu-id="dccab-116">Lagringskontonycklar gör det möjligt att få fullständig åtkomst till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="dccab-116">Storage account keys enable full access to your storage account.</span></span> <span data-ttu-id="dccab-117">Läcker en nyckel kan det resultera i oåterkalleliga skador och dyra fakturor.</span><span class="sxs-lookup"><span data-stu-id="dccab-117">Leaking a key can result in unrecoverable damage and large bills.</span></span> <span data-ttu-id="dccab-118">Lagringsguidning och tips om hur du återställer efter att en nyckel har läckts finns i avsnittet Ytterligare läsning i slutet av den här modulen.</span><span class="sxs-lookup"><span data-stu-id="dccab-118">See the Further Reading section at the end of this module for storage guidance and advice about how to recover from a leaked key.</span></span>

## <a name="initialize-the-blob-storage-object-model"></a><span data-ttu-id="dccab-119">Initiera objektmodellen för Blob Storage</span><span class="sxs-lookup"><span data-stu-id="dccab-119">Initialize the Blob storage object model</span></span>

<span data-ttu-id="dccab-120">I Azure Storage SDK för .NET Core består standardmönstret för att använda Blob Storage av följande steg:</span><span class="sxs-lookup"><span data-stu-id="dccab-120">In the Azure Storage SDK for .NET Core, the standard pattern for using Blob storage consists of the following steps:</span></span>

1. <span data-ttu-id="dccab-121">Anropa `CloudStorageAccount.Parse` (eller `TryParse`) med anslutningssträngen för att hämta en `CloudStorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="dccab-121">Call `CloudStorageAccount.Parse` (or `TryParse`) with your connection string to get a `CloudStorageAccount`.</span></span>

1. <span data-ttu-id="dccab-122">Anropa `CreateCloudBlobClient` på `CloudStorageAccount` för att hämta en `CloudBlobClient`.</span><span class="sxs-lookup"><span data-stu-id="dccab-122">Call `CreateCloudBlobClient` on the `CloudStorageAccount` to get a `CloudBlobClient`.</span></span>

1. <span data-ttu-id="dccab-123">Anropa `GetContainerReference` på `CloudBlobClient` för att hämta en `CloudBlobContainer`.</span><span class="sxs-lookup"><span data-stu-id="dccab-123">Call `GetContainerReference` on the `CloudBlobClient` to get a `CloudBlobContainer`.</span></span>

1. <span data-ttu-id="dccab-124">Använd metoder för att containern ska hämta en lista över blobar och/eller hämta referenser till enskilda blobar och ladda upp och ned data.</span><span class="sxs-lookup"><span data-stu-id="dccab-124">Use methods on the container to get a list of blobs and/or get references to individual blobs to upload and download data.</span></span>

<span data-ttu-id="dccab-125">I kod ser steg 1&ndash;3 ut så här:</span><span class="sxs-lookup"><span data-stu-id="dccab-125">In code, steps 1&ndash;3 look like this:</span></span>

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

<span data-ttu-id="dccab-126">Den här initieringskoden utför inga anrop via nätverket.</span><span class="sxs-lookup"><span data-stu-id="dccab-126">None of this initialization code makes calls over the network.</span></span> <span data-ttu-id="dccab-127">Det innebär att vissa undantag som uppstår på grund av felaktig information inte uppstår förrän senare.</span><span class="sxs-lookup"><span data-stu-id="dccab-127">This means that some exceptions that occur because of incorrect information won't be thrown until later.</span></span> <span data-ttu-id="dccab-128">Till exempel genererar anropet till `CloudStorageAccount.Parse` ett undantagsfel omedelbart om anslutningssträngen är felaktigt formaterad, men inget undantag genereras om lagringskontot som en anslutningssträng pekar på inte finns.</span><span class="sxs-lookup"><span data-stu-id="dccab-128">For example, the call to `CloudStorageAccount.Parse` will throw an exception immediately if the connection string is formatted incorrectly, but no exception will be thrown if the storage account that a connection string points to doesn't exist.</span></span>

## <a name="create-containers-at-startup"></a><span data-ttu-id="dccab-129">Skapa containrar vid start</span><span class="sxs-lookup"><span data-stu-id="dccab-129">Create containers at startup</span></span>

<span data-ttu-id="dccab-130">Att anropa `CreateIfNotExistsAsync` på en `CloudBlobContainer` är det bästa sättet att skapa en container när programmet startas eller när det för första gången försöker använda den.</span><span class="sxs-lookup"><span data-stu-id="dccab-130">Calling `CreateIfNotExistsAsync` on a `CloudBlobContainer` is the best way to create a container when your application starts or when it first tries to use it.</span></span>

<span data-ttu-id="dccab-131">`CreateIfNotExistsAsync` utlöser inget undantag om containern redan finns, men ett nätverksanrop görs till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dccab-131">`CreateIfNotExistsAsync` won't throw an exception if the container already exists, but it does make a network call to Azure Storage.</span></span> <span data-ttu-id="dccab-132">Anropa det en gång under initieringen, inte varje gång du försöker använda en container.</span><span class="sxs-lookup"><span data-stu-id="dccab-132">Call it once during initialization, not every time you try to use a container.</span></span>

## <a name="exercise"></a><span data-ttu-id="dccab-133">Övning</span><span class="sxs-lookup"><span data-stu-id="dccab-133">Exercise</span></span>

### <a name="clone-and-explore-the-unfinished-app"></a><span data-ttu-id="dccab-134">Klona och utforska den ofärdiga appen</span><span class="sxs-lookup"><span data-stu-id="dccab-134">Clone and explore the unfinished app</span></span>

<span data-ttu-id="dccab-135">Vi börjar med att klona startappen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="dccab-135">First, let's clone the starter app from GitHub.</span></span> <span data-ttu-id="dccab-136">I Cloud Shell-terminalen kör du följande för att hämta en kopia av källkoden och öppna den i redigeringsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="dccab-136">In the Cloud Shell terminal, run the following command to get a copy of the source code and open it in the editor:</span></span>

```console
git clone https://github.com/MicrosoftDocs/mslearn-store-data-in-azure.git
cd mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start
code .
```

<span data-ttu-id="dccab-137">Öppna filen `Controllers/FilesController.cs` i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="dccab-137">Open the file `Controllers/FilesController.cs` in the editor.</span></span> <span data-ttu-id="dccab-138">Du har inget arbete att göra här, men vi ska ändå ta en snabb titt på vad appen gör.</span><span class="sxs-lookup"><span data-stu-id="dccab-138">There's no work to do here, but we're going to have a quick look at what the app does.</span></span>

<span data-ttu-id="dccab-139">Den här styrenheten implementerar ett API med tre åtgärder:</span><span class="sxs-lookup"><span data-stu-id="dccab-139">This controller implements an API with three actions:</span></span>

- <span data-ttu-id="dccab-140">**Index** (GET-/api/Files) returnerar en lista med webbadresser, en för varje fil som har laddats upp.</span><span class="sxs-lookup"><span data-stu-id="dccab-140">**Index** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded.</span></span> <span data-ttu-id="dccab-141">Appens front end anropar den här metoden för att skapa en lista med hyperlänkar till de uppladdade filerna.</span><span class="sxs-lookup"><span data-stu-id="dccab-141">The app front end calls this method to build a list of hyperlinks to the uploaded files.</span></span>
- <span data-ttu-id="dccab-142">**Upload** (POST /api/Files) tar emot en uppladdad fil och sparar den.</span><span class="sxs-lookup"><span data-stu-id="dccab-142">**Upload** (POST /api/Files) receives an uploaded file and saves it.</span></span>
- <span data-ttu-id="dccab-143">**Download** (GET /api/Files/{filnamn}) laddar ned en enskild fil efter dess namn.</span><span class="sxs-lookup"><span data-stu-id="dccab-143">**Download** (GET /api/Files/{filename}) downloads an individual file by its name.</span></span>

<span data-ttu-id="dccab-144">Varje metod använder en `IStorage`-instans som heter `storage` för att utföra sin funktion.</span><span class="sxs-lookup"><span data-stu-id="dccab-144">Each method uses an `IStorage` instance called `storage` to do its work.</span></span> <span data-ttu-id="dccab-145">Det finns en ofullständig implementering av `IStorage` i `Models/BlobStorage.cs` som vi ska fylla i.</span><span class="sxs-lookup"><span data-stu-id="dccab-145">There is an incomplete implementation of `IStorage` in `Models/BlobStorage.cs` that we're going to fill in.</span></span>

### <a name="add-the-nuget-package"></a><span data-ttu-id="dccab-146">Lägga till NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="dccab-146">Add the NuGet package</span></span>

<span data-ttu-id="dccab-147">Lägg först till en referens till Azure Storage SDK.</span><span class="sxs-lookup"><span data-stu-id="dccab-147">First, add a reference to the Azure Storage SDK.</span></span> <span data-ttu-id="dccab-148">Kör följande i terminalen:</span><span class="sxs-lookup"><span data-stu-id="dccab-148">In the terminal, run the following:</span></span>

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

<span data-ttu-id="dccab-149">Det ser till att vi använder den senaste versionen av Blob Storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="dccab-149">This will make sure we're using the newest version of the Blob storage client library.</span></span>

### <a name="configure"></a><span data-ttu-id="dccab-150">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="dccab-150">Configure</span></span>

<span data-ttu-id="dccab-151">De konfigurationsvärden vi behöver är anslutningssträngen för lagringskontot och namnet på den container som appen använder för att lagra filer.</span><span class="sxs-lookup"><span data-stu-id="dccab-151">The configuration values we need are the storage account connection string and the name of the container the app will use to store files.</span></span> <span data-ttu-id="dccab-152">I den här övningen ska vi bara köra appen i Azure App Service, och därför följer vi bästa praxis för App Service och lagrar värdena i programinställningarna för App Service.</span><span class="sxs-lookup"><span data-stu-id="dccab-152">In this module, we're only going to run the app in Azure App Service, so we'll follow App Service best practice and store the values in App Service application settings.</span></span> <span data-ttu-id="dccab-153">Det sker när vi skapar App Service-instansen, så det är inget vi behöver göra just nu.</span><span class="sxs-lookup"><span data-stu-id="dccab-153">We'll do that when we create the App Service instance, so there's nothing we need to do at the moment.</span></span>

<span data-ttu-id="dccab-154">När det handlar om att *använda* konfigurationen innehåller vår startapp redan de grunder vi behöver.</span><span class="sxs-lookup"><span data-stu-id="dccab-154">When it comes to *using* the configuration, our starter app already includes the plumbing we need.</span></span> <span data-ttu-id="dccab-155">Konstruktorparametern `IOptions<AzureStorageConfig>` i `BlobStorage` har två egenskaper: anslutningssträngen för lagringskontot och namnet på containern som vår app använder för att lagra blobar.</span><span class="sxs-lookup"><span data-stu-id="dccab-155">The `IOptions<AzureStorageConfig>` constructor parameter in `BlobStorage` has two properties: the storage account connection string and the name of the container our app will store blobs in.</span></span> <span data-ttu-id="dccab-156">Det finns kod i metoden `ConfigureServices` för `Startup.cs` som läser in värdena från konfigurationen när appen startar.</span><span class="sxs-lookup"><span data-stu-id="dccab-156">There is code in the `ConfigureServices` method of `Startup.cs` that loads the values from configuration when the app starts.</span></span>

### <a name="initialize"></a><span data-ttu-id="dccab-157">Initiera</span><span class="sxs-lookup"><span data-stu-id="dccab-157">Initialize</span></span>

<span data-ttu-id="dccab-158">Öppna `Models/BlobStorage.cs` i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="dccab-158">Open `Models/BlobStorage.cs` in the editor.</span></span> <span data-ttu-id="dccab-159">Lägg till följande `using`-uttryck överst i filen för att förbereda den för den kod som du kommer att lägga till under den här övningen.</span><span class="sxs-lookup"><span data-stu-id="dccab-159">Add the following `using` statements to the top of the file to prepare it for the code you're going to add during the exercise.</span></span>

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="dccab-160">Leta upp metoden `Initialize`.</span><span class="sxs-lookup"><span data-stu-id="dccab-160">Locate the `Initialize` method.</span></span> <span data-ttu-id="dccab-161">Vår app anropar den här metoden när `BlobStorage` används för första gången.</span><span class="sxs-lookup"><span data-stu-id="dccab-161">Our app will call this method when `BlobStorage` is used for the first time.</span></span> <span data-ttu-id="dccab-162">Om du är nyfiken kan du titta på `ConfigureServices` i `Startup.cs` och se hur detta utförs.</span><span class="sxs-lookup"><span data-stu-id="dccab-162">If you're curious, you can look at `ConfigureServices` in `Startup.cs` to see how this is done.</span></span>

<span data-ttu-id="dccab-163">`Initialize` är platsen där vi vill skapa vår container om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="dccab-163">`Initialize` is where we want to create our container if it doesn't already exist.</span></span> <span data-ttu-id="dccab-164">Ersätt den aktuella implementationen av `Initialize` med följande kod och spara ditt arbete:</span><span class="sxs-lookup"><span data-stu-id="dccab-164">Replace the current implementation of `Initialize` with the following code and save your work:</span></span>

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```