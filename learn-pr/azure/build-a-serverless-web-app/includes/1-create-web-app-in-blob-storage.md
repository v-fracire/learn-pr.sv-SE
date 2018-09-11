<span data-ttu-id="9bd4f-101">I den här modulen ska du distribuera ett enkelt webbprogram som visar ett HTML-baserad användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-101">In this module, you will deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="9bd4f-102">Med en serverlös funktion kan programmet ladda upp bilder och automatiskt hämta bildtexter som beskriver bilderna.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-102">A serverless back end enables the application to upload images and automatically get captions that describe them.</span></span>

![Webbapp som körs](../media/0-app-screenshot-finished.png)

<span data-ttu-id="9bd4f-104">I följande diagram visas de Azure-tjänster som används av programmet.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-104">The following diagram shows the Azure services that are used by the application.</span></span>

1. <span data-ttu-id="9bd4f-105">Azure Blob Storage hanterar statiskt webbinnehåll (HTML, CSS och JS) och lagrar bilder.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-105">Azure Blob storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="9bd4f-106">Azure Functions hanterar uppladdning, storleksändring och metadatalagring för bilder.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-106">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="9bd4f-107">Azure Cosmos DB lagrar bildmetadata.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-107">Azure Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="9bd4f-108">Azure Logic Apps hämtar bildtexter från API:et för visuellt innehåll i Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-108">Azure Logic Apps gets image captions from the Cognitive Services Computer Vision API.</span></span>
5. <span data-ttu-id="9bd4f-109">Azure Active Directory hanterar användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-109">Azure Active Directory manages user authentication.</span></span>

![Diagram över lösningsarkitektur](../media/0-architecture.jpg)

<span data-ttu-id="9bd4f-111">I den här kursdelen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="9bd4f-111">In this unit, you will learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="9bd4f-112">Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-112">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="9bd4f-113">Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-113">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="9bd4f-114">Ändra storlek på bilder med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-114">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="9bd4f-115">Lagra bildmetadata i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-115">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="9bd4f-116">Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-116">Use the Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="9bd4f-117">Använd Azure Active Directory för att skydda webbappen med användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-117">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="9bd4f-118">Azure Blob Storage är en tjänst med extremt hög skalbarhet för lagring av statiska filer till låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-118">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="9bd4f-119">I den här självstudien använder du tjänsten för att hantera statiskt innehåll (till exempel HTML, JavaScript, CSS) för en webbapp som du skapar.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-119">For this tutorial, you use it to serve static content (for example, HTML, JavaScript, CSS) for the web app that you build.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="9bd4f-120">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="9bd4f-120">Create an Azure Storage account</span></span>

<span data-ttu-id="9bd4f-121">Ett Azure Storage-konto är en Azure-resurs där du kan lagra tabeller, köer, filer, blobbar (objekt) och VM-diskar.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-121">An Azure Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="9bd4f-122">Välj knappen **Enter focus mode** (Använd fokusläge) för att starta Azure Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="9bd4f-122">Select the **Enter focus mode** button to launch Azure Cloud Shell (Bash).</span></span> <span data-ttu-id="9bd4f-123">Knappen finns längst upp till höger eller längst ned på sidan, beroende på hur stort ditt webbläsarfönster är.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-123">This button is at the top right or the bottom of the page, depending on how wide your browser window is.</span></span> <span data-ttu-id="9bd4f-124">I fokusläge dockas Cloud Shell-fönstret till höger i webbläsarfönstret, så att du enkelt kan köra kommandona som beskrivs i självstudien.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-124">Focus mode docks a Cloud Shell window on the right side of your browser window, so you can easily execute commands that are shown in the tutorial.</span></span>

1. <span data-ttu-id="9bd4f-125">I Azure är en resursgrupp en container med relaterade Azure-resurser som underlättar hanteringen.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-125">In Azure, a resource group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="9bd4f-126">Skapa en ny resursgrupp med namnet **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-126">Create a new resource group named **first-serverless-app**.</span></span>

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. <span data-ttu-id="9bd4f-127">Det statiska innehållet (HTML-, CSS- och JavaScript-filer) för den här självstudiekursen finns i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-127">The static content (HTML, CSS, and JavaScript files) for this tutorial is hosted in Blob storage.</span></span> <span data-ttu-id="9bd4f-128">För Blob Storage krävs ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-128">Blob storage requires a Storage account.</span></span> <span data-ttu-id="9bd4f-129">Skapa ett lagringskonto (generell användning V2) i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-129">Create a Storage account (general-purpose V2) in the resource group.</span></span> <span data-ttu-id="9bd4f-130">Ersätt `<storage account name>` med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-130">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```
    
1. <span data-ttu-id="9bd4f-131">Använd sökfältet överst i [Azure Portal](https://portal.azure.com) för att leta rätt på det lagringskonto som du just skapade.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-131">Use the Search bar at the top of the [Azure portal](https://portal.azure.com) to find the storage account that you just created.</span></span> <span data-ttu-id="9bd4f-132">Öppna kontot.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-132">Open the account.</span></span>

1. <span data-ttu-id="9bd4f-133">Välj **Statisk webbplats (förhandsversion)** i det vänstra navigeringsfönstret för att konfigurera en container för lagring av statiskt webbplatsinnehåll.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-133">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="9bd4f-134">Välj **Aktiverad** för att aktivera en statisk webbplats.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-134">Select **Enabled** to enable a static website.</span></span>
    - <span data-ttu-id="9bd4f-135">Ange **index.html** som namn på indexdokumentet.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-135">Enter **index.html** as the index document name.</span></span> <span data-ttu-id="9bd4f-136">I fältet står det redan *index.html* med grått teckensnitt, men det är bara exempeltext.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-136">The box already has *index.html* in a gray font, but this is only example text.</span></span> <span data-ttu-id="9bd4f-137">Du måste ändå ange **index.html** i fältet.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-137">You still have to enter **index.html** in the box.</span></span>
    - <span data-ttu-id="9bd4f-138">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-138">Click **Save**.</span></span>
    
    ![Ange inställningar för statisk webbplats](../media/1-storage-static-website.png)

1. <span data-ttu-id="9bd4f-140">Spara den **primära slutpunkten** på en plats som du enkelt kan kopiera den från när du går igenom självstudien.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-140">Save the **Primary Endpoint** in a place where you can conveniently copy it from while working through the tutorial.</span></span> <span data-ttu-id="9bd4f-141">Denna slutpunkt är webbadressen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-141">This endpoint is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="9bd4f-142">Ladda upp webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="9bd4f-142">Upload the web application</span></span>

1. <span data-ttu-id="9bd4f-143">Källfilerna för programmet som du skapar i den här självstudien finns på en [GitHub-lagringsplats](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span><span class="sxs-lookup"><span data-stu-id="9bd4f-143">The source files for the application that you build in this tutorial are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="9bd4f-144">Kontrollera att du är i din hemkatalog i Cloud Shell och klona lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-144">Make sure that you're in your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="9bd4f-145">Lagringsplatsen klonas till `/home/<username>/functions-first-serverless-web-application`.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-145">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`.</span></span>

1. <span data-ttu-id="9bd4f-146">Webbprogrammet på klientsidan finns i mappen **www** och skapas med hjälp av JavaScript-ramverket Vue.js.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-146">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="9bd4f-147">Växla till mappen och kör **npm**-kommandon för att installera programmets beroenden och skapa programmet.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-147">Change into the folder and run **npm** commands to install the application dependencies and build the application.</span></span> <span data-ttu-id="9bd4f-148">Det sista av kommandona kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-148">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="9bd4f-149">Programmet genereras i mappen **dist**.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-149">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="9bd4f-150">Ändra aktuell katalog till mappen **dist** och ladda upp programmet till blobcontainern **$web**.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-150">Change the current directory to the **dist** folder and upload the application to the **$web** blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="9bd4f-151">Visa programmet genom att öppna webbadressen för den primära slutpunkten för statiska webbplatser i lagringskontot i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-151">To view the application, open the Storage static websites primary endpoint URL in a web browser.</span></span>

    ![Startsida för den första serverlösa webbappen](../media/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="9bd4f-153">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9bd4f-153">Summary</span></span>

<span data-ttu-id="9bd4f-154">I den här kursdelen har du skapat en resursgrupp med namnet **first-serverless-app** som innehåller ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-154">In this unit, you created a resource group named **first-serverless-app** that contains a Storage account.</span></span> <span data-ttu-id="9bd4f-155">En blobcontainer med namnet **$web** i lagringskontot lagrar webbappens statiska innehåll och gör innehållet offentligt tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-155">A blob container named **$web** in the Storage account stores the static content for your web application and makes the content publicly available.</span></span> <span data-ttu-id="9bd4f-156">I nästa avsnitt lär du dig använda en serverlös funktion för att ladda upp bilder till Blob Storage från det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9bd4f-156">Next, you learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>