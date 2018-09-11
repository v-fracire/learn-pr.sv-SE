<span data-ttu-id="f2324-101">Distribuerade program använder köer, till exempel Service Bus-köer, som tillfälliga lagringsplatser för meddelanden som väntar på leverans till en målkomponent.</span><span class="sxs-lookup"><span data-stu-id="f2324-101">Distributed applications use queues, such as Service Bus queues, as temporary storage locations for messages that are awaiting delivery to a destination component.</span></span> <span data-ttu-id="f2324-102">För att skicka och ta emot meddelanden via en kö måste du skriva kod i käll- och målkomponenterna.</span><span class="sxs-lookup"><span data-stu-id="f2324-102">To send and receive messages through a queue, you must write code in the source and destination components.</span></span>

<span data-ttu-id="f2324-103">Ta Contoso Slices-programmet som exempel.</span><span class="sxs-lookup"><span data-stu-id="f2324-103">Consider the Contoso Slices application.</span></span> <span data-ttu-id="f2324-104">Användaren gör beställningen via en webbplats eller mobilapp.</span><span class="sxs-lookup"><span data-stu-id="f2324-104">The customer places the order through a website or mobile app.</span></span> <span data-ttu-id="f2324-105">Eftersom dessa körs på kundernas enheter finns det ingen egentlig gräns för hur många beställningar som kan komma in på en gång.</span><span class="sxs-lookup"><span data-stu-id="f2324-105">Since those run on customer devices, there is really no limit to how many orders could come in at once.</span></span> <span data-ttu-id="f2324-106">Genom att bestämma att mobilappen och webbplatsen placerar beställningarna i en kö kan vi låta serverkomponenten (en webbapp) bearbeta beställningarna från kön i sin egen takt.</span><span class="sxs-lookup"><span data-stu-id="f2324-106">By having the mobile app and website deposit the orders in a queue, we can allow the back-end component (a web app) to process orders from that queue at its own pace.</span></span>

<span data-ttu-id="f2324-107">Contoso Slices-programmet har faktiskt flera steg för att hantera en ny beställning.</span><span class="sxs-lookup"><span data-stu-id="f2324-107">The Contoso Slices application actually has several steps to handle a new order.</span></span> <span data-ttu-id="f2324-108">Men alla steg är beroende av att betalningen först godkänns, så vi väljer att använda en kö.</span><span class="sxs-lookup"><span data-stu-id="f2324-108">But all of them are dependent on first authorizing payment, so we decide to use a queue.</span></span> <span data-ttu-id="f2324-109">Den mottagande komponentens första jobb blir att bearbeta betalningen.</span><span class="sxs-lookup"><span data-stu-id="f2324-109">Our receiving component's first job will be processing the payment.</span></span>

<span data-ttu-id="f2324-110">Contoso måste i både mobilappen och webbplatsen skriva kod som lägger till ett meddelande i kön.</span><span class="sxs-lookup"><span data-stu-id="f2324-110">In the mobile app and website, Contoso needs to write code that adds a message to the queue.</span></span> <span data-ttu-id="f2324-111">I serverdelswebbappen skriver de kod som plockar upp meddelanden från kön.</span><span class="sxs-lookup"><span data-stu-id="f2324-111">In the back-end web app, they'll write code that picks messages up from the the queue.</span></span>

<span data-ttu-id="f2324-112">Här lär du dig skriva den koden.</span><span class="sxs-lookup"><span data-stu-id="f2324-112">Here, you will learn how to write that code.</span></span>

## <a name="the-microsoftazureservicebus-nuget-package"></a><span data-ttu-id="f2324-113">NuGet-paketet Microsoft.Azure.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="f2324-113">The Microsoft.Azure.ServiceBus NuGet package</span></span>

<span data-ttu-id="f2324-114">Microsoft har ett bibliotek med .NET-klasser som gör det enkelt att skriva kod som skickar och tar emot meddelanden via Service Bus. Du kan använda biblioteket i alla .NET Framework-språk för att interagera med en Service Bus-kö, ett Service Bus-ämne eller Service Bus-relä.</span><span class="sxs-lookup"><span data-stu-id="f2324-114">To make it easy to write code that sends and receives messages through Service Bus, Microsoft provides a library of .NET classes, which you can use in any .NET Framework language to interact with a Service Bus queue, topic, or relay.</span></span> <span data-ttu-id="f2324-115">Du kan inkludera det här biblioteket i ditt program genom att lägga till NuGet-paketet **Microsoft.Azure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="f2324-115">You can include this library in your application by adding the **Microsoft.Azure.ServiceBus** NuGet package.</span></span>

<span data-ttu-id="f2324-116">Den viktigaste klassen för köer i biblioteket är klassen `QueueClient`.</span><span class="sxs-lookup"><span data-stu-id="f2324-116">The most important class in this library for queues is the `QueueClient` class.</span></span> <span data-ttu-id="f2324-117">Du måste börja med att skapa en instans av den här klassen i både skickande och mottagande komponenter.</span><span class="sxs-lookup"><span data-stu-id="f2324-117">You must start by instantiating this class both in sending and receiving components.</span></span>

## <a name="connection-strings-and-keys"></a><span data-ttu-id="f2324-118">Anslutningssträngar och nycklar</span><span class="sxs-lookup"><span data-stu-id="f2324-118">Connection Strings and Keys</span></span>

<span data-ttu-id="f2324-119">Både källkomponenter och målkomponenter behöver två typer av information för att ansluta till en kö i en Service Bus-namnrymd:</span><span class="sxs-lookup"><span data-stu-id="f2324-119">Both source components and destination components need two pieces of information to connect to a queue in a Service Bus namespace:</span></span>

- <span data-ttu-id="f2324-120">Platsen för Service Bus-namnrymden.</span><span class="sxs-lookup"><span data-stu-id="f2324-120">The location of the Service Bus namespace.</span></span> <span data-ttu-id="f2324-121">Kallas även för **slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="f2324-121">Also known as an **endpoint**.</span></span> <span data-ttu-id="f2324-122">Platsen anges som ett fullständigt domännamn inom domänen **servicebus.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="f2324-122">The location is specified as a fully-qualified domain name within the **servicebus.windows.net** domain.</span></span> <span data-ttu-id="f2324-123">Till exempel: **pizzaService.servicebus.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="f2324-123">For example: **pizzaService.servicebus.windows.net**.</span></span>
- <span data-ttu-id="f2324-124">En åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="f2324-124">An access key.</span></span> <span data-ttu-id="f2324-125">Service Bus begränsar åtkomsten till köer, ämnen och reläer genom att kräva en åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="f2324-125">Service Bus restricts access to queues, topics, and relays by requiring an access key.</span></span>

<span data-ttu-id="f2324-126">De här två typerna av information anges för `QueueClient`-objektet i form av en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="f2324-126">Both of these pieces of information are provided to the `QueueClient` object in the form of a connection string.</span></span> <span data-ttu-id="f2324-127">Du kan hämta den rätta anslutningssträngen för namnrymden från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2324-127">You can obtain the correct connection string for your namespace from the Azure portal.</span></span>

## <a name="calling-methods-asynchronously"></a><span data-ttu-id="f2324-128">Anropa metoder asynkront</span><span class="sxs-lookup"><span data-stu-id="f2324-128">Calling methods asynchronously</span></span>

<span data-ttu-id="f2324-129">Kön i Azure kan finnas hundratals mil från skickande och mottagande komponenter.</span><span class="sxs-lookup"><span data-stu-id="f2324-129">The queue in Azure may be located thousands of miles away from sending and receiving components.</span></span> <span data-ttu-id="f2324-130">Även om den är fysiskt nära kan långsamma anslutningar och konkurrens om bandbredden orsaka fördröjningar när en komponent anropar en metod i kön.</span><span class="sxs-lookup"><span data-stu-id="f2324-130">Even if it is physically close, slow connections and bandwidth contention may cause delays when a component calls a method on the queue.</span></span> <span data-ttu-id="f2324-131">Därför tillhandahåller ServiceBus-klientbiblioteket `async`-metoder för att interagera med köerna.</span><span class="sxs-lookup"><span data-stu-id="f2324-131">For this reason, the serviceBus client library makes `async` methods available for interacting with the queues.</span></span> <span data-ttu-id="f2324-132">Genom att använda de här metoderna kan vi undvika att blockera en tråd medan vi väntar på att anrop ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="f2324-132">We'll use these methods to avoid blocking a thread while waiting for calls to complete.</span></span>

<span data-ttu-id="f2324-133">Använd till exempel metoden `QueueClient.SendAsync()` med nyckelordet `await` när ett meddelande ska skickas till en kö.</span><span class="sxs-lookup"><span data-stu-id="f2324-133">When sending a message to a queue, for example, use the `QueueClient.SendAsync()` method with the `await` keyword.</span></span>

## <a name="write-code-that-sends-to-queues"></a><span data-ttu-id="f2324-134">Skriva kod som skickar till köer</span><span class="sxs-lookup"><span data-stu-id="f2324-134">Write code that sends to queues</span></span> 

<span data-ttu-id="f2324-135">I alla skickande och mottagande komponenter bör du lägga till följande using-uttryck i alla kodfiler som anropar en Service Bus-kö:</span><span class="sxs-lookup"><span data-stu-id="f2324-135">In any sending or receiving component, you should add the following using statements to any code file that calls a Service Bus queue:</span></span>

    ```C#
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

<span data-ttu-id="f2324-136">Skapa sedan ett nytt `QueueClient`-objekt, och skicka anslutningssträngen och köns namn till objektet:</span><span class="sxs-lookup"><span data-stu-id="f2324-136">Next, create a new `QueueClient` object and pass it the connection string and the name of the queue:</span></span>

    ```C#
    queueClient = new QueueClient(TextAppConnectionString, "PrivateMessageQueue");
    ```

<span data-ttu-id="f2324-137">Du kan skicka ett meddelande till kön genom att anropa metoden `QueueClient.SendAsync()` och skicka meddelandet i form av en UTF8-kodad sträng:</span><span class="sxs-lookup"><span data-stu-id="f2324-137">You can send a message to the queue, by calling the `QueueClient.SendAsync()` method and passing the message in the form of a UTF8 encoded string:</span></span>

    ```C#
    string message = "Sure would like a large pepperoni!";
    var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
    await queueClient.SendAsync(encodedMessage);
    ```

## <a name="receive-messages-from-queue"></a><span data-ttu-id="f2324-138">Ta emot meddelanden från kön</span><span class="sxs-lookup"><span data-stu-id="f2324-138">Receive messages from queue</span></span>

<span data-ttu-id="f2324-139">För att ta emot meddelanden måste du först registrera en meddelandehanterare. Det här är metoden i din kod som anropas när ett meddelande finns tillgängligt i kön.</span><span class="sxs-lookup"><span data-stu-id="f2324-139">To receive messages, you must first register a message handler - this is the method in your code that will be invoked when a message is available on the queue.</span></span>

    ```C#
    queueClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
    ```

<span data-ttu-id="f2324-140">Utför bearbetningsarbetet.</span><span class="sxs-lookup"><span data-stu-id="f2324-140">Do your processing work.</span></span> <span data-ttu-id="f2324-141">Inom meddelandehanteraren anropar du sedan metoden `QueueClient.CompleteAsync()`, så tas meddelandet bort från kön:</span><span class="sxs-lookup"><span data-stu-id="f2324-141">Then within the message handler, call the `QueueClient.CompleteAsync()` method to remove the message from the queue:</span></span>

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```
    