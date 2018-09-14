Du har valt att använda ett Azure Service Bus-ämne för att distribuera meddelanden om försäljningsresultat i ditt Salesforce-distribuerade program. Appen som säljpersonalen använder på sina mobila enheter skickar meddelanden som sammanfattar försäljningssiffrorna för varje område och tidsperiod. Dessa meddelanden kommer att distribueras till webbtjänster i företagets operativa regioner, inklusive USA och Europa.

Du har redan implementerat den nödvändiga infrastrukturen i din Azure-prenumeration, inklusive ämne och prenumerationer. Nu ska du skriva koden som skickar meddelanden till ämnet och hämtar meddelanden från varje prenumeration.

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Konfigurera en anslutningssträng för en Service Bus-namnrymd

Börja med att konfigurera anslutningssträngar i de sändande och mottagande komponenterna:

1. Växla till Azure Portal.

1. På startsidan klickar du på **Alla resurser** och sedan på Service Bus-namnrymden du skapade tidigare.

1. Under **INSTÄLLNINGAR** klickar du på **Principer för delad åtkomst**.

1. I listan med principer klickar du på **RootManageSharedAccessKey**.

1. Till höger om textrutan **Primär anslutningssträng** klickar du på knappen **Klicka för att kopiera**.

1. Växla till **Visual Studio Code**.

1. I **Explorer**-fönstret i mappen **performancemessagesender** klickar du på filen **Program.cs**.

1. Leta upp följande kodrad:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Placera markören mellan citattecknen och tryck på **Ctrl-V**.

1. I **Explorer**-fönstret i mappen **performancemessagereceiver** klickar du på filen **Program.cs**.

1. Leta upp följande kodrad:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Placera markören mellan citattecknen och tryck på **Ctrl-V**.

1. Klicka på **Arkiv** och sedan på **Spara alla**.

1. Stäng alla öppna redigeringsfönster.

## <a name="write-code-that-sends-a-message-to-the-topic"></a>Skriv koden som skickar ett meddelande till ämnet

Följ dessa steg för att slutföra komponenten som skickar meddelanden om försäljningsresultat:

1. I Visual Studio Code, i **Explorer**-fönstret i mappen **performancemessagesender**, klickar du på filen **Program.cs**.

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

1. Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.

## <a name="send-a-message-to-the-topic"></a>Skicka ett meddelande till ämnet

Följ dessa steg om du vill köra komponenten som skickar ett meddelande om en försäljning:

1. I Visual Studio Code går du till menyn **Visa** och klickar på **Felsöka**.

1. I fönstret **Felsöka** väljer du **Starta Performance Message Sender** i den nedrullningsbara listrutan och trycker på **F5**. Visual Studio Code bygger och kör konsolprogrammet i felsökningsläge.

1. När programmet körs kan du granska meddelandena som visas i **Felsökningskonsolen**.

1. Växla till Azure Portal.

1. Om Service Bus-namnrymden inte visas går du till startsidan och klickar på **Alla resurser** och därefter på den Service Bus-namnrymd du skapade tidigare.

1. På **Service Bus-namnrymd**-bladet, under **ENTITETER**, klickar du på **Ämnen** och sedan på ämnet **salesperformancemessages**. I listan över prenumerationer bör det finnas ett meddelande som visas i både prenumerationen för **USA** och **Europa**.

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a>Skriva kod som tar emot ett meddelande från en ämnesprenumeration

Följ dessa steg för att slutföra komponenten som hämtar meddelanden om försäljningsresultat:

1. I Visual Studio Code, i **Explorer**-fönstret i mappen **performancemessagereceiver** klickar du på filen **Program.cs**.

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

Följ dessa steg för att köra komponenten som hämtar ett meddelande om försäljningsresultat:

1. I Visual Studio Code går du till menyn **Visa** och klickar på **Felsöka**.

1. I fönstret **Felsöka** väljer du **Starta Performance Message Receiver** i den nedrullningsbara listrutan och trycker på **F5**. Visual Studio Code bygger och kör konsolprogrammet i felsökningsläge.

1. När programmet körs kan du granska meddelandena som visas i **Felsökningskonsolen**.

1. När du ser att meddelandet har tagits emot och visas i konsolen klickar du på menyn **Felsöka** och därefter på **Stoppa felsökning**.

1. Växla till Azure Portal.

1. Om Service Bus-namnrymden inte visas går du till startsidan och klickar på **Alla resurser** och därefter på den Service Bus-namnrymd du skapade tidigare.

1. På **Service Bus-namnrymd**-bladet, under **ENTITETER**, klickar du på **Ämnen** och sedan på ämnet **salesperformancemessages**. I listan över prenumerationer bör det finnas noll meddelanden som visas i prenumerationen **USA** eftersom ditt program har bearbetat och tagit bort det enda meddelandet. Observera att meddelandet är kvar i prenumerationen **Europa**.
