<span data-ttu-id="e97ee-101">Du måste ofta uppdatera flera dokument i dina databaser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e97ee-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="e97ee-102">Här går vi igenom hur du skapar, registrerar och kör lagrade procedurer från dina .NET-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="e97ee-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="e97ee-103">Skapa en lagrad procedur i din app</span><span class="sxs-lookup"><span data-stu-id="e97ee-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="e97ee-104">I den här lagrade proceduren används OrderId, som innehåller en lista med alla artiklar i ordern, till att beräkna en ordersumma.</span><span class="sxs-lookup"><span data-stu-id="e97ee-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="e97ee-105">Ordersumman beräknas genom att artiklarna i ordern summeras, minus eventuell kredit kunden har och med hänsyn till eventuella rabattkoder.</span><span class="sxs-lookup"><span data-stu-id="e97ee-105">The order total is calculated from the sum of the items in the order, less any dividends (credits) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="e97ee-106">I Visual Studio Code går du till fliken **Azure: Cosmos DB**, expanderar ditt Azure Cosmos DB-konto > **Användare** > **WebCustomers**, högerklickar på **Lagrade procedurer** och klickar sedan på **Skapa lagrad procedur**.</span><span class="sxs-lookup"><span data-stu-id="e97ee-106">In Visual Studio Code, in the **Azure: Cosmos DB** tab, expand your Azure Cosmos DB account > **Users** > **WebCustomers** and then right-click **Stored Procedures** and then click **Create Stored Procedure**.</span></span>

1. <span data-ttu-id="e97ee-107">I textrutan längst upp på skärmen skriver du `UpdateOrderTotal`. Klicka på Retur för att ge den lagrade proceduren ett namn.</span><span class="sxs-lookup"><span data-stu-id="e97ee-107">In the text box at the top of the screen, type `UpdateOrderTotal` and press Enter to give the stored procedure a name.</span></span>

1. <span data-ttu-id="e97ee-108">På fliken **Azure: Cosmos DB** expanderar du **Lagrade procedurer** och klickar på **UpdateOrderTotal**.</span><span class="sxs-lookup"><span data-stu-id="e97ee-108">In the **Azure: Cosmos DB** tab, expand **Stored Procedures** and click **UpdateOrderTotal**.</span></span>

    <span data-ttu-id="e97ee-109">En lagrad procedur hämtar som standard det första objektet.</span><span class="sxs-lookup"><span data-stu-id="e97ee-109">By default, a stored procedure that retrieves the first item is provided.</span></span>

1. <span data-ttu-id="e97ee-110">Om du vill köra den lagrade standardproceduren från ditt program lägger du till följande kod i filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="e97ee-110">To run this default stored procedure from your application, add the following code to the **Program.cs** file.</span></span>

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
        Console.WriteLine("Stored procedure complete");
    }
    ```

1. <span data-ttu-id="e97ee-111">Kopiera nedanstående kod och klistra in den före raden `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` i metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="e97ee-111">Now copy the following code and paste it before the `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` line in the **BasicOperations** method.</span></span>

    ```csharp
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="e97ee-112">I den integrerade terminalen använder du följande kommando för att köra exemplet med den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="e97ee-112">In the integrated terminal, run the following command to run the sample with the stored procedure.</span></span>

    ```bash
    dotnet run
    ```

<span data-ttu-id="e97ee-113">Konsolen visar utdata som anger att den lagrade proceduren har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e97ee-113">The console displays output indicating that the stored procedure was completed.</span></span>
