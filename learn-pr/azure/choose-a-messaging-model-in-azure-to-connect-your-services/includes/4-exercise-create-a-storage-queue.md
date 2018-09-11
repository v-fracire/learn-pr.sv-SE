<span data-ttu-id="e638a-101">I den här övningen ska du skapa ett nytt lagringskonto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e638a-101">In this exercise, you will create a new storage account in your Azure subscription.</span></span> <span data-ttu-id="e638a-102">Du kommer sedan att använda Azure Cloud Shell till att skapa en ny kö, lägga till ett meddelande i den, läsa meddelandet och sedan ta bort det från kön.</span><span class="sxs-lookup"><span data-stu-id="e638a-102">You will then use Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="e638a-103">Det här är samma åtgärder som vidtas av komponenterna i ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="e638a-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="e638a-104">En mobilapp kan till exempel lägga till ett meddelande i en kö, där det väntar på att en webbtjänst ska hämta och bearbeta det.</span><span class="sxs-lookup"><span data-stu-id="e638a-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="e638a-105">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e638a-105">Create a storage account</span></span>

<span data-ttu-id="e638a-106">Eftersom Azure Storage-köer ingår i Azure-lagringskonton för generell användning, måste du börja med att skapa ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="e638a-106">Since Azure Storage queues are part of Azure general-purpose storage accounts, you must start by creating a storage account:</span></span>

1. <span data-ttu-id="e638a-107">I en webbläsare går du till [Azure Portal](https://portal.azure.com?azure-portal=true) och loggar in med dina vanliga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e638a-107">In a browser, navigate to the [Azure portal](https://portal.azure.com?azure-portal=true), and sign in with your normal credentials.</span></span>

1. <span data-ttu-id="e638a-108">Klicka på **Alla tjänster** i det övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="e638a-108">In the top left, click **All services**.</span></span>

1. <span data-ttu-id="e638a-109">Rulla ned till avsnittet **Lagring** och klicka sedan på **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="e638a-109">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>

1. <span data-ttu-id="e638a-110">Klicka på **Lägg till** längst upp till vänster på bladet **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="e638a-110">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

  ![Skärmbild av bladet Lagringskonton med Lägg till markerat](../media-draft/4-create-a-storage-account-1.png)

1. <span data-ttu-id="e638a-112">Ange följande information i dialogrutan som visas. Vart och ett av dessa alternativ har en `(i)`- ikon i portalen som du kan använda för att få mer information om alternativet.</span><span class="sxs-lookup"><span data-stu-id="e638a-112">In the resulting dialog, enter the following information, each of these options has a `(i)` icon in the portal which you can use to get more information about what the option does.</span></span>

    - <span data-ttu-id="e638a-113">Ange ett unikt namn för lagringskontot i textrutan **Namn**.</span><span class="sxs-lookup"><span data-stu-id="e638a-113">In the **Name** text box, type a unique name for the storage account.</span></span>
    - <span data-ttu-id="e638a-114">Kontrollera att **Resource Manager** är valt under **Distributionsmodell**.</span><span class="sxs-lookup"><span data-stu-id="e638a-114">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
    - <span data-ttu-id="e638a-115">Välj **Lagring (generell användning v2)** i listrutan **Typ av konto**.</span><span class="sxs-lookup"><span data-stu-id="e638a-115">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
    - <span data-ttu-id="e638a-116">Välj en region nära dig i listrutan **Plats**.</span><span class="sxs-lookup"><span data-stu-id="e638a-116">In the **Location** drop-down list, select a region near you.</span></span>
    - <span data-ttu-id="e638a-117">Välj **Lokalt redundant lagring (LRS)** i listrutan **Replikering**.</span><span class="sxs-lookup"><span data-stu-id="e638a-117">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
    - <span data-ttu-id="e638a-118">Välj **Standard** under **Prestanda**.</span><span class="sxs-lookup"><span data-stu-id="e638a-118">Under **Performance**, select **Standard**.</span></span>
    - <span data-ttu-id="e638a-119">Under **Åtkomstnivå** väljer du **Lågfrekvent**.</span><span class="sxs-lookup"><span data-stu-id="e638a-119">Under **Access tier**, select **Cool**.</span></span>
    - <span data-ttu-id="e638a-120">Under **Säker överföring krävs** väljer du **Inaktiverat**.</span><span class="sxs-lookup"><span data-stu-id="e638a-120">Under **Secure transfer required**, select **Disabled**.</span></span>
    - <span data-ttu-id="e638a-121">Välj din prenumeration under **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="e638a-121">Under **Subscription**, select your subscription.</span></span>
    - <span data-ttu-id="e638a-122">Välj **Skapa ny** under **Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="e638a-122">Under **Resource group**, select **Create new**.</span></span> <span data-ttu-id="e638a-123">Skriv **MusicSharingResourceGroup** i textrutan.</span><span class="sxs-lookup"><span data-stu-id="e638a-123">In the text box, type **MusicSharingResourceGroup**.</span></span>
    - <span data-ttu-id="e638a-124">Under **Virtuella nätverk** väljer du **Inaktiverat**.</span><span class="sxs-lookup"><span data-stu-id="e638a-124">Under **Virtual networks**, select **Disabled**.</span></span> 

    ![Skärmbild av dialogrutan Skapa lagringskonto](../media-draft/4-create-a-storage-account-2.png)

1. <span data-ttu-id="e638a-126">Klicka på **Skapa** – Azure skapar en ny resursgrupp och ett nytt lagringskonto som är associerade med den.</span><span class="sxs-lookup"><span data-stu-id="e638a-126">Click **Create** - Azure will create a new resource group and a new storage account associated with it.</span></span>

    ![Skärmbild av dialogrutan Skapa lagringskonto med Skapa markerat](../media-draft/4-create-a-storage-account-3.png)

## <a name="create-a-queue"></a><span data-ttu-id="e638a-128">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="e638a-128">Create a queue</span></span>

<span data-ttu-id="e638a-129">Nu när lagringskontot har skapats kan du lägga till en ny kö till det.</span><span class="sxs-lookup"><span data-stu-id="e638a-129">Now that the storage account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="e638a-130">Du måste skapa kön med hjälp av PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="e638a-130">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="e638a-131">Klicka på länken **Cloud Shell** uppe till höger i portalen `(>_)`.</span><span class="sxs-lookup"><span data-stu-id="e638a-131">In the top right of the portal, click the **Cloud Shell** link `(>_)`.</span></span>

    ![Skärmbild av Azure Portal med ikonen Cloud Shell markerad](../media-draft/4-create-a-storage-queue-1.png)

1. <span data-ttu-id="e638a-133">På skärmen **Välkommen till Azure Cloud Shell** klickar du på **PowerShell (Linux)**.</span><span class="sxs-lookup"><span data-stu-id="e638a-133">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>

1. <span data-ttu-id="e638a-134">Om skärmen **You have no storage mounted** (Du har ingen lagring monterad) visas klickar du på **Skapa lagring**.</span><span class="sxs-lookup"><span data-stu-id="e638a-134">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>

1. <span data-ttu-id="e638a-135">Skapa lagringskontot genom att skriva följande kommando när kommandotolken `PS Azure` visas.</span><span class="sxs-lookup"><span data-stu-id="e638a-135">When the `PS Azure` prompt appears, to obtain the storage account, type the following command.</span></span> <span data-ttu-id="e638a-136">Ersätt `<storageaccountname>` med det unika namnet för ditt lagringskonto som du skapade ovan och tryck sedan på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="e638a-136">Substitute `<storageaccountname>` with the unique name of your storage account you created above, and then press **Enter**.</span></span> <span data-ttu-id="e638a-137">Vi tilldelar det resulterande objektet till en variabel med namnet `$storageaccount`.</span><span class="sxs-lookup"><span data-stu-id="e638a-137">We want to assign the resulting object to a variable named `$storageaccount`.</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="e638a-138">Därefter måste vi hämta lagringskontots _sammanhang_ – vilket är en egenskap för det returnerade objektet.</span><span class="sxs-lookup"><span data-stu-id="e638a-138">Next, we need to get the storage account _context_ - this is a property on the returned object.</span></span> <span data-ttu-id="e638a-139">Nu ska vi tilldela det till en annan variabel med namnet `$context`.</span><span class="sxs-lookup"><span data-stu-id="e638a-139">Let's assign it to another variable named `$context`.</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="e638a-140">Nu är vi redo att skapa kön.</span><span class="sxs-lookup"><span data-stu-id="e638a-140">Now we are ready to create the queue.</span></span> <span data-ttu-id="e638a-141">Använd kommandot `New-AzureStorageQueue` och tilldela den till en `$messageQueue`-variabel.</span><span class="sxs-lookup"><span data-stu-id="e638a-141">Use the `New-AzureStorageQueue` command and assign it to a `$messageQueue` variable.</span></span>
    - <span data-ttu-id="e638a-142">Skicka en `-Name`-parameter med värdet `musicsharingmessages`</span><span class="sxs-lookup"><span data-stu-id="e638a-142">Pass a `-Name` parameter with the value `musicsharingmessages`</span></span>
    - <span data-ttu-id="e638a-143">Skicka en `-Context`-parameter med värdet som du hämtade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e638a-143">Pass a `-Context` parameter with the value you retrieved in the previous step.</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="e638a-144">Lägga till ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="e638a-144">Add a message to the queue</span></span>

<span data-ttu-id="e638a-145">Nu när du har skapat en kö på lagringskontot kan du lägga till ett meddelande i den.</span><span class="sxs-lookup"><span data-stu-id="e638a-145">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="e638a-146">Du skapar ett nytt meddelande med metoden `New-Object` där du skapar en .NET `CloudQueueMessage` med ett strängbaserat argument:</span><span class="sxs-lookup"><span data-stu-id="e638a-146">To create a new message, use the `New-Object` method to create a .NET `CloudQueueMessage` with a string-based argument:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="e638a-147">Om du vill lägga till det nya meddelandet i den nya kön, skickar du den skapade `CloudQueueMessage` till `AddMessageAsync`-metoden i din `$messageQueue`-kö.</span><span class="sxs-lookup"><span data-stu-id="e638a-147">To add the new message to the new queue, pass the created `CloudQueueMessage` to the `AddMessageAsync` method on your `$messageQueue` queue.</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

## <a name="verify-the-message-was-queued"></a><span data-ttu-id="e638a-148">Kontrollera att meddelandet placerades i kön</span><span class="sxs-lookup"><span data-stu-id="e638a-148">Verify the message was queued</span></span>

<span data-ttu-id="e638a-149">Vi kan använda **Storage Explorer** när vi arbetar med vår kö.</span><span class="sxs-lookup"><span data-stu-id="e638a-149">We can use the **Storage Explorer** to work with our queue.</span></span> <span data-ttu-id="e638a-150">Det finns två tillgängliga varianter:</span><span class="sxs-lookup"><span data-stu-id="e638a-150">There are two variations available:</span></span>

- <span data-ttu-id="e638a-151">En plattformsoberoende skrivbordsapp för Linux, macOS och Windows som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="e638a-151">A cross-platform desktop app for Linux, macOS, and Windows that you can download.</span></span>
- <span data-ttu-id="e638a-152">En webbförhandsversion i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="e638a-152">A preview web version in the Azure portal.</span></span> <span data-ttu-id="e638a-153">Det är den som vi använder här, men du kan installera skrivbordsversionen om du föredrar det – instruktionerna är mycket lika.</span><span class="sxs-lookup"><span data-stu-id="e638a-153">This is the one we will use here, but you can install the desktop version if you prefer - the instructions are very similar.</span></span>

1. <span data-ttu-id="e638a-154">Klicka på **Alla resurser** i menyn på vänster sida i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="e638a-154">In the Azure portal, in the navigation on the left, click **All resources**.</span></span>

1. <span data-ttu-id="e638a-155">Klicka på lagringskontot du skapade tidigare i listan med resurser.</span><span class="sxs-lookup"><span data-stu-id="e638a-155">In the list of resources, click the storage account you created earlier.</span></span>

1. <span data-ttu-id="e638a-156">Klicka på **Storage Explorer (förhandsversion)** på bladet för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="e638a-156">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>

1. <span data-ttu-id="e638a-157">Klicka på **musicsharingmessages** i Storage Explorer under **KÖER**.</span><span class="sxs-lookup"><span data-stu-id="e638a-157">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="e638a-158">Storage Explorer bör nu visa meddelandet som du nyss lade till.</span><span class="sxs-lookup"><span data-stu-id="e638a-158">The Storage Explorer should display the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="e638a-159">Hämta och ta bort meddelandet</span><span class="sxs-lookup"><span data-stu-id="e638a-159">Retrieve and remove the message</span></span>

<span data-ttu-id="e638a-160">En målkomponent för ett meddelande i en lagringskö måste hämta meddelandet som ligger först i kön.</span><span class="sxs-lookup"><span data-stu-id="e638a-160">A destination component for a message in a Storage queue must retrieve the message at the front of the queue.</span></span> <span data-ttu-id="e638a-161">Målkomponenten måste sedan bearbeta meddelandet och ta bort det från kön så att andra komponenter inte hämtar det.</span><span class="sxs-lookup"><span data-stu-id="e638a-161">Then the destination component must process the message and delete it from the queue so that other components do not retrieve it.</span></span>

1. <span data-ttu-id="e638a-162">Vi kan hämta det första meddelande som är tillgängligt i PowerShell med hjälp av `GetMessageAsync`-metoden i vår kö.</span><span class="sxs-lookup"><span data-stu-id="e638a-162">We can retrieve the first available message in PowerShell using the `GetMessageAsync` method on our queue.</span></span> <span data-ttu-id="e638a-163">Det här är en asynkron .NET-metod, eftersom vi vill vänta tills vi bara kan använda `Result`-egenskapen för att hämta det returnerade värdet.</span><span class="sxs-lookup"><span data-stu-id="e638a-163">This is an asynchronous .NET method, since we want to wait for it we can just use the `Result` property to get the return value.</span></span> <span data-ttu-id="e638a-164">Det här returnerar ett objekt som motsvarar det meddelande som vi kan tilldela till en parameter.</span><span class="sxs-lookup"><span data-stu-id="e638a-164">This returns an object representing the message which we can assign to a parameter.</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="e638a-165">Vi kan få en textversion av meddelandet genom att anropa `AsString` – detta kommer att visa värdet i konsolen.</span><span class="sxs-lookup"><span data-stu-id="e638a-165">We can get a textual version of the message by calling `AsString` - this will output the value on the console.</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="e638a-166">Vi kan också visa alla egenskaper för meddelandet genom att bara skriva variabelnamnet och trycka på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="e638a-166">Or, we can display all the properties of the message by just typing the variable name and pressing **Enter**.</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="e638a-167">`GetMessageAsync` tar *inte* bort meddelandet – det bara returnerar det, vilket innebär att vi kan bearbeta det igen.</span><span class="sxs-lookup"><span data-stu-id="e638a-167">`GetMessageAsync` does *not* remove the message - it simply returns it, which means we could process it again.</span></span> <span data-ttu-id="e638a-168">Om du vill ta bort meddelandet från kön kan vi använda `DeleteMessageAsync`-metoden på kön – detta kräver att vi skickar in det meddelande som vi ta bort.</span><span class="sxs-lookup"><span data-stu-id="e638a-168">To remove the message from the queue, we can use the `DeleteMessageAsync` method on the queue - this requires that we pass in the message we want to remove.</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="e638a-169">Om du vill kontrollera att meddelandet är borta, uppdaterar du kön som visas i Azure Portal genom att gå till bladet Lagringskonto och välja **Översikt > Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e638a-169">To verify that the message is gone, refresh the queue display in the Azure portal by navigating to the Storage Account blade and selecting **Overview > Storage Explorer**.</span></span> <span data-ttu-id="e638a-170">Klicka på **musicsharingmessages** under **KÖER**.</span><span class="sxs-lookup"><span data-stu-id="e638a-170">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="e638a-171">Storage Explorer bör nu visa att kön är tom eftersom du har tagit bort det enda meddelandet.</span><span class="sxs-lookup"><span data-stu-id="e638a-171">The Storage Explorer should now show that the queue is empty because you removed the only message.</span></span>


## <a name="summary"></a><span data-ttu-id="e638a-172">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e638a-172">Summary</span></span>
<span data-ttu-id="e638a-173">Köer för lagringskonton är en bra lösning när du vill skicka _meddelanden_ mellan komponenterna i ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="e638a-173">Storage account queues are a good solution when you want to pass _messages_ between the components of a distributed application.</span></span> <span data-ttu-id="e638a-174">Det här är ett bra alternativ om du vill garantera leveransen eller säkerställa att meddelandena levereras i samma ordning som du skickade dem.</span><span class="sxs-lookup"><span data-stu-id="e638a-174">This is a great choice if you want to guarantee delivery or to ensure that messages are delivered in the same order you sent them.</span></span> <span data-ttu-id="e638a-175">Köer innebär dock att avsändaren och mottagaren måste förstå formatet för de data som skickas – det finns ett underförstått datakontrakt mellan dem som lägger till en slags ”koppling” mellan de två kommunikationstjänsterna.</span><span class="sxs-lookup"><span data-stu-id="e638a-175">However, queues imply that the sender and receiver understand the format of the data being passed - there's an implied data contract between them which adds a bit of "coupling" between the two communicating services.</span></span>

<span data-ttu-id="e638a-176">Det är inte alla arkitekturer som behöver skicka formaterade datablock, vissa skickar egentligen bara enkla meddelanden som vi kan läsa och glömma utan att behöva ha någon kunskap om vad som hanterar meddelandet.</span><span class="sxs-lookup"><span data-stu-id="e638a-176">Not all architectures need to pass formatted blocks of data, some really just need simple messages which we want to fire-and-forget without any knowledge of what will handle the message.</span></span> <span data-ttu-id="e638a-177">I dessa scenarion är användningen av en kö inte något bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="e638a-177">In these scenarios, a queue isn't a great choice.</span></span> <span data-ttu-id="e638a-178">Låt oss titta på en annan strategi för meddelanden som är lämpligare än den här typen av kommunikation.</span><span class="sxs-lookup"><span data-stu-id="e638a-178">Let's look at another messaging strategy which is more suited to this style of communication.</span></span>