<span data-ttu-id="7baf7-101">Azure Cosmos DB är Microsofts serverlösa, globalt distribuerade databas för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="7baf7-101">Azure Cosmos DB is Microsoft's serverless, globally distributed, multi-model database.</span></span> <span data-ttu-id="7baf7-102">I den här modulen lär du dig hur du använder Azure Functions för att lagra och hämta bildmetadata som JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7baf7-102">In this module, you learn how to use Azure Functions to store and retrieve image metadata as JSON documents in Azure Cosmos DB.</span></span>

## <a name="create-an-azure-cosmos-db-account-database-and-collection"></a><span data-ttu-id="7baf7-103">Lär dig att skapa ett Azure Cosmos DB-konto, en databas och en samling</span><span class="sxs-lookup"><span data-stu-id="7baf7-103">Create an Azure Cosmos DB account, database, and collection</span></span>

<span data-ttu-id="7baf7-104">Ett Azure Cosmos DB-konto är en Azure-resurs som innehåller Azure Cosmos DB-databaser.</span><span class="sxs-lookup"><span data-stu-id="7baf7-104">An Azure Cosmos DB account is an Azure resource that contains Azure Cosmos DB databases.</span></span>

1. <span data-ttu-id="7baf7-105">Skapa ett Azure Cosmos DB-konto med ett unikt namn i samma resursgrupp som de andra resurserna i den här modulen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-105">Create an Azure Cosmos DB account with a unique name in the same resource group as the other resources in this module.</span></span> <span data-ttu-id="7baf7-106">Det kan ta ett par minuter innan kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7baf7-106">This command may take a minute or two to complete.</span></span>

    ```azurecli
    az cosmosdb create \
        -g <rgn>[Sandbox resource group name]</rgn> \
        -n <cosmos db account name>
    ```

1. <span data-ttu-id="7baf7-107">När Azure Cosmos DB-kontot har skapats skapar du en ny databas med namnet **imagesdb** i kontot.</span><span class="sxs-lookup"><span data-stu-id="7baf7-107">After the Azure Cosmos DB account is created, create a new database named **imagesdb** in the account.</span></span>

    ```azurecli
    az cosmosdb database create \
        -g <rgn>[Sandbox resource group name]</rgn> \
        -n <cosmos db account name> \
        --db-name imagesdb
    ```

1. <span data-ttu-id="7baf7-108">När databasen har skapats skapar du en ny samling med namnet **images** i databasen med ett dataflöde på 400 enheter för programbegäran (RU:er).</span><span class="sxs-lookup"><span data-stu-id="7baf7-108">After the database is created, create a new collection named **images** in the database with a throughput of 400 request units (RUs).</span></span>

    ```azurecli
    az cosmosdb collection create \
        -g <rgn>[Sandbox resource group name]</rgn> \
        -n <cosmos db account name> \
        --db-name imagesdb \
        --collection-name images \
        --throughput 400
    ```

## <a name="save-a-document-to-azure-cosmos-db-when-a-thumbnail-is-created"></a><span data-ttu-id="7baf7-109">Spara ett dokument till Azure Cosmos DB när en miniatyrbild skapas</span><span class="sxs-lookup"><span data-stu-id="7baf7-109">Save a document to Azure Cosmos DB when a thumbnail is created</span></span>

<span data-ttu-id="7baf7-110">Med Azure Cosmos DB-utdatabindningen kan du skapa dokument i en Azure Cosmos DB-samling från Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="7baf7-110">The Azure Cosmos DB output binding lets you create documents in an Azure Cosmos DB collection from Azure Functions.</span></span> <span data-ttu-id="7baf7-111">I följande steg ska du konfigurera en Azure Cosmos DB-utdatabindning i funktionen **ResizeImage** och ändra funktionen så att den returnerar ett dokument (objekt) som ska sparas.</span><span class="sxs-lookup"><span data-stu-id="7baf7-111">In the following steps, you configure an Azure Cosmos DB output binding in the **ResizeImage** function and modify the function to return a document (object) to be saved.</span></span>

1. <span data-ttu-id="7baf7-112">Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du har aktiverat sandbox-miljön med.</span><span class="sxs-lookup"><span data-stu-id="7baf7-112">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="7baf7-113">Öppna funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-113">Open the function app.</span></span>

1. <span data-ttu-id="7baf7-114">Expandera funktionen **ResizeImage** i det vänstra navigeringsfönstret och välj **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-114">In the left navigation, expand the **ResizeImage** function, and then select **Integrate**.</span></span>

1. <span data-ttu-id="7baf7-115">Klicka på **Nya utdata** under **Utdata**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-115">Under **Outputs**, click **New Output**.</span></span>

1. <span data-ttu-id="7baf7-116">Leta upp och markera **Azure Cosmos DB**-objektet.</span><span class="sxs-lookup"><span data-stu-id="7baf7-116">Find the **Azure Cosmos DB** item and select it.</span></span> <span data-ttu-id="7baf7-117">Klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-117">Then click **Select**.</span></span>

    ![Välj Nya utdata](../media/4-new-output.jpg)

1. <span data-ttu-id="7baf7-119">Fyll i fälten under **Azure Cosmos DB-utdata** med följande värden.</span><span class="sxs-lookup"><span data-stu-id="7baf7-119">Fill out the fields under **Azure Cosmos DB output** with the following values.</span></span>

    | <span data-ttu-id="7baf7-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="7baf7-120">Setting</span></span>      |  <span data-ttu-id="7baf7-121">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="7baf7-121">Suggested value</span></span>   | <span data-ttu-id="7baf7-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7baf7-122">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="7baf7-123">**Dokumentparameternamn**</span><span class="sxs-lookup"><span data-stu-id="7baf7-123">**Document parameter name**</span></span> | <span data-ttu-id="7baf7-124">Välj **Använd funktionsreturvärde**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-124">Select **Use function return value**.</span></span> | <span data-ttu-id="7baf7-125">Värdet i textrutan ställs automatiskt in till **$return**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-125">The value in the box is automatically set to **$return**.</span></span> |
    | <span data-ttu-id="7baf7-126">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="7baf7-126">**Database name**</span></span> | <span data-ttu-id="7baf7-127">imagesdb</span><span class="sxs-lookup"><span data-stu-id="7baf7-127">imagesdb</span></span> | <span data-ttu-id="7baf7-128">Använd namnet på databasen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="7baf7-128">Use the name of the database that you created.</span></span> |
    | <span data-ttu-id="7baf7-129">**Samlingsnamn**</span><span class="sxs-lookup"><span data-stu-id="7baf7-129">**Collection name**</span></span> | <span data-ttu-id="7baf7-130">images</span><span class="sxs-lookup"><span data-stu-id="7baf7-130">images</span></span> | <span data-ttu-id="7baf7-131">Använd namnet på samlingen som du skapat.</span><span class="sxs-lookup"><span data-stu-id="7baf7-131">Use the name of the collection that you created.</span></span> |

1. <span data-ttu-id="7baf7-132">Klicka på **Ny** bredvid **Azure Cosmos DB-kontoanslutningen**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-132">Next to **Azure Cosmos DB account connection**, click **new**.</span></span> <span data-ttu-id="7baf7-133">Välj Azure Cosmos DB-kontot som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="7baf7-133">Select the Azure Cosmos DB account that you previously created.</span></span>

    ![Ange inställningar för Azure Cosmos DB-utdatabindningen](../media/4-cosmos-db-output.png)

1. <span data-ttu-id="7baf7-135">Skapa Azure Cosmos DB-utdatabindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-135">Click **Save** to create the Azure Cosmos DB output binding.</span></span>

1. <span data-ttu-id="7baf7-136">Klicka på funktionsnamnet **ResizeImage** till vänster för att öppna funktionen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-136">Click on the **ResizeImage** function name on the left to open the function.</span></span>

<span data-ttu-id="7baf7-137">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="7baf7-137">::: zone pivot="csharp"</span></span>

10. <span data-ttu-id="7baf7-138">Ändra returtypen för funktionen från **void** till **object**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-138">Close the error popup and change the return type of the function from **void** to **object**.</span></span>

1. <span data-ttu-id="7baf7-139">Returnera dokumentet som ska sparas genom att lägga till följande kodblock i slutet av funktionen:</span><span class="sxs-lookup"><span data-stu-id="7baf7-139">At the end of the function, add the following code block to return the document to be saved:</span></span>

    ```csharp
    return new {
        id = name,
        imgPath = "/images/" + name,
        thumbnailPath = "/thumbnails/" + name
    };
    ```

    ![run.csx för funktionen ResizeImages efter ändringarna](../media/4-update-function.png)

1. <span data-ttu-id="7baf7-141">Klicka på **Loggar** under kodfönstret för att expanera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-141">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="7baf7-142">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-142">Click **Save**.</span></span> <span data-ttu-id="7baf7-143">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="7baf7-143">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>

<span data-ttu-id="7baf7-144">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="7baf7-144">::: zone-end</span></span>

<span data-ttu-id="7baf7-145">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="7baf7-145">::: zone pivot="javascript"</span></span>

10. <span data-ttu-id="7baf7-146">Ändra instruktionen `context.done()` i `else`-satsen så att dokumentet returneras för att sparas i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7baf7-146">Change the `context.done()` statement in the `else` clause to return the document to be saved to Azure Cosmos DB.</span></span>

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
1. <span data-ttu-id="7baf7-147">Klicka på **Loggar** under kodfönstret för att expanera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-147">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="7baf7-148">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-148">Click **Save**.</span></span> <span data-ttu-id="7baf7-149">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="7baf7-149">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>

<span data-ttu-id="7baf7-150">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="7baf7-150">::: zone-end</span></span>

## <a name="create-a-function-to-list-images-from-azure-cosmos-db"></a><span data-ttu-id="7baf7-151">Skapa en funktion för att visa bilderna från Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7baf7-151">Create a function to list images from Azure Cosmos DB</span></span>

<span data-ttu-id="7baf7-152">Webbprogrammet kräver ett API för att hämta bildmetadata från Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7baf7-152">The web application requires an API to retrieve image metadata from Azure Cosmos DB.</span></span> <span data-ttu-id="7baf7-153">I följande steg ska du skapa en HTTP-utlöst funktion som använder en Azure Cosmos DB-indatabindning för att fråga databassamlingen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-153">In the following steps, you create an HTTP-triggered function that uses an Azure Cosmos DB input binding to query the database collection.</span></span>

1. <span data-ttu-id="7baf7-154">Peka på **Funktioner** till vänster i Funktionsapp och klicka på plustecknet (+) för att skapa en ny funktion.</span><span class="sxs-lookup"><span data-stu-id="7baf7-154">In your function app, point to **Functions** on the left and click the plus sign (+) to create a new function.</span></span>

1. <span data-ttu-id="7baf7-155">Leta upp och välj mallen **HttpTrigger**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-155">Find the **HttpTrigger** template and select it.</span></span>

1. <span data-ttu-id="7baf7-156">Använd dessa värden för att skapa en funktion som genererar en URL för att hämta bilder:</span><span class="sxs-lookup"><span data-stu-id="7baf7-156">Use these values to create a function that generates a get images URL:</span></span>

    | <span data-ttu-id="7baf7-157">Inställning</span><span class="sxs-lookup"><span data-stu-id="7baf7-157">Setting</span></span>      |  <span data-ttu-id="7baf7-158">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="7baf7-158">Suggested value</span></span>   | <span data-ttu-id="7baf7-159">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7baf7-159">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="7baf7-160">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="7baf7-160">**Name your function**</span></span> | <span data-ttu-id="7baf7-161">GetImages</span><span class="sxs-lookup"><span data-stu-id="7baf7-161">GetImages</span></span> | <span data-ttu-id="7baf7-162">Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-162">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="7baf7-163">**Auktoriseringsnivå**</span><span class="sxs-lookup"><span data-stu-id="7baf7-163">**Authorization level**</span></span> | <span data-ttu-id="7baf7-164">Anonym</span><span class="sxs-lookup"><span data-stu-id="7baf7-164">Anonymous</span></span> | <span data-ttu-id="7baf7-165">Gör funktionen offentligt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7baf7-165">Allow the function to be accessed publicly.</span></span> |

1. <span data-ttu-id="7baf7-166">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-166">Click **Create**.</span></span>

1. <span data-ttu-id="7baf7-167">När den nya funktionen har skapats klickar du på **Integrera** under funktionens namn i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="7baf7-167">When the new function is created, click **Integrate** under the function name on the left navigation.</span></span>

1. <span data-ttu-id="7baf7-168">Klicka på **Nya indata** och välj **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-168">Click **New Input** and select **Azure Cosmos DB**.</span></span>

    ![Välj Nya indata](../media/4-new-input.jpg)

1. <span data-ttu-id="7baf7-170">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-170">Click **Select**.</span></span>

1. <span data-ttu-id="7baf7-171">Fyll i följande värden:</span><span class="sxs-lookup"><span data-stu-id="7baf7-171">Fill out the following values:</span></span>

    | <span data-ttu-id="7baf7-172">Inställning</span><span class="sxs-lookup"><span data-stu-id="7baf7-172">Setting</span></span>      |  <span data-ttu-id="7baf7-173">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="7baf7-173">Suggested value</span></span>   | <span data-ttu-id="7baf7-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7baf7-174">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="7baf7-175">**Dokumentparameternamn**</span><span class="sxs-lookup"><span data-stu-id="7baf7-175">**Document parameter name**</span></span> | <span data-ttu-id="7baf7-176">documents</span><span class="sxs-lookup"><span data-stu-id="7baf7-176">documents</span></span> | <span data-ttu-id="7baf7-177">Matchar parameternamnet i funktionen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-177">Matches the parameter name in the function.</span></span> |
    | <span data-ttu-id="7baf7-178">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="7baf7-178">**Database name**</span></span> | <span data-ttu-id="7baf7-179">imagesdb</span><span class="sxs-lookup"><span data-stu-id="7baf7-179">imagesdb</span></span> |  |
    | <span data-ttu-id="7baf7-180">**Samlingsnamn**</span><span class="sxs-lookup"><span data-stu-id="7baf7-180">**Collection name**</span></span> | <span data-ttu-id="7baf7-181">images</span><span class="sxs-lookup"><span data-stu-id="7baf7-181">images</span></span> |  |
    | <span data-ttu-id="7baf7-182">**SQL-fråga**</span><span class="sxs-lookup"><span data-stu-id="7baf7-182">**SQL query**</span></span> | <span data-ttu-id="7baf7-183">select \* from c order by c._ts desc</span><span class="sxs-lookup"><span data-stu-id="7baf7-183">select \* from c order by c._ts desc</span></span> | <span data-ttu-id="7baf7-184">Hämta dokument, de senaste dokumenten först.</span><span class="sxs-lookup"><span data-stu-id="7baf7-184">Get documents, latest documents first.</span></span> |
    | <span data-ttu-id="7baf7-185">**Azure Cosmos DB-kontoanslutning**</span><span class="sxs-lookup"><span data-stu-id="7baf7-185">**Azure Cosmos DB account connection**</span></span> | <span data-ttu-id="7baf7-186">Välj den befintliga anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="7baf7-186">Select the existing connection string.</span></span> |  |

1. <span data-ttu-id="7baf7-187">Skapa indatabindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-187">Click **Save** to create the input binding.</span></span>

<span data-ttu-id="7baf7-188">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="7baf7-188">::: zone pivot="csharp"</span></span>

10. <span data-ttu-id="7baf7-189">Klicka på funktionsnamnet för att öppna kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="7baf7-189">Click the function name to open the code window.</span></span> <span data-ttu-id="7baf7-190">Byt ut hela filen **run.csx** mot innehållet i filen [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span><span class="sxs-lookup"><span data-stu-id="7baf7-190">Replace all of the **run.csx** file with the content in the [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx) file.</span></span>

<span data-ttu-id="7baf7-191">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="7baf7-191">::: zone-end</span></span>

<span data-ttu-id="7baf7-192">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="7baf7-192">::: zone pivot="javascript"</span></span>

10. <span data-ttu-id="7baf7-193">Klicka på funktionsnamnet för att öppna kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="7baf7-193">Click the function name to open the code window.</span></span> <span data-ttu-id="7baf7-194">Byt ut hela filen **index.js** med innehållet i filen [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span><span class="sxs-lookup"><span data-stu-id="7baf7-194">Replace all of the **index.js** file with the content in the [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js) file.</span></span>

<span data-ttu-id="7baf7-195">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="7baf7-195">::: zone-end</span></span>

11. <span data-ttu-id="7baf7-196">Klicka på **Loggar** under kodfönstret för att expandera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-196">Click **Logs** below the code window to expand the Logs panel.</span></span>

1. <span data-ttu-id="7baf7-197">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-197">Click **Save**.</span></span> <span data-ttu-id="7baf7-198">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="7baf7-198">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="7baf7-199">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="7baf7-199">Test the application</span></span>

1. <span data-ttu-id="7baf7-200">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="7baf7-200">Open the application in a browser.</span></span> <span data-ttu-id="7baf7-201">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="7baf7-201">Select an image file and upload it.</span></span>

1. <span data-ttu-id="7baf7-202">Efter några sekunder visas miniatyrbilden för den nya bilden på sidan.</span><span class="sxs-lookup"><span data-stu-id="7baf7-202">After a few seconds, the thumbnail of the new image appears on the page.</span></span>

1. <span data-ttu-id="7baf7-203">Använd rutan **Sök** på Azure-portalen för att söka efter ditt Azure Cosmos DB-konto efter namn.</span><span class="sxs-lookup"><span data-stu-id="7baf7-203">In the Azure portal, use the **Search** box to search for your Azure Cosmos DB account by name.</span></span> <span data-ttu-id="7baf7-204">Klicka på namnet för att öppna kontot.</span><span class="sxs-lookup"><span data-stu-id="7baf7-204">Click on the name to open the account.</span></span>

1. <span data-ttu-id="7baf7-205">Klicka på **Datautforskaren** till vänster för att hitta samlingar och dokument.</span><span class="sxs-lookup"><span data-stu-id="7baf7-205">Click **Data Explorer** on the left to browse collections and documents.</span></span>

1. <span data-ttu-id="7baf7-206">Välj samlingen **images** under databasen **imagesdb**.</span><span class="sxs-lookup"><span data-stu-id="7baf7-206">Under the **imagesdb** database, select the **images** collection.</span></span>

1. <span data-ttu-id="7baf7-207">Kontrollera att ett dokument har skapats för den uppladdade bilden.</span><span class="sxs-lookup"><span data-stu-id="7baf7-207">Confirm that a document was created for the uploaded image.</span></span>

    ![Datautforskaren visar ett dokument för en uppladdad bild](../media/4-data-explorer.png)

## <a name="summary"></a><span data-ttu-id="7baf7-209">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7baf7-209">Summary</span></span>

<span data-ttu-id="7baf7-210">I den här delen har du lärt dig hur du skapar ett konto, en databas och en samling i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7baf7-210">In this unit, you learned how to create an Azure Cosmos DB account, database, and collection.</span></span> <span data-ttu-id="7baf7-211">Du har också lärt dig hur du använder Azure Cosmos DB-bindningar till att spara och hämta bildmetadata i Azure Cosmos DB-samlingen.</span><span class="sxs-lookup"><span data-stu-id="7baf7-211">You also learned how to use the Azure Cosmos DB bindings to save and retrieve image metadata in the Azure Cosmos DB collection.</span></span> <span data-ttu-id="7baf7-212">I nästa lektion får du lära dig att automatiskt generera en bildtext för varje uppladdad bild med hjälp av Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="7baf7-212">Next, you will learn how to automatically generate a caption for each uploaded image using Microsoft Cognitive Services.</span></span>