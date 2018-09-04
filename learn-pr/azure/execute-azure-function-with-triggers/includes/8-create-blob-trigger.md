<span data-ttu-id="e3037-101">I den här övningen ska vi skapa en Azure-funktion som visar namnet och storleken för en blob när den har skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="e3037-101">In this exercise, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span> 

> [!NOTE]
> <span data-ttu-id="e3037-102">För att kunna slutföra den här övningen måste du vara inloggad på [Azure Portal](https://portal.azure.com?azure-portal=true) med ett giltigt konto.</span><span class="sxs-lookup"><span data-stu-id="e3037-102">To complete this exercise, make sure you're signed into the [Azure portal](https://portal.azure.com?azure-portal=true) with a valid account.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="e3037-103">Skapa en blobutlösare</span><span class="sxs-lookup"><span data-stu-id="e3037-103">Create a blob trigger</span></span>

<span data-ttu-id="e3037-104">Vi ska fortsätta att använda vårt befintliga Azure Functions-program och lägger till en blobutlösare.</span><span class="sxs-lookup"><span data-stu-id="e3037-104">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="e3037-105">Peka på **Funktioner** och välj plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="e3037-105">Point to **Functions** and select the plus (+) icon.</span></span>

    ![Peka på Funktioner och välj plustecknet](../media-drafts/4-hover-function.png)

2. <span data-ttu-id="e3037-107">Välj **Anpassad funktion** och sedan **Blob-utlösare**.</span><span class="sxs-lookup"><span data-stu-id="e3037-107">Select **Custom Function** and then **Blob trigger**.</span></span>

3. <span data-ttu-id="e3037-108">Välj **C#** som språk.</span><span class="sxs-lookup"><span data-stu-id="e3037-108">Select **C#** as the language.</span></span> 

4. <span data-ttu-id="e3037-109">Lämna standardvärdet för **Namn**.</span><span class="sxs-lookup"><span data-stu-id="e3037-109">Leave the **Name** set to the default value.</span></span>

5. <span data-ttu-id="e3037-110">Lämna standardvärdet för **Sökväg**.</span><span class="sxs-lookup"><span data-stu-id="e3037-110">Leave the **Path** set to the default value.</span></span>

6. <span data-ttu-id="e3037-111">Välj ett befintligt Azure Storage-konto eller välj **Skapa** om du vill att Azure ska skapa ett nytt konto åt dig.</span><span class="sxs-lookup"><span data-stu-id="e3037-111">Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="e3037-112">Skapa en blobcontainer</span><span class="sxs-lookup"><span data-stu-id="e3037-112">Create a blob container</span></span>

<span data-ttu-id="e3037-113">Nu när vi har skapat en blobutlösare ska vi använda Storage Explorer för att skapa en blob och utlösa funktionen.</span><span class="sxs-lookup"><span data-stu-id="e3037-113">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="e3037-114">Öppna det lagringskonto som du valde att använda (eller skapade) på en ny flik. Ett enkelt sätt att göra detta är att öppna en ny flik på Azure Portal och klicka på **Lagringskonton** i sidopanelen eller att använda **Alla tjänster** i sidopanelen och sedan filtrera efter namn.</span><span class="sxs-lookup"><span data-stu-id="e3037-114">Open the storage account you used (or created) in a new tab. An easy way to do this is to open a new Azure Portal tab and click on **Storage accounts** in the sidebar, or to use **All services** in the sidebar and then filter by the name.</span></span> <span data-ttu-id="e3037-115">Vi vill använda en ny flik så att vi kan växla mellan de två tjänsterna som vi arbetar med.</span><span class="sxs-lookup"><span data-stu-id="e3037-115">We want to use a new tab so we can switch between the two services we are working with.</span></span>

2. <span data-ttu-id="e3037-116">Klicka på **Storage Explorer (förhandsversion)**. Nu öppnas ett nytt blad där du kan arbeta med blobar och filer.</span><span class="sxs-lookup"><span data-stu-id="e3037-116">Click on the **Storage Explorer (preview)** section - this will open a new blade where you can work with blobs and files.</span></span>

<span data-ttu-id="e3037-117">Kom ihåg att vår blobutlösare endast övervakar den plats som beskrivs i fältet **Sökväg**.</span><span class="sxs-lookup"><span data-stu-id="e3037-117">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="e3037-118">Som standard bör sökvägen vara:</span><span class="sxs-lookup"><span data-stu-id="e3037-118">By default, our path should be:</span></span>

> <span data-ttu-id="e3037-119">samples-workitems/{namn}</span><span class="sxs-lookup"><span data-stu-id="e3037-119">samples-workitems/{name}</span></span>

<span data-ttu-id="e3037-120">Vi behöver skapa en container med namnet **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="e3037-120">We need to create a container called **samples-workitems**.</span></span>

3. <span data-ttu-id="e3037-121">Högerklicka på **BLOBCONTAINRAR** och välj **Skapa blobcontainer**.</span><span class="sxs-lookup"><span data-stu-id="e3037-121">Right-click **BLOB CONTAINERS** and select **Create blob container**.</span></span>

4. <span data-ttu-id="e3037-122">Ange **samples-workitems** som namn och lämna standardinställningen **Privat** för åtkomstnivån.</span><span class="sxs-lookup"><span data-stu-id="e3037-122">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="e3037-123">Aktivera blobutlösaren</span><span class="sxs-lookup"><span data-stu-id="e3037-123">Turn on your blob trigger</span></span>

<span data-ttu-id="e3037-124">Nu när vi har skapat containern som ska övervakas kör vi vår funktion så att vi ser utdata när en blob skapas.</span><span class="sxs-lookup"><span data-stu-id="e3037-124">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="e3037-125">Växla tillbaka till fliken Azure-funktion (eller öppna den igen).</span><span class="sxs-lookup"><span data-stu-id="e3037-125">Switch back to the Azure Function tab (or reopen it).</span></span>

2. <span data-ttu-id="e3037-126">Välj blobutlösaren för att öppna kodskärmen.</span><span class="sxs-lookup"><span data-stu-id="e3037-126">Select your blob trigger to open the code screen.</span></span>

3. <span data-ttu-id="e3037-127">Välj **Kör**. Utdatafönstret öppnas.</span><span class="sxs-lookup"><span data-stu-id="e3037-127">Select **Run** - this will open the output window.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="e3037-128">Skapa en blob</span><span class="sxs-lookup"><span data-stu-id="e3037-128">Create a blob</span></span>

<span data-ttu-id="e3037-129">Nu är blobutlösaren aktiverad och lyssnar efter aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e3037-129">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="e3037-130">Nu ska vi skapa en blob och se om vi får ett loggmeddelande.</span><span class="sxs-lookup"><span data-stu-id="e3037-130">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="e3037-131">Välj containern **samples-workitems** i Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="e3037-131">In Storage explorer, select the **samples-workitems** container.</span></span>

2. <span data-ttu-id="e3037-132">Välj **Överför** från verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="e3037-132">Select **Upload** from the toolbar.</span></span>

3. <span data-ttu-id="e3037-133">Välj en fil från datorn.</span><span class="sxs-lookup"><span data-stu-id="e3037-133">Select any file from your computer.</span></span>

4. <span data-ttu-id="e3037-134">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="e3037-134">Select **Upload**.</span></span>

5. <span data-ttu-id="e3037-135">Gå tillbaka till fliken Azure-funktion och kontrollera utdataloggarna för ett meddelande som visar vilken fil som laddades upp.</span><span class="sxs-lookup"><span data-stu-id="e3037-135">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>

## <a name="pause-the-function"></a><span data-ttu-id="e3037-136">Pausa funktionen</span><span class="sxs-lookup"><span data-stu-id="e3037-136">Pause the Function</span></span>

<span data-ttu-id="e3037-137">Klicka på **Pausa** ovanför loggfönstret så att du inte debiteras för ytterligare begäranden.</span><span class="sxs-lookup"><span data-stu-id="e3037-137">To ensure that you aren't charged for additional requests, you can click **Pause** above the log window.</span></span>

![Pausa funktionen](../media-drafts/4-pause-timer.png)


