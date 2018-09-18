<span data-ttu-id="3283f-101">Anta att du har en webbplats för fotodelning med data som lagras på virtuella Azure-datorer (VM) som kör SQL Server och anpassade program.</span><span class="sxs-lookup"><span data-stu-id="3283f-101">Suppose you run a photo sharing site, with data stored on Azure virtual machines (VMs) running SQL Server and custom applications.</span></span> <span data-ttu-id="3283f-102">Du vill göra följande justeringar.</span><span class="sxs-lookup"><span data-stu-id="3283f-102">You want to make the following adjustments.</span></span>

- <span data-ttu-id="3283f-103">Du behöver ändra inställningarna för diskcache på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3283f-103">You need change the disk cache settings on a VM</span></span>
- <span data-ttu-id="3283f-104">Du vill lägga till en ny datadisk till den virtuella datorn med cachelagring aktiverat</span><span class="sxs-lookup"><span data-stu-id="3283f-104">You want dd a new data disk to the VM with caching enabled</span></span>

<span data-ttu-id="3283f-105">Du har valt att göra dessa ändringar via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3283f-105">You've decided to make these changes through the Azure portal.</span></span>

<span data-ttu-id="3283f-106">I den här övningen går vi igenom hur du genomför de ändringar av en virtuell dator som beskrevs ovan.</span><span class="sxs-lookup"><span data-stu-id="3283f-106">In this exercise, we'll walk through making the changes to a VM that we described above.</span></span> <span data-ttu-id="3283f-107">Först loggar vi in på portalen och skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3283f-107">First, let's sign in to the portal and create a VM.</span></span>

## <a name="sign-in-to-the-azure-portal"></a><span data-ttu-id="3283f-108">Logga in på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3283f-108">Sign in to the Azure portal</span></span>
<!---TODO: Update for sandbox?--->

1. <span data-ttu-id="3283f-109">Logga in på [Azure-portalen](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="3283f-109">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

## <a name="create-a-virtual-machine"></a><span data-ttu-id="3283f-110">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3283f-110">Create a virtual machine</span></span>

<span data-ttu-id="3283f-111">I det här steget skapar vi en virtuell dator med följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3283f-111">In this step, we're going to create a VM with the following properties.</span></span>

|<span data-ttu-id="3283f-112">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3283f-112">Property</span></span>  |<span data-ttu-id="3283f-113">Värde</span><span class="sxs-lookup"><span data-stu-id="3283f-113">Value</span></span>  |
|---------|---------|
|<span data-ttu-id="3283f-114">Avbildning</span><span class="sxs-lookup"><span data-stu-id="3283f-114">Image</span></span>     |   <span data-ttu-id="3283f-115">**Windows Server 2016 Datacenter**</span><span class="sxs-lookup"><span data-stu-id="3283f-115">**Windows Server 2016 Datacenter**</span></span>      |
|<span data-ttu-id="3283f-116">Namn</span><span class="sxs-lookup"><span data-stu-id="3283f-116">Name</span></span>     |   <span data-ttu-id="3283f-117">**fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="3283f-117">**fotoshareVM**</span></span>     |
|<span data-ttu-id="3283f-118">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="3283f-118">Resource group</span></span>     |   <span data-ttu-id="3283f-119">**fotoshare-rg**</span><span class="sxs-lookup"><span data-stu-id="3283f-119">**fotoshare-rg**</span></span>      |


1. <span data-ttu-id="3283f-120">I menyn till vänster på portalen väljer du **Virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="3283f-120">In the left menu of the portal, select **Virtual machines**</span></span>

1. <span data-ttu-id="3283f-121">Välj nu **+ Lägg till** uppe till vänster på skärmen **Virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="3283f-121">Now select **+ Add** in the top left of the **Virtual machines** screen.</span></span> <span data-ttu-id="3283f-122">Den här åtgärden startar skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="3283f-122">This action starts the creation process.</span></span>

1. <span data-ttu-id="3283f-123">På panelen Compute (Beräkning) som visar en lista över tillgängliga VM-avbildningar skriver du *Windows Server 2016 Datacenter* i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="3283f-123">On the Compute panel that lists available VM images, type *Windows Server 2016 Datacenter* into the search box.</span></span>

1. <span data-ttu-id="3283f-124">Välj **Windows Server 2016 Datacenter** bland sökresultatet och välj sedan **Skapa** för att starta VM-skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="3283f-124">Select **Windows Server 2016 Datacenter** from the search results and then select **Create** to start the VM creation process.</span></span>

1. <span data-ttu-id="3283f-125">På panelen **Basics** (Grundläggande) går du till rutan **Namn** och anger **fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="3283f-125">In the **Basics** panel, in the **Name** box, enter **fotoshareVM**</span></span>

1. <span data-ttu-id="3283f-126">I rutorna **Användarnamn** och **Lösenord** anger du ett namn och lösenord för ett administratörskonto på den här servern.</span><span class="sxs-lookup"><span data-stu-id="3283f-126">In the **Username** and **Password boxes**, enter a name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="3283f-127">I listrutan **Prenumeration** väljer du din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3283f-127">In the **Subscription** dropdown, select your Azure subscription.</span></span>

1. <span data-ttu-id="3283f-128">Under **Resursgrupp** väljer du **Skapa ny** och skriver in **fotoshare-rg**.</span><span class="sxs-lookup"><span data-stu-id="3283f-128">Under **Resource Group**, select **Create new**, and in the box, type **fotoshare-rg**.</span></span>

1. <span data-ttu-id="3283f-129">Välj en region nära dig i listrutan **Plats**.</span><span class="sxs-lookup"><span data-stu-id="3283f-129">In the **Location** drop-down list, select a region near you.</span></span>

<span data-ttu-id="3283f-130">Följande är ett exempel på hur den **Grundläggande** konfigurationen ser ut när den har fyllts i.</span><span class="sxs-lookup"><span data-stu-id="3283f-130">The following is an example of what the **Basics** configuration looks like when filled out.</span></span>

![Skärmbild på Grundläggande för en virtuell dator som har fyllts i.](../media-draft/vm-basics-settings.PNG)

1. <span data-ttu-id="3283f-132">Välj **OK** för att gå vidare till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="3283f-132">Select **OK** to move onto the next step.</span></span>

<span data-ttu-id="3283f-133">Nu behöver du välja en storlek för den virtuella datorn och sedan starta distributionen:</span><span class="sxs-lookup"><span data-stu-id="3283f-133">You now need to choose a size for the VM, and then start the deployment:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3283f-134">Kom ihåg att diskcachelagring inte kan ändras för virtuella datorer i L-serien och B-serien.</span><span class="sxs-lookup"><span data-stu-id="3283f-134">Remember that disk caching can't be changed for L-Series and B-series virtual machines.</span></span> <span data-ttu-id="3283f-135">Vi väljer en annan storlek.</span><span class="sxs-lookup"><span data-stu-id="3283f-135">We'll select a different size.</span></span>

1. <span data-ttu-id="3283f-136">I avsnittet **Välj storlek** väljer du **Standard** SKU, till exempel **F1s** och väljer sedan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="3283f-136">In the **Choose a size** section, select a **Standard** SKU, such as **F1s**, and then choose **Select**.</span></span>

1. <span data-ttu-id="3283f-137">Rulla nedåt i fönsterrutan **Inställningar** och notera att det inte finns något alternativ för att konfigurera diskcachelagring.</span><span class="sxs-lookup"><span data-stu-id="3283f-137">Scroll down the **Settings** pane and observe that there is no option to configure disk caching.</span></span> <span data-ttu-id="3283f-138">Vi accepterar standardinställningarna på det här steget och väljer **OK** längst ned i fönsterrutan.</span><span class="sxs-lookup"><span data-stu-id="3283f-138">Let's accept the defaults on this step and choose **OK** at the bottom of the pane.</span></span>

1. <span data-ttu-id="3283f-139">På panelen **Skapa** granskar du sammanfattningen och väljer sedan **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3283f-139">On the **Create** panel, review the summary, and then choose **Create**.</span></span>

1. <span data-ttu-id="3283f-140">Det kan ta en stund att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3283f-140">VM creation can take a while.</span></span> <span data-ttu-id="3283f-141">Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="3283f-141">Wait until the VM has deployed before continuing with the exercise.</span></span> <span data-ttu-id="3283f-142">Du får ett meddelande i meddelandehubben när processen är klar.</span><span class="sxs-lookup"><span data-stu-id="3283f-142">You'll get a message in the Notification hub when the process is complete.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3283f-143">Vi använder den här virtuella datorn i nästa kurs, så behåll den ett tag.</span><span class="sxs-lookup"><span data-stu-id="3283f-143">We'll use this VM in the next lesson, so keep it around for a while!</span></span>

## <a name="view-os-disk-cache-status-in-the-portal"></a><span data-ttu-id="3283f-144">Visa status för OS-diskcache i portalen</span><span class="sxs-lookup"><span data-stu-id="3283f-144">View OS disk cache status in the portal</span></span>

<span data-ttu-id="3283f-145">När vår virtuella dator har distribuerats kan vi bekräfta cachelagringsstatus för OS-disken med följande steg.</span><span class="sxs-lookup"><span data-stu-id="3283f-145">Once our VM is deployed, we can confirm the caching status of the OS disk using the following steps.</span></span>

1. <span data-ttu-id="3283f-146">I den vänstra menyn klickar du på **Alla resurser** och väljer sedan din virtuella dator **fotoshareVM**.</span><span class="sxs-lookup"><span data-stu-id="3283f-146">In the left menu, click **All resources**, and then select your VM,  **fotoshareVM**.</span></span>

1. <span data-ttu-id="3283f-147">På skärmen **Virtuella datorer** går du till **INSTÄLLNINGAR** och väljer **Diskar**.</span><span class="sxs-lookup"><span data-stu-id="3283f-147">On the **Virtual machine** screen, under **SETTINGS**, select **Disks**.</span></span>

1. <span data-ttu-id="3283f-148">På fönsterrutan **Diskar** har den virtuella datorn en disk, OS-disken.</span><span class="sxs-lookup"><span data-stu-id="3283f-148">On the **Disks** pane, the VM has one disk, the OS disk.</span></span> <span data-ttu-id="3283f-149">Dess cachetyp är för närvarande inställd på standardvärdet **Läsa/skriva**.</span><span class="sxs-lookup"><span data-stu-id="3283f-149">Its cache type is currently set to the default value of **Read/write**.</span></span>

![Skärmbild på våra OS- och datadiskar som båda är inställda på skrivskyddad cachelagring.](../media-draft/os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a><span data-ttu-id="3283f-151">Ändra inställningarna för cachelagring för OS-disken i portalen</span><span class="sxs-lookup"><span data-stu-id="3283f-151">Change the cache settings of the OS disk in the portal</span></span>

1. <span data-ttu-id="3283f-152">På fönsterrutan **Diskar** väljer du **Redigera** i det övre vänstra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="3283f-152">On the **Disks** pane, select **Edit** in the upper left of the screen.</span></span>

1. <span data-ttu-id="3283f-153">Ändra värdet **HOST CACHING** (Cachelagring för värd) för OS-disken till **Read-only** (Skrivskyddad) med hjälp av listrutan, och välj sedan **Spara** i det övre vänstra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="3283f-153">Change the **HOST CACHING** value for the OS disk to **Read-only** using the dropdown and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="3283f-154">Den här uppdateringen tar en stund.</span><span class="sxs-lookup"><span data-stu-id="3283f-154">This update takes a while.</span></span> <span data-ttu-id="3283f-155">Anledningen är att när du ändrar cacheinställningen för en Azure-disk så frånkopplas och återansluts måldisken.</span><span class="sxs-lookup"><span data-stu-id="3283f-155">The reason is that changing the cache setting of an Azure disk detaches and reattaches the target disk.</span></span> <span data-ttu-id="3283f-156">Om det är operativsystemdisken startas även den virtuella datorn om.</span><span class="sxs-lookup"><span data-stu-id="3283f-156">If it's the operating system disk, the VM is also restarted.</span></span> <span data-ttu-id="3283f-157">Du får ett meddelande som liknar följande meddelande när uppdateringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3283f-157">You'll get a message similar to the following notification when the update has finished.</span></span>

![Exempel på meddelande som du får när uppdateringen av cacheinställningen har slutförts.](../media-draft/vm-disk-update-complete.PNG)

4. <span data-ttu-id="3283f-159">När det är klart ställs cachetypen för OS-disken in på **Read-only** (Skrivskyddad).</span><span class="sxs-lookup"><span data-stu-id="3283f-159">Once complete, the OS disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="3283f-160">Vi går vidare till konfigurationen av cachelagring för datadisk.</span><span class="sxs-lookup"><span data-stu-id="3283f-160">Let's move on to data disk cache configuration.</span></span> <span data-ttu-id="3283f-161">För att konfigurera en disk behöver vi först skapa en.</span><span class="sxs-lookup"><span data-stu-id="3283f-161">To configure a disk, we'll need to first create one.</span></span>

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a><span data-ttu-id="3283f-162">Lägga till en datadisk till den virtuella datorn och ange cachelagringstyp</span><span class="sxs-lookup"><span data-stu-id="3283f-162">Add a data disk to the VM and set caching type</span></span>

1. <span data-ttu-id="3283f-163">När du är tillbaka i vyn **Diskar** för den virtuella datorn i portalen väljer du **Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="3283f-163">Back on the **Disks** view of our VM in the portal, go ahead and select **Add data disk**.</span></span> <span data-ttu-id="3283f-164">Ett fel visas omedelbart i fältet **Namn**, vilket anger att fältet inte får vara tomt.</span><span class="sxs-lookup"><span data-stu-id="3283f-164">An error immediately appears in the **Name** field telling us that the field can't be empty.</span></span> <span data-ttu-id="3283f-165">Vi har inte någon datadisk ännu, så vi tar och skapar en.</span><span class="sxs-lookup"><span data-stu-id="3283f-165">We don't have a data disk yet, so let's create one.</span></span>

1. <span data-ttu-id="3283f-166">Klicka på listan **Namn** och klicka sedan på **Skapa disk**.</span><span class="sxs-lookup"><span data-stu-id="3283f-166">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="3283f-167">I fönsterrutan **Skapa hanterad disk** går du till rutan **Namn** och skriver **fotosharesVM-data**.</span><span class="sxs-lookup"><span data-stu-id="3283f-167">In the **Create managed disk** pane, in the **Name** box, type **fotosharesVM-data**.</span></span>

1. <span data-ttu-id="3283f-168">Under **Resursgrupp** väljer du **Använd befintlig** och väljer **fotoshare-rg** från listrutan.</span><span class="sxs-lookup"><span data-stu-id="3283f-168">Under **Resource Group**, select **Use existing**, and select **fotoshare-rg** from the dropdown menu.</span></span>

1. <span data-ttu-id="3283f-169">Välj **Skapa** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="3283f-169">Select **Create** at the bottom of the screen.</span></span>

1. <span data-ttu-id="3283f-170">Vänta tills disken har skapats innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="3283f-170">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="3283f-171">Ändra värdet **HOST CACHING** (Cachelagring för värd) för den nya datadisken till **Read-only** (Skrivskyddad) med hjälp av listrutan, och välj sedan **Spara** i det övre vänstra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="3283f-171">Change the **HOST CACHING** value for our new data disk to **Read-only** using the dropdown and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="3283f-172">Vänta tills den virtuella datorn har uppdaterat.</span><span class="sxs-lookup"><span data-stu-id="3283f-172">Wait for the VM to update.</span></span> <span data-ttu-id="3283f-173">Uppdateringen tar en stund eftersom Azure frånkopplar och återansluter datadisken för att ändra den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="3283f-173">Updating takes a while because Azure detaches and reattaches the data disk to change this setting.</span></span>

1. <span data-ttu-id="3283f-174">När det är klart ställs cachetypen för datadisken in på **Read-only** (Skrivskyddad).</span><span class="sxs-lookup"><span data-stu-id="3283f-174">Once complete, the data disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="3283f-175">I den här övningen använde vi Azure-portalen för att konfigurera cachelagring på en ny virtuell dator, ändra inställningarna för cachelagring på en befintlig disk och konfigurera cachelagring på en ny datadisk.</span><span class="sxs-lookup"><span data-stu-id="3283f-175">In this exercise, we used the Azure portal to configure caching on a new VM, change cache settings on an existing disk, and to configure caching on a new data disk.</span></span> <span data-ttu-id="3283f-176">Följande skärmbild visar den slutgiltiga konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="3283f-176">The following screenshot shows the final configuration.</span></span> 

![Skärmbild på våra OS- och datadiskar som båda är inställda på skrivskyddad cachelagring.](../media-draft/disks-final-config-portal.PNG)