<span data-ttu-id="34af5-101">I ett distribuerat program måste meddelanden skickas till en enda mottagarkomponent.</span><span class="sxs-lookup"><span data-stu-id="34af5-101">In a distributed application, some messages need to be sent to a single recipient component.</span></span> <span data-ttu-id="34af5-102">Andra meddelanden måste nå fler än ett mål.</span><span class="sxs-lookup"><span data-stu-id="34af5-102">Other messages need to reach more than one destination.</span></span>

<span data-ttu-id="34af5-103">Nu ska vi diskutera vad som händer när en användare avbokar en pizzabeställning.</span><span class="sxs-lookup"><span data-stu-id="34af5-103">Let's discuss what happens when a user cancels a pizza order.</span></span> <span data-ttu-id="34af5-104">Det är lite annorlunda än att göra den inledande beställningen.</span><span class="sxs-lookup"><span data-stu-id="34af5-104">This is a little different than placing the initial order.</span></span> <span data-ttu-id="34af5-105">I det fallet ville vi vänta tills betalningsbearbetningen har slutförts för beställningen innan den skickas vidare till andra steg (som att förbereda och laga den i restaurangen).</span><span class="sxs-lookup"><span data-stu-id="34af5-105">In that case, we wanted to wait until the order cleared payment processing before sending the order on to other steps (like having it prepared and cooked at the local storefront).</span></span> <span data-ttu-id="34af5-106">Men för avbokningen meddelar vi restaurangen *och* betalningshanteraren samtidigt.</span><span class="sxs-lookup"><span data-stu-id="34af5-106">But for the cancel operation, we are going to notify both the storefront *and* the payment processor at the same time.</span></span> <span data-ttu-id="34af5-107">På så sätt minimeras risken att ingredienser går till spillo eller att leveranstiden förlängs.</span><span class="sxs-lookup"><span data-stu-id="34af5-107">This approach minimizes the chances that we waste ingredients or delivery driver time.</span></span>

<span data-ttu-id="34af5-108">För att flera komponenter ska få samma meddelande använder vi ett Azure Service Bus-ämne.</span><span class="sxs-lookup"><span data-stu-id="34af5-108">To allow multiple components to receive the same message, we'll use an Azure Service Bus topic.</span></span>

## <a name="how-code-that-uses-topics-differs-from-queues"></a><span data-ttu-id="34af5-109">Hur kod som använder ämnen skiljer sig från köer</span><span class="sxs-lookup"><span data-stu-id="34af5-109">How code that uses topics differs from queues</span></span>

<span data-ttu-id="34af5-110">Om du vill att alla meddelanden som skickats till ämnet ska levereras till alla prenumererande komponenter använder du ämnen.</span><span class="sxs-lookup"><span data-stu-id="34af5-110">If you want every message sent to the topic to be delivered to all subscribing components, you'll use topics.</span></span> <span data-ttu-id="34af5-111">Att skriva kod som använder ämnen är ett sätt att ersätta köer.</span><span class="sxs-lookup"><span data-stu-id="34af5-111">Writing code that uses topics is a way to replace queues.</span></span> <span data-ttu-id="34af5-112">Du använder samma **Microsoft.Azure.ServiceBus** NuGet-paket konfigurerar du anslutningssträngar och använder asynkrona programmeringsmönster.</span><span class="sxs-lookup"><span data-stu-id="34af5-112">You use the same **Microsoft.Azure.ServiceBus** NuGet package, configure connection strings, and use asynchronous programming patterns.</span></span>

<span data-ttu-id="34af5-113">Men du använder klassen `TopicClient` i stället för klassen `QueueClient` till att skicka meddelanden och klassen `SubscriptionClient` för att ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="34af5-113">However, you use the `TopicClient` class instead of the `QueueClient` class to send messages and the `SubscriptionClient` class to receive messages.</span></span>

## <a name="setting-filters-on-subscriptions"></a><span data-ttu-id="34af5-114">Ange filter för prenumerationer</span><span class="sxs-lookup"><span data-stu-id="34af5-114">Setting filters on subscriptions</span></span>

<span data-ttu-id="34af5-115">Om du vill kontrollera vilka meddelanden som skickats till ämnet levereras till vilka prenumerationer kan du placera filter på varje prenumeration i ämnet.</span><span class="sxs-lookup"><span data-stu-id="34af5-115">If you want to control which messages sent to the topic are delivered to which subscriptions, you can place filters on each subscription in the topic.</span></span> <span data-ttu-id="34af5-116">I pizzaprogrammet, till exempel, kör våra restauranger UWP-program (Universal Windows Platform).</span><span class="sxs-lookup"><span data-stu-id="34af5-116">In the pizza application, for instance, our storefronts are running Universal Windows Platform (UWP) applications.</span></span> <span data-ttu-id="34af5-117">Varje restaurang kan prenumerera på ämnet ”OrderCancellation” (Avboka beställning) men filtrera efter sitt eget StoreId.</span><span class="sxs-lookup"><span data-stu-id="34af5-117">Each store can subscribe to the "OrderCancellation" topic but filter for its own StoreId.</span></span> <span data-ttu-id="34af5-118">Vi sparar internetbandbredd eftersom vi inte skickar onödiga meddelanden till avlägsna försäljningsställen.</span><span class="sxs-lookup"><span data-stu-id="34af5-118">We save internet bandwidth because we are not sending unnecessary messages to distant store locations.</span></span> <span data-ttu-id="34af5-119">Samtidigt prenumererar komponenten för betalningshantering på alla våra avbokningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="34af5-119">Meanwhile, the payment processing component subscribes to all our cancellation messages.</span></span>

<span data-ttu-id="34af5-120">Filter kan vara någon av följande tre typer:</span><span class="sxs-lookup"><span data-stu-id="34af5-120">Filters can be one of three types:</span></span>

- <span data-ttu-id="34af5-121">**Booleska filter.**</span><span class="sxs-lookup"><span data-stu-id="34af5-121">**Boolean Filters.**</span></span> <span data-ttu-id="34af5-122">`TrueFilter` ser till att alla meddelanden som skickats till ämnen levereras till den aktuella prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="34af5-122">The `TrueFilter` ensures that all messages sent to the topic are delivered to the current subscription.</span></span> <span data-ttu-id="34af5-123">`FalseFilter` ser till att inga meddelanden skickas till den aktuella prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="34af5-123">The `FalseFilter` ensures that none of the messages are delivered to the current subscription.</span></span> <span data-ttu-id="34af5-124">(Detta blockerar eller stänger av prenumerationen.)</span><span class="sxs-lookup"><span data-stu-id="34af5-124">(This effectively blocks or switches off the subscription.)</span></span>
- <span data-ttu-id="34af5-125">**SQL-filter.**</span><span class="sxs-lookup"><span data-stu-id="34af5-125">**SQL Filters.**</span></span> <span data-ttu-id="34af5-126">Ett SQL-filter anger ett villkor genom att använda samma syntax som en `WHERE`-sats i en SQL-fråga.</span><span class="sxs-lookup"><span data-stu-id="34af5-126">A SQL filter specifies a condition by using the same syntax as a `WHERE` clause in a SQL query.</span></span> <span data-ttu-id="34af5-127">Bara meddelanden som returnerar `True` vid utvärdering mot prenumerationen levereras till prenumeranterna.</span><span class="sxs-lookup"><span data-stu-id="34af5-127">Only messages that return `True` when evaluated against this subscription will be delivered to the subscribers.</span></span>
- <span data-ttu-id="34af5-128">**Korrelationsfilter.**</span><span class="sxs-lookup"><span data-stu-id="34af5-128">**Correlation Filters.**</span></span> <span data-ttu-id="34af5-129">Ett korrelationsfilter innehåller en uppsättning villkor som matchas mot egenskaperna för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="34af5-129">A correlation filter holds a set of conditions that are matched against the properties of each message.</span></span> <span data-ttu-id="34af5-130">Om egenskapen i filtret och egenskaperna för meddelandet har samma värde betraktas den som en matchning.</span><span class="sxs-lookup"><span data-stu-id="34af5-130">If the property in the filter and the property on the message have the same value, it is considered a match.</span></span>

<span data-ttu-id="34af5-131">För vårt StoreId-filter *kan* vi använda ett SQL-filter.</span><span class="sxs-lookup"><span data-stu-id="34af5-131">For our StoreId filter, we *could* use a SQL filter.</span></span> <span data-ttu-id="34af5-132">De är de mest flexibla men också de dyraste vad gäller databearbetning, och de kan göra Service Bus-dataflödet långsammare.</span><span class="sxs-lookup"><span data-stu-id="34af5-132">Those filters are the most flexible, but they're also the most computationally expensive and could slow down our Service Bus throughput.</span></span> <span data-ttu-id="34af5-133">I det här fallet väljer vi i stället ett korrelationsfilter.</span><span class="sxs-lookup"><span data-stu-id="34af5-133">In this case, we choose a correlation filter instead.</span></span> 

## <a name="topicclient-example"></a><span data-ttu-id="34af5-134">TopicClient-exempel</span><span class="sxs-lookup"><span data-stu-id="34af5-134">TopicClient example</span></span>

<span data-ttu-id="34af5-135">I alla sändande och mottagande komponenter bör du lägga till följande `using`-uttryck i alla kodfiler som anropar en Service Bus-kö:</span><span class="sxs-lookup"><span data-stu-id="34af5-135">In any sending or receiving component, you should add the following `using` statements to any code file that calls a Service Bus topic:</span></span>

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

<span data-ttu-id="34af5-136">Om du vill skicka ett meddelande börjar du med att skapa ett `TopicClient`-objekt och skicka anslutningssträngen och namnet på ämnet till objektet:</span><span class="sxs-lookup"><span data-stu-id="34af5-136">To send a message, start by creating a new `TopicClient` object and pass it the connection string and the name of the topic:</span></span>

```C#
topicClient = new TopicClient(TextAppConnectionString, "GroupMessageTopic");
```

<span data-ttu-id="34af5-137">Du kan skicka ett meddelande till ämnet genom att anropa metoden `TopicClient.SendAsync()` och skicka meddelandet.</span><span class="sxs-lookup"><span data-stu-id="34af5-137">You can send a message to the topic by calling the `TopicClient.SendAsync()` method and passing the message.</span></span> <span data-ttu-id="34af5-138">Precis som för köer måste meddelandet vara en UTF8-kodad sträng:</span><span class="sxs-lookup"><span data-stu-id="34af5-138">As with queues, the message must be in the form of a UTF-8 encoded string:</span></span>

```C#
string message = "Cancel! I can't believe you use canned mushrooms!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await topicClient.SendAsync(encodedMessage);
```

<span data-ttu-id="34af5-139">Om du vill ta emot meddelanden måste du skapa ett `SubscriptionClient`-objekt, inte ett `TopicClient`-objekt, och skicka anslutningssträngen, namnet på ämnet **och** namnet på prenumerationen:</span><span class="sxs-lookup"><span data-stu-id="34af5-139">To receive messages, you must create a `SubscriptionClient` object, not a `TopicClient` object, and pass it the connection string, the name of the topic, **and** the name of the subscription:</span></span>

```C#
subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, "GroupMessageTopic", "NorthAmerica");
```

<span data-ttu-id="34af5-140">Registrera sedan en meddelandehanterare – det här är den asynkrona metoden i koden som bearbetar det hämtade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="34af5-140">Then register a message handler - this is the asynchronous method in your code that processes the retrieved message.</span></span>

```C#
subscriptionClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

<span data-ttu-id="34af5-141">Anropa `SubscriptionClient.CompleteAsync()`-metoden i meddelandehanteraren, så tas meddelandet bort från kön:</span><span class="sxs-lookup"><span data-stu-id="34af5-141">Within the message handler, call the `SubscriptionClient.CompleteAsync()` method to remove the message from the queue:</span></span>

```C#
await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
```