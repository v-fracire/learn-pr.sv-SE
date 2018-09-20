Att arbeta med en enskild blob i Azure Storage SDK för .NET Core kräver en *blob-referens* och &mdash; en instans av ett `ICloudBlob`-objekt.

Du kan få en `ICloudBlob` genom att begära den med blobens namn eller välja den från en lista över blobar i containern. Båda kräver en `CloudBlobContainer`, som du såg hur du skaffar i den senaste delen.

## <a name="getting-blobs-by-name"></a>Hämta blobar efter namn

Anropa en av metoderna `GetXXXReference` på en `CloudBlobContainer` för att hämta en `ICloudBlob` efter namn. Om du vet vilken typ av blob som du hämtar kan du använda en av de specifika metoderna (`GetBlockBlobReference`, `GetAppendBlobReference` eller `GetPageBlobReference`) för att hämta ett objekt som innehåller metoder och egenskaper som är skräddarsydda för den blobtypen.

Ingen av dessa metoder utför nätverksanrop och de bekräftar inte heller huruvida målbloben finns eller ej. De skapar bara ett blobreferensobjekt lokalt, som sedan kan användas för att anropa metoder som *fungerar* via nätverket och interagerar med blobar i Storage. En annan metod, `GetBlobReferenceFromServerAsync`, anropar API:et för Blob Storage och genererar ett undantagsfel om bloben inte finns.

## <a name="listing-blobs-in-a-container"></a>Visa blobar i en container

Du kan hämta en lista över blobar i en container med hjälp av `CloudBlobContainer`s metod `ListBlobsSegmentedAsync`. *Segmenterad* syftar till de separata sidor med resultat som returnerades och &mdash; ett enda anrop till `ListBlobsSegmentedAsync` kan aldrig garantera att alla resultat returneras på en enda sida. Vi kan behöva anropa den flera gånger med hjälp av `ContinuationToken` som den returnerar för att arbeta igenom sidorna. Det här gör att koden för att lista blobar blir lite mer komplicerad än koden för att ladda upp eller ned, men det finns ett standardmönster som du kan använda för att hämta alla blobar i en container:

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

Detta anropar `ListBlobsSegmentedAsync` upprepade gånger till `continuationToken` är `null`, vilket signalerar slutet av resultaten.

> [!IMPORTANT]
> Anta aldrig resultatet av `ListBlobsSegmentedAsync` ska tas emot på en enda sida. Titta alltid efter en fortsättningstoken och använd den om den finns.

### <a name="processing-list-results"></a>Bearbetning av listresultat

Objektet du får tillbaka från `ListBlobsSegmentedAsync` innehåller en `Results`-egenskap av typen `IEnumerable<IListBlobItem>`. Gränssnittet `IListBlobItem` innehåller endast ett fåtal egenskaper om blob-containern och webbadressen och är inte särskilt användbar på egen hand.

Du kan få mer användbara blob-objekt från `Results` genom att använda metoden `OfType<>` för att filtrera och konvertera resultatet till mer specifika blob-objekttyper. Några exempel:

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

> [!TIP]
> Om du vill använda `OfType<>` krävs en referens till namnområdet `System.Linq` (`using System.Linq;`).

## <a name="exercise"></a>Övning

En av funktionerna i vår app kräver att du hämtar en lista över blobar från API:et. Vi använder mönstret som visas ovan för att visa alla blobar i vår container. Vi hittar namnet för varje blob när vi bearbetar listan.

Ersätt `GetNames` med `BlobStorage.cs` i redigeraren med följande kod och spara dina ändringar.

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
> Observera att metodsignaturen nu måste ange `async`.

De namn som returneras av den här metoden bearbetas av `FilesController` som omvandlar dem till webbadresser. När de returneras till klienten renderas de som hyperlänkar på sidan.