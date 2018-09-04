<span data-ttu-id="67b41-101">Här går vi igenom hur du skriver kod för att få åtkomst till en kö.</span><span class="sxs-lookup"><span data-stu-id="67b41-101">Here, we are going to discuss how to write code to access a queue.</span></span> <span data-ttu-id="67b41-102">I exemplet med fotodelning skulle detta användas av klientdelsappar för att lägga till foton till kön och av medelnivån att hämta meddelanden för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="67b41-102">In the photo-sharing example, this would be used by the front-end apps to add photos to the queue and by the mid-tier to retrieve messages for processing.</span></span>

## <a name="message-flow"></a><span data-ttu-id="67b41-103">Meddelandeflöde</span><span class="sxs-lookup"><span data-stu-id="67b41-103">Message flow</span></span>

<span data-ttu-id="67b41-104">Det typiska flödet för ett meddelande genom kön visas nedan.</span><span class="sxs-lookup"><span data-stu-id="67b41-104">The typical flow of a message through the queue is shown below.</span></span> <span data-ttu-id="67b41-105">Avsändaren skapar kön och lägger till ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="67b41-105">The sender creates the queue and adds a message.</span></span> <span data-ttu-id="67b41-106">Mottagaren hämtar ett meddelande, bearbetar det och sedan tar bort meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="67b41-106">The receiver retrieves a message, processes it, and then deletes the message from the queue.</span></span>

![Meddelandeflöde](../media-draft/6-message-flow.png)

<span data-ttu-id="67b41-108">Observera att _get_ (hämta) och _delete_ (ta bort) är separata åtgärder.</span><span class="sxs-lookup"><span data-stu-id="67b41-108">Notice that _get_ and _delete_ are separate operations.</span></span> <span data-ttu-id="67b41-109">Den här strukturen hanterar potentiella fel i mottagaren och implementerar ett begrepp som kallas _leverans minst en gång_.</span><span class="sxs-lookup"><span data-stu-id="67b41-109">This arrangement handles potential failures in the receiver and implements a concept called _at-least-once delivery_.</span></span> <span data-ttu-id="67b41-110">När mottagaren får ett meddelande finns meddelandet kvar i kön men är osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="67b41-110">After the receiver gets a message, that message remains in the queue but is invisible for 30 seconds.</span></span> <span data-ttu-id="67b41-111">Om mottagaren kraschar eller utsätts för ett strömavbrott under bearbetningen tar den aldrig bort meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="67b41-111">If the receiver crashes or experiences a power failure during processing, then it will never delete the message from the queue.</span></span> <span data-ttu-id="67b41-112">Efter 30 sekunder visas meddelandet i kön igen, och då kan en annan instans av mottagaren bearbeta det så att det slutförs.</span><span class="sxs-lookup"><span data-stu-id="67b41-112">After 30 seconds, the message will reappear in the queue and another instance of the receiver can process it to completion.</span></span>

## <a name="the-azure-storage-client-library-for-net"></a><span data-ttu-id="67b41-113">Azure Storage-klientbiblioteket för .NET</span><span class="sxs-lookup"><span data-stu-id="67b41-113">The Azure Storage Client Library for .NET</span></span>

<span data-ttu-id="67b41-114">Azure Storage-klientbiblioteket för .NET tillhandahåller typer för att representera vart och ett av de objekt som du behöver för att interagera med:</span><span class="sxs-lookup"><span data-stu-id="67b41-114">The Azure Storage Client Library for .NET provides types to represent each of the objects you need to interact with:</span></span>

- <span data-ttu-id="67b41-115">`CloudStorageAccount` representerar ditt Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="67b41-115">`CloudStorageAccount` represents your Azure Storage account</span></span>
- <span data-ttu-id="67b41-116">`CloudQueueClient` representerar Azure Queue Storage-tjänsten</span><span class="sxs-lookup"><span data-stu-id="67b41-116">`CloudQueueClient` represents the Azure Queue storage service</span></span>
- <span data-ttu-id="67b41-117">`CloudQueue` representerar en av dina köinstanser</span><span class="sxs-lookup"><span data-stu-id="67b41-117">`CloudQueue` represents one of your queue instances</span></span>
- <span data-ttu-id="67b41-118">`CloudQueueMessage` representerar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="67b41-118">`CloudQueueMessage` represents a message.</span></span>

<span data-ttu-id="67b41-119">Du använder dessa klasser för att få programmatisk åtkomst till din kö.</span><span class="sxs-lookup"><span data-stu-id="67b41-119">You will use these classes to get programmatic access to your queue.</span></span> <span data-ttu-id="67b41-120">Biblioteket har både synkrona och asynkrona metoder. Du använder de asynkrona versionerna för att undvika blockering i klientapparna.</span><span class="sxs-lookup"><span data-stu-id="67b41-120">The library has both synchronous and asynchronous methods; you will use the asynchronous versions to avoid blocking in the client apps.</span></span>

> [!NOTE]
> <span data-ttu-id="67b41-121">Azure Storage-klientbiblioteket för .NET finns i **WindowsAzure.Storage** NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="67b41-121">The Azure Storage Client Library for .NET is available in the **WindowsAzure.Storage** NuGet package.</span></span>

## <a name="how-to-connect"></a><span data-ttu-id="67b41-122">Så ansluter du</span><span class="sxs-lookup"><span data-stu-id="67b41-122">How to connect</span></span>

<span data-ttu-id="67b41-123">Om du vill ansluta till en kö måste du först skapa en `CloudStorageAccount`, därefter en `CloudQueueClient` och slutligen en `CloudQueue`-instans.</span><span class="sxs-lookup"><span data-stu-id="67b41-123">To connect to a queue, you first create a `CloudStorageAccount`, then a `CloudQueueClient`, and finally a `CloudQueue` instance.</span></span> <span data-ttu-id="67b41-124">Koden visas nedan.</span><span class="sxs-lookup"><span data-stu-id="67b41-124">The code is shown below.</span></span>

```C#
CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);

CloudQueueClient client = account.CreateCloudQueueClient();

CloudQueue queue = client.GetQueueReference("myqueue");
```

## <a name="how-to-create-a-queue"></a><span data-ttu-id="67b41-125">Så skapar du en resurs</span><span class="sxs-lookup"><span data-stu-id="67b41-125">How to create a queue</span></span>

<span data-ttu-id="67b41-126">Du kommer att använda ett vanligt mönster för skapandet av kön: avsändarprogrammet blir ansvarigt för att skapa kön.</span><span class="sxs-lookup"><span data-stu-id="67b41-126">You will use a common pattern for queue creation: the sender application will be responsible for creating the queue.</span></span> <span data-ttu-id="67b41-127">Det här gör att programmet blir mer självständigt och mindre beroende av administrativ konfiguration.</span><span class="sxs-lookup"><span data-stu-id="67b41-127">This keeps your application more self-contained and less dependant on administrative set-up.</span></span> <span data-ttu-id="67b41-128">Den typiska koden visas nedan.</span><span class="sxs-lookup"><span data-stu-id="67b41-128">Typical code is shown below.</span></span>

```C#
CloudQueue queue;
//...

await queue.CreateIfNotExistsAsync();
```

## <a name="how-to-send-a-message"></a><span data-ttu-id="67b41-129">Så skickar du ett meddelande</span><span class="sxs-lookup"><span data-stu-id="67b41-129">How to send a message</span></span>

<span data-ttu-id="67b41-130">För att skicka ett meddelande instansierar du ett `CloudQueueMessage`objekt.</span><span class="sxs-lookup"><span data-stu-id="67b41-130">To send a message, you instantiate a `CloudQueueMessage` object.</span></span> <span data-ttu-id="67b41-131">Klassen har några överlagrade konstruktorer som läser in dina data i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="67b41-131">The class has a few overloaded constructors that load your data into the message.</span></span> <span data-ttu-id="67b41-132">Vi använder den konstruktor som tar en sträng.</span><span class="sxs-lookup"><span data-stu-id="67b41-132">We will use the constructor that takes a string.</span></span> <span data-ttu-id="67b41-133">Efter att meddelandet har skapats kan du använda ett `CloudQueue`-objekt för att skicka det (se nedan).</span><span class="sxs-lookup"><span data-stu-id="67b41-133">After creating the message, you use a `CloudQueue` object to send it (see below).</span></span>

```C#
var message = new CloudQueueMessage("your message here");

CloudQueue queue;
//...

await queue.AddMessageAsync(message);
```

## <a name="how-to-receive-and-delete-a-message"></a><span data-ttu-id="67b41-134">Så tar du emot och tar bort ett meddelande</span><span class="sxs-lookup"><span data-stu-id="67b41-134">How to receive and delete a message</span></span>

<span data-ttu-id="67b41-135">I mottagaren hämtar nästa meddelande, bearbetar det och tar sedan bort det efter att bearbetningen är klar (se nedan).</span><span class="sxs-lookup"><span data-stu-id="67b41-135">In the receiver, you get the next message, process it, and then delete it after processing succeeds (see below).</span></span>

```C#
CloudQueue queue;
//...

CloudQueueMessage message = await queue.GetMessageAsync();

if (message != null)
{
    // Process the message
    //...

    await queue.DeleteMessageAsync(message);
}
```

## <a name="summary"></a><span data-ttu-id="67b41-136">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="67b41-136">Summary</span></span>

<span data-ttu-id="67b41-137">Azure Storage-klientbiblioteket för .NET ger ett enkelt sätt att få åtkomst till köer programmatiskt utan att behöva hantera råa REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="67b41-137">The Azure Storage Client Library for .NET provides a convenient way to access queues programmatically without the need to deal with raw REST calls.</span></span> <span data-ttu-id="67b41-138">Om du förstår leverans minst en gång och asynkrona anrop blir det enklare att använda biblioteket på ett effektivt sätt.</span><span class="sxs-lookup"><span data-stu-id="67b41-138">Understanding at-least-once delivery and asynchronous calls will help you use the library effectively.</span></span>