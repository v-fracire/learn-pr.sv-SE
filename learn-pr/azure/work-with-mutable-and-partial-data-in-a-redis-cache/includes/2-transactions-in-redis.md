Det finns tillfällen när du måste garantera att flera åtgärder köra tillsammans. Till exempel i programmet för snabbmeddelanden, kan användare skicka en enskild bild, en enskilda SMS eller en bild och text visas tillsammans. När användaren väljer att skicka ett meddelande med bilder och text tillsammans, måste du se till att andra medlemmar i gruppen får dem på samma gång. Detta är viktigt eftersom om en bild och text-meddelande inte är emot tillsammans, är det möjligt att ett separat meddelande kunde skickas mellan bilden och texten visas. Som kan göra övergripande konversationen förvirrande.

Här kan ska vi titta på hur du skapar en transaktion i Azure Redis Cache och garanterar att flera åtgärder utförs tillsammans.

## <a name="how-to-create-a-transaction"></a>Så här skapar du en transaktion

Du måste ha ett block transaktion för att skapa en transaktion. Det här är en kö som innehåller de kommandon som körs tillsammans. Den `MULTI` används för att skapa en transaktion block och alla efterföljande kommandon ställs i kö för körning av atomiska.

## <a name="how-to-execute-a-transaction"></a>Hur du kör en transaktion

För att köra kommandona i blocket en transaktion som du använder den `EXEC` kommando. Detta kommer kör alla köade kommandon och återställa anslutningen till normal. Om du inte vill köra en transaktion kan du använda den `DISCARD` kommando, vilket tar bort blockeringen transaktion och ställa in anslutningstillståndet till normal.

En sak som är viktig att känna till om Azure Redis Cache transaktioner är att de inte stöder återställningar. Det innebär att om ett kommando i en transaktion misslyckas, de resterande kommandona fortfarande körs.

## <a name="what-is-servicestackredis"></a>What is ServiceStack.Redis?

**ServiceStack.Redis** är en C#-klientbiblioteket för att interagera med Azure Redis Cache. Detta innebär att i stället för på den låga nivån Azure Redis Cache-kommandon, vi kan använda C#-klasser och metoder. Till exempel istället för att använda den `MULTI` och `EXEC` kommandon för att köra en transaktion med **ServiceStack.Redis** skapar vi en `IRedisTransaction` objekt med hjälp av den `CreateTransaction()` metoden.

```csharp
var transaction = redisClient.CreateTransaction();
```

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a>Skapa en transaktion med C# och ServiceStack.Redis-klienten

Här är ett exempel på hur du använder **ServiceStack.Redis** att skapa en transaktion som kan skicka ett meddelande som innehåller en bild och text.

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        return transactionResult;
    }
}
```
Transaktioner se till att flera åtgärder utförs tillsammans. Du skapar en transaktion med hjälp av den `MULTI` kommando. Om du använder ett klientbibliotek som **ServiceStack.Redis**, du kan använda den `CreateTransaction()` metoden.