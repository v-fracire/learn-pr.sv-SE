<span data-ttu-id="44c6d-101">I den här övningen ska du skapa ett nytt lagringskonto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="44c6d-101">In this exercise, you will create a new storage account in your Azure subscription.</span></span> <span data-ttu-id="44c6d-102">Du kommer sedan att använda Azure Cloud Shell till att skapa en ny kö, lägga till ett meddelande i den, läsa meddelandet och sedan ta bort det från kön.</span><span class="sxs-lookup"><span data-stu-id="44c6d-102">You will then use Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="44c6d-103">Det här är samma åtgärder som vidtas av komponenterna i ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="44c6d-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="44c6d-104">En mobilapp kan till exempel lägga till ett meddelande i en kö, där det väntar på att en webbtjänst ska hämta och bearbeta det.</span><span class="sxs-lookup"><span data-stu-id="44c6d-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="44c6d-105">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="44c6d-105">Create a storage account</span></span>

<span data-ttu-id="44c6d-106">Eftersom Azure Storage-köer är en del av Azure-lagringskonton av typen generell användning måste du börja med att skapa ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="44c6d-106">Since Azure Storage queues are part of Azure general purpose storage accounts, you must start by creating a storage account:</span></span>

1. <span data-ttu-id="44c6d-107">Öppna en webbläsare och gå till [Azure-portalen](http://portal.azure.com) och logga in med dina vanliga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="44c6d-107">In a browser, navigate to the [Azure portal](http://portal.azure.com), and sign in with your normal credentials.</span></span>
1. <span data-ttu-id="44c6d-108">Klicka på **Alla tjänster** i det övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="44c6d-108">In the top left, click **All services**.</span></span>
1. <span data-ttu-id="44c6d-109">Rulla ned till avsnittet **Lagring** och klicka sedan på **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-109">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>
1. <span data-ttu-id="44c6d-110">Klicka på **Lägg till** längst upp till vänster på bladet **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-110">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

    ![Skärmbild bladet Lagringskonton med Lägg till markerat](../images/5-create-a-storage-account-1.png)

1. <span data-ttu-id="44c6d-112">Ange ett unikt namn för lagringskontot i textrutan **Namn**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-112">In the **Name** text box, type a unique name for the storage account.</span></span>
1. <span data-ttu-id="44c6d-113">Kontrollera att **Resource Manager** är valt under **Distributionsmodell**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-113">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
1. <span data-ttu-id="44c6d-114">Välj **Lagring (generell användning v2)** i listrutan **Typ av konto**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-114">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
1. <span data-ttu-id="44c6d-115">Välj en region nära dig i listrutan **Plats**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-115">In the **Location** drop-down list, select a region near you.</span></span>
1. <span data-ttu-id="44c6d-116">Välj **Lokalt redundant lagring (LRS)** i listrutan **Replikering**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-116">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
1. <span data-ttu-id="44c6d-117">Välj **Standard** under **Prestanda**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-117">Under **Performance**, select **Standard**.</span></span>
1. <span data-ttu-id="44c6d-118">Under **Åtkomstnivå** väljer du **Lågfrekvent**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-118">Under **Access tier**, select **Cool**.</span></span>
1. <span data-ttu-id="44c6d-119">Under **Säker överföring krävs** väljer du **Inaktiverat**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-119">Under **Secure transfer required**, select **Disabled**.</span></span>
1. <span data-ttu-id="44c6d-120">Välj din prenumeration under **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-120">Under **Subscription**, select your subscription.</span></span>

    ![Skärmbild av dialogrutan Skapa lagringskonto](../images/5-create-a-storage-account-2.png)

1. <span data-ttu-id="44c6d-122">Välj **Skapa ny** under **Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-122">Under **Resource group**, select **Create new**.</span></span> <span data-ttu-id="44c6d-123">Skriv **MusicSharingResourceGroup** i textrutan.</span><span class="sxs-lookup"><span data-stu-id="44c6d-123">In the text box, type **MusicSharingResourceGroup**.</span></span>
1. <span data-ttu-id="44c6d-124">Välj **Inaktiverat** under **Virtuella nätverk** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-124">Under **Virtual networks**, select **Disabled**, and then click **Create**.</span></span>

    ![Skärmbild av dialogrutan Skapa lagringskonto med Skapa markerat](../images/5-create-a-storage-account-3.png)

<span data-ttu-id="44c6d-126">Azure skapar det nya lagringskontot och den nya resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="44c6d-126">Azure creates the new storage account and the new resource group.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="44c6d-127">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="44c6d-127">Create a queue</span></span>

<span data-ttu-id="44c6d-128">Nu när lagringskontot har skapats kan du lägga till en ny kö till det.</span><span class="sxs-lookup"><span data-stu-id="44c6d-128">Now that the storage account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="44c6d-129">Du måste skapa kön med hjälp av PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="44c6d-129">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="44c6d-130">Klicka på länken **Cloud Shell** uppe till höger i portalen.</span><span class="sxs-lookup"><span data-stu-id="44c6d-130">In the top right of the portal, click the **Cloud Shell** link.</span></span>

    ![Skärmbild av Azure-portalen med ikonen Cloud Shell markerat](../images/5-create-a-storage-queue-1.png)

1. <span data-ttu-id="44c6d-132">På skärmen **Välkommen till Azure Cloud Shell** klickar du på **PowerShell (Linux)**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-132">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>
1. <span data-ttu-id="44c6d-133">Om skärmen **You have no storage mounted** (Du har ingen lagring monterad) visas klickar du på **Skapa lagring**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-133">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>
1. <span data-ttu-id="44c6d-134">Skapa lagringskontot genom att skriva följande kommando när kommandotolken `PS Azure` visas.</span><span class="sxs-lookup"><span data-stu-id="44c6d-134">When the `PS Azure` prompt appears, to obtain the storage account, type the following command.</span></span> <span data-ttu-id="44c6d-135">Ersätt `<storageaccountname>` med det unika namnet för ditt lagringskonto och tryck sedan på retur:</span><span class="sxs-lookup"><span data-stu-id="44c6d-135">Substitute `<storageaccountname>` with the unique name of your storage account, and then press Enter:</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="44c6d-136">Ange följande kommando och tryck sedan på retur för att få kontexten för lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="44c6d-136">To obtain the context of the storage account, type the following command, and then press Enter:</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="44c6d-137">Ange följande kommando och tryck sedan på retur för att skapa en ny kö:</span><span class="sxs-lookup"><span data-stu-id="44c6d-137">To create a new queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="44c6d-138">Lägga till ett meddelande till kön</span><span class="sxs-lookup"><span data-stu-id="44c6d-138">Add a message to the queue</span></span>

<span data-ttu-id="44c6d-139">Nu när du har skapat en kö i lagringskontot kan du lägga till ett meddelande till det.</span><span class="sxs-lookup"><span data-stu-id="44c6d-139">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="44c6d-140">Ange följande kommando och tryck sedan på retur för att skapa ett nytt meddelande:</span><span class="sxs-lookup"><span data-stu-id="44c6d-140">To create a new message, type the following command, and then press Enter:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="44c6d-141">Ange följande kommando och tryck sedan på retur för att lägga till det nya meddelandet till den nya kön:</span><span class="sxs-lookup"><span data-stu-id="44c6d-141">To add the new message to the new queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. <span data-ttu-id="44c6d-142">Klicka på **Alla resurser** i menyn på vänster sida i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="44c6d-142">In the Azure portal, in the navigation on the left, click **All resources**.</span></span>
1. <span data-ttu-id="44c6d-143">Klicka på lagringskontot du skapade tidigare i listan med resurser.</span><span class="sxs-lookup"><span data-stu-id="44c6d-143">In the list of resources, click the storage account you created earlier.</span></span>
1. <span data-ttu-id="44c6d-144">Klicka på **Storage Explorer (förhandsversion)** på bladet för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="44c6d-144">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>
1. <span data-ttu-id="44c6d-145">Klicka på **musicsharingmessages** i Storage Explorer under **KÖER**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-145">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="44c6d-146">Storage Explorer visar meddelandet du just lade till.</span><span class="sxs-lookup"><span data-stu-id="44c6d-146">The Storage Explorer displays the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="44c6d-147">Hämta och ta bort meddelandet</span><span class="sxs-lookup"><span data-stu-id="44c6d-147">Retrieve and remove the message</span></span>

<span data-ttu-id="44c6d-148">En målkomponent för ett meddelande i en lagringskö måste hämta meddelandet längst fram i kön.</span><span class="sxs-lookup"><span data-stu-id="44c6d-148">A destination component for a message in a Storage queue must retrieve the message at the front of the queue.</span></span> <span data-ttu-id="44c6d-149">Målkomponenten måste sedan bearbeta meddelandet och ta bort det från kön så att andra komponenter inte hämtar det.</span><span class="sxs-lookup"><span data-stu-id="44c6d-149">Then the destination component must process the message, and delete it from the queue so that other components do not retrieve it.</span></span>

1. <span data-ttu-id="44c6d-150">För att hämta meddelandet som är först i kön skriver du följande kommando och trycker sedan på retur i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="44c6d-150">In Cloud Shell, to get the message at the front of the queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="44c6d-151">Ange följande kommando och tryck sedan på retur för att visa meddelandet:</span><span class="sxs-lookup"><span data-stu-id="44c6d-151">To display the message, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="44c6d-152">Ange följande kommando och tryck sedan på retur för att visa alla egenskaper för meddelandet:</span><span class="sxs-lookup"><span data-stu-id="44c6d-152">To display all the properties of the message, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="44c6d-153">Ange följande kommando och tryck sedan på retur för att ta bort meddelandet från kön:</span><span class="sxs-lookup"><span data-stu-id="44c6d-153">To remove the message from the queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="44c6d-154">Du kan uppdatera kövisningen i Azure-portalen genom att gå till bladet Lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="44c6d-154">In the Azure portal, to refresh the queue display, go to the Storage Account blade.</span></span> <span data-ttu-id="44c6d-155">Klicka på **Översikt** och sedan på **Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-155">Click **Overview**, and then click **Storage Explorer**.</span></span>
1. <span data-ttu-id="44c6d-156">Klicka på **musicsharingmessages** under **KÖER**.</span><span class="sxs-lookup"><span data-stu-id="44c6d-156">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="44c6d-157">Storage Explorer visar att kön är tom eftersom du har tagit bort det enda meddelandet.</span><span class="sxs-lookup"><span data-stu-id="44c6d-157">The Storage Explorer shows that the queue is empty because you removed the only message.</span></span>

## <a name="cleanup"></a><span data-ttu-id="44c6d-158">Rensa</span><span class="sxs-lookup"><span data-stu-id="44c6d-158">Cleanup</span></span>

<span data-ttu-id="44c6d-159">Om du vill ta bort alla resurser som skapades under den här övningen anger du följande kommando i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="44c6d-159">To remove all resources created during this exercise, enter the following command in Cloud Shell:</span></span> 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a><span data-ttu-id="44c6d-160">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="44c6d-160">Summary</span></span>

<span data-ttu-id="44c6d-161">Här skapade du ett lagringskonto i din Azure-prenumeration och en ny kö i det.</span><span class="sxs-lookup"><span data-stu-id="44c6d-161">Here, you created a storage account in your Azure subscription, and created a new queue in it.</span></span> <span data-ttu-id="44c6d-162">Du kan även använda PowerShell för att simulera åtgärderna för distribuerade programkomponenter genom att lägga till ett meddelande till kön och sedan hämta och ta bort det.</span><span class="sxs-lookup"><span data-stu-id="44c6d-162">You also used PowerShell to simulate the actions of distributed application components by adding a message to the queue, and then retrieving and removing it.</span></span>

<span data-ttu-id="44c6d-163">Köer för lagringskonton är en bra lösning när du vill skicka meddelanden mellan komponenterna i ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="44c6d-163">Storage account queues are a good solution when you want to pass messages between the components of a distributed application.</span></span> <span data-ttu-id="44c6d-164">Välj inte lagringsköer om du vill publicera händelser.</span><span class="sxs-lookup"><span data-stu-id="44c6d-164">Do not choose Storage queues when you want to publish events.</span></span>