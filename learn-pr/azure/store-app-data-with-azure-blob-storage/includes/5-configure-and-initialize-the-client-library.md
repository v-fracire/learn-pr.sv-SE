Här följer det vanliga arbetsflödet för appar som använder Azure Blob Storage:

1. **Hämta konfigurationen**: Läs in konfigurationen för lagringskontot vid start. Detta är vanligtvis en anslutningssträng för lagringskontot.

1. **Initiera klienten**: Använd anslutningssträngen för att initiera Azure Storage-klientbiblioteket. Det skapar de objekt som appen kommer att använda med Blob Storages API.

1. **Använd**: Utför API-anrop med klientbiblioteket för att arbeta med containrar och blobar.

## <a name="configure-your-connection-string"></a>Konfigurera anslutningssträngen

Innan du skriver någon kod behöver du anslutningssträngen för det lagringskonto som du ska använda. Du kan använda valfritt Azure-hanteringsgränssnitt för att hämta den, inklusive Microsoft Azure-portalen, Azure CLI eller Azure PowerShell. När vi konfigurera webbappen för att köra vår kod mot slutet av den här modulen kommer vi att använda Azure CLI för att hämta anslutningssträngen för lagringskontot som du skapade tidigare.

Lagringskontons anslutningssträngar innehåller kontonyckeln. Kontonyckeln anses vara en hemlighet och bör lagras på ett säkert sätt. Här lagrar vi anslutningssträngen i en programinställning för App Service. Programinställningar i App Service är en säker plats för programhemligheter, men den här modellen har inte stöd för lokal utveckling och är ingen robust lösning från slutpunkt till slutpunkt på egen hand.

> [!WARNING]
> **Placera inte lagringskontonycklar i kod eller i oskyddade konfigurationsfiler.** Lagringskontonycklar gör det möjligt att få fullständig åtkomst till ditt lagringskonto. Läcker en nyckel kan det resultera i oåterkalleliga skador och dyra fakturor. Lagringsguidning och tips om hur du återställer efter att en nyckel har läckts finns i avsnittet Ytterligare läsning i slutet av den här modulen.

## <a name="initialize-the-blob-storage-object-model"></a>Initiera objektmodellen för Blob Storage

I Azure Storage SDK för .NET Core består standardmönstret för att använda Blob Storage av följande steg:

1. Anropa `CloudStorageAccount.Parse` (eller `TryParse`) med anslutningssträngen för att hämta en `CloudStorageAccount`.

1. Anropa `CreateCloudBlobClient` på `CloudStorageAccount` för att hämta en `CloudBlobClient`.

1. Anropa `GetContainerReference` på `CloudBlobClient` för att hämta en `CloudBlobContainer`.

1. Använd metoder för att containern ska hämta en lista över blobar och/eller hämta referenser till enskilda blobar och ladda upp och ned data.

I kod ser steg 1&ndash;3 ut så här:

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

Den här initieringskoden utför inga anrop via nätverket. Det innebär att vissa undantag som uppstår på grund av felaktig information inte uppstår förrän senare. Till exempel genererar anropet till `CloudStorageAccount.Parse` ett undantagsfel omedelbart om anslutningssträngen är felaktigt formaterad, men inget undantag genereras om lagringskontot som en anslutningssträng pekar på inte finns.

## <a name="create-containers-at-startup"></a>Skapa containrar vid start

Att anropa `CreateIfNotExistsAsync` på en `CloudBlobContainer` är det bästa sättet att skapa en container när programmet startas eller när det för första gången försöker använda den.

`CreateIfNotExistsAsync` utlöser inget undantag om containern redan finns, men ett nätverksanrop görs till Azure Storage. Anropa det en gång under initieringen, inte varje gång du försöker använda en container.

## <a name="exercise"></a>Övning

### <a name="clone-and-explore-the-unfinished-app"></a>Klona och utforska den ofärdiga appen

Vi börjar med att klona startappen från GitHub. I Cloud Shell-terminalen kör du följande för att hämta en kopia av källkoden och öppna den i redigeringsprogrammet:

```console
git clone https://github.com/MicrosoftDocs/mslearn-store-data-in-azure.git
cd mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start
code .
```

Öppna filen `Controllers/FilesController.cs` i en textredigerare. Du har inget arbete att göra här, men vi ska ändå ta en snabb titt på vad appen gör.

Den här styrenheten implementerar ett API med tre åtgärder:

- **Index** (GET-/api/Files) returnerar en lista med webbadresser, en för varje fil som har laddats upp. Appens front end anropar den här metoden för att skapa en lista med hyperlänkar till de uppladdade filerna.
- **Upload** (POST /api/Files) tar emot en uppladdad fil och sparar den.
- **Download** (GET /api/Files/{filnamn}) laddar ned en enskild fil efter dess namn.

Varje metod använder en `IStorage`-instans som heter `storage` för att utföra sin funktion. Det finns en ofullständig implementering av `IStorage` i `Models/BlobStorage.cs` som vi ska fylla i.

### <a name="add-the-nuget-package"></a>Lägga till NuGet-paketet

Lägg först till en referens till Azure Storage SDK. Kör följande i terminalen:

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

Det ser till att vi använder den senaste versionen av Blob Storage-klientbiblioteket.

### <a name="configure"></a>Konfigurera

De konfigurationsvärden vi behöver är anslutningssträngen för lagringskontot och namnet på den container som appen använder för att lagra filer. I den här övningen ska vi bara köra appen i Azure App Service, och därför följer vi bästa praxis för App Service och lagrar värdena i programinställningarna för App Service. Det sker när vi skapar App Service-instansen, så det är inget vi behöver göra just nu.

När det handlar om att *använda* konfigurationen innehåller vår startapp redan de grunder vi behöver. Konstruktorparametern `IOptions<AzureStorageConfig>` i `BlobStorage` har två egenskaper: anslutningssträngen för lagringskontot och namnet på containern som vår app använder för att lagra blobar. Det finns kod i metoden `ConfigureServices` för `Startup.cs` som läser in värdena från konfigurationen när appen startar.

### <a name="initialize"></a>Initiera

Öppna `Models/BlobStorage.cs` i redigeraren. Lägg till följande `using`-uttryck överst i filen för att förbereda den för den kod som du kommer att lägga till under den här övningen.

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Leta upp metoden `Initialize`. Vår app anropar den här metoden när `BlobStorage` används för första gången. Om du är nyfiken kan du titta på `ConfigureServices` i `Startup.cs` och se hur detta utförs.

`Initialize` är platsen där vi vill skapa vår container om den inte redan finns. Ersätt den aktuella implementationen av `Initialize` med följande kod och spara ditt arbete:

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```