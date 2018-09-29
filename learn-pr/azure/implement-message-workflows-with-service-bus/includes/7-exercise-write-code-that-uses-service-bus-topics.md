<span data-ttu-id="47c0b-101">Du har valt att använda ett Azure Service Bus-ämne för att distribuera meddelanden om försäljningsresultat i ditt Salesforce-distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="47c0b-101">You have chosen to use an Azure Service Bus topic to distribute messages about sales performance in your sales force distributed application.</span></span> <span data-ttu-id="47c0b-102">Appen som säljpersonalen använder på sina mobila enheter skickar meddelanden som sammanfattar försäljningssiffrorna för varje område och tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="47c0b-102">The app used by sales personnel on their mobile devices will send messages that summarize sales figures for each area and time period.</span></span> <span data-ttu-id="47c0b-103">Dessa meddelanden kommer att distribueras till webbtjänster i företagets operativa regioner, inklusive USA och Europa.</span><span class="sxs-lookup"><span data-stu-id="47c0b-103">Those messages will be distributed to web services located in the company's operational regions, including the Americas and Europe.</span></span>

<span data-ttu-id="47c0b-104">Du har redan implementerat den nödvändiga infrastrukturen i din Azure-prenumeration, inklusive ämne och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="47c0b-104">You have already implemented the necessary infrastructure in your Azure subscription, including the topic and subscriptions.</span></span> <span data-ttu-id="47c0b-105">Nu ska du skriva koden som skickar meddelanden till ämnet och hämtar meddelanden från en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="47c0b-105">Now, you want to write the code that sends messages to the topic and retrieves messages from a subscription.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="47c0b-106">Konfigurera en anslutningssträng för en Service Bus-namnrymd</span><span class="sxs-lookup"><span data-stu-id="47c0b-106">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="47c0b-107">Börja med att konfigurera anslutningssträngar i de sändande och mottagande komponenterna:</span><span class="sxs-lookup"><span data-stu-id="47c0b-107">Start by configuring connection strings both in the sending and receiving components:</span></span>

1. <span data-ttu-id="47c0b-108">I redigeraren, öppna **performancemessagesender/Program.cs** och leta upp följande rad med kod:</span><span class="sxs-lookup"><span data-stu-id="47c0b-108">In the editor, open **performancemessagesender/Program.cs** and locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    <span data-ttu-id="47c0b-109">Klistra in anslutningssträngen mellan citattecknen och spara filen antingen via menyn ”...” eller snabbtangenten (<kbd>Ctrl + S</kbd> i Windows och Linux, <kbd>Cmd + S</kbd> på macOS).</span><span class="sxs-lookup"><span data-stu-id="47c0b-109">Paste the connection string between the quotation marks and save the file either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="47c0b-110">Upprepa föregående steg i **performancemessagereceiver/Program.cs**, och klistra in samma anslutningssträngvärde och spara filen.</span><span class="sxs-lookup"><span data-stu-id="47c0b-110">Repeat the previous step in **performancemessagereceiver/Program.cs**, pasting in the same connection string value and save the file.</span></span>

## <a name="write-code-that-sends-a-message-to-the-topic"></a><span data-ttu-id="47c0b-111">Skriv den kod som skickar ett meddelande till ämnet</span><span class="sxs-lookup"><span data-stu-id="47c0b-111">Write code that sends a message to the topic</span></span>

<span data-ttu-id="47c0b-112">Följ dessa steg för att slutföra komponenten som skickar meddelanden om försäljningsresultat:</span><span class="sxs-lookup"><span data-stu-id="47c0b-112">To complete the component that sends messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="47c0b-113">Öppna **performancemessagesender/Program.cs** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="47c0b-113">Open **performancemessagesender/Program.cs** in the editor.</span></span>

1. <span data-ttu-id="47c0b-114">Leta upp metoden `SendPerformanceMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="47c0b-114">Locate the `SendPerformanceMessageAsync()` method.</span></span>

1. <span data-ttu-id="47c0b-115">Leta upp följande kodrad i den aktuella metoden:</span><span class="sxs-lookup"><span data-stu-id="47c0b-115">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a Topic Client here
    ```

1. <span data-ttu-id="47c0b-116">Skapa en ämnesklient genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="47c0b-116">To create a topic client, replace that line of code with the following code:</span></span>

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. <span data-ttu-id="47c0b-117">Leta upp följande kodrad i `try...catch`-blocket:</span><span class="sxs-lookup"><span data-stu-id="47c0b-117">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="47c0b-118">Skapa och utforma ett meddelande till kön genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="47c0b-118">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="47c0b-119">För att visa meddelandet i konsolen lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="47c0b-119">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="47c0b-120">För att skicka meddelandet till kön lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="47c0b-120">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await topicClient.SendAsync(message);
    ```

1. <span data-ttu-id="47c0b-121">Leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="47c0b-121">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the topic here
    ```

1. <span data-ttu-id="47c0b-122">För att stänga anslutningen till Service Bus ersätter du kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="47c0b-122">To close the connection to Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await topicClient.CloseAsync();
    ```

1. <span data-ttu-id="47c0b-123">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="47c0b-123">Save the file.</span></span>

## <a name="send-a-message-to-the-topic"></a><span data-ttu-id="47c0b-124">Skicka ett meddelande till ämnet</span><span class="sxs-lookup"><span data-stu-id="47c0b-124">Send a message to the topic</span></span>

<span data-ttu-id="47c0b-125">För att köra den komponent som skickar ett meddelande om en försäljning kör du följande kommando i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="47c0b-125">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p performancemessagesender
```

<span data-ttu-id="47c0b-126">När programmet körs ser du meddelanden som skrivs ut och indikerar att det skickar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="47c0b-126">As the program executes, you'll see messages printed indicating that it's sending a message.</span></span> <span data-ttu-id="47c0b-127">Varje gång du kör appen läggs ett extra meddelande till i ämnet, och varje prenumerant får en kopia.</span><span class="sxs-lookup"><span data-stu-id="47c0b-127">Each time you run the app, one additional message will be added to the topic, and each subscriber will receive a copy.</span></span>

<span data-ttu-id="47c0b-128">När det är klart kör du följande kommando för att se hur många meddelanden som finns i prenumerationen Americas:</span><span class="sxs-lookup"><span data-stu-id="47c0b-128">Once it's finished, run the following command to see how many messages are in the Americas subscription:</span></span>

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

<span data-ttu-id="47c0b-129">Om du använder `EuropeAndAfrica` i stället för `Americas` bör du se att båda prenumerationerna har samma antal meddelanden.</span><span class="sxs-lookup"><span data-stu-id="47c0b-129">If you substitute `EuropeAndAfrica` for `Americas`, you should see that both subscriptions have the same number of messages.</span></span>

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a><span data-ttu-id="47c0b-130">Skrivaa kod som tar emot ett meddelande från en ämnesprenumeration</span><span class="sxs-lookup"><span data-stu-id="47c0b-130">Write code that receives a message from a topic subscription</span></span>

<span data-ttu-id="47c0b-131">Följ dessa steg för att slutföra komponenten som hämtar meddelanden om försäljningsresultat:</span><span class="sxs-lookup"><span data-stu-id="47c0b-131">To complete the component that retrieves messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="47c0b-132">Öppna **performancemessagereceiver/Program.cs** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="47c0b-132">Open **performancemessagereceiver/Program.cs** in the editor.</span></span>

1. <span data-ttu-id="47c0b-133">Leta upp metoden `MainAsync()`.</span><span class="sxs-lookup"><span data-stu-id="47c0b-133">Locate the `MainAsync()` method.</span></span>

1. <span data-ttu-id="47c0b-134">Leta upp följande kodrad i den aktuella metoden:</span><span class="sxs-lookup"><span data-stu-id="47c0b-134">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a subscription client here
    ```

1. <span data-ttu-id="47c0b-135">Skapa en prenumerationsklient genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="47c0b-135">To create a subscription client, replace that line with the following code:</span></span>

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. <span data-ttu-id="47c0b-136">Leta upp metoden `RegisterMessageHandler()`.</span><span class="sxs-lookup"><span data-stu-id="47c0b-136">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="47c0b-137">Om du vill konfigurera alternativ för meddelandehantering ersätter du all kod i metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="47c0b-137">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="47c0b-138">För att registrera meddelandehanteraren lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="47c0b-138">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="47c0b-139">Leta upp metoden `ProcessMessagesAsync()`.</span><span class="sxs-lookup"><span data-stu-id="47c0b-139">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="47c0b-140">Du har registrerat den här metoden som den som hanterar inkommande meddelanden.</span><span class="sxs-lookup"><span data-stu-id="47c0b-140">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="47c0b-141">Om du vill visa inkommande meddelanden i konsolen ersätter du all kod i metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="47c0b-141">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="47c0b-142">För att ta bort det mottagna meddelandet från prenumerationen lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="47c0b-142">To remove the received message from the subscription, on the next line, add the following code:</span></span>

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="47c0b-143">Återvänd till metoden `MainAsync()` och leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="47c0b-143">Return to the `MainAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the subscription here
    ```

1. <span data-ttu-id="47c0b-144">För att stänga anslutningen till Service Bus ersätter du koden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="47c0b-144">To close the connection to Service Bus, replace that code with the following code:</span></span>

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. <span data-ttu-id="47c0b-145">Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.</span><span class="sxs-lookup"><span data-stu-id="47c0b-145">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-a-topic-subscription"></a><span data-ttu-id="47c0b-146">Hämta ett meddelande från en ämnesprenumeration</span><span class="sxs-lookup"><span data-stu-id="47c0b-146">Retrieve a message from a topic subscription</span></span>

<span data-ttu-id="47c0b-147">Följ dessa steg för att köra den komponent som hämtar ett meddelande om försäljningsresultat:</span><span class="sxs-lookup"><span data-stu-id="47c0b-147">To run the component that retrieves a message about sales performance, follow these steps:</span></span>

```bash
dotnet run -p performancemessagereceiver
```

<span data-ttu-id="47c0b-148">När programmet slutar skriva ut aviseringar om att det tar emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="47c0b-148">When the program stops printing notifications that it is receiving messages.</span></span> <span data-ttu-id="47c0b-149">trycker du på `Enter` för att stoppa appen.</span><span class="sxs-lookup"><span data-stu-id="47c0b-149">press `Enter` to stop the app.</span></span> <span data-ttu-id="47c0b-150">Kör sedan samma kommando som tidigare för att bekräfta att det finns noll återstående meddelanden i prenumerationen `Americas`.</span><span class="sxs-lookup"><span data-stu-id="47c0b-150">Then, run the same command as before to confirm that there are zero remaining messages in the `Americas` subscription.</span></span>

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

<span data-ttu-id="47c0b-151">Om du använder `EuropeAndAfrica` i stället för `Americas` ser du att meddelandeantalet inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="47c0b-151">If you substitute `EuropeAndAfrica` for `Americas`, you'll see that the message count has not changed.</span></span> <span data-ttu-id="47c0b-152">Programmet fick endast meddelanden från prenumerationen `Americas`.</span><span class="sxs-lookup"><span data-stu-id="47c0b-152">The application only received messages from the `Americas` subscription.</span></span>
