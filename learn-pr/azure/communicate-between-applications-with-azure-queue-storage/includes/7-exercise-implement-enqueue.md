<span data-ttu-id="9c3f2-101">Nu när alla krav är uppfyllda kan du skriva kod som skapar en ny lagringskö och lägger till ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-101">Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message.</span></span> <span data-ttu-id="9c3f2-102">Normalt skulle vi placera den här koden i apparna på klientsidan där data genereras.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-102">We would typically place this code in our front-end apps that generate the data.</span></span>

## <a name="add-the-client-library-for-azure-storage"></a><span data-ttu-id="9c3f2-103">Lägg till klientbiblioteket för Azure Storage</span><span class="sxs-lookup"><span data-stu-id="9c3f2-103">Add the client library for Azure Storage</span></span>

<span data-ttu-id="9c3f2-104">Vi börjar med att lägga till **Azure Storage-klientbiblioteket för .NET** i appen.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-104">Let's start by adding the **Azure Storage Client Library for .NET** to our app.</span></span> <span data-ttu-id="9c3f2-105">Du kan installera det med NuGet (en .NET-pakethanterare).</span><span class="sxs-lookup"><span data-stu-id="9c3f2-105">It can be installed with NuGet (a .NET package manager).</span></span> 

> [!NOTE]
> <span data-ttu-id="9c3f2-106">Även om ett nytt Cloud Shell skapades finns dina filer fortfarande kvar.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-106">Even though a new Cloud Shell was created, your files are all still present.</span></span> <span data-ttu-id="9c3f2-107">Du måste dock byta till mappen `QueueApp` eftersom du kommer att stå i roten för lagringsutrymmet.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-107">However, you will need to switch into the `QueueApp` folder since you will now be at the root of your storage area.</span></span> <span data-ttu-id="9c3f2-108">Skriv bara `cd QueueApp` för att byta mapp.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-108">Just type `cd QueueApp` to switch folders.</span></span> <span data-ttu-id="9c3f2-109">Gör det här innan du försöker lägga till paketet eftersom .NET-appen finns i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-109">Make sure to do this before attempting to add the package since the .NET app is in that folder.</span></span>

1. <span data-ttu-id="9c3f2-110">Ändra aktuell katalog till mappen `QueueApp`.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-110">Change the current directory to the `QueueApp` folder.</span></span>

1. <span data-ttu-id="9c3f2-111">Installera paketet `WindowsAzure.Storage` i projektet med kommandot `dotnet add package`.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-111">Install the `WindowsAzure.Storage` package to the project with the `dotnet add package` command.</span></span>

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a><span data-ttu-id="9c3f2-112">Lägg till en metod för att skicka en nyhetsavisering</span><span class="sxs-lookup"><span data-stu-id="9c3f2-112">Add a method to send a news alert</span></span>

<span data-ttu-id="9c3f2-113">Nu ska vi skapa en ny metod för att skicka en nyhet till en kö.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-113">Next, let's create a new method to send a news story into a queue.</span></span>

1. <span data-ttu-id="9c3f2-114">Öppna kodredigeraren genom att skriva `code .`</span><span class="sxs-lookup"><span data-stu-id="9c3f2-114">Open the code editor by typing `code .`</span></span>

1. <span data-ttu-id="9c3f2-115">Öppna filen `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-115">Open the `Program.cs` file.</span></span>

1. <span data-ttu-id="9c3f2-116">Lägg till följande namnområden överst i filen.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-116">At the top of the file, add the following namespaces.</span></span> <span data-ttu-id="9c3f2-117">Vi kommer att använda typer från båda när vi ansluter till Azure Storage och sedan arbetar med köer.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-117">We'll be using types from both of these to connect to Azure Storage and then to work with queues.</span></span>
    - <span data-ttu-id="9c3f2-118">Microsoft.WindowsAzure.Storage</span><span class="sxs-lookup"><span data-stu-id="9c3f2-118">Microsoft.WindowsAzure.Storage</span></span>
    - <span data-ttu-id="9c3f2-119">Microsoft.WindowsAzure.Storage.Queue</span><span class="sxs-lookup"><span data-stu-id="9c3f2-119">Microsoft.WindowsAzure.Storage.Queue</span></span>

1. <span data-ttu-id="9c3f2-120">Skapa en statisk metod i klassen `Program` med namnet `SendArticleAsync` som tar emot ett argument av typen `string` och returnerar ett `Task`-objekt.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-120">Create a static method in the `Program` class named `SendArticleAsync` that takes a `string` and returns a `Task`.</span></span> <span data-ttu-id="9c3f2-121">Vi använder den här metoden till att skicka en nyhetsartikel till vår tjänst.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-121">We'll use this method to send a news article in to our service.</span></span> <span data-ttu-id="9c3f2-122">Ge indataparametern namnet `newsMesasage` enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-122">Name the input parameter `newsMesasage` as shown below.</span></span>

    ```csharp
    ...
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue; 
    
    class Program
    {
        ...
    
        static async Task SendArticleAsync(string newsMessage)
        {
        }
    }
    ```
    
1. <span data-ttu-id="9c3f2-123">Använd den statiska metoden `CloudStorageAccount.Parse` i den nya metoden till att parsa anslutningssträngen (kom ihåg att vi lade den i en konstantsträng).</span><span class="sxs-lookup"><span data-stu-id="9c3f2-123">In your new method, use the static `CloudStorageAccount.Parse` method to parse your connection string (recall we placed it into a constant string).</span></span> <span data-ttu-id="9c3f2-124">Den här metoden returnerar ett `CloudStorageAccount`-objekt som måste lagras i en variabel.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-124">This method returns a `CloudStorageAccount` object that needs to be stored in a variable.</span></span>

1. <span data-ttu-id="9c3f2-125">Anropa metoden `CreateCloudQueueClient()` i lagringskontoobjektet för att få ett klientobjekt och lagra det i en variabel.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-125">Call the `CreateCloudQueueClient()` method on the storage account object to get a client object and store that in a variable.</span></span>

1. <span data-ttu-id="9c3f2-126">Anropa sedan metoden `GetQueueReference` i klientobjektet och skicka strängen `"newsqueue"` som könamn.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-126">Next, call `GetQueueReference` method on the client object and pass the string `"newsqueue"` for the queue name.</span></span> <span data-ttu-id="9c3f2-127">Då returneras ett `CloudQueue`-objekt som vi kan använda för att arbeta med kön.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-127">This returns a `CloudQueue` object that we can use to work with the queue.</span></span> <span data-ttu-id="9c3f2-128">Kön behöver inte finnas ännu.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-128">It's OK if the queue does not exist yet.</span></span>

1. <span data-ttu-id="9c3f2-129">Anropa `CreateIfNotExistsAsync()` på `CloudQueue`-objektet för att se till att kön är redo att användas.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-129">Call `CreateIfNotExistsAsync()` on the `CloudQueue` object to ensure the queue is ready for use.</span></span> <span data-ttu-id="9c3f2-130">Då skapas kön om det behövs.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-130">This will create the queue if necessary.</span></span>
    - <span data-ttu-id="9c3f2-131">Eftersom det här är en asynkron metod använder du C#-nyckelordet `await` så att vi använder den korrekt.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-131">Since this is an asynchronous method, use the C# `await` keyword to ensure we work properly with it.</span></span> <span data-ttu-id="9c3f2-132">Det innebär också att du måste utöka _metoden_ med nyckelordet `async`.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-132">That also means you need to decorate the _method_ with the `async` keyword.</span></span> 
    - <span data-ttu-id="9c3f2-133">`CreateIfNotExistsAsync` returnerar ett `bool`-värde som är `true` om kön har skapats och `false` om den redan fanns.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-133">`CreateIfNotExistsAsync` returns a `bool` value that will be `true` if the queue was created and `false` if it already exists.</span></span> <span data-ttu-id="9c3f2-134">Skicka ett meddelande till konsolen om vi skapade kön.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-134">Output a message to the console if we created the queue.</span></span>
    - <span data-ttu-id="9c3f2-135">Här är ett exempel om du behöver hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-135">Here's an example if you need some help.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. <span data-ttu-id="9c3f2-136">Skapa en instans av ett `CloudQueueMessage`-objekt.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-136">Create an instance of a `CloudQueueMessage`.</span></span> 
    - <span data-ttu-id="9c3f2-137">Det tar ett `string`-värde som parameter, skicka metodparametern (`newsMessage`).</span><span class="sxs-lookup"><span data-stu-id="9c3f2-137">It takes a `string` parameter, pass in the method parameter (`newsMessage`).</span></span> <span data-ttu-id="9c3f2-138">Det här blir _brödtexten_ i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-138">This will be the _body_ of the message.</span></span> <span data-ttu-id="9c3f2-139">Det finns också en statisk metod som kan skapa binära meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-139">There is also a static method named  that can create a binary message payload.</span></span>
    

1. <span data-ttu-id="9c3f2-140">Anropa `AddMessageAsync` på `CloudQueue`-objektet för att lägga till meddelandet i kön.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-140">Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue.</span></span> <span data-ttu-id="9c3f2-141">Det här är också en asynkron metod, så du måste använda nyckelordet `await`.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-141">This is also an asynchronous method and you will need to use the `await` keyword to ensure we properly interact with it.</span></span>

1. <span data-ttu-id="9c3f2-142">Spara filen och kompilera den genom att skriva `dotnet build` i kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-142">Save the file and build it by typing `dotnet build` into the command window.</span></span> <span data-ttu-id="9c3f2-143">Om du åtgärda eventuella fel kan du använda följande kod till att kontrollera ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-143">Fix any errors, you can use the following code to check your work.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference(QueueName);
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a><span data-ttu-id="9c3f2-144">Lägg till kod för att skicka ett meddelande</span><span class="sxs-lookup"><span data-stu-id="9c3f2-144">Add code to send a message</span></span>

<span data-ttu-id="9c3f2-145">Nu ska vi ändra metoden `Main` så att alla mottagna parametrar skickas till vår nya metod, så att vi kan testa den nya sändmetoden.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-145">Let's modify the `Main` method to pass any parameters received into our new method so we can test our new send method.</span></span>

1. <span data-ttu-id="9c3f2-146">Leta upp metoden `Main`.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-146">Locate the `Main` method.</span></span>

1. <span data-ttu-id="9c3f2-147">Kontrollera den överförda `args`-parametern och se om några data skickades till kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-147">Check the passed `args` parameter to see if any data was passed to the command line.</span></span> <span data-ttu-id="9c3f2-148">Använd i så fall `String.Join` och skapa en enda sträng av alla ord, med ett blanksteg som avgränsare.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-148">If so, use `String.Join` to create a single string from all the words using a space as the separator.</span></span>

1. <span data-ttu-id="9c3f2-149">Skicka strängen till den nya `SendArticleAsync`-metoden.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-149">Pass that to the new `SendArticleAsync` method.</span></span> 

1. <span data-ttu-id="9c3f2-150">När den returnerar ett värde skriver ut meddelandet som skickades med `Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-150">Once it returns, use `Console.WriteLine` to output the message we sent.</span></span>

1. <span data-ttu-id="9c3f2-151">Spara filen och kompilera programmet (`dotnet build`). Du kan använda koden nedan för att kontrollera ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-151">Save the file and build the program (`dotnet build`), you can use the code below to check your work.</span></span>

> [!NOTE]
> <span data-ttu-id="9c3f2-152">Eftersom vår metod tekniskt sett är asynkron skulle vi normalt vilja använda nyckelordet `await`, men den här funktionen är inte tillgänglig för metoden `Main` såvida du inte använder C# 7.2 (det är en ny funktion).</span><span class="sxs-lookup"><span data-stu-id="9c3f2-152">Since our method is technically asynchronous, we normally would want to use the `await` keyword, however that feature isn't available on your `Main` method unless you are using C# 7.2 (it's a new feature).</span></span> <span data-ttu-id="9c3f2-153">Om du vill använda funktionen i tidigare versioner av språket kan du anropa `Wait()` för metoden så att du väntar tills ett värde returneras.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-153">To ensure we can use this on older versions of the language, just call `Wait()` on the method to actually block waiting for the method to return.</span></span>

    ```csharp
    static void Main(string[] args)
    {
        if (args.Length > 0)
        {
            string value = String.Join(" ", args);
            SendArticleAsync(value).Wait();
            Console.WriteLine($"Sent: {value}");
        }
    }
    ```

## <a name="execute-the-application"></a><span data-ttu-id="9c3f2-154">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="9c3f2-154">Execute the application</span></span>

<span data-ttu-id="9c3f2-155">Programmet kan nu skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-155">The application can now send messages.</span></span> <span data-ttu-id="9c3f2-156">Du kan testa programmet genom att köra det från kommandoraden med kommandot `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-156">To test it, you can run it from the command line with the `dotnet run` command.</span></span> <span data-ttu-id="9c3f2-157">Eventuella ytterligare strängar skickas som parametrar till programmet.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-157">Any additional strings are passed as parameters to the application.</span></span> <span data-ttu-id="9c3f2-158">Du kan till exempel skriva så här:</span><span class="sxs-lookup"><span data-stu-id="9c3f2-158">As an example, you can type:</span></span>

    ```azurecli
    dotnet run Send this message
    ```

> [!WARNING]
> <span data-ttu-id="9c3f2-159">Se till att du har sparat alla filer i onlineredigeraren innan du kompilerar och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-159">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="9c3f2-160">Nu bör strängen `"Send this message"` läggas till i kön.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-160">This should add the string `"Send this message"` into the queue.</span></span>

## <a name="check-your-results"></a><span data-ttu-id="9c3f2-161">Kontrollera dina resultat</span><span class="sxs-lookup"><span data-stu-id="9c3f2-161">Check your results</span></span>

<span data-ttu-id="9c3f2-162">Du kan kontrollera köer i Azure Portal med hjälp av **Lagringsutforskaren**. Om du öppnar den här produkten kan du se data i alla lagringskonton du äger.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-162">You can check queues in the Azure portal using the **Storage Explorer**, if you open that product it will let you explore the data in each storage account you own.</span></span>

<span data-ttu-id="9c3f2-163">Du kan också använda Azure CLI eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-163">Alternatively, you can use the Azure CLI or PowerShell.</span></span> <span data-ttu-id="9c3f2-164">Testa det här kommandot i gränssnittet och ersätt värdet `<connection-string>` med den faktiska anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="9c3f2-164">Try this command in the shell, replacing the `<connection-string>` value with your specific connection string:</span></span>

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="9c3f2-165">Du bör då se informationen i ditt meddelande, ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="9c3f2-165">This should dump the information for your message, which will look something like this:</span></span>

```json
[
  {
    "content": "U2VuZCB0aGlzIG1lc3NhZ2U=",
    "dequeueCount": 0,
    "expirationTime": "2018-09-05T02:43:40+00:00",
    "id": "b47cbe9f-a246-4a81-86ae-fa02ea8d98bc",
    "insertionTime": "2018-09-24T02:43:40+00:00",
    "popReceipt": null,
    "timeNextVisible": null
  }
]
```

<span data-ttu-id="9c3f2-166">Du kan prova flera andra kommandon i verktygen, skriv både `az storage queue --help` och `az storage message --help` för att utforska dem.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-166">There are several other commands available that you can try with the tools - check out both `az storage queue --help` and `az storage message --help` to explore them.</span></span>

> [!TIP]
> <span data-ttu-id="9c3f2-167">Placera anslutningssträngen i miljövariabeln `AZURE_STORAGE_CONNECTION_STRING` så att du inte behöver skriva parametern `--connection-string` varje gång!</span><span class="sxs-lookup"><span data-stu-id="9c3f2-167">Put your connection string value into an environment variable named `AZURE_STORAGE_CONNECTION_STRING` to save yourself from having to type the `--connection-string` parameter every time!</span></span>

<span data-ttu-id="9c3f2-168">Nu när vi har skickat ett meddelande är det sista steget att lägga till stöd för att _ta emot_ meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9c3f2-168">Now that we have sent a message, the last step is to add support to _receive_ the message.</span></span>