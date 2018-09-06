Azure Storages klientbibliotek tillhandahåller en objektmodell som används för att interagera med Azure Storage-kontona. Den används för att snabbt ansluta till ett Azure Storage-konto och använda Azure Storages tjänst-API:er. 

Här ska du få använda kontots objektmodell i koden för att upprätta en anslutning till Azure Storage-kontot.

## <a name="azure-storage-client-library-object-model"></a>Objektmodell från Azure Storages klientbibliotek

Förmågan att förstå hur man använder objektmodellen från Azure Storages klientbibliotek är själva nyckeln till en enkel implementering vid anslutning till ditt Azure Storage-konto.

Själva grunden till objektmodellen från .NET Core-klientbiblioteket är **CloudStorageAccount**. Det enklaste sättet att initiera objektmodellen är att använda `CloudStorageAccount.Parse` eller `CloudStorageAccount.TryParse` för att parsa anslutningssträngen.

Klientbiblioteket försöker inte ansluta förrän en åtgärd som efterfrågar detta anropas. `Parse()` och `TryParse()` säkerställer endast att anslutningssträngen är välformad, de kan de inte verifiera att kontot finns eller att nyckeln är korrekt. Den resulterande `CloudStorageAccount`-instansen som returneras av metodanropen `Parse()` eller `TryParse()` visar metoder som används för att skapa en klient för blob-, fil-, tabell- eller kölagring. Detta används sedan för att skapa instanser av klientobjektet för blob-, fil-, kö- och tabellagring. Kodfragmentet nedan visar ett exempel på hur du skapar en klient för användning med bloblagring:

```c#
using Microsoft.WindowsAzure.Storage;

var storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");
var blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` och klientobjekten är enkla och kan skapas på begäran eller i förväg för delning i ditt program. En vanlig metod är att använda `CloudStorageAccount.TryParse()` nära startpunkten för programmet för att skapa en instans och göra den tillgänglig i ditt program när du vill skapa klientinstanser.

## <a name="summary"></a>Sammanfattning

Objektmodellen från Azure Storages klientbibliotek gör det enklare att ansluta till Azure Storage-kontot. När du tillhandahåller en anslutningssträng kontrollerar objektmodellen dess format och att kontot finns. När du väl har en `CloudStorageAccount`-instans kan du skapa nya klienter för blob-, fil-, tabell- eller kölagring.