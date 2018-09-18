<span data-ttu-id="901b6-101">Nu skapar vi en Azure Redis Cache för att lagra och returnera värden som används ofta.</span><span class="sxs-lookup"><span data-stu-id="901b6-101">Let's create an Azure Redis Cache instance to store and return commonly used values.</span></span>

<!-- TODO: do we need to activate the sandbox here? -->

## <a name="create-a-redis-cache-in-azure"></a><span data-ttu-id="901b6-102">Skapa en Redis-cache på Azure</span><span class="sxs-lookup"><span data-stu-id="901b6-102">Create a Redis cache in Azure</span></span>

1. <span data-ttu-id="901b6-103">Logga in på [Azure-portalen](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="901b6-103">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="901b6-104">Klicka på **Skapa en resurs**, klicka på **Databaser** och klicka på **Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="901b6-104">Click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="901b6-105">Följande skärmbild visar Redis Cache-platsen i de olika alternativen för databasresurser på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="901b6-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![Skärmbild som visar Azure-portalens databasalternativ, med alternativen Skapa en resurs, Databas och Redis Cache markerade.](../media/4-create-a-cache-1.png)

### <a name="identify-the-location-for-the-cache"></a><span data-ttu-id="901b6-107">Identifiera platsen för cachen</span><span class="sxs-lookup"><span data-stu-id="901b6-107">Identify the location for the cache</span></span>

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="configure-your-cache"></a><span data-ttu-id="901b6-108">Konfigurera din cache</span><span class="sxs-lookup"><span data-stu-id="901b6-108">Configure your cache</span></span>

<span data-ttu-id="901b6-109">Använd följande inställningar för cachen.</span><span class="sxs-lookup"><span data-stu-id="901b6-109">Apply the following settings on the cache.</span></span>

1. <span data-ttu-id="901b6-110">**DNS-namn:** Skapa ett globalt unikt namn, till exempel **ContosoSportsApp1028**.</span><span class="sxs-lookup"><span data-stu-id="901b6-110">**DNS Name:** Create a globally unique name such as **ContosoSportsApp1028**.</span></span>

1. <span data-ttu-id="901b6-111">**Prenumeration**: välj Azure Sandbox-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="901b6-111">**Subscription:** Select the Azure Sandbox subscription.</span></span>

1. <span data-ttu-id="901b6-112">**Resursgrupp**: välj <rgn>[Namn på Sandbox-resursgrupp]</rgn> för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="901b6-112">**Resource group:** Select <rgn>[Sandbox resource group name]</rgn> for the Resource Group.</span></span>

1. <span data-ttu-id="901b6-113">**Plats:** normalt väljer du en plats nära dina kunder – i det här fallet östkusten.</span><span class="sxs-lookup"><span data-stu-id="901b6-113">**Location:** Normally, you would select a location near your customers - in this case, the East Coast.</span></span> <span data-ttu-id="901b6-114">Azure Sandbox tillåter dock bara att specifika regioner väljs för resurser enligt det som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="901b6-114">However, the Azure Sandbox only allows specific regions to be selected for resources as noted above.</span></span> <span data-ttu-id="901b6-115">Välj någon av dessa platser.</span><span class="sxs-lookup"><span data-stu-id="901b6-115">Please select one of those locations.</span></span>

1. <span data-ttu-id="901b6-116">**Prisnivå:** Välj **Basic C0**.</span><span class="sxs-lookup"><span data-stu-id="901b6-116">**Pricing tier:** Select **Basic C0**.</span></span> <span data-ttu-id="901b6-117">Det här är den lägsta nivån som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="901b6-117">This is the lowest tier you can use.</span></span> <span data-ttu-id="901b6-118">Produktionsappar vill förmodligen lagra mer data och använda vissa Premium-funktioner, till exempel klustring, vilket kräver val av en högre nivå.</span><span class="sxs-lookup"><span data-stu-id="901b6-118">Production apps would likely want to store more data and utilize some of the Premium features such as clustering which would require a higher tier selection.</span></span>

1. <span data-ttu-id="901b6-119">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="901b6-119">Click **Create**.</span></span>

    <span data-ttu-id="901b6-120">Följande skärmbild visar en typisk konfiguration som används för att skapa en ny Redis Cache-resurs.</span><span class="sxs-lookup"><span data-stu-id="901b6-120">The following screenshot shows a representative configuration used to create a new Redis Cache resource.</span></span> <span data-ttu-id="901b6-121">Observera att din kommer att se något annorlunda ut på grund av Azure Sandbox.</span><span class="sxs-lookup"><span data-stu-id="901b6-121">Note that yours will be slightly different due to the Azure Sandbox.</span></span>

    ![Skärmbild som visar bladet för Azure-portalen när du skapar en ny Redis Cache-resurs, med exempel på DNS-namn, prenumeration, ny resursgrupp, plats och prisnivå.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> <span data-ttu-id="901b6-123">Du måste vänta tills cachen distribueras innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="901b6-123">You will have to wait until the cache is deployed before continuing.</span></span> <span data-ttu-id="901b6-124">Den här processen kan ta lite tid.</span><span class="sxs-lookup"><span data-stu-id="901b6-124">This process might take some time.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="901b6-125">Hämta åtkomstnycklarna och värdnamnet</span><span class="sxs-lookup"><span data-stu-id="901b6-125">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="901b6-126">Navigera till din nya cacheinstans i Azure-portalen och välj **Inställningar** > **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="901b6-126">Navigate to your new cache instance in the Azure portal and select **Settings** > **Access keys**.</span></span> 

1. <span data-ttu-id="901b6-127">Kopiera den **Primära anslutningssträngen (StackExchange.Redis)** till en säker plats. Du behöver den i nästa övning.</span><span class="sxs-lookup"><span data-stu-id="901b6-127">Copy the **Primary connection string (StackExchange.Redis)** to a safe place, you will need it for the next exercise.</span></span>

    <span data-ttu-id="901b6-128">Den här nyckeln innehåller din primära nyckel och värdnamnet i en fullständig anslutningssträng, för användning i programinställningarna för **StackExchange.Redis**-paketet som vi kommer att använda.</span><span class="sxs-lookup"><span data-stu-id="901b6-128">This key includes your primary key and host name in a complete connection string for use within your application settings for the **StackExchange.Redis** package we are going to use.</span></span>

<span data-ttu-id="901b6-129">Nu går vi igenom några av de kommandon som vi kan använda för att fråga ut cachen.</span><span class="sxs-lookup"><span data-stu-id="901b6-129">Next, let's learn about some of the commands we can use to interrogate the cache.</span></span>