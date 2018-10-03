<span data-ttu-id="9a849-101">I den här övningen ska vi skapa en Azure-funktion som visar namnet och storleken för en blob när den har skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="9a849-101">In this unit, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="9a849-102">Skapa en blobutlösare</span><span class="sxs-lookup"><span data-stu-id="9a849-102">Create a blob trigger</span></span>

<span data-ttu-id="9a849-103">Vi ska fortsätta att använda vårt befintliga Azure Functions-program och lägger till en blobutlösare.</span><span class="sxs-lookup"><span data-stu-id="9a849-103">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="9a849-104">Kontrollera att du är inloggad på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="9a849-104">Make sure you are signed into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="9a849-105">Navigera till skärmen **Alla resurser** och välj din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="9a849-105">Navigate to the **All resources** screen and select your function app.</span></span>

1. <span data-ttu-id="9a849-106">Peka på **Funktioner** och välj plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="9a849-106">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="9a849-107">Välj **Blob-utlösare**.</span><span class="sxs-lookup"><span data-stu-id="9a849-107">Select **Blob trigger**.</span></span>

1. <span data-ttu-id="9a849-108">Lämna standardvärdet för **Namn**.</span><span class="sxs-lookup"><span data-stu-id="9a849-108">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="9a849-109">Lämna standardvärdet för **Sökväg**.</span><span class="sxs-lookup"><span data-stu-id="9a849-109">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="9a849-110">Välj den _nya_ länken bredvid listrutan **lagringskontoanslutning**.</span><span class="sxs-lookup"><span data-stu-id="9a849-110">Select the _new_ link next to the **Storage account connection** dropdown.</span></span> <span data-ttu-id="9a849-111">I bladet som öppnas klickar du på **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="9a849-111">In the blade that pops up, click **Create new**.</span></span> <span data-ttu-id="9a849-112">Ange ett unikt namn för det nya lagringskontot och välj **OK** att skapa lagringskontot och stänga fönstret.</span><span class="sxs-lookup"><span data-stu-id="9a849-112">Enter a unique name for the new storage account and select **OK** to create the storage account and close pane.</span></span>

1. <span data-ttu-id="9a849-113">När du har kommit tillbaka till skärmen Ny funktion väljer du **Skapa** för att skapa funktionen.</span><span class="sxs-lookup"><span data-stu-id="9a849-113">Once you have returned to the New Function screen, select **Create** to create the function.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="9a849-114">Skapa en blobcontainer</span><span class="sxs-lookup"><span data-stu-id="9a849-114">Create a blob container</span></span>

<span data-ttu-id="9a849-115">Nu när vi har skapat en blobutlösare ska vi använda Storage Explorer för att skapa en blob och utlösa funktionen.</span><span class="sxs-lookup"><span data-stu-id="9a849-115">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="9a849-116">Öppna det lagringskonto som du valde att använda (eller skapade) på en ny flik.</span><span class="sxs-lookup"><span data-stu-id="9a849-116">Open the storage account you used (or created) in a new tab.</span></span>

    > [!TIP]
    > <span data-ttu-id="9a849-117">Du kan duplicera en flik i de flesta webbläsare genom att högerklicka på fliken i fråga och välja **Duplicera** från menyn som visas.</span><span class="sxs-lookup"><span data-stu-id="9a849-117">You can duplicate a tab in most browsers by right-clicking on the tab in question and selecting **Duplicate** from the menu that appears.</span></span> <span data-ttu-id="9a849-118">Vi vill använda en ny flik så att vi kan växla mellan de två tjänsterna som vi arbetar med.</span><span class="sxs-lookup"><span data-stu-id="9a849-118">We want to use a new tab so we can switch between the two services we are working with.</span></span>

1. <span data-ttu-id="9a849-119">Välj **Lagringskonton** i sidopanelen eller välj **Alla resurser** i sidopanelen och filtrera sedan efter namn.</span><span class="sxs-lookup"><span data-stu-id="9a849-119">Select **Storage accounts** in the sidebar, or select **All resources** in the sidebar and then filter by the name.</span></span>

1. <span data-ttu-id="9a849-120">Klicka på sektionen **Storage Explorer (förhandsversion)** för att öppna ett nytt fönster där du kan arbeta med blobbar och filer.</span><span class="sxs-lookup"><span data-stu-id="9a849-120">Click on the **Storage Explorer (preview)** section - this will open a new panel where you can work with blobs and files.</span></span>

<span data-ttu-id="9a849-121">Kom ihåg att vår blobutlösare endast övervakar den plats som beskrivs i fältet **Sökväg**.</span><span class="sxs-lookup"><span data-stu-id="9a849-121">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="9a849-122">Som standard bör sökvägen vara:</span><span class="sxs-lookup"><span data-stu-id="9a849-122">By default, our path should be:</span></span>

> <span data-ttu-id="9a849-123">samples-workitems/{namn}</span><span class="sxs-lookup"><span data-stu-id="9a849-123">samples-workitems/{name}</span></span>

<span data-ttu-id="9a849-124">Vi behöver skapa en container med namnet **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="9a849-124">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="9a849-125">Högerklicka på **BLOBCONTAINRAR** och välj **Skapa blobcontainer**.</span><span class="sxs-lookup"><span data-stu-id="9a849-125">Right-click **BLOB CONTAINERS** and select **Create Blob Container**.</span></span>

1. <span data-ttu-id="9a849-126">Ange **samples-workitems** som namn och lämna standardinställningen **Privat** för åtkomstnivån och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a849-126">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting, and select **OK**.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="9a849-127">Aktivera blobutlösaren</span><span class="sxs-lookup"><span data-stu-id="9a849-127">Turn on your blob trigger</span></span>

<span data-ttu-id="9a849-128">Nu när vi har skapat containern som ska övervakas kör vi vår funktion så att vi ser utdata när en blob skapas.</span><span class="sxs-lookup"><span data-stu-id="9a849-128">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="9a849-129">Gå tillbaka till webbläsarfliken med din Azure-funktion (eller öppna den igen).</span><span class="sxs-lookup"><span data-stu-id="9a849-129">Switch back to the browser tab with your Azure Function (or reopen it).</span></span>

1. <span data-ttu-id="9a849-130">Välj blobutlösaren för att öppna kodskärmen.</span><span class="sxs-lookup"><span data-stu-id="9a849-130">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="9a849-131">Öppna **Loggpanelen** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="9a849-131">Open the **Logs** tab at the bottom of the screen.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="9a849-132">Skapa en blob</span><span class="sxs-lookup"><span data-stu-id="9a849-132">Create a blob</span></span>

<span data-ttu-id="9a849-133">Nu är blobutlösaren aktiverad och lyssnar efter aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9a849-133">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="9a849-134">Nu ska vi skapa en blob och se om vi får ett loggmeddelande.</span><span class="sxs-lookup"><span data-stu-id="9a849-134">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="9a849-135">Gå tillbaka till webbläsarfliken med Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="9a849-135">Switch back to the browser tab with Storage Explorer.</span></span>

1. <span data-ttu-id="9a849-136">I Storage Explorer väljer du containern **samples-workitems** från listan **BLOBBCONTAINER**.</span><span class="sxs-lookup"><span data-stu-id="9a849-136">In Storage Explorer, select the **samples-workitems** container from the **BLOB CONTAINERS** list.</span></span>

1. <span data-ttu-id="9a849-137">Välj **Överför** från verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="9a849-137">Select **Upload** from the toolbar.</span></span>

1. <span data-ttu-id="9a849-138">Välj en fil från datorn.</span><span class="sxs-lookup"><span data-stu-id="9a849-138">Select any file from your computer.</span></span>

1. <span data-ttu-id="9a849-139">Under **Autentiseringstyp**, välj **SSH**.</span><span class="sxs-lookup"><span data-stu-id="9a849-139">Under **Authentication type**, select **SAS**.</span></span>

1. <span data-ttu-id="9a849-140">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="9a849-140">Select **Upload**.</span></span>

1. <span data-ttu-id="9a849-141">Gå tillbaka till fliken Azure-funktion och kontrollera utdataloggarna för ett meddelande som visar vilken fil som laddades upp.</span><span class="sxs-lookup"><span data-stu-id="9a849-141">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>