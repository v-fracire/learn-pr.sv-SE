<span data-ttu-id="3b11f-101">I den här övningen ska du skapa ett nytt lagringskonto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3b11f-101">In this exercise, you will create a new Storage Account in your Azure subscription.</span></span> <span data-ttu-id="3b11f-102">Du kommer sedan att använda Azure Cloud Shell för att skapa en ny kö, lägga till ett meddelande till den och sedan läsa meddelandet och ta bort det från kön.</span><span class="sxs-lookup"><span data-stu-id="3b11f-102">You will then use the Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="3b11f-103">Det här är samma åtgärder som vidtas av komponenterna i ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="3b11f-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="3b11f-104">En mobilapp kan till exempel lägga till ett meddelande till en kö där det väntar på att en webbtjänst ska hämta och bearbeta det.</span><span class="sxs-lookup"><span data-stu-id="3b11f-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="3b11f-105">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="3b11f-105">Create a Storage Account</span></span>

<span data-ttu-id="3b11f-106">Eftersom lagringsköer är en del av Azure-lagringskonton för generell användning.</span><span class="sxs-lookup"><span data-stu-id="3b11f-106">Since Storage queues are part of Azure general purpose Storage accounts.</span></span> <span data-ttu-id="3b11f-107">Du måste börja med att skapa ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="3b11f-107">You must start by creating a Storage account:</span></span>

1. <span data-ttu-id="3b11f-108">Öppna en webbläsare och gå till [Azure-portalen](http://portal.azure.com) och logga in med dina vanliga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3b11f-108">In a browser, navigate to the [Azure Portal](http://portal.azure.com) and sign in with your normal credentials.</span></span>
1. <span data-ttu-id="3b11f-109">Klicka på **Alla tjänster** i det övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="3b11f-109">In the top left, click **All services**.</span></span>
1. <span data-ttu-id="3b11f-110">Rulla ned till avsnittet **Lagring** och klicka på **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-110">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>
1. <span data-ttu-id="3b11f-111">Klicka på **Lägg till** längst upp till vänster på bladet **lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-111">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

    ![Skapa ett lagringskonto](../images/5-create-a-storage-account-1.png)

1. <span data-ttu-id="3b11f-113">Ange ett unikt namn för lagringskontot i textrutan **Namn**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-113">In the **Name** text box, type a unique name for the storage account.</span></span>
1. <span data-ttu-id="3b11f-114">Kontrollera att **Resource Manager** är valt under **Distributionsmodell**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-114">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
1. <span data-ttu-id="3b11f-115">Välj **Lagring (general-purpose v2)** i listrutan **Typ av konto**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-115">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
1. <span data-ttu-id="3b11f-116">Välj en region nära dig i listrutan **Plats**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-116">In the **Location** drop-down list, select a region near you.</span></span>
1. <span data-ttu-id="3b11f-117">Välj **Lokalt redundant lagring (LRS)** i listrutan **Replikering**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-117">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
1. <span data-ttu-id="3b11f-118">Välj **Standard** under **Prestanda**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-118">Under **Performance**, select **Standard**.</span></span>
1. <span data-ttu-id="3b11f-119">Välj **Lågfrekvent** under **Åtkomstnivå**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-119">Under **Access tier**, select **Cool**.</span></span>
1. <span data-ttu-id="3b11f-120">Välj **Inaktiverat** under **Säker överföring krävs**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-120">Under **Secure transfer required** select, **Disabled**.</span></span>
1. <span data-ttu-id="3b11f-121">Välj din prenumeration under **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-121">Under **Subscription**, select your subscription.</span></span>

    ![Skapa ett lagringskonto](../images/5-create-a-storage-account-2.png)

1. <span data-ttu-id="3b11f-123">Välj **Skapa ny** under **Resursgrupp** och skriv **MusicSharingResourceGroup** i textrutan.</span><span class="sxs-lookup"><span data-stu-id="3b11f-123">Under **Resource group** select **Create new**, and then in the textbox type **MusicSharingResourceGroup**.</span></span>
1. <span data-ttu-id="3b11f-124">Välj **Inaktiverat** under **Virtuella nätverk** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-124">Under **Virtual networks** select **Disabled** and then click **Create**.</span></span>

    ![Skapa ett lagringskonto](../images/5-create-a-storage-account-3.png)

<span data-ttu-id="3b11f-126">Azure skapar det nya lagringskontot och den nya resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3b11f-126">Azure creates the new storage account and the new resource group.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="3b11f-127">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="3b11f-127">Create a Queue</span></span>

<span data-ttu-id="3b11f-128">Nu när lagringskontot har skapats kan du lägga till en ny kö till det.</span><span class="sxs-lookup"><span data-stu-id="3b11f-128">Now that the Storage Account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="3b11f-129">Du måste skapa kön med hjälp av PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="3b11f-129">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="3b11f-130">Klicka på länken **Cloud Shell** uppe till höger i portalen.</span><span class="sxs-lookup"><span data-stu-id="3b11f-130">In the top right of the portal, click the **Cloud Shell** link.</span></span>

    ![Starta Cloud Shell](../images/5-create-a-storage-queue-1.png)

1. <span data-ttu-id="3b11f-132">På skärmen **Välkommen till Azure Cloud Shell** klickar du på **PowerShell (Linux)**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-132">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>
1. <span data-ttu-id="3b11f-133">Om skärmen **Du har ingen lagring monterad** visas klickar du på **Skapa lagring**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-133">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>
1. <span data-ttu-id="3b11f-134">När frågan `PS Azure` visas skriver du följande kommando och ersätter `<storageaccountname>` med det unika namnet på ditt lagringskonto och sedan trycker du på retur för att hämta lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="3b11f-134">When the `PS Azure` prompt appears, to obtain the storage account, type the following command, substituting `<storageaccountname>` with the unique name of your storage account, and then press Enter:</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="3b11f-135">Ange följande kommando och tryck sedan på retur för att få kontexten för lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="3b11f-135">To obtain the context of the storage account, type the following command and then press Enter:</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="3b11f-136">Ange följande kommando och tryck sedan på retur för att skapa en ny kö:</span><span class="sxs-lookup"><span data-stu-id="3b11f-136">To create a new queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="3b11f-137">Lägg till ett meddelande till kön</span><span class="sxs-lookup"><span data-stu-id="3b11f-137">Add a Message to the Queue</span></span>

<span data-ttu-id="3b11f-138">Nu när du har skapat en kö i lagringskontot kan du lägga till ett meddelande till det.</span><span class="sxs-lookup"><span data-stu-id="3b11f-138">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="3b11f-139">Ange följande kommando och tryck sedan på retur för att skapa ett nytt meddelande:</span><span class="sxs-lookup"><span data-stu-id="3b11f-139">To create a new message, type the following command and then press Enter:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="3b11f-140">Ange följande kommando och tryck sedan på retur för att lägga till det nya meddelandet till den nya kön:</span><span class="sxs-lookup"><span data-stu-id="3b11f-140">To add the new message to the new queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. <span data-ttu-id="3b11f-141">Klicka på **Alla resurser** i menyn på vänster sida i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b11f-141">In the Azure Portal, in the navigation on the left, click **All resources**.</span></span>
1. <span data-ttu-id="3b11f-142">Klicka på lagringskontot du skapade tidigare i listan över resurser.</span><span class="sxs-lookup"><span data-stu-id="3b11f-142">In the list of resources, click the storage account you created earlier.</span></span>
1. <span data-ttu-id="3b11f-143">Klicka på **Storage Explorer (Förhandsversion)** på bladet för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="3b11f-143">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>
1. <span data-ttu-id="3b11f-144">Klicka på **musicsharingmessages** i Storage Explorer under **KÖER**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-144">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="3b11f-145">Storage Explorer visar meddelandet du just lade till.</span><span class="sxs-lookup"><span data-stu-id="3b11f-145">The Storage Explorer displays the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="3b11f-146">Hämta och ta bort meddelandet</span><span class="sxs-lookup"><span data-stu-id="3b11f-146">Retrieve and Remove the Message</span></span>

<span data-ttu-id="3b11f-147">En målkomponent för ett meddelande i en lagringskö måste hämta meddelandet som är först i kön, bearbeta det och sedan ta bort det från kön så att andra komponenter inte hämtar det:</span><span class="sxs-lookup"><span data-stu-id="3b11f-147">A destination component for a message in a Storage queue, must retrieve the message at the front of the queue, process it, and then delete it from the queue so that other components do not retrieve it:</span></span>

1. <span data-ttu-id="3b11f-148">För att hämta meddelandet som är först i kön skriver du följande kommando och trycker sedan på retur i Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="3b11f-148">In the Azure Cloud Shell, to get the message at the front of the queue, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="3b11f-149">Ange följande kommando och tryck sedan på retur för att visa meddelandet:</span><span class="sxs-lookup"><span data-stu-id="3b11f-149">To display the message, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="3b11f-150">Ange följande kommando och tryck sedan på retur för att visa alla egenskaper för meddelandet:</span><span class="sxs-lookup"><span data-stu-id="3b11f-150">To display all the properties of the message, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="3b11f-151">Ange följande kommando och tryck sedan på retur för att ta bort meddelandet från kön:</span><span class="sxs-lookup"><span data-stu-id="3b11f-151">To remove the message from the queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="3b11f-152">Klicka på **Översikt** och sedan på **Storage Explorer** i Azure-portalen för att uppdatera kövyn på bladet lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3b11f-152">In the Azure Portal, to refresh the queue display, in the Storage Account blade, click **Overview** and then click **Storage Explorer**.</span></span>
1. <span data-ttu-id="3b11f-153">Klicka på **musicsharingmessages** under **KÖER**.</span><span class="sxs-lookup"><span data-stu-id="3b11f-153">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="3b11f-154">Storage Explorer visar att kön är tom eftersom du har tagit bort det enda meddelandet.</span><span class="sxs-lookup"><span data-stu-id="3b11f-154">The Storage Explorer shows that the queue is empty because you removed the only message.</span></span>

## <a name="cleanup"></a><span data-ttu-id="3b11f-155">Rensa</span><span class="sxs-lookup"><span data-stu-id="3b11f-155">Cleanup</span></span>

<span data-ttu-id="3b11f-156">Om du vill ta bort alla resurser som skapades under den här övningen anger du följande kommando i Azure Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="3b11f-156">To remove all resources created during this exercise enter the following command in the Azure Cloud Shell</span></span> 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a><span data-ttu-id="3b11f-157">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3b11f-157">Summary</span></span>

<span data-ttu-id="3b11f-158">Här skapade du ett lagringskonto i din Azure-prenumeration och en ny kö i det.</span><span class="sxs-lookup"><span data-stu-id="3b11f-158">Here, you created a Storage Account in your Azure subscription and created a new queue in it.</span></span> <span data-ttu-id="3b11f-159">Du kan även använda PowerShell för att simulera åtgärderna för distribuerade programkomponenter genom att lägga till ett meddelande till kön och sedan hämta och ta bort det.</span><span class="sxs-lookup"><span data-stu-id="3b11f-159">You also used PowerShell to simulate the actions of distributed application components by adding a message to the queue and then retrieving and removing it.</span></span>

<span data-ttu-id="3b11f-160">Köer för Azure lagringskonton är en bra lösning när du vill skicka meddelanden mellan komponenterna i ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="3b11f-160">Azure Storage Account queues are a good solution when you want to pass messages between the components of a distributed application.</span></span> <span data-ttu-id="3b11f-161">Välj inte lagringsköer om du vill publicera händelser.</span><span class="sxs-lookup"><span data-stu-id="3b11f-161">Do not choose Storage queues when you want to publish events.</span></span>