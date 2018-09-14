Här kan du lägger till en förfallotid i våra data i Azure Redis Cache.

## <a name="add-an-expiration-time"></a>Lägg till en förfallotid

I den förra övningen vi slutade med följande kod i **Program.cs**.

```csharp
using (RedisClient redisClient = new RedisClient(redisConnectionString))
{
    //Create the transaction
    var transaction = redisClient.CreateTransaction();

    //Add multiple operations to the transaction
    transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
    transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

    //Commit and get result of transaction
    var transactionResult = transaction.Commit();

    if(transactionResult)
        Console.WriteLine("Transaction committed");
    else
        Console.WriteLine("Transaction failed to commit");
}
```

Lägg till en giltighetstid på 15 sekunder att både **MyKey1** och **MyKey2**.

Lägg till följande kod innan du skickar transaktionen:

    ```csharp
    //Add an expiration time
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
    ```

I den här koden i **upphör att gälla** metoden är en del av den **RedisNativeClient**. För att komma åt metoden måste vi först omvandla vår objekt.

## <a name="verify-the-expiration"></a>Kontrollera förfallodatum

Nu när vi har lagt till kod för att gå ut våra data vi kör programmet och kontrollera att data tas bort från Azure Redis Cache.

1. Kör programmet.

    ```bash
    dotnet run
    ```
    
1. Gå tillbaka till Azure Redis Cache-konsolen i Azure-portalen.

1. Verifiera att data är kvar, kör du följande kommando:

    ```
    get MyKey1
    ```

1. Efter 15 sekunder kan du utfärda kommandot igen. Du bör se att data inte längre finns där.

    ![Skärmbild av konsolen Azure Redis Cache med värdet för MyKey1 som noll](../media/6-redis-console-data-expiration.png)