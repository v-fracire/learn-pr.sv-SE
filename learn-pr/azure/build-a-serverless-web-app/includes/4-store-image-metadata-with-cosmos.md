<span data-ttu-id="8bc8d-101">Azure Cosmos DB är Microsofts serverlösa, globalt distribuerade databas för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-101">Azure Cosmos DB is Microsoft's serverless, globally distributed, multi-model database.</span></span> <span data-ttu-id="8bc8d-102">I den här modulen lär du dig hur du använder Azure Functions för att lagra och hämta bildmetadata som JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-102">In this module, you learn how to use Azure Functions to store and retrieve image metadata as JSON documents in Azure Cosmos DB.</span></span>

## <a name="create-an-azure-cosmos-db-account-database-and-collection"></a><span data-ttu-id="8bc8d-103">Lär dig att skapa ett Azure Cosmos DB-konto, en databas och en samling</span><span class="sxs-lookup"><span data-stu-id="8bc8d-103">Create an Azure Cosmos DB account, database, and collection</span></span>

<span data-ttu-id="8bc8d-104">Ett Azure Cosmos DB-konto är en Azure-resurs som innehåller Azure Cosmos DB-databaser.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-104">An Azure Cosmos DB account is an Azure resource that contains Azure Cosmos DB databases.</span></span>

1. <span data-ttu-id="8bc8d-105">Kontrollera att du fortfarande är inloggad i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-105">Ensure you're still signed into Cloud Shell.</span></span> <span data-ttu-id="8bc8d-106">Om du inte är det väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-106">If you aren't, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="8bc8d-107">Skapa ett Azure Cosmos DB-konto med ett unikt namn i samma resursgrupp som de andra resurserna i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-107">Create an Azure Cosmos DB account with a unique name in the same resource group as the other resources in this tutorial.</span></span>

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. <span data-ttu-id="8bc8d-108">När Azure Cosmos DB-kontot har skapats skapar du en ny databas med namnet **imagesdb** i kontot.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-108">After the Azure Cosmos DB account is created, create a new database named **imagesdb** in the account.</span></span>

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. <span data-ttu-id="8bc8d-109">När databasen har skapats skapar du en ny samling med namnet **images** i databasen med ett dataflöde på 400 enheter för programbegäran (RU:er).</span><span class="sxs-lookup"><span data-stu-id="8bc8d-109">After the database is created, create a new collection named **images** in the database with a throughput of 400 request units (RUs).</span></span>

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-azure-cosmos-db-when-a-thumbnail-is-created"></a><span data-ttu-id="8bc8d-110">Spara ett dokument till Azure Cosmos DB när en miniatyrbild skapas</span><span class="sxs-lookup"><span data-stu-id="8bc8d-110">Save a document to Azure Cosmos DB when a thumbnail is created</span></span>

<span data-ttu-id="8bc8d-111">Med Azure Cosmos DB-utdatabindningen kan du skapa dokument i en Azure Cosmos DB-samling från Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-111">The Azure Cosmos DB output binding lets you create documents in an Azure Cosmos DB collection from Azure Functions.</span></span> <span data-ttu-id="8bc8d-112">I följande steg ska du konfigurera en Azure Cosmos DB-utdatabindning i funktionen **ResizeImage** och ändra funktionen för att returnera ett dokument (objekt) som ska sparas.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-112">In the following steps, you configure an Azure Cosmos DB output binding in the **ResizeImage** function and modify the function to return a document (object) to be saved.</span></span>

1. <span data-ttu-id="8bc8d-113">Öppna Funktionsapp på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-113">Open the function app in the Azure portal.</span></span>

1. <span data-ttu-id="8bc8d-114">Expandera funktionen **ResizeImage** i det vänstra navigeringsfönstret och välj **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-114">In the left navigation, expand the **ResizeImage** function, and then select **Integrate**.</span></span>

1. <span data-ttu-id="8bc8d-115">Klicka på **Nya utdata** under **Utdata**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-115">Under **Outputs**, click **New Output**.</span></span>

1. <span data-ttu-id="8bc8d-116">Leta upp och markera **Azure Cosmos DB**-objektet.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-116">Find the **Azure Cosmos DB** item and select it.</span></span> <span data-ttu-id="8bc8d-117">Klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-117">Then click **Select**.</span></span>

    ![Välj Nya utdata](../media/4-new-output.jpg)

1. <span data-ttu-id="8bc8d-119">Fyll i fälten under **Azure Cosmos DB-utdata** med följande värden.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-119">Fill out the fields under **Azure Cosmos DB output** with the following values.</span></span>

    | <span data-ttu-id="8bc8d-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="8bc8d-120">Setting</span></span>      |  <span data-ttu-id="8bc8d-121">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="8bc8d-121">Suggested value</span></span>   | <span data-ttu-id="8bc8d-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8bc8d-122">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="8bc8d-123">**Dokumentparameternamn**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-123">**Document parameter name**</span></span> | <span data-ttu-id="8bc8d-124">Välj **Använd funktionsreturvärde**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-124">Select **Use function return value**.</span></span> | <span data-ttu-id="8bc8d-125">Värdet i textrutan ställs automatiskt in till **$return**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-125">The value in the box is automatically set to **$return**.</span></span> |
    | <span data-ttu-id="8bc8d-126">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-126">**Database name**</span></span> | <span data-ttu-id="8bc8d-127">imagesdb</span><span class="sxs-lookup"><span data-stu-id="8bc8d-127">imagesdb</span></span> | <span data-ttu-id="8bc8d-128">Använd namnet på databasen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-128">Use the name of the database that you created.</span></span> |
    | <span data-ttu-id="8bc8d-129">**Samlingsnamn**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-129">**Collection name**</span></span> | <span data-ttu-id="8bc8d-130">images</span><span class="sxs-lookup"><span data-stu-id="8bc8d-130">images</span></span> | <span data-ttu-id="8bc8d-131">Använd namnet på samlingen som du skapat.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-131">Use the name of the collection that you created.</span></span> |

1. <span data-ttu-id="8bc8d-132">Klicka på **Ny** bredvid **Azure Cosmos DB-kontoanslutningen**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-132">Next to **Azure Cosmos DB account connection**, click **new**.</span></span> <span data-ttu-id="8bc8d-133">Välj Azure Cosmos DB-kontot som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-133">Select the Azure Cosmos DB account that you previously created.</span></span>

    ![Ange inställningar för Azure Cosmos DB-utdatabindningen](../media/4-cosmos-db-output.png)

1. <span data-ttu-id="8bc8d-135">Skapa Azure Cosmos DB-utdatabindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-135">Click **Save** to create the Azure Cosmos DB output binding.</span></span>

1. <span data-ttu-id="8bc8d-136">Klicka på funktionsnamnet **ResizeImage** till vänster för att öppna funktionen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-136">Click on the **ResizeImage** function name on the left to open the function.</span></span>

1. <span data-ttu-id="8bc8d-137">**C#**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-137">**C#**</span></span>

    1. <span data-ttu-id="8bc8d-138">(C#) Ändra returtypen för funktionen från **void** till **object**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-138">(C#) Change the return type of the function from **void** to **object**.</span></span>

    1. <span data-ttu-id="8bc8d-139">(C#) Returnera dokumentet som ska sparas genom att lägga till följande kodblock i slutet av funktionen:</span><span class="sxs-lookup"><span data-stu-id="8bc8d-139">(C#) At the end of the function, add the following code block to return the document to be saved:</span></span>
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![run.csx för ResizeImages-funktionen efter ändringarna](../media/4-update-function.png)

1. <span data-ttu-id="8bc8d-141">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-141">**JavaScript**</span></span>

    <span data-ttu-id="8bc8d-142">(JavaScript) Ändra `context.done()`-instruktionen i `else`-satsen för att returnera dokumentet som ska sparas i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-142">(JavaScript) Change the `context.done()` statement in the `else` clause to return the document to be saved to Azure Cosmos DB.</span></span>

    ```javascript
    if (error) {
        context.done(error);
    } else {
        context.bindings.thumbnail = stream;
        context.done(null, {
            id: context.bindingData.name,
            imgPath: "/images/" + context.bindingData.name,
            thumbnailPath: "/thumbnails/" + context.bindingData.name
        });
    }
    ```

1. <span data-ttu-id="8bc8d-143">Klicka på **Loggar** under kodfönstret för att expanera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-143">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="8bc8d-144">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-144">Click **Save**.</span></span> <span data-ttu-id="8bc8d-145">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-145">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="create-a-function-to-list-images-from-azure-cosmos-db"></a><span data-ttu-id="8bc8d-146">Skapa en funktion för att visa bilderna från Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8bc8d-146">Create a function to list images from Azure Cosmos DB</span></span>

<span data-ttu-id="8bc8d-147">Webbprogrammet kräver ett API för att hämta bildmetadata från Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-147">The web application requires an API to retrieve image metadata from Azure Cosmos DB.</span></span> <span data-ttu-id="8bc8d-148">I följande steg ska du skapa en HTTP-utlöst funktion som använder en Azure Cosmos DB-indatabindning för att fråga databassamlingen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-148">In the following steps, you create an HTTP-triggered function that uses an Azure Cosmos DB input binding to query the database collection.</span></span>

1. <span data-ttu-id="8bc8d-149">Peka på **Funktioner** till vänster i Funktionsapp och klicka på plustecknet (+) för att skapa en ny funktion.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-149">In your function app, point to **Functions** on the left and click the plus sign (+) to create a new function.</span></span>

1. <span data-ttu-id="8bc8d-150">Leta upp och välj mallen **HttpTrigger**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-150">Find the **HttpTrigger** template and select it.</span></span>

1. <span data-ttu-id="8bc8d-151">Använd dessa värden för att skapa en funktion som genererar en URL för att hämta bilder:</span><span class="sxs-lookup"><span data-stu-id="8bc8d-151">Use these values to create a function that generates a get images URL:</span></span>

    | <span data-ttu-id="8bc8d-152">Inställning</span><span class="sxs-lookup"><span data-stu-id="8bc8d-152">Setting</span></span>      |  <span data-ttu-id="8bc8d-153">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="8bc8d-153">Suggested value</span></span>   | <span data-ttu-id="8bc8d-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8bc8d-154">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="8bc8d-155">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-155">**Name your function**</span></span> | <span data-ttu-id="8bc8d-156">GetImages</span><span class="sxs-lookup"><span data-stu-id="8bc8d-156">GetImages</span></span> | <span data-ttu-id="8bc8d-157">Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-157">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="8bc8d-158">**Auktorisationsnivå**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-158">**Authorization level**</span></span> | <span data-ttu-id="8bc8d-159">Anonym</span><span class="sxs-lookup"><span data-stu-id="8bc8d-159">Anonymous</span></span> | <span data-ttu-id="8bc8d-160">Gör funktionen offentligt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-160">Allow the function to be accessed publicly.</span></span> |

1. <span data-ttu-id="8bc8d-161">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-161">Click **Create**.</span></span>

1. <span data-ttu-id="8bc8d-162">När den nya funktionen har skapats klickar du på **Integrera** under funktionens namn i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-162">When the new function is created, click **Integrate** under the function name on the left navigation.</span></span>

1. <span data-ttu-id="8bc8d-163">Klicka på **Nya indata** och välj **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-163">Click **New Input** and select **Azure Cosmos DB**.</span></span> 

    ![Välj Nya indata](../media/4-new-input.jpg)

1. <span data-ttu-id="8bc8d-165">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-165">Click **Select**.</span></span>

1. <span data-ttu-id="8bc8d-166">Fyll i följande värden:</span><span class="sxs-lookup"><span data-stu-id="8bc8d-166">Fill out the following values:</span></span>

    | <span data-ttu-id="8bc8d-167">Inställning</span><span class="sxs-lookup"><span data-stu-id="8bc8d-167">Setting</span></span>      |  <span data-ttu-id="8bc8d-168">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="8bc8d-168">Suggested value</span></span>   | <span data-ttu-id="8bc8d-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8bc8d-169">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="8bc8d-170">**Dokumentparameternamn**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-170">**Document parameter name**</span></span> | <span data-ttu-id="8bc8d-171">documents</span><span class="sxs-lookup"><span data-stu-id="8bc8d-171">documents</span></span> | <span data-ttu-id="8bc8d-172">Matchar parameternamnet i funktionen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-172">Matches the parameter name in the function.</span></span> |
    | <span data-ttu-id="8bc8d-173">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-173">**Database name**</span></span> | <span data-ttu-id="8bc8d-174">imagesdb</span><span class="sxs-lookup"><span data-stu-id="8bc8d-174">imagesdb</span></span> |  |
    | <span data-ttu-id="8bc8d-175">**Samlingsnamn**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-175">**Collection name**</span></span> | <span data-ttu-id="8bc8d-176">images</span><span class="sxs-lookup"><span data-stu-id="8bc8d-176">images</span></span> |  |
    | <span data-ttu-id="8bc8d-177">**SQL-fråga**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-177">**SQL query**</span></span> | <span data-ttu-id="8bc8d-178">select \* from c order by c._ts desc</span><span class="sxs-lookup"><span data-stu-id="8bc8d-178">select \* from c order by c._ts desc</span></span> | <span data-ttu-id="8bc8d-179">Hämta dokument, de senaste dokumenten först.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-179">Get documents, latest documents first.</span></span> |
    | <span data-ttu-id="8bc8d-180">**Azure Cosmos DB-kontoanslutning**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-180">**Azure Cosmos DB account connection**</span></span> | <span data-ttu-id="8bc8d-181">Välj den befintliga anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-181">Select existing the connection string.</span></span> |  |

1. <span data-ttu-id="8bc8d-182">Skapa indatabindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-182">Click **Save** to create the input binding.</span></span>

1. <span data-ttu-id="8bc8d-183">**C#**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-183">**C#**</span></span>

    <span data-ttu-id="8bc8d-184">Klicka på funktionsnamnet för att öppna kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-184">Click the function name to open the code window.</span></span> <span data-ttu-id="8bc8d-185">Byt ut hela **run.csx**-filen med innehållet i [ **/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span><span class="sxs-lookup"><span data-stu-id="8bc8d-185">Replace all of the **run.csx** file with the content in the [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx) file.</span></span>

1. <span data-ttu-id="8bc8d-186">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="8bc8d-186">**JavaScript**</span></span>

    <span data-ttu-id="8bc8d-187">Klicka på funktionsnamnet för att öppna kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-187">Click the function name to open the code window.</span></span> <span data-ttu-id="8bc8d-188">Byt ut hela filen **index.js**-filen med innehållet i [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span><span class="sxs-lookup"><span data-stu-id="8bc8d-188">Replace all of the **index.js** file with the content in the [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js) file.</span></span>

1. <span data-ttu-id="8bc8d-189">Klicka på **Loggar** under kodfönstret för att expandera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-189">Click **Logs** below the code window to expand the Logs panel.</span></span>

1. <span data-ttu-id="8bc8d-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-190">Click **Save**.</span></span> <span data-ttu-id="8bc8d-191">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-191">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="8bc8d-192">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="8bc8d-192">Test the application</span></span>

1. <span data-ttu-id="8bc8d-193">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-193">Open the application in a browser.</span></span> <span data-ttu-id="8bc8d-194">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-194">Select an image file and upload it.</span></span>

1. <span data-ttu-id="8bc8d-195">Efter några sekunder visas miniatyrbilden för den nya bilden på sidan.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-195">After a few seconds, the thumbnail of the new image appears on the page.</span></span>

1. <span data-ttu-id="8bc8d-196">Använd rutan **Sök** på Azure-portalen för att söka efter ditt Azure Cosmos DB-konto efter namn.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-196">In the Azure portal, use the **Search** box to search for your Azure Cosmos DB account by name.</span></span> <span data-ttu-id="8bc8d-197">Klicka på namnet för att öppna kontot.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-197">Click on the name to open the account.</span></span>

1. <span data-ttu-id="8bc8d-198">Klicka på **Datautforskaren** till vänster för att hitta samlingar och dokument.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-198">Click **Data Explorer** on the left to browse collections and documents.</span></span>

1. <span data-ttu-id="8bc8d-199">Välj samlingen **images** under databasen **imagesdb**.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-199">Under the **imagesdb** database, select the **images** collection.</span></span>

1. <span data-ttu-id="8bc8d-200">Kontrollera att ett dokument har skapats för den uppladdade bilden.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-200">Confirm that a document was created for the uploaded image.</span></span>

    ![Datautforskaren visar ett dokument för en uppladdad bild](../media/4-data-explorer.png)



## <a name="summary"></a><span data-ttu-id="8bc8d-202">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8bc8d-202">Summary</span></span>

<span data-ttu-id="8bc8d-203">I den här delen har du lärt dig hur du skapar ett konto, en databas och en samling i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-203">In this unit, you learned how to create an Azure Cosmos DB account, database, and collection.</span></span> <span data-ttu-id="8bc8d-204">Du har också lärt dig hur du använder Azure Cosmos DB-bindningar för att spara och hämta bildmetadata i Azure Cosmos DB-samlingen.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-204">You also learned how to use the Azure Cosmos DB bindings to save and retrieve image metadata in the Azure Cosmos DB collection.</span></span> <span data-ttu-id="8bc8d-205">I nästa avsnitt lär du dig hur du automatiskt genererar en bildtext för varje uppladdad bild med hjälp av Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="8bc8d-205">Next, you learn how to automatically generate a caption for each uploaded image using Microsoft Cognitive Services.</span></span>