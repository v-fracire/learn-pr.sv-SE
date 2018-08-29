När vi har en referens till en blob kan vi överföra och hämta data. `ICloudBlob`-objekt har `Upload`- och `Download`-metoder som stöder bytematriser, strömmar och filer som källor och mål. Vissa typer har ytterligare metoder för att underlätta &mdash; exempelvis har `CloudBlockBlob` stöd för att överföra och hämta strängar med `UploadTextAsync` och `DownloadTextAsync`.

## <a name="creating-new-blobs"></a>Skapa nya blobbar

Om du vill skapa en ny blob anropar du en av `Upload`-metoderna för en blob som inte finns. Två saker händer: Blobben skapas och data överförs. 

## <a name="moving-data-to-and-from-blobs"></a>Flytta data till och från blobbar

Att flytta data till och från en blob är en nätverksåtgärd som tar tid. I Azure Storage SDK för .NET Core kommer alla metoder som kräver nätverksaktivitet att returnera `Task`, så se till att dina kontrollantmetoder är `async` med relevanta värden och att du använder `await` för metodanrop i stället för `Wait`.

En vanlig rekommendation när du arbetar med stora dataobjekt är att använda strömmar i stället för minnesinterna strukturer som t.ex. bytematriser eller strängar. Detta förhindrar att hela innehållet buffras i minnet innan det skickas till målet. ASP.NET Core har stöd för läsning och skrivning av strömmar från begäranden och svar.

## <a name="concurrent-access"></a>Samtidig åtkomst

Det kan hända att andra processer kan lägga till, ändra eller ta bort blobbar samtidigt som din app använder dem. Använd alltid kod på ett defensivt sätt och förutsäg problem som kan orsakas av samtidighet, t.ex att blobbar tas bort samtidigt som du försöker ladda ned från dem eller blobbar vars innehåll ändras när du inte förväntar dig det. Se avsnittet Ytterligare resurser i slutet av den här modulen för information om hur du använder AccessConditions och blobblån för att hantera samtidig blobbåtkomst.

## <a name="exercise"></a>Övning

Nu ska vi slutföra vår app genom att lägga till uppladdnings- och nedladdningskod, och sedan distribuera den till Azure App Service för att testa den.

### <a name="upload"></a>Ladda upp

Vi laddar upp en blob genom att implementera metoden `BlobStorage.Save` med hjälp av `GetBlockBlobReference` för att få en `CloudBlockBlob` från behållaren. `FilesController.Upload` skickar filströmmen till `Save`, så att vi kan använda `UploadFromStreamAsync` till att genomföra överföringen för maximal effektivitet.

Öppna `BlobStorage.cs` i redigeringsprogrammet och fyll i `Save`-implementeringen med följande kod:

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
> Den strömbaserade uppladdningskod som visas här är effektivare än att läsa filen till en bytematris innan den skickas till Azures blobblagring. Men den `IFormFile`-teknik som vi använder för att hämta filen från klienten är inte en verklig strömmande implementering från slutpunkt till slutpunkt och passar bara till att hantera uppladdningar av små filer. Se avsnittet Ytterligare resurser i slutet av den här modulen för mer information om helt strömmade filöverföringar.

### <a name="download"></a>Ladda ned

`BlobStorage.Load` returnerar en `Stream`, vilket innebär att vår kod inte behöver flytta byte fysiskt från blobblagringen alls &mdash; vi behöver bara returnera en referens till blobbströmmen. Vi kan göra det med `OpenReadAsync`. ASP.NET Core hanterar läsningen och stänger dataströmmen när den skapar klientsvaret.

Fyll i `Load` med den här koden:

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a>Publicera och köra i Azure

Vår app är klar &mdash; nu ska vi distribuera den och se hur den fungerar. Kör följande kod i Azure Cloud Shell-terminalen för att skapa koden och distribuera den till en ny App Service-instans. Vi ska också lägga till lagringskontots anslutningssträng till konfigurationen.

```console

```

...

Överför och hämta några filer om du vill testa appen och kör sedan följande i gränssnittet för att se vilka blobbar som har överförts:

```console

```

**TODO-bild från portalen**