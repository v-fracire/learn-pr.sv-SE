<span data-ttu-id="f60ba-101">Du måste ofta uppdatera flera dokument i dina databaser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="f60ba-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="f60ba-102">Här går vi igenom hur du skapar, registrerar och kör lagrade procedurer från dina .NET-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="f60ba-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="f60ba-103">Skapa en lagrad procedur i din app</span><span class="sxs-lookup"><span data-stu-id="f60ba-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="f60ba-104">I den här lagrade proceduren används OrderId, som innehåller en lista med alla artiklar i ordern, till att beräkna en ordersumma.</span><span class="sxs-lookup"><span data-stu-id="f60ba-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="f60ba-105">Ordersumman beräknas genom att artiklarna i ordern summeras, minus eventuell kredit kunden har och med hänsyn till eventuella rabattkoder.</span><span class="sxs-lookup"><span data-stu-id="f60ba-105">The order total is calculated from the sum of the items in the order, less any dividend (credit) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="f60ba-106">Kopiera följande kod och klistra in den i slutet av filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="f60ba-106">Copy the following code and paste it into the end of the Program.cs file.</span></span>

    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->
    ```csharp
    private async Task UpdateOrderTotal(string databaseName, string collectionName, Order orderId)
    {
    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "ValidateDocumentAge"), document, 1920);
    }
    ```

2. <span data-ttu-id="f60ba-107">Kopiera sedan följande kod och klistra in den i slutet av metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="f60ba-107">Now copy the following code and paste it into the end of the **BasicOperations** method.</span></span>

    ```
    await this.UpdateOrderTotal("Users", "WebCustomers", "5-6235");
    ```

3. <span data-ttu-id="f60ba-108">Spara filen Program.cs och kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="f60ba-108">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```

    <span data-ttu-id="f60ba-109">Konsolen visar följande resultat.</span><span class="sxs-lookup"><span data-stu-id="f60ba-109">The console displays the following output.</span></span>

    ```
    Order total is 90.85.
    Press any key to continue ...
    ```

## <a name="clean-up"></a><span data-ttu-id="f60ba-110">Rensa</span><span class="sxs-lookup"><span data-stu-id="f60ba-110">Clean up</span></span>

<span data-ttu-id="f60ba-111">Om du planerar att fortsätta med modulerna i den här utbildningsvägen kan du hoppa över rensningsprocessen. Annars använder du följande steg för att ta bort dina resurser så att du inte debiteras för användning av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f60ba-111">If you plan to continue working on the modules in this learning path, skip the clean-up process, or else use the following steps to delete your resources to avoid incurring charges for use of the service.</span></span>

1. <span data-ttu-id="f60ba-112">Välj **Resursgrupper** i Azure Portal längst till vänster och välj sedan den resursgrupp du skapat.</span><span class="sxs-lookup"><span data-stu-id="f60ba-112">In the Azure portal, select **Resource groups** on the far left, and then select the resource group you created.</span></span>  

    <span data-ttu-id="f60ba-113">Om den vänstra menyn döljs klickar du på</span><span class="sxs-lookup"><span data-stu-id="f60ba-113">If the left menu is collapsed, click</span></span> ![Knappen Expandera](../media/5-javascript-programming/expand.png) <span data-ttu-id="f60ba-115">för att expandera den.</span><span class="sxs-lookup"><span data-stu-id="f60ba-115">to expand it.</span></span>

   ![Mått i Azure Portal](../media/5-javascript-programming/delete-resources-select.png)

2. <span data-ttu-id="f60ba-117">Markera resursgruppen i det nya fönstret och klicka sedan på **Ta bort resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="f60ba-117">In the new window select the resource group, and then click **Delete resource group**.</span></span>

   ![Mått i Azure Portal](../media/5-javascript-programming/delete-resources.png)

3. <span data-ttu-id="f60ba-119">I det nya fönstret, skriv namnet på resursgruppen som ska tas bort och klicka sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f60ba-119">In the new window, type the name of the resource group to delete, and then click **Delete**.</span></span>

## <a name="summary"></a><span data-ttu-id="f60ba-120">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f60ba-120">Summary</span></span>

<span data-ttu-id="f60ba-121">I den här modulen har du skapat ett .NET Core-konsolprogram som skapar, uppdaterar och tar bort användarposter, ställer frågor mot användarna med hjälp av SQL och LINQ samt kör en lagrad procedur för att skapa en ordersumma som tar hänsyn till krediter och rabatter.</span><span class="sxs-lookup"><span data-stu-id="f60ba-121">In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to create an order total that takes the users dividends and coupons into account.</span></span>