Du har valt att använda en Service Bus-kö för att utbyta meddelanden om enskilda försäljningar mellan den mobilapp som din säljpersonal använder och webbtjänsten på Azure, som lagrar information om varje försäljning i en Azure SQL-databas.

Du har redan implementerat nödvändiga objekt i Azure-prenumerationen. Nu vill du skriva kod som skickar meddelanden till kön och hämtar meddelanden.

## <a name="clone-and-open-the-starter-application"></a>Klona och öppna startprogrammet

I den här enheten kommer du att slutföra två konsolprogram i **Visual Studio Code**. Det första programmet placerar meddelanden i en Service Bus-kö och det andra hämtar dem. Programmen är en del av en enda .NET Core-lösning. 

Börja genom att klona lösningen:

1. Starta en kommandotolk och ändra till den katalog där du vill hantera källkoden för programmet.

1. Ange följande kommando och tryck på **Retur**:

    ```powershell
    git clone https:\\ <!-- TODO: (add git URL) -->
    ```

1. När kloningen är genomförd byter du till startmappen.

1. Ange följande kommando och tryck på **Retur**.

    ```powershell
    code .
    ```

1. Om ett meddelande visas som frågar om du vill återställa beroenden klickar du på **Ja**.

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Konfigurera en anslutningssträng för en Service Bus-namnrymd

För att få åtkomst till en Service Bus-namnrymd och använda en kö, måste du konfigurera två typer av information i din konsolapp:

* Slutpunkten för din namnrymd
* Den delade åtkomstnyckeln för autentisering

Båda dessa värden kan hämtas från Azure Portal i form av en fullständig anslutningssträng.

> [!NOTE]
> För enkelhets skull hårdkodar du anslutningssträngen i filen **Program.cs** för båda konsolprogrammen. I ett produktionsprogram skulle du kunna använda en konfigurationsfil eller Azure Key Vault för att lagra anslutningssträngen.

1. Växla till Azure-portalen.

1. På startsidan klickar du på **Alla resurser** och sedan på Service Bus-namnrymden du skapade tidigare.

1. Under **INSTÄLLNINGAR** klickar du på **Principer för delad åtkomst**.

1. I listan med principer klickar du på **RootManageSharedAccessKey**.

1. Till höger om textrutan **Primär anslutningssträng** klickar du på knappen **Klicka för att kopiera**.

1. Växla till **Visual Studio Code**.

1. I **Explorer**-fönsterrutan i mappen **privatemessagesender** klickar du på filen **Program.cs**.

1. Leta rätt på följande kodrad:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Placera markören mellan citattecknen och tryck på **Ctrl-V**.

1. I **Explorer**-fönsterrutan i mappen **privatemessagereceiver** klickar du på filen **Program.cs**.

1. Leta rätt på följande kodrad:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Placera markören mellan citattecknen och tryck på **Ctrl-V**.

1. Klicka på **Arkiv** och sedan på **Spara alla**.

1. Stäng alla öppna redigeringsfönster.

## <a name="write-code-that-sends-a-message-to-the-queue"></a>Skriv koden som skickar ett meddelande till kön

Följ dessa steg för att slutföra komponenten som skickar meddelanden om försäljning:

1. I Visual Studio Code, i **Explorer**-fönsterrutan i mappen **privatemessagesender**, klickar du på filen **Program.cs**.

1. Leta upp metoden `SendSalesMessageAsync()`.

1. Leta upp följande kodrad i den aktuella metoden:

    ```C#
    // Create a queue client here
    ```

1. Skapa en kö-klient genom att ersätta kodraden med följande kod:

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
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

1. Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.

## <a name="send-a-message-to-the-queue"></a>Skicka ett meddelande till kön

Följ dessa steg om du vill köra komponenten som skickar ett meddelande om en försäljning:

1. I Visual Studio Code går du till menyn **Visa** och klickar på **Felsöka**.

1. I fönsterrutan **Felsöka** väljer du **Starta Skicka privat meddelande** i den nedrullningsbara listrutan och trycker på **F5**. Visual Studio Code bygger och kör konsolprogrammet i felsökningsläge.

1. När programmet körs kan du granska meddelandena som visas i **Felsökningskonsolen**.

1. Växla till Azure Portal.

1. Om Service Bus-namnrymden inte visas går du till startsidan och klickar på **Alla resurser** och därefter på den Service Bus-namnrymd du skapade tidigare.

1. På **Service Bus-namnrymd**-bladet, under **ENTITETER**, klickar du på **Köer** och sedan på kön **salesmessages**. **ANTAL AKTIVA MEDDELANDEN** ska visa att ett meddelande har lagts till i kön.

## <a name="write-code-that-receives-a-message-from-the-queue"></a>Skriv kod som tar emot ett meddelande från kön

Följ dessa steg för att slutföra komponenten som tar emot meddelanden om försäljning:

1. I Visual Studio Code, i **Explorer**-fönsterrutan i mappen **privatemessagereceiver**, klickar du på filen **Program.cs**.

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

1. Stäng alla redigeringsfönster i Visual Studio Code och spara alla ändrade filer.

## <a name="retrieve-a-message-from-the-queue"></a>Hämta ett meddelande från kön

Följ dessa steg om du vill köra komponenten som tar emot ett meddelande om en försäljning:

1. I Visual Studio Code går du till menyn **Visa** och klickar på **Felsöka**.

1. I fönsterrutan **Felsöka** väljer du **Starta Ta emot privat meddelande** i den nedrullningsbara listrutan och trycker på **F5**. Visual Studio Code bygger och kör konsolprogrammet i felsökningsläge.

1. När programmet körs kan du granska meddelandena som visas i **Felsökningskonsolen**.

1. När du ser att meddelandet har tagits emot och visas i konsolen klickar du på menyn **Felsöka** och därefter på **Stoppa felsökning**.

1. Växla till Azure Portal.

1. Om Service Bus-namnrymden inte visas går du till startsidan och klickar på **Alla resurser** och därefter på den Service Bus-namnrymd du skapade tidigare.

1. På **Service Bus-namnrymd**-bladet, under **ENTITETER**, klickar du på **Köer** och sedan på kön **salesmessages**. **ANTAL AKTIVA MEDDELANDEN** ska visa att ett meddelande har tagits bort från kön.

Du har skrivit kod som skickar ett meddelande om enskilda försäljningar till en Service Bus-kö. I det distribuerade programmet för säljpersonal skriver du den här koden i mobilappen som säljarna använder på sina enheter.

Du har även skrivit kod som tar emot ett meddelande från Service Bus-kön. I det distribuerade programmet för säljpersonal skriver du den här koden i webbtjänsten som körs på Azure och bearbetar mottagna meddelanden.
