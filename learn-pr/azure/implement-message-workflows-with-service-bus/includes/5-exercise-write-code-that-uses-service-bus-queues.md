<span data-ttu-id="3558f-101">Du har valt att använda en Service Bus-kö för att utbyta meddelanden om enskilda försäljningar mellan den mobilapp som din säljpersonal använder och webbtjänsten på Azure, som lagrar information om varje försäljning i en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3558f-101">You've chosen to use a Service Bus queue to exchange messages about individual sales between the mobile app that your sales personnel use and the web service, hosted in Azure, that will store details about each sale in an Azure SQL Database instance.</span></span>

<span data-ttu-id="3558f-102">Du har redan implementerat nödvändiga objekt i Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="3558f-102">You've already implemented the necessary objects in your Azure subscription.</span></span> <span data-ttu-id="3558f-103">Nu vill du skriva kod som skickar meddelanden till kön och hämtar meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3558f-103">Now, you want to write code that sends messages to that queue and retrieves messages.</span></span>

## <a name="clone-and-open-the-starter-application"></a><span data-ttu-id="3558f-104">Klona och öppna startprogrammet</span><span class="sxs-lookup"><span data-stu-id="3558f-104">Clone and open the starter application</span></span>

<span data-ttu-id="3558f-105">I den här enheten kommer du att slutföra två konsolprogram i **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="3558f-105">In this unit, you'll build two console applications in **Visual Studio Code**.</span></span> <span data-ttu-id="3558f-106">Det första programmet placerar meddelanden i en Service Bus-kö och det andra hämtar dem.</span><span class="sxs-lookup"><span data-stu-id="3558f-106">The first application places messages into a Service Bus queue and the second retrieves them.</span></span> <span data-ttu-id="3558f-107">Programmen är en del av en enda .NET Core-lösning.</span><span class="sxs-lookup"><span data-stu-id="3558f-107">The applications are part of a single .NET Core solution.</span></span> 

<span data-ttu-id="3558f-108">Börja genom att klona lösningen:</span><span class="sxs-lookup"><span data-stu-id="3558f-108">Start by cloning the solution:</span></span>

1. <span data-ttu-id="3558f-109">Starta en kommandotolk och ändra till den katalog där du vill hantera källkoden för programmet.</span><span class="sxs-lookup"><span data-stu-id="3558f-109">Start a command prompt and change to the directory where you want to host the source code for the application.</span></span>

1. <span data-ttu-id="3558f-110">Ange följande kommando och tryck på **Retur**:</span><span class="sxs-lookup"><span data-stu-id="3558f-110">Type the following command, and then press **Enter**:</span></span>

    ```powershell
    git clone https:\\ <!-- TODO: (add git URL) -->
    ```

1. <span data-ttu-id="3558f-111">När kloningen är genomförd byter du till startmappen.</span><span class="sxs-lookup"><span data-stu-id="3558f-111">When the clone operation is complete, change to the starter folder.</span></span>

1. <span data-ttu-id="3558f-112">Ange följande kommando och tryck på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="3558f-112">Type the following command, and then press **Enter**.</span></span>

    ```powershell
    code .
    ```

1. <span data-ttu-id="3558f-113">Om ett meddelande visas som frågar om du vill återställa beroenden klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="3558f-113">If a message appears asking if you want to restore dependencies, click **Yes**.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="3558f-114">Konfigurera en anslutningssträng för en Service Bus-namnrymd</span><span class="sxs-lookup"><span data-stu-id="3558f-114">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="3558f-115">För att få åtkomst till en Service Bus-namnrymd och använda en kö, måste du konfigurera två typer av information i din konsolapp:</span><span class="sxs-lookup"><span data-stu-id="3558f-115">In order to access a Service Bus namespace and use a queue, you must configure two pieces of information in your console apps:</span></span>

* <span data-ttu-id="3558f-116">Slutpunkten för din namnrymd</span><span class="sxs-lookup"><span data-stu-id="3558f-116">The endpoint for your namespace</span></span>
* <span data-ttu-id="3558f-117">Den delade åtkomstnyckeln för autentisering</span><span class="sxs-lookup"><span data-stu-id="3558f-117">The shared access key for authentication</span></span>

<span data-ttu-id="3558f-118">Båda dessa värden kan hämtas från Azure Portal i form av en fullständig anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="3558f-118">Both of these values can be obtained from the Azure portal in the form of a complete connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="3558f-119">För enkelhets skull hårdkodar du anslutningssträngen i filen **Program.cs** för båda konsolprogrammen.</span><span class="sxs-lookup"><span data-stu-id="3558f-119">For simplicity, you will hard-code the connection string in the **Program.cs** file of both console applications.</span></span> <span data-ttu-id="3558f-120">I ett produktionsprogram skulle du kunna använda en konfigurationsfil eller Azure Key Vault för att lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="3558f-120">In a production application, you might use a configuration file or even Azure Key Vault to store the connection string.</span></span>

1. <span data-ttu-id="3558f-121">Växla till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3558f-121">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="3558f-122">På startsidan klickar du på **Alla resurser** och sedan på Service Bus-namnrymden du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="3558f-122">In the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="3558f-123">Under **INSTÄLLNINGAR** klickar du på **Principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="3558f-123">Under **SETTINGS**, click **Shared Access Policies**.</span></span>

1. <span data-ttu-id="3558f-124">I listan med principer klickar du på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="3558f-124">In the list of policies, click **RootManageSharedAccessKey**.</span></span>

1. <span data-ttu-id="3558f-125">Till höger om textrutan **Primär anslutningssträng** klickar du på knappen **Klicka för att kopiera**.</span><span class="sxs-lookup"><span data-stu-id="3558f-125">To the right of the **Primary Connection string** text box, click the **Click to copy** button.</span></span>

1. <span data-ttu-id="3558f-126">Växla till **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="3558f-126">Switch to **Visual Studio Code**.</span></span>

1. <span data-ttu-id="3558f-127">I **Explorer**-fönsterrutan i mappen **privatemessagesender** klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="3558f-127">In the **Explorer** pane, in the **privatemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="3558f-128">Leta rätt på följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="3558f-128">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="3558f-129">Placera markören mellan citattecknen och tryck på **Ctrl-V**.</span><span class="sxs-lookup"><span data-stu-id="3558f-129">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="3558f-130">I **Explorer**-fönsterrutan i mappen **privatemessagereceiver** klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="3558f-130">In the **Explorer** pane, in the **privatemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="3558f-131">Leta rätt på följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="3558f-131">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="3558f-132">Placera markören mellan citattecknen och tryck på **Ctrl-V**.</span><span class="sxs-lookup"><span data-stu-id="3558f-132">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="3558f-133">Klicka på **Arkiv** och sedan på **Spara alla**.</span><span class="sxs-lookup"><span data-stu-id="3558f-133">Click **File**, and then click **Save All**.</span></span>

1. <span data-ttu-id="3558f-134">Stäng alla öppna redigeringsfönster.</span><span class="sxs-lookup"><span data-stu-id="3558f-134">Close all open editor windows.</span></span>

## <a name="write-code-that-sends-a-message-to-the-queue"></a><span data-ttu-id="3558f-135">Skriv koden som skickar ett meddelande till kön</span><span class="sxs-lookup"><span data-stu-id="3558f-135">Write code that sends a message to the queue</span></span>

<span data-ttu-id="3558f-136">Följ dessa steg för att slutföra komponenten som skickar meddelanden om försäljning:</span><span class="sxs-lookup"><span data-stu-id="3558f-136">To complete the component that sends messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="3558f-137">I Visual Studio Code, i **Explorer**-fönsterrutan i mappen **privatemessagesender**, klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="3558f-137">In Visual Studio Code, in the **Explorer** pane, in the **privatemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="3558f-138">Leta upp metoden `SendSalesMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="3558f-138">Locate the `SendSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="3558f-139">Leta upp följande kodrad i den aktuella metoden:</span><span class="sxs-lookup"><span data-stu-id="3558f-139">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="3558f-140">Skapa en kö-klient genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="3558f-140">To create a queue client, replace that line of code with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="3558f-141">Leta upp följande kodrad i `try...catch`-blocket:</span><span class="sxs-lookup"><span data-stu-id="3558f-141">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="3558f-142">Skapa och utforma ett meddelande till kön genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="3558f-142">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="3558f-143">För att visa meddelandet i konsolen lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="3558f-143">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="3558f-144">För att skicka meddelandet till kön lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="3558f-144">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.SendAsync(message);
    ```

1. <span data-ttu-id="3558f-145">Leta rätt på följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="3558f-145">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the queue here
    ```

1. <span data-ttu-id="3558f-146">För att stänga anslutningen till Service Bus ersätter du kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="3558f-146">To close the connection the Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="3558f-147">Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.</span><span class="sxs-lookup"><span data-stu-id="3558f-147">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="send-a-message-to-the-queue"></a><span data-ttu-id="3558f-148">Skicka ett meddelande till kön</span><span class="sxs-lookup"><span data-stu-id="3558f-148">Send a message to the queue</span></span>

<span data-ttu-id="3558f-149">Följ dessa steg om du vill köra komponenten som skickar ett meddelande om en försäljning:</span><span class="sxs-lookup"><span data-stu-id="3558f-149">To run the component that sends a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="3558f-150">I Visual Studio Code går du till menyn **Visa** och klickar på **Felsöka**.</span><span class="sxs-lookup"><span data-stu-id="3558f-150">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="3558f-151">I fönsterrutan **Felsöka** väljer du **Starta Skicka privat meddelande** i den nedrullningsbara listrutan och trycker på **F5**.</span><span class="sxs-lookup"><span data-stu-id="3558f-151">In the **Debug** pane, in the drop-down list, select **Launch Private Message Sender**, and then press **F5**.</span></span> <span data-ttu-id="3558f-152">Visual Studio Code bygger och kör konsolprogrammet i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="3558f-152">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="3558f-153">När programmet körs kan du granska meddelandena som visas i **Felsökningskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="3558f-153">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="3558f-154">Växla till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="3558f-154">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="3558f-155">Om Service Bus-namnrymden inte visas går du till startsidan och klickar på **Alla resurser** och därefter på den Service Bus-namnrymd du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="3558f-155">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="3558f-156">På **Service Bus-namnrymd**-bladet, under **ENTITETER**, klickar du på **Köer** och sedan på kön **salesmessages**.</span><span class="sxs-lookup"><span data-stu-id="3558f-156">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Queues**, and then click the **salesmessages** queue.</span></span> <span data-ttu-id="3558f-157">**ANTAL AKTIVA MEDDELANDEN** ska visa att ett meddelande har lagts till i kön.</span><span class="sxs-lookup"><span data-stu-id="3558f-157">The **ACTIVE MESSAGE COUNT** should indicate that one message has been added to the queue.</span></span>

## <a name="write-code-that-receives-a-message-from-the-queue"></a><span data-ttu-id="3558f-158">Skriv kod som tar emot ett meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="3558f-158">Write code that receives a message from the queue</span></span>

<span data-ttu-id="3558f-159">Följ dessa steg för att slutföra komponenten som tar emot meddelanden om försäljning:</span><span class="sxs-lookup"><span data-stu-id="3558f-159">To complete the component that retrieves messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="3558f-160">I Visual Studio Code, i **Explorer**-fönsterrutan i mappen **privatemessagereceiver**, klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="3558f-160">In Visual Studio Code, in the **Explorer** pane, in the **privatemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="3558f-161">Leta upp metoden `ReceiveSalesMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="3558f-161">Locate the `ReceiveSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="3558f-162">Leta upp följande kodrad i den aktuella metoden:</span><span class="sxs-lookup"><span data-stu-id="3558f-162">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="3558f-163">Skapa en kö-klient genom att ersätta raden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="3558f-163">To create a queue client, replace that line with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="3558f-164">Leta upp metoden `RegisterMessageHandler()`.</span><span class="sxs-lookup"><span data-stu-id="3558f-164">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="3558f-165">Om du vill konfigurera alternativ för meddelandehantering ersätter du all kod i metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="3558f-165">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="3558f-166">För att registrera meddelandehanteraren lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="3558f-166">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="3558f-167">Leta upp metoden `ProcessMessagesAsync()`.</span><span class="sxs-lookup"><span data-stu-id="3558f-167">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="3558f-168">Du har registrerat den här metoden som den som hanterar inkommande meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3558f-168">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="3558f-169">Om du vill visa inkommande meddelanden i konsolen ersätter du all kod i metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="3558f-169">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="3558f-170">För att ta bort det mottagna meddelandet från kön lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="3558f-170">To remove the received message from the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="3558f-171">Återvänd till metoden `ReceiveSalesMessageAsync()` och leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="3558f-171">Return to the `ReceiveSalesMessageAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the queue here
    ```

1. <span data-ttu-id="3558f-172">Stäng anslutningen till Service Bus genom att ersätta du koden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="3558f-172">To close the connection to Service Bus, replace that line with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="3558f-173">Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.</span><span class="sxs-lookup"><span data-stu-id="3558f-173">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-the-queue"></a><span data-ttu-id="3558f-174">Hämta ett meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="3558f-174">Retrieve a message from the queue</span></span>

<span data-ttu-id="3558f-175">Följ dessa steg om du vill köra komponenten som tar emot ett meddelande om en försäljning:</span><span class="sxs-lookup"><span data-stu-id="3558f-175">To run the component that retrieves a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="3558f-176">I Visual Studio Code går du till menyn **Visa** och klickar på **Felsöka**.</span><span class="sxs-lookup"><span data-stu-id="3558f-176">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="3558f-177">I fönsterrutan **Felsöka** väljer du **Starta Ta emot privat meddelande** i den nedrullningsbara listrutan och trycker på **F5**.</span><span class="sxs-lookup"><span data-stu-id="3558f-177">In the **Debug** pane, in the drop-down list, select **Launch Private Message Receiver**, and then press **F5**.</span></span> <span data-ttu-id="3558f-178">Visual Studio Code bygger och kör konsolprogrammet i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="3558f-178">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="3558f-179">När programmet körs kan du granska meddelandena som visas i **Felsökningskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="3558f-179">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="3558f-180">När du ser att meddelandet har tagits emot och visas i konsolen klickar du på menyn **Felsöka** och därefter på **Stoppa felsökning**.</span><span class="sxs-lookup"><span data-stu-id="3558f-180">When you see that the message has been received and displayed in the console, on the **Debug** menu, click **Stop Debugging**.</span></span>

1. <span data-ttu-id="3558f-181">Växla till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="3558f-181">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="3558f-182">Om Service Bus-namnrymden inte visas går du till startsidan och klickar på **Alla resurser** och därefter på den Service Bus-namnrymd du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="3558f-182">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="3558f-183">På **Service Bus-namnrymd**-bladet, under **ENTITETER**, klickar du på **Köer** och sedan på kön **salesmessages**.</span><span class="sxs-lookup"><span data-stu-id="3558f-183">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Queues**, and then click the **salesmessages** queue.</span></span> <span data-ttu-id="3558f-184">**ANTAL AKTIVA MEDDELANDEN** ska visa att ett meddelande har tagits bort från kön.</span><span class="sxs-lookup"><span data-stu-id="3558f-184">The **ACTIVE MESSAGE COUNT** should indicate that the message has been removed from the queue.</span></span>

<span data-ttu-id="3558f-185">Du har skrivit kod som skickar ett meddelande om enskilda försäljningar till en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="3558f-185">You have written code that sends a message about individual sales to a Service Bus queue.</span></span> <span data-ttu-id="3558f-186">I det distribuerade programmet för säljpersonal skriver du den här koden i mobilappen som säljarna använder på sina enheter.</span><span class="sxs-lookup"><span data-stu-id="3558f-186">In the sales force distributed application, you should write this code in the mobile app that sales personnel use on devices.</span></span>

<span data-ttu-id="3558f-187">Du har även skrivit kod som tar emot ett meddelande från Service Bus-kön.</span><span class="sxs-lookup"><span data-stu-id="3558f-187">You have also written code that receives a message from the Service Bus queue.</span></span> <span data-ttu-id="3558f-188">I det distribuerade programmet för säljpersonal skriver du den här koden i webbtjänsten som körs på Azure och bearbetar mottagna meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3558f-188">In the sales force distributed application, you should write this code in the web service that runs in Azure and processes received messages.</span></span>
