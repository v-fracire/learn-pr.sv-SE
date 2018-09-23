<span data-ttu-id="3a385-101">Din webbserver är igång men du inser att du behöver mer datorkraft för att göra upplevelsen bra för dina användare.</span><span class="sxs-lookup"><span data-stu-id="3a385-101">Your web server is up and running, but you realize you need more computing power to make the experience great for your users.</span></span> <span data-ttu-id="3a385-102">Hur kan du få den virtuella datorn att fungera snabbare?</span><span class="sxs-lookup"><span data-stu-id="3a385-102">How can you make your VM run faster?</span></span>

<span data-ttu-id="3a385-103">I ditt datacenter kan du flytta webbservern till kraftfullare maskinvara för att lösa prestandaproblemen.</span><span class="sxs-lookup"><span data-stu-id="3a385-103">In your data center, you might move your web server to more powerful hardware to solve performance problems.</span></span> <span data-ttu-id="3a385-104">Problemet är att du måste köpa, sätta upp och driva det nya systemet.</span><span class="sxs-lookup"><span data-stu-id="3a385-104">The problem is you need to buy, rack, and power your new system.</span></span> <span data-ttu-id="3a385-105">Med Azure är svaret mycket enklare.</span><span class="sxs-lookup"><span data-stu-id="3a385-105">With Azure, the answer is much simpler.</span></span>

<span data-ttu-id="3a385-106">Innan du skalar upp din virtuella dator till en kraftfullare storlek, låt oss först definiera vad skala innebär.</span><span class="sxs-lookup"><span data-stu-id="3a385-106">Before you scale up your VM to a more powerful size, let's first define what scale means.</span></span>

## <a name="what-is-scale"></a><span data-ttu-id="3a385-107">Vad är skalning?</span><span class="sxs-lookup"><span data-stu-id="3a385-107">What is scale?</span></span>

<span data-ttu-id="3a385-108">_Skalning_ innebär att lägga till bandbredd, minne eller datorkraft för att få bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="3a385-108">_Scale_ refers to adding network bandwidth, memory, storage, or compute power to achieve better performance.</span></span>  

<span data-ttu-id="3a385-109">Du kanske har hört termerna _skala upp_ och _skala ut_.</span><span class="sxs-lookup"><span data-stu-id="3a385-109">You may have heard the terms _scaling up_ and _scaling out_.</span></span>

<span data-ttu-id="3a385-110">Uppskalning, eller vertikal skalning betyder att öka minnet, lagringsutrymmet eller datorkraften på en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3a385-110">Scaling up, or vertical scaling, means to increase the memory, storage, or compute power on an existing virtual machine.</span></span> <span data-ttu-id="3a385-111">Du kan till exempel lägga till mer minne på en webb- eller databasserver så att den körs snabbare.</span><span class="sxs-lookup"><span data-stu-id="3a385-111">For example, you can add additional memory to a web or database server to make it run faster.</span></span>

<span data-ttu-id="3a385-112">Utskalning, eller horisontell skalning, betyder att driva ett program genom flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3a385-112">Scaling out, or horizontal scaling, means to additional virtual machines to power your application.</span></span> <span data-ttu-id="3a385-113">Du kan till exempel skapa många virtuella datorer som konfigureras på exakt samma sätt och använda en belastningsutjämnare för att fördela arbetet på datorerna.</span><span class="sxs-lookup"><span data-stu-id="3a385-113">For example, you might create many virtual machines configured in exactly the same way and use a load balancer to distribute work across them.</span></span>

> [!TIP]
> <span data-ttu-id="3a385-114">Molnet är elastisk.</span><span class="sxs-lookup"><span data-stu-id="3a385-114">The cloud is elastic.</span></span> <span data-ttu-id="3a385-115">Du kan _skala ned_ eller _skala in_ din distribution om du bara behöver skala upp eller skala ut tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="3a385-115">You can _scale down_ or _scale in_ your deployment if you needed to scale up or scale out only temporarily.</span></span> <span data-ttu-id="3a385-116">Att skala ned eller skala in kan hjälpa dig att spara pengar.</span><span class="sxs-lookup"><span data-stu-id="3a385-116">Scaling down or scaling in can help you save money.</span></span><br><br><span data-ttu-id="3a385-117">**Azure Advisor** och **Azure Cost Management** är två tjänster som hjälper dig optimera molnutgifter.</span><span class="sxs-lookup"><span data-stu-id="3a385-117">**Azure Advisor** and **Azure Cost Management** are two services that help you optimize cloud spend.</span></span> <span data-ttu-id="3a385-118">Du kan använda de här tjänsterna för att identifiera var du använder mer än du behöver och sedan skala tillbaka till den kapacitet som du faktiskt använder.</span><span class="sxs-lookup"><span data-stu-id="3a385-118">You can use these services to identify where you're using more than you need, and then scale back to the capacity you're actually using.</span></span>

## <a name="scale-up-your-vm"></a><span data-ttu-id="3a385-119">Skala upp din virtuella dator</span><span class="sxs-lookup"><span data-stu-id="3a385-119">Scale up your VM</span></span>

<span data-ttu-id="3a385-120">Kom ihåg att du angav storleken **Standard_DS2_v2** när du skapade din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="3a385-120">Recall that you specified the size **Standard_DS2_v2** when you created your VM.</span></span> <span data-ttu-id="3a385-121">Den virtuella datorn har två virtuella processorer och 7 GB minne.</span><span class="sxs-lookup"><span data-stu-id="3a385-121">Your VM currently has two virtual CPUs and 7 GB of memory.</span></span>

<span data-ttu-id="3a385-122">Vi uppgraderar till nästa storlek **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="3a385-122">Let's bump up to the next size, **Standard_DS3_v2**.</span></span> <span data-ttu-id="3a385-123">Den virtuella datorn har då fyra virtuella processorer och 14 GB minne.</span><span class="sxs-lookup"><span data-stu-id="3a385-123">Your VM will then have four virtual CPUs and 14 GB of memory.</span></span>

<span data-ttu-id="3a385-124">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="3a385-124">::: zone pivot="windows-cloud"</span></span>

1. <span data-ttu-id="3a385-125">Från Cloud Shell kör du `az vm resize` för att öka storleken på din virtuella dator till **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="3a385-125">From Cloud Shell, run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.</span></span>

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    <span data-ttu-id="3a385-126">Uppdateringen tar ungefär en minut.</span><span class="sxs-lookup"><span data-stu-id="3a385-126">The update process takes about a minute.</span></span> <span data-ttu-id="3a385-127">Din virtuella dator startas om under processen.</span><span class="sxs-lookup"><span data-stu-id="3a385-127">Your VM restarts during the process.</span></span>

1. <span data-ttu-id="3a385-128">Kör `az vm show` för att verifiera att den virtuella datorn körs med den nya storleken.</span><span class="sxs-lookup"><span data-stu-id="3a385-128">Run `az vm show` to verify that your VM is running the new size.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    <span data-ttu-id="3a385-129">Du ser den nya storleken på den virtuella datorn, **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="3a385-129">You see your new VM size, **Standard_DS3_v2**.</span></span>
    ```output
    Standard_DS3_v2
    ```

<span data-ttu-id="3a385-130">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="3a385-130">::: zone-end</span></span>

<span data-ttu-id="3a385-131">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="3a385-131">::: zone pivot="linux-cloud"</span></span>

1. <span data-ttu-id="3a385-132">Från Cloud Shell kör du `az vm resize` för att öka storleken på din virtuella dator till **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="3a385-132">From Cloud Shell, run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.</span></span>

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    <span data-ttu-id="3a385-133">Uppdateringen tar ungefär en minut.</span><span class="sxs-lookup"><span data-stu-id="3a385-133">The update process takes about a minute.</span></span> <span data-ttu-id="3a385-134">Din virtuella dator startas om under processen.</span><span class="sxs-lookup"><span data-stu-id="3a385-134">Your VM restarts during the process.</span></span>

1. <span data-ttu-id="3a385-135">Kör `az vm show` för att verifiera att den virtuella datorn körs med den nya storleken.</span><span class="sxs-lookup"><span data-stu-id="3a385-135">Run `az vm show` to verify that your VM is running the new size.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    <span data-ttu-id="3a385-136">Du ser den nya storleken på den virtuella datorn, **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="3a385-136">You see your new VM size, **Standard_DS3_v2**.</span></span>
    ```output
    Standard_DS3_v2
    ```

<span data-ttu-id="3a385-137">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="3a385-137">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="3a385-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3a385-138">Summary</span></span>

<span data-ttu-id="3a385-139">Bra jobbat!</span><span class="sxs-lookup"><span data-stu-id="3a385-139">Nice job!</span></span> <span data-ttu-id="3a385-140">Med bara ett kommando har din virtuella datorn nu blivit dubbelt så kraftfull.</span><span class="sxs-lookup"><span data-stu-id="3a385-140">With just one command, your VM is now twice as powerful.</span></span>

<span data-ttu-id="3a385-141">Att skala upp och skala ut är två sätt att öka prestandan.</span><span class="sxs-lookup"><span data-stu-id="3a385-141">Scaling up and scaling out are two ways to increase performance.</span></span> <span data-ttu-id="3a385-142">Här har du skalat upp den virtuella datorn för att öka dess datorkraft.</span><span class="sxs-lookup"><span data-stu-id="3a385-142">Here you scaled up your VM to increase its compute power.</span></span>