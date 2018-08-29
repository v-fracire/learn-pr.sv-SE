Här följer det vanliga arbetsflödet för appar som använder Azure Blob Storage:

1. **Hämta konfiguration**: Läs in konfigurationen, till exempel anslutningssträngen med kontonyckeln vid start. Detta krävs för att autentisera API-anrop.
1. **Initiera klienten**: Använd anslutningssträngen för att initiera Azure Storage-klientbiblioteket. Det skapar de objekt som appen kommer att använda med Blob Storages API.
1. **Använd**: Utför API-anrop med klientbiblioteket för att arbeta med behållare och blobar.

## <a name="configure-your-connection-string"></a>Konfigurera anslutningssträngen

Innan du skriver någon kod behöver du anslutningssträngen för lagringskontot som du ska använda. 

Anslutningssträngen innehåller din kontonyckel. Kontonyckeln anses vara en hemlighet och bör lagras på ett säkert sätt. Vi lagrar anslutningssträngen i en programinställning för App Service. En programinställning är en säker plats för programhemligheter, men den har inte stöd för lokal utveckling och inte är en robust lösning från slutpunkt till slutpunkt på egen hand.

> [!WARNING]
> **Placera inte lagringskontonycklar i kod eller i oskyddade konfigurationsfiler.** Lagringskontonycklar gör det möjligt att få fullständig åtkomst till ditt lagringskonto. Läcker en nyckel kan det resultera i oåterkalleliga skador och dyra fakturor. Lagringsguidning och tips om hur du återställer efter att en nyckel har läckts finns i avsnittet Fler resurser i slutet av den här modulen.

## <a name="initialize-the-blob-storage-object-model"></a>Initiera objektmodellen för Blob Storage

I Azure Storage SDK för .NET Core består standardmönstret för att använda Blob Storage av följande steg:

1. Anropa `CloudStorageAccount.Parse` (eller `TryParse`) med anslutningssträngen för att hämta en `CloudStorageAccount`.
1. Anropa `CreateCloudBlobClient` på `CloudStorageAccount` för att hämta en `CloudBlobClient`.
1. Anropa `GetContainerReference` på `CloudBlobClient` för att hämta en `CloudBlobContainer`.
1. Använd metoder för att behållaren ska hämta en lista över blobar och/eller hämta referenser till enskilda blobar och ladda upp och ned data.

I kod ser steg 1&ndash;3 ut så här:

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

Den här initieringskoden utför inga anrop via nätverket. Det innebär att undantag som uppstår från felaktig information inte visas förrän senare. Till exempel kommer anropet till `GetContainerReference` lyckas oavsett om behållaren faktiskt finns i kontot eller ej.

## <a name="create-containers-at-startup"></a>Skapa behållare vid start

Det är vanligt för program att skapa nödvändiga behållare i kod, även när vi vet vad behållarnas startkostnader är. Att anropa `CreateIfNotExistsAsync` på en `CloudBlobContainer` är det bästa sättet att göra detta och vi bör använda det för att skapa varje behållare som vi vet att vi kommer att behöva innan vi använder dem.

`CreateIfNotExistsAsync` *utför* ett nätverksanrop till Azure Storage. Det är bäst att utföra anropet en gång vid start och inte varje gång vi använder en behållare.

## <a name="exercise"></a>Övning

### <a name="clone-and-explore-the-unfinished-app"></a>Klona och utforska den ofärdiga appen

Vi börjar med att klona startappen från GitHub. I Cloud Shell-terminalen kör du följande för att hämta en kopia av källkoden och öppna den i redigeringsprogrammet:

```console
git clone TODO
cd TODO
code .
```

Öppna filen `Controllers/FilesController.cs`.

Den här styrenheten implementerar ett API med tre åtgärder:

* **Index** (GET-/api/Files) returnerar en lista med webbadresser, en för varje fil som har laddats upp. Appens front end anropar den här metoden för att skapa en lista med hyperlänkar till de uppladdade filerna.
* **Ladda upp** (POST /api/Files) tar emot en uppladdad fil och sparar den.
* **Ladda ned** (GET /api/Files/{file-name}) laddar ned en enskild fil efter dess namn.

Varje metod använder en `IStorage`-instans som heter `storage` för att utföra sin funktion. Det finns en ofullständig implementering av `IStorage` i `Models/BlobStorage.cs`.

### <a name="add-the-nuget-package"></a>Lägg till NuGet-paketet

Lägg först till en referens till Azure Storage SDK. Kör följande i terminalen:

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

Det ser till att vi använder den senaste versionen av Blob Storage-klientbiblioteket.

### <a name="configure"></a>Konfigurera

Vår startapp innehåller redan grunderna för konfigurationerna vi behöver. Konstruktorparametern `IOptions<AzureStorageConfig>` i `BlobStorage` har två egenskaper: anslutningssträngen för lagringskontot och namnet på behållaren som vår app använder för att lagra blobar. Det finns kod i metoden `ConfigureServices` för `Startup.cs` som läser in värdena från konfigurationen när appen startar.

I den här övningen kommer vi att köra appen i Azure App Service, så vi lägger till konfigurationsvärdena till App Service-programinställningarna senare. För tillfället behöver vi inte gör något arbete som rör konfigurationen.

### <a name="initialize"></a>Initiera

Öppna `BlobStorage.cs`.

Platsen för metoden `Initialize`. Vår app anropar den här metoden när den används för första gången. Om du är nyfiken kan du titta på `ConfigureServices` i `Startup.cs` att se hur detta utförs. 

`Initialize` är platsen där vi vill skapa vår behållare om den inte redan finns. Fyll i `Initialize` med följande kod och spara ditt arbete:

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```