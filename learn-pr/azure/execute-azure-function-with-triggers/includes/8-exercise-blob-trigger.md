<span data-ttu-id="3e7f5-101">I den här övningen ska vi skapa en Azure-funktion som visar namn och storlek för en blob när den har skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-101">In this exercise, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span> 

> [!NOTE]
> <span data-ttu-id="3e7f5-102">För att kunna slutföra den här övningen måste du vara inloggad på [Azure-portalen](https://portal.azure.com/) med ett giltigt konto.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-102">To complete this exercise, make sure you're signed in to the [Azure portal](https://portal.azure.com/) with a valid account.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="3e7f5-103">Skapa en blob-utlösare</span><span class="sxs-lookup"><span data-stu-id="3e7f5-103">Create a blob trigger</span></span>

<span data-ttu-id="3e7f5-104">Vi ska fortsätta att använda vårt befintliga Azure Functions-program och lägger till en blobutlösare.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-104">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="3e7f5-105">Peka på **Funktioner** och välj plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="3e7f5-105">Point to **Functions** and select the plus (+) icon.</span></span>

    ![Peka på Functions och välj plustecknet](../media-drafts/4-hover-function.png)

1. <span data-ttu-id="3e7f5-107">Välj **Blob-utlösare**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-107">Select **Blob trigger**.</span></span>

1. <span data-ttu-id="3e7f5-108">Välj **#C** som språk.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-108">Select **C#** as the language.</span></span> 

1. <span data-ttu-id="3e7f5-109">Lämna standardvärdet för **Namn**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-109">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="3e7f5-110">Lämna standardvärdet för **Sökväg**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-110">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="3e7f5-111">Välj ett befintligt Azure Storage-konto eller välj **Skapa** om du vill att Azure ska skapa ett nytt konto åt dig.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-111">Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.</span></span>

## <a name="download-storage-explorer"></a><span data-ttu-id="3e7f5-112">Hämta Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="3e7f5-112">Download Storage explorer</span></span>

<span data-ttu-id="3e7f5-113">Nu när vi har skapat en blob-utlösare kan vi hämta Lagringsutforskaren, som gör att vi enkelt kan skapa en blob.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-113">Now that we've created a blob trigger, let's download Storage explorer, which will allow us to easily create a blob.</span></span>

- <span data-ttu-id="3e7f5-114">Hämta [Lagringsutforskaren](http://storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="3e7f5-114">Download [Storage explorer](http://storageexplorer.com).</span></span>

## <a name="connect-to-your-azure-storage-account"></a><span data-ttu-id="3e7f5-115">Anslut till ditt Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="3e7f5-115">Connect to your Azure Storage account</span></span>

<span data-ttu-id="3e7f5-116">Nu har vi hämtat Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-116">We now have Storage explorer downloaded.</span></span> <span data-ttu-id="3e7f5-117">Låt oss logga in med de autentiseringsuppgifter som vi har fått.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-117">Let's sign in using the credentials that were supplied.</span></span>

1. <span data-ttu-id="3e7f5-118">Välj plustecknet (+) till vänster i Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-118">In Storage explorer, select the plus (+) icon on the left.</span></span>

1. <span data-ttu-id="3e7f5-119">Välj **Använd ett kontonamn och en nyckel för lagringen**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-119">Select **Use a storage account name and key**.</span></span>

1. <span data-ttu-id="3e7f5-120">Välj **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-120">Select **Next**.</span></span>

1. <span data-ttu-id="3e7f5-121">Under blob-utlösaren i Azure väljer du **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-121">In Azure, under your blob trigger, select **Integrate**.</span></span>

1. <span data-ttu-id="3e7f5-122">Välj **Dokumentation** för att expandera vyn.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-122">Select **Documentation** to expand the view.</span></span>

1. <span data-ttu-id="3e7f5-123">Kopiera **Kontonamn** och **Kontonyckel**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-123">Copy the **Account Name** and **Account Key**.</span></span>

1. <span data-ttu-id="3e7f5-124">Gå tillbaka till Lagringsutforskaren och klistra in **Kontonamn** och **Kontonyckel**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-124">Back in Storage explorer, paste in the **Account Name** and **Account Key**.</span></span>

1. <span data-ttu-id="3e7f5-125">Ange ett **Visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-125">Enter a **Display name**.</span></span> <span data-ttu-id="3e7f5-126">Det här värdet är namnet på anslutningen i Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-126">This value is the name of the connection in Storage explorer.</span></span>

1. <span data-ttu-id="3e7f5-127">Välj **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-127">Select **Next**.</span></span>

1. <span data-ttu-id="3e7f5-128">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-128">Select **Connect**.</span></span> 

## <a name="create-a-blob-container"></a><span data-ttu-id="3e7f5-129">Skapa en blobcontainer</span><span class="sxs-lookup"><span data-stu-id="3e7f5-129">Create a blob container</span></span>

<span data-ttu-id="3e7f5-130">Vi är inte anslutna till vårt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-130">We aren't connected to our Azure Storage account.</span></span> <span data-ttu-id="3e7f5-131">Kom ihåg att vår blob-utlösare endast övervakar den plats som beskrivs i fältet **Sökväg**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-131">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="3e7f5-132">Som standard bör sökvägen vara:</span><span class="sxs-lookup"><span data-stu-id="3e7f5-132">By default, our path should be:</span></span>

> <span data-ttu-id="3e7f5-133">samples-workitems/{namn}</span><span class="sxs-lookup"><span data-stu-id="3e7f5-133">samples-workitems/{name}</span></span>

<span data-ttu-id="3e7f5-134">Vi måste skapa en behållare med namnet **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-134">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="3e7f5-135">Expandera ditt lagringskonto i Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-135">In Storage explorer, expand your storage account.</span></span> <span data-ttu-id="3e7f5-136">Namnet ska vara det **Visningsnamn** som du angav vid anslutningen.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-136">The name should be the **Display name** that you provided during the connection process.</span></span>

1. <span data-ttu-id="3e7f5-137">Högerklicka på **Blobbehållare** och välj **Skapa blobbehållare**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-137">Right-click **Blob Containers** and select **Create blob container**.</span></span>

1. <span data-ttu-id="3e7f5-138">Ange **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-138">Enter **samples-workitems**.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="3e7f5-139">Aktivera din blob-utlösare</span><span class="sxs-lookup"><span data-stu-id="3e7f5-139">Turn on your blob trigger</span></span>

<span data-ttu-id="3e7f5-140">Nu när vi har skapat vår behållare som ska övervakas, kör vi vår funktion för att kunna se utdata när en blob skapas.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-140">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="3e7f5-141">Välj den blob-utlösare som öppnar kodskärmen.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-141">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="3e7f5-142">Välj **Kör**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-142">Select **Run**.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="3e7f5-143">Skapa en blob</span><span class="sxs-lookup"><span data-stu-id="3e7f5-143">Create a blob</span></span>

<span data-ttu-id="3e7f5-144">Nu är blobutlösaren aktiverad och lyssnar efter aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-144">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="3e7f5-145">Nu ska vi skapa en blob och se om vi får ett loggmeddelande.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-145">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="3e7f5-146">I Lagringsutforskaren väljer du behållaren **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-146">In Storage explorer, select the **samples-workitems** container.</span></span>

1. <span data-ttu-id="3e7f5-147">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-147">Select **Upload**.</span></span> 

1. <span data-ttu-id="3e7f5-148">Välj **Överför filer**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-148">Select **Upload Files**.</span></span>

1. <span data-ttu-id="3e7f5-149">Välj en fil från datorn.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-149">Select any file from your computer.</span></span>

1. <span data-ttu-id="3e7f5-150">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-150">Select **Upload**.</span></span>

1. <span data-ttu-id="3e7f5-151">Gå tillbaka till Azure.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-151">Go back to Azure.</span></span> <span data-ttu-id="3e7f5-152">Titta i dina loggar om det finns något meddelande som visar vilken fil som överfördes.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-152">Check your logs for a message that displays what file was uploaded.</span></span>

## <a name="clean-up"></a><span data-ttu-id="3e7f5-153">Rensa</span><span class="sxs-lookup"><span data-stu-id="3e7f5-153">Clean up</span></span>

<span data-ttu-id="3e7f5-154">För att undvika att debiteras för den här funktionen väljer du **Pausa** ovanför loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="3e7f5-154">To ensure that you aren't charged for this function, select **Pause** above the log window.</span></span>

![Pausa](../media-drafts/4-pause-timer.png)


