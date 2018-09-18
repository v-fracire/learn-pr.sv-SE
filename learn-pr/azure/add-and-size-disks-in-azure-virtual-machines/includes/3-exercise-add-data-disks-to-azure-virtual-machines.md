<span data-ttu-id="a67b2-101">I den här övningen antar vi att ditt företag kör en SMTP-e-postserver (Simple Mail Transfer Protocol).</span><span class="sxs-lookup"><span data-stu-id="a67b2-101">In this exercise, let's assume your company runs a Simple Mail Transfer Protocol (SMTP) email server.</span></span> <span data-ttu-id="a67b2-102">Du vill migrera den här servern till Azure.</span><span class="sxs-lookup"><span data-stu-id="a67b2-102">You want to migrate this server into Azure.</span></span> <span data-ttu-id="a67b2-103">Du vill att SMTP-servern ska lagra inkommande meddelanden för din egen domän i en mapp med namnet ”Drop” på en dedikerad virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="a67b2-103">You want the SMTP server to store incoming messages for your own domain in a folder called "Drop" on a dedicated VHD.</span></span>

<span data-ttu-id="a67b2-104">Målet med den här övningen är att skapa en virtuell Windows-dator (VM) och ansluta en ny virtuell hårddisk (VHD) med namnet ”Incoming” (Inkommande) för att lagra katalogen ”Drop”.</span><span class="sxs-lookup"><span data-stu-id="a67b2-104">The goal of the exercise is to create a Windows virtual machine (VM) and attach a new virtual hard disk (VHD) called "Incoming" to store the "Drop" directory.</span></span>

## <a name="sign-in-to-azure"></a><span data-ttu-id="a67b2-105">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="a67b2-105">Sign in to Azure</span></span>
<!---TODO: Need update for sanbox?--->

1. <span data-ttu-id="a67b2-106">Logga in på [Azure-portalen](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="a67b2-106">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

## <a name="create-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="a67b2-107">Skapa en virtuell Windows-dator i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a67b2-107">Create a Windows VM in the Azure portal</span></span>

<span data-ttu-id="a67b2-108">Skapa en virtuell dator som värd för SMTP-servern med dess dataenheter genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="a67b2-108">To create a VM to host the SMTP server with its data drives, follow these steps:</span></span>

1. <span data-ttu-id="a67b2-109">Välj **Skapa en resurs** längst upp till vänster i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-109">Choose **Create a resource** in the upper left corner of the Azure portal.</span></span>

1. <span data-ttu-id="a67b2-110">I sökrutan ovanför listan över resurser i Azure Marketplace söker du efter och väljer **Windows Server 2016 Datacenter** och därefter **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-110">In the search box above the list of Azure Marketplace resources, search for and select **Windows Server 2016 Datacenter**, and then choose **Create**.</span></span>

1. <span data-ttu-id="a67b2-111">I fönsterrutan **Basics** (Grundläggande) som öppnas till höger anger du följande egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="a67b2-111">In the **Basics** pane that opens to the right, enter the following property values.</span></span> 


|<span data-ttu-id="a67b2-112">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a67b2-112">Property</span></span>  |<span data-ttu-id="a67b2-113">Värde</span><span class="sxs-lookup"><span data-stu-id="a67b2-113">Value</span></span>  |<span data-ttu-id="a67b2-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a67b2-114">Notes</span></span>  |
|---------|---------|---------|
|<span data-ttu-id="a67b2-115">Namn</span><span class="sxs-lookup"><span data-stu-id="a67b2-115">Name</span></span>     |   <span data-ttu-id="a67b2-116">**MailSenderVM**</span><span class="sxs-lookup"><span data-stu-id="a67b2-116">**MailSenderVM**</span></span>      |         |
|<span data-ttu-id="a67b2-117">Typ av virtuell datordisk</span><span class="sxs-lookup"><span data-stu-id="a67b2-117">VM disk type</span></span>     |  <span data-ttu-id="a67b2-118">**Standard HDD**</span><span class="sxs-lookup"><span data-stu-id="a67b2-118">**Standard HDD**</span></span>       |   <span data-ttu-id="a67b2-119">Välj det här värdet i listrutan.</span><span class="sxs-lookup"><span data-stu-id="a67b2-119">Select this value from the dropdown.</span></span>      |
|<span data-ttu-id="a67b2-120">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="a67b2-120">User name</span></span>     |  <span data-ttu-id="a67b2-121">**mailmaster**</span><span class="sxs-lookup"><span data-stu-id="a67b2-121">**mailmaster**</span></span>       |         |
|<span data-ttu-id="a67b2-122">Lösenord</span><span class="sxs-lookup"><span data-stu-id="a67b2-122">Password</span></span>     |  <span data-ttu-id="a67b2-123">Lösenordet måste vara minst 12 tecken långt och uppfylla [de definierade kraven på komplexitet](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="a67b2-123">The password must be at least 12 characters long and meet the [defined complexity requirements](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).</span></span>       | <span data-ttu-id="a67b2-124">Se till att komma ihåg det här användarnamnet och lösenordet eftersom vi kommer använda dem i hela modulen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-124">Make sure to remember this user name and password because we'll use them throughout the module.</span></span>         |
|<span data-ttu-id="a67b2-125">Prenumeration</span><span class="sxs-lookup"><span data-stu-id="a67b2-125">Subscription</span></span>     |  <span data-ttu-id="a67b2-126">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a67b2-126">Choose your subscription.</span></span>       |  <span data-ttu-id="a67b2-127">Välj det här värdet i listrutan.</span><span class="sxs-lookup"><span data-stu-id="a67b2-127">Select this value from the dropdown.</span></span>       |
|<span data-ttu-id="a67b2-128">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a67b2-128">Resource group</span></span>     |  <span data-ttu-id="a67b2-129">Välj **Skapa nytt** och skriv sedan **MailInfrastructure**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-129">Select **Create new**, and then type **MailInfrastructure**.</span></span>       |  <span data-ttu-id="a67b2-130">Vi samlar in alla resurser som används i den här modulen till en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a67b2-130">We'll gather all resource used in this module into one resource group.</span></span>       |
|<span data-ttu-id="a67b2-131">Plats</span><span class="sxs-lookup"><span data-stu-id="a67b2-131">Location</span></span>     |   <span data-ttu-id="a67b2-132">En plats nära dig.</span><span class="sxs-lookup"><span data-stu-id="a67b2-132">A location near you.</span></span>      | <span data-ttu-id="a67b2-133">Välj det här värdet i listrutan.</span><span class="sxs-lookup"><span data-stu-id="a67b2-133">Select this value from the dropdown.</span></span>        |

4. <span data-ttu-id="a67b2-134">Välj **OK** längst ned på sidan **Basics** (Grundläggande) för att fortsätta till konfigurationsrutan **Storlek**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-134">Select **OK** at the bottom of the **Basics** page to continue to the **Size** configuration pane.</span></span>

1. <span data-ttu-id="a67b2-135">I konfigurationsrutan **Storlek** söker du efter och väljer **B1ms**, och sedan klickar du på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-135">In the **Size** configuration pane, search for and select **B1ms**, and then click **Select**.</span></span>

1. <span data-ttu-id="a67b2-136">I fönsterrutan **Inställningar** går du till **Använd hanterade diskar** och klickar på **Nej**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-136">In the **Settings** pane, under **Use managed disks**, click **No**.</span></span> <span data-ttu-id="a67b2-137">Vi går igenom hanterade diskar senare i den här modulen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-137">We'll discuss managed disks later in this module.</span></span>

1. <span data-ttu-id="a67b2-138">I listrutan **Välj offentliga inkommande portar**  väljer du **RDP (3389)**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-138">In the **Select public inbound ports** dropdown list, select **RDP (3389)**.</span></span> <span data-ttu-id="a67b2-139">Vi använder den här porten för att fjärransluta till den virtuella datorn när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="a67b2-139">We'll use this port to remote into our VM after it's created.</span></span>

1. <span data-ttu-id="a67b2-140">Lämna alla de andra inställningarna på deras standardvärden och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-140">Leave all the other settings at their default, and then click **OK**.</span></span>

1. <span data-ttu-id="a67b2-141">I fönsterrutan **Skapa** granskar du konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-141">In the **Create** pane, review the configuration.</span></span>

1. <span data-ttu-id="a67b2-142">När du har granskat konfigurationen väljer du **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-142">When you have reviewed the configuration,  select **Create**.</span></span> <span data-ttu-id="a67b2-143">Azure skapar och startar den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a67b2-143">Azure creates and starts the new VM.</span></span>

> [!TIP]
> <span data-ttu-id="a67b2-144">Det kan ta några minuter att skapa den virtuella datorn och distribuera den i Azure.</span><span class="sxs-lookup"><span data-stu-id="a67b2-144">Creating your VM and deploying it in Azure can take a few minutes.</span></span> <span data-ttu-id="a67b2-145">Du kan övervaka förloppet i hubben **Meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-145">You can watch the progress in the **Notifications** hub.</span></span> <span data-ttu-id="a67b2-146">Azure visar en dialogruta med ett meddelande när det är klart.</span><span class="sxs-lookup"><span data-stu-id="a67b2-146">Azure will display a notification dialog when it finishes.</span></span>

## <a name="add-an-empty-data-disk-to-our-vm"></a><span data-ttu-id="a67b2-147">Lägga till en tom datadisk till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="a67b2-147">Add an empty data disk to our VM</span></span>

<span data-ttu-id="a67b2-148">Vi kommer att ge den disk som lagrar katalogen ”Drop” för SMTP-servern namnet ”Incoming” (Inkommande).</span><span class="sxs-lookup"><span data-stu-id="a67b2-148">We're going to name the disk stores the "Drop" directory for your SMTP server "Incoming".</span></span> <span data-ttu-id="a67b2-149">Vi lägger till en ny tom disk till servern med följande steg:</span><span class="sxs-lookup"><span data-stu-id="a67b2-149">Let's add a new empty disk to the server using the following steps:</span></span>

1. <span data-ttu-id="a67b2-150">I navigeringsfönstret till vänster går du till **FAVORITER** och väljer **Virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-150">In the navigation on the left, under **FAVORITES**, select **Virtual machines**.</span></span>

1. <span data-ttu-id="a67b2-151">I listan över virtuella datorer väljer du **MailSenderVM**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-151">In the list of VMs, select **MailSenderVM**.</span></span>

1. <span data-ttu-id="a67b2-152">Under **INSTÄLLNINGAR** i konfigurationsmenyn **MailSenderVM** till vänster väljer du **Diskar**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-152">Under **SETTINGS** of the **MailSenderVM** configuration menu on the left, select **Disks**.</span></span>

1. <span data-ttu-id="a67b2-153">Under **Datadiskar** väljer du **Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-153">Under **Data disks**, select **Add data disk**.</span></span>

1. <span data-ttu-id="a67b2-154">I fönsterrutan **Attach unmanaged disks** (Anslut ohanterade diskar) anger du följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a67b2-154">In the **Attach unmanaged disks** pane, set the following properties.</span></span>


|<span data-ttu-id="a67b2-155">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a67b2-155">Property</span></span>  |<span data-ttu-id="a67b2-156">Värde</span><span class="sxs-lookup"><span data-stu-id="a67b2-156">Value</span></span>  |<span data-ttu-id="a67b2-157">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a67b2-157">Notes</span></span>  |
|---------|---------|---------|
|<span data-ttu-id="a67b2-158">Namn</span><span class="sxs-lookup"><span data-stu-id="a67b2-158">Name</span></span>     |   <span data-ttu-id="a67b2-159">**MailSenderVMIncoming**</span><span class="sxs-lookup"><span data-stu-id="a67b2-159">**MailSenderVMIncoming**</span></span>      |         |
|<span data-ttu-id="a67b2-160">Källtyp</span><span class="sxs-lookup"><span data-stu-id="a67b2-160">Source type</span></span>     |  <span data-ttu-id="a67b2-161">**Ny (tom disk)**</span><span class="sxs-lookup"><span data-stu-id="a67b2-161">**New (empty disk)**</span></span>       |   <span data-ttu-id="a67b2-162">Välj det här värdet i listrutan.</span><span class="sxs-lookup"><span data-stu-id="a67b2-162">Select this value from the dropdown.</span></span>       |
|<span data-ttu-id="a67b2-163">Kontotyp</span><span class="sxs-lookup"><span data-stu-id="a67b2-163">Account type</span></span>     |  <span data-ttu-id="a67b2-164">**Standard HDD**</span><span class="sxs-lookup"><span data-stu-id="a67b2-164">**Standard HDD**</span></span>       |  <span data-ttu-id="a67b2-165">Välj det här värdet i listrutan.</span><span class="sxs-lookup"><span data-stu-id="a67b2-165">Select this value from the dropdown.</span></span>        |


6. <span data-ttu-id="a67b2-166">Till vänster om fältet **Lagringscontainer** väljer du **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-166">To the left of the **Storage container** field, select **Browse**.</span></span>

1. <span data-ttu-id="a67b2-167">I listan över lagringskonton söker du efter det lagringskonto vars namn börjar med **mailinfrastructure** och väljer det.</span><span class="sxs-lookup"><span data-stu-id="a67b2-167">In the list of storage accounts, search for the storage account whose name begins with **mailinfrastructure** and select it.</span></span>

1. <span data-ttu-id="a67b2-168">I listan över containrar klickar du på **vhds** och sedan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-168">In the list of containers, click **vhds** and then choose **Select**.</span></span>

1. <span data-ttu-id="a67b2-169">När du är tillbaka på skärmen **Attach unmanaged disk** (Anslut ohanterad disk) väljer du **OK**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-169">Back on the **Attach unmanaged disk** screen, select **OK**.</span></span>

1. <span data-ttu-id="a67b2-170">När du är tillbaka på skärmen **MailSenderVM - Disks** (Diskar) väljer du **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-170">Back on the **MailSenderVM - Disks** screen, select **Save**.</span></span>

<span data-ttu-id="a67b2-171">Vi har nu definierat en disk med namnet **MainSenderVMIncoming**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-171">We've now defined a disk called **MainSenderVMIncoming**.</span></span> <span data-ttu-id="a67b2-172">För att använda disken behöver vi först partitionera och formatera den när vi loggar in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a67b2-172">To use the disk, we'll first need to partition and format it when we log into the VM.</span></span> 

## <a name="partition-and-format-a-data-disk"></a><span data-ttu-id="a67b2-173">Partitionera och formatera en datadisk</span><span class="sxs-lookup"><span data-stu-id="a67b2-173">Partition and format a data disk</span></span>

<span data-ttu-id="a67b2-174">På samma sätt som med fysiska diskar initierar och formaterar du en partition på en virtuell hårddisk innan den kan användas.</span><span class="sxs-lookup"><span data-stu-id="a67b2-174">As with physical disks, initiate and format a partition on a VHD before it can be used.</span></span>

### <a name="log-into-our-windows-vm-using-rdp"></a><span data-ttu-id="a67b2-175">Logga in på vår virtuella Windows-dator med hjälp av RDP</span><span class="sxs-lookup"><span data-stu-id="a67b2-175">Log into our Windows VM using RDP</span></span>

1. <span data-ttu-id="a67b2-176">På huvudskärmen för den virtuella datorn **MailSenderVM** väljer du **Översikt**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-176">In the **MailSenderVM** virtual machine main screen, select **Overview**.</span></span>

1. <span data-ttu-id="a67b2-177">Välj **Anslut** längst upp till vänster på översiktsskärmen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-177">Select **Connect** from the top left of the overview screen.</span></span>

1. <span data-ttu-id="a67b2-178">I dialogrutan **Ansluta till den virtuella datorn** som öppnas till höger väljer du **Download to RDP File** (Ladda ned till RDP-fil).</span><span class="sxs-lookup"><span data-stu-id="a67b2-178">In the **Connect to virtual machine** dialog that opens on the right, select **Download to RDP File**.</span></span>

   ![Skärmbild av dialogrutan ”Ansluta till den virtuella datorn” med knappen ”Download RDP file” (Ladda ned RDP-fil) markerad.](../media-draft/download-rdp.png)

4. <span data-ttu-id="a67b2-180">En fil som heter **MailSenderVM.rdp** laddas ned till din lokala `Downloads`-mapp.</span><span class="sxs-lookup"><span data-stu-id="a67b2-180">A file called **MailSenderVM.rdp** is downloaded to your local `Downloads` folder.</span></span> <span data-ttu-id="a67b2-181">Den här filen är konfigurationsfilen för fjärrskrivbord för den virtuella datorn MailSenderVM.</span><span class="sxs-lookup"><span data-stu-id="a67b2-181">This file is the remote desktop configuration file for the MailSenderVM virtual machine.</span></span> <span data-ttu-id="a67b2-182">Öppna filen för att starta anslutningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-182">Open the file to start the connection process.</span></span>

1. <span data-ttu-id="a67b2-183">I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-183">In the **Remote Desktop Connection** dialog, click **Connect**.</span></span>

1. <span data-ttu-id="a67b2-184">I dialogrutan **Windows-säkerhet** klickar du på **Använd ett annat konto**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-184">In the **Windows Security** dialog, click **Use another account**.</span></span>

1. <span data-ttu-id="a67b2-185">I textrutan **Användarnamn** skriver du **mailmaster**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-185">In the **Username** textbox, type **mailmaster**.</span></span>

1. <span data-ttu-id="a67b2-186">I textrutan **Lösenord** skriver du det lösenord som du angav för det här användarnamnet i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-186">In the **Password** textbox, type the password you entered for this user name in this exercise.</span></span> 

1. <span data-ttu-id="a67b2-187">I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-187">In the **Remote Desktop Connection** dialog, click **Yes**.</span></span>

<span data-ttu-id="a67b2-188">En Fjärrinloggningssession till den virtuella datorn startas nu.</span><span class="sxs-lookup"><span data-stu-id="a67b2-188">A remote desktop session to the virtual machine is now started.</span></span> <span data-ttu-id="a67b2-189">Det kan ta en stund att logga in för första gången.</span><span class="sxs-lookup"><span data-stu-id="a67b2-189">It might take a few moments to sign in for the first time.</span></span> <span data-ttu-id="a67b2-190">När inloggningen är klar visas verktyget **Serverhanteraren** på skärmen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-190">When sign-in is finished, the **Server Manager** tool will be displayed on the screen.</span></span>

### <a name="partition-and-format-our-data-disk-using-server-manager"></a><span data-ttu-id="a67b2-191">Partitionera och formatera datadisken med hjälp av Serverhanteraren</span><span class="sxs-lookup"><span data-stu-id="a67b2-191">Partition and format our data disk using Server Manager</span></span>

1. <span data-ttu-id="a67b2-192">När **Serverhanteraren** visas väljer du **File and Storage Services** (Fil- och lagringstjänster) i navigeringsfönstret till vänster.</span><span class="sxs-lookup"><span data-stu-id="a67b2-192">When **Server Manager** is displayed, select **File and Storage Services** in the navigation on the left.</span></span>

1. <span data-ttu-id="a67b2-193">Under **Volymer** väljer du **Diskar**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-193">Under **Volumes**, select **Disks**.</span></span>

1. <span data-ttu-id="a67b2-194">I listan över diskar är disk **0** operativsystemdisken och disk **1** är den temporära disken.</span><span class="sxs-lookup"><span data-stu-id="a67b2-194">In the list of disks, disk **0** is the operating system disk and disk **1** is the temporary disk.</span></span> <span data-ttu-id="a67b2-195">Väljer disk **2**, som är den nya virtuella hårddisken som du just lade till.</span><span class="sxs-lookup"><span data-stu-id="a67b2-195">Select disk **2**, which is the new VHD you just added.</span></span>

1. <span data-ttu-id="a67b2-196">Överst på fönsterrutan **VOLYMER** väljer du **UPPGIFTER** följt av **New Volume...** (Ny volym...). Menyn finns längst upp på skärmen enligt följande.</span><span class="sxs-lookup"><span data-stu-id="a67b2-196">At the top of the **VOLUMES** pane, select **TASKS** followed by **New Volume...**. The menu is in the top right of the screen as follows.</span></span>

   ![Skärmbild på menyn ”UPPGIFTER” expanderad för att visa tre menykommandon.](../media-draft/tasks-menu.png)


1. <span data-ttu-id="a67b2-199">I guiden **Ny volym** klickar du på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-199">In the **New Volume** wizard, click **Next**.</span></span>

1. <span data-ttu-id="a67b2-200">På sidan **Select server and disk** (Välj server och disk) väljer du **MailSenderVM** och **Disk 2** och klickar sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-200">In the **Select server and disk** page, select **MailSenderVM** and **Disk 2**, and then click **Next**.</span></span>

1. <span data-ttu-id="a67b2-201">I dialogrutan **Offline or Uninitiated Disk** (Offlinedisk eller oinitierad disk) klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-201">In the **Offline or Uninitiated Disk** dialog, click **OK**.</span></span>

1. <span data-ttu-id="a67b2-202">På sidan **Specify the size of the volume** (Ange storlek på volymen) klickar du på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-202">In the **Specify the size of the volume** page, click **Next**.</span></span>

1. <span data-ttu-id="a67b2-203">På sidan **Assign a drive letter** (Tilldela en enhetsbeteckning) väljer du **F:** följt av **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-203">In the **Assign a drive letter** page, select **F:** followed by **Next**.</span></span>

1. <span data-ttu-id="a67b2-204">På sidan **Select file system settings** (Välj filsysteminställningar) går du till textrutan **Volume label** (Volymetikett) och skriver **Inkommande** och väljer sedan **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-204">In the **Select file system settings** page, in the **Volume label** textbox, type **Incoming** and then select **Next**.</span></span>

1. <span data-ttu-id="a67b2-205">På sidan **Confirm selections** (Bekräfta val) väljer du **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-205">In the **Confirm selections** page, select **Create**.</span></span> <span data-ttu-id="a67b2-206">Windows initierar disken och formaterar den nya partitionen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-206">Windows initiates the disk and formats the new partition.</span></span>

1. <span data-ttu-id="a67b2-207">På sidan **Completion** (Slutförande) väljer du **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-207">In the **Completion** page, select **Close**.</span></span>

<span data-ttu-id="a67b2-208">Nu tittar vi på vår nya diskvolym i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="a67b2-208">Let's have a look at our new disk volume in File Explorer.</span></span> 
1. <span data-ttu-id="a67b2-209">Öppna **Utforskaren**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-209">Open **File Explorer**.</span></span>

1. <span data-ttu-id="a67b2-210">I navigeringsfönstret till vänster klickar du på **Den här datorn** och dubbelklickar sedan på **Inkommande (F:)**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-210">In the navigation in the left, click **This PC** and then double-click **Incoming (F:)**.</span></span>

1. <span data-ttu-id="a67b2-211">Välj **Start** och sedan **Ny mapp**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-211">Select **Home**, and then **New Folder**.</span></span>

1. <span data-ttu-id="a67b2-212">Typ **Drop** och tryck sedan på Retur.</span><span class="sxs-lookup"><span data-stu-id="a67b2-212">Type **Drop** and then press Enter.</span></span>

<span data-ttu-id="a67b2-213">Nu har vi en ny volym som skapats på vår virtuella hårddisk som kallas **Incoming** (Inkommande), och vi har skapat en mapp med namnet **Drop** på volymen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-213">We now have a new volume created on our VHD called **Incoming**, and we've created a folder called **Drop** on that volume.</span></span>  

1. <span data-ttu-id="a67b2-214">Stäng Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="a67b2-214">Close Windows Explorer.</span></span>

1. <span data-ttu-id="a67b2-215">På **Aktivitetsfältet** klickar du på **Start-knappen** följt av **Strömknappen** och sedan på **Disconnect** (Koppla från).</span><span class="sxs-lookup"><span data-stu-id="a67b2-215">On the **Task Bar**, click the **Start** button, click the **Power** button, and then click **Disconnect**.</span></span>

<span data-ttu-id="a67b2-216">Grattis!</span><span class="sxs-lookup"><span data-stu-id="a67b2-216">Congratulations!</span></span> <span data-ttu-id="a67b2-217">Du har skapat en virtuell Windows-dator, anslutit en ny virtuell hårddisk, skapat en volym på den virtuella hårddisken och lagt till en mapp i den volymen.</span><span class="sxs-lookup"><span data-stu-id="a67b2-217">You've successfully created a Windows VM, attached a new VHD, created a volume on that VHD and added a folder to that volume.</span></span> <span data-ttu-id="a67b2-218">Som du kanske kommer ihåg är den disktyp som vi använde för den nya virtuella hårddisken **Standard HDD**.</span><span class="sxs-lookup"><span data-stu-id="a67b2-218">If you recall, the disk type we used for the new VHD was a **Standard HDD**.</span></span> <span data-ttu-id="a67b2-219">I nästa enhet går vi igenom skillnaderna mellan Standard- och Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="a67b2-219">In the next unit, we'll learn the differences between standard and premium storage.</span></span> 
