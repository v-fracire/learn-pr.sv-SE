<span data-ttu-id="1ecae-101">Nu skapar vi en Azure Redis Cache för att lagra och returnera värden som används ofta.</span><span class="sxs-lookup"><span data-stu-id="1ecae-101">Let's create an Azure Redis Cache instance to store and return commonly used values.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-redis-cache-in-azure"></a><span data-ttu-id="1ecae-102">Skapa en Redis-cache på Azure</span><span class="sxs-lookup"><span data-stu-id="1ecae-102">Create a Redis cache in Azure</span></span>

1. <span data-ttu-id="1ecae-103">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="1ecae-103">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="1ecae-104">Klicka på **Skapa en resurs**, klicka på **Databaser** och klicka på **Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="1ecae-104">Click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="1ecae-105">Följande skärmbild visar Redis Cache-platsen i de olika alternativen för databasresurser på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1ecae-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![Skärmbild som visar Azure-portalens databasalternativ, med Skapa en resurs, Databas och Redis Cache-alternativet markerade.](../media/4-create-a-cache-1.png)

### <a name="configure-your-cache"></a><span data-ttu-id="1ecae-107">Konfigurera din cache</span><span class="sxs-lookup"><span data-stu-id="1ecae-107">Configure your cache</span></span>

<span data-ttu-id="1ecae-108">Använd följande inställningar för cachen.</span><span class="sxs-lookup"><span data-stu-id="1ecae-108">Apply the following settings on the cache.</span></span>

1. <span data-ttu-id="1ecae-109">**DNS-namn:** skapar ett globalt unikt namn som **ContosoSportsApp [nnn]**, där `[nnn]` ersätts med slumpmässiga siffror.</span><span class="sxs-lookup"><span data-stu-id="1ecae-109">**DNS Name:** Create a globally unique name such as **ContosoSportsApp[nnn]**, where `[nnn]` is replaced with random numbers.</span></span>

1. <span data-ttu-id="1ecae-110">**Prenumeration:** välj Concierge-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="1ecae-110">**Subscription:** Select the Concierge subscription.</span></span>

1. <span data-ttu-id="1ecae-111">**Resursgrupp**: välj <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn> för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1ecae-111">**Resource group:** Select <rgn>[sandbox resource group name]</rgn> for the Resource Group.</span></span>

1. <span data-ttu-id="1ecae-112">**Plats:** normalt väljer du en plats nära dina kunder – i det här fallet östkusten.</span><span class="sxs-lookup"><span data-stu-id="1ecae-112">**Location:** Normally, you would select a location near your customers - in this case, the East Coast.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

5. <span data-ttu-id="1ecae-113">**Prisnivå:** välj **Basic C0**.</span><span class="sxs-lookup"><span data-stu-id="1ecae-113">**Pricing tier:** Select **Basic C0**.</span></span> <span data-ttu-id="1ecae-114">Det här är den lägsta nivån som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="1ecae-114">This is the lowest tier you can use.</span></span> <span data-ttu-id="1ecae-115">Produktionsappar vill förmodligen lagra mer data och använda vissa Premium-funktioner, till exempel klustring, vilket kräver val av en högre nivå.</span><span class="sxs-lookup"><span data-stu-id="1ecae-115">Production apps would likely want to store more data and utilize some of the Premium features such as clustering which would require a higher tier selection.</span></span>

1. <span data-ttu-id="1ecae-116">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1ecae-116">Click **Create**.</span></span> <span data-ttu-id="1ecae-117">Azure skapar och distribuerar Redis Cache-instansen åt dig.</span><span class="sxs-lookup"><span data-stu-id="1ecae-117">Azure will create and deploy the Redis Cache instance for you.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="1ecae-118">Redis Cache-resursen skapas och visas vanligtvis mycket snabbt i Azure Portal, men själva cacheminnet är inte tillgänglig under några minuter.</span><span class="sxs-lookup"><span data-stu-id="1ecae-118">Usually, the Redis cache resource will be created and viewable in the Azure portal very quickly, but the cache itself will not be available for a few minutes.</span></span> <span data-ttu-id="1ecae-119">Nästa steg visar hur du kontrollerar ditt cacheminnes status.</span><span class="sxs-lookup"><span data-stu-id="1ecae-119">The next steps show how to check the status of your cache.</span></span>

## <a name="verify-your-data"></a><span data-ttu-id="1ecae-120">Verifiera dina data</span><span class="sxs-lookup"><span data-stu-id="1ecae-120">Verify your data</span></span>

<span data-ttu-id="1ecae-121">Du kan använda **konsol**-funktion i Azure-portalen för att utfärda kommandon till din Redis cache-instans när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="1ecae-121">You can use the **Console** feature in the Azure portal to issue commands to your Redis cache instance after it has been created.</span></span>

1. <span data-ttu-id="1ecae-122">Hitta din Redis cache via popupmenyn **Meddelanden** när den är klar med distributionen, eller genom att välja **Alla resurser** i det vänstra sidofältet och använda filterrutan till vänster för att välja Redis Cache-instanser.</span><span class="sxs-lookup"><span data-stu-id="1ecae-122">Locate your Redis cache through the **Notification** popup when it finishes deployment, or by selecting **All Resources** in the left-hand sidebar and using the filter box on the left to select Redis Cache instances.</span></span> <span data-ttu-id="1ecae-123">Du kan alternativt använda sökrutan högst upp och skriva namnet på cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="1ecae-123">Alternatively, you can use the search box at the top and type the name of the cache.</span></span>

1. <span data-ttu-id="1ecae-124">Välj din Redis Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="1ecae-124">Select your Redis cache instance.</span></span>

1. <span data-ttu-id="1ecae-125">Kontrollera statusfältets värde.</span><span class="sxs-lookup"><span data-stu-id="1ecae-125">Check the value of the "Status" field.</span></span> <span data-ttu-id="1ecae-126">Cacheminnet är inte klart förrän statusen är ”Körs”.</span><span class="sxs-lookup"><span data-stu-id="1ecae-126">The cache is not ready until the status is "Running".</span></span> <span data-ttu-id="1ecae-127">Du får kanske vänta några minuter innan du kan fortsätta.</span><span class="sxs-lookup"><span data-stu-id="1ecae-127">You might have to wait for a few minutes before proceeding.</span></span>

1. <span data-ttu-id="1ecae-128">När cacheminnet väl körs klickar du på `>_ Console`-knappen i verktygsfältet på bladet **Översikt**-bladet för din Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="1ecae-128">Once the cache is running, Click the `>_ Console` button in the toolbar on the **Overview** blade for your Redis Cache.</span></span> <span data-ttu-id="1ecae-129">Redis-konsolen öppnas, där du kan ange Redis-kommandon på låg nivå.</span><span class="sxs-lookup"><span data-stu-id="1ecae-129">This will open a Redis console, which allows you to enter low-level Redis commands.</span></span> <span data-ttu-id="1ecae-130">Prova något av följande:</span><span class="sxs-lookup"><span data-stu-id="1ecae-130">Try some of the following:</span></span>

    ```console
    ping
    ```

    ```output
    PONG
    ```

    ```console
    set test one
    ```

    ```output
    OK
    ```

    ```console
    get test
    ```

    ```output
    "one"
    ```

<span data-ttu-id="1ecae-131">Gå tillbaka till den **Översikt**-panelen genom navigeringsfältet längst upp eller använda rullningslisten för att dra tillbaka vyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="1ecae-131">Switch back to the **Overview** panel either through the breadcrumb bar on the top, or use the scrollbar to slide the view back to the left.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="1ecae-132">Hämta åtkomstnycklarna och värdnamnet</span><span class="sxs-lookup"><span data-stu-id="1ecae-132">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="1ecae-133">Välj **Inställningar** > **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="1ecae-133">Select **Settings** > **Access keys**.</span></span>

1. <span data-ttu-id="1ecae-134">Kopiera den **Primära anslutningssträngen (StackExchange.Redis)** till en säker plats. Du behöver den i nästa övning.</span><span class="sxs-lookup"><span data-stu-id="1ecae-134">Copy the **Primary connection string (StackExchange.Redis)** to a safe place, you will need it for the next exercise.</span></span>

    <span data-ttu-id="1ecae-135">Den här nyckeln innehåller din primära nyckel och värdnamnet i en fullständig anslutningssträng, för användning i programinställningarna för **StackExchange.Redis**-paketet som vi kommer att använda.</span><span class="sxs-lookup"><span data-stu-id="1ecae-135">This key includes your primary key and host name in a complete connection string for use within your application settings for the **StackExchange.Redis** package we are going to use.</span></span>

<span data-ttu-id="1ecae-136">Nu går vi igenom några av de kommandon som vi kan använda för att fråga ut cachen.</span><span class="sxs-lookup"><span data-stu-id="1ecae-136">Next, let's learn about some of the commands we can use to interrogate the cache.</span></span>