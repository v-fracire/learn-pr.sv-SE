<span data-ttu-id="e1528-101">I den här övningen använder du Azure-portalen för att skapa ett lagringskonto som är lämpligt för en fiktiv webbapp för surfrapporter i södra Kalifornien.</span><span class="sxs-lookup"><span data-stu-id="e1528-101">In this exercise, you will use the Azure portal to create a storage account that is appropriate for a fictitious southern California surf report Web App.</span></span>

## <a name="design-goals"></a><span data-ttu-id="e1528-102">Designmål</span><span class="sxs-lookup"><span data-stu-id="e1528-102">Design goals</span></span>

<span data-ttu-id="e1528-103">Webbplatsen med surfrapporter kan användas till att ladda upp foton och videoklipp som visar förhållandena på lokala surfplatser.</span><span class="sxs-lookup"><span data-stu-id="e1528-103">The surf-report site lets users upload photos and videos of their local beach conditions.</span></span> <span data-ttu-id="e1528-104">Tittarna använder innehållet för att välja den strand som har de bästa surfvillkoren.</span><span class="sxs-lookup"><span data-stu-id="e1528-104">Viewers will use the content to help them choose the beach with the best surfing conditions.</span></span> <span data-ttu-id="e1528-105">Din lista över design- och funktionsmål är:</span><span class="sxs-lookup"><span data-stu-id="e1528-105">Your list of design and feature goals is:</span></span>

- <span data-ttu-id="e1528-106">Videoinnehåll måste läsas in snabbt</span><span class="sxs-lookup"><span data-stu-id="e1528-106">Video content must load quickly</span></span>
- <span data-ttu-id="e1528-107">Platsen måste kunna hantera oväntade toppar i uppladdningsvolym</span><span class="sxs-lookup"><span data-stu-id="e1528-107">The site must handle unexpected spikes in upload volume</span></span>
- <span data-ttu-id="e1528-108">Inaktuellt innehåll tas bort allteftersom surfvillkoren ändras så att webbplatsen alltid visar aktuella villkor</span><span class="sxs-lookup"><span data-stu-id="e1528-108">Outdated content will be removed as surf conditions change so the site always shows current conditions</span></span>

<span data-ttu-id="e1528-109">Du väljer en implementering som buffrar uppladdat innehåll i en Azure-kö för bearbetning och sedan flyttar det till en Azure-blob för lagring.</span><span class="sxs-lookup"><span data-stu-id="e1528-109">You decide on an implementation that buffers uploaded content in an Azure Queue for processing and then moves it into an Azure Blob for storage.</span></span> <span data-ttu-id="e1528-110">Du behöver ett lagringskonto som kan innehålla både köer och blobar samtidigt som det ger åtkomst med kort svarstid till ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="e1528-110">You need a storage account that can hold both Queues and Blobs while delivering low-latency access to your content.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="e1528-111">Övningssteg</span><span class="sxs-lookup"><span data-stu-id="e1528-111">Exercise steps</span></span>

### <a name="launch-the-blade"></a><span data-ttu-id="e1528-112">Starta bladet</span><span class="sxs-lookup"><span data-stu-id="e1528-112">Launch the blade</span></span>

1. <span data-ttu-id="e1528-113">I en webbläsare navigerar du till [Azure-portalen](https://portal.azure.com?azure-portal=true) och loggar in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="e1528-113">In your web browser, navigate to the [Azure Portal](https://portal.azure.com?azure-portal=true) and sign into your account.</span></span>

1. <span data-ttu-id="e1528-114">I sidofältet till vänster väljer du **Skapa en resurs**.</span><span class="sxs-lookup"><span data-stu-id="e1528-114">In the left sidebar, select **Create a resource**.</span></span>

1. <span data-ttu-id="e1528-115">Välj rubriken **Lagring** på Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e1528-115">Select the **Storage** heading in the Azure Marketplace.</span></span>

1. <span data-ttu-id="e1528-116">Välj **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="e1528-116">Select **Storage account**.</span></span> <span data-ttu-id="e1528-117">Portalen visar bladet **Skapa lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="e1528-117">The portal will display the **Create storage account** blade.</span></span>

### <a name="configure-the-basic-options"></a><span data-ttu-id="e1528-118">Konfigurera de grundläggande alternativen</span><span class="sxs-lookup"><span data-stu-id="e1528-118">Configure the basic options</span></span>

1. <span data-ttu-id="e1528-119">Välj fliken **Grundläggande inställningar** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="e1528-119">Select the **Basics** tab at the top of the blade.</span></span>

1. <span data-ttu-id="e1528-120">**Prenumeration:** Välj en av dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="e1528-120">**Subscription**: Select one of your subscriptions.</span></span>

1. <span data-ttu-id="e1528-121">**Resursgrupp**: Skapa en ny resursgrupp med namnet **SurfReportResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="e1528-121">**Resource group**: Create a new resource group named **SurfReportResourceGroup**.</span></span>

1. <span data-ttu-id="e1528-122">**Namn på lagringskonto**: Ange ett globalt unikt värde som `surfreport` plus dina initialer plus en siffra.</span><span class="sxs-lookup"><span data-stu-id="e1528-122">**Storage account name**: Enter a globally-unique value like `surfreport` + your initials + a number.</span></span>

 1. <span data-ttu-id="e1528-123">**Plats**: Välj **USA, västra**.</span><span class="sxs-lookup"><span data-stu-id="e1528-123">**Location**: Select **West US**.</span></span>

    <span data-ttu-id="e1528-124">Anledning: Programmet är avsett för användare i södra Kalifornien.</span><span class="sxs-lookup"><span data-stu-id="e1528-124">Rationale: The application is intended for users in southern California.</span></span> <span data-ttu-id="e1528-125">För att minimera svarstiden vid inläsning av videor ska blobarna hanteras nära dessa användare. Detta gör **USA, västra** till ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="e1528-125">To minimize latency when loading videos, the Blobs should be hosted close to these users; this makes **West US** a good choice.</span></span>

1. <span data-ttu-id="e1528-126">**Distributionsmodell**: Välj **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="e1528-126">**Deployment model**: Select **Resource manager**.</span></span>
    
    <span data-ttu-id="e1528-127">Anledning: **Resurshanterare** är lämplig eftersom den gör att du kan använda en resursgrupp för att hantera webbappen, lagringskontot osv. för programmet.</span><span class="sxs-lookup"><span data-stu-id="e1528-127">Rationale: **Resource manager** is appropriate because it will let you use a resource group to manage the Web App, storage account, etc. for the application.</span></span>

1. <span data-ttu-id="e1528-128">**Prestanda**: Välj **Standard**.</span><span class="sxs-lookup"><span data-stu-id="e1528-128">**Performance**: Select **Standard**.</span></span>

    <span data-ttu-id="e1528-129">Anledning: Du kan inte använda alternativet **Premium** eftersom det skulle begränsa lagringskontot sidblobar.</span><span class="sxs-lookup"><span data-stu-id="e1528-129">Rationale: You cannot use the **Premium** option because it would limit the storage account to page Blobs.</span></span> <span data-ttu-id="e1528-130">Du behöver blockblobar för dina videor och en kö för buffring. Båda dessa är bara tillgängliga i alternativet **Standard**.</span><span class="sxs-lookup"><span data-stu-id="e1528-130">You need block Blobs for your videos and a Queue for buffering both of which are only available in the **Standard** option.</span></span>

1. <span data-ttu-id="e1528-131">**Typ av konto**: Välj **StorageV2 (generell användning v2)**.</span><span class="sxs-lookup"><span data-stu-id="e1528-131">**Account kind**: Select **StorageV2 (general purpose v2)**.</span></span>

    <span data-ttu-id="e1528-132">Anledning: **StorageV2 (generell användning v2)** är det rätta valet i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="e1528-132">Rationale: **StorageV2 (general purpose v2)** is the right choice here.</span></span> <span data-ttu-id="e1528-133">Du behöver en blandning av blobar och en kö, så alternativet **Blob-lagring** fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="e1528-133">You need a mix of Blobs and a Queue, so the **Blob storage** option will not work.</span></span> <span data-ttu-id="e1528-134">För det här programmet skulle det inte finnas någon fördel med att välja ett konto för **Storage (generell användning v1)** eftersom det skulle begränsa de funktioner du kan komma åt och förmodligen inte skulle minska kostnaden för den förväntade arbetsbelastningen.</span><span class="sxs-lookup"><span data-stu-id="e1528-134">For this application, there would be no benefit to choosing a **Storage (general purpose v1)** account since that would limit the features you could access and would be unlikely to reduce the cost of your expected workload.</span></span>

1. <span data-ttu-id="e1528-135">**Replikering**: Välj **Lokalt redundant lagring (LRS)**.</span><span class="sxs-lookup"><span data-stu-id="e1528-135">**Replication**: Select **Locally-redundant storage (LRS)**.</span></span>

    <span data-ttu-id="e1528-136">Anledning: Bilderna och videorna blir snabbt inaktuella och tas bort från platsen.</span><span class="sxs-lookup"><span data-stu-id="e1528-136">Rationale: The images and videos quickly become out-of-date and are removed from the site.</span></span> <span data-ttu-id="e1528-137">Det innebär att det inte finns någon större poäng med att betala extra för global redundans.</span><span class="sxs-lookup"><span data-stu-id="e1528-137">This means there is little value to paying extra for global redundancy.</span></span> <span data-ttu-id="e1528-138">I händelse av en katastrof som leder till dataförlust kan du starta om webbplatsen med nytt innehåll från användarna.</span><span class="sxs-lookup"><span data-stu-id="e1528-138">If a catastrophic event results in data loss, you can restart the site with fresh content from your users.</span></span>

1. <span data-ttu-id="e1528-139">**Åtkomstnivå (Standard)**: Välj **Frekvent**.</span><span class="sxs-lookup"><span data-stu-id="e1528-139">**Access tier (default)**: Select **Hot**.</span></span>
   
    <span data-ttu-id="e1528-140">Anledning: Du vill att videor ska läsas in snabbt, och därför använder du alternativet med höga prestanda för dina blobar.</span><span class="sxs-lookup"><span data-stu-id="e1528-140">Rationale: You want the videos to load quickly so you will use the high-performance option for your Blobs.</span></span>
   
<span data-ttu-id="e1528-141">Följande skärmbild visar slutförda inställningar för fliken **Grundläggande inställningar**.</span><span class="sxs-lookup"><span data-stu-id="e1528-141">The following screenshot shows the completed settings for the **Basics** tab.</span></span>

![Skärmbild av ett blad för att skapa ett lagringskonto med fliken **Grundläggande inställningar** markerad.](../media-drafts/5-create-storage-account-basics.png)

### <a name="configure-the-advanced-options"></a><span data-ttu-id="e1528-143">Konfigurera avancerade alternativ</span><span class="sxs-lookup"><span data-stu-id="e1528-143">Configure the advanced options</span></span>

1. <span data-ttu-id="e1528-144">Välj fliken **Avancerat** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="e1528-144">Select the **Advanced** tab at the top of the blade.</span></span>

1. <span data-ttu-id="e1528-145">**Säker överföring krävs**: Välj **Aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="e1528-145">**Secure transfer required**: Select **Enabled**.</span></span>

    <span data-ttu-id="e1528-146">Anledning: Https via kabeln anses allmänt vara bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="e1528-146">Rationale: Https across the wire is generally considered best practice.</span></span>

1. <span data-ttu-id="e1528-147">**Virtuella nätverk**: Välj **Inaktiverat**.</span><span class="sxs-lookup"><span data-stu-id="e1528-147">**Virtual Networks**: Select **Disabled**.</span></span>

    <span data-ttu-id="e1528-148">Anledning: Innehållet är offentligt, och du behöver tillåta åtkomst från offentliga klienter.</span><span class="sxs-lookup"><span data-stu-id="e1528-148">Rationale: The content is public facing and you need to allow access from public clients.</span></span>

<span data-ttu-id="e1528-149">Följande skärmbild visar slutförda inställningar för fliken **Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="e1528-149">The following screenshot shows the completed settings for the **Advanced** tab.</span></span>

![Skärmbild av ett blad för att skapa ett lagringskonto med fliken **Avancerat** markerad.](../media-drafts/5-create-storage-account-advanced.png)

### <a name="create"></a><span data-ttu-id="e1528-151">Skapa</span><span class="sxs-lookup"><span data-stu-id="e1528-151">Create</span></span>

1. <span data-ttu-id="e1528-152">Klicka på knappen **Granska + skapa** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="e1528-152">Click the **Review + create** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="e1528-153">På nästa skärm klickar du på knappen **Skapa** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="e1528-153">On the next screen, click the **Create** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="e1528-154">Vänta tills resursen skapas.</span><span class="sxs-lookup"><span data-stu-id="e1528-154">Wait for the resource to be created.</span></span>

### <a name="verify"></a><span data-ttu-id="e1528-155">Verifiera</span><span class="sxs-lookup"><span data-stu-id="e1528-155">Verify</span></span>

1. <span data-ttu-id="e1528-156">Välj länken **Lagringskonton** i det vänstra sidofältet.</span><span class="sxs-lookup"><span data-stu-id="e1528-156">Select the **Storage accounts** link in the left sidebar.</span></span>

1. <span data-ttu-id="e1528-157">Leta upp det nya lagringskontot i listan för att verifiera skapandet.</span><span class="sxs-lookup"><span data-stu-id="e1528-157">Locate the new storage account in the list to verify that creation succeeded.</span></span>

### <a name="clean-up"></a><span data-ttu-id="e1528-158">Rensa</span><span class="sxs-lookup"><span data-stu-id="e1528-158">Clean up</span></span>

1. <span data-ttu-id="e1528-159">Välj länken **Resursgrupper** i det vänstra sidofältet.</span><span class="sxs-lookup"><span data-stu-id="e1528-159">Select the **Resource groups** link in the left sidebar.</span></span>

1. <span data-ttu-id="e1528-160">Leta upp **SurfReportResourceGroup** i listan.</span><span class="sxs-lookup"><span data-stu-id="e1528-160">Locate **SurfReportResourceGroup** in the list.</span></span>

1. <span data-ttu-id="e1528-161">Högerklicka på posten **SurfReportResourceGroup** och välj **Ta bort resursgrupp** på snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="e1528-161">Right-click on the **SurfReportResourceGroup** entry and select **Delete resource group** from the context menu.</span></span>

1. <span data-ttu-id="e1528-162">Ange resursgruppens namn i bekräftelsefältet.</span><span class="sxs-lookup"><span data-stu-id="e1528-162">Type the resource group name into the confirmation field.</span></span>

1. <span data-ttu-id="e1528-163">Klicka på knappen **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e1528-163">Click the **Delete** button.</span></span>

## <a name="summary"></a><span data-ttu-id="e1528-164">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e1528-164">Summary</span></span>

<span data-ttu-id="e1528-165">Du har skapat ett lagringskonto med inställningar som styrs av dina affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="e1528-165">You created a storage account with settings driven by your business requirements.</span></span> <span data-ttu-id="e1528-166">Till exempel valde du ett datacenter i USA, västra eftersom dina kunder främst fanns i södra Kalifornien.</span><span class="sxs-lookup"><span data-stu-id="e1528-166">For example, you selected a West US datacenter because your customers were primarily located in southern California.</span></span> <span data-ttu-id="e1528-167">Det här är ett typiskt flöde: först analyserar du dina data och mål och sedan konfigurerar du alternativen för lagringskonton så att de matchar.</span><span class="sxs-lookup"><span data-stu-id="e1528-167">This is a typical flow: first analyze your data and goals and then configure the storage account options to match.</span></span>