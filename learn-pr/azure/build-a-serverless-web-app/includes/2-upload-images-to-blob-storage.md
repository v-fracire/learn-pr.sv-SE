<span data-ttu-id="2f945-101">Programmet som du skapar är ett fotogalleri.</span><span class="sxs-lookup"><span data-stu-id="2f945-101">The application you're building is a photo gallery.</span></span> <span data-ttu-id="2f945-102">Programmet använder JavaScript på klientsidan för att anropa API:er för uppladdning och visning av bilder.</span><span class="sxs-lookup"><span data-stu-id="2f945-102">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="2f945-103">I den här övningen skapar du ett API med hjälp av en serverlös funktion som genererar en tidsbegränsad URL för uppladdning av en bild.</span><span class="sxs-lookup"><span data-stu-id="2f945-103">In this unit, you will create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="2f945-104">Webbprogrammet använder denna URL för att ladda upp en avbildning till Blob Storage med hjälp av [REST-API:et för Blob Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span><span class="sxs-lookup"><span data-stu-id="2f945-104">The web application uses this URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="2f945-105">Skapa en Blob Storage-container för bilder</span><span class="sxs-lookup"><span data-stu-id="2f945-105">Create a Blob storage container for images</span></span>

<span data-ttu-id="2f945-106">Programmet kräver en separat lagringscontainer för att ladda upp och lagra bilder.</span><span class="sxs-lookup"><span data-stu-id="2f945-106">The application requires a separate storage container to upload and host images.</span></span>

<span data-ttu-id="2f945-107">Skapa en ny container med namnet **images** (bilder) i ditt lagringskonto med offentlig åtkomst till alla blobbar.</span><span class="sxs-lookup"><span data-stu-id="2f945-107">Create a new container in your Storage account named **images** in your Storage account with public access to all blobs.</span></span>

```azurecli
az storage container create \
    -n images \
    --account-name <storage account name> \
    --public-access blob
```

## <a name="create-a-function-app"></a><span data-ttu-id="2f945-108">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="2f945-108">Create a function app</span></span>

<span data-ttu-id="2f945-109">Med tjänsten Azure Functions kan du köra serverlösa funktioner.</span><span class="sxs-lookup"><span data-stu-id="2f945-109">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="2f945-110">En serverlös funktion kan utlösas (anropas) av händelser såsom en HTTP-begäran eller när en blobb skapas i en lagringscontainer.</span><span class="sxs-lookup"><span data-stu-id="2f945-110">A serverless function can be triggered (called) by events, such as an HTTP request, or when a blob is created in a storage container.</span></span>

<span data-ttu-id="2f945-111">En funktionsapp är en container för en eller flera serverlösa funktioner.</span><span class="sxs-lookup"><span data-stu-id="2f945-111">A function app is a container for one or more serverless functions.</span></span>

<span data-ttu-id="2f945-112">Skapa en ny funktionsapp med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="2f945-112">Create a new function app with a unique name.</span></span> <span data-ttu-id="2f945-113">Ett lagringskonto krävs för funktionsappar.</span><span class="sxs-lookup"><span data-stu-id="2f945-113">Function apps require a Storage account.</span></span> <span data-ttu-id="2f945-114">I den här övningen använder du det befintliga lagringskonto som du skapade i den senaste enheten.</span><span class="sxs-lookup"><span data-stu-id="2f945-114">In this unit, you will use the existing storage account you created in the last unit.</span></span>

```azurecli
az functionapp create \
    -n <function app name> \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -s <storage account name> \
    --consumption-plan-location $(az group show -n <rgn>[Sandbox resource group name]</rgn> --query location --output tsv)
```

## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="2f945-115">Skapa en HTTP-utlöst serverlös funktion</span><span class="sxs-lookup"><span data-stu-id="2f945-115">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="2f945-116">Webbappen för fotogalleriet skickar en HTTP-begäran till den serverlösa funktionen för att generera en tidsbegränsad URL för säker uppladdning av en bild till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2f945-116">To securely upload an image to Blob storage, the photo gallery web app makes an HTTP request to the serverless function to generate a time-limited URL.</span></span> <span data-ttu-id="2f945-117">Funktionen utlöses av en HTTP-begäran och använder Azure Storage SDK för att generera och returnera den säkra URL:en.</span><span class="sxs-lookup"><span data-stu-id="2f945-117">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="2f945-118">Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="2f945-118">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="2f945-119">Sök efter funktionsappen som du nyss skapade med hjälp av **Sökrutan**.</span><span class="sxs-lookup"><span data-stu-id="2f945-119">Search for the function app you just created using the **Search** box.</span></span> <span data-ttu-id="2f945-120">Klicka på resultatet för att öppna det.</span><span class="sxs-lookup"><span data-stu-id="2f945-120">Click on the result to open it.</span></span>

    ![Öppna funktionsappen](../media/2-search-function-app.png)

1. <span data-ttu-id="2f945-122">I funktionsappfönstrets vänstra navigeringsfönster pekar du på **Functions** och klickar på plustecknet (+) för att skapa en ny serverlös funktion.</span><span class="sxs-lookup"><span data-stu-id="2f945-122">In the left navigation of the Function Apps window, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span>

    ![Skapa en ny funktion](../media/2-new-function.png)

1. <span data-ttu-id="2f945-124">Klicka på **Anpassad funktion** för att visa en lista med funktionsmallar.</span><span class="sxs-lookup"><span data-stu-id="2f945-124">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="2f945-125">Leta rätt på mallen **HttpTrigger** och klicka på C# eller JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2f945-125">Find the **HttpTrigger** template and click C# or JavaScript.</span></span>

1. <span data-ttu-id="2f945-126">Använd dessa värden för att skapa en funktion som genererar en blobbuppladdnings-URL:</span><span class="sxs-lookup"><span data-stu-id="2f945-126">Use the following values to create a function that generates a blob upload URL:</span></span>

    | <span data-ttu-id="2f945-127">Inställning</span><span class="sxs-lookup"><span data-stu-id="2f945-127">Setting</span></span>      |  <span data-ttu-id="2f945-128">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="2f945-128">Suggested value</span></span>   | <span data-ttu-id="2f945-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f945-129">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="2f945-130">**Språk**</span><span class="sxs-lookup"><span data-stu-id="2f945-130">**Language**</span></span> | <span data-ttu-id="2f945-131">C# eller JavaScript</span><span class="sxs-lookup"><span data-stu-id="2f945-131">C# or JavaScript</span></span> | <span data-ttu-id="2f945-132">Välj det språk som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="2f945-132">Select the language that you want to use.</span></span> |
    | <span data-ttu-id="2f945-133">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="2f945-133">**Name your function**</span></span> | <span data-ttu-id="2f945-134">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="2f945-134">GetUploadUrl</span></span> | <span data-ttu-id="2f945-135">Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen.</span><span class="sxs-lookup"><span data-stu-id="2f945-135">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="2f945-136">**Auktoriseringsnivå**</span><span class="sxs-lookup"><span data-stu-id="2f945-136">**Authorization level**</span></span> | <span data-ttu-id="2f945-137">Anonym</span><span class="sxs-lookup"><span data-stu-id="2f945-137">Anonymous</span></span> | <span data-ttu-id="2f945-138">Gör funktionen offentligt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="2f945-138">Allows the function to be accessed publicly.</span></span> |

    ![Ange inställningarna för en ny HTTP-utlöst funktion](../media/2-new-function-httptrigger.png)

1. <span data-ttu-id="2f945-140">Klicka på **Skapa** för att skapa funktionen.</span><span class="sxs-lookup"><span data-stu-id="2f945-140">Click **Create** to create the function.</span></span>

<span data-ttu-id="2f945-141">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="2f945-141">::: zone pivot="csharp"</span></span>

8. <span data-ttu-id="2f945-142">När funktionens källkod visas ersätter du allt innehåll i **run.csx**-filen med innehållet i filen [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span><span class="sxs-lookup"><span data-stu-id="2f945-142">When the function's source code appears, replace all of the content in the **run.csx** file with the content in the [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) file.</span></span>

1. <span data-ttu-id="2f945-143">Klicka på **Loggar** under kodfönstret för att expanera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f945-143">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="2f945-144">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2f945-144">Click **Save**.</span></span> <span data-ttu-id="2f945-145">Granska loggpanelen för att bekräfta att funktionen har kompilerats.</span><span class="sxs-lookup"><span data-stu-id="2f945-145">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="2f945-146">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2f945-146">::: zone-end</span></span>

<span data-ttu-id="2f945-147">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="2f945-147">::: zone pivot="javascript"</span></span>

8. <span data-ttu-id="2f945-148">Den här funktionen kräver `azure-storage`-paketet från npm.</span><span class="sxs-lookup"><span data-stu-id="2f945-148">This function requires the `azure-storage` package from npm.</span></span> <span data-ttu-id="2f945-149">Paketet genererar den SAS-token (signatur för delad åtkomst) som krävs för att skapa den säkra URL:en.</span><span class="sxs-lookup"><span data-stu-id="2f945-149">The package generates the shared access signature (SAS) token that's required to build the secure URL.</span></span> <span data-ttu-id="2f945-150">Installera npm-paketet genom att klicka på Functions-appen i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="2f945-150">To install the npm package, click on the Functions app on the left navigation and click **Platform features**.</span></span>

1. <span data-ttu-id="2f945-151">Öppna ett konsolfönster genom att klicka på **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="2f945-151">Click **Console** to reveal a console window.</span></span>

    ![Öppna ett konsolfönster](../media/2-open-console.jpg)

1. <span data-ttu-id="2f945-153">Kontrollera att den aktuella katalogen är **d:\home\site\wwwroot** genom att köra kommandot `cd d:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="2f945-153">Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

1. <span data-ttu-id="2f945-154">Kör kommandot `npm init -y` för att skapa en tom **package.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="2f945-154">Run the command `npm init -y` to create an empty **package.json** file.</span></span>

1. <span data-ttu-id="2f945-155">Kör kommandot `npm install --save azure-storage` i konsolen för att installera paketet.</span><span class="sxs-lookup"><span data-stu-id="2f945-155">To install the package, run the command `npm install --save azure-storage` in the console.</span></span> <span data-ttu-id="2f945-156">Spara paketet som **package.json**.</span><span class="sxs-lookup"><span data-stu-id="2f945-156">Save the package as **package.json**.</span></span> <span data-ttu-id="2f945-157">Det kan ta några minuter att slutföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="2f945-157">It may take a few minutes to complete the operation.</span></span>

1. <span data-ttu-id="2f945-158">Klicka på funktionen (**GetUploadUrl**) i det vänstra navigeringsfönstret för att visa funktionen.</span><span class="sxs-lookup"><span data-stu-id="2f945-158">Click on the function (**GetUploadUrl**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="2f945-159">Byt ut hela innehållet i filen **index.js** med innehållet i filen [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span><span class="sxs-lookup"><span data-stu-id="2f945-159">Replace all of the content in the **index.js** file with the content in the [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) file.</span></span>

    ![Innehållet i index.js efter uppdatering](../media/2-paste-js.jpg)

1. <span data-ttu-id="2f945-161">Klicka på **Loggar** under kodfönstret för att expanera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f945-161">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="2f945-162">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2f945-162">Click **Save**.</span></span> <span data-ttu-id="2f945-163">Granska loggpanelen för att bekräfta att funktionen har kompilerats.</span><span class="sxs-lookup"><span data-stu-id="2f945-163">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="2f945-164">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2f945-164">::: zone-end</span></span>

<span data-ttu-id="2f945-165">Funktionen genererar en SAS-URL (signatur för delad åtkomst) som används för att ladda upp en fil till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2f945-165">The function generates a shared access signature (SAS) URL that's used to upload a file to Blob storage.</span></span> <span data-ttu-id="2f945-166">SAS-URL:en är giltig under en kort tid och tillåter bara uppladdning av en enda fil.</span><span class="sxs-lookup"><span data-stu-id="2f945-166">The SAS URL is valid for a short time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="2f945-167">Läs dokumentationen för Blob Storage om du vill veta mer om [hur du använder signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="2f945-167">Consult the Blob storage documentation for more information on [how to use shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="2f945-168">Lägg till en miljövariabel för anslutningssträngen för lagring</span><span class="sxs-lookup"><span data-stu-id="2f945-168">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="2f945-169">Funktionen som du har skapat behöver en anslutningssträng för lagringskontot för att kunna generera SAS-URL:en.</span><span class="sxs-lookup"><span data-stu-id="2f945-169">The function that you created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="2f945-170">I stället för att hårdkoda anslutningssträngen i funktionens kod kan du lagra den som en programinställning.</span><span class="sxs-lookup"><span data-stu-id="2f945-170">Instead of hardcoding the connection string in the function body, it can be stored as an application setting.</span></span> <span data-ttu-id="2f945-171">Alla funktioner i funktionsappen kan komma åt programinställningar som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="2f945-171">Application settings are accessible as environment variables by all functions in the function app.</span></span>

1. <span data-ttu-id="2f945-172">Hämta anslutningssträngen för lagringskontot från Cloud Shell och spara den i en bash-variabel med namnet **STORAGE_CONNECTION_STRING**.</span><span class="sxs-lookup"><span data-stu-id="2f945-172">In Cloud Shell, query the Storage account connection string and save it to a Bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="2f945-173">Bekräfta att variabeln är korrekt angiven.</span><span class="sxs-lookup"><span data-stu-id="2f945-173">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="2f945-174">Skapa en ny programinställning med namnet **AZURE_STORAGE_CONNECTION_STRING** genom att använda värdet som sparades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="2f945-174">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set \
        -n <function app name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING \
        -o table
    ```

    <span data-ttu-id="2f945-175">Bekräfta att kommandots utdata innehåller den nya programinställningen med rätt värde.</span><span class="sxs-lookup"><span data-stu-id="2f945-175">Confirm that the command's output contains the new application setting with the correct value.</span></span>

## <a name="test-the-serverless-function"></a><span data-ttu-id="2f945-176">Testa den serverlösa funktionen</span><span class="sxs-lookup"><span data-stu-id="2f945-176">Test the serverless function</span></span>

<span data-ttu-id="2f945-177">Förutom att skapa och redigera funktioner innehåller Azure Portal även ett inbyggt verktyg för att testa funktioner.</span><span class="sxs-lookup"><span data-stu-id="2f945-177">In addition to creating and editing functions, the Azure portal also provides a built-in tool for testing functions.</span></span>

1. <span data-ttu-id="2f945-178">Testa den serverlösa HTTP-funktionen genom att klicka på fliken **Testa** till höger om kodfönstret för att expandera testpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f945-178">To test the HTTP serverless function, on the right of the code window, click on the **Test** tab to expand the test panel.</span></span>

1. <span data-ttu-id="2f945-179">Ändra **Http-metoden** till **GET**.</span><span class="sxs-lookup"><span data-stu-id="2f945-179">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="2f945-180">Under **Fråga** klickar du på **Lägg till parameter** och lägger till följande parameter:</span><span class="sxs-lookup"><span data-stu-id="2f945-180">Under **Query**, click **Add parameter** and add the following parameter:</span></span>

    | <span data-ttu-id="2f945-181">Namn</span><span class="sxs-lookup"><span data-stu-id="2f945-181">Name</span></span>      |  <span data-ttu-id="2f945-182">Värde</span><span class="sxs-lookup"><span data-stu-id="2f945-182">Value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="2f945-183">**filename**</span><span class="sxs-lookup"><span data-stu-id="2f945-183">**filename**</span></span> | <span data-ttu-id="2f945-184">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="2f945-184">image1.jpg</span></span> |

1. <span data-ttu-id="2f945-185">Klicka på **Kör** på testpanelen för att skicka en HTTP-begäran till funktionen.</span><span class="sxs-lookup"><span data-stu-id="2f945-185">In the test panel, click **Run** to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="2f945-186">Funktionen returnerar en uppladdnings-URL.</span><span class="sxs-lookup"><span data-stu-id="2f945-186">The function returns an upload URL in the output.</span></span> <span data-ttu-id="2f945-187">Funktionskörningen visas på loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f945-187">The function execution appears in the Logs panel.</span></span>

    ![Loggar som visar att funktionen körts utan problem](../media/2-test-function.png)

## <a name="configure-cors-in-the-function-app"></a><span data-ttu-id="2f945-189">Konfigurera CORS i funktionsappen</span><span class="sxs-lookup"><span data-stu-id="2f945-189">Configure CORS in the function app</span></span>

<span data-ttu-id="2f945-190">Eftersom funktionens klientdel finns i Blob Storage har den ett annat domännamn än funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="2f945-190">Because the function front end is hosted in Blob storage, it has a different domain name than the function app.</span></span> <span data-ttu-id="2f945-191">För att JavaScript-koden på klientsidan ska kunna anropa funktionen som du har skapat måste funktionsappen konfigureras för CORS (Cross-Origin Resource Sharing).</span><span class="sxs-lookup"><span data-stu-id="2f945-191">For the client-side JavaScript to successfully call the function that you created, the function app has to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="2f945-192">Klicka på namnet på funktionsappen i det vänstra navigeringsfältet i Funktionsapp-fönstret.</span><span class="sxs-lookup"><span data-stu-id="2f945-192">In the left navigation of the Function Apps window, click on the name of your function app.</span></span>

1. <span data-ttu-id="2f945-193">Visa en lista med avancerade funktioner genom att klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="2f945-193">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="2f945-194">Klicka på **CORS** under **API**.</span><span class="sxs-lookup"><span data-stu-id="2f945-194">Under **API**, click **CORS**.</span></span>

    ![Välj CORS](../media/2-open-cors.jpg)

1. <span data-ttu-id="2f945-196">Lägg till tillåtna ursprung URL:en för den webbplats som du skapade i föregående enhet och utelämna det avslutande snedstrecket (/).</span><span class="sxs-lookup"><span data-stu-id="2f945-196">Add an allow origin for the URL of your web site you created in the previous unit and omit the trailing slash (/).</span></span> <span data-ttu-id="2f945-197">Till exempel: `https://firstserverlessweb.z4.web.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="2f945-197">For example: `https://firstserverlessweb.z4.web.core.windows.net`.</span></span>

    ![CORS-inställningar som visar att den serverlösa webbappens URL har lagts till](../media/2-add-cors.png)

1. <span data-ttu-id="2f945-199">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2f945-199">Click **Save**.</span></span>

<span data-ttu-id="2f945-200">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="2f945-200">::: zone pivot="javascript"</span></span>

6. <span data-ttu-id="2f945-201">Medan du är kvar i Azure-portalen går du till Funktionsapp-fönstret och klickar på namnet på din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="2f945-201">Still in the Azure portal, navigate to the Function Apps window and click on the name of your function app.</span></span> <span data-ttu-id="2f945-202">Välj fliken **Översikt**. Klicka på **Starta om** för att kontrollera om ändringarna för CORS har tillämpats.</span><span class="sxs-lookup"><span data-stu-id="2f945-202">Select the **Overview** tab. Click **Restart** to make sure that the changes for CORS take effect.</span></span>

<span data-ttu-id="2f945-203">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2f945-203">::: zone-end</span></span>

<span data-ttu-id="2f945-204">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="2f945-204">::: zone pivot="csharp"</span></span>

6. <span data-ttu-id="2f945-205">Gå tillbaka till funktionen `GetUploadUrl` och välj fliken **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="2f945-205">Navigate back to the `GetUploadUrl` function and select the **Integrate** tab.</span></span>

1. <span data-ttu-id="2f945-206">Under **Selected HTTP methods** (Valda HTTP-metoder) väljer du **OPTIONS** (Alternativ).</span><span class="sxs-lookup"><span data-stu-id="2f945-206">Under **Selected HTTP methods**, select **OPTIONS**.</span></span>

    <span data-ttu-id="2f945-207">**GET**, **POST** och **OPTIONS** bör vara markerade.</span><span class="sxs-lookup"><span data-stu-id="2f945-207">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="2f945-208">CORS använder **OPTIONS**-metoden, som inte är markerad som standard för C#-funktioner.</span><span class="sxs-lookup"><span data-stu-id="2f945-208">CORS uses the **OPTIONS** method, which isn't selected by default for C# functions.</span></span>  

1. <span data-ttu-id="2f945-209">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2f945-209">Click **Save**.</span></span>

1. <span data-ttu-id="2f945-210">Medan du är kvar i Azure-portalen går du till Funktionsapp-fönstret och klickar på namnet på din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="2f945-210">Still in the Azure portal, navigate to the Function Apps window and click on the name of your function app.</span></span> <span data-ttu-id="2f945-211">Välj fliken **Översikt**. Klicka på **Starta om** för att kontrollera om ändringarna för CORS har tillämpats.</span><span class="sxs-lookup"><span data-stu-id="2f945-211">Select the **Overview** tab. Click **Restart** to make sure that the changes for CORS take effect.</span></span>

<span data-ttu-id="2f945-212">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2f945-212">::: zone-end</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="2f945-213">Konfigurera CORS i lagringskontot</span><span class="sxs-lookup"><span data-stu-id="2f945-213">Configure CORS in the Storage account</span></span>

<span data-ttu-id="2f945-214">Eftersom funktionsappen även gör JavaScript-anrop till Blob Storage för filuppladdning måste du även konfigurera lagringskontot för CORS.</span><span class="sxs-lookup"><span data-stu-id="2f945-214">Because the function app also makes client-side JavaScript calls to Blob storage to upload files, you have to configure the Storage account for CORS.</span></span>

- <span data-ttu-id="2f945-215">Kör följande kommando för att tillåta att alla ursprung laddar upp filer till lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="2f945-215">Run the following command to allow all origins to upload files to the Storage account:</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```

## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="2f945-216">Ändra webbappen för att ladda upp bilder</span><span class="sxs-lookup"><span data-stu-id="2f945-216">Modify the web app to upload images</span></span>

<span data-ttu-id="2f945-217">Webbappen hämtar inställningar från en fil med namnet **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="2f945-217">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="2f945-218">Skapa filen med hjälp av Cloud Shell i de följande stegen.</span><span class="sxs-lookup"><span data-stu-id="2f945-218">In the following steps, you create the file using Cloud Shell.</span></span> <span data-ttu-id="2f945-219">Ange `window.apiBaseUrl` till URL:en för funktionsappen och `window.blobBaseUrl` till URL:en för Azure Blob Storage-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="2f945-219">You set `window.apiBaseUrl` to the URL of the function app, and `window.blobBaseUrl` to the URL of the Azure Blob storage endpoint.</span></span>

1. <span data-ttu-id="2f945-220">Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella katalogen.</span><span class="sxs-lookup"><span data-stu-id="2f945-220">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```bash
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="2f945-221">Öppna Cloud Shell-redigeraren i den aktuella katalogen genom att skriva kommandot `code .`, inklusive punkten.</span><span class="sxs-lookup"><span data-stu-id="2f945-221">Open the Cloud Shell Editor in the current directory by typing the command `code .`, including the dot.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="2f945-222">I Cloud Shell-fönstret under redigeraren kör du en fråga mot funktionsappens URL.</span><span class="sxs-lookup"><span data-stu-id="2f945-222">In the Cloud Shell window below the editor, query the function app's URL.</span></span>

    ```azurecli
    echo "https://"$(az functionapp show -n <function app name> -g <rgn>[Sandbox resource group name]</rgn> --query "defaultHostName" --output tsv)
    ```

1. <span data-ttu-id="2f945-223">Lägg till följande rad i redigeringsfönstret med hjälp av funktionsappens URL som du hämtade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="2f945-223">Add the following line into the editor window, using the function app URL you retrieved in the previous step.</span></span>

    ```bash
    window.apiBaseUrl = '<function app url>'
    ```

1. <span data-ttu-id="2f945-224">I Cloud Shell-fönstret under redigeraren kör du en fråga mot Azure Cloud Shell-slutpunktens URL.</span><span class="sxs-lookup"><span data-stu-id="2f945-224">In the Cloud Shell window below the editor, query the Azure Blob Storage endpoint URL.</span></span>

    ```azurecli
    echo $(az storage account show -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

1. <span data-ttu-id="2f945-225">Lägg till en andra rad i redigeringsfönstret med hjälp av Storage-slutpunktens URL som du hämtade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="2f945-225">Append a second line into the editor window, using the Storage endpoint URL you retrieved in the previous step.</span></span>

    ```bash
    window.blobBaseUrl = '<blob storage endpoint url>'
    ```

1. <span data-ttu-id="2f945-226">Spara filen som **settings.js** och stäng redigeraren.</span><span class="sxs-lookup"><span data-stu-id="2f945-226">Save the file as **settings.js** and close the editor.</span></span>

1. <span data-ttu-id="2f945-227">Bekräfta att filen skrivits korrekt och att den nu innehåller två rader.</span><span class="sxs-lookup"><span data-stu-id="2f945-227">Confirm the file was successfully written and it now contains 2 lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="2f945-228">Ladda upp filen till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2f945-228">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload \
        -c \$web \
        --account-name <storage account name> \
        -f settings.js \
        -n settings.js
    ```

## <a name="test-the-web-application"></a><span data-ttu-id="2f945-229">Testa webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="2f945-229">Test the web application</span></span>

<span data-ttu-id="2f945-230">Nu kan galleriprogrammet ladda upp en bild till Blob Storage, men den kan inte visa bilder än.</span><span class="sxs-lookup"><span data-stu-id="2f945-230">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="2f945-231">Den kommer att försöka anropa en `GetImages`-funktion som inte finns ännu eftersom du skapar den i en senare modul.</span><span class="sxs-lookup"><span data-stu-id="2f945-231">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="2f945-232">Anropet misslyckas och sidan verkar ha fastnat på ”Analyserar...”, men bilden du valt kommer att laddas upp korrekt.</span><span class="sxs-lookup"><span data-stu-id="2f945-232">The call will fail and the web page will appear to be stuck on "Analyzing...," but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="2f945-233">Du kan kontrollera att en bild har laddats upp genom att kontrollera innehållet i containern **images** på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="2f945-233">You can verify that an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="2f945-234">Gå till programmet i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="2f945-234">In a browser window, browse to the application.</span></span> <span data-ttu-id="2f945-235">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="2f945-235">Select an image file and upload it.</span></span> <span data-ttu-id="2f945-236">Uppladdningen slutförs, men eftersom vi inte har lagt till möjligheten att visa bilder ännu så visar inte appen det uppladdade fotot.</span><span class="sxs-lookup"><span data-stu-id="2f945-236">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="2f945-237">(Sidan verkar ha fastnat på ”Analyserar bild...” Du åtgärdar detta senare.)</span><span class="sxs-lookup"><span data-stu-id="2f945-237">(The web page appears to be stuck on "Analyzing image..." You'll fix that later.)</span></span>

1. <span data-ttu-id="2f945-238">Kontrollera i Cloud Shell att bilden har laddats upp till containern **images**.</span><span class="sxs-lookup"><span data-stu-id="2f945-238">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list \
        --account-name <storage account name> \
        -c images \
        -o table
    ```

1. <span data-ttu-id="2f945-239">Ta bort alla filer i containern **images** innan du går vidare till nästa enhet.</span><span class="sxs-lookup"><span data-stu-id="2f945-239">Before moving on to the next unit, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch \
        -s images \
        --account-name <storage account name>
    ```

## <a name="summary"></a><span data-ttu-id="2f945-240">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2f945-240">Summary</span></span>

<span data-ttu-id="2f945-241">I den här enheten har du skapat en Azure Functions-app och lärt dig hur du använder en serverlös funktion för att ladda upp bilder till Blob Storage från ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="2f945-241">In this unit, you created an Azure Functions app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="2f945-242">I nästa avsnitt lär du dig att skapa miniatyrbilder för de uppladdade bilderna med hjälp av en blobbutlöst serverlös funktion.</span><span class="sxs-lookup"><span data-stu-id="2f945-242">Next, you will learn how to create thumbnails for the uploaded images using a blob-triggered serverless function.</span></span>