<span data-ttu-id="d5709-101">I den här övningen ska du skapa en virtuell dator och sedan ändra storlek på den med hjälp av portalen och Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5709-101">In this exercise, you will create a virtual machine and then resize it using the portal and Azure PowerShell.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-vm-with-powershell"></a><span data-ttu-id="d5709-102">Skapa en virtuell dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5709-102">Create a VM with PowerShell</span></span>

1. <span data-ttu-id="d5709-103">Vi använder cmdleten `New-AzureRmVm` i Azure PowerShell för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d5709-103">Let's use the `New-AzureRmVm` cmdlet in Azure PowerShell to create a VM.</span></span>
    - <span data-ttu-id="d5709-104">Använd resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="d5709-104">Use the Resource Group **<rgn>[sandbox resource group name]</rgn>**.</span></span>
    - <span data-ttu-id="d5709-105">Ge den namnet my-test-vm.</span><span class="sxs-lookup"><span data-stu-id="d5709-105">Name it "my-test-vm".</span></span>
    - <span data-ttu-id="d5709-106">Skicka in _Standard_DS2_v2_ för **Storlek**-parametern.</span><span class="sxs-lookup"><span data-stu-id="d5709-106">Pass in _Standard_DS2_v2_ for the **Size** parameter.</span></span>
    - <span data-ttu-id="d5709-107">Välj en plats nära dig i följande lista som är tillgänglig i Azure-sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="d5709-107">Select a location close to you from the following list available in the Azure sandbox.</span></span> <span data-ttu-id="d5709-108">Ändra värdet i nedanstående exempelkommando om du använder kopiera och klistra in.</span><span class="sxs-lookup"><span data-stu-id="d5709-108">Make sure to change the value in the below example command if you are using copy and paste.</span></span>

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - <span data-ttu-id="d5709-109">Använd cmdleten `Get-Credential` och mata in resultaten i parametern `Credential` som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="d5709-109">Use the `Get-Credential` cmdlet and feed the results into the `Credential` parameter as shown below.</span></span>

       <span data-ttu-id="d5709-110">När du uppmanas att ange autentiseringsuppgifter måste du använda LocalAdmin som användarnamn och Adm1nPa$$word som lösenord.</span><span class="sxs-lookup"><span data-stu-id="d5709-110">When prompted to enter credentials, use LocalAdmin as the user name and Adm1nPa$$word as the password.</span></span>

    ```powershell
    New-AzureRmVm `
        -ResourceGroupName <rgn>[sandbox resource group name]</rgn> `
        -Name my-test-vm `
        -Credential (Get-Credential) `
        -Size "Standard_DS2_v2" `
        -Location eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


1. <span data-ttu-id="d5709-111">Vänta tills distributionen är klar innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="d5709-111">Wait until the deployment is complete before continuing the exercise.</span></span>

## <a name="resize-using-the-portal"></a><span data-ttu-id="d5709-112">Ändra storlek med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="d5709-112">Resize using the portal</span></span>

1. <span data-ttu-id="d5709-113">Logga in på [Azure-portalen för Sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du aktiverade sandbox-miljön med.</span><span class="sxs-lookup"><span data-stu-id="d5709-113">Sign into the [Azure portal for sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="d5709-114">Välj **Resursgrupper** i det vänstra sidofältet.</span><span class="sxs-lookup"><span data-stu-id="d5709-114">Select **Resource Groups** from the left side bar.</span></span>

1. <span data-ttu-id="d5709-115">Välj resursgruppen <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn> och klicka på det virtuella datorobjektet **my-test-vm** i översikten.</span><span class="sxs-lookup"><span data-stu-id="d5709-115">Select the <rgn>[sandbox resource group name]</rgn> resource group, and in the overview, click the **my-test-vm** virtual machine object.</span></span>

1. <span data-ttu-id="d5709-116">I **översikten** för den virtuella datorn bör du lägga märke till att den aktuella storleken på den virtuella datorn är _Standard DS2 v2 (2 virtuella processorer, 7 GB minne)_ vilket är vad vi skapade alldeles nyss.</span><span class="sxs-lookup"><span data-stu-id="d5709-116">On the **Overview** of the virtual machine, notice that the current size of the VM is _Standard DS2 v2 (2 vcpus, 7 GB memory)_ which is what we created a moment ago.</span></span>

1. <span data-ttu-id="d5709-117">I avsnittet **Inställningar** väljer du **Storlek**.</span><span class="sxs-lookup"><span data-stu-id="d5709-117">In the **Settings** section, select **Size**.</span></span>

1. <span data-ttu-id="d5709-118">På bladet **Välj en storlek** försöker du hitta storleken **F2s_v2**.</span><span class="sxs-lookup"><span data-stu-id="d5709-118">On the **Choose a size** blade, try to find the **F2s_v2** size.</span></span> <span data-ttu-id="d5709-119">Du kommer inte se den i listan över tillgängliga storlekar eftersom den virtuella datorn körs och den storleken är från en annan familj.</span><span class="sxs-lookup"><span data-stu-id="d5709-119">You will not see it in the list of available sizes because the VM is currently running and that size is from a different family.</span></span> <span data-ttu-id="d5709-120">I vissa fall kommer du behöva stoppa den virtuella datorn om du vill se alla tillgängliga virtuella datorstorlekar.</span><span class="sxs-lookup"><span data-stu-id="d5709-120">In some cases, you will need to stop the VM in order to see all available VM sizes.</span></span>

1. <span data-ttu-id="d5709-121">Nu väljer vi en storlek som är tillgänglig för tillfället medan den här virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="d5709-121">Let's choose a size that is currently available while this VM is running.</span></span> <span data-ttu-id="d5709-122">På bladet **Välj en storlek** väljer du _DS3_v2 Standard_ och klickar sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="d5709-122">While still on the **Choose a size** blade, select _DS3_v2 Standard_ and then click **Select**.</span></span> <span data-ttu-id="d5709-123">Observera meddelandet om att ändra storlek på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d5709-123">Notice the notification about resizing the virtual machine.</span></span>

1. <span data-ttu-id="d5709-124">På panelen **Översikt** bekräftar du att den virtuella datorn har storleksändrats till _Standard DS3 v2 (4 vCPU, 14 GB minne)_.</span><span class="sxs-lookup"><span data-stu-id="d5709-124">On the **Overview** panel, confirm that the VM has been resized to _Standard DS3 v2 (4 vcpus, 14 GB memory)_.</span></span>

## <a name="resize-using-powershell"></a><span data-ttu-id="d5709-125">Ändra storlek med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5709-125">Resize using PowerShell</span></span>

1. <span data-ttu-id="d5709-126">Öppna Azure Cloud Shell i Azure-portalen genom att klicka på knappen Cloud Shell i det övre verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="d5709-126">In the Azure portal, open Azure Cloud Shell by clicking the Cloud Shell button in the top toolbar.</span></span>

    <span data-ttu-id="d5709-127">Kontrollera att Cloud Shell är konfigurerat att använda PowerShell och inte Bash, längst upp till vänster i fönstret Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="d5709-127">Ensure that the Cloud Shell is set to use PowerShell and not Bash, in the top left of the Cloud Shell window.</span></span>

1. <span data-ttu-id="d5709-128">Använd följande cmdlet för att hämta listan med tillgängliga virtuella datorstorlekar.</span><span class="sxs-lookup"><span data-stu-id="d5709-128">Use the following cmdlet to get the list of available virtual machine sizes.</span></span>

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    ```

1. <span data-ttu-id="d5709-129">Använd följande cmdlet för att ändra storlek på den virtuella datorn tillbaka till storleken _DS2_v2_.</span><span class="sxs-lookup"><span data-stu-id="d5709-129">Use the following cmdlet to resize the virtual machine back to a _DS2_v2_ size.</span></span>

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    $vm.HardwareProfile.VmSize = "Standard_DS2_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```

1. <span data-ttu-id="d5709-130">Klicka på knappen **Uppdatera** på bladet **my-test-vm** medan du väntar på att PowerShell-kommandot ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="d5709-130">Click the **Refresh** button in the **my-test-vm** blade while you are waiting for the PowerShell command to finish.</span></span> <span data-ttu-id="d5709-131">Du bör se att den virtuella datorn startas om efter storleksändringen.</span><span class="sxs-lookup"><span data-stu-id="d5709-131">You should notice that the virtual machine is restarting to accommodate the change in size.</span></span>

<span data-ttu-id="d5709-132">I den här övningen skapade du en virtuell dator och ändrade storlek på den med två olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="d5709-132">In this exercise, you created a virtual machine and resized it with two different tools.</span></span> <span data-ttu-id="d5709-133">Ett bra tips att tänka på är att målstorleken kanske inte är tillgänglig medan den virtuella datorn körs. Om du stoppar den virtuella datorn kan du välja fler storlekar.</span><span class="sxs-lookup"><span data-stu-id="d5709-133">A good tip to keep in mind is that the target size may not be available while the virtual machine is running; stopping the virtual machine lets you choose more sizes.</span></span>