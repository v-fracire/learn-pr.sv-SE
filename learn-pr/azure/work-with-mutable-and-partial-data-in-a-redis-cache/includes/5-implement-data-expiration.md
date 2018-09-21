<span data-ttu-id="4d385-101">Här lägger du till en förfallotid för dina data i Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="4d385-101">Here, you'll add an expiration time to our data in the Azure Redis Cache.</span></span>

## <a name="add-an-expiration-time"></a><span data-ttu-id="4d385-102">Lägga till en förfallotid</span><span class="sxs-lookup"><span data-stu-id="4d385-102">Add an expiration time</span></span>

<span data-ttu-id="4d385-103">I den senaste övningen slutade vi med följande kod i **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="4d385-103">In the last exercise, we left off with the following code in **Program.cs**.</span></span>

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

<span data-ttu-id="4d385-104">Nu ska vi lägga till en förfallotid på 15 sekunder för både **MyKey1** och **MyKey2**.</span><span class="sxs-lookup"><span data-stu-id="4d385-104">Let’s add an expiration of 15 seconds to both **MyKey1** and **MyKey2**.</span></span>

<span data-ttu-id="4d385-105">Lägg till följande kod innan du checkar in transaktionen:</span><span class="sxs-lookup"><span data-stu-id="4d385-105">Add the following code before you commit the transaction:</span></span>

```csharp
//Add an expiration time
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
```

<span data-ttu-id="4d385-106">I den här koden är metoden **Expire** (Upphör) en del av **RedisNativeClient**.</span><span class="sxs-lookup"><span data-stu-id="4d385-106">In this code, the **Expire** method is a part of the **RedisNativeClient**.</span></span> <span data-ttu-id="4d385-107">För att få åtkomst till metoden måste vi först omvandla objektet.</span><span class="sxs-lookup"><span data-stu-id="4d385-107">To access the method, we must first cast our object.</span></span>

## <a name="verify-the-expiration"></a><span data-ttu-id="4d385-108">Kontrollera förfallotiden</span><span class="sxs-lookup"><span data-stu-id="4d385-108">Verify the expiration</span></span>

<span data-ttu-id="4d385-109">Nu när vi har lagt till koden för att data ska förfalla ska vi köra programmet och kontrollera att data tas bort Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="4d385-109">Now that we added the code to expire our data, let's run the program and check that the data is removed from Azure Redis Cache.</span></span>

1. <span data-ttu-id="4d385-110">Kör programmet.</span><span class="sxs-lookup"><span data-stu-id="4d385-110">Run the program.</span></span>

    ```bash
    dotnet run
    ```

1. <span data-ttu-id="4d385-111">Byt tillbaka till Azure Redis Cache-konsolen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="4d385-111">Switch back to the Azure Redis Cache console in the Azure portal.</span></span>

1. <span data-ttu-id="4d385-112">Verifiera att data fortfarande finns där genom att utfärda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4d385-112">To verify that the data is still there, issue the following command:</span></span>

    ```console
    get MyKey1
    ```

1. <span data-ttu-id="4d385-113">Utfärda kommandot igen efter 15 sekunder.</span><span class="sxs-lookup"><span data-stu-id="4d385-113">After 15 seconds, issue the command again.</span></span> <span data-ttu-id="4d385-114">Du bör se att data inte längre finns där.</span><span class="sxs-lookup"><span data-stu-id="4d385-114">You should see that the data is no longer there.</span></span>

    ![Skärmbild av Azure Redis Cache-konsolen som visar att värdet för MyKey1 är noll](../media/6-redis-console-data-expiration.png)