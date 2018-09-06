<span data-ttu-id="a3a94-101">I den föregående delen lärde du dig hur du kan använda en serverlös funktion för att på ett säkert sätt ladda upp bilder till Blob Storage från ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a3a94-101">In the previous unit, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="a3a94-102">I den här modulen ska du skapa en annan serverlös funktion för att bevaka uppladdade bilder och skapa miniatyrbilder från dem.</span><span class="sxs-lookup"><span data-stu-id="a3a94-102">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="a3a94-103">Skapa en Blob Storage-container för miniatyrbilder</span><span class="sxs-lookup"><span data-stu-id="a3a94-103">Create a Blob storage container for thumbnails</span></span>

<span data-ttu-id="a3a94-104">Bilderna i fullstorlek lagras i en container med namnet **images** (bilder).</span><span class="sxs-lookup"><span data-stu-id="a3a94-104">The full-size images are stored in a container named **images**.</span></span> <span data-ttu-id="a3a94-105">Du behöver en annan container för att lagra miniatyrbilderna för dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="a3a94-105">You need another container to store thumbnails of those images.</span></span>

1. <span data-ttu-id="a3a94-106">Kontrollera att du fortfarande är inloggad i Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="a3a94-106">Ensure you're still signed in to Cloud Shell (Bash).</span></span> <span data-ttu-id="a3a94-107">Om du inte är det väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.</span><span class="sxs-lookup"><span data-stu-id="a3a94-107">If you aren't, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="a3a94-108">Skapa en ny container med namnet **thumbnails** (miniatyrbilder) i ditt lagringskonto med offentlig åtkomst till alla blobar.</span><span class="sxs-lookup"><span data-stu-id="a3a94-108">Create a new container named **thumbnails** in your Storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="a3a94-109">Skapa en blobutlöst serverlös funktion</span><span class="sxs-lookup"><span data-stu-id="a3a94-109">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="a3a94-110">En utlösare definierar hur en funktion anropas.</span><span class="sxs-lookup"><span data-stu-id="a3a94-110">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="a3a94-111">Funktionen du nu ska skapa använder en blob-utlösare.</span><span class="sxs-lookup"><span data-stu-id="a3a94-111">The function you create next uses a blob trigger.</span></span> <span data-ttu-id="a3a94-112">Funktionen anropas automatiskt när en blob (bildfil) laddas upp till containern **images**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-112">The function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="a3a94-113">En funktion måste ha en utlösare.</span><span class="sxs-lookup"><span data-stu-id="a3a94-113">A function must have one trigger.</span></span> <span data-ttu-id="a3a94-114">Utlösare har associerade data, vilket vanligtvis är nyttolasten som utlöste funktionen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-114">Triggers have associated data, which is usually the payload that triggered the function.</span></span>

<span data-ttu-id="a3a94-115">Bindningar definierar hur en funktion läser eller skriver data i Azure eller tredjepartstjänster.</span><span class="sxs-lookup"><span data-stu-id="a3a94-115">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="a3a94-116">Den här funktionen skapar en miniatyrbild för bilden som utlöser den och sparar miniatyrbilden i containern *thumbnails*.</span><span class="sxs-lookup"><span data-stu-id="a3a94-116">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="a3a94-117">Öppna Funktionsapp på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-117">Open your Functions app in the Azure portal.</span></span>

1. <span data-ttu-id="a3a94-118">I Funktionsapp-fönstrets vänstra navigeringsfönstret pekar du på **Functions** och klickar på plustecknet (+) för att skapa en ny serverlös funktion.</span><span class="sxs-lookup"><span data-stu-id="a3a94-118">In the Functions app window's left navigation, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span> <span data-ttu-id="a3a94-119">Om en snabbstartssida visas klickar du på **Anpassad funktion** för att visa en lista med funktionsmallar.</span><span class="sxs-lookup"><span data-stu-id="a3a94-119">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="a3a94-120">Leta upp och välj mallen **BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-120">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="a3a94-121">Använd dessa värden för att skapa en funktion som genererar miniatyrbilder när bilder laddas upp:</span><span class="sxs-lookup"><span data-stu-id="a3a94-121">Use these values to create a function that creates thumbnails as images are uploaded:</span></span>

    | <span data-ttu-id="a3a94-122">Inställning</span><span class="sxs-lookup"><span data-stu-id="a3a94-122">Setting</span></span>      |  <span data-ttu-id="a3a94-123">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="a3a94-123">Suggested value</span></span>   | <span data-ttu-id="a3a94-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a3a94-124">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="a3a94-125">**Språk**</span><span class="sxs-lookup"><span data-stu-id="a3a94-125">**Language**</span></span> | <span data-ttu-id="a3a94-126">C# eller JavaScript</span><span class="sxs-lookup"><span data-stu-id="a3a94-126">C# or JavaScript</span></span> | <span data-ttu-id="a3a94-127">Välj det språk du föredrar.</span><span class="sxs-lookup"><span data-stu-id="a3a94-127">Choose your preferred language.</span></span> |
    | <span data-ttu-id="a3a94-128">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="a3a94-128">**Name your function**</span></span> | <span data-ttu-id="a3a94-129">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="a3a94-129">ResizeImage</span></span> | <span data-ttu-id="a3a94-130">Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-130">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="a3a94-131">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="a3a94-131">**Path**</span></span> | <span data-ttu-id="a3a94-132">images/{name}</span><span class="sxs-lookup"><span data-stu-id="a3a94-132">images/{name}</span></span> | <span data-ttu-id="a3a94-133">Kör funktionen när en fil läggs till i containern **images**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-133">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="a3a94-134">**Lagringskontoinformation**</span><span class="sxs-lookup"><span data-stu-id="a3a94-134">**Storage account information**</span></span> | <span data-ttu-id="a3a94-135">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="a3a94-135">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="a3a94-136">Använd miljövariabeln som skapades tidigare med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-136">Use the environment variable previously created with the connection string.</span></span> |

    ![Ange inställningarna för den nya funktionen](../media/3-new-function.png)

1. <span data-ttu-id="a3a94-138">Skapa funktionen genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-138">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="a3a94-139">När funktionen har skapats klickar du på **Integrera** för att visa funktionens utlösare, indata och utdatabindningar.</span><span class="sxs-lookup"><span data-stu-id="a3a94-139">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="a3a94-140">Klicka på **Nya utdata** för att skapa en ny utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="a3a94-140">Click **New output** to create a new output binding.</span></span>

    ![Välj Nya utdata på fliken Integrera](../media/3-new-output.jpg)

1. <span data-ttu-id="a3a94-142">Välj **Azure Blob Storage** och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-142">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="a3a94-143">Du kan behöva rulla ned för att se knappen **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-143">You may have to scroll down to see the **Select** button.</span></span>

    ![Välj Azure Blob Storage](../media/3-storage-output.jpg)

1. <span data-ttu-id="a3a94-145">Ange följande värden:</span><span class="sxs-lookup"><span data-stu-id="a3a94-145">Enter the following values:</span></span>

    | <span data-ttu-id="a3a94-146">Inställning</span><span class="sxs-lookup"><span data-stu-id="a3a94-146">Setting</span></span>      |  <span data-ttu-id="a3a94-147">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="a3a94-147">Suggested value</span></span>   | <span data-ttu-id="a3a94-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a3a94-148">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="a3a94-149">**Blobparameternamn**</span><span class="sxs-lookup"><span data-stu-id="a3a94-149">**Blob parameter name**</span></span> | <span data-ttu-id="a3a94-150">thumbnail</span><span class="sxs-lookup"><span data-stu-id="a3a94-150">thumbnail</span></span> | <span data-ttu-id="a3a94-151">Funktionen använder parametern med det här namnet för att skriva miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="a3a94-151">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="a3a94-152">**Använd funktionsreturvärde**</span><span class="sxs-lookup"><span data-stu-id="a3a94-152">**Use function return value**</span></span> | <span data-ttu-id="a3a94-153">Nej</span><span class="sxs-lookup"><span data-stu-id="a3a94-153">No</span></span> |  |
    | <span data-ttu-id="a3a94-154">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="a3a94-154">**Path**</span></span> | <span data-ttu-id="a3a94-155">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="a3a94-155">thumbnails/{name}</span></span> | <span data-ttu-id="a3a94-156">Miniatyrbilderna skrivs till en container med namnet **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-156">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="a3a94-157">**Lagringskontoanslutning**</span><span class="sxs-lookup"><span data-stu-id="a3a94-157">**Storage account connection**</span></span> | <span data-ttu-id="a3a94-158">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="a3a94-158">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="a3a94-159">Använd miljövariabeln som skapades tidigare med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-159">Use the environment variable previously created with the connection string.</span></span> |

    ![Ange inställningarna för blobutdatabindningen](../media/3-blob-output.png)


1. <span data-ttu-id="a3a94-161">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="a3a94-161">**JavaScript**</span></span>

    1. <span data-ttu-id="a3a94-162">(JavaScript) Klicka på **Avancerad redigerare** i det övre högra hörnet i fönstret för att visa den JSON som representerar bindningarna.</span><span class="sxs-lookup"><span data-stu-id="a3a94-162">(JavaScript) Click on **Advanced editor** in the top right corner of the window to reveal the JSON that represents the bindings.</span></span>

    1. <span data-ttu-id="a3a94-163">(JavaScript) Lägg till en egenskap med namnet `dataType` med värdet `binary` i `blobTrigger`-bindningen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-163">(JavaScript) In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="a3a94-164">När du gör det konfigureras bindningen så att blobinnehållet överförs till funktionen som binära data.</span><span class="sxs-lookup"><span data-stu-id="a3a94-164">This configures the binding to pass the blob contents to the function as binary data.</span></span>

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

1. <span data-ttu-id="a3a94-165">Skapa den nya bindningen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-165">Click **Save** to create the new binding.</span></span>

1. <span data-ttu-id="a3a94-166">**C#**</span><span class="sxs-lookup"><span data-stu-id="a3a94-166">**C#**</span></span>

    1. <span data-ttu-id="a3a94-167">(C#) Välj funktionsnamnet **ResizeImage** i det vänstra navigeringsfönstret för att öppna källkoden för funktionen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-167">(C#) Select the **ResizeImage** function name in the left navigation to open the function's source code.</span></span>

    1. <span data-ttu-id="a3a94-168">(C#) Funktionen kräver ett NuGet-paket med namnet **ImageResizer** för att generera miniatyrbilderna.</span><span class="sxs-lookup"><span data-stu-id="a3a94-168">(C#) The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="a3a94-169">NuGet-paket läggs till i C#-funktioner med hjälp av en **project.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="a3a94-169">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="a3a94-170">Skapa filen genom att först klicka på **Visa filer** till höger för att visa filerna som funktionen består av.</span><span class="sxs-lookup"><span data-stu-id="a3a94-170">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>
    
    1. <span data-ttu-id="a3a94-171">(C#) Klicka på **Lägg till** för att lägga till en ny fil med namnet **project.json**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-171">(C#) Click **Add** to add a new file named **project.json**.</span></span>
    
    1. <span data-ttu-id="a3a94-172">(C#) Kopiera innehållet i [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) till den nya filen som skapats.</span><span class="sxs-lookup"><span data-stu-id="a3a94-172">(C#) Copy the contents of the [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) file into the newly created file.</span></span> <span data-ttu-id="a3a94-173">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-173">Save the file.</span></span> <span data-ttu-id="a3a94-174">Paket återställs automatiskt när filen uppdateras.</span><span class="sxs-lookup"><span data-stu-id="a3a94-174">Packages are automatically restored when the file is updated.</span></span>
    
        ![project.json-fil med ImageResizer](../media/3-project-json.png)
    
    1. <span data-ttu-id="a3a94-176">(C#) Under **Visa filer**väljer du **run.csx**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-176">(C#) Under **View Files**, select **run.csx**.</span></span> <span data-ttu-id="a3a94-177">Ersätt dess innehåll med innehållet i filen [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span><span class="sxs-lookup"><span data-stu-id="a3a94-177">Replace its content with the content in the [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx) file.</span></span>

1. <span data-ttu-id="a3a94-178">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="a3a94-178">**JavaScript**</span></span> 

    1. <span data-ttu-id="a3a94-179">(JavaScript) Den här funktionen kräver `jimp`-paketet från npm för att ändra storlek på fotot.</span><span class="sxs-lookup"><span data-stu-id="a3a94-179">(JavaScript) This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="a3a94-180">Du installerar npm-paketet genom att klicka på Funktionsapp-namnet i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-180">To install the npm package, click on the Functions app name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="a3a94-181">(JavaScript) Öppna ett konsolfönster genom att klicka på **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-181">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="a3a94-182">(JavaScript) Kör kommandot `npm install jimp` i konsolen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-182">(JavaScript) Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="a3a94-183">Det kan ta några minuter att slutföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a3a94-183">It may take a few minutes to complete the operation.</span></span>

    1. <span data-ttu-id="a3a94-184">(JavaScript) Klicka på funktionsnamnet **ResizeImage** i det vänstra navigeringsfönstret för att visa funktionen.</span><span class="sxs-lookup"><span data-stu-id="a3a94-184">(JavaScript) Click on the **ResizeImage** function name in the left navigation to reveal the function.</span></span> <span data-ttu-id="a3a94-185">Byt ut hela **index.js**-filen mot innehållet i filen [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span><span class="sxs-lookup"><span data-stu-id="a3a94-185">Replace all the content in the **index.js** file with the content of the [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js) file.</span></span>

1. <span data-ttu-id="a3a94-186">Expandera loggpanelen genom att klicka på **Loggar** under kodfönstret.</span><span class="sxs-lookup"><span data-stu-id="a3a94-186">To expand the logs panel, click **Logs** below the code window.</span></span>

1. <span data-ttu-id="a3a94-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-187">Click **Save**.</span></span> <span data-ttu-id="a3a94-188">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="a3a94-188">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="a3a94-189">Testa den serverlösa funktionen</span><span class="sxs-lookup"><span data-stu-id="a3a94-189">Test the serverless function</span></span>

1. <span data-ttu-id="a3a94-190">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a3a94-190">Open the application in a browser.</span></span> <span data-ttu-id="a3a94-191">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="a3a94-191">Select an image file and upload it.</span></span> <span data-ttu-id="a3a94-192">Uppladdningen slutförs, men eftersom vi inte har lagt till möjligheten att visa bilder ännu så visar inte appen det uppladdade fotot.</span><span class="sxs-lookup"><span data-stu-id="a3a94-192">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="a3a94-193">Bekräfta i Cloud Shell att bilden har laddats upp till containern **images**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-193">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="a3a94-194">Kontrollera att miniatyrbilden har skapats i en container med namnet **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="a3a94-194">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. <span data-ttu-id="a3a94-195">Hämta URL:en för miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="a3a94-195">Get the URL for the thumbnail.</span></span>

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    <span data-ttu-id="a3a94-196">Öppna URL:en i en webbläsare och bekräfta att miniatyrbilden har skapats korrekt.</span><span class="sxs-lookup"><span data-stu-id="a3a94-196">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="a3a94-197">Ta bort alla filer i containrarna **images** och **thumbnails** innan du går vidare till nästa självstudie.</span><span class="sxs-lookup"><span data-stu-id="a3a94-197">Before continuing to the next tutorial, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```
    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="a3a94-198">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a3a94-198">Summary</span></span>

<span data-ttu-id="a3a94-199">I den här delen har du skapat en serverlös funktion som genererar en miniatyrbild varje gång en bild laddas upp till en Blob Storage-container.</span><span class="sxs-lookup"><span data-stu-id="a3a94-199">In this unit, you created a serverless function to create a thumbnail when an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="a3a94-200">I nästa avsnitt lär du dig hur du använder Azure Cosmos DB för att lagra och visa bildmetadata.</span><span class="sxs-lookup"><span data-stu-id="a3a94-200">Next, you learn how to use Azure Cosmos DB to store and list image metadata.</span></span>