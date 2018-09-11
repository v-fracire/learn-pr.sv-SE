<span data-ttu-id="2bee6-101">Du måste ofta uppdatera flera dokument i dina databaser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="2bee6-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="2bee6-102">Här går vi igenom hur du skapar, registrerar och kör lagrade procedurer från dina .NET-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="2bee6-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="2bee6-103">Skapa en lagrad procedur i din app</span><span class="sxs-lookup"><span data-stu-id="2bee6-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="2bee6-104">I den här lagrade proceduren används OrderId, som innehåller en lista med alla artiklar i ordern, till att beräkna en ordersumma.</span><span class="sxs-lookup"><span data-stu-id="2bee6-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="2bee6-105">Ordersumman beräknas genom att artiklarna i ordern summeras, minus eventuell kredit kunden har och med hänsyn till eventuella rabattkoder.</span><span class="sxs-lookup"><span data-stu-id="2bee6-105">The order total is calculated from the sum of the items in the order, less any dividends (credits) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="2bee6-106">I Visual Studio Code på fliken Azure expanderar du **inlärningsmodulen (SQL)** > **Användare** > **WebCustomers**. Högerklicka sedan på **Lagrade procedurer** och klicka på **Skapa lagrad procedur**.</span><span class="sxs-lookup"><span data-stu-id="2bee6-106">In Visual Studio Code, in the Azure tab, expand the **learning-module (SQL)** > **Users** > **WebCustomers** and then right-click **Stored Procedures** and then click **Create Stored Procedure**.</span></span>

1. <span data-ttu-id="2bee6-107">I textrutan längst upp på skärmen skriver du *UpdateOrderTotal*. Klicka på Retur för att ge den lagrade proceduren ett namn.</span><span class="sxs-lookup"><span data-stu-id="2bee6-107">In the text box at the top of the screen, type *UpdateOrderTotal* and click Enter to give the stored procedure a name.</span></span>

1. <span data-ttu-id="2bee6-108">Expandera **Lagrade procedurer** och klicka på **UpdateOrderTotal**.</span><span class="sxs-lookup"><span data-stu-id="2bee6-108">Expand **Stored Procedures** and click **UpdateOrderTotal**.</span></span>

1. <span data-ttu-id="2bee6-109">En lagrad procedur hämtar som standard det första objektet.</span><span class="sxs-lookup"><span data-stu-id="2bee6-109">By default, a stored procedure that retrieves the first item is provided.</span></span>

1. <span data-ttu-id="2bee6-110">Om du vill köra den lagrade proceduren från ditt program, lägger du till följande kod i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="2bee6-110">To run this stored procedure from your application, add the following code to the Program.cs file.</span></span>

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "sample"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```
    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->

1. <span data-ttu-id="2bee6-111">Kopiera sedan nedanstående kod och klistra in den i slutet av metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="2bee6-111">Now copy the following code and paste it into the end of the **BasicOperations** method.</span></span>

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="2bee6-112">I den integrerade terminalen använder du följande kommando för att köra exemplet med den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="2bee6-112">In the integrated terminal, run the following command to run the sample with the stored procedure.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="2bee6-113">Konsolen visar utdata som anger att den lagrade proceduren har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2bee6-113">The console displays output indicating that the stored procedure was completed.</span></span>

## <a name="clean-up"></a><span data-ttu-id="2bee6-114">Rensa</span><span class="sxs-lookup"><span data-stu-id="2bee6-114">Clean up</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="2bee6-115">Om du planerar att fortsätta med modulerna i den här utbildningsvägen kan du hoppa över rensningsprocessen. Annars använder du följande steg för att ta bort dina resurser så att du inte debiteras för användning av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2bee6-115">If you plan to continue working on the modules in this learning path, skip the clean-up process, or else use the following steps to delete your resources to avoid incurring charges for use of the service.</span></span>

1. <span data-ttu-id="2bee6-116">Välj **Resursgrupper** i Azure Portal längst till vänster och välj sedan den resursgrupp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="2bee6-116">In the Azure portal, select **Resource groups** on the far left, and then select the resource group you created.</span></span>  

    <span data-ttu-id="2bee6-117">Om den vänstra menyn döljs klickar du på</span><span class="sxs-lookup"><span data-stu-id="2bee6-117">If the left menu is collapsed, click</span></span> ![Knappen Expandera](../media/5-javascript-programming/expand.png) <span data-ttu-id="2bee6-119">för att expandera den.</span><span class="sxs-lookup"><span data-stu-id="2bee6-119">to expand it.</span></span>

   ![Mått i Azure Portal](../media/5-javascript-programming/delete-resources-select.png)

1. <span data-ttu-id="2bee6-121">Markera resursgruppen i det nya fönstret och klicka sedan på **Ta bort resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="2bee6-121">In the new window select the resource group, and then click **Delete resource group**.</span></span>

   ![Mått i Azure Portal](../media/5-javascript-programming/delete-resources.png)

1. <span data-ttu-id="2bee6-123">I det nya fönstret skriver du namnet på resursgruppen som ska tas bort. Klicka sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2bee6-123">In the new window, type the name of the resource group to delete, and then click **Delete**.</span></span>

## <a name="summary"></a><span data-ttu-id="2bee6-124">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2bee6-124">Summary</span></span>

<span data-ttu-id="2bee6-125">I den här modulen har du skapat ett .NET Core-konsolprogram som skapar, uppdaterar och tar bort användarposter, ställer frågor till användarna med hjälp av SQL och LINQ, samt kör en lagrad procedur för att fråga efter objekt i databasen.</span><span class="sxs-lookup"><span data-stu-id="2bee6-125">In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to query items in the database.</span></span>