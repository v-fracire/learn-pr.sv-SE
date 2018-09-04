---
zone_pivot_groups: dev-lang-csharp-javascript
ms.openlocfilehash: a8e95da1016ebc0d32c3b703cd1237333a442ee2
ms.sourcegitcommit: 53f17751cda7c2ae9d388896ca0ab35501766568
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/23/2018
ms.locfileid: "42798999"
---
<span data-ttu-id="7b912-101">Programmet som du skapar är ett fotogalleri.</span><span class="sxs-lookup"><span data-stu-id="7b912-101">The application that you're building is a photo gallery.</span></span> <span data-ttu-id="7b912-102">Programmet använder JavaScript på klientsidan för att anropa API:er för uppladdning och visning av bilder.</span><span class="sxs-lookup"><span data-stu-id="7b912-102">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="7b912-103">I den här modulen skapar du ett API med hjälp av en serverlös funktion som genererar en tidsbegränsad URL för uppladdning av en bild.</span><span class="sxs-lookup"><span data-stu-id="7b912-103">In this module, you create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="7b912-104">Webbprogrammet använder den genererade URL:en för att ladda upp en avbildning till Blob Storage med hjälp av [REST-API:et för Blob Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span><span class="sxs-lookup"><span data-stu-id="7b912-104">The web application uses the generated URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="7b912-105">Skapa en Blob Storage-container för bilder</span><span class="sxs-lookup"><span data-stu-id="7b912-105">Create a blob storage container for images</span></span>

<span data-ttu-id="7b912-106">Programmet kräver en separat lagringscontainer för att ladda upp och lagra bilder.</span><span class="sxs-lookup"><span data-stu-id="7b912-106">The application requires a separate storage container to upload and host images.</span></span>

1. <span data-ttu-id="7b912-107">Kontrollera att du fortfarande är inloggad i Cloud Shell (bash).</span><span class="sxs-lookup"><span data-stu-id="7b912-107">Ensure you are still signed in to the Cloud Shell (bash).</span></span> <span data-ttu-id="7b912-108">Om inte väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.</span><span class="sxs-lookup"><span data-stu-id="7b912-108">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span>

1.  <span data-ttu-id="7b912-109">Skapa en ny container med namnet **images** (bilder) i ditt lagringskonto med offentlig åtkomst till alla blobbar.</span><span class="sxs-lookup"><span data-stu-id="7b912-109">Create a new container named **images** in your storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-function-app"></a><span data-ttu-id="7b912-110">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="7b912-110">Create an Azure Function app</span></span>

<span data-ttu-id="7b912-111">Med tjänsten Azure Functions kan du köra serverlösa funktioner.</span><span class="sxs-lookup"><span data-stu-id="7b912-111">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="7b912-112">En serverlös funktion kan utlösas (anropas) av händelser som till exempel en HTTP-begäran eller när en blobb skapas i en lagringscontainer.</span><span class="sxs-lookup"><span data-stu-id="7b912-112">A serverless function can be triggered (called) by events such as an HTTP request or when a blob is created in a storage container.</span></span>

<span data-ttu-id="7b912-113">En Azure Functions-app är en container för en eller flera serverlösa funktioner.</span><span class="sxs-lookup"><span data-stu-id="7b912-113">An Azure Function app is a container for one or more serverless functions.</span></span>

1. <span data-ttu-id="7b912-114">Skapa en ny Azure Functions-app med ett unikt namn i resursgruppen som du skapade tidigare med namnet **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="7b912-114">Create a new Azure Function app with a unique name in the resource group you created earlier named **first-serverless-app**.</span></span> <span data-ttu-id="7b912-115">Funktionsappar kräver ett lagringskonto. I den här självstudien använder du det befintliga lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="7b912-115">Function apps require a Storage account; in this tutorial, you use the existing storage account.</span></span>

    ```azurecli
    az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
    ```

## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="7b912-116">Skapa en HTTP-utlöst serverlös funktion</span><span class="sxs-lookup"><span data-stu-id="7b912-116">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="7b912-117">Webbprogrammet för fotogalleriet skickar en HTTP-begäran till den serverlösa funktionen för att generera en tidsbegränsad URL för säker uppladdning av en bild till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7b912-117">The photo gallery web application makes an HTTP request to serverless function to generate a time-limited URL to securely upload an image to Blob storage.</span></span> <span data-ttu-id="7b912-118">Funktionen utlöses av en HTTP-begäran och använder Azure Storage SDK för att generera och returnera den säkra URL:en.</span><span class="sxs-lookup"><span data-stu-id="7b912-118">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="7b912-119">När funktionsappen har skapats kan du söka efter den på Azure Portal med hjälp av sökrutan och klicka för att öppna den.</span><span class="sxs-lookup"><span data-stu-id="7b912-119">After the Function app is created, search for it in the Azure Portal using the Search box and click to open it.</span></span>

    ![Öppna funktionsappen](../images/2-search-function-app.png)

1. <span data-ttu-id="7b912-121">Hovra över **Funktioner** i det vänstra navigeringsfönstret i funktionsappens fönster och klicka på **+** för att börja skapa en ny serverlös funktion.</span><span class="sxs-lookup"><span data-stu-id="7b912-121">In the function app window's left hand navigation, hover over **Functions** and click **+** to start creating a new serverless function.</span></span>

    ![Skapa en ny funktion](../images/2-new-function.png)

1. <span data-ttu-id="7b912-123">Klicka på **Anpassad funktion** för att visa en lista med funktionsmallar.</span><span class="sxs-lookup"><span data-stu-id="7b912-123">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="7b912-124">Leta upp mallen **HttpTrigger** och klicka på önskat språk (C# eller JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7b912-124">Find the **HttpTrigger** template and click the language to use (C# or JavaScript).</span></span>

1. <span data-ttu-id="7b912-125">Använd dessa värden för att skapa en funktion som genererar en blobbuppladdnings-URL.</span><span class="sxs-lookup"><span data-stu-id="7b912-125">Use these values to create a function that generates a blob upload URL.</span></span>

    | <span data-ttu-id="7b912-126">Inställning</span><span class="sxs-lookup"><span data-stu-id="7b912-126">Setting</span></span>      |  <span data-ttu-id="7b912-127">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="7b912-127">Suggested value</span></span>   | <span data-ttu-id="7b912-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7b912-128">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="7b912-129">**Språk**</span><span class="sxs-lookup"><span data-stu-id="7b912-129">**Language**</span></span> | <span data-ttu-id="7b912-130">C# eller JavaScript</span><span class="sxs-lookup"><span data-stu-id="7b912-130">C# or JavaScript</span></span> | <span data-ttu-id="7b912-131">Välj det språk som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="7b912-131">Select the language you want to use.</span></span> |
    | <span data-ttu-id="7b912-132">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="7b912-132">**Name your function**</span></span> | <span data-ttu-id="7b912-133">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="7b912-133">GetUploadUrl</span></span> | <span data-ttu-id="7b912-134">Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen.</span><span class="sxs-lookup"><span data-stu-id="7b912-134">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="7b912-135">**Auktorisationsnivå**</span><span class="sxs-lookup"><span data-stu-id="7b912-135">**Authorization level**</span></span> | <span data-ttu-id="7b912-136">Anonym</span><span class="sxs-lookup"><span data-stu-id="7b912-136">Anonymous</span></span> | <span data-ttu-id="7b912-137">Gör funktionen offentligt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7b912-137">Allow the function to be accessed publicly.</span></span> |

    ![Ange inställningarna för en ny HTTP-utlöst funktion](../images/2-new-function-httptrigger.png)

1. <span data-ttu-id="7b912-139">Klicka på **Skapa** för att skapa funktionen.</span><span class="sxs-lookup"><span data-stu-id="7b912-139">Click **Create** to create the function.</span></span>

<span data-ttu-id="7b912-140">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="7b912-140">::: zone pivot="csharp"</span></span>
1. <span data-ttu-id="7b912-141">**C#**</span><span class="sxs-lookup"><span data-stu-id="7b912-141">**C#**</span></span> 

    1. <span data-ttu-id="7b912-142">När funktionens källkod visas ersätter du allt innehåll i **run.csx** med innehållet i [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span><span class="sxs-lookup"><span data-stu-id="7b912-142">When the function's source code appears, replace all of **run.csx** with the content of [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span></span>

<span data-ttu-id="7b912-143">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="7b912-143">::: zone-end</span></span>

<span data-ttu-id="7b912-144">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="7b912-144">::: zone pivot="javascript"</span></span>
1. <span data-ttu-id="7b912-145">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="7b912-145">**JavaScript**</span></span> 

    1. <span data-ttu-id="7b912-146">(JavaScript) Den här funktionen kräver `azure-storage`-paketet från npm för att generera den SAS-token (signatur för delad åtkomst) som krävs för att skapa den säkra URL:en.</span><span class="sxs-lookup"><span data-stu-id="7b912-146">(JavaScript) This function requires the `azure-storage` package from npm to generate the shared access signature (SAS) token required to build the secure URL.</span></span> <span data-ttu-id="7b912-147">Du installerar npm-paketet genom att klicka på funktionsappens namn i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="7b912-147">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="7b912-148">(JavaScript) Öppna ett konsolfönster genom att klicka på **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="7b912-148">(JavaScript) Click **Console** to reveal a console window.</span></span>

        ![Öppna ett konsolfönster](../images/2-open-console.jpg)

    1. <span data-ttu-id="7b912-150">(JavaScript) Kontrollera att den aktuella katalogen är **d:\home\site\wwwroot** genom att köra kommandot `cd d:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="7b912-150">(JavaScript) Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

    1. <span data-ttu-id="7b912-151">(JavaScript) Kör kommandot `npm init -y` för att skapa en tom **package.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="7b912-151">(JavaScript) Run the command `npm init -y` to create an empty **package.json** file.</span></span>

    1. <span data-ttu-id="7b912-152">(JavaScript) Kör kommandot `npm install --save azure-storage` i konsolen för att installera paketet och spara det i **package.json**.</span><span class="sxs-lookup"><span data-stu-id="7b912-152">(JavaScript) Run the command `npm install --save azure-storage` in the console to install the package and save it in **package.json**.</span></span> <span data-ttu-id="7b912-153">Åtgärden kan ta ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="7b912-153">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="7b912-154">(JavaScript) Klicka på funktionsnamnet (**GetUploadUrl**) i det vänstra navigeringsfönstret för att visa funktionen och ersätt allt innehåll i **index.js** med innehållet i [ **javascript/GetUploadUrl / index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span><span class="sxs-lookup"><span data-stu-id="7b912-154">(JavaScript) Click on the function name (**GetUploadUrl**) in the left navigation to reveal the function, replace all of **index.js** with the content of [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span></span>
    
        ![index.js efter uppdatering](../images/2-paste-js.jpg)

<span data-ttu-id="7b912-156">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="7b912-156">::: zone-end</span></span>

1. <span data-ttu-id="7b912-157">Klicka på **Loggar** under kodfönstret för att expandera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="7b912-157">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="7b912-158">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7b912-158">Click **Save**.</span></span> <span data-ttu-id="7b912-159">Granska loggpanelen för att bekräfta att funktionen har kompilerats.</span><span class="sxs-lookup"><span data-stu-id="7b912-159">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="7b912-160">Funktionen genererar en så kallad SAS-URL (signatur för delad åtkomst) som används för att ladda upp en fil till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7b912-160">The function generates what is called a shared access signature (SAS) URL that is used to upload a file to Blob storage.</span></span> <span data-ttu-id="7b912-161">SAS-URL:en är giltig en kort tid och tillåter bara uppladdning av en enda fil.</span><span class="sxs-lookup"><span data-stu-id="7b912-161">The SAS URL is valid for a short period of time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="7b912-162">Läs dokumentationen för Blob Storage om du vill veta mer om [hur du använder signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="7b912-162">Consult the Blob storage documentation for more information on [using shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="7b912-163">Lägg till en miljövariabel för anslutningssträngen för lagring</span><span class="sxs-lookup"><span data-stu-id="7b912-163">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="7b912-164">Funktionen som du nyss skapat behöver en anslutningssträng för lagringskontot för att kunna generera SAS-URL:en.</span><span class="sxs-lookup"><span data-stu-id="7b912-164">The function you just created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="7b912-165">I stället för att hårdkoda anslutningssträngen i funktionens kod, kan du lagra den som en programinställning.</span><span class="sxs-lookup"><span data-stu-id="7b912-165">Instead of hardcoding the connection string in the function's body, it can be stored as an application setting.</span></span> <span data-ttu-id="7b912-166">Alla funktioner i funktionsappen kan komma åt programinställningar som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="7b912-166">Application settings are accessible as environment variables by all functions in the Function app.</span></span>

1. <span data-ttu-id="7b912-167">Hämta anslutningssträngen för lagringskontot från Cloud Shell och spara den i en bash-variabel med namnet **STORAGE_CONNECTION_STRING**.</span><span class="sxs-lookup"><span data-stu-id="7b912-167">In the Cloud Shell, query the Storage account connection string and save it to a bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="7b912-168">Bekräfta att variabeln är korrekt angiven.</span><span class="sxs-lookup"><span data-stu-id="7b912-168">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="7b912-169">Skapa en ny programinställning med namnet **AZURE_STORAGE_CONNECTION_STRING** genom att använda värdet som sparades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="7b912-169">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    <span data-ttu-id="7b912-170">Bekräfta att kommandots utdata innehåller den nya programinställningen med rätt värde.</span><span class="sxs-lookup"><span data-stu-id="7b912-170">Confirm that the command's output contains the new application setting with the correct value.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="7b912-171">Testa den serverlösa funktionen</span><span class="sxs-lookup"><span data-stu-id="7b912-171">Test the serverless function</span></span>

<span data-ttu-id="7b912-172">Förutom att skapa och redigera funktioner innehåller Azure Portal även ett inbyggt verktyg för att testa funktioner.</span><span class="sxs-lookup"><span data-stu-id="7b912-172">In addition to creating and editing functions, the Azure portal also provides an built-in tool for testing functions.</span></span>

1. <span data-ttu-id="7b912-173">Testa den serverlösa HTTP-funktionen genom att klicka på fliken **Testa** till höger om kodfönstret för att expandera testpanelen.</span><span class="sxs-lookup"><span data-stu-id="7b912-173">To test the HTTP serverless function, click on the **Test** tab on the right of the code window to expand the test panel.</span></span>

1. <span data-ttu-id="7b912-174">Ändra **Http-metoden** till **GET**.</span><span class="sxs-lookup"><span data-stu-id="7b912-174">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="7b912-175">Under **Fråga** klickar du på *Lägg till parameter*\* och lägger till följande parameter:</span><span class="sxs-lookup"><span data-stu-id="7b912-175">Under **Query**, click *Add parameter*\* and add the following parameter:</span></span>

    | <span data-ttu-id="7b912-176">name (namn)</span><span class="sxs-lookup"><span data-stu-id="7b912-176">name</span></span>      |  <span data-ttu-id="7b912-177">value (värde)</span><span class="sxs-lookup"><span data-stu-id="7b912-177">value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="7b912-178">filename (filnamn)</span><span class="sxs-lookup"><span data-stu-id="7b912-178">filename</span></span> | <span data-ttu-id="7b912-179">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="7b912-179">image1.jpg</span></span> |

1. <span data-ttu-id="7b912-180">Klicka på **Kör** på testpanelen för att skicka en HTTP-begäran till funktionen.</span><span class="sxs-lookup"><span data-stu-id="7b912-180">Click **Run** in the test panel to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="7b912-181">Funktionen returnerar en uppladdnings-URL.</span><span class="sxs-lookup"><span data-stu-id="7b912-181">The function returns an upload URL in the output.</span></span> <span data-ttu-id="7b912-182">Funktionskörningen visas på loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="7b912-182">The function execution appears in the logs panel.</span></span>

    ![Loggar som visar att funktionen körts utan problem](../images/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a><span data-ttu-id="7b912-184">Konfigurera CORS i funktionsappen</span><span class="sxs-lookup"><span data-stu-id="7b912-184">Configure CORS in the function app</span></span>

<span data-ttu-id="7b912-185">Eftersom appens klientdel finns i Blob Storage har den ett annat domännamn än Azure Functions-appen.</span><span class="sxs-lookup"><span data-stu-id="7b912-185">Because the app's frontend is hosted in Blob storage, it has a different domain name than the Azure Function app.</span></span> <span data-ttu-id="7b912-186">För att JavaScript-koden på klientsidan ska kunna anropa funktionen som du precis skapat måste funktionsappen konfigureras för CORS (Cross-Origin Resource Sharing).</span><span class="sxs-lookup"><span data-stu-id="7b912-186">For the client-side JavaScript to successfully call the function you just created, the function app needs to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="7b912-187">Klicka på namnet på funktionsappen i det vänstra navigeringsfältet i funktionsappens fönster.</span><span class="sxs-lookup"><span data-stu-id="7b912-187">In the left navigation bar of the function app window, click on the name of your function app.</span></span>

1. <span data-ttu-id="7b912-188">Visa en lista med avancerade funktioner genom att klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="7b912-188">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="7b912-189">Klicka på **CORS** under **API**.</span><span class="sxs-lookup"><span data-stu-id="7b912-189">Under **API**, click **CORS**.</span></span>

    ![Välj CORS](../images/2-open-cors.jpg)

1. <span data-ttu-id="7b912-191">Lägg till tillåtna ursprung för program-URL:en från den föregående modulen, men utelämna det avslutande **/** (till exempel: `https://firstserverlessweb.z4.web.core.windows.net`).</span><span class="sxs-lookup"><span data-stu-id="7b912-191">Add an allow origin for the application URL from the previous module, omitting the trailing **/** (for example: `https://firstserverlessweb.z4.web.core.windows.net`).</span></span>

    ![CORS-inställningar som visar att den serverlösa webbappens URL har lagts till](../images/2-add-cors.png)

1. <span data-ttu-id="7b912-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7b912-193">Click **Save**.</span></span>

1. <span data-ttu-id="7b912-194">C#</span><span class="sxs-lookup"><span data-stu-id="7b912-194">C#</span></span>

   1. <span data-ttu-id="7b912-195">(C#) Gå tillbaka till funktionen `GetUploadUrl` och välj sedan fliken **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="7b912-195">(C#) Navigate back to the `GetUploadUrl` function, and then select the **Integrate** tab.</span></span>

   1. <span data-ttu-id="7b912-196">(C#) Välj **OPTIONS** under Utvalda HTTP-metoder.</span><span class="sxs-lookup"><span data-stu-id="7b912-196">(C#) Under Selected HTTP methods, select **OPTIONS**.</span></span>

      <span data-ttu-id="7b912-197">**GET**, **POST** och **OPTIONS** bör vara markerade.</span><span class="sxs-lookup"><span data-stu-id="7b912-197">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="7b912-198">CORS använder OPTIONS-metoden, som inte är markerad som standard för C#-funktioner.</span><span class="sxs-lookup"><span data-stu-id="7b912-198">CORS uses the OPTIONS method, which is not selected by default for C# functions.</span></span>  

   1. <span data-ttu-id="7b912-199">(C#) Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7b912-199">(C#) Click **Save**.</span></span>

1. <span data-ttu-id="7b912-200">Navigera till funktionsappen från portalen, välj fliken **Översikt** och klicka sedan på **Starta om** för att bekräfta att ändringarna för CORS har tillämpats.</span><span class="sxs-lookup"><span data-stu-id="7b912-200">Still in the portal, navigate to the function app, select the **Overview** tab, and then click **Restart** to make sure that the changes for CORS take effect.</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="7b912-201">Konfigurera CORS i lagringskontot</span><span class="sxs-lookup"><span data-stu-id="7b912-201">Configure CORS in the Storage account</span></span>

<span data-ttu-id="7b912-202">Eftersom appen även gör JavaScript-anrop till Blob Storage för filuppladdning måste du även konfigurera lagringskontot för CORS.</span><span class="sxs-lookup"><span data-stu-id="7b912-202">Because the app also makes client-side JavaScript calls to Blob Storage to upload files, you also have to configure the Storage account for CORS.</span></span>

1. <span data-ttu-id="7b912-203">Kör följande kommando för att tillåta att alla ursprung laddar upp filer till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="7b912-203">Run the following command to allow all origins to upload files to the Storage account.</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="7b912-204">Ändra webbappen för att ladda upp bilder</span><span class="sxs-lookup"><span data-stu-id="7b912-204">Modify the web app to upload images</span></span>

<span data-ttu-id="7b912-205">Webbappen hämtar inställningar från en fil med namnet **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="7b912-205">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="7b912-206">I följande steg ska du skapa filen med hjälp av Cloud Shell och sedan ange `window.apiBaseUrl` till URL:en för funktionsappen och `window.blobBaseUrl` till URL:en för Azure Blob Storage-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7b912-206">In the following steps, you create the file using Cloud Shell, then set `window.apiBaseUrl` to the URL of the Function app and `window.blobBaseUrl` to the URL of the Azure Blob Storage endpoint.</span></span>

1. <span data-ttu-id="7b912-207">Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella mappen.</span><span class="sxs-lookup"><span data-stu-id="7b912-207">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="7b912-208">Kör en fråga för att hämta appens URL och lagra den i en bash-variabel med namnet **FUNCTION_APP_URL**.</span><span class="sxs-lookup"><span data-stu-id="7b912-208">Query the function app's URL and store it in a bash variable named **FUNCTION_APP_URL**.</span></span>

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    <span data-ttu-id="7b912-209">Bekräfta att variabeln är korrekt angiven.</span><span class="sxs-lookup"><span data-stu-id="7b912-209">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. <span data-ttu-id="7b912-210">Ställ in bas-URI:n för API-anrop till funktionsappen genom att skapa **settings.js** och lägga till funktionsappens URL som du ser nedan.</span><span class="sxs-lookup"><span data-stu-id="7b912-210">To set the base URI of API calls to your function app, create **settings.js** and add the function app URL like the following.</span></span>

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    <span data-ttu-id="7b912-211">Du kan göra ändringen genom att köra följande kommando eller genom att använda en kommandoradsredigerare som VIM.</span><span class="sxs-lookup"><span data-stu-id="7b912-211">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    <span data-ttu-id="7b912-212">Kontrollera att filen skrivits korrekt.</span><span class="sxs-lookup"><span data-stu-id="7b912-212">Confirm the file was successfully written.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="7b912-213">Kör en fråga för att hämta bas-URL:en för Blob Storage och lagra den i en bash-variabel med namnet **BLOB_BASE_URL**.</span><span class="sxs-lookup"><span data-stu-id="7b912-213">Query the Blob Storage base URL and store it in a bash variable named **BLOB_BASE_URL**.</span></span>

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    <span data-ttu-id="7b912-214">Bekräfta att variabeln är korrekt angiven.</span><span class="sxs-lookup"><span data-stu-id="7b912-214">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. <span data-ttu-id="7b912-215">Ställ in bas-URI:n för API-anrop till funktionsappen genom att lägga till lagrings-URL:en som i följande kodrad i **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="7b912-215">To set the base URI of API calls to your function app, append the storage URL like the following line of code to **settings.js**.</span></span>

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    <span data-ttu-id="7b912-216">Du kan göra ändringen genom att köra följande kommando eller genom att använda en kommandoradsredigerare som VIM.</span><span class="sxs-lookup"><span data-stu-id="7b912-216">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    <span data-ttu-id="7b912-217">Bekräfta att filen skrivits korrekt och att den nu innehåller två rader.</span><span class="sxs-lookup"><span data-stu-id="7b912-217">Confirm the file was successfully written and it now contains 2 lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="7b912-218">Ladda upp filen till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7b912-218">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a><span data-ttu-id="7b912-219">Testa webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="7b912-219">Test the web application</span></span>

<span data-ttu-id="7b912-220">Nu kan galleriprogrammet ladda upp en bild till Blob Storage, men den kan inte visa bilder än.</span><span class="sxs-lookup"><span data-stu-id="7b912-220">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="7b912-221">Den kommer att försöka anropa en `GetImages`-funktion som inte finns ännu eftersom du skapar den i en senare modul.</span><span class="sxs-lookup"><span data-stu-id="7b912-221">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="7b912-222">Anropet misslyckas och sidan verkar ha fastnat på ”Analyserar...”, men bilden du valt kommer att laddas upp korrekt.</span><span class="sxs-lookup"><span data-stu-id="7b912-222">That call will fail, and the web page will appear to be stuck on "Analyzing...", but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="7b912-223">Du kan kontrollera att en bild har laddats upp genom att kontrollera innehållet i containern **images** på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7b912-223">You can verify an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="7b912-224">Gå till programmet i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="7b912-224">In a browser window, browse to the application.</span></span> <span data-ttu-id="7b912-225">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="7b912-225">Select an image file and upload it.</span></span> <span data-ttu-id="7b912-226">Uppladdningen slutförs, men eftersom vi inte har lagt till möjligheten att visa bilder ännu så visar inte appen det uppladdade fotot.</span><span class="sxs-lookup"><span data-stu-id="7b912-226">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="7b912-227">(Sidan verkar ha fastnat på ”Analyserar bild...”. Du ska åtgärda detta senare.)</span><span class="sxs-lookup"><span data-stu-id="7b912-227">(The web page appears to be stuck on "Analyzing image..."; you'll fix that later.)</span></span>

1. <span data-ttu-id="7b912-228">Bekräfta i Cloud Shell att bilden har laddats upp till containern **images**.</span><span class="sxs-lookup"><span data-stu-id="7b912-228">In the Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="7b912-229">Ta bort alla filer i containern **images** innan du går vidare till nästa självstudie.</span><span class="sxs-lookup"><span data-stu-id="7b912-229">Before moving on to the next tutorial, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="7b912-230">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7b912-230">Summary</span></span>

<span data-ttu-id="7b912-231">I den här enheten har du skapat en Azure Functions-app och lärt dig hur du använder en serverlös funktion för att ladda upp bilder till Blob Storage från ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7b912-231">In this unit, you created an Azure Function app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="7b912-232">I nästa avsnitt lär du dig hur du skapar miniatyrbilder för de uppladdade bilderna med hjälp av en blobbutlöst serverlös funktion.</span><span class="sxs-lookup"><span data-stu-id="7b912-232">Next, you learn how to create thumbnails for the uploaded images using a Blob triggered serverless function.</span></span>