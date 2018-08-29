<span data-ttu-id="83806-101">I den här övningen använder du RDP-klienten till att ansluta den virtuella Windows-dator du skapade i föregående utbildningsenhet.</span><span class="sxs-lookup"><span data-stu-id="83806-101">In this exercise, you'll use the RDP client to connect to the Windows VM that you created in the previous unit.</span></span> <span data-ttu-id="83806-102">Du kan upprätta anslutningen genom att hämta och köra en RDP-fil från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="83806-102">You can connect by downloading and running an RDP file from the Azure portal.</span></span> <span data-ttu-id="83806-103">Den här RDP-filen innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="83806-103">This RDP file will have:</span></span>

* <span data-ttu-id="83806-104">den virtuella datorns offentliga IP-adress</span><span class="sxs-lookup"><span data-stu-id="83806-104">The public IP address of the VM.</span></span>
* <span data-ttu-id="83806-105">portnumret.</span><span class="sxs-lookup"><span data-stu-id="83806-105">The port number.</span></span>

## <a name="motivation"></a><span data-ttu-id="83806-106">Motivering</span><span class="sxs-lookup"><span data-stu-id="83806-106">Motivation</span></span>

<span data-ttu-id="83806-107">I vårt utbildningsexempel är du en student som vill lära dig om att lägga till roller och funktioner i en Windows Server-dator.</span><span class="sxs-lookup"><span data-stu-id="83806-107">From our exercise scenario, you're now a student and you want to learn about adding roles and features to a Windows Server computer.</span></span> <span data-ttu-id="83806-108">Nätverksadministratören vill dock inte att du gör det här på en fysisk dator i nätverket, och universitetets datorer är inte tillräckligt kraftiga för att köra Windows Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="83806-108">However, your network administrator doesn't want you to do this on a physical computer on the network, and the school's computers are too poorly specified to run Windows Hyper-V.</span></span> <span data-ttu-id="83806-109">Azure är den perfekta lösningen.</span><span class="sxs-lookup"><span data-stu-id="83806-109">Azure provides the perfect solution.</span></span>

## <a name="configure-network-and-public-ip-address-settings"></a><span data-ttu-id="83806-110">Konfigurera inställningar för nätverk och offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="83806-110">Configure network and public IP address settings</span></span>

1. <span data-ttu-id="83806-111">Se till att bladet för den virtuella dator du skapade tidigare är öppen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="83806-111">In the Azure portal, ensure the blade for the virtual machine that you created earlier is open.</span></span> <span data-ttu-id="83806-112">Du hittar bladet under **Alla resurser** om du behöver öppna det.</span><span class="sxs-lookup"><span data-stu-id="83806-112">You can find the blade under **All Resources** if you need to open it.</span></span>

1. <span data-ttu-id="83806-113">Gå till avsnittet **Nätverk**.</span><span class="sxs-lookup"><span data-stu-id="83806-113">Go to the **Networking** section.</span></span> <span data-ttu-id="83806-114">Längst upp i det här avsnittet finns länkar till det virtuella undernätet och dynamiska IP-adresser som skapades tillsammans med den virtuella datorn eftersom vi använde standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="83806-114">The top of this section has links to the virtual subnet and dynamic IP address that were created along with our VM since we used the defaults.</span></span> <span data-ttu-id="83806-115">Vi skulle följa dessa länkar om vi ville ändra resurserna (till exempel för att byta till en statisk IP-adress).</span><span class="sxs-lookup"><span data-stu-id="83806-115">We would follow these links if we wanted to change those resources (for example, changing to a static IP address).</span></span>

1. <span data-ttu-id="83806-116">Klicka på **Lägg till regel för inkommande port**.</span><span class="sxs-lookup"><span data-stu-id="83806-116">Click **Add inbound port rule**.</span></span>

1. <span data-ttu-id="83806-117">Längst upp i dialogrutan **Lägg till inkommande säkerhetsregel** klickar du på **grundläggande**.</span><span class="sxs-lookup"><span data-stu-id="83806-117">At the top of the **Add inbound security rule** dialog box, click **basic**.</span></span>

1. <span data-ttu-id="83806-118">Under **Tjänst** väljer du **RDP** och klickar sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="83806-118">Under **Service**, select **RDP**, and then click **Add**.</span></span>

## <a name="connect-to-the-vm-by-using-rdp"></a><span data-ttu-id="83806-119">Ansluta till den virtuella datorn med RDP</span><span class="sxs-lookup"><span data-stu-id="83806-119">Connect to the VM by using RDP</span></span>

<span data-ttu-id="83806-120">Så här hämtar du RDP-filen och ansluter till den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="83806-120">To download the RDP file and connect to the VM, carry out the following steps.</span></span>

### <a name="download-the-rdp-file"></a><span data-ttu-id="83806-121">Ladda ned RDP-filen</span><span class="sxs-lookup"><span data-stu-id="83806-121">Download the RDP file</span></span>

1. <span data-ttu-id="83806-122">I avsnittet **Översikt** på bladet för den virtuella datorn klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="83806-122">In the **Overview** section of the virtual machine's blade, click **Connect**.</span></span>

1. <span data-ttu-id="83806-123">På bladet **Anslut till virtuell dator** ska du notera inställningarna för **IP-adress** och **Portnummer**. Klicka sedan på **Ladda ned RDP-fil**.</span><span class="sxs-lookup"><span data-stu-id="83806-123">In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings, then click **Download RDP File**.</span></span>

1. <span data-ttu-id="83806-124">Klicka på **Öppna** eller **Kör** i webbläsaren för att öppna RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="83806-124">In your browser, click **Open** or **Run** to open the RDP file.</span></span>

### <a name="connect-to-the-windows-vm"></a><span data-ttu-id="83806-125">Ansluta till den virtuella Windows-datorn</span><span class="sxs-lookup"><span data-stu-id="83806-125">Connect to the Windows VM</span></span>

1. <span data-ttu-id="83806-126">I dialogrutan **Anslutning till fjärrskrivbord** noterar du säkerhetsvarningen och fjärrdatorns IP-adress, och klickar sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="83806-126">In the **Remote Desktop Connection** dialog box, note the security warning and the remote computer IP address, then click **Connect**.</span></span>

1. <span data-ttu-id="83806-127">I dialogrutan **Windows-säkerhet** anger du samma användarnamn och lösenord som i steg 6 och 7.</span><span class="sxs-lookup"><span data-stu-id="83806-127">In the **Windows Security** dialog box, enter your user name and password that you used in steps 6 and 7.</span></span>

1. <span data-ttu-id="83806-128">I den andra dialogrutan **Anslutning till fjärrskrivbord** noterar du certifikatfelen och klickar sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="83806-128">In the second **Remote Desktop Connection** dialog box, note the certificate errors, then click **Yes**.</span></span>

   > [!Note]
   > <span data-ttu-id="83806-129">Det tar en stund innan den virtuella datorns skrivbord visas.</span><span class="sxs-lookup"><span data-stu-id="83806-129">The virtual machine desktop takes a while to appear.</span></span> <span data-ttu-id="83806-130">Det här beror på B1-avbildningens bristfälliga specifikation.</span><span class="sxs-lookup"><span data-stu-id="83806-130">This effect is because the B1 image is under-specified.</span></span> <span data-ttu-id="83806-131">Du kan se ett meddelande om låg minnesallokering.</span><span class="sxs-lookup"><span data-stu-id="83806-131">You may receive a message about low memory allocation.</span></span>

1. <span data-ttu-id="83806-132">I dialogrutan **Nätverk** klickar du på **Nej**.</span><span class="sxs-lookup"><span data-stu-id="83806-132">In the **Networks** dialog, click **No**.</span></span>

### <a name="resize-the-vm-in-the-azure-portal"></a><span data-ttu-id="83806-133">Ändra storlek på den virtuella datorn i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="83806-133">Resize the VM in the Azure portal</span></span>

1. <span data-ttu-id="83806-134">Växla tillbaka till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="83806-134">Switch back to the Azure portal.</span></span> <span data-ttu-id="83806-135">Under **Inställningar** på den virtuella datorns egenskapssida klickar du på **Storlek**.</span><span class="sxs-lookup"><span data-stu-id="83806-135">On the virtual machine properties page, under **Settings**, click **Size**.</span></span>

1. <span data-ttu-id="83806-136">Klicka på **D2s_v3** (2 virtuella processorer, 8 GB RAM-minne) och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="83806-136">Click **D2s_v3** (2 vCPUs, 8-GB RAM), and then click **Select**.</span></span> <span data-ttu-id="83806-137">Ett meddelande om storleksändringen visas.</span><span class="sxs-lookup"><span data-stu-id="83806-137">A resizing virtual machine message will display.</span></span> <span data-ttu-id="83806-138">Den virtuella datorn i RDP-fönstret stängs också.</span><span class="sxs-lookup"><span data-stu-id="83806-138">The VM in the RDP window will also close down.</span></span>

1. <span data-ttu-id="83806-139">Växla tillbaka till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="83806-139">Switch back to the Azure portal.</span></span> <span data-ttu-id="83806-140">Klicka på **Virtuella datorer** i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="83806-140">In the left pane, click **Virtual machines**.</span></span>

1. <span data-ttu-id="83806-141">Under **Virtuella datorer** väntar du tills den virtuella datorn har statusen **Körs**.</span><span class="sxs-lookup"><span data-stu-id="83806-141">Under **Virtual machines**, wait until your VM shows a status of **Running**.</span></span> <span data-ttu-id="83806-142">Du kan behöva klicka på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="83806-142">You may need to click **Refresh**.</span></span>

1. <span data-ttu-id="83806-143">Klicka på den virtuella datorns namn. Under **Inställningar** klickar du på **Storlek**.</span><span class="sxs-lookup"><span data-stu-id="83806-143">Click the virtual machine's name, then in **Settings**, click **Size**.</span></span> <span data-ttu-id="83806-144">Nu är den virtuella datorns storlek inställd på **D2s_v3**.</span><span class="sxs-lookup"><span data-stu-id="83806-144">Note the VM size is now set to **D2s_v3**.</span></span>

### <a name="reconnect-to-the-resized-vm"></a><span data-ttu-id="83806-145">Återansluta till den ändrade virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="83806-145">Reconnect to the resized VM</span></span>

1. <span data-ttu-id="83806-146">Klicka på **Virtuella datorer** och sedan på din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="83806-146">Click **Virtual machines**, then click your VM.</span></span> <span data-ttu-id="83806-147">Värdet för **Offentlig IP-adress** har förmodligen ändrats.</span><span class="sxs-lookup"><span data-stu-id="83806-147">Note the **Public IP Address** value has probably changed.</span></span> <span data-ttu-id="83806-148">Klicka på **Anslut** och sedan på **Kör** eller **Öppna** i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="83806-148">Click **Connect**, and then click **Run** or **Open** in your browser.</span></span>

1. <span data-ttu-id="83806-149">I dialogrutan **Anslutning till fjärrskrivbord** noterar du säkerhetsvarningen och fjärrdatorns IP-adress, och klickar sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="83806-149">In the **Remote Desktop Connection** dialog box, note the security warning and the remote computer IP address, then click **Connect**.</span></span>

1. <span data-ttu-id="83806-150">I dialogrutan **Windows-säkerhet** anger du samma användarnamn och lösenord som i steg 6 och 7.</span><span class="sxs-lookup"><span data-stu-id="83806-150">In the **Windows Security** dialog box, enter your user name and password that you used in steps 6 and 7.</span></span>

1. <span data-ttu-id="83806-151">I den andra dialogrutan **Anslutning till fjärrskrivbord** noterar du certifikatfelen och klickar sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="83806-151">In the second **Remote Desktop Connection** dialog box, note the certificate errors, then click **Yes**.</span></span> <span data-ttu-id="83806-152">Notera hur mycket snabbare det går att logga in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83806-152">Notice how much more responsive the virtual machine is to the sign-in process.</span></span>

1. <span data-ttu-id="83806-153">Högerklicka i aktivitetsfältet och klicka på **Aktivitetshanteraren**.</span><span class="sxs-lookup"><span data-stu-id="83806-153">Right-click the taskbar and click **Task Manager**.</span></span>

1. <span data-ttu-id="83806-154">I **Aktivitetshanteraren** klickar du på **Mer information**.</span><span class="sxs-lookup"><span data-stu-id="83806-154">In the **Task Manager** window, click **More details**.</span></span>

1. <span data-ttu-id="83806-155">Klicka på fliken **Prestanda**. Den totala mängden tillgängligt minnet är 8 GB, och ungefär 1,2 GB används.</span><span class="sxs-lookup"><span data-stu-id="83806-155">Click the **Performance** tab. Note the total available memory is 8 GB of which about 1.2 GB will be in use.</span></span> <span data-ttu-id="83806-156">Stäng **Aktivitetshanteraren**.</span><span class="sxs-lookup"><span data-stu-id="83806-156">Close **Task Manager**.</span></span>

## <a name="summary"></a><span data-ttu-id="83806-157">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="83806-157">Summary</span></span>

<span data-ttu-id="83806-158">Du har nu anslutit en virtuell Windows-dator med hjälp av RDP.</span><span class="sxs-lookup"><span data-stu-id="83806-158">You have connected to a Windows VM by using RDP.</span></span> <span data-ttu-id="83806-159">Du kan administrera den här virtuella datorn precis som vanliga Windows-datorer via skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="83806-159">With Desktop UI access, you can administer this VM as you would any Windows computer.</span></span>
