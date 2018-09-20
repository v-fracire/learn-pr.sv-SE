<span data-ttu-id="ade19-101">I den föregående delen lärde du dig hur du kan använda en serverlös funktion för att på ett säkert sätt ladda upp bilder till Blob Storage från ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ade19-101">In the previous unit, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="ade19-102">I den här modulen ska du skapa en annan serverlös funktion för att bevaka uppladdade bilder och skapa miniatyrbilder från dem.</span><span class="sxs-lookup"><span data-stu-id="ade19-102">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="ade19-103">Skapa en Blob Storage-container för miniatyrbilder</span><span class="sxs-lookup"><span data-stu-id="ade19-103">Create a Blob storage container for thumbnails</span></span>

<span data-ttu-id="ade19-104">Bilderna i fullstorlek lagras i en container med namnet **images** (bilder).</span><span class="sxs-lookup"><span data-stu-id="ade19-104">The full-size images are stored in a container named **images**.</span></span> <span data-ttu-id="ade19-105">Du behöver en annan container för att lagra miniatyrbilderna för dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="ade19-105">You need another container to store thumbnails of those images.</span></span>

<span data-ttu-id="ade19-106">Skapa en ny container med namnet **thumbnails** (miniatyrbilder) i ditt lagringskonto med offentlig åtkomst till alla blobar.</span><span class="sxs-lookup"><span data-stu-id="ade19-106">Create a new container named **thumbnails** in your Storage account with public access to all blobs.</span></span>

```azurecli
az storage container create -n thumbnails --account-name <storage account name> --public-access blob
```

## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="ade19-107">Skapa en blobutlöst serverlös funktion</span><span class="sxs-lookup"><span data-stu-id="ade19-107">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="ade19-108">En utlösare definierar hur en funktion anropas.</span><span class="sxs-lookup"><span data-stu-id="ade19-108">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="ade19-109">Funktionen du nu ska skapa använder en blob-utlösare.</span><span class="sxs-lookup"><span data-stu-id="ade19-109">The function you create next uses a blob trigger.</span></span> <span data-ttu-id="ade19-110">Funktionen anropas automatiskt när en blob (bildfil) laddas upp till containern **images**.</span><span class="sxs-lookup"><span data-stu-id="ade19-110">The function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="ade19-111">En funktion måste ha en utlösare.</span><span class="sxs-lookup"><span data-stu-id="ade19-111">A function must have one trigger.</span></span> <span data-ttu-id="ade19-112">Utlösare har associerade data, vilket vanligtvis är nyttolasten som utlöste funktionen.</span><span class="sxs-lookup"><span data-stu-id="ade19-112">Triggers have associated data, which are usually the payload that triggered the function.</span></span>

<span data-ttu-id="ade19-113">Bindningar definierar hur en funktion läser och skriver data i Azure eller andra tjänster från tredje part.</span><span class="sxs-lookup"><span data-stu-id="ade19-113">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="ade19-114">Den här funktionen skapar en miniatyrbild för bilden som utlöser den och sparar miniatyrbilden i containern *thumbnails*.</span><span class="sxs-lookup"><span data-stu-id="ade19-114">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="ade19-115">Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du har aktiverat sandbox-miljön med.</span><span class="sxs-lookup"><span data-stu-id="ade19-115">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="ade19-116">Öppna din funktionsapp och använd **Sökrutan** längst upp på portalen och sök efter namn.</span><span class="sxs-lookup"><span data-stu-id="ade19-116">Open your Function app, you can use the **Search** box at the top of the portal to find it by name.</span></span>

1. <span data-ttu-id="ade19-117">I appens vänstra navigeringsfönster pekar du på **Functions** och klickar på plustecknet (+) för att skapa en ny serverlös funktion.</span><span class="sxs-lookup"><span data-stu-id="ade19-117">In the Functions app window's left navigation, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span> <span data-ttu-id="ade19-118">Om en snabbstartssida visas klickar du på **Anpassad funktion** för att visa en lista med funktionsmallar.</span><span class="sxs-lookup"><span data-stu-id="ade19-118">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="ade19-119">Leta upp och välj mallen **BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="ade19-119">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="ade19-120">Använd dessa värden för att skapa en funktion som genererar miniatyrbilder när bilder laddas upp:</span><span class="sxs-lookup"><span data-stu-id="ade19-120">Use these values to create a function that creates thumbnails as images are uploaded:</span></span>

    | <span data-ttu-id="ade19-121">Inställning</span><span class="sxs-lookup"><span data-stu-id="ade19-121">Setting</span></span>      |  <span data-ttu-id="ade19-122">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="ade19-122">Suggested value</span></span>   | <span data-ttu-id="ade19-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ade19-123">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="ade19-124">**Språk**</span><span class="sxs-lookup"><span data-stu-id="ade19-124">**Language**</span></span> | <span data-ttu-id="ade19-125">C# eller JavaScript</span><span class="sxs-lookup"><span data-stu-id="ade19-125">C# or JavaScript</span></span> | <span data-ttu-id="ade19-126">Välj det språk du föredrar.</span><span class="sxs-lookup"><span data-stu-id="ade19-126">Choose your preferred language.</span></span> |
    | <span data-ttu-id="ade19-127">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="ade19-127">**Name your function**</span></span> | <span data-ttu-id="ade19-128">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="ade19-128">ResizeImage</span></span> | <span data-ttu-id="ade19-129">Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen.</span><span class="sxs-lookup"><span data-stu-id="ade19-129">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="ade19-130">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="ade19-130">**Path**</span></span> | <span data-ttu-id="ade19-131">images/{name}</span><span class="sxs-lookup"><span data-stu-id="ade19-131">images/{name}</span></span> | <span data-ttu-id="ade19-132">Kör funktionen när en fil läggs till i containern **images**.</span><span class="sxs-lookup"><span data-stu-id="ade19-132">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="ade19-133">**Lagringskontoinformation**</span><span class="sxs-lookup"><span data-stu-id="ade19-133">**Storage account information**</span></span> | <span data-ttu-id="ade19-134">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="ade19-134">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="ade19-135">Använd miljövariabeln som skapades tidigare med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="ade19-135">Use the environment variable previously created with the connection string.</span></span> |

    ![Ange inställningarna för den nya funktionen](../media/3-new-function.png)

1. <span data-ttu-id="ade19-137">Skapa funktionen genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ade19-137">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="ade19-138">När funktionen har skapats klickar du på **Integrera** för att visa funktionens utlösare, indata och utdatabindningar.</span><span class="sxs-lookup"><span data-stu-id="ade19-138">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="ade19-139">Klicka på **Nya utdata** för att skapa en ny utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="ade19-139">Click **New output** to create a new output binding.</span></span>

    ![Välj Nya utdata på fliken Integrera](../media/3-new-output.jpg)

1. <span data-ttu-id="ade19-141">Välj **Azure Blob Storage** och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="ade19-141">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="ade19-142">Du kan behöva rulla ned för att se knappen **Välj**.</span><span class="sxs-lookup"><span data-stu-id="ade19-142">You may have to scroll down to see the **Select** button.</span></span>

    ![Välj Azure Blob Storage](../media/3-storage-output.jpg)

1. <span data-ttu-id="ade19-144">Ange följande värden:</span><span class="sxs-lookup"><span data-stu-id="ade19-144">Enter the following values:</span></span>

    | <span data-ttu-id="ade19-145">Inställning</span><span class="sxs-lookup"><span data-stu-id="ade19-145">Setting</span></span>      |  <span data-ttu-id="ade19-146">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="ade19-146">Suggested value</span></span>   | <span data-ttu-id="ade19-147">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ade19-147">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="ade19-148">**Blobparameternamn**</span><span class="sxs-lookup"><span data-stu-id="ade19-148">**Blob parameter name**</span></span> | <span data-ttu-id="ade19-149">thumbnail</span><span class="sxs-lookup"><span data-stu-id="ade19-149">thumbnail</span></span> | <span data-ttu-id="ade19-150">Funktionen använder parametern med det här namnet för att skriva miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="ade19-150">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="ade19-151">**Använd funktionsreturvärde**</span><span class="sxs-lookup"><span data-stu-id="ade19-151">**Use function return value**</span></span> | <span data-ttu-id="ade19-152">Nej</span><span class="sxs-lookup"><span data-stu-id="ade19-152">No</span></span> |  |
    | <span data-ttu-id="ade19-153">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="ade19-153">**Path**</span></span> | <span data-ttu-id="ade19-154">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="ade19-154">thumbnails/{name}</span></span> | <span data-ttu-id="ade19-155">Miniatyrbilderna skrivs till en container med namnet **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="ade19-155">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="ade19-156">**Lagringskontoanslutning**</span><span class="sxs-lookup"><span data-stu-id="ade19-156">**Storage account connection**</span></span> | <span data-ttu-id="ade19-157">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="ade19-157">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="ade19-158">Använd miljövariabeln som skapades tidigare med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="ade19-158">Use the environment variable previously created with the connection string.</span></span> |

    ![Ange inställningar för blobutdatabindningen](../media/3-blob-output.png)

1. <span data-ttu-id="ade19-160">Tryck på **Spara** för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="ade19-160">Click **Save** to save your changes.</span></span>

<span data-ttu-id="ade19-161">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="ade19-161">::: zone pivot="javascript"</span></span>

11. <span data-ttu-id="ade19-162">Klicka på **Avancerad redigerare** i det övre högra hörnet i fönstret för att visa den JSON som representerar bindningarna.</span><span class="sxs-lookup"><span data-stu-id="ade19-162">Click on **Advanced editor** in the top right corner of the window to reveal the JSON that represents the bindings.</span></span>

1. <span data-ttu-id="ade19-163">Lägg till en egenskap med namnet `dataType` med värdet `binary` i `blobTrigger`-bindningen.</span><span class="sxs-lookup"><span data-stu-id="ade19-163">In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="ade19-164">När du gör det konfigureras bindningen så att blobinnehållet överförs till funktionen som binärdata.</span><span class="sxs-lookup"><span data-stu-id="ade19-164">This configures the binding to pass the blob contents to the function as binary data.</span></span>

    ```json
    {
        "name": "myBlob",
        "type": "blobTrigger",
        "direction": "in",
        "path": "images/{name}",
        "connection": "AZURE_STORAGE_CONNECTION_STRING",
        "dataType": "binary"
    }
    ```

1. <span data-ttu-id="ade19-165">Skapa den nya bindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ade19-165">Click **Save** to create the new binding.</span></span>

<span data-ttu-id="ade19-166">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="ade19-166">::: zone-end</span></span>

<span data-ttu-id="ade19-167">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="ade19-167">::: zone pivot="csharp"</span></span>

11. <span data-ttu-id="ade19-168">Välj funktionsnamnet **ResizeImage** i det vänstra navigeringsfönstret för att öppna källkoden för funktionen.</span><span class="sxs-lookup"><span data-stu-id="ade19-168">Select the **ResizeImage** function name in the left navigation to open the function's source code.</span></span>

1. <span data-ttu-id="ade19-169">Funktionen kräver ett NuGet-paket med namnet **ImageResizer** för att generera miniatyrbilderna.</span><span class="sxs-lookup"><span data-stu-id="ade19-169">The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="ade19-170">NuGet-paket läggs till i C#-funktioner med hjälp av en **project.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="ade19-170">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="ade19-171">Skapa filen genom att först klicka på **Visa filer** till höger för att visa filerna som funktionen består av.</span><span class="sxs-lookup"><span data-stu-id="ade19-171">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>

1. <span data-ttu-id="ade19-172">Klicka på **Lägg till** för att lägga till en ny fil med namnet **project.json**.</span><span class="sxs-lookup"><span data-stu-id="ade19-172">Click **Add** to add a new file named **project.json**.</span></span> <span data-ttu-id="ade19-173">Tryck på **Retur** när du är klar för att lägga till filen.</span><span class="sxs-lookup"><span data-stu-id="ade19-173">Press **Enter** when done to add the file.</span></span>

1. <span data-ttu-id="ade19-174">Kopiera innehållet i [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) till den nya filen som skapats.</span><span class="sxs-lookup"><span data-stu-id="ade19-174">Copy the contents of the [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) file into the newly created file.</span></span> <span data-ttu-id="ade19-175">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="ade19-175">Save the file.</span></span> <span data-ttu-id="ade19-176">Paket återställs automatiskt när filen uppdateras.</span><span class="sxs-lookup"><span data-stu-id="ade19-176">Packages are automatically restored when the file is updated.</span></span>

    ![project.json-fil med ImageResizer](../media/3-project-json.png)

1. <span data-ttu-id="ade19-178">Under **Visa filer**väljer du **run.csx**.</span><span class="sxs-lookup"><span data-stu-id="ade19-178">Under **View Files**, select **run.csx**.</span></span> <span data-ttu-id="ade19-179">Ersätt dess innehåll med innehållet i filen [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span><span class="sxs-lookup"><span data-stu-id="ade19-179">Replace its content with the content in the [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx) file.</span></span>

1. <span data-ttu-id="ade19-180">Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="ade19-180">To expand the logs panel, click **Logs** below the code window.</span></span>

1. <span data-ttu-id="ade19-181">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ade19-181">Click **Save**.</span></span> <span data-ttu-id="ade19-182">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="ade19-182">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>

<span data-ttu-id="ade19-183">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="ade19-183">::: zone-end</span></span>

<span data-ttu-id="ade19-184">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="ade19-184">::: zone pivot="javascript"</span></span>

14. <span data-ttu-id="ade19-185">I den här funktionen behövs paketet `jimp` från npm till att ändra storlek på fotot.</span><span class="sxs-lookup"><span data-stu-id="ade19-185">This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="ade19-186">Du installerar npm-paketet genom att klicka på Funktionsapp-namnet i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="ade19-186">To install the npm package, click on the Functions app name on the left navigation and click **Platform features**.</span></span>

1. <span data-ttu-id="ade19-187">Öppna ett konsolfönster genom att klicka på **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="ade19-187">Click **Console** to reveal a console window.</span></span>

1. <span data-ttu-id="ade19-188">Kör kommandot `npm install jimp` i konsolen.</span><span class="sxs-lookup"><span data-stu-id="ade19-188">Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="ade19-189">Det kan ta några minuter att slutföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ade19-189">It may take a few minutes to complete the operation.</span></span>

1. <span data-ttu-id="ade19-190">Klicka på funktionsnamnet **ResizeImage** i det vänstra navigeringsfönstret för att visa funktionen.</span><span class="sxs-lookup"><span data-stu-id="ade19-190">Click on the **ResizeImage** function name in the left navigation to reveal the function.</span></span> <span data-ttu-id="ade19-191">Byt ut hela innehållet filen **index.js** mot innehållet i filen [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span><span class="sxs-lookup"><span data-stu-id="ade19-191">Replace all the content in the **index.js** file with the content of the [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js) file.</span></span>

1. <span data-ttu-id="ade19-192">Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="ade19-192">To expand the logs panel, click **Logs** below the code window.</span></span>

1. <span data-ttu-id="ade19-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ade19-193">Click **Save**.</span></span> <span data-ttu-id="ade19-194">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="ade19-194">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>

<span data-ttu-id="ade19-195">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="ade19-195">::: zone-end</span></span>

## <a name="test-the-serverless-function"></a><span data-ttu-id="ade19-196">Testa den serverlösa funktionen</span><span class="sxs-lookup"><span data-stu-id="ade19-196">Test the serverless function</span></span>

1. <span data-ttu-id="ade19-197">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ade19-197">Open the application in a browser.</span></span> <span data-ttu-id="ade19-198">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="ade19-198">Select an image file and upload it.</span></span> <span data-ttu-id="ade19-199">Uppladdningen slutförs, men eftersom vi inte har lagt till möjligheten att visa bilder ännu så visar inte appen det uppladdade fotot.</span><span class="sxs-lookup"><span data-stu-id="ade19-199">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="ade19-200">Bekräfta i Cloud Shell att bilden har laddats upp till containern **images**.</span><span class="sxs-lookup"><span data-stu-id="ade19-200">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list \
        --account-name <storage account name> \
        -c images \
        -o table
    ```

1. <span data-ttu-id="ade19-201">Kontrollera att miniatyrbilden har skapats i en container med namnet **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="ade19-201">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list \
        --account-name <storage account name> \
        -c thumbnails \
        -o table
    ```

1. <span data-ttu-id="ade19-202">Hämta URL:en för miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="ade19-202">Get the URL for the thumbnail.</span></span>

    ```azurecli
    az storage blob url \
        --account-name <storage account name> \
        -c thumbnails \
        -n <filename> \
        --output tsv
    ```

    <span data-ttu-id="ade19-203">Öppna URL:en i en webbläsare och bekräfta att miniatyrbilden har skapats korrekt.</span><span class="sxs-lookup"><span data-stu-id="ade19-203">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="ade19-204">Ta bort alla filer i containrarna **images** och **thumbnails** innan du går vidare till nästa enhet.</span><span class="sxs-lookup"><span data-stu-id="ade19-204">Before continuing to the next unit, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch \
        -s images \
        --account-name <storage account name>
    ```

    ```azurecli
    az storage blob delete-batch \
        -s thumbnails \
        --account-name <storage account name>
    ```

## <a name="summary"></a><span data-ttu-id="ade19-205">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ade19-205">Summary</span></span>

<span data-ttu-id="ade19-206">I den här delen har du skapat en serverlös funktion som genererar en miniatyrbild varje gång en bild laddas upp till en Blob Storage-container.</span><span class="sxs-lookup"><span data-stu-id="ade19-206">In this unit, you created a serverless function to create a thumbnail when an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="ade19-207">I nästa avsnitt får du lära du dig att använda Azure Cosmos DB till att lagra och visa metadata om bilder.</span><span class="sxs-lookup"><span data-stu-id="ade19-207">Next, you will learn how to use Azure Cosmos DB to store and list image metadata.</span></span>