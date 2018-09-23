Det finns tillfällen då du måste garantera att flera åtgärder körs tillsammans. I programmet för snabbmeddelanden kan användarna exempelvis skicka följande: en enskild bild, ett enskilt textmeddelande eller en bild och text tillsammans. När användaren väljer att skicka ett meddelande med bilder och text tillsammans måste du kontrollera att andra medlemmar i gruppen får dem på samma gång. Detta är viktigt. Om ett bild- och textmeddelande inte tas emot tillsammans, är det nämligen möjligt att ett separat meddelande kan skickas mellan bilden och textmeddelandet. Det kan göra den övergripande konversationen förvirrande.

Här ska vi titta på hur du skapar en transaktion i Azure Redis Cache för att garantera att flera åtgärder körs tillsammans.

## <a name="creating-and-running-transactions"></a>Skapa och köra transaktioner

Transaktioner i Redis fungerar genom att sätta flera kommandon i kö så de körs som en grupp. När en transaktion körs, garanteras att de kommandon som är i kö inuti den körs utan andra kommandon från andra klienter överlagrade mellan dem.

För att påbörja ett transaktionsblock, anger du kommandot `MULTI`. Ytterligare kommandon kommer i kö och utförs inte omedelbart. Om du kör kommandot `EXEC` så körs alla köade kommandon som en transaktionell enhet. Om du vill att avbryta en öppen transaktion när queuing kommandon körs den `DISCARD` stängs transaktion blocket utan att köra kommandot _alla_ av kommandona i kö.

Redis-transaktioner stöder inte begreppet återställning. Om du köar ett kommando med felaktig syntax i ett transaktionsblock så kommer blocket att fortsätta vara öppet, men kommer automatiskt att tas bort om du försöker köra det med `EXEC`. Kommandon i en transaktion som misslyckas _under_ körning (efter `EXEC` kallas) leder inte till att transaktionen avbröts eller återställts &mdash; Redis kommer fortfarande köra alla kommandon och anser att transaktionen har slutförts.

## <a name="redis-transactions-with-servicestackredis"></a>Redis-transaktioner med ServiceStack.Redis

**ServiceStack.Redis** är ett C#-klientbiblioteket som används vid interaktion med Azure Redis Cache.

Transaktioner i ServiceStack.Redis skapas genom att anropa `IRedisClient.CreateTransaction()`. Det `IRedisTransaction` objekt som returneras kan ha flera kommandon i sin kö med `QueueCommand()`. Om du anropar `Commit()` på transaktionsobjektet så körs det.

`IRedisTransaction` objekt är disponibla och kommer automatiskt utfärda ett `DISCARD`-kommando om de tas bort innan du anropar `Commit()`. Den här funktionen fungerar bra med C# `using`-block: Om du inte genomför en transaktion av någon anledning, kan du lita på att transaktionen kommer att tas bort automatiskt så att Redis-anslutning kan fortsätta användas.

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a>Skapa en transaktion med C# och ServiceStack.Redis-klienten

Här är ett exempel på hur du använder ServiceStack.Redis för att skapa en transaktion som kan skicka ett meddelande som innehåller en bild-URL och innehållet i ett textmeddelande.

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    return transactionResult;
}
```

Transaktioner ser till att flera åtgärder utförs tillsammans utan åtgärder från andra klienter däremellan. Du skapar en transaktion med kommandot `MULTI`. Med ServiceStack.Redis, använder du `CreateTransaction()`-metoden.