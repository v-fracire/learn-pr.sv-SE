<span data-ttu-id="ed432-101">I den här modulen ska du distribuera ett enkelt webbprogram som visar ett HTML-baserad användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ed432-101">In this module, you will deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="ed432-102">En serverlös funktion gör att programmet kan ladda upp bilder och automatiskt hämta bildtexter som beskriver bilderna.</span><span class="sxs-lookup"><span data-stu-id="ed432-102">A serverless back end enables the application to upload images and automatically get captions describing them.</span></span>

![Webbapp som körs](../images/0-app-screenshot-finished.png)

<span data-ttu-id="ed432-104">I följande diagram visas de Azure-tjänster som används av programmet:</span><span class="sxs-lookup"><span data-stu-id="ed432-104">The following diagram shows the Azure services used by the application:</span></span>

1. <span data-ttu-id="ed432-105">Blob Storage hanterar statiskt webbinnehåll (HTML, CSS och JS) och lagrar bilder.</span><span class="sxs-lookup"><span data-stu-id="ed432-105">Blob Storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="ed432-106">Azure Functions hanterar uppladdning, storleksändring och metadatalagring för bilder.</span><span class="sxs-lookup"><span data-stu-id="ed432-106">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="ed432-107">Cosmos DB lagrar bildmetadata.</span><span class="sxs-lookup"><span data-stu-id="ed432-107">Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="ed432-108">Logic Apps hämtar bildtexter från API:et för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="ed432-108">Logic Apps gets image captions from Computer Vision API.</span></span>
5. <span data-ttu-id="ed432-109">Azure Active Directory hanterar användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="ed432-109">Azure Active Directory manages user authentication.</span></span>

![Diagram över lösningsarkitektur](../images/0-architecture.jpg)

<span data-ttu-id="ed432-111">I den här kursdelen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="ed432-111">In this unit, you will learn how to:</span></span>
- <span data-ttu-id="ed432-112">Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.</span><span class="sxs-lookup"><span data-stu-id="ed432-112">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
- <span data-ttu-id="ed432-113">Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="ed432-113">Upload images to Azure Blob storage using Azure Functions.</span></span>
- <span data-ttu-id="ed432-114">Ändra storlek på bilder med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="ed432-114">Resize images using Azure Functions.</span></span>
- <span data-ttu-id="ed432-115">Lagra bildmetadata i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ed432-115">Store image metadata in Azure Cosmos DB.</span></span>
- <span data-ttu-id="ed432-116">Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ed432-116">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
- <span data-ttu-id="ed432-117">Använda Azure Active Directory för att skydda webbappen genom att autentisera användare.</span><span class="sxs-lookup"><span data-stu-id="ed432-117">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="ed432-118">Azure Blob Storage är en tjänst med extremt hög skalbarhet för lagring av statiska filer till låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="ed432-118">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="ed432-119">I den här självstudien använder du tjänsten för att hantera statiskt innehåll (till exempel HTML, JavaScript, CSS) för en webbapp som du skapar.</span><span class="sxs-lookup"><span data-stu-id="ed432-119">For this tutorial, you use it to serve static content (for example, HTML, JavaScript, CSS) for the web app that you build.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="ed432-120">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="ed432-120">Create a Storage account</span></span>

<span data-ttu-id="ed432-121">Ett lagringskonto är en Azure-resurs där du kan lagra tabeller, köer, filer, blobbar (objekt) och VM-diskar.</span><span class="sxs-lookup"><span data-stu-id="ed432-121">A Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="ed432-122">Logga in i Cloud Shell (Bash) genom att välja knappen **Enter focus mode** (växla till fokusläge).</span><span class="sxs-lookup"><span data-stu-id="ed432-122">Log in to the Cloud Shell (Bash), by selecting the **Enter focus mode** button.</span></span> <span data-ttu-id="ed432-123">Du hittar knappen längst upp till höger eller längst ned på sidan, beroende på hur stort ditt webbläsarfönster är.</span><span class="sxs-lookup"><span data-stu-id="ed432-123">This button is at the top right or the bottom of the page, depending on how wide your browser window is.</span></span> <span data-ttu-id="ed432-124">I fokusläge dockas Cloud Shell-fönstret till höger i webbläsarfönstret, så att du enkelt kan köra kommandona som beskrivs i självstudien.</span><span class="sxs-lookup"><span data-stu-id="ed432-124">Focus mode docks a Cloud Shell window on the right side of your browser window, so you can easily execute commands that are shown in the tutorial.</span></span>

1. <span data-ttu-id="ed432-125">I Azure är en resursgrupp en container med relaterade Azure-resurser som underlättar hanteringen.</span><span class="sxs-lookup"><span data-stu-id="ed432-125">In Azure, a Resource Group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="ed432-126">Skapa en ny resursgrupp med namnet **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="ed432-126">Create a new resource group named **first-serverless-app**.</span></span>

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. <span data-ttu-id="ed432-127">Det statiska innehållet (HTML-, CSS- och JavaScript-filer) för den här självstudiekursen finns i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ed432-127">The static content (HTML, CSS, and JavaScript files) for this tutorial is hosted in Blob Storage.</span></span> <span data-ttu-id="ed432-128">Blob Storage kräver ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ed432-128">Blob Storage requires a Storage account.</span></span> <span data-ttu-id="ed432-129">Skapa ett lagringskonto (generell användning V2) i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ed432-129">Create a Storage account (general purpose V2) in the resource group.</span></span> <span data-ttu-id="ed432-130">Ersätt `<storage account name>` med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="ed432-130">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. <span data-ttu-id="ed432-131">Använd sökfältet överst på [Azure Portal](https://portal.azure.com?azure-portal=true) för att leta upp lagringskontot som du precis skapat och öppna det.</span><span class="sxs-lookup"><span data-stu-id="ed432-131">Using the Search bar at the top of the [Azure portal](https://portal.azure.com?azure-portal=true), find the storage account you just created and open it.</span></span>

1. <span data-ttu-id="ed432-132">Välj **Statisk webbplats (förhandsversion)** i det vänstra navigeringsfönstret för att konfigurera en container för lagring av statiskt webbplatsinnehåll.</span><span class="sxs-lookup"><span data-stu-id="ed432-132">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="ed432-133">Välj **Aktiverad** för att aktivera Statisk webbplats.</span><span class="sxs-lookup"><span data-stu-id="ed432-133">Select **Enabled** to enable static website.</span></span>
    - <span data-ttu-id="ed432-134">Ange *index.html* som namn på indexdokumentet.</span><span class="sxs-lookup"><span data-stu-id="ed432-134">Enter *index.html* as the index document name.</span></span> <span data-ttu-id="ed432-135">Fältet innehåller redan *index.html* med grå teckenfärg, men detta är endast en exempeltext. Du måste fortfarande ange värdet i fältet.</span><span class="sxs-lookup"><span data-stu-id="ed432-135">The field already has *index.html* in a gray font but this is only example text; you still have to enter that value in the field.</span></span>
    - <span data-ttu-id="ed432-136">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ed432-136">Click **Save**.</span></span>
    
    ![Ange inställningarna för Statisk webbplats](../images/1-storage-static-website.png)

1. <span data-ttu-id="ed432-138">Spara den **primära slutpunkten** på en plats som du enkelt kan kopiera den från när du går igenom självstudien.</span><span class="sxs-lookup"><span data-stu-id="ed432-138">Save the **Primary Endpoint** somewhere you can conveniently copy it from while working through the tutorial.</span></span> <span data-ttu-id="ed432-139">Det här är URL:en för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ed432-139">This is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="ed432-140">Ladda upp webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="ed432-140">Upload the web application</span></span>

1. <span data-ttu-id="ed432-141">Källfilerna för programmet som du skapar i den här självstudien finns på en [GitHub-lagringsplats](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span><span class="sxs-lookup"><span data-stu-id="ed432-141">The source files for the application that you build in this tutorial are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="ed432-142">Kontrollera att du är i din hemkatalog i Cloud Shell och klona lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ed432-142">Make sure that you are in your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="ed432-143">Lagringsplatsen klonas till `/home/<username>/functions-first-serverless-web-application`.</span><span class="sxs-lookup"><span data-stu-id="ed432-143">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`.</span></span>

1. <span data-ttu-id="ed432-144">Webbprogrammet på klientsidan finns i mappen **www** och skapas med hjälp av JavaScript-ramverket Vue.js.</span><span class="sxs-lookup"><span data-stu-id="ed432-144">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="ed432-145">Växla till mappen och kör npm-kommandon för att installera programmets beroenden och skapa programmet.</span><span class="sxs-lookup"><span data-stu-id="ed432-145">Change into the folder and run npm commands to install the application's dependencies and build the application.</span></span> <span data-ttu-id="ed432-146">Det sista av dessa kommandon kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="ed432-146">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="ed432-147">Programmet genereras i mappen **dist**.</span><span class="sxs-lookup"><span data-stu-id="ed432-147">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="ed432-148">Ändra den aktuella katalogen till **dist** och ladda upp programmet till blobbcontainern **$web**.</span><span class="sxs-lookup"><span data-stu-id="ed432-148">Change the current directory to **dist** and upload the application to the **$web** Blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="ed432-149">Visa programmet genom att öppna URL:en för den primära slutpunkten för statiska webbplatser i lagringskontot i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ed432-149">Open the Storage static websites primary endpoint URL in a web browser to view the application.</span></span>

    ![Startsidan för den första serverlösa webbappen](../images/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="ed432-151">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ed432-151">Summary</span></span>

<span data-ttu-id="ed432-152">I den här kursdelen har du skapat en resursgrupp med namnet **first-serverless-app** som innehåller ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ed432-152">In this unit, you created a resource group named **first-serverless-app** containing a Storage account.</span></span> <span data-ttu-id="ed432-153">En blobcontainer med namnet **$web** i lagringskontot lagrar webbappens statiska innehåll och gör innehållet offentligt tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="ed432-153">A blob container named **$web** in the Storage account stores the static content for your web application and makes the content available publicly.</span></span> <span data-ttu-id="ed432-154">I nästa avsnitt lär du dig hur du använder en serverlös funktion för att ladda upp bilder till Blob Storage från det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ed432-154">Next, you learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>