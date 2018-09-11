<span data-ttu-id="6db81-101">Nu gör vi färdigt programmet genom att skriva kod som läser nästa meddelande i kön, bearbetar det och sedan tar bort det från kön.</span><span class="sxs-lookup"><span data-stu-id="6db81-101">Now we want to complete the application by writing code to read the next message in the queue, processes it, and then delete it from the queue.</span></span> 

<span data-ttu-id="6db81-102">Vi placerar koden i samma program och kör den när du inte skickar några parametrar, men i vårt nyhetsscenario skulle vi egentligen placera koden på våra mellannivåservrar för bearbetning av nyheterna.</span><span class="sxs-lookup"><span data-stu-id="6db81-102">We're going to place this code into the same application and execute it when you don't pass any parameters, however in our news service scenario, we would really place this code into our middle-tier servers to process the stories.</span></span>

## <a name="dequeue-a-message"></a><span data-ttu-id="6db81-103">Ta bort ett meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="6db81-103">Dequeue a message</span></span>

<span data-ttu-id="6db81-104">Vi lägger till en ny metod som hämtar nästa meddelande från kön.</span><span class="sxs-lookup"><span data-stu-id="6db81-104">Let's add a new method that retrieves the next message from the queue.</span></span>

1. <span data-ttu-id="6db81-105">Växla till mappen `QueueApp` i Cloud Shell och skriv `code .` för att öppna onlineredigeraren.</span><span class="sxs-lookup"><span data-stu-id="6db81-105">Switch to the `QueueApp` folder in the Cloud Shell and type `code .` to open the online editor.</span></span>
 
1. <span data-ttu-id="6db81-106">Öppna källfilen `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="6db81-106">Open the `Program.cs` source file.</span></span>

1. <span data-ttu-id="6db81-107">Skapa en statisk metod i klassen `Program` med namnet `ReceiveArticleAsync` som inte tar några parametrar och returnerar en `Task<string>`.</span><span class="sxs-lookup"><span data-stu-id="6db81-107">Create a static method in the `Program` class named `ReceiveArticleAsync` that takes no parameters and returns a `Task<string>`.</span></span> <span data-ttu-id="6db81-108">Vi använder den här metoden till att hämta en nyhetsartikel från kön och returnera den.</span><span class="sxs-lookup"><span data-stu-id="6db81-108">We'll use this method to pull a news article from the queue and return it.</span></span>
    - <span data-ttu-id="6db81-109">Gå vidare och lägg till nyckelordet `async` i metoden eftersom vi ska använda en del asynkrona `Task`-baserade metoder.</span><span class="sxs-lookup"><span data-stu-id="6db81-109">Go ahead and add the `async` keyword to the method since we'll be using some asynchronous `Task`-based methods.</span></span>

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
    
1. <span data-ttu-id="6db81-110">I metoden `ReceiveArticleAsync` ska du anropa den nya `GetQueue`-metoden för att hämta köreferensen och tilldela den till en variabel.</span><span class="sxs-lookup"><span data-stu-id="6db81-110">In your `ReceiveArticleAsync` method, call the new `GetQueue` method to retrieve your queue reference and assign it to a variable.</span></span>

1. <span data-ttu-id="6db81-111">Anropa sedan metoden `ExistsAsync` i `CloudQueue`-objektet, då returneras information om kön har skapats.</span><span class="sxs-lookup"><span data-stu-id="6db81-111">Next, call the `ExistsAsync` method on the `CloudQueue` object; this will return whether the queue has been created.</span></span> <span data-ttu-id="6db81-112">Om vi försöker hämta ett meddelande från en kö som inte finns genereras ett undantag i API:t.</span><span class="sxs-lookup"><span data-stu-id="6db81-112">If we attempt to retrieve a message from a non-existent queue, the API will throw an exception.</span></span>
    - <span data-ttu-id="6db81-113">Den här metoden är asynkron, så använde `await` för att hämta returvärdet.</span><span class="sxs-lookup"><span data-stu-id="6db81-113">This method is asynchronous so use `await` to get the return value.</span></span>
    - <span data-ttu-id="6db81-114">Du bör redan ha nyckelordet `async` i metoden `ReceiveArticleAsync`, annars lägger du till det nu.</span><span class="sxs-lookup"><span data-stu-id="6db81-114">You should already have the `async` keyword on the `ReceiveArticleAsync` method, but if not add it now.</span></span>


1. <span data-ttu-id="6db81-115">Lägg till ett `if`-block som använder returvärdet från `ExistsAsync`.</span><span class="sxs-lookup"><span data-stu-id="6db81-115">Add an `if` block that uses the return value from `ExistsAsync`.</span></span> <span data-ttu-id="6db81-116">Vi lägger till kod för att läsa in ett värde från kön till blocket.</span><span class="sxs-lookup"><span data-stu-id="6db81-116">We'll add our code to read a value from the queue into the block.</span></span> <span data-ttu-id="6db81-117">Lägg till en sista retursträngen i metoden som anger att inget värde har lästs.</span><span class="sxs-lookup"><span data-stu-id="6db81-117">Add a final return string to the method that indicates no value was read.</span></span> <span data-ttu-id="6db81-118">Metoden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="6db81-118">Your method should be looking something like this:</span></span>

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

1. <span data-ttu-id="6db81-119">Anropa `GetMessageAsync` i `CloudQueue`-objektet för att hämta den första `CloudQueueMessage`-instansen från kön.</span><span class="sxs-lookup"><span data-stu-id="6db81-119">Call `GetMessageAsync` on the `CloudQueue` object to get the first `CloudQueueMessage` from the queue.</span></span> <span data-ttu-id="6db81-120">Det returnerade värdet blir `null` om kön är tom.</span><span class="sxs-lookup"><span data-stu-id="6db81-120">The return value will be `null` if the queue is empty.</span></span>

1. <span data-ttu-id="6db81-121">Om värdet inte är null använder du egenskapen `AsString` i `CloudQueueMessage`-objektet till att hämta meddelandets innehåll.</span><span class="sxs-lookup"><span data-stu-id="6db81-121">If it's non-null, use the `AsString` property on the `CloudQueueMessage` object to get the contents of the message.</span></span>

1. <span data-ttu-id="6db81-122">Anropa `DeleteMessageAsync` i `CloudQueue`-objektet för att ta bort meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="6db81-122">Call `DeleteMessageAsync` on the `CloudQueue` object to delete the message from the queue.</span></span>

<span data-ttu-id="6db81-123">Implementeringen av den sista metoden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="6db81-123">The final method implementation should resemble:</span></span>

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

## <a name="call-the-receivearticleasync-method"></a><span data-ttu-id="6db81-124">Anropa metoden ReceiveArticleAsync</span><span class="sxs-lookup"><span data-stu-id="6db81-124">Call the ReceiveArticleAsync method</span></span>

<span data-ttu-id="6db81-125">Slutligen ska vi lägga till stöd för att anropa vår nya metod.</span><span class="sxs-lookup"><span data-stu-id="6db81-125">Finally, let's add support to invoke our new method.</span></span> <span data-ttu-id="6db81-126">Det här gör vi när vi inte skickar några parametrar till programmet.</span><span class="sxs-lookup"><span data-stu-id="6db81-126">We'll do this when we don't pass any parameters into the program.</span></span>

1. <span data-ttu-id="6db81-127">Leta rätt på metoden `Main`, och i synnerhet `if`-blocket du lade till tidigare för att söka efter parametrar.</span><span class="sxs-lookup"><span data-stu-id="6db81-127">Locate the `Main` method and specifically the `if` block you added earlier to look for parameters.</span></span>

1. <span data-ttu-id="6db81-128">Lägg till ett `else`-villkor och anropa metoden `ReceiveArticleAsync`.</span><span class="sxs-lookup"><span data-stu-id="6db81-128">Add an `else` condition and call the `ReceiveArticleAsync` method.</span></span> 

1. <span data-ttu-id="6db81-129">Eftersom metoden är asynkron vill vi normalt använda `await`, men som vi förklarade tidigare så fungerar inte det i alla versioner av C#. Använd istället egenskapen `Result` till att hämta returvärdet och skriva ut det i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="6db81-129">Since it's asynchronous, we would normally want to use `await`, however as explained earlier that doesn't work in all versions of C# - so just use the `Result` property to get the return value and print it to the console window.</span></span>

<span data-ttu-id="6db81-130">Koden bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="6db81-130">Your code should look something like:</span></span>

```csharp
if (args.Length > 0)
{
    // ...
}
else
{
    string value = ReceiveArticleAsync().Result;
    Console.WriteLine($"Received {value}");
}
```

## <a name="execute-the-application"></a><span data-ttu-id="6db81-131">Kör programmet</span><span class="sxs-lookup"><span data-stu-id="6db81-131">Execute the application</span></span>

<span data-ttu-id="6db81-132">Koden är nu färdig.</span><span class="sxs-lookup"><span data-stu-id="6db81-132">The code is now complete.</span></span> <span data-ttu-id="6db81-133">Programmet kan skicka och hämta meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6db81-133">It can now send and retrieve messages.</span></span> <span data-ttu-id="6db81-134">Testa det med `dotnet run` och ange parametrar för att skicka meddelanden, och kör programmet utan parametrar för att läsa enstaka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6db81-134">To test it, use `dotnet run` and pass parameters to send messages, and leave off parameters to read a single message.</span></span>

<span data-ttu-id="6db81-135">Om du vill testa programmet när det inte finns någon kö kan du ta bort kön (och alla data) med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6db81-135">If you want to test when a queue doesn't exist, you can delete the queue (and all the data) with the Azure CLI.</span></span> <span data-ttu-id="6db81-136">Se till att ersätta parametern `<connection-string>` (eller ställa in miljövariabeln).</span><span class="sxs-lookup"><span data-stu-id="6db81-136">Make sure to replace the `<connection-string>` parameter (or set the environment variable).</span></span>

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="6db81-137">Nästa gång du lägger till ett meddelande ska kön återskapas.</span><span class="sxs-lookup"><span data-stu-id="6db81-137">The next time you add a message, the queue should be re-created.</span></span>