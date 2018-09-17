<span data-ttu-id="e0876-101">I den här övningen ska vi skapa en Azure-funktion som visar namnet och storleken för en blob när den har skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="e0876-101">In this unit, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="e0876-102">Skapa en blobutlösare</span><span class="sxs-lookup"><span data-stu-id="e0876-102">Create a blob trigger</span></span>

<span data-ttu-id="e0876-103">Vi ska fortsätta att använda vårt befintliga Azure Functions-program och lägger till en blobutlösare.</span><span class="sxs-lookup"><span data-stu-id="e0876-103">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="e0876-104">Logga in på [Azure-portalen](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="e0876-104">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="e0876-105">Peka på **Funktioner** och välj plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="e0876-105">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="e0876-106">Välj **Anpassad funktion** och sedan **Blob-utlösare**.</span><span class="sxs-lookup"><span data-stu-id="e0876-106">Select **Custom Function** and then **Blob trigger**.</span></span>

1. <span data-ttu-id="e0876-107">Välj **C#** som språk.</span><span class="sxs-lookup"><span data-stu-id="e0876-107">Select **C#** as the language.</span></span>

1. <span data-ttu-id="e0876-108">Lämna standardvärdet för **Namn**.</span><span class="sxs-lookup"><span data-stu-id="e0876-108">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="e0876-109">Lämna standardvärdet för **Sökväg**.</span><span class="sxs-lookup"><span data-stu-id="e0876-109">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="e0876-110">Välj ett befintligt Azure Storage-konto eller välj **Skapa** om du vill att Azure ska skapa ett nytt konto åt dig.</span><span class="sxs-lookup"><span data-stu-id="e0876-110">Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="e0876-111">Skapa en blobcontainer</span><span class="sxs-lookup"><span data-stu-id="e0876-111">Create a blob container</span></span>

<span data-ttu-id="e0876-112">Nu när vi har skapat en blobutlösare ska vi använda Storage Explorer för att skapa en blob och utlösa funktionen.</span><span class="sxs-lookup"><span data-stu-id="e0876-112">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="e0876-113">Öppna det lagringskonto som du valde att använda (eller skapade) på en ny flik. Ett enkelt sätt att göra detta är att öppna en ny flik på Azure Portal och klicka på **Lagringskonton** i sidopanelen eller att använda **Alla tjänster** i sidopanelen och sedan filtrera efter namn.</span><span class="sxs-lookup"><span data-stu-id="e0876-113">Open the storage account you used (or created) in a new tab. An easy way to do this is to open a new Azure Portal tab and click on **Storage accounts** in the sidebar, or to use **All services** in the sidebar and then filter by the name.</span></span> <span data-ttu-id="e0876-114">Vi vill använda en ny flik så att vi kan växla mellan de två tjänsterna som vi arbetar med.</span><span class="sxs-lookup"><span data-stu-id="e0876-114">We want to use a new tab so we can switch between the two services we are working with.</span></span>

1. <span data-ttu-id="e0876-115">Klicka på **Storage Explorer (förhandsversion)**. Nu öppnas ett nytt blad där du kan arbeta med blobar och filer.</span><span class="sxs-lookup"><span data-stu-id="e0876-115">Click on the **Storage Explorer (preview)** section - this will open a new blade where you can work with blobs and files.</span></span>

<span data-ttu-id="e0876-116">Kom ihåg att vår blobutlösare endast övervakar den plats som beskrivs i fältet **Sökväg**.</span><span class="sxs-lookup"><span data-stu-id="e0876-116">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="e0876-117">Som standard bör sökvägen vara:</span><span class="sxs-lookup"><span data-stu-id="e0876-117">By default, our path should be:</span></span>

> <span data-ttu-id="e0876-118">samples-workitems/{namn}</span><span class="sxs-lookup"><span data-stu-id="e0876-118">samples-workitems/{name}</span></span>

<span data-ttu-id="e0876-119">Vi behöver skapa en container med namnet **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="e0876-119">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="e0876-120">Högerklicka på **BLOBCONTAINRAR** och välj **Skapa blobcontainer**.</span><span class="sxs-lookup"><span data-stu-id="e0876-120">Right-click **BLOB CONTAINERS** and select **Create blob container**.</span></span>

1. <span data-ttu-id="e0876-121">Ange **samples-workitems** som namn och lämna standardinställningen **Privat** för åtkomstnivån.</span><span class="sxs-lookup"><span data-stu-id="e0876-121">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="e0876-122">Aktivera blobutlösaren</span><span class="sxs-lookup"><span data-stu-id="e0876-122">Turn on your blob trigger</span></span>

<span data-ttu-id="e0876-123">Nu när vi har skapat containern som ska övervakas kör vi vår funktion så att vi ser utdata när en blob skapas.</span><span class="sxs-lookup"><span data-stu-id="e0876-123">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="e0876-124">Växla tillbaka till fliken Azure-funktion (eller öppna den igen).</span><span class="sxs-lookup"><span data-stu-id="e0876-124">Switch back to the Azure Function tab (or reopen it).</span></span>

1. <span data-ttu-id="e0876-125">Välj blobutlösaren för att öppna kodskärmen.</span><span class="sxs-lookup"><span data-stu-id="e0876-125">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="e0876-126">Välj **Kör**. Utdatafönstret öppnas.</span><span class="sxs-lookup"><span data-stu-id="e0876-126">Select **Run** - this will open the output window.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="e0876-127">Skapa en blob</span><span class="sxs-lookup"><span data-stu-id="e0876-127">Create a blob</span></span>

<span data-ttu-id="e0876-128">Nu är blobutlösaren aktiverad och lyssnar efter aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e0876-128">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="e0876-129">Nu ska vi skapa en blob och se om vi får ett loggmeddelande.</span><span class="sxs-lookup"><span data-stu-id="e0876-129">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="e0876-130">Välj containern **samples-workitems** i Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="e0876-130">In Storage explorer, select the **samples-workitems** container.</span></span>

1. <span data-ttu-id="e0876-131">Välj **Överför** från verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="e0876-131">Select **Upload** from the toolbar.</span></span>

1. <span data-ttu-id="e0876-132">Välj en fil från datorn.</span><span class="sxs-lookup"><span data-stu-id="e0876-132">Select any file from your computer.</span></span>

1. <span data-ttu-id="e0876-133">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="e0876-133">Select **Upload**.</span></span>

1. <span data-ttu-id="e0876-134">Gå tillbaka till fliken Azure-funktion och kontrollera utdataloggarna för ett meddelande som visar vilken fil som laddades upp.</span><span class="sxs-lookup"><span data-stu-id="e0876-134">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>

## <a name="pause-the-function"></a><span data-ttu-id="e0876-135">Pausa funktionen</span><span class="sxs-lookup"><span data-stu-id="e0876-135">Pause the Function</span></span>

<span data-ttu-id="e0876-136">Klicka på **Pausa** ovanför loggfönstret så att du inte debiteras för ytterligare begäranden.</span><span class="sxs-lookup"><span data-stu-id="e0876-136">To ensure that you aren't charged for additional requests, you can click **Pause** above the log window.</span></span>
