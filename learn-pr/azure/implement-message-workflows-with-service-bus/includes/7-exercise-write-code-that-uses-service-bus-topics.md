Du har valt att använda ett Azure Service Bus-ämne för att distribuera meddelanden om försäljningsresultat i ditt Salesforce-distribuerade program. Appen som säljpersonalen använder på sina mobila enheter skickar meddelanden som sammanfattar försäljningssiffrorna för varje område och tidsperiod. Dessa meddelanden kommer att distribueras till webbtjänster i företagets operativa regioner, inklusive USA och Europa.

Du har redan implementerat den nödvändiga infrastrukturen i din Azure-prenumeration, inklusive ämne och prenumerationer. Nu ska du skriva koden som skickar meddelanden till ämnet och hämtar meddelanden från en prenumeration.

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Konfigurera en anslutningssträng för en Service Bus-namnrymd

Börja med att konfigurera anslutningssträngar i de sändande och mottagande komponenterna:

1. I redigeraren, öppna **performancemessagesender/Program.cs** och leta upp följande rad med kod:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    Klistra in anslutningssträngen mellan citattecknen och spara filen antingen via menyn ”...” eller snabbtangenten (<kbd>Ctrl + S</kbd> i Windows och Linux, <kbd>Cmd + S</kbd> på macOS).

1. Upprepa föregående steg i **performancemessagereceiver/Program.cs**, och klistra in samma anslutningssträngvärde och spara filen.

## <a name="write-code-that-sends-a-message-to-the-topic"></a>Skriv den kod som skickar ett meddelande till ämnet

Följ dessa steg för att slutföra komponenten som skickar meddelanden om försäljningsresultat:

1. Öppna **performancemessagesender/Program.cs** i redigeraren.

1. Leta upp metoden `SendPerformanceMessageAsync()`.

1. Leta upp följande kodrad i den aktuella metoden:

    ```C#
    // Create a Topic Client here
    ```

1. Skapa en ämnesklient genom att ersätta kodraden med följande kod:

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. Leta upp följande kodrad i `try...catch`-blocket:

    ```C#
    // Create and send a message here
    ```

1. Skapa och utforma ett meddelande till kön genom att ersätta kodraden med följande kod:

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. För att visa meddelandet i konsolen lägger du till följande kod på nästa rad:

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. För att skicka meddelandet till kön lägger du till följande kod på nästa rad:

    ```C#
    await topicClient.SendAsync(message);
    ```

1. Leta upp följande kodrad:

    ```C#
    // Close the connection to the topic here
    ```

1. För att stänga anslutningen till Service Bus ersätter du kodraden med följande kod:

    ```C#
    await topicClient.CloseAsync();
    ```

1. Spara filen.

## <a name="send-a-message-to-the-topic"></a>Skicka ett meddelande till ämnet

För att köra den komponent som skickar ett meddelande om en försäljning kör du följande kommando i Cloud Shell:

```bash
dotnet run -p performancemessagesender
```

När programmet körs ser du meddelanden som skrivs ut och indikerar att det skickar ett meddelande. Varje gång du kör appen läggs ett extra meddelande till i ämnet, och varje prenumerant får en kopia.

När det är klart kör du följande kommando för att se hur många meddelanden som finns i prenumerationen Americas:

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

Om du använder `EuropeAndAfrica` i stället för `Americas` bör du se att båda prenumerationerna har samma antal meddelanden.

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a>Skrivaa kod som tar emot ett meddelande från en ämnesprenumeration

Följ dessa steg för att slutföra komponenten som hämtar meddelanden om försäljningsresultat:

1. Öppna **performancemessagereceiver/Program.cs** i redigeraren.

1. Leta upp metoden `MainAsync()`.

1. Leta upp följande kodrad i den aktuella metoden:

    ```C#
    // Create a subscription client here
    ```

1. Skapa en prenumerationsklient genom att ersätta kodraden med följande kod:

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. Leta upp metoden `RegisterMessageHandler()`.

1. Om du vill konfigurera alternativ för meddelandehantering ersätter du all kod i metoden med följande kod:

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. För att registrera meddelandehanteraren lägger du till följande kod på nästa rad:

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. Leta upp metoden `ProcessMessagesAsync()`. Du har registrerat den här metoden som den som hanterar inkommande meddelanden.

1. Om du vill visa inkommande meddelanden i konsolen ersätter du all kod i metoden med följande kod:

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. För att ta bort det mottagna meddelandet från prenumerationen lägger du till följande kod på nästa rad:

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. Återvänd till metoden `MainAsync()` och leta upp följande kodrad:

    ```C#
    // Close the subscription here
    ```

1. För att stänga anslutningen till Service Bus ersätter du koden med följande kod:

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.

## <a name="retrieve-a-message-from-a-topic-subscription"></a>Hämta ett meddelande från en ämnesprenumeration

Följ dessa steg för att köra den komponent som hämtar ett meddelande om försäljningsresultat:

```bash
dotnet run -p performancemessagereceiver
```

När programmet slutar skriva ut aviseringar om att det tar emot meddelanden trycker du på `Enter` för att stoppa appen. Kör sedan samma kommando som tidigare för att bekräfta att det finns noll återstående meddelanden i prenumerationen `Americas`.

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

Om du använder `EuropeAndAfrica` i stället för `Americas` ser du att meddelandeantalet inte har ändrats. Programmet fick endast meddelanden från prenumerationen `Americas`.
