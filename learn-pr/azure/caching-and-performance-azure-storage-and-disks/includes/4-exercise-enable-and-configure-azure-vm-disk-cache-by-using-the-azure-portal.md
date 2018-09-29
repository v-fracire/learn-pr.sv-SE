
<span data-ttu-id="bafa5-101">Anta att du har en webbplats för fotodelning med data som lagras på virtuella Azure-datorer (VM) som kör SQL Server och anpassade program.</span><span class="sxs-lookup"><span data-stu-id="bafa5-101">Suppose you run a photo sharing site, with data stored on Azure virtual machines (VMs) running SQL Server and custom applications.</span></span> <span data-ttu-id="bafa5-102">Du vill göra följande justeringar:</span><span class="sxs-lookup"><span data-stu-id="bafa5-102">You want to make the following adjustments:</span></span>

- <span data-ttu-id="bafa5-103">Du behöver ändra inställningarna för diskcache på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bafa5-103">You need to change the disk cache settings on a VM.</span></span>
- <span data-ttu-id="bafa5-104">Du vill lägga till en ny datadisk till den virtuella datorn med cachelagring aktiverat.</span><span class="sxs-lookup"><span data-stu-id="bafa5-104">You want to add a new data disk to the VM with caching enabled.</span></span>

<span data-ttu-id="bafa5-105">Du har valt att göra dessa ändringar via Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="bafa5-105">You've decided to make these changes through the Azure portal.</span></span>

<span data-ttu-id="bafa5-106">I den här övningen går vi igenom hur du genomför de ändringar av en virtuell dator som beskrevs ovan.</span><span class="sxs-lookup"><span data-stu-id="bafa5-106">In this exercise, we'll walk through making the changes to a VM that we described above.</span></span> <span data-ttu-id="bafa5-107">Först loggar vi in på portalen och skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bafa5-107">First, let's sign in to the portal and create a VM.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-virtual-machine"></a><span data-ttu-id="bafa5-108">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="bafa5-108">Create a virtual machine</span></span>

<span data-ttu-id="bafa5-109">I det här steget skapar vi en virtuell dator med följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="bafa5-109">In this step, we're going to create a VM with the following properties:</span></span>

| <span data-ttu-id="bafa5-110">Egenskap</span><span class="sxs-lookup"><span data-stu-id="bafa5-110">Property</span></span>        | <span data-ttu-id="bafa5-111">Värde</span><span class="sxs-lookup"><span data-stu-id="bafa5-111">Value</span></span>   |
|-----------------|---------|
| <span data-ttu-id="bafa5-112">Avbildning</span><span class="sxs-lookup"><span data-stu-id="bafa5-112">Image</span></span>           | <span data-ttu-id="bafa5-113">**Windows Server 2016 Datacenter**</span><span class="sxs-lookup"><span data-stu-id="bafa5-113">**Windows Server 2016 Datacenter**</span></span> |
| <span data-ttu-id="bafa5-114">Namn</span><span class="sxs-lookup"><span data-stu-id="bafa5-114">Name</span></span>            | <span data-ttu-id="bafa5-115">**fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="bafa5-115">**fotoshareVM**</span></span> |
| <span data-ttu-id="bafa5-116">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="bafa5-116">Resource group</span></span>  |   <span data-ttu-id="bafa5-117">**<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**</span><span class="sxs-lookup"><span data-stu-id="bafa5-117">**<rgn>[sandbox resource group name]</rgn>**</span></span> |
| <span data-ttu-id="bafa5-118">Plats</span><span class="sxs-lookup"><span data-stu-id="bafa5-118">Location</span></span>        | <span data-ttu-id="bafa5-119">Se nedan.</span><span class="sxs-lookup"><span data-stu-id="bafa5-119">See below.</span></span> |

1. <span data-ttu-id="bafa5-120">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du har aktiverat sandbox-miljön med.</span><span class="sxs-lookup"><span data-stu-id="bafa5-120">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="bafa5-121">Välj **Skapa en resurs** i sidofältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="bafa5-121">Select **Create a resource** in the sidebar menu on the left.</span></span>

1. <span data-ttu-id="bafa5-122">_Windows Server 2016 VM_ ska finnas i listan över **Populära** Marketplace-element.</span><span class="sxs-lookup"><span data-stu-id="bafa5-122">_Windows Server 2016 VM_ should be in the list of **Popular** Marketplace elements.</span></span> <span data-ttu-id="bafa5-123">Om inte, försök att söka efter ”Windows Server 2016 DataCenter” med hjälp av sökrutan längst upp.</span><span class="sxs-lookup"><span data-stu-id="bafa5-123">If not, try searching for "Windows Server 2016 DataCenter" using the search box on the top.</span></span>

1. <span data-ttu-id="bafa5-124">Välj den virtuella Windows-datorn och klicka på **Skapa** för att börja skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bafa5-124">Select the Windows VM and click **Create** to start the VM creation process.</span></span>

1. <span data-ttu-id="bafa5-125">Kontrollera att vald **Prenumeration** i panelen **Grunder** är _Concierge-prenumeration_.</span><span class="sxs-lookup"><span data-stu-id="bafa5-125">In the **Basics** panel, verify the selected **Subscription** is _Concierge Subscription_.</span></span>

1. <span data-ttu-id="bafa5-126">Under **Resursgrupp**, välj **Använd befintlig** och välj _<rgn>[sandbox-resursgruppnamn]</rgn>_.</span><span class="sxs-lookup"><span data-stu-id="bafa5-126">Under **Resource Group**, select **Use Existing** and choose _<rgn>[sandbox resource group name]</rgn>_.</span></span>

1. <span data-ttu-id="bafa5-127">Skriv _fotoshareVM_ rutan **Namn på virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="bafa5-127">In the **Virtual machine name** box, enter _fotoshareVM_.</span></span>

1. <span data-ttu-id="bafa5-128">Välj regionen som ligger närmast dig i listrutan **Plats** från följande lista.</span><span class="sxs-lookup"><span data-stu-id="bafa5-128">In the **Location** drop-down list, select the closest region to you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="bafa5-129">För den virtuella datorns **Storlek** är standardvärdet **DS1 v2**, som ger dig en enskild processor och 3,5 GB minne.</span><span class="sxs-lookup"><span data-stu-id="bafa5-129">For the VM **Size**, the default is **DS1 v2** which gives you a single CPU and 3.5 GB of memory.</span></span> <span data-ttu-id="bafa5-130">Det räcker för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="bafa5-130">That's fine for this example.</span></span>

1. <span data-ttu-id="bafa5-131">I avsnittet **ADMINISTRATÖRSKONT** ska du ange ett **Användarnamn** och ett **Lösenord**/**Bekräfta lösenord** för ett administratörskonto på den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bafa5-131">In **ADMINISTRATOR ACCOUNT** section, enter a **Username** and **Password**/**Confirm password** for an administrator account on the new VM.</span></span>

1. <span data-ttu-id="bafa5-132">Följande bild är ett exempel på hur den **Grundläggande** konfigurationen ser ut när den har fyllts i. Lämna standardinställningarna för återstående flikar och fält och klicka på **Granska + skapa**.</span><span class="sxs-lookup"><span data-stu-id="bafa5-132">The following image is an example of what the **Basics** configuration looks like when filled out. Leave the defaults for the remaining tabs and fields and click **Review + create**.</span></span>

    ![Skärmbild av Azure Portal som visar bladet Skapa en virtuell dator med vissa grundläggande konfigurationer ifyllda enligt beskrivningen.](../media/4-basics-vm.png)

1. <span data-ttu-id="bafa5-134">När du har granskat dina nya inställningar för den virtuella datorn klickar du på **Skapa** för att börja distribuera den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bafa5-134">After reviewing your new VM settings, click **Create** to start the deploying your new VM.</span></span>

<span data-ttu-id="bafa5-135">Det tar några minuter att skapa en virtuell dator eftersom man skapar alla olika resurser (lagring, nätverksgränssnitt, etc.) för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bafa5-135">VM creation can take a few minutes as it creates all the various resources (storage, network interface, etc.) to support the virtual machine.</span></span> <span data-ttu-id="bafa5-136">Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="bafa5-136">Wait until the VM has deployed before continuing with the exercise.</span></span>

## <a name="view-os-disk-cache-status-in-the-portal"></a><span data-ttu-id="bafa5-137">Visa status för OS-diskcache i portalen</span><span class="sxs-lookup"><span data-stu-id="bafa5-137">View OS disk cache status in the portal</span></span>

<span data-ttu-id="bafa5-138">När vår virtuella dator har distribuerats kan vi bekräfta cachelagringsstatus för OS-disken med följande steg:</span><span class="sxs-lookup"><span data-stu-id="bafa5-138">Once our VM is deployed, we can confirm the caching status of the OS disk using the following steps:</span></span>

1. <span data-ttu-id="bafa5-139">Välj **fotoshareVM**-resursen för att öppna information om den virtuella datorn i portalen.</span><span class="sxs-lookup"><span data-stu-id="bafa5-139">Select the **fotoshareVM** resource to open the VM details in the portal.</span></span> <span data-ttu-id="bafa5-140">Du kan också klicka på **Alla resurser** i det vänstra sidofältet och sedan välja din virtuella dator, **fotoshareVM**.</span><span class="sxs-lookup"><span data-stu-id="bafa5-140">Alternatively, you can click **All resources** in the left sidebar, and then select your VM, **fotoshareVM**.</span></span>

1. <span data-ttu-id="bafa5-141">Välj **Diskar** under **Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="bafa5-141">Under **Settings**, select **Disks**.</span></span>

1. <span data-ttu-id="bafa5-142">I panelen **Diskar** har den virtuella datorn en disk, OS-disken.</span><span class="sxs-lookup"><span data-stu-id="bafa5-142">On the **Disks** pane, the VM has one disk, the OS disk.</span></span> <span data-ttu-id="bafa5-143">Dess cachetyp är för närvarande inställd på standardvärdet **Läsa/skriva**.</span><span class="sxs-lookup"><span data-stu-id="bafa5-143">Its cache type is currently set to the default value of **Read/write**.</span></span>

![Skärmbild av Azure Portal som visar avsnittet Diskar på ett blad för en virtuell dator, där OS-disken visas och har ställts in på skrivskyddad cachelagring.](../media/4-os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a><span data-ttu-id="bafa5-145">Ändra inställningarna för cachelagring för OS-disken i portalen</span><span class="sxs-lookup"><span data-stu-id="bafa5-145">Change the cache settings of the OS disk in the portal</span></span>

1. <span data-ttu-id="bafa5-146">I panelen **Diskar** väljer du **Redigera** i det övre vänstra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="bafa5-146">On the **Disks** pane, select **Edit** in the upper left of the screen.</span></span>

1. <span data-ttu-id="bafa5-147">Ändra värdet **HOST CACHING** (Cachelagring för värd) för OS-disken till **Read-only** (Skrivskyddad) med hjälp av listrutan och välj sedan **Spara** i det övre vänstra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="bafa5-147">Change the **HOST CACHING** value for the OS disk to **Read-only** using the drop-down list, and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="bafa5-148">Uppdateringen kan ta lite tid.</span><span class="sxs-lookup"><span data-stu-id="bafa5-148">This update can take some time.</span></span> <span data-ttu-id="bafa5-149">Anledningen är att när du ändrar cacheinställningen för en Azure-disk så frånkopplas och återansluts måldisken.</span><span class="sxs-lookup"><span data-stu-id="bafa5-149">The reason is that changing the cache setting of an Azure disk detaches and reattaches the target disk.</span></span> <span data-ttu-id="bafa5-150">Om det är operativsystemdisken startas även den virtuella datorn om.</span><span class="sxs-lookup"><span data-stu-id="bafa5-150">If it's the operating system disk, the VM is also restarted.</span></span> <span data-ttu-id="bafa5-151">När åtgärden har slutförts får du ett meddelande om att den virtuella datorns diskar har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="bafa5-151">When the operation completes, you'll get a notification saying the VM disks have been updated.</span></span>

1. <span data-ttu-id="bafa5-152">När det är klart ställs cachetypen för OS-disken in på **Read-only** (Skrivskyddad).</span><span class="sxs-lookup"><span data-stu-id="bafa5-152">Once complete, the OS disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="bafa5-153">Vi går vidare till konfigurationen av cachelagring för datadisk.</span><span class="sxs-lookup"><span data-stu-id="bafa5-153">Let's move on to data disk cache configuration.</span></span> <span data-ttu-id="bafa5-154">För att konfigurera en disk behöver vi först skapa en.</span><span class="sxs-lookup"><span data-stu-id="bafa5-154">To configure a disk, we'll need first to create one.</span></span>

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a><span data-ttu-id="bafa5-155">Lägga till en datadisk till den virtuella datorn och ange cachelagringstyp</span><span class="sxs-lookup"><span data-stu-id="bafa5-155">Add a data disk to the VM and set caching type</span></span>

1. <span data-ttu-id="bafa5-156">När du är tillbaka i vyn **Diskar** för den virtuella datorn i portalen ska du klicka på **Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="bafa5-156">Back on the **Disks** view of our VM in the portal, go ahead and click **Add data disk**.</span></span> <span data-ttu-id="bafa5-157">Ett fel visas omedelbart i fältet **Namn**, vilket anger att fältet inte får vara tomt.</span><span class="sxs-lookup"><span data-stu-id="bafa5-157">An error immediately appears in the **Name** field, telling us that the field can't be empty.</span></span> <span data-ttu-id="bafa5-158">Vi har inte någon datadisk ännu, så vi tar och skapar en.</span><span class="sxs-lookup"><span data-stu-id="bafa5-158">We don't have a data disk yet, so let's create one.</span></span>

1. <span data-ttu-id="bafa5-159">Klicka på listan **Namn** och klicka sedan på **Skapa disk**.</span><span class="sxs-lookup"><span data-stu-id="bafa5-159">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="bafa5-160">I fönsterrutan **Skapa hanterad disk** går du till rutan **Namn** och skriver **fotosharesVM-data**.</span><span class="sxs-lookup"><span data-stu-id="bafa5-160">In the **Create managed disk** pane, in the **Name** box, type **fotosharesVM-data**.</span></span>

1. <span data-ttu-id="bafa5-161">Under **Resursgrupp** väljer du **Använd befintlig** och väljer _<rgn>[Sandbox-resursgruppnamn]</rgn>_.</span><span class="sxs-lookup"><span data-stu-id="bafa5-161">Under **Resource Group**, select **Use existing**, and select _<rgn>[sandbox resource group name]</rgn>_.</span></span>

1. <span data-ttu-id="bafa5-162">Observera standardinställningarna för återstående fält:</span><span class="sxs-lookup"><span data-stu-id="bafa5-162">Note the defaults for the remaining fields:</span></span>
    - <span data-ttu-id="bafa5-163">Premium SSD</span><span class="sxs-lookup"><span data-stu-id="bafa5-163">Premium SSD</span></span>
    - <span data-ttu-id="bafa5-164">1 023 GB i storlek</span><span class="sxs-lookup"><span data-stu-id="bafa5-164">1023 GB in size</span></span>
    - <span data-ttu-id="bafa5-165">På samma plats som den virtuella datorn (kan inte ändras).</span><span class="sxs-lookup"><span data-stu-id="bafa5-165">In the same location as the VM (not changeable).</span></span>
    - <span data-ttu-id="bafa5-166">IOPS-gräns –5 000</span><span class="sxs-lookup"><span data-stu-id="bafa5-166">IOPS limit - 5000</span></span>
    - <span data-ttu-id="bafa5-167">Dataflödesgräns (MB/s) – 200</span><span class="sxs-lookup"><span data-stu-id="bafa5-167">Throughput limit (MB/s) - 200</span></span>

1. <span data-ttu-id="bafa5-168">Välj **Skapa** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="bafa5-168">Click **Create** at the bottom of the screen.</span></span>

    <span data-ttu-id="bafa5-169">Vänta tills disken har skapats innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="bafa5-169">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="bafa5-170">Ändra värdet **HOST CACHING** (Cachelagring för värd) för den nya datadisken till **Read-only** (Skrivskyddad) med hjälp av listrutan (det kan redan ha angetts) och klicka sedan på **Spara** i det övre vänstra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="bafa5-170">Change the **HOST CACHING** value for our new data disk to **Read-only** using the drop-down list (it might be set already), and then click **Save** in the upper left of the screen.</span></span>

    <span data-ttu-id="bafa5-171">Vänta tills den virtuella datorn är klar med uppdateringen av den nya datadisken.</span><span class="sxs-lookup"><span data-stu-id="bafa5-171">Wait for the VM to finish updating the new data disk.</span></span> <span data-ttu-id="bafa5-172">När den är klar har du en ny datadisk på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bafa5-172">Once complete, you will have a new data disk on your virtual machine.</span></span>

<span data-ttu-id="bafa5-173">I den här övningen använde vi Azure Portal för att konfigurera cachelagring på en ny virtuell dator, ändra inställningarna för cachelagring på en befintlig disk och konfigurera cachelagring på en ny datadisk.</span><span class="sxs-lookup"><span data-stu-id="bafa5-173">In this exercise, we used the Azure portal to configure caching on a new VM, change cache settings on an existing disk, and configure caching on a new data disk.</span></span> <span data-ttu-id="bafa5-174">Följande skärmbild visar den slutgiltiga konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="bafa5-174">The following screenshot shows the final configuration:</span></span>

![Skärmbild av Azure Portal som visar OS-disken och den nya datadisken i avsnittet Diskar på bladet för den virtuella datorn, där båda diskarna har ställts in på skrivskyddad cachelagring.](../media/disks-final-config-portal.PNG)
