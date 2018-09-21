Här lägger du till en förfallotid för dina data i Azure Redis Cache.

## <a name="add-an-expiration-time"></a>Lägga till en förfallotid

I den senaste övningen slutade vi med följande kod i **Program.cs**.

```csharp
bool transactionResult = false;

using (RedisClient redisClient = new RedisClient(redisConnectionString))
using (var transaction = redisClient.CreateTransaction())
{
    //Add multiple operations to the transaction
    transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
    transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

    //Commit and get result of transaction
    transactionResult = transaction.Commit();
}

if(transactionResult)
{
    Console.WriteLine("Transaction committed");
}
else
{
    Console.WriteLine("Transaction failed to commit");
}
```

Nu ska vi lägga till en förfallotid på 15 sekunder för både **MyKey1** och **MyKey2**.

Lägg till följande kod innan du checkar in transaktionen:

```csharp
//Add an expiration time
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
```

I den här koden är metoden **Expire** (Upphör) en del av **RedisNativeClient**. För att få åtkomst till metoden måste vi först omvandla objektet.

## <a name="verify-the-expiration"></a>Kontrollera förfallotiden

Nu när vi har lagt till koden för att data ska förfalla ska vi köra programmet och kontrollera att data tas bort Azure Redis Cache.

1. Kör programmet.

    ```bash
    dotnet run
    ```

1. Byt tillbaka till Azure Redis Cache-konsolen i Azure Portal.

1. Verifiera att data fortfarande finns där genom att utfärda följande kommando:

    ```console
    get MyKey1
    ```

1. Utfärda kommandot igen efter 15 sekunder. Du bör se att data inte längre finns där.

    ![Skärmbild av Azure Redis Cache-konsolen som visar att värdet för MyKey1 är noll](../media/6-redis-console-data-expiration.png)