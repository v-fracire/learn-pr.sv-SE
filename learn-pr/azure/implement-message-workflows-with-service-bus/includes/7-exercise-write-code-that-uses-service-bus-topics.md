<span data-ttu-id="b0365-101">Du har valt att använda ett Azure Service Bus-ämne för att distribuera meddelanden om försäljningsresultat i ditt Salesforce-distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="b0365-101">You have chosen to use an Azure Service Bus topic to distribute messages about sales performance in your sales force distributed application.</span></span> <span data-ttu-id="b0365-102">Appen som säljpersonalen använder på sina mobila enheter skickar meddelanden som sammanfattar försäljningssiffrorna för varje område och tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="b0365-102">The app used by sales personnel on their mobile devices will send messages that summarize sales figures for each area and time period.</span></span> <span data-ttu-id="b0365-103">Dessa meddelanden kommer att distribueras till webbtjänster i företagets operativa regioner, inklusive USA och Europa.</span><span class="sxs-lookup"><span data-stu-id="b0365-103">Those messages will be distributed to web services located in the company's operational regions, including the Americas and Europe.</span></span>

<span data-ttu-id="b0365-104">Du har redan implementerat den nödvändiga infrastrukturen i din Azure-prenumeration, inklusive ämne och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b0365-104">You have already implemented the necessary infrastructure in your Azure subscription, including the topic and subscriptions.</span></span> <span data-ttu-id="b0365-105">Nu ska du skriva koden som skickar meddelanden till ämnet och hämtar meddelanden från varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b0365-105">Now, you want to write the code that sends messages to the topic and retrieves messages from each subscription.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="b0365-106">Konfigurera en anslutningssträng för en Service Bus-namnrymd</span><span class="sxs-lookup"><span data-stu-id="b0365-106">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="b0365-107">Börja med att konfigurera anslutningssträngar i de sändande och mottagande komponenterna:</span><span class="sxs-lookup"><span data-stu-id="b0365-107">Start by configuring connection strings both in the sending and receiving components:</span></span>

1. <span data-ttu-id="b0365-108">Växla till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b0365-108">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="b0365-109">På startsidan klickar du på **Alla resurser** och sedan på Service Bus-namnrymden du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="b0365-109">In the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="b0365-110">Under **INSTÄLLNINGAR** klickar du på **Principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="b0365-110">Under **SETTINGS**, click **Shared Access Policies**.</span></span>

1. <span data-ttu-id="b0365-111">I listan med principer klickar du på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="b0365-111">In the list of policies, click **RootManageSharedAccessKey**.</span></span>

1. <span data-ttu-id="b0365-112">Till höger om textrutan **Primär anslutningssträng** klickar du på knappen **Klicka för att kopiera**.</span><span class="sxs-lookup"><span data-stu-id="b0365-112">To the right of the **Primary Connection string** text box, click the **Click to copy** button.</span></span>

1. <span data-ttu-id="b0365-113">Växla till **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="b0365-113">Switch to **Visual Studio Code**.</span></span>

1. <span data-ttu-id="b0365-114">I **Explorer**-fönstret i mappen **performancemessagesender** klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="b0365-114">In the **Explorer** pane, in the **performancemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="b0365-115">Leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="b0365-115">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="b0365-116">Placera markören mellan citattecknen och tryck på **Ctrl-V**.</span><span class="sxs-lookup"><span data-stu-id="b0365-116">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="b0365-117">I **Explorer**-fönstret i mappen **performancemessagereceiver** klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="b0365-117">In the **Explorer** pane, in the **performancemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="b0365-118">Leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="b0365-118">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="b0365-119">Placera markören mellan citattecknen och tryck på **Ctrl-V**.</span><span class="sxs-lookup"><span data-stu-id="b0365-119">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="b0365-120">Klicka på **Arkiv** och sedan på **Spara alla**.</span><span class="sxs-lookup"><span data-stu-id="b0365-120">Click **File**, and then click **Save All**.</span></span>

1. <span data-ttu-id="b0365-121">Stäng alla öppna redigeringsfönster.</span><span class="sxs-lookup"><span data-stu-id="b0365-121">Close all open editor windows.</span></span>

## <a name="write-code-that-sends-a-message-to-the-topic"></a><span data-ttu-id="b0365-122">Skriv koden som skickar ett meddelande till ämnet</span><span class="sxs-lookup"><span data-stu-id="b0365-122">Write code that sends a message to the topic</span></span>

<span data-ttu-id="b0365-123">Följ dessa steg för att slutföra komponenten som skickar meddelanden om försäljningsresultat:</span><span class="sxs-lookup"><span data-stu-id="b0365-123">To complete the component that sends messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="b0365-124">I Visual Studio Code, i **Explorer**-fönstret i mappen **performancemessagesender**, klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="b0365-124">In Visual Studio Code, in the **Explorer** pane, in the **performancemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="b0365-125">Leta upp metoden `SendPerformanceMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="b0365-125">Locate the `SendPerformanceMessageAsync()` method.</span></span>

1. <span data-ttu-id="b0365-126">Leta upp följande kodrad i den aktuella metoden:</span><span class="sxs-lookup"><span data-stu-id="b0365-126">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a Topic Client here
    ```

1. <span data-ttu-id="b0365-127">Skapa en ämnesklient genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b0365-127">To create a topic client, replace that line of code with the following code:</span></span>

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. <span data-ttu-id="b0365-128">Leta upp följande kodrad i `try...catch`-blocket:</span><span class="sxs-lookup"><span data-stu-id="b0365-128">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="b0365-129">Skapa och utforma ett meddelande till kön genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b0365-129">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="b0365-130">För att visa meddelandet i konsolen lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="b0365-130">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="b0365-131">För att skicka meddelandet till kön lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="b0365-131">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await topicClient.SendAsync(message);
    ```

1. <span data-ttu-id="b0365-132">Leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="b0365-132">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the topic here
    ```

1. <span data-ttu-id="b0365-133">För att stänga anslutningen till Service Bus ersätter du kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b0365-133">To close the connection to Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await topicClient.CloseAsync();
    ```

1. <span data-ttu-id="b0365-134">Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.</span><span class="sxs-lookup"><span data-stu-id="b0365-134">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="send-a-message-to-the-topic"></a><span data-ttu-id="b0365-135">Skicka ett meddelande till ämnet</span><span class="sxs-lookup"><span data-stu-id="b0365-135">Send a message to the topic</span></span>

<span data-ttu-id="b0365-136">Följ dessa steg om du vill köra komponenten som skickar ett meddelande om en försäljning:</span><span class="sxs-lookup"><span data-stu-id="b0365-136">To run the component that sends a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="b0365-137">I Visual Studio Code går du till menyn **Visa** och klickar på **Felsöka**.</span><span class="sxs-lookup"><span data-stu-id="b0365-137">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="b0365-138">I fönstret **Felsöka** väljer du **Starta Performance Message Sender** i den nedrullningsbara listrutan och trycker på **F5**.</span><span class="sxs-lookup"><span data-stu-id="b0365-138">In the **Debug** pane, in the drop-down list, select **Launch Performance Message Sender**, and then press **F5**.</span></span> <span data-ttu-id="b0365-139">Visual Studio Code bygger och kör konsolprogrammet i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="b0365-139">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="b0365-140">När programmet körs kan du granska meddelandena som visas i **Felsökningskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="b0365-140">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="b0365-141">Växla till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b0365-141">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="b0365-142">Om Service Bus-namnrymden inte visas går du till startsidan och klickar på **Alla resurser** och därefter på den Service Bus-namnrymd du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="b0365-142">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="b0365-143">På **Service Bus-namnrymd**-bladet, under **ENTITETER**, klickar du på **Ämnen** och sedan på ämnet **salesperformancemessages**.</span><span class="sxs-lookup"><span data-stu-id="b0365-143">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Topics**, and then click the **salesperformancemessages** topic.</span></span> <span data-ttu-id="b0365-144">I listan över prenumerationer bör det finnas ett meddelande som visas i både prenumerationen för **USA** och **Europa**.</span><span class="sxs-lookup"><span data-stu-id="b0365-144">In the list of subscriptions, there should be one message displayed in both the **Americas** and **Europe** subscriptions.</span></span>

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a><span data-ttu-id="b0365-145">Skriva kod som tar emot ett meddelande från en ämnesprenumeration</span><span class="sxs-lookup"><span data-stu-id="b0365-145">Write code that receives a message from a topic subscription</span></span>

<span data-ttu-id="b0365-146">Följ dessa steg för att slutföra komponenten som hämtar meddelanden om försäljningsresultat:</span><span class="sxs-lookup"><span data-stu-id="b0365-146">To complete the component that retrieves messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="b0365-147">I Visual Studio Code, i **Explorer**-fönstret i mappen **performancemessagereceiver** klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="b0365-147">In Visual Studio Code, in the **Explorer** pane, in the **performancemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="b0365-148">Leta upp metoden `MainAsync()`.</span><span class="sxs-lookup"><span data-stu-id="b0365-148">Locate the `MainAsync()` method.</span></span>

1. <span data-ttu-id="b0365-149">Leta upp följande kodrad i den aktuella metoden:</span><span class="sxs-lookup"><span data-stu-id="b0365-149">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a subscription client here
    ```

1. <span data-ttu-id="b0365-150">Skapa en prenumerationsklient genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b0365-150">To create a subscription client, replace that line with the following code:</span></span>

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. <span data-ttu-id="b0365-151">Leta upp metoden `RegisterMessageHandler()`.</span><span class="sxs-lookup"><span data-stu-id="b0365-151">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="b0365-152">Om du vill konfigurera alternativ för meddelandehantering ersätter du all kod i metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b0365-152">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="b0365-153">För att registrera meddelandehanteraren lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="b0365-153">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="b0365-154">Leta upp metoden `ProcessMessagesAsync()`.</span><span class="sxs-lookup"><span data-stu-id="b0365-154">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="b0365-155">Du har registrerat den här metoden som den som hanterar inkommande meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b0365-155">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="b0365-156">Om du vill visa inkommande meddelanden i konsolen ersätter du all kod i metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b0365-156">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="b0365-157">För att ta bort det mottagna meddelandet från prenumerationen lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="b0365-157">To remove the received message from the subscription, on the next line, add the following code:</span></span>

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="b0365-158">Återvänd till metoden `MainAsync()` och leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="b0365-158">Return to the `MainAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the subscription here
    ```

1. <span data-ttu-id="b0365-159">För att stänga anslutningen till Service Bus ersätter du koden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b0365-159">To close the connection to Service Bus, replace that code with the following code:</span></span>

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. <span data-ttu-id="b0365-160">Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.</span><span class="sxs-lookup"><span data-stu-id="b0365-160">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-a-topic-subscription"></a><span data-ttu-id="b0365-161">Hämta ett meddelande från en ämnesprenumeration</span><span class="sxs-lookup"><span data-stu-id="b0365-161">Retrieve a message from a topic subscription</span></span>

<span data-ttu-id="b0365-162">Följ dessa steg för att köra komponenten som hämtar ett meddelande om försäljningsresultat:</span><span class="sxs-lookup"><span data-stu-id="b0365-162">To run the component that retrieves a message about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="b0365-163">I Visual Studio Code går du till menyn **Visa** och klickar på **Felsöka**.</span><span class="sxs-lookup"><span data-stu-id="b0365-163">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="b0365-164">I fönstret **Felsöka** väljer du **Starta Performance Message Receiver** i den nedrullningsbara listrutan och trycker på **F5**.</span><span class="sxs-lookup"><span data-stu-id="b0365-164">In the **Debug** pane, in the drop-down list, select **Launch Performance Message Receiver**, and then press **F5**.</span></span> <span data-ttu-id="b0365-165">Visual Studio Code bygger och kör konsolprogrammet i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="b0365-165">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="b0365-166">När programmet körs kan du granska meddelandena som visas i **Felsökningskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="b0365-166">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="b0365-167">När du ser att meddelandet har tagits emot och visas i konsolen klickar du på menyn **Felsöka** och därefter på **Stoppa felsökning**.</span><span class="sxs-lookup"><span data-stu-id="b0365-167">When you see that the message has been received and displayed in the console, on the **Debug** menu, click **Stop Debugging**.</span></span>

1. <span data-ttu-id="b0365-168">Växla till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b0365-168">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="b0365-169">Om Service Bus-namnrymden inte visas går du till startsidan och klickar på **Alla resurser** och därefter på den Service Bus-namnrymd du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="b0365-169">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="b0365-170">På **Service Bus-namnrymd**-bladet, under **ENTITETER**, klickar du på **Ämnen** och sedan på ämnet **salesperformancemessages**.</span><span class="sxs-lookup"><span data-stu-id="b0365-170">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Topics**, and then click the **salesperformancemessages** topic.</span></span> <span data-ttu-id="b0365-171">I listan över prenumerationer bör det finnas noll meddelanden som visas i prenumerationen **USA** eftersom ditt program har bearbetat och tagit bort det enda meddelandet.</span><span class="sxs-lookup"><span data-stu-id="b0365-171">In the list of subscriptions, there should be zero messages displayed in the **Americas** subscription because your application has processed and removed the only message.</span></span> <span data-ttu-id="b0365-172">Observera att meddelandet är kvar i prenumerationen **Europa**.</span><span class="sxs-lookup"><span data-stu-id="b0365-172">Notice that the message is still present in the **Europe** subscription.</span></span>
