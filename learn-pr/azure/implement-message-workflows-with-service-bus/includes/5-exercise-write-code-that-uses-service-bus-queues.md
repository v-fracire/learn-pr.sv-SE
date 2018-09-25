<span data-ttu-id="d3929-101">Du har valt att använda en Service Bus-kö för att utbyta meddelanden om enskilda försäljningar mellan den mobilapp som din säljpersonal använder och webbtjänsten på Azure, som lagrar information om varje försäljning i en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d3929-101">You've chosen to use a Service Bus queue to exchange messages about individual sales between the mobile app that your sales personnel use and the web service, hosted in Azure, that will store details about each sale in an Azure SQL Database instance.</span></span>

<span data-ttu-id="d3929-102">Du har redan implementerat nödvändiga objekt i Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="d3929-102">You've already implemented the necessary objects in your Azure subscription.</span></span> <span data-ttu-id="d3929-103">Nu vill du skriva kod som skickar meddelanden till kön och hämtar meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d3929-103">Now, you want to write code that sends messages to that queue and retrieves messages.</span></span>

## <a name="clone-and-open-the-starter-application"></a><span data-ttu-id="d3929-104">Klona och öppna startprogrammet</span><span class="sxs-lookup"><span data-stu-id="d3929-104">Clone and open the starter application</span></span>

<span data-ttu-id="d3929-105">I den här enheten kommer du att slutföra två konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="d3929-105">In this unit, you'll build two console applications.</span></span> <span data-ttu-id="d3929-106">Det första programmet placerar meddelanden i en Service Bus-kö och det andra hämtar dem.</span><span class="sxs-lookup"><span data-stu-id="d3929-106">The first application places messages into a Service Bus queue and the second retrieves them.</span></span> <span data-ttu-id="d3929-107">Programmen är en del av en enda .NET Core-lösning.</span><span class="sxs-lookup"><span data-stu-id="d3929-107">The applications are part of a single .NET Core solution.</span></span>

1. <span data-ttu-id="d3929-108">Börja genom att klona lösningen: kör följande kommandon i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="d3929-108">Start by cloning the solution: run the following commands in the Cloud Shell:</span></span>

```bash
cd ~
git clone https://github.com/MicrosoftDocs/mslearn-connect-services-together.git
```

2. <span data-ttu-id="d3929-109">Sedan ändrar du kataloger till startmappen och öppnar Cloud Shell-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="d3929-109">Next, change directories into the starter folder and open the Cloud Shell editor.</span></span>

```bash
cd mslearn-connect-services-together/implement-message-workflows-with-service-bus/src/start
code .
```

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="d3929-110">Konfigurera en anslutningssträng för en Service Bus-namnrymd</span><span class="sxs-lookup"><span data-stu-id="d3929-110">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="d3929-111">För att få åtkomst till en Service Bus-namnrymd och använda en kö, måste du konfigurera två typer av information i din konsolapp:</span><span class="sxs-lookup"><span data-stu-id="d3929-111">In order to access a Service Bus namespace and use a queue, you must configure two pieces of information in your console apps:</span></span>

* <span data-ttu-id="d3929-112">Slutpunkten för din namnrymd</span><span class="sxs-lookup"><span data-stu-id="d3929-112">The endpoint for your namespace</span></span>
* <span data-ttu-id="d3929-113">Den delade åtkomstnyckeln för autentisering</span><span class="sxs-lookup"><span data-stu-id="d3929-113">The shared access key for authentication</span></span>

<span data-ttu-id="d3929-114">Båda dessa värden kan hämtas från Azure Portal i form av en fullständig anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="d3929-114">Both of these values can be obtained from the Azure portal in the form of a complete connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="d3929-115">För enkelhets skull hårdkodar du anslutningssträngen i filen **Program.cs** för båda konsolprogrammen.</span><span class="sxs-lookup"><span data-stu-id="d3929-115">For simplicity, you will hard-code the connection string in the **Program.cs** file of both console applications.</span></span> <span data-ttu-id="d3929-116">I ett produktionsprogram skulle du kunna använda en konfigurationsfil eller Azure Key Vault för att lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="d3929-116">In a production application, you might use a configuration file or even Azure Key Vault to store the connection string.</span></span>

1. <span data-ttu-id="d3929-117">I Cloud Shell kör du följande kommando för att visa den primära anslutningssträngen för Service Bus-namnrymden.</span><span class="sxs-lookup"><span data-stu-id="d3929-117">In the Cloud Shell, run the following command to display the primary connection string for your Service Bus namespace.</span></span> <span data-ttu-id="d3929-118">Ersätt `<namespace-name>` med namnet på Service Bus-namnrymden.</span><span class="sxs-lookup"><span data-stu-id="d3929-118">Replace `<namespace-name>` with the name of your Service Bus namespace.</span></span>

    ```azurecli
    az servicebus namespace authorization-rule keys list \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --namespace-name <namespace-name> \
        --name RootManageSharedAccessKey \
        --query primaryConnectionString \
        --output tsv
    ```

    <span data-ttu-id="d3929-119">Du kommer att behöva den här anslutningssträngen flera gånger under den här modulen, så det kan vara bra att klistra in den någonstans där du enkelt kommer åt den.</span><span class="sxs-lookup"><span data-stu-id="d3929-119">You'll be needing this connection string multiple times throughout this module, so you might want to paste it somewhere handy.</span></span>

1. <span data-ttu-id="d3929-120">Kopiera nyckeln från Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="d3929-120">Copy the key from Cloud Shell.</span></span> <span data-ttu-id="d3929-121">I redigeraren, öppna **privatemessagesender/Program.cs** och leta upp följande rad med kod:</span><span class="sxs-lookup"><span data-stu-id="d3929-121">In the editor, open **privatemessagesender/Program.cs** and locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    <span data-ttu-id="d3929-122">Klistra in anslutningssträngen mellan citattecknen.</span><span class="sxs-lookup"><span data-stu-id="d3929-122">Paste the connection string between the quotation marks.</span></span> <span data-ttu-id="d3929-123">Spara filen med <kbd>Ctrl+S</kbd>.</span><span class="sxs-lookup"><span data-stu-id="d3929-123">Save the file with <kbd>Ctrl+S</kbd>.</span></span>

1. <span data-ttu-id="d3929-124">Upprepa föregående steg i **privatemessagereceiver/Program.cs**, och klistra in samma anslutningssträngvärde.</span><span class="sxs-lookup"><span data-stu-id="d3929-124">Repeat the previous step in **privatemessagereceiver/Program.cs**, pasting in the same connection string value.</span></span> <span data-ttu-id="d3929-125">Spara filen antingen via menyn ”...” eller snabbtangenten (<kbd>Ctrl + S</kbd> i Windows och Linux, <kbd>Cmd + S</kbd> på macOS).</span><span class="sxs-lookup"><span data-stu-id="d3929-125">Save the file either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

## <a name="write-code-that-sends-a-message-to-the-queue"></a><span data-ttu-id="d3929-126">Skriv den kod som skickar ett meddelande till kön</span><span class="sxs-lookup"><span data-stu-id="d3929-126">Write code that sends a message to the queue</span></span>

<span data-ttu-id="d3929-127">Följ dessa steg för att slutföra komponenten som skickar meddelanden om försäljning:</span><span class="sxs-lookup"><span data-stu-id="d3929-127">To complete the component that sends messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="d3929-128">Öppna **privatemessagesender/Program.cs** i redigeraren</span><span class="sxs-lookup"><span data-stu-id="d3929-128">Open **privatemessagesender/Program.cs** in the editor</span></span>

1. <span data-ttu-id="d3929-129">Leta upp metoden `SendSalesMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="d3929-129">Locate the `SendSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="d3929-130">Leta upp följande kodrad i den aktuella metoden:</span><span class="sxs-lookup"><span data-stu-id="d3929-130">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="d3929-131">Skapa en kö-klient genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d3929-131">To create a queue client, replace that line of code with the following code:</span></span>

    ```C#
    var queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="d3929-132">Leta upp följande kodrad i `try...catch`-blocket:</span><span class="sxs-lookup"><span data-stu-id="d3929-132">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="d3929-133">Skapa och utforma ett meddelande till kön genom att ersätta kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d3929-133">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="d3929-134">För att visa meddelandet i konsolen lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="d3929-134">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="d3929-135">För att skicka meddelandet till kön lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="d3929-135">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.SendAsync(message);
    ```

1. <span data-ttu-id="d3929-136">Leta rätt på följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="d3929-136">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the queue here
    ```

1. <span data-ttu-id="d3929-137">För att stänga anslutningen till Service Bus ersätter du kodraden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d3929-137">To close the connection the Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="d3929-138">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="d3929-138">Save the file.</span></span>

## <a name="send-a-message-to-the-queue"></a><span data-ttu-id="d3929-139">Skicka ett meddelande till kön</span><span class="sxs-lookup"><span data-stu-id="d3929-139">Send a message to the queue</span></span>

<span data-ttu-id="d3929-140">För att köra den komponent som skickar ett meddelande om en försäljning kör du följande kommando i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="d3929-140">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p privatemessagesender
```

> [!NOTE]
> <span data-ttu-id="d3929-141">De appar som du kör under den här övningen kan ta en stund att starta eftersom `dotnet` måste återställa paket från fjärrkällor och bygga apparna första gången de körs.</span><span class="sxs-lookup"><span data-stu-id="d3929-141">The apps you run during this exercise may take a moment to start up, as `dotnet` has to restore packages from remote sources and build the apps the first time they are run.</span></span>

<span data-ttu-id="d3929-142">När programmet körs ser du meddelanden som skrivs ut och indikerar att det skickar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="d3929-142">As the program executes, you'll see messages printed indicating that it's sending a message.</span></span> <span data-ttu-id="d3929-143">Varje gång du kör appen läggs ett extra meddelande till i kön.</span><span class="sxs-lookup"><span data-stu-id="d3929-143">Each time you run the app, one additional message will be added to the queue.</span></span>

<span data-ttu-id="d3929-144">När det är klart kör du följande kommando för att se hur många meddelanden som finns i kön:</span><span class="sxs-lookup"><span data-stu-id="d3929-144">Once it's finished, run the following command to see how many messages are in the queue:</span></span>

```azurecli
az servicebus queue show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

## <a name="write-code-that-receives-a-message-from-the-queue"></a><span data-ttu-id="d3929-145">Skriv kod som tar emot ett meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="d3929-145">Write code that receives a message from the queue</span></span>

1. <span data-ttu-id="d3929-146">Öppna **privatemessagereceiver/Program.cs** i redigeraren</span><span class="sxs-lookup"><span data-stu-id="d3929-146">Open **privatemessagereceiver/Program.cs** in the editor</span></span>

1. <span data-ttu-id="d3929-147">Leta upp metoden `ReceiveSalesMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="d3929-147">Locate the `ReceiveSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="d3929-148">Leta upp följande kodrad i den aktuella metoden:</span><span class="sxs-lookup"><span data-stu-id="d3929-148">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="d3929-149">Skapa en kö-klient genom att ersätta raden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d3929-149">To create a queue client, replace that line with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="d3929-150">Leta upp metoden `RegisterMessageHandler()`.</span><span class="sxs-lookup"><span data-stu-id="d3929-150">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="d3929-151">Om du vill konfigurera alternativ för meddelandehantering ersätter du all kod i metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d3929-151">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="d3929-152">För att registrera meddelandehanteraren lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="d3929-152">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="d3929-153">Leta upp metoden `ProcessMessagesAsync()`.</span><span class="sxs-lookup"><span data-stu-id="d3929-153">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="d3929-154">Du har registrerat den här metoden som den som hanterar inkommande meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d3929-154">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="d3929-155">Om du vill visa inkommande meddelanden i konsolen ersätter du all kod i metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d3929-155">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="d3929-156">För att ta bort det mottagna meddelandet från kön lägger du till följande kod på nästa rad:</span><span class="sxs-lookup"><span data-stu-id="d3929-156">To remove the received message from the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="d3929-157">Återvänd till metoden `ReceiveSalesMessageAsync()` och leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="d3929-157">Return to the `ReceiveSalesMessageAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the queue here
    ```

1. <span data-ttu-id="d3929-158">Stäng anslutningen till Service Bus genom att ersätta du koden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d3929-158">To close the connection to Service Bus, replace that line with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="d3929-159">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="d3929-159">Save the file.</span></span>

## <a name="retrieve-a-message-from-the-queue"></a><span data-ttu-id="d3929-160">Hämta ett meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="d3929-160">Retrieve a message from the queue</span></span>

<span data-ttu-id="d3929-161">För att köra den komponent som skickar ett meddelande om en försäljning kör du följande kommando i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="d3929-161">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p privatemessagereceiver
```

<span data-ttu-id="d3929-162">När du ser att meddelandet har tagits emot och visas i konsolen trycker du på `Enter` för att stoppa appen.</span><span class="sxs-lookup"><span data-stu-id="d3929-162">When you see that the message has been received and displayed in the console, press `Enter` to stop the app.</span></span> <span data-ttu-id="d3929-163">Kör sedan samma kommando som förut för att bekräfta att alla meddelanden har tagits bort från kön:</span><span class="sxs-lookup"><span data-stu-id="d3929-163">Then, run the same command as before to confirm that all of the messages have been removed from the queue:</span></span>

```azurecli
az servicebus queue show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

<span data-ttu-id="d3929-164">Detta visar `0` om alla meddelanden har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="d3929-164">This will show `0` if all the messages have been removed.</span></span>

<span data-ttu-id="d3929-165">Du har skrivit kod som skickar ett meddelande om enskilda försäljningar till en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="d3929-165">You have written code that sends a message about individual sales to a Service Bus queue.</span></span> <span data-ttu-id="d3929-166">I det distribuerade programmet för säljpersonal skriver du den här koden i mobilappen som säljarna använder på sina enheter.</span><span class="sxs-lookup"><span data-stu-id="d3929-166">In the sales force distributed application, you should write this code in the mobile app that sales personnel use on devices.</span></span>

<span data-ttu-id="d3929-167">Du har även skrivit kod som tar emot ett meddelande från Service Bus-kön.</span><span class="sxs-lookup"><span data-stu-id="d3929-167">You have also written code that receives a message from the Service Bus queue.</span></span> <span data-ttu-id="d3929-168">I det distribuerade programmet för säljpersonal skriver du den här koden i webbtjänsten som körs på Azure och bearbetar mottagna meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d3929-168">In the sales force distributed application, you should write this code in the web service that runs in Azure and processes received messages.</span></span>
