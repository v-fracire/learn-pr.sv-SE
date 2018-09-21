<span data-ttu-id="a805c-101">Nu gör vi färdigt programmet genom att skriva kod som läser nästa meddelande i kön, bearbetar det och tar bort det från kön.</span><span class="sxs-lookup"><span data-stu-id="a805c-101">Now we want to complete the application by writing code to read the next message in the queue, process it, and delete it from the queue.</span></span> 

<span data-ttu-id="a805c-102">Vi placerar koden i samma program och kör den när du inte skickar några parametrar, men i vårt nyhetsscenario skulle vi egentligen placera koden på våra mellannivåservrar för bearbetning av nyheterna.</span><span class="sxs-lookup"><span data-stu-id="a805c-102">We're going to place this code into the same application and execute it when you don't pass any parameters, however in our news service scenario, we would really place this code into our middle-tier servers to process the stories.</span></span>

## <a name="dequeue-a-message"></a><span data-ttu-id="a805c-103">Ta bort ett meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="a805c-103">Dequeue a message</span></span>

<span data-ttu-id="a805c-104">Vi lägger till en ny metod som hämtar nästa meddelande från kön.</span><span class="sxs-lookup"><span data-stu-id="a805c-104">Let's add a new method that retrieves the next message from the queue.</span></span>

1. <span data-ttu-id="a805c-105">Öppna källfilen `Program.cs` i din redigerare.</span><span class="sxs-lookup"><span data-stu-id="a805c-105">Open the `Program.cs` source file in your editor.</span></span>

1. <span data-ttu-id="a805c-106">Skapa en statisk metod i klassen `Program` med namnet `ReceiveArticleAsync` som inte tar några parametrar och returnerar en `Task<string>`.</span><span class="sxs-lookup"><span data-stu-id="a805c-106">Create a static method in the `Program` class named `ReceiveArticleAsync` that takes no parameters and returns a `Task<string>`.</span></span> <span data-ttu-id="a805c-107">Vi använder den här metoden till att hämta en nyhetsartikel från kön och returnera den.</span><span class="sxs-lookup"><span data-stu-id="a805c-107">We'll use this method to pull a news article from the queue and return it.</span></span>
    - <span data-ttu-id="a805c-108">Gå vidare och lägg till nyckelordet `async` i metoden eftersom vi ska använda en del asynkrona `Task`-baserade metoder.</span><span class="sxs-lookup"><span data-stu-id="a805c-108">Go ahead and add the `async` keyword to the method since we'll be using some asynchronous `Task`-based methods.</span></span>

    ```csharp
    static async Task<string> ReceiveArticleAsync()
    {
    }

1. All of the setup code to get a `CloudQueue` will be identical to what we did in the last exercise. Code duplication is a bad habit, even in samples so go ahead and, refactor the code that obtains the `CloudQueue` to a new method named `GetQueue` and change the `SendArticleAsync` to use your new method.
     - Make sure to leave the code that _creates_ the queue in the `SendArticleAsync` method; remember only the **publisher** should create the queue.

    ```csharp
    const string ConnectionString = ...;
    // ...

    static CloudQueue GetQueue()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        return queueClient.GetQueueReference("newsqueue");
    }
    ```
    
1. <span data-ttu-id="a805c-109">I metoden `ReceiveArticleAsync` ska du anropa den nya `GetQueue`-metoden för att hämta köreferensen och tilldela den till en variabel.</span><span class="sxs-lookup"><span data-stu-id="a805c-109">In your `ReceiveArticleAsync` method, call the new `GetQueue` method to retrieve your queue reference and assign it to a variable.</span></span>

1. <span data-ttu-id="a805c-110">Anropa sedan metoden `ExistsAsync` i `CloudQueue`-objektet, då returneras information om kön har skapats.</span><span class="sxs-lookup"><span data-stu-id="a805c-110">Next, call the `ExistsAsync` method on the `CloudQueue` object; this will return whether the queue has been created.</span></span> <span data-ttu-id="a805c-111">Om vi försöker hämta ett meddelande från en kö som inte finns genereras ett undantag i API:t.</span><span class="sxs-lookup"><span data-stu-id="a805c-111">If we attempt to retrieve a message from a non-existent queue, the API will throw an exception.</span></span>
    - <span data-ttu-id="a805c-112">Den här metoden är asynkron, så använde `await` för att hämta returvärdet.</span><span class="sxs-lookup"><span data-stu-id="a805c-112">This method is asynchronous so use `await` to get the return value.</span></span>
    - <span data-ttu-id="a805c-113">Du bör redan ha nyckelordet `async` i metoden `ReceiveArticleAsync`, annars lägger du till det nu.</span><span class="sxs-lookup"><span data-stu-id="a805c-113">You should already have the `async` keyword on the `ReceiveArticleAsync` method, but if not add it now.</span></span>


1. <span data-ttu-id="a805c-114">Lägg till ett `if`-block som använder returvärdet från `ExistsAsync`.</span><span class="sxs-lookup"><span data-stu-id="a805c-114">Add an `if` block that uses the return value from `ExistsAsync`.</span></span> <span data-ttu-id="a805c-115">Vi lägger till kod för att läsa in ett värde från kön till blocket.</span><span class="sxs-lookup"><span data-stu-id="a805c-115">We'll add our code to read a value from the queue into the block.</span></span> <span data-ttu-id="a805c-116">Lägg till en sista retursträngen i metoden som anger att inget värde har lästs.</span><span class="sxs-lookup"><span data-stu-id="a805c-116">Add a final return string to the method that indicates no value was read.</span></span> <span data-ttu-id="a805c-117">Metoden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="a805c-117">Your method should be looking something like this:</span></span>

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
    }

    return "<queue empty or not created>";
}
```

1. <span data-ttu-id="a805c-118">Anropa `GetMessageAsync` i `CloudQueue`-objektet för att hämta den första `CloudQueueMessage`-instansen från kön.</span><span class="sxs-lookup"><span data-stu-id="a805c-118">Call `GetMessageAsync` on the `CloudQueue` object to get the first `CloudQueueMessage` from the queue.</span></span> <span data-ttu-id="a805c-119">Det returnerade värdet blir `null` om kön är tom.</span><span class="sxs-lookup"><span data-stu-id="a805c-119">The return value will be `null` if the queue is empty.</span></span>

1. <span data-ttu-id="a805c-120">Om värdet inte är null använder du egenskapen `AsString` i `CloudQueueMessage`-objektet till att hämta meddelandets innehåll.</span><span class="sxs-lookup"><span data-stu-id="a805c-120">If it's non-null, use the `AsString` property on the `CloudQueueMessage` object to get the contents of the message.</span></span>

1. <span data-ttu-id="a805c-121">Anropa `DeleteMessageAsync` på `CloudQueue`-objektet för att ta bort meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="a805c-121">Call `DeleteMessageAsync` on the `CloudQueue` object to delete the message from the queue.</span></span>

<span data-ttu-id="a805c-122">Implementeringen av den sista metoden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="a805c-122">The final method implementation should resemble:</span></span>

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
        CloudQueueMessage retrievedArticle = await queue.GetMessageAsync();
        if (retrievedArticle != null)
        {
            string newsMessage = retrievedArticle.AsString;
            await queue.DeleteMessageAsync(retrievedArticle);
            return newsMessage;
        }
    }

    return "<queue empty or not created>";
}
```

## <a name="call-the-receivearticleasync-method"></a><span data-ttu-id="a805c-123">Anropa metoden ReceiveArticleAsync</span><span class="sxs-lookup"><span data-stu-id="a805c-123">Call the ReceiveArticleAsync method</span></span>

<span data-ttu-id="a805c-124">Slutligen ska vi lägga till stöd för att anropa vår nya metod.</span><span class="sxs-lookup"><span data-stu-id="a805c-124">Finally, let's add support to invoke our new method.</span></span> <span data-ttu-id="a805c-125">Det här gör vi när vi inte skickar några parametrar till programmet.</span><span class="sxs-lookup"><span data-stu-id="a805c-125">We'll do this when we don't pass any parameters into the program.</span></span>

1. <span data-ttu-id="a805c-126">Leta rätt på metoden `Main`, och i synnerhet `if`-blocket du lade till tidigare för att söka efter parametrar.</span><span class="sxs-lookup"><span data-stu-id="a805c-126">Locate the `Main` method and specifically the `if` block you added earlier to look for parameters.</span></span>

1. <span data-ttu-id="a805c-127">Lägg till ett `else`-villkor och anropa metoden `ReceiveArticleAsync`.</span><span class="sxs-lookup"><span data-stu-id="a805c-127">Add an `else` condition and call the `ReceiveArticleAsync` method.</span></span> 

1. <span data-ttu-id="a805c-128">Eftersom den är asynkron, bör du använda nyckelordet `await` för att hämta resultatet och skriva ut det i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="a805c-128">Since it's asynchronous, use the `await` keyword to retrieve the result and print it to the console window.</span></span> <span data-ttu-id="a805c-129">Om du inte konverterade din app till C# 7.1, kan du hämta värdet med hjälp av egenskapen `Result` från den returnerade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a805c-129">If you didn't convert your app to C# 7.1, you can get the value using the `Result` property from the returning task.</span></span>

<span data-ttu-id="a805c-130">Koden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="a805c-130">Your code should look something like:</span></span>

```csharp
if (args.Length > 0)
{
    // ...
}
else
{
    string value = await ReceiveArticleAsync();
    Console.WriteLine($"Received {value}");
}
```

## <a name="execute-the-application"></a><span data-ttu-id="a805c-131">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="a805c-131">Execute the application</span></span>

<span data-ttu-id="a805c-132">Koden är nu färdig.</span><span class="sxs-lookup"><span data-stu-id="a805c-132">The code is now complete.</span></span> <span data-ttu-id="a805c-133">Programmet kan skicka och hämta meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a805c-133">It can now send and retrieve messages.</span></span> 

> [!WARNING]
> <span data-ttu-id="a805c-134">Se till att du har sparat alla filer i onlineredigeraren innan du kompilerar och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="a805c-134">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="a805c-135">Testa det med `dotnet run` och ange parametrar för att skicka meddelanden, och kör programmet utan parametrar för att läsa enstaka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a805c-135">To test it, use `dotnet run` and pass parameters to send messages, and leave off parameters to read a single message.</span></span>

<span data-ttu-id="a805c-136">Om du vill testa programmet när det inte finns någon kö kan du ta bort kön (och alla data) med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a805c-136">If you want to test when a queue doesn't exist, you can delete the queue (and all the data) with the Azure CLI.</span></span> <span data-ttu-id="a805c-137">Se till att ersätta parametern `<connection-string>` (eller ställa in miljövariabeln).</span><span class="sxs-lookup"><span data-stu-id="a805c-137">Make sure to replace the `<connection-string>` parameter (or set the environment variable).</span></span>

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="a805c-138">Nästa gång du lägger till ett meddelande ska kön återskapas.</span><span class="sxs-lookup"><span data-stu-id="a805c-138">The next time you add a message, the queue should be re-created.</span></span>

> [!NOTE]
> <span data-ttu-id="a805c-139">Borttagningen sker faktiskt asynkront.</span><span class="sxs-lookup"><span data-stu-id="a805c-139">The delete operation actually occurs asynchronously.</span></span> <span data-ttu-id="a805c-140">Om det inte har slutförts kan du få ett undantag vid försök att återskapa kön.</span><span class="sxs-lookup"><span data-stu-id="a805c-140">If it has not completed you may get an exception when you attempt to re-create the queue.</span></span>