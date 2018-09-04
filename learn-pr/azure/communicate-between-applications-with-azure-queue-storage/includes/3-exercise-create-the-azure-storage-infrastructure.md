<span data-ttu-id="30b6b-101">I den här övningen skapar du ett lagringskonto som är lämpligt för användning med en kö.</span><span class="sxs-lookup"><span data-stu-id="30b6b-101">In this exercise, you will create a Storage Account that is appropriate for use with a queue.</span></span>

<span data-ttu-id="30b6b-102">Anta att du arbetar för en nyhetsorganisation som får artiklar, blogginlägg och recensioner från ett världsomspännande nätverk med journalister.</span><span class="sxs-lookup"><span data-stu-id="30b6b-102">Suppose you work for a news organization that receives articles, blog posts, and reviews from a worldwide network of journalists.</span></span> <span data-ttu-id="30b6b-103">Artikelbidrag kan överbelasta systemet när globalt viktiga händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="30b6b-103">Article submissions can overwhelm the system whenever globally significant events occur.</span></span> <span data-ttu-id="30b6b-104">För att hantera detta har du bestämt dig för att lägga till en kö mellan klientdelen och mellannivån i ditt program för artikeluppladdning.</span><span class="sxs-lookup"><span data-stu-id="30b6b-104">To deal with this, you've decided to add a queue between the front-end and the middle tier in your article-upload application.</span></span> 

<span data-ttu-id="30b6b-105">Köer är en del av Azure-lagringskonton av typen generell användning, så du måste börja med att skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="30b6b-105">Queues are part of Azure general purpose Storage Accounts, so you must start by creating a Storage Account.</span></span>

## <a name="steps"></a><span data-ttu-id="30b6b-106">Steg</span><span class="sxs-lookup"><span data-stu-id="30b6b-106">Steps</span></span>

<span data-ttu-id="30b6b-107">Skapa ett lagringskonto och en resursgrupp genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="30b6b-107">To create a storage account and a resource group, follow these steps:</span></span>

1. <span data-ttu-id="30b6b-108">Logga in på [Azure-portalen](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="30b6b-108">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="30b6b-109">I det övre vänstra hörnet klickar du på **Alla tjänster**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-109">In the top left, click **All services**.</span></span>

1. <span data-ttu-id="30b6b-110">Rulla ned till avsnittet **Lagring** och klicka på **Lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="30b6b-110">Scroll down to the **Storage** section, and then click **Storage accounts**</span></span>

    ![Skapa ett lagringskonto](../media-draft/3-create-storage-account-1.png)

1. <span data-ttu-id="30b6b-112">Längst upp till vänster på bladet **lagringskonton** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-112">In the top left of the **Storage accounts** blade, click **Add**.</span></span>

1. <span data-ttu-id="30b6b-113">Ange ett unikt namn för lagringskontot i textrutan **Namn**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-113">In the **Name** text box, type a unique name for the storage account.</span></span> <span data-ttu-id="30b6b-114">Exempel: ”articlesubmission” plus *dina initialer* + *dagens datum*</span><span class="sxs-lookup"><span data-stu-id="30b6b-114">For example, "articlesubmission" + *your initials* + *current date*</span></span>

1. <span data-ttu-id="30b6b-115">Under **Distributionsmodell** väljer du **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-115">Under **Deployment model**, select **Resource manager**.</span></span>

1. <span data-ttu-id="30b6b-116">I listrutan **Typ av konto** väljer du **Lagring (general-purpose v2)**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-116">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>

1. <span data-ttu-id="30b6b-117">Välj en region nära dig i listrutan **Plats**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-117">In the **Location** drop-down list, select a region near you.</span></span>

1. <span data-ttu-id="30b6b-118">I listrutan **Replikering** väljer du **Lokalt redundant lagring (LRS)**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-118">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>

1. <span data-ttu-id="30b6b-119">Under **Prestanda** väljer du **Standard**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-119">Under **Performance**, select **Standard**.</span></span>

1. <span data-ttu-id="30b6b-120">Under **Åtkomstnivå** väljer du **Lågfrekvent**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-120">Under **Access tier**, select **Cool**.</span></span>

1. <span data-ttu-id="30b6b-121">Under **Säker överföring krävs** väljer du **Inaktiverat**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-121">Under **Secure transfer required**, select **Disabled**.</span></span>

1. <span data-ttu-id="30b6b-122">Välj din prenumeration under **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-122">Under **Subscription**, select your subscription.</span></span>

    ![Skapa ett lagringskonto](../media-draft/3-create-storage-account-2.png)

1. <span data-ttu-id="30b6b-124">Under **Resursgrupp** väljer du **Skapa ny** och skriver **ArticleSubmissionResourceGroup** i textrutan.</span><span class="sxs-lookup"><span data-stu-id="30b6b-124">Under **Resource group**, select **Create new**, and then in the textbox type **ArticleSubmissionResourceGroup**.</span></span>

1. <span data-ttu-id="30b6b-125">Under **Virtuella nätverk** väljer du **Inaktiverat**.</span><span class="sxs-lookup"><span data-stu-id="30b6b-125">Under **Virtual networks**, select **Disabled**.</span></span>

1. <span data-ttu-id="30b6b-126">Klicka på **Skapa** för att skapa det nya lagringskontot och den nya resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="30b6b-126">Click **Create** to create the new storage account and the new resource group.</span></span>

    ![Skapa ett lagringskonto](../media-draft/3-create-storage-account-3.png)

## <a name="summary"></a><span data-ttu-id="30b6b-128">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="30b6b-128">Summary</span></span>

<span data-ttu-id="30b6b-129">Här har du skapat ett Azure-lagringskonto med inställningar som är anpassade för användning med en kö.</span><span class="sxs-lookup"><span data-stu-id="30b6b-129">Here, you created an Azure storage account with settings customized for use with a queue.</span></span>