<span data-ttu-id="39aa6-101">I den här övningen skapar du en lastbalanserare, ett virtuellt nätverk och flera virtuella datorer med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="39aa6-101">In this exercise, you will create a load balancer, a virtual network, and multiple virtual machines using the Azure portal.</span></span>

<span data-ttu-id="39aa6-102">Anta att du arbetar för Woodgrove Bank, ett nystartat företag som håller på att starta onlinetjänster för bankärenden.</span><span class="sxs-lookup"><span data-stu-id="39aa6-102">Suppose you work for Woodgrove Bank, a startup that is about to launch online banking services.</span></span> <span data-ttu-id="39aa6-103">Konkurrensen är mycket kraftig i den här sektorn, så du måste garantera minst 99,99 % tjänsttillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="39aa6-103">This sector is highly competitive, so you need to guarantee of a minimum of 99.99% service availability.</span></span> <span data-ttu-id="39aa6-104">Du har fastställt att en Azure Load Balancer med en pool med tre virtuella datorer uppfyller det här målet.</span><span class="sxs-lookup"><span data-stu-id="39aa6-104">You have determined that an Azure Load Balancer with a pool of three virtual machines will meet this goal.</span></span>

## <a name="create-a-public-load-balancer"></a><span data-ttu-id="39aa6-105">Skapa en offentlig lastbalanserare</span><span class="sxs-lookup"><span data-stu-id="39aa6-105">Create a public load balancer</span></span>

1. <span data-ttu-id="39aa6-106">I en webbläsare navigerar du till [Azure-portalen](https://portal.azure.com/?azure-portal=true) och loggar in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="39aa6-106">In a browser, navigate to the [Azure portal](https://portal.azure.com/?azure-portal=true) and sign in to your account.</span></span>

1. <span data-ttu-id="39aa6-107">I sidofältet klickar du på **Skapa en resurs**, och i det **nya** bladet klickar du på **Nätverk** och sedan på **Lastbalanserare**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-107">In the sidebar, click **Create a resource**, then in the **New** blade, click **Networking**, and then click **Load Balancer**.</span></span>

1. <span data-ttu-id="39aa6-108">På bladet Skapa lastbalanserare anger eller väljer du följande information:</span><span class="sxs-lookup"><span data-stu-id="39aa6-108">In the Create load balancer blade, enter or select the following information:</span></span>
    - <span data-ttu-id="39aa6-109">Namn: **woodgrove-LB**</span><span class="sxs-lookup"><span data-stu-id="39aa6-109">Name: **woodgrove-LB**</span></span>
    - <span data-ttu-id="39aa6-110">Typ: **Offentlig**</span><span class="sxs-lookup"><span data-stu-id="39aa6-110">Type: **Public**</span></span>
    - <span data-ttu-id="39aa6-111">SKU: **Basic**</span><span class="sxs-lookup"><span data-stu-id="39aa6-111">SKU: **Basic**</span></span>
    - <span data-ttu-id="39aa6-112">Offentlig IP-adress: välj **Skapa ny**, skriv **woodgrove-LB-ip** i textrutan och låt Tilldelning vara kvar som **Dynamisk**</span><span class="sxs-lookup"><span data-stu-id="39aa6-112">Public IP address: Select **Create new** and in the text box, type **woodgrove-LB-ip** in the text box, and leave the Assignment as **Dynamic**</span></span>
    - <span data-ttu-id="39aa6-113">Resursgrupp: välj **Skapa ny** och skriv **woodgrove-RG** i rutan</span><span class="sxs-lookup"><span data-stu-id="39aa6-113">Resource group: Select **Create new**, and in the box, type **woodgrove-RG**</span></span>
    - <span data-ttu-id="39aa6-114">Plats: Välj en region nära dig</span><span class="sxs-lookup"><span data-stu-id="39aa6-114">Location: Select a region near you</span></span>

1. <span data-ttu-id="39aa6-115">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-115">Click **Create**.</span></span>

1. <span data-ttu-id="39aa6-116">Vänta tills lastbalanseraren har distribuerats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="39aa6-116">Wait until the load balancer has deployed before continuing with the exercise.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="39aa6-117">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="39aa6-117">Create a Virtual Network</span></span>

1. <span data-ttu-id="39aa6-118">I den vänstra menyn klickar du på **Skapa en resurs**, och i det **nya** bladet klickar du på **Nätverk** och sedan på **Virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-118">In the left menu, click **Create a resource**, then in the **New** blade, click **Networking**, and then click **Virtual network**.</span></span>

1. <span data-ttu-id="39aa6-119">På bladet **Skapa virtuellt nätverk** anger eller väljer du följande information:</span><span class="sxs-lookup"><span data-stu-id="39aa6-119">In the **Create virtual network** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="39aa6-120">Namn: **woodgrove-VNET**</span><span class="sxs-lookup"><span data-stu-id="39aa6-120">Name: **woodgrove-VNET**</span></span>
    - <span data-ttu-id="39aa6-121">Adressutrymme: **172.20.0.0/16**</span><span class="sxs-lookup"><span data-stu-id="39aa6-121">Address space: **172.20.0.0/16**</span></span>
    - <span data-ttu-id="39aa6-122">Resursgrupp: välj **Använd befintlig** och sedan **woodgrove-RG**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-122">Resource group: Select **Use existing**, and select **woodgrove-RG**.</span></span>
    - <span data-ttu-id="39aa6-123">Undernät: **backendSubnet**</span><span class="sxs-lookup"><span data-stu-id="39aa6-123">Subnet: **backendSubnet**</span></span>
    - <span data-ttu-id="39aa6-124">Adressutrymme: **172.20.0.0/24**</span><span class="sxs-lookup"><span data-stu-id="39aa6-124">Address space: **172.20.0.0/24**</span></span>
    - <span data-ttu-id="39aa6-125">DDoS-skydd: **Basic**</span><span class="sxs-lookup"><span data-stu-id="39aa6-125">DDoS protection: **Basic**</span></span>
    - <span data-ttu-id="39aa6-126">Tjänstens slutpunkter: **Inaktiverat**</span><span class="sxs-lookup"><span data-stu-id="39aa6-126">Service endpoints: **Disabled**</span></span>

1. <span data-ttu-id="39aa6-127">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-127">Click **Create**.</span></span>

1. <span data-ttu-id="39aa6-128">Vänta tills det virtuella nätverket har distribuerats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="39aa6-128">Wait until the virtual network has deployed before continuing with the exercise.</span></span>

## <a name="create-a-vm-template"></a><span data-ttu-id="39aa6-129">Skapa en mall för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="39aa6-129">Create a VM template</span></span>

<span data-ttu-id="39aa6-130">Börja med att definiera grundläggande information för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="39aa6-130">Start by defining the basic VM information:</span></span>

1. <span data-ttu-id="39aa6-131">I Azure-portalen går du till den vänstra menyn och klickar på **Virtuella datorer** och sedan på **Skapa virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-131">In the Azure portal, in the left menu, click **Virtual machines**, and then click **Create virtual machine**.</span></span>

1. <span data-ttu-id="39aa6-132">På bladet Beräkning går du till området **Rekommenderas** och klickar på **Windows Server**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-132">On the Compute blade, in the **Recommended** section, click **Windows Server**.</span></span>

1. <span data-ttu-id="39aa6-133">På bladet **Windows Server** klickar du på **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-133">In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="39aa6-134">På bladet **Windows Server 2016 Datacenter** klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-134">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="39aa6-135">På bladet **Basic** går du till rutan **Namn** och skriver **woodgrove-SVR01**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-135">In the **Basics** blade, in the **Name** box, type **woodgrove-SVR01**.</span></span>

1. <span data-ttu-id="39aa6-136">I rutorna **Användarnamn** och **Lösenord** skriver du ett säkert namn och lösenord för ett administratörskonto på den här servern.</span><span class="sxs-lookup"><span data-stu-id="39aa6-136">In the **Username** and **Password boxes**, type a secure name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="39aa6-137">I rutan **Prenumeration** väljer du din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="39aa6-137">In the **Subscription** box, select your Azure subscription.</span></span>

1. <span data-ttu-id="39aa6-138">Under **Resursgrupp** väljer du **Använd befintlig**, och i listan väljer du **woodgrove-RG**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-138">Under **Resource group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="39aa6-139">I listrutan **Plats** väljer du en region nära dig.</span><span class="sxs-lookup"><span data-stu-id="39aa6-139">In the **Location** drop-down list, select a region near you.SAME</span></span>

1. <span data-ttu-id="39aa6-140">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-140">Click **OK**.</span></span>

<span data-ttu-id="39aa6-141">Välj en storlek för den virtuella datorn och konfigurera inställningar:</span><span class="sxs-lookup"><span data-stu-id="39aa6-141">Choose a size for the VM and configure settings:</span></span>

1. <span data-ttu-id="39aa6-142">På bladet **Välj storlek** väljer du en **Standard**-SKU, till exempel **D2s_v3** och klickar sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-142">On the **Choose a size** blade, select a **Standard** SKU, such as **D2s_v3**, and then click **Select**.</span></span>

1. <span data-ttu-id="39aa6-143">På bladet **Inställningar** klickar du på **Tillgänglighetsuppsättning**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-143">On the **Settings** blade, click **Availability set**.</span></span>

1. <span data-ttu-id="39aa6-144">På bladet **Ändra tillgänglighetsuppsättning** klickar du på **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-144">On the **Change availability set** blade, click **Create new**.</span></span>

1. <span data-ttu-id="39aa6-145">På bladet **Skapa ny** går du till rutan **Namn**, skriver **woodgrove-AS** och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-145">On the **Create new** blade, in the **Name** box, type **woodgrove-AS**, and then click **OK**.</span></span>

1. <span data-ttu-id="39aa6-146">På bladet **Inställningar** går du till **Nätverkssäkerhetsgrupp**, klickar på **Avancerat** och klickar sedan på **(new) woodgrove-SVR01-nsg**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-146">On the **Settings** blade, under **Network Security Group**, click **Advanced**, and then click **(new) woodgrove-SVR01-nsg**.</span></span>

1. <span data-ttu-id="39aa6-147">På bladet **Skapa nätverkssäkerhetsgrupp** går du till rutan **Namn**, ändrar namnet till **woodgrove-NSG** och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-147">On the **Create network Security group** blade, in the **Name** box, change the name to **woodgrove-NSG**, and then click **OK**.</span></span>

1. <span data-ttu-id="39aa6-148">I bladet **Inställningar** klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-148">On the **Settings** blade, click **OK**.</span></span>

<span data-ttu-id="39aa6-149">Spara inställningarna till en mall så att du enkelt kan distribuera flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="39aa6-149">Save the settings to a template, so that you easily deploy multiple VMs.</span></span>

1. <span data-ttu-id="39aa6-150">På bladet **Skapa** klickar du på **Ladda ned mall och parametrar**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-150">On the **Create** blade, click **Download template and parameters**.</span></span>

1. <span data-ttu-id="39aa6-151">På bladet **Mall** klickar du på **Lägg till i bibliotek**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-151">On the **Template** blade, click **Add to library**.</span></span>

1. <span data-ttu-id="39aa6-152">På bladet **Spara mallen** går du till rutorna **Namn** och **Beskrivning**, skriver in **woodgrove-server-template** och klickar sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-152">On the **Save template** blade, in the **Name** and **Description** boxes, type **woodgrove-server-template**, and then click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="39aa6-153">Om du behöver hitta den här mallen klickar du på **Alla tjänster** i den vänstra menyn, skriver **template** (mall) i filterrutan och klickar sedan på **Templates (PREVIEW)** (Mallar (FÖRHANDSVERSION)).</span><span class="sxs-lookup"><span data-stu-id="39aa6-153">If you need to find this template, click **All services** in the left menu, type **template** in the filter box, and then click **Templates (PREVIEW)**.</span></span>

## <a name="use-the-template-to-provision-the-first-vm"></a><span data-ttu-id="39aa6-154">Använda mallen för att etablera den första virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="39aa6-154">Use the template to provision the first VM</span></span>

1. <span data-ttu-id="39aa6-155">På bladet **Mall** klickar du på **Distribuera**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-155">On the **Template** blade, click **Deploy**.</span></span>

1. <span data-ttu-id="39aa6-156">På bladet **Custom deployment** (Anpassad distribution) går du till **Resursgrupp**, väljer **Använd befintlig** och väljer **woodgrove-RG** i listan.</span><span class="sxs-lookup"><span data-stu-id="39aa6-156">On the **Custom deployment** blade, under **Resource Group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="39aa6-157">På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Admin password** (Administratörslösenord) och skriver in samma lösenord som du använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="39aa6-157">On the **Custom deployment** blade, in the **Admin password** box, type the same password that you used previously.</span></span>

1. <span data-ttu-id="39aa6-158">På bladet **Custom deployment** (Anpassad distribution) markerar du kryssrutan **I agree to the terms and conditions** (Jag godkänner villkoren) och klickar sedan på **Köp** (kostnaden är den vanliga Azure-beräkningskostnaden, som beror på prisnivån för virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="39aa6-158">On the **Custom deployment** blade, select the **I agree to the terms and conditions** check box, and then click **Purchase** (the cost is the regular Azure compute charge, which depends on the VM pricing tier).</span></span>

1. <span data-ttu-id="39aa6-159">Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen; detta är så att du kan vara säker på att mallen är korrekt konfigurerad innan du använder den för att etablera ytterligare virtuella datorer och att alla de associerade resurserna har skapats.</span><span class="sxs-lookup"><span data-stu-id="39aa6-159">Wait until the VM has deployed before continuing with the exercise; this is so you can be sure the template is correctly configured before you use it to provision additional VMs, and all the associated resources have been created.</span></span>

## <a name="use-the-template-to-provision-two-additional-vms"></a><span data-ttu-id="39aa6-160">Använda mallen för att etablera ytterligare två virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="39aa6-160">Use the template to provision two additional VMs</span></span>

1. <span data-ttu-id="39aa6-161">I Azure-bladet går du till bladet **Mall** och klickar på **Distribuera**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-161">In the Azure portal, on the **Template** blade, click **Deploy**.</span></span>

1. <span data-ttu-id="39aa6-162">På bladet **Custom deployment** (Anpassad distribution) går du till **Resursgrupp**, väljer **Använd befintlig** och väljer **woodgrove-RG** i listan.</span><span class="sxs-lookup"><span data-stu-id="39aa6-162">On the **Custom deployment** blade, under **Resource Group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="39aa6-163">På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Namn på virtuell dator** och ändrar namnet till **woodgrove-SVR02**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-163">On the **Custom deployment** blade, in the **Virtual Machine Name** box, change the name to **woodgrove-SVR02**.</span></span>

1. <span data-ttu-id="39aa6-164">På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Network Interface Name** (Namn på nätverksgränssnitt) och ändrar namnet till **woodgrovesvr02222**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-164">On the **Custom deployment** blade, in the **Network Interface Name** box, change the name to **woodgrovesvr02222**.</span></span>

1. <span data-ttu-id="39aa6-165">På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Admin password** (Administratörslösenord) och skriver in samma lösenord som du använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="39aa6-165">On the **Custom deployment** blade, in the **Admin password** box, type the same password that you used previously.</span></span>

1. <span data-ttu-id="39aa6-166">På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Public Ip Address Name** (Namn på offentlig IP-adress) och ändrar namnet till **woodgrove-SVR02-ip**.</span><span class="sxs-lookup"><span data-stu-id="39aa6-166">On the **Custom deployment** blade, in the **Public Ip Address Name** box, change the name to **woodgrove-SVR02-ip**.</span></span>

1. <span data-ttu-id="39aa6-167">På bladet **Custom deployment** (Anpassad distribution) markerar du kryssrutan **I agree to the terms and conditions** (Jag godkänner villkoren) och klickar sedan på **Köp** (kostnaden är den vanliga Azure-beräkningskostnaden, som beror på prisnivån för virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="39aa6-167">On the **Custom deployment** blade, select the **I agree to the terms and conditions** check box, and then click **Purchase** (the cost is the regular Azure compute charge, which depends on the VM pricing tier).</span></span>

1. <span data-ttu-id="39aa6-168">Upprepa stegen 1–7 med följande information:</span><span class="sxs-lookup"><span data-stu-id="39aa6-168">Repeat steps 1 - 7, using the following information:</span></span>
    - <span data-ttu-id="39aa6-169">Namn på virtuell dator: **woodgrove-SVR03**</span><span class="sxs-lookup"><span data-stu-id="39aa6-169">Virtual Machine Name: **woodgrove-SVR03**</span></span>
    - <span data-ttu-id="39aa6-170">Namn på nätverksgränssnitt: **woodgrovesvr03333**</span><span class="sxs-lookup"><span data-stu-id="39aa6-170">Network Interface Name: **woodgrovesvr03333**</span></span>
    - <span data-ttu-id="39aa6-171">Namn på offentlig IP-adress: **woodgrove-SVRr03-ip**</span><span class="sxs-lookup"><span data-stu-id="39aa6-171">Public Ip Address Name: **woodgrove-SVRr03-ip**</span></span>

1. <span data-ttu-id="39aa6-172">Vänta tills de virtuella datorerna har distribuerats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="39aa6-172">Wait until the VMs have deployed before continuing with the exercise.</span></span>

<span data-ttu-id="39aa6-173">Nu har du en offentlig lastbalanserare som är redo att konfigureras och tre virtuella datorer som är redo att användas med den här lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="39aa6-173">You now have a public load balancer ready to configure, and three VMs ready to use with this load balancer.</span></span>