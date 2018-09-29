<span data-ttu-id="2bdd2-101">Nu när alla krav är uppfyllda kan du skriva kod som skapar en ny lagringskö och lägger till ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-101">Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message.</span></span> <span data-ttu-id="2bdd2-102">Normalt skulle vi placera den här koden i apparna på klientsidan där data genereras.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-102">We would typically place this code in our front-end apps that generate the data.</span></span>

## <a name="add-the-client-library-for-azure-storage"></a><span data-ttu-id="2bdd2-103">Lägg till klientbiblioteket för Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2bdd2-103">Add the client library for Azure Storage</span></span>

<span data-ttu-id="2bdd2-104">Vi börjar med att lägga till **Azure Storage-klientbiblioteket för .NET** i appen.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-104">Let's start by adding the **Azure Storage Client Library for .NET** to our app.</span></span> <span data-ttu-id="2bdd2-105">Du kan installera det med NuGet (en .NET-pakethanterare).</span><span class="sxs-lookup"><span data-stu-id="2bdd2-105">It can be installed with NuGet (a .NET package manager).</span></span> 

1. <span data-ttu-id="2bdd2-106">Installera paketet `WindowsAzure.Storage` i projektet med kommandot `dotnet add package`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-106">Install the `WindowsAzure.Storage` package to the project with the `dotnet add package` command.</span></span> <span data-ttu-id="2bdd2-107">Du kan göra detta i terminalfönstret _nedan_, Cloud Shell eller i samma mapp som projektet om du arbetar med din lokala dator i en terminal/konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-107">You can do this in the terminal window _below_ the Cloud Shell, or if you are working on your local computer, in a terminal/console window in the same folder as the project.</span></span>

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a><span data-ttu-id="2bdd2-108">Lägg till en metod för att skicka en nyhetsavisering</span><span class="sxs-lookup"><span data-stu-id="2bdd2-108">Add a method to send a news alert</span></span>

<span data-ttu-id="2bdd2-109">Nu ska vi skapa en ny metod för att skicka en nyhet till en kö.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-109">Next, let's create a new method to send a news story into a queue.</span></span>

1. <span data-ttu-id="2bdd2-110">Öppna filen `Program.cs` i ditt kodredigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-110">Open the `Program.cs` file in your code editor.</span></span>

1. <span data-ttu-id="2bdd2-111">Lägg till följande namnområden överst i filen.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-111">At the top of the file, add the following namespaces.</span></span> <span data-ttu-id="2bdd2-112">Vi kommer att använda typer från båda när vi ansluter till Azure Storage och sedan arbetar med köer.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-112">We'll be using types from both of these to connect to Azure Storage and then to work with queues.</span></span>

    - `System.Threading.Tasks`
    - `Microsoft.WindowsAzure.Storage`
    - `Microsoft.WindowsAzure.Storage.Queue`

1. <span data-ttu-id="2bdd2-113">Skapa en statisk metod i klassen `Program` med namnet `SendArticleAsync` som tar emot ett argument av typen `string` och returnerar ett `Task`-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-113">Create a static method in the `Program` class named `SendArticleAsync` that takes a `string` and returns a `Task`.</span></span> <span data-ttu-id="2bdd2-114">Vi använder den här metoden till att skicka en nyhetsartikel till vår tjänst.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-114">We'll use this method to send a news article in to our service.</span></span> <span data-ttu-id="2bdd2-115">Ge indataparametern namnet `newsMessage` enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-115">Name the input parameter `newsMessage` as shown below.</span></span>

    ```csharp
    ...
    using System.Threading.Tasks;
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
    
1. <span data-ttu-id="2bdd2-116">Använd den statiska metoden `CloudStorageAccount.Parse` i den nya metoden till att parsa anslutningssträngen (kom ihåg att vi lade den i en konstantsträng).</span><span class="sxs-lookup"><span data-stu-id="2bdd2-116">In your new method, use the static `CloudStorageAccount.Parse` method to parse your connection string (recall we placed it into a constant string).</span></span> <span data-ttu-id="2bdd2-117">Den här metoden returnerar ett `CloudStorageAccount`-objekt som måste lagras i en variabel.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-117">This method returns a `CloudStorageAccount` object that needs to be stored in a variable.</span></span>

1. <span data-ttu-id="2bdd2-118">Anropa metoden `CreateCloudQueueClient()` i lagringskontoobjektet för att få ett klientobjekt och lagra det i en variabel.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-118">Call the `CreateCloudQueueClient()` method on the storage account object to get a client object and store that in a variable.</span></span>

1. <span data-ttu-id="2bdd2-119">Anropa sedan metoden `GetQueueReference` i klientobjektet och skicka strängen ”newsqueue” som könamn.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-119">Next, call `GetQueueReference` method on the client object and pass the string "newsqueue" for the queue name.</span></span> <span data-ttu-id="2bdd2-120">Då returneras ett `CloudQueue`-objekt som vi kan använda för att arbeta med kön.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-120">This returns a `CloudQueue` object that we can use to work with the queue.</span></span> <span data-ttu-id="2bdd2-121">Kön behöver inte finnas ännu.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-121">It's OK if the queue does not exist yet.</span></span>

1. <span data-ttu-id="2bdd2-122">Anropa `CreateIfNotExistsAsync()` på `CloudQueue`-objektet för att se till att kön är redo att användas.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-122">Call `CreateIfNotExistsAsync()` on the `CloudQueue` object to ensure the queue is ready for use.</span></span> <span data-ttu-id="2bdd2-123">Då skapas kön om det behövs.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-123">This will create the queue if necessary.</span></span>
    - <span data-ttu-id="2bdd2-124">Eftersom det här är en asynkron metod använder du C#-nyckelordet `await` så att vi använder den korrekt.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-124">Since this is an asynchronous method, use the C# `await` keyword to ensure we work properly with it.</span></span> <span data-ttu-id="2bdd2-125">Det innebär också att du måste utöka _metoden_ med nyckelordet `async`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-125">That also means you need to decorate the _method_ with the `async` keyword.</span></span> 
    - <span data-ttu-id="2bdd2-126">`CreateIfNotExistsAsync` returnerar ett `bool`-värde som är `true` om kön har skapats och `false` om den redan fanns.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-126">`CreateIfNotExistsAsync` returns a `bool` value that will be `true` if the queue was created and `false` if it already exists.</span></span> <span data-ttu-id="2bdd2-127">Skicka ett meddelande till konsolen om vi skapade kön.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-127">Output a message to the console if we created the queue.</span></span>
    - <span data-ttu-id="2bdd2-128">Här är ett exempel om du behöver hjälp.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-128">Here's an example if you need some help.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        // Not Shown here - code from prior steps
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. <span data-ttu-id="2bdd2-129">Skapa en instans av ett `CloudQueueMessage`-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-129">Create an instance of a `CloudQueueMessage`.</span></span> 
    - <span data-ttu-id="2bdd2-130">Det tar ett `string`-värde som parameter, skicka metodparametern (`newsMessage`).</span><span class="sxs-lookup"><span data-stu-id="2bdd2-130">It takes a `string` parameter, pass in the method parameter (`newsMessage`).</span></span> <span data-ttu-id="2bdd2-131">Det här blir _brödtexten_ i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-131">This will be the _body_ of the message.</span></span> <span data-ttu-id="2bdd2-132">Det finns också en statisk metod som kan skapa binära meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-132">There is also a static method named  that can create a binary message payload.</span></span>

1. <span data-ttu-id="2bdd2-133">Anropa `AddMessageAsync` på `CloudQueue`-objektet för att lägga till meddelandet i kön.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-133">Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue.</span></span> <span data-ttu-id="2bdd2-134">Det här är också en asynkron metod, så du måste använda nyckelordet `await`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-134">This is also an asynchronous method and you will need to use the `await` keyword to ensure we properly interact with it.</span></span>

1. <span data-ttu-id="2bdd2-135">Spara filen och kompilera den genom att skriva `dotnet build` i kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-135">Save the file and build it by typing `dotnet build` into the command window.</span></span> <span data-ttu-id="2bdd2-136">Om du åtgärda eventuella fel kan du använda följande kod till att kontrollera ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-136">Fix any errors, you can use the following code to check your work.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference("newsqueue");
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a><span data-ttu-id="2bdd2-137">Lägg till kod för att skicka ett meddelande</span><span class="sxs-lookup"><span data-stu-id="2bdd2-137">Add code to send a message</span></span>

<span data-ttu-id="2bdd2-138">Nu ska vi ändra metoden `Main` så att alla mottagna parametrar skickas till vår nya metod, så att vi kan testa den nya sändmetoden.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-138">Let's modify the `Main` method to pass any parameters received into our new method so we can test our new send method.</span></span>

1. <span data-ttu-id="2bdd2-139">Leta upp metoden `Main`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-139">Locate the `Main` method.</span></span>

1. <span data-ttu-id="2bdd2-140">Kontrollera den överförda `args`-parametern och se om några data skickades till kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-140">Check the passed `args` parameter to see if any data was passed to the command line.</span></span> <span data-ttu-id="2bdd2-141">Använd i så fall `String.Join` och skapa en enda sträng av alla ord, med ett blanksteg som avgränsare.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-141">If so, use `String.Join` to create a single string from all the words using a space as the separator.</span></span>

1. <span data-ttu-id="2bdd2-142">Skicka strängen till den nya `SendArticleAsync`-metoden.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-142">Pass that to the new `SendArticleAsync` method.</span></span> 

1. <span data-ttu-id="2bdd2-143">När den returnerar ett värde skriver ut meddelandet som skickades med `Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-143">Once it returns, use `Console.WriteLine` to output the message we sent.</span></span>

1. <span data-ttu-id="2bdd2-144">Spara filen och kompilera programmet (`dotnet build`). Du kan använda koden nedan för att kontrollera ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-144">Save the file and build the program (`dotnet build`), you can use the code below to check your work.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2bdd2-145">Eftersom vår metod tekniskt sett är asynkron skulle vi normalt vilja använda nyckelordet `await`, men den här funktionen är inte tillgänglig för metoden `Main` såvida du inte använder C# 7.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-145">Since our method is technically asynchronous, we will want to use the `await` keyword, however that feature isn't available on your `Main` method unless you are using C# 7.1 or later.</span></span> <span data-ttu-id="2bdd2-146">Det är bara att anropa `Wait()` i metoden så blockeras väntetiden för att metoden ska återvända. Vi kan fixa detta på ett ögonblick.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-146">Just call `Wait()` on the method to actually block waiting for the method to return, we'll fix that in a minute.</span></span>

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

## <a name="execute-the-application"></a><span data-ttu-id="2bdd2-147">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="2bdd2-147">Execute the application</span></span>

<span data-ttu-id="2bdd2-148">Programmet kan nu skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-148">The application can now send messages.</span></span> <span data-ttu-id="2bdd2-149">Du kan testa programmet genom att köra det från kommandoraden med kommandot `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-149">To test it, you can run it from the command line with the `dotnet run` command.</span></span> <span data-ttu-id="2bdd2-150">Eventuella ytterligare strängar skickas som parametrar till programmet.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-150">Any additional strings are passed as parameters to the application.</span></span> 

> [!WARNING]
> <span data-ttu-id="2bdd2-151">Se till att du har sparat alla filer i onlineredigeraren innan du kompilerar och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-151">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="2bdd2-152">Du kan till exempel skriva så här:</span><span class="sxs-lookup"><span data-stu-id="2bdd2-152">As an example, you can type:</span></span>

```azurecli
dotnet run Send this message
```

<span data-ttu-id="2bdd2-153">Nu bör strängen `"Send this message"` läggas till i kön.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-153">This should add the string `"Send this message"` into the queue.</span></span>

## <a name="check-your-results"></a><span data-ttu-id="2bdd2-154">Kontrollera dina resultat</span><span class="sxs-lookup"><span data-stu-id="2bdd2-154">Check your results</span></span>

<span data-ttu-id="2bdd2-155">Du kan kontrollera köer i Azure Portal med hjälp av **Lagringsutforskaren**. Om du öppnar den här produkten kan du se data i alla lagringskonton du äger.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-155">You can check queues in the Azure portal using the **Storage Explorer**, if you open that product it will let you explore the data in each storage account you own.</span></span>

<span data-ttu-id="2bdd2-156">Du kan också använda Azure CLI eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-156">Alternatively, you can use the Azure CLI or PowerShell.</span></span> <span data-ttu-id="2bdd2-157">Testa det här kommandot i gränssnittet och ersätt värdet `<connection-string>` med den faktiska anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="2bdd2-157">Try this command in the shell, replacing the `<connection-string>` value with your specific connection string:</span></span>

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="2bdd2-158">Du bör då se informationen i ditt meddelande, ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="2bdd2-158">This should dump the information for your message, which will look something like this:</span></span>

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

<span data-ttu-id="2bdd2-159">Du kan prova flera andra kommandon i verktygen, skriv både `az storage queue --help` och `az storage message --help` för att utforska dem.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-159">There are several other commands available that you can try with the tools - check out both `az storage queue --help` and `az storage message --help` to explore them.</span></span>

> [!TIP]
> <span data-ttu-id="2bdd2-160">Placera anslutningssträngen i miljövariabeln `AZURE_STORAGE_CONNECTION_STRING` så att du inte behöver skriva parametern `--connection-string` varje gång!</span><span class="sxs-lookup"><span data-stu-id="2bdd2-160">Put your connection string value into an environment variable named `AZURE_STORAGE_CONNECTION_STRING` to save yourself from having to type the `--connection-string` parameter every time!</span></span>

## <a name="optional---use-the-async-versions-of-the-methods"></a><span data-ttu-id="2bdd2-161">Valfritt – Använda asynkrona versioner av metoderna</span><span class="sxs-lookup"><span data-stu-id="2bdd2-161">Optional - Use the async versions of the methods</span></span>

<span data-ttu-id="2bdd2-162">Vi använde `Wait()` för sändmetoden ovan men detta är inte ett effektivt bruk av våra processorresurser.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-162">We used `Wait()` on the send method above but that's not an efficient use of our computing resources.</span></span> <span data-ttu-id="2bdd2-163">I stället ska vi använda C# `async` och metoderna `await`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-163">Instead, we want to use the C# `async` and `await` methods.</span></span> <span data-ttu-id="2bdd2-164">Men vi kommer att behöva använda _minst_ C# 7.1 för att dessa nyckelord ska kunna tillämpas i vår **Main**-metod.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-164">However, we will need to be using _at least_ C# 7.1 to be able to apply these keywords to our **Main** method.</span></span>

### <a name="switch-to-c-71"></a><span data-ttu-id="2bdd2-165">Växla till C# 7.1</span><span class="sxs-lookup"><span data-stu-id="2bdd2-165">Switch to C# 7.1</span></span>

<span data-ttu-id="2bdd2-166">C#-`async` och `await`-nyckelord var inte giltiga nyckelord i **Main**-metoderna före C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-166">C#'s `async` and `await` keywords were not valid keywords in **Main** methods until C# 7.1.</span></span> <span data-ttu-id="2bdd2-167">Vi kan enkelt växla till den kompilatorn via en flagga i **.csproj**-filen.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-167">We can easily switch to that compiler through a flag in the **.csproj** file.</span></span>

1. <span data-ttu-id="2bdd2-168">Öppna filen **QueueApp.csproj** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-168">Open the **QueueApp.csproj** file in the editor.</span></span>

1. <span data-ttu-id="2bdd2-169">Lägg till `<LangVersion>7.1</LangVersion>` i den första `PropertyGroup` i versionsfilen.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-169">Add `<LangVersion>7.1</LangVersion>` into the first `PropertyGroup` in the build file.</span></span> <span data-ttu-id="2bdd2-170">Den bör se ut enligt nedan när du är klar.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-170">It should look like the following when you are finished.</span></span>
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a><span data-ttu-id="2bdd2-171">Använda async-nyckelordet</span><span class="sxs-lookup"><span data-stu-id="2bdd2-171">Apply the async keyword</span></span>

<span data-ttu-id="2bdd2-172">Tillämpa sedan `async`-nyckelordet på **Main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-172">Next, apply the `async` keyword to the **Main** method.</span></span> <span data-ttu-id="2bdd2-173">Vi måste göra tre saker.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-173">We will have to do three things.</span></span>

1. <span data-ttu-id="2bdd2-174">Lägga till `async`-nyckelordet i **Main**-metodens signatur.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-174">Add the `async` keyword onto the **Main** method signature.</span></span>
1. <span data-ttu-id="2bdd2-175">Ändra returtypen från `void` till `Task`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-175">Change the return type from `void` to `Task`.</span></span>
1. <span data-ttu-id="2bdd2-176">Ta bort anropet till `Wait()` för anropet till `SendArticleAsync` och ersätt det med `await`.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-176">Remove the call to `Wait()` on the call to `SendArticleAsync` and replace it with `await`.</span></span>

<span data-ttu-id="2bdd2-177">Kör appen igen – det bör fortfarande fungera precis som det innan, men nu kan koden frigöra tråden tillbaka till arbetstråden i .NET medan vi väntar på att meddelandet ska placeras i kö.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-177">Try running the app again - it should still work exactly as before, but now the code is able to release the thread back to the .NET threadpool while we wait for the message to be queued.</span></span>

<span data-ttu-id="2bdd2-178">Nu när vi har skickat ett meddelande är det sista steget att lägga till stöd för att _ta emot_ meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2bdd2-178">Now that we have sent a message, the last step is to add support to _receive_ the message.</span></span>