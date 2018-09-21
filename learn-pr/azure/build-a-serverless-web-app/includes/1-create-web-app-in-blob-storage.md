<span data-ttu-id="8ad27-101">I den här modulen ska du distribuera ett enkelt webbprogram som visar ett HTML-baserad användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="8ad27-101">In this module, you will deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="8ad27-102">En serverlös funktion gör att programmet kan ladda upp bilder och automatiskt generera beskrivande bildtexter.</span><span class="sxs-lookup"><span data-stu-id="8ad27-102">A serverless back end enables the application to upload images and automatically generate descriptive captions.</span></span>

![Webbapp som körs](../media/0-app-screenshot-finished.png)

<span data-ttu-id="8ad27-104">I följande diagram visas de Azure-tjänster som används av programmet.</span><span class="sxs-lookup"><span data-stu-id="8ad27-104">The following illustration shows the Azure services that are used by the application.</span></span>

![<span data-ttu-id="8ad27-105">Diagram som visar hur olika Azure-tjänster, till exempel Azure Blob-storage, Azure-funktioner, Cosmos DB, Azure Logic Apps och Azure Active Directory används av programmet.</span><span class="sxs-lookup"><span data-stu-id="8ad27-105">An illustration showing how different Azure services such as, Azure Blob storage, Azure functions, Cosmos DB, Azure logic apps, and Azure active directory are used by the application.</span></span> ](../media/0-architecture.jpg)

1. <span data-ttu-id="8ad27-106">Azure Blob Storage hanterar statiskt webbinnehåll (HTML, CSS och JS) och lagrar bilder.</span><span class="sxs-lookup"><span data-stu-id="8ad27-106">Azure Blob storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="8ad27-107">Azure Functions hanterar uppladdning, storleksändring och metadatalagring för bilder.</span><span class="sxs-lookup"><span data-stu-id="8ad27-107">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="8ad27-108">Azure Cosmos DB lagrar bildmetadata.</span><span class="sxs-lookup"><span data-stu-id="8ad27-108">Azure Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="8ad27-109">Azure Logic Apps hämtar bildtexter från API för visuellt innehåll i Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="8ad27-109">Azure Logic Apps retrieves image captions from the Cognitive Services Computer Vision API.</span></span>
5. <span data-ttu-id="8ad27-110">Azure Active Directory hanterar användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="8ad27-110">Azure Active Directory manages user authentication.</span></span>

<span data-ttu-id="8ad27-111">Azure Blob Storage är en tjänst med extremt hög skalbarhet för lagring av statiska filer till låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="8ad27-111">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="8ad27-112">I den här modulen använder du Blob Storage för statiskt innehåll (till exempel HTML, JavaScript eller CSS) för en webbapp som du skapar.</span><span class="sxs-lookup"><span data-stu-id="8ad27-112">In this module, you will use Blob storage to serve static content (for example, HTML, JavaScript, or CSS) for a web app you build.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="8ad27-113">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="8ad27-113">Create an Azure Storage account</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="8ad27-114">Ett Azure Storage-konto är en Azure-resurs där du kan lagra tabeller, köer, filer, blobbar (objekt) och virtuella datordiskar.</span><span class="sxs-lookup"><span data-stu-id="8ad27-114">An Azure Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="8ad27-115">Det statiska innehållet (HTML-, CSS- och JavaScript-filer) för den här modulen finns i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8ad27-115">The static content (HTML, CSS, and JavaScript files) for this module is hosted in Blob storage.</span></span> <span data-ttu-id="8ad27-116">För Blob Storage krävs ett Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8ad27-116">Blob storage requires a Storage account.</span></span> <span data-ttu-id="8ad27-117">Skapa ett lagringskonto (generell användning v2; GPv2) i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="8ad27-117">Create a general-purpose v2 (GPv2) Storage account in the resource group.</span></span> <span data-ttu-id="8ad27-118">Ersätt `<storage account name>` med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="8ad27-118">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create \
        -n <storage account name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --kind StorageV2 \
        --https-only true \
        --sku Standard_LRS
    ```
    
1. <span data-ttu-id="8ad27-119">Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="8ad27-119">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="8ad27-120">Använd sökfältet länAnvänd sökfältet överst på portalen för att leta rätt på det lagringskonto som du just skapade.</span><span class="sxs-lookup"><span data-stu-id="8ad27-120">Use the Search bar at the top of the portal to find the storage account that you just created.</span></span> <span data-ttu-id="8ad27-121">Öppna kontot.</span><span class="sxs-lookup"><span data-stu-id="8ad27-121">Open the account.</span></span>

1. <span data-ttu-id="8ad27-122">Välj **Statisk webbplats (förhandsversion)** i det vänstra navigeringsfönstret för att konfigurera en container för lagring av statiskt webbplatsinnehåll.</span><span class="sxs-lookup"><span data-stu-id="8ad27-122">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="8ad27-123">Välj **Aktiverad** för att aktivera en statisk webbplats.</span><span class="sxs-lookup"><span data-stu-id="8ad27-123">Select **Enabled** to enable a static website.</span></span>
    - <span data-ttu-id="8ad27-124">Ange **index.html** som namn på indexdokumentet.</span><span class="sxs-lookup"><span data-stu-id="8ad27-124">Enter **index.html** as the index document name.</span></span> <span data-ttu-id="8ad27-125">I fältet står det redan *index.html* med grått teckensnitt, men det är bara exempeltext.</span><span class="sxs-lookup"><span data-stu-id="8ad27-125">The box already has *index.html* in a gray font, but this is only example text.</span></span> <span data-ttu-id="8ad27-126">Du måste ändå ange **index.html** i fältet.</span><span class="sxs-lookup"><span data-stu-id="8ad27-126">You still have to enter **index.html** in the box.</span></span>
    - <span data-ttu-id="8ad27-127">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8ad27-127">Click **Save**.</span></span>
    
    ![Ange inställningar för statisk webbplats](../media/1-storage-static-website.png)

1. <span data-ttu-id="8ad27-129">Spara den **primära slutpunkten** på en plats som du enkelt kan kopiera den från när du går igenom modulen.</span><span class="sxs-lookup"><span data-stu-id="8ad27-129">Save the **Primary Endpoint** in a place where you can conveniently copy it from while working through the module.</span></span> <span data-ttu-id="8ad27-130">Denna slutpunkt är webbadressen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8ad27-130">This endpoint is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="8ad27-131">Ladda upp webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="8ad27-131">Upload the web application</span></span>

1. <span data-ttu-id="8ad27-132">Källfilerna för programmet som du skapar i den här modulen finns på en [GitHub-lagringsplats](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span><span class="sxs-lookup"><span data-stu-id="8ad27-132">The source files for the application that you build in this module are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="8ad27-133">Gå till din hemkatalog i Cloud Shell och klona lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="8ad27-133">Go to your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="8ad27-134">Lagringsplatsen klonas till `/home/<username>/functions-first-serverless-web-application`</span><span class="sxs-lookup"><span data-stu-id="8ad27-134">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`</span></span>

1. <span data-ttu-id="8ad27-135">Webbprogrammet på klientsidan finns i mappen **www** och skapas med hjälp av JavaScript-ramverket Vue.js.</span><span class="sxs-lookup"><span data-stu-id="8ad27-135">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="8ad27-136">Växla till mappen **www** och kör **npm**-kommandon för att installera programmets beroenden och skapa programmet.</span><span class="sxs-lookup"><span data-stu-id="8ad27-136">Open the **www** folder and run **npm** commands to install the application dependencies and build the application.</span></span> <span data-ttu-id="8ad27-137">Det sista av kommandona kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="8ad27-137">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="8ad27-138">Programmet genereras i mappen **dist**.</span><span class="sxs-lookup"><span data-stu-id="8ad27-138">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="8ad27-139">Ändra aktuell katalog till mappen **dist** och ladda upp programmet till blobcontainern **$web**.</span><span class="sxs-lookup"><span data-stu-id="8ad27-139">Change the current directory to the **dist** folder and upload the application to the **$web** blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="8ad27-140">Visa programmet genom att öppna den primära slutpunktswebbadressen för den statiska webbplatsen i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="8ad27-140">To view the application, open the static website’s primary endpoint URL in a web browser.</span></span>

    ![Startsida för den första serverlösa webbappen](../media/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="8ad27-142">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8ad27-142">Summary</span></span>

<span data-ttu-id="8ad27-143">I den här enheten skapade du ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="8ad27-143">In this unit, you created a storage account.</span></span> <span data-ttu-id="8ad27-144">En blob-container med namnet **$web** i lagringskontot lagrar webbappens statiska innehåll och gör innehållet offentligt tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="8ad27-144">A blob container named **$web** in the storage account stores the static content for your web application and makes the content publicly available.</span></span> <span data-ttu-id="8ad27-145">I nästa avsnitt lär du dig använda en serverlös funktion för att ladda upp bilder till Blob Storage från det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8ad27-145">Next, you will learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>