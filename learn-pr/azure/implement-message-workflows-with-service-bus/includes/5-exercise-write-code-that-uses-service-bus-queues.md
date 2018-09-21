Du har valt att använda en Service Bus-kö för att utbyta meddelanden om enskilda försäljningar mellan den mobilapp som din säljpersonal använder och webbtjänsten på Azure, som lagrar information om varje försäljning i en Azure SQL-databas.

Du har redan implementerat nödvändiga objekt i Azure-prenumerationen. Nu vill du skriva kod som skickar meddelanden till kön och hämtar meddelanden.

## <a name="clone-and-open-the-starter-application"></a>Klona och öppna startprogrammet

I den här enheten kommer du att slutföra två konsolprogram. Det första programmet placerar meddelanden i en Service Bus-kö och det andra hämtar dem. Programmen är en del av en enda .NET Core-lösning.

1. Börja genom att klona lösningen: kör följande kommandon i Cloud Shell:

```bash
cd ~
git clone https://github.com/MicrosoftDocs/mslearn-connect-services-together.git
```

2. Sedan ändrar du kataloger till startmappen och öppnar Cloud Shell-redigeraren.

```bash
cd mslearn-connect-services-together/implement-message-workflows-with-service-bus/src/start
code .
```

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Konfigurera en anslutningssträng för en Service Bus-namnrymd

För att få åtkomst till en Service Bus-namnrymd och använda en kö, måste du konfigurera två typer av information i din konsolapp:

* Slutpunkten för din namnrymd
* Den delade åtkomstnyckeln för autentisering

Båda dessa värden kan hämtas från Azure Portal i form av en fullständig anslutningssträng.

> [!NOTE]
> För enkelhets skull hårdkodar du anslutningssträngen i filen **Program.cs** för båda konsolprogrammen. I ett produktionsprogram skulle du kunna använda en konfigurationsfil eller Azure Key Vault för att lagra anslutningssträngen.

1. I Cloud Shell kör du följande kommando för att visa den primära anslutningssträngen för Service Bus-namnrymden. Ersätt `<namespace-name>` med namnet på Service Bus-namnrymden.

    ```azurecli
    az servicebus namespace authorization-rule keys list \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --namespace-name <namespace-name> \
        --name RootManageSharedAccessKey \
        --query primaryConnectionString \
        --output tsv
    ```

    Du kommer att behöva den här anslutningssträngen flera gånger under den här modulen, så det kan vara bra att klistra in den någonstans där du enkelt kommer åt den.

1. Kopiera nyckeln från Cloud Shell. I redigeraren, öppna **privatemessagesender/Program.cs** och leta upp följande rad med kod:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    Klistra in anslutningssträngen mellan citattecknen. Spara filen med <kbd>Ctrl+S</kbd>.

1. Upprepa föregående steg i **privatemessagereceiver/Program.cs**, och klistra in samma anslutningssträngvärde. Spara filen antingen via menyn ”...” eller snabbtangenten (<kbd>Ctrl + S</kbd> i Windows och Linux, <kbd>Cmd + S</kbd> på macOS).

## <a name="write-code-that-sends-a-message-to-the-queue"></a>Skriv den kod som skickar ett meddelande till kön

Följ dessa steg för att slutföra komponenten som skickar meddelanden om försäljning:

1. Öppna **privatemessagesender/Program.cs** i redigeraren

1. Leta upp metoden `SendSalesMessageAsync()`.

1. Leta upp följande kodrad i den aktuella metoden:

    ```C#
    // Create a queue client here
    ```

1. Skapa en kö-klient genom att ersätta kodraden med följande kod:

    ```C#
    var queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. Leta upp följande kodrad i `try...catch`-blocket:

    ```C#
    // Create and send a message here
    ```

1. Skapa och utforma ett meddelande till kön genom att ersätta kodraden med följande kod:

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. För att visa meddelandet i konsolen lägger du till följande kod på nästa rad:

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. För att skicka meddelandet till kön lägger du till följande kod på nästa rad:

    ```C#
    await queueClient.SendAsync(message);
    ```

1. Leta rätt på följande kodrad:

    ```C#
    // Close the connection to the queue here
    ```

1. För att stänga anslutningen till Service Bus ersätter du kodraden med följande kod:

    ```C#
    await queueClient.CloseAsync();
    ```

1. Spara filen.

## <a name="send-a-message-to-the-queue"></a>Skicka ett meddelande till kön

För att köra den komponent som skickar ett meddelande om en försäljning kör du följande kommando i Cloud Shell:

```bash
dotnet run -p privatemessagesender
```

> [!NOTE]
> De appar som du kör under den här övningen kan ta en stund att starta eftersom `dotnet` måste återställa paket från fjärrkällor och bygga apparna första gången de körs.

När programmet körs ser du meddelanden som skrivs ut och indikerar att det skickar ett meddelande. Varje gång du kör appen läggs ett extra meddelande till i kön.

När det är klart kör du följande kommando för att se hur många meddelanden som finns i kön:

```azurecli
az servicebus queue show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

## <a name="write-code-that-receives-a-message-from-the-queue"></a>Skriv kod som tar emot ett meddelande från kön

1. Öppna **privatemessagereceiver/Program.cs** i redigeraren

1. Leta upp metoden `ReceiveSalesMessageAsync()`.

1. Leta upp följande kodrad i den aktuella metoden:

    ```C#
    // Create a queue client here
    ```

1. Skapa en kö-klient genom att ersätta raden med följande kod:

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
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
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. Leta upp metoden `ProcessMessagesAsync()`. Du har registrerat den här metoden som den som hanterar inkommande meddelanden.

1. Om du vill visa inkommande meddelanden i konsolen ersätter du all kod i metoden med följande kod:

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. För att ta bort det mottagna meddelandet från kön lägger du till följande kod på nästa rad:

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. Återvänd till metoden `ReceiveSalesMessageAsync()` och leta upp följande kodrad:

    ```C#
    // Close the queue here
    ```

1. Stäng anslutningen till Service Bus genom att ersätta du koden med följande kod:

    ```C#
    await queueClient.CloseAsync();
    ```

1. Spara filen.

## <a name="retrieve-a-message-from-the-queue"></a>Hämta ett meddelande från kön

För att köra den komponent som skickar ett meddelande om en försäljning kör du följande kommando i Cloud Shell:

```bash
dotnet run -p privatemessagereceiver
```

När du ser att meddelandet har tagits emot och visas i konsolen trycker du på `Enter` för att stoppa appen. Kör sedan samma kommando som förut för att bekräfta att alla meddelanden har tagits bort från kön:

```azurecli
az servicebus queue show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

Detta visar `0` om alla meddelanden har tagits bort.

Du har skrivit kod som skickar ett meddelande om enskilda försäljningar till en Service Bus-kö. I det distribuerade programmet för säljpersonal skriver du den här koden i mobilappen som säljarna använder på sina enheter.

Du har även skrivit kod som tar emot ett meddelande från Service Bus-kön. I det distribuerade programmet för säljpersonal skriver du den här koden i webbtjänsten som körs på Azure och bearbetar mottagna meddelanden.
