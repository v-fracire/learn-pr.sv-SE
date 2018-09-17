Distribuerade program använder köer, till exempel Service Bus-köer, som tillfälliga lagringsplatser för meddelanden som väntar på leverans till en målkomponent. För att skicka och ta emot meddelanden via en kö måste du skriva kod i käll- och målkomponenterna.

Ta Contoso Slices-programmet som exempel. Användaren gör beställningen via en webbplats eller mobilapp. Eftersom webbplatser och mobilappar körs på kundernas enheter finns det ingen egentlig gräns för hur många beställningar som kan komma in samtidigt. Genom att bestämma att mobilappen och webbplatsen placerar beställningarna i en kö kan vi låta serverkomponenten (en webbapp) bearbeta beställningarna från kön i sin egen takt.

Contoso Slices-programmet har faktiskt flera steg för att hantera en ny beställning. Men alla steg är beroende av att betalningen först godkänns, så vi väljer att använda en kö. Den mottagande komponentens första jobb blir att bearbeta betalningen.

Contoso måste i både mobilappen och webbplatsen skriva kod som lägger till ett meddelande i kön. I serverdelswebbappen skriver de kod som plockar upp meddelanden från kön.

Här lär du dig skriva den koden.

## <a name="the-microsoftazureservicebus-nuget-package"></a>NuGet-paketet Microsoft.Azure.ServiceBus

Microsoft har ett bibliotek med .NET-klasser som gör det enkelt att skriva kod som skickar och tar emot meddelanden via Service Bus. Du kan använda biblioteket i alla .NET Framework-språk för att interagera med en Service Bus-kö, ett Service Bus-ämne eller Service Bus-relä. Du kan inkludera det här biblioteket i ditt program genom att lägga till NuGet-paketet **Microsoft.Azure.ServiceBus**.

Den viktigaste klassen för köer i biblioteket är klassen `QueueClient`. Du måste börja med att skapa en instans av den här klassen i både skickande och mottagande komponenter.

## <a name="connection-strings-and-keys"></a>Anslutningssträngar och nycklar

Käll- och målkomponenterna behöver två typer av information för att ansluta till en kö i en Service Bus-namnrymd:

- Platsen för Service Bus-namnrymden, som även kallas en **slutpunkt**. Platsen anges som ett fullständigt domännamn inom domänen **servicebus.windows.net**. Till exempel: **pizzaService.servicebus.windows.net**.
- En åtkomstnyckel. Service Bus begränsar åtkomsten till köer, ämnen och reläer genom att kräva en åtkomstnyckel.

De här två typerna av information anges för `QueueClient`-objektet i form av en anslutningssträng. Du kan hämta den rätta anslutningssträngen för namnrymden från Azure-portalen.

## <a name="calling-methods-asynchronously"></a>Anropa metoder asynkront

Kön i Azure kan finnas hundratals mil från skickande och mottagande komponenter. Även om den är fysiskt nära kan långsamma anslutningar och konkurrens om bandbredden orsaka fördröjningar när en komponent anropar en metod i kön. Därför tillhandahåller Service Bus-klientbiblioteket `async`-metoder för att interagera med köerna. Genom att använda de här metoderna kan vi undvika att blockera en tråd medan vi väntar på att anrop ska slutföras.

Använd till exempel metoden `QueueClient.SendAsync()` med nyckelordet `await` när ett meddelande ska skickas till en kö.

## <a name="write-code-that-sends-to-queues"></a>Skriva kod som skickar till köer 

I alla sändande och mottagande komponenter bör du lägga till följande `using`-uttryck i alla kodfiler som anropar en Service Bus-kö:

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

Skapa sedan ett nytt `QueueClient`-objekt, och skicka anslutningssträngen och köns namn till objektet:

```C#
queueClient = new QueueClient(TextAppConnectionString, "PrivateMessageQueue");
```

Du kan skicka ett meddelande till kön genom att anropa metoden `QueueClient.SendAsync()` och skicka meddelandet i form av en UTF8-kodad sträng:

```C#
string message = "Sure would like a large pepperoni!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await queueClient.SendAsync(encodedMessage);
```

## <a name="receive-messages-from-queue"></a>Ta emot meddelanden från kön

För att ta emot meddelanden måste du först registrera en meddelandehanterare. Det här är metoden i din kod som anropas när ett meddelande finns tillgängligt i kön.

```C#
queueClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

Utför bearbetningsarbetet. Inom meddelandehanteraren anropar du sedan metoden `QueueClient.CompleteAsync()`, så tas meddelandet bort från kön:

```C#
await queueClient.CompleteAsync(message.SystemProperties.LockToken);
```
    