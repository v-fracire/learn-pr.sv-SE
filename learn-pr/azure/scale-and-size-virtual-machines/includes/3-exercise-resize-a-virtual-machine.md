<span data-ttu-id="87b6f-101">I den här övningen ska du skapa en virtuell dator och sedan ändra storlek på den med hjälp av portalen och Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87b6f-101">In this exercise, you will create a virtual machine and then resize it using the portal and Azure PowerShell.</span></span>

## <a name="create-a-vm"></a><span data-ttu-id="87b6f-102">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="87b6f-102">Create a VM</span></span>

1. <span data-ttu-id="87b6f-103">I webbläsaren går du till [Azure Portal](https://portal.azure.com?azure-portal=true) och loggar in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="87b6f-103">In your web browser, navigate to the [Azure Portal](https://portal.azure.com?azure-portal=true) and sign into your account.</span></span>

1. <span data-ttu-id="87b6f-104">Skapa en ny resurs i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="87b6f-104">In the Azure portal, create a new resource.</span></span> <span data-ttu-id="87b6f-105">På bladet **Nytt**  skriver du **virtuell dator** i sökrutan. Tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="87b6f-105">In the **New** blade, type **virtual machine** in the search box, and then press ENTER.</span></span>

1. <span data-ttu-id="87b6f-106">På bladet **Allting** under **Resultat**, klickar du på **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-106">In the **Everything** blade, under **Results**, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="87b6f-107">Klicka på **Skapa** på bladet **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-107">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="87b6f-108">På bladet **Grunder** fyller du i följande information och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-108">In the **Basics** blade, complete the details using the following information, and then click **OK**.</span></span>

    |<span data-ttu-id="87b6f-109">Inställning</span><span class="sxs-lookup"><span data-stu-id="87b6f-109">Setting</span></span>|<span data-ttu-id="87b6f-110">Värde</span><span class="sxs-lookup"><span data-stu-id="87b6f-110">Value</span></span>|
    |---|---|
    |<span data-ttu-id="87b6f-111">Namn</span><span class="sxs-lookup"><span data-stu-id="87b6f-111">Name</span></span>|<span data-ttu-id="87b6f-112">DB01</span><span class="sxs-lookup"><span data-stu-id="87b6f-112">DB01</span></span>|
    |<span data-ttu-id="87b6f-113">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="87b6f-113">Username</span></span>|<span data-ttu-id="87b6f-114">LocalAdmin</span><span class="sxs-lookup"><span data-stu-id="87b6f-114">LocalAdmin</span></span>|
    |<span data-ttu-id="87b6f-115">Lösenord och Bekräfta lösenord</span><span class="sxs-lookup"><span data-stu-id="87b6f-115">Password and Confirm password</span></span>|<span data-ttu-id="87b6f-116">Adm1nPa$$word</span><span class="sxs-lookup"><span data-stu-id="87b6f-116">Adm1nPa$$word</span></span>|
    |<span data-ttu-id="87b6f-117">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="87b6f-117">Resource group</span></span>|<span data-ttu-id="87b6f-118">ExerciseRG</span><span class="sxs-lookup"><span data-stu-id="87b6f-118">ExerciseRG</span></span>|
    |<span data-ttu-id="87b6f-119">Plats</span><span class="sxs-lookup"><span data-stu-id="87b6f-119">Location</span></span>|<span data-ttu-id="87b6f-120">USA, centrala</span><span class="sxs-lookup"><span data-stu-id="87b6f-120">Central US</span></span>|

1. <span data-ttu-id="87b6f-121">På bladet **Välj en storlek** väljer du **D2s_v3** och klickar sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-121">On the **Choose a size** blade, select **D2s_v3**, and then click **Select**.</span></span>

1. <span data-ttu-id="87b6f-122">På bladet **Inställningar** under **Välj offentliga inkommande portar** väljer du **HTTP**, **HTTPS** och **RDP (3389)** under **Startdiagnostik**. Klicka på **Inaktiverad**, lämna alla andra inställningar med standardvärdet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-122">On the **Settings** blade, under **Select public inbound ports** select **HTTP**, **HTTPS**, and **RDP (3389)**, under **Boot diagnostics** click **Disabled**, leave all other settings at the default value, and then click **OK**.</span></span>

1. <span data-ttu-id="87b6f-123">På bladet **Skapa** klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-123">On the **Create** blade, click **Create**.</span></span>

1. <span data-ttu-id="87b6f-124">Vänta tills distributionen är klar innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="87b6f-124">Wait until the deployment is complete before continuing the exercise.</span></span>

## <a name="resize-using-the-portal"></a><span data-ttu-id="87b6f-125">Ändra storlek med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="87b6f-125">Resize using the portal</span></span>

1. <span data-ttu-id="87b6f-126">I Azure Portal bläddrar du till resursgruppen ExerciseRG och på bladet **ExerciseRG** klickar du på det virtuella datorobjektet **DB01**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-126">In the Azure portal, browse to the ExerciseRG resource group, and in the **ExerciseRG** blade, click the **DB01** virtual machine object.</span></span>

1. <span data-ttu-id="87b6f-127">På bladet **DB01** klickar du på **Storlek**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-127">On the **DB01** blade, click **Size**.</span></span> <span data-ttu-id="87b6f-128">Observera att den markerade storleken är storleken som du valde när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="87b6f-128">Note the currently highlighted size is the size you selected when creating the virtual machine.</span></span>

1. <span data-ttu-id="87b6f-129">På bladet **Välj en storlek** försöker du hitta storleken **F2s_v2** – den bör inte finnas i listan med storlekar eftersom den virtuella datorn körs och den storleken är från en annan familj.</span><span class="sxs-lookup"><span data-stu-id="87b6f-129">On the **Choose a size** blade, try to find the **F2s_v2** size - it should not be available in the list of sizes because the VM is currently running and that size is from a different family.</span></span> <span data-ttu-id="87b6f-130">Stäng bladet **Välj en storlek**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-130">Close the **Choose a size** blade.</span></span>

1. <span data-ttu-id="87b6f-131">På bladet **DB01** klickar du på **Stoppa**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-131">In the **DB01** blade, click **Stop**.</span></span> <span data-ttu-id="87b6f-132">I dialogrutan **Stoppa denna virtuella dator** klickar du på **Ja** och väntar tills statusen för den virtuella datorn visar **Stoppad (frigjord)**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-132">In the **Stop this virtual machine** dialog box, click **Yes**, and wait for the virtual machine status to show **Stopped (deallocated)**.</span></span>

1. <span data-ttu-id="87b6f-133">På bladet **DB01** klickar du på **Storlek**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-133">On the **DB01** blade, click **Size**.</span></span> <span data-ttu-id="87b6f-134">På bladet **Välj en storlek** väljer du **F2s_v2** och klickar sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-134">On the **Choose a size** blade, select **F2s_v2** and then click **Select**.</span></span> <span data-ttu-id="87b6f-135">Observera meddelandet om att ändra storlek på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="87b6f-135">Notice the notification about resizing the virtual machine.</span></span>

1. <span data-ttu-id="87b6f-136">På bladet **DB01** klickar du på **Översikt** och sedan på **Starta**.</span><span class="sxs-lookup"><span data-stu-id="87b6f-136">On the **DB01** blade, click **Overview**, then click **Start**.</span></span>

## <a name="resize-using-powershell"></a><span data-ttu-id="87b6f-137">Ändra storlek med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="87b6f-137">Resize using PowerShell</span></span>

1. <span data-ttu-id="87b6f-138">Öppna Cloud Shell i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="87b6f-138">In the Azure portal, open the Cloud Shell.</span></span>

1. <span data-ttu-id="87b6f-139">Använd följande cmdlet för att hämta listan med tillgängliga virtuella datorstorlekar.</span><span class="sxs-lookup"><span data-stu-id="87b6f-139">Use the following cmdlet to get the list of available virtual machine sizes.</span></span>

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName ExerciseRG -VMName DB01
    ```

1. <span data-ttu-id="87b6f-140">Använd följande cmdlet för att ändra storlek på den virtuella datorn till en F4s_v2-storlek.</span><span class="sxs-lookup"><span data-stu-id="87b6f-140">Use the following cmdlet to resize the virtual machine to an F4s_v2 size.</span></span>

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName ExerciseRG -VMName DB01
    $vm.HardwareProfile.VmSize = "Standard_F4s_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName ExerciseRG
    ```

1. <span data-ttu-id="87b6f-141">Klicka på knappen Uppdatera på bladet DB01 medan du väntar på att PowerShell-kommandot ska slutföras. Du bör se att den virtuella datorn startas om efter ändringen i storlek.</span><span class="sxs-lookup"><span data-stu-id="87b6f-141">Click the Refresh button in the DB01 blade while you are waiting for the PowerShell command to complete - you should notice that the virtual machine is restarting to accommodate the change in size.</span></span>

<span data-ttu-id="87b6f-142">I den här övningen skapade du en virtuell dator och ändrade storlek på den med två olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="87b6f-142">In this exercise, you created a virtual machine and resized it with two different tools.</span></span> <span data-ttu-id="87b6f-143">Ett bra tips att tänka på är att målstorleken kanske inte är tillgänglig medan den virtuella datorn körs. Om du stoppar den virtuella datorn kan du välja fler storlekar.</span><span class="sxs-lookup"><span data-stu-id="87b6f-143">A good tip to keep in mind is that the target size may not be available while the virtual machine is running; stopping the virtual machine lets you choose more sizes.</span></span>