::: zone pivot="csharp" Vi ska nu lägga till kod för att hämta anslutningssträngen för Azure Storage-kontot från konfigurationen och använda den för att ansluta till Azure Storage-kontot.

## <a name="retrieve-the-connection-string"></a>Hämta anslutningssträngen

1. Välj **Program.cs** för att öppna den i kodredigeraren.

1. Lägg till en `using`-instruktion överst i filen för att referera till `Microsoft.WindowsAzure.Storage`-namnområdet:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. I slutet av `Main`-metoden lägger du till följande rad för att hämta anslutningssträngen för Azure Storage-kontot från konfigurationsfilen. Den överförda _nyckeln_ måste matcha namnet som används i din **appsettings.json**-fil.

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>Skapa en blobbklient

1. Använd den statiska `CloudStorageAccount.TryParse`-metoden till att skapa ett `CloudStorageAccount`-objekt. Den använder anslutningssträngen och en `out`-parameter för att returnera objektet. Returnerar ett `bool`-värde som visar om strängen har parsats.
    - Om det misslyckas skickas ett meddelande till konsolen och returneras från metoden.

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString, 
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. Använd det returnerade `CloudStorageAccount`-objektet för att skapa en blobbklient.

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Använd sedan blobbklienten för att hämta en referens till en container med namnet ”photoblobs”. Precis som med kontonamn måste blobbcontainerns namn vara gemener och bestå av bokstäver och siffror.

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. Använd `CreateIfNotExistsAsync`-metoden till att skapa containern. Detta returnerar en `bool` som visar om containern har skapats. Lagra detta i en variabel med namnet `created`.
    - Observera att detta är en **async**-metod, vilket innebär att det utförs ett faktiskt nätverksanrop.
    - Du måste använda `await`-nyckelorden för att kunna hämta `bool`-resultatet.

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. Eftersom vi använder `await`-nyckelordet, går vi vidare och ändrar signaturen för `Main`-metoden till `async` och returnerar en `Task`.

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

1. Slutligen ser vi om vi har skapat blobbcontainern.

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. Spara filen.

Den slutgiltiga filen bör se ut så här om du vill kontrollera ditt arbete.

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;

namespace PhotoSharingApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
            var connectionString = configuration["StorageAccountConnectionString"];

            if (!CloudStorageAccount.TryParse(connectionString, out CloudStorageAccount storageAccount))
            {
                Console.WriteLine("Unable to parse connection string");
                return;
            }

            var blobClient = storageAccount.CreateCloudBlobClient();
            var blobContainer = blobClient.GetContainerReference("photoblobs");
            bool created = await blobContainer.CreateIfNotExistsAsync();

            Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
        }
    }
}
```

## <a name="use-c-71-to-build-our-app"></a>Använda C# 7.1 till att skapa vår app

Stöd för `async` och `await` i `Main`-metoderna har lagts till i C# 7.1. Detta kanske inte är standardversionen av kompilatorn som du använder. Låt oss kontrollera att vi använder den versionen av kompilatorn genom att konfigurera den i vår konfigurationsfil.

1. Öppna `PhotoSharingApp.csproj` och lägg till `<LangVersion>7.1</LangVersion>` i `PropertyGroup` som anger `TargetFramework`. Det bör se ut så här när du är klar:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <LangVersion>7.1</LangVersion> 
        <TargetFramework>netcoreapp2.0</TargetFramework>
      </PropertyGroup>
      ...
    </Project>
    ```

1. Spara filen.

## <a name="run-the-app"></a>Kör appen

1. Skapa och kör programmet. **Obs!** Se till att du befinner dig i rätt arbetskatalog.

    ```bash
    dotnet run
    ```

::: zone-end

::: zone-pivot="javascript" Nu ska vi lägga till kod för att ansluta till Azure-lagringskontot med hjälp av vår lagrade anslutningssträng. Azure-klientbiblioteket använder automatiskt miljövariabeln **AZURE_STORAGE_CONNECTION_STRING** till att hämta anslutningssträngen.

## <a name="create-a-blob-client"></a>Skapa en blobbklient

1. Öppna **index.js** i kodredigeraren.

1. Starta genom att inkludera modulen **azure-storage**. Lagra modulen i en variabel med namnet **storage**.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();
    
    const storage = require('azure-storage');
    ```

1. Direkt efteråt använder du **storage**-objektet till att skapa `BlobService`-objektet och lagra det globalt med namnet **blobService**. Kom ihåg att detta är förenklade objekt som motsvarar åtkomst till lagringskontot.

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. Lägg till en konstant som motsvarar den container som vi vill skapa. Vi ger containern namnet ”photoblobs”.

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a>Skapa en container

Vi kan använda `BlobService`-objektet för att arbeta med blobb-API: er i Azure Storage. Som tidigare nämnts är alla API:er som utför nätverksanrop asynkrona för att appen ska vara responsiv. `createContainerIfNotExists`-metoden är en sådan metod. Vi använder _promises_ till att hantera återanrop som innehåller svaret.

1. Använd `util.promisify` för återanropsversionen av `createContainerIfNotExists` och omvandla den till en metod som returnerar ett ”promise”.
    - Eftersom återanropsmetoden finns i ett objekt måste du lägga till ett `bind`-anrop i slutet för att ansluta det till denna kontext.
    - Tilldela det returnerade värdet till en konstant överst i filen som heter `createContainerAsync` enligt nedan.

```javascript
const storage = require('azure-storage');
const blobService = storage.createBlobService();

const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';
```

1. Ta bort ”Hello World!” utdata från `main()`.

1. Anropa ditt nya `createContainerAsync`-promise.
    - Skicka det till konstanten **containerName**.
    - Tillämpa sedan `await`-nyckelordet på anropet.
    - Omslut anropet i en `try` / `catch`-konstruktion och visa eventuella fel.

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```
    
1. `createContainerAsync`-promise returnerar det första värdet från den underliggande `createContainerIfNotExists`, vilket är resultatet för containern. Detta inkluderar information om containern har skapats eller inte. Samla in resultatet i en variabel och visa om containern har skapats baserat på `result.created`-egenskapen.

    ```javascript
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. Slutligen kan du anpassa `main`-funktionen med `async`-nyckelordet.
        
1. Spara filen.

Den slutgiltiga filen bör se ut så här om du vill kontrollera ditt arbete.

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function run() {
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

run();
```

## <a name="run-the-app"></a>Kör appen

1. Skapa och kör programmet. **Obs!** Se till att du befinner dig i rätt arbetskatalog.

    ```bash
    node index.js
    ```

::: zone-end

Den bör rapportera att blobbcontainern har skapats. Om du kör den en gång till, bör den informera dig om att den redan finns.

Verifiera containern:

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. Gå till ditt lagringskonto. Du kan använda avsnittet **Alla resurser** för att hitta lagringskontot, eller söka efter namnet i _sökrutan_ överst i portalfönstret. 

1. Välj posten **Blobbar** i lagringskontot i avsnittet **Blobbtjänster**.

1. Du bör se din container **photoblobs** i blobbpanelen. Du kan ta bort containern via menyn ”...” på höger sida av posten och försöka återskapa den med din app.

> [!NOTE]
> Containern försvinner från portalen mycket snabbt, men det tar några minuter innan den egentligen är borta. Du får ett felsvar från Azure om du försöker återskapa den under tiden som den tas bort.

## <a name="delete-the-app"></a>Ta bort appen
Om du inte vill behålla programmets källkod i Cloud Shell-miljön, kan du använda följande kommando för att ta bort mappen och allt innehåll.

```bash
rm -r PhotoSharingApp/
```
