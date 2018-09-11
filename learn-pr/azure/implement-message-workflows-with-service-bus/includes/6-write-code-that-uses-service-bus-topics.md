I ett distribuerat program måste meddelanden läggas på en enda mottagarkomponent. Andra meddelanden måste nå fler än ett mål.

Nu ska vi diskutera vad som händer när en användare avbokar en pizzabeställning. Det är lite annorlunda än att göra den inledande beställningen. I det fallet ville vi vänta tills betalningsbearbetningen har slutförts för beställningen innan den skickas vidare till andra steg (som att förbereda och laga den i restaurangen). Men för avbokningen meddelar vi restaurangen och *och* betalningshanteraren samtidigt. På så sätt minimeras risken att ingredienser går till spillo eller att leveranstiden förlängs.

För att flera komponenter ska få samma meddelande använder vi ett Azure Service Bus-ämne.

## <a name="how-code-that-uses-topics-differs-from-queues"></a>Hur kod som använder ämnen skiljer sig från köer

Om du vill att alla meddelanden som skickats till ämnet ska levereras till alla prenumererande komponenter använder du ämnen. Att skriva kod som använder ämnen är en väg till köer. Du använder samma **Microsoft.Azure.ServiceBus** NuGet-paket konfigurerar du anslutningssträngar och använder asynkrona programmeringsmönster.

Men du använder klassen `TopicClient` i stället för klassen `QueueClient` till att skicka meddelanden och klassen `SubscriptionClient` för att ta emot meddelanden.

## <a name="setting-filters-on-subscriptions"></a>Ange filter för prenumerationer

Om du vill kontrollera vilka meddelanden som skickats till ämnet levereras till vilka prenumerationer kan du placera filter på varje prenumeration i ämnet. I pizzaprogrammet kör, till exempel, våra restauranger UWP-program. Varje restaurang kan prenumerera på ämnet ”OrderCancellation” (Avboka beställning) men filtrera efter eget StoreId. Vi sparar internetbandbredd eftersom vi inte skickar onödiga meddelanden till avlägsna försäljningsställen. Samtidigt prenumererar komponenten för betalningshantering på alla våra avbokningsmeddelanden.

Filter kan vara någon av följande tre typer:

- **Booleska filter.** `TrueFilter` ser till att alla meddelanden som skickats till ämnen levereras till den aktuella prenumerationen. `FalseFilter` ser till att inget meddelande levereras till den aktuella prenumerationen (detta blockerar eller stänger av prenumerationen).
- **SQL-filter.** Ett SQL-filter anger ett villkor genom att använda samma syntax som en `WHERE`-sats i en SQL-fråga. Bara meddelanden som returnerar `True`, vid utvärdering mot prenumerationen, levereras till prenumeranterna.
- **Korrelationsfilter.** Ett korrelationsfilter innehåller en uppsättning villkor som matchas mot egenskaperna för varje meddelande. Om egenskapen i filtret och egenskaperna för meddelandet har samma värde betraktas den som en matchning.

För vårt StoreId-filter *kan* vi använda ett SQL-filter. De är de mest flexibla men också de databehandlingsmässigt dyra och kan göra Service Bus-dataflödet långsammare. I det här fallet väljer vi i stället ett korrelationsfilter. 

## <a name="topicclient-example"></a>TopicClient-exampel

I alla skickande och mottagande komponenter bör du lägga till följande using-uttryck till alla kodfiler som anropar ett Service Bus-ämne:

    ```C#
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

Om du vill skicka ett meddelande börjar du med att skapa ett `TopicClient`-objekt och skicka anslutningssträngen och namnet på ämnet till objektet:

    ```C#
    topicClient = new TopicClient(TextAppConnectionString, "GroupMessageTopic");
    ```

Du kan skicka ett meddelande till ämnet, genom att anropa metoden `TopicClient.SendAsync()` och skicka meddelande. Som för köer måste meddelandet ha formatet UTF8-kodad sträng:

    ```C#
    string message = "Cancel! I can't believe you use canned mushrooms!";
    var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
    await topicClient.SendAsync(encodedMessage);
    ```

Om du vill ta emot meddelanden måste du skapa ett `SubscriptionClient`-objekt, inte ett `TopicClient`-objekt, och skicka anslutningssträngen, namnet på ämnet **och** namnet på prenumerationen:

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, "GroupMessageTopic", "NorthAmerica");
    ```

Registrera sedan en meddelandehanterare – det här är den asynkrona metoden i koden som bearbetar det hämtade meddelandet.

    ```C#
    subscriptionClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
    ```

Anropa `SubscriptionClient.CompleteAsync()`-metoden i meddelandehanteraren, så tas meddelandet bort från kön:

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```