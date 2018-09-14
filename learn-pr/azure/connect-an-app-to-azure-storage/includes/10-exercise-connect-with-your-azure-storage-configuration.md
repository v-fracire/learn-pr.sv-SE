::: zon pivot = ”csharp” ska vi lägga till kod för att hämta anslutningssträngen från konfigurationen och använda den för att ansluta till Azure storage-kontot.

## <a name="retrieve-the-connection-string"></a>Hämta anslutningssträngen

1. Välj **Program.cs** att öppna den i kodredigeraren.

1. Lägg till en `using` instruktion överst i filen för att referera till den `Microsoft.WindowsAzure.Storage` namnområde:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. I slutet av den `Main` metoden lägger du till följande rad för att hämta anslutningssträngen för Azure storage-konto från konfigurationsfilen. Den överförda _nyckel_ måste matcha namnet som används i din **appsettings.json** fil.

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>Skapa en blobbklient

1. Använder du statiskhet `CloudStorageAccount.TryParse` metod för att skapa en `CloudStorageAccount` objekt – det tar att anslutningssträngen och en `out` parametern ska returneras objektet. Returnerar en `bool` -värde som anger om det har parsats strängen.
    - Om det inte går ut ett meddelande till konsolen och returneras från metoden.

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString, 
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. Använd den returnerade `CloudStorageAccount` objektet för att skapa en blob-klient.

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Använd blobbklienten för att hämta en referens till en behållare med namnet ”photoblobs”. Mycket som kontonamn, måste Blob-behållarnamn vara gemener och består av bokstäver och siffror.

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. Använd den `CreateIfNotExistsAsync` metod för att skapa behållaren. Detta returnerar en `bool` som anger om behållaren har skapats. Store detta i en variabel med namnet `created`.
    - Observera att detta är en **async** metod - innebär det utförs ett faktiska nätverks-anrop.
    - Du måste använda den `await` nyckelord att hämta den `bool` resultatet.

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. Eftersom vi använder den `await` nyckelord, gå vidare och ändra signaturen för den `Main` -metoden är `async` och returnera en `Task`.

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

1. Slutligen matar ut om vi skapade Blob-behållaren.

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

## <a name="use-c-71-to-build-our-app"></a>Använd C# 7.1 för att bygga vår app

Stöd för `async` och `await` på `Main` metoder har lagts till i C# 7.1. Detta kanske inte standardversionen av kompilatorn som du använder. Vi behöver kontrollera att vi använder den versionen av kompilatorn genom att ange den i vår konfigurationsfilen.

1. Öppna den `PhotoSharingApp.csproj` och Lägg till `<LangVersion>7.1</LangVersion>` till den `PropertyGroup` som anger den `TargetFramework`. Det bör se ut så här när du är klar:

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

1. Skapa och kör programmet. **Obs:** se till att du befinner dig i rätt arbetskatalogen.

    ```bash
    dotnet run
    ```

::: zone-end

::: zon pivot = ”javascript” ska vi lägga till kod för att ansluta till Azure storage-konto med hjälp av vår lagrade anslutningssträngen. Azure-klientbiblioteket använder automatiskt den **AZURE_STORAGE_CONNECTION_STRING** miljövariabeln att hämta anslutningssträngen.

## <a name="create-a-blob-client"></a>Skapa en blobbklient

1. Öppna **index.js** i kodredigeraren.

1. Starta genom att inkludera den **azure-storage** modulen. Store modulen i en variabel med namnet **storage**.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();
    
    const storage = require('azure-storage');
    ```

1. Direkt efter det, Använd sedan den **storage** objektet för att skapa den `BlobService` objekt och lagra den på en global med namnet **blobService**. Kom ihåg att dessa är lätta objekt som representerar åtkomst till lagringskontot.

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. Lägg till en konstant som representerar den behållare som vi vill skapa. Vi kan namnge behållaren ”photoblobs”.

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a>Skapa en container

Vi kan använda den `BlobService` objekt att arbeta med blob API: er i Azure storage. Som tidigare nämnts, är alla API: er som gör nätverksanrop asynkrona att appen svarar. Den `createContainerIfNotExists` metoden är en sådan metod. Vi använder _lovar_ att hantera det återanrop som innehåller certifikatutfärdarens svar.

1. Använd `util.promisify` ska börja återanrop-versionen av `createContainerIfNotExists` och omvandla dem till ett löfte returnerar-metoden.
    - Eftersom motringningsmetoden är på ett objekt, se till att lägga till en `bind` anropa i slutet för att ansluta till denna kontext.
    - Tilldela det returnera värdet en konstant överst i filen som heter `createContainerAsync`, enligt nedan.

```javascript
const storage = require('azure-storage');
const blobService = storage.createBlobService();

const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';
```

1. Ta bort den ”Hello, World”! utdata från `main()`.

1. Anropa din nya `createContainerAsync` löftet.
    - Skickar den de **containerName** konstant.
    - Tillämpa den `await` nyckelord i anropet.
    - Omsluta anropet i en `try`  /  `catch` konstruera och utdata eventuella fel.

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```
    
1. Den `createContainerAsync` löftet returnerar det första värdet från den underliggande `createContainerIfNotExists`, som är resultatet för behållaren. Detta omfattar information om om behållaren har skapats eller inte. Samla in resultatet i en variabel och utdata om behållaren har skapats utifrån den `result.created` egenskapen.

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

1. Slutligen kan anpassa den `main` fungerar med den `async` nyckelord.
        
1. Spara filen.

Den slutgiltiga filen bör se ut så här, om du vill kontrollera ditt arbete.

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

1. Skapa och kör programmet. **Obs:** se till att du befinner dig i rätt arbetskatalogen.

    ```bash
    node index.js
    ```

::: zone-end

Det bör rapportera att blob-behållaren har skapats. Om du kör en gång, bör den informera dig om den redan finns.

För att verifiera behållaren:

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. Navigera till ditt lagringskonto. Du kan använda den **alla resurser** avsnitt för att hitta lagringskontot eller Sök efter namn från den _sökrutan_ längst ned i portalfönstret. 

1. Välj den **Blobar** inmatning av lagringskontot i den **Blob tjänster** avsnittet.

1. Du bör se din **photoblobs** behållare i panelen Blobar. Du kan ta bort behållaren via menyn ”...” till höger i posten för att försöka återskapa den med din app.

> [!NOTE]
> Behållaren försvinner från portalen mycket snabbt, men det tar några minuter att bort. Du får ett felsvar från Azure medan det tas bort om du försöker återskapa den.

## <a name="delete-the-app"></a>Ta bort appen
Om du inte vill behålla programmets källkod i Cloud Shell-miljön kan använda du följande kommando för att ta bort mappen och allt innehåll.

```bash
rm -r PhotoSharingApp/
```
