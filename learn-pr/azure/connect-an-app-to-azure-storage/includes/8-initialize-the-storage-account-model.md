<span data-ttu-id="fcdbb-101">Azure Storages klientbibliotek tillhandahåller en objektmodell som används för att interagera med Azure Storage-kontona.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-101">The Azure Storage client library provides an object model that is used to interact with Azure Storage accounts.</span></span> <span data-ttu-id="fcdbb-102">Den används för att snabbt ansluta till ett Azure Storage-konto och använda Azure Storages tjänst-API:er.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-102">It's used to quickly connect to an Azure Storage account and use the Azure Storage service APIs.</span></span> 

<span data-ttu-id="fcdbb-103">Här ska du få använda kontots objektmodell i koden för att upprätta en anslutning till Azure Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-103">Here, you'll use the account object model in your code to connect to your Azure Storage account.</span></span>

## <a name="azure-storage-client-library-object-model"></a><span data-ttu-id="fcdbb-104">Objektmodell från Azure Storages klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="fcdbb-104">Azure Storage client library object model</span></span>

<span data-ttu-id="fcdbb-105">Förmågan att förstå hur man använder objektmodellen från Azure Storages klientbibliotek är själva nyckeln till en enkel implementering vid anslutning till ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-105">Understanding how to use the Azure Storage client library object model is key to simplicity of implementation when connecting to your Azure Storage account.</span></span>

<span data-ttu-id="fcdbb-106">Själva grunden till objektmodellen från .NET Core-klientbiblioteket är **CloudStorageAccount**.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-106">The foundation of the storage account object model in the .NET Core client library is **CloudStorageAccount**.</span></span> <span data-ttu-id="fcdbb-107">Det enklaste sättet att initiera objektmodellen är att använda `CloudStorageAccount.Parse` eller `CloudStorageAccount.TryParse` för att parsa anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-107">The simplest way to initialize the object model is to use `CloudStorageAccount.Parse` or `CloudStorageAccount.TryParse` to parse the connection string.</span></span>

<span data-ttu-id="fcdbb-108">Klientbiblioteket försöker inte ansluta förrän en åtgärd som efterfrågar detta anropas.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-108">The client library will not attempt to connect until an operation is invoked that requires it.</span></span> <span data-ttu-id="fcdbb-109">`Parse()` och `TryParse()` säkerställer endast att anslutningssträngen är välformad, de kan de inte verifiera att kontot finns eller att nyckeln är korrekt.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-109">`Parse()` and `TryParse()` only guarantee that the connection string is well-formatted, they don't verify that the account exists or that the key is correct.</span></span> <span data-ttu-id="fcdbb-110">Den resulterande `CloudStorageAccount`-instansen som returneras av metodanropen `Parse()` eller `TryParse()` visar metoder som används för att skapa en klient för blob-, fil-, tabell- eller kölagring.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-110">The resulting `CloudStorageAccount` instance returned from the `Parse()` or `TryParse()` method call exposes methods to create a client for blob, file, table, or queue storage types.</span></span> <span data-ttu-id="fcdbb-111">Detta används sedan för att skapa instanser av klientobjektet för blob-, fil-, kö- och tabellagring.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-111">This is then used to create instances of service client objects for the blob, file, queue and table storage services.</span></span> <span data-ttu-id="fcdbb-112">Kodfragmentet nedan visar ett exempel på hur du skapar en klient för användning med bloblagring:</span><span class="sxs-lookup"><span data-stu-id="fcdbb-112">The code snippet below shows an example of creating a client to use for blob storage:</span></span>

```c#
using Microsoft.WindowsAzure.Storage;

var storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");
var blobClient = storageAccount.CreateCloudBlobClient()
```

<span data-ttu-id="fcdbb-113">`CloudStorageAccount` och klientobjekten är enkla och kan skapas på begäran eller i förväg för delning i ditt program.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-113">`CloudStorageAccount` and the client objects are lightweight which can be created on demand or created upfront to be shared within your application.</span></span> <span data-ttu-id="fcdbb-114">En vanlig metod är att använda `CloudStorageAccount.TryParse()` nära startpunkten för programmet för att skapa en instans och göra den tillgänglig i ditt program när du vill skapa klientinstanser.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-114">A standard approach is to use `CloudStorageAccount.TryParse()` near the entry point of your application to create an instance, and make it available within your application for creating client instances.</span></span>

## <a name="summary"></a><span data-ttu-id="fcdbb-115">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="fcdbb-115">Summary</span></span>

<span data-ttu-id="fcdbb-116">Objektmodellen från Azure Storages klientbibliotek gör det enklare att ansluta till Azure Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-116">The Azure Storage client library object model provides a simple way to connect to your Azure Storage account.</span></span> <span data-ttu-id="fcdbb-117">När du tillhandahåller en anslutningssträng kontrollerar objektmodellen dess format och att kontot finns.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-117">Your job is to provide a connection string and the object model will check its format and verify that the account exists.</span></span> <span data-ttu-id="fcdbb-118">När du väl har en `CloudStorageAccount`-instans kan du skapa nya klienter för blob-, fil-, tabell- eller kölagring.</span><span class="sxs-lookup"><span data-stu-id="fcdbb-118">Once you have a `CloudStorageAccount` instance you're able to create clients for blob, file, table, or queue storage types.</span></span>