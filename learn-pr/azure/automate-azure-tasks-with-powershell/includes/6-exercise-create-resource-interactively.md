<span data-ttu-id="a94d1-101">Kom ihåg vårt ursprungliga scenario – vi skapade virtuella datorer för att testa vår CRM-programvara.</span><span class="sxs-lookup"><span data-stu-id="a94d1-101">Recall our original scenario - creating VMs to test our CRM software.</span></span> <span data-ttu-id="a94d1-102">När en ny version är tillgänglig vill vi förbättra vår nya virtuella dator så att vi kan testa hela installationen från en ren avbildning.</span><span class="sxs-lookup"><span data-stu-id="a94d1-102">When a new build is available, we want to spin up a new VM so we can test the full install experience from a clean image.</span></span> <span data-ttu-id="a94d1-103">När vi är klara vill vi ta bort den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a94d1-103">Then when we are finished, we want to delete the VM.</span></span>

<span data-ttu-id="a94d1-104">Nu ska vi prova de kommandon som du använder för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a94d1-104">Let's try the commands you would use to create a VM.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm-with-azure-powershell"></a><span data-ttu-id="a94d1-105">Skapa en virtuell Linux-dator med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a94d1-105">Create a Linux VM with Azure PowerShell</span></span>

<span data-ttu-id="a94d1-106">Eftersom vi använder sandbox-miljön för Azure kan behöver du inte skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a94d1-106">Since we are using the Azure Sandbox, you won't have to create a Resource Group.</span></span> <span data-ttu-id="a94d1-107">Välj istället **resursgruppen<rgn> </rgn>[Resursgruppsnamn för sandbox-miljö]**.</span><span class="sxs-lookup"><span data-stu-id="a94d1-107">Instead, use the Resource Group **<rgn>[Sandbox resource group name]</rgn>**.</span></span> <span data-ttu-id="a94d1-108">Dessutom bör du vara medveten om platsbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="a94d1-108">In addition, be aware of the location restrictions.</span></span>

<span data-ttu-id="a94d1-109">Nu ska vi skapa en ny virtuell Azure-dator med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a94d1-109">Let's create a new Azure VM with PowerShell.</span></span>

1. <span data-ttu-id="a94d1-110">Använd cmdlet:en `New-AzureRmVm` för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a94d1-110">Use the `New-AzureRmVm` cmdlet to create a VM.</span></span>
    - <span data-ttu-id="a94d1-111">Använd den resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="a94d1-111">Use the Resource Group **<rgn>[Sandbox resource group name]</rgn>**.</span></span>
    - <span data-ttu-id="a94d1-112">Namnge den virtuella datorn – vanligtvis du vill använda ett mer beskrivande namn som identifierar den virtuella datorns syfte, plats, och antal instanser (om det finns fler än en).</span><span class="sxs-lookup"><span data-stu-id="a94d1-112">Give the VM a name - typically you want to use something meaningful that identifies the purposes of the VM, location, and (if there is more than one) instance number.</span></span> <span data-ttu-id="a94d1-113">Vi använder ”testvm-eus-01” för ”virtuell testdator East US instans 1”.</span><span class="sxs-lookup"><span data-stu-id="a94d1-113">We'll use "testvm-eus-01" for "Test VM in East US, instance 1".</span></span> <span data-ttu-id="a94d1-114">Använd ditt eget namn baserat på var den virtuella datorn finns.</span><span class="sxs-lookup"><span data-stu-id="a94d1-114">Come up with your own name based on where you place the VM.</span></span>
    - <span data-ttu-id="a94d1-115">Välj en plats nära dig i följande lista som är tillgänglig i Azure-sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="a94d1-115">Select a location close to you from the following list available in the Azure Sandbox.</span></span> <span data-ttu-id="a94d1-116">Ändra värdet i nedanstående exempelkommando om du använder kopiera och klistra in.</span><span class="sxs-lookup"><span data-stu-id="a94d1-116">Make sure to change the value in the below example command if you are using copy and paste.</span></span>

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - <span data-ttu-id="a94d1-117">Använd ”UbuntuLTS” för en här avbildningen – Detta är Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="a94d1-117">Use "UbuntuLTS" for the image - this is Ubuntu Linux.</span></span>
    - <span data-ttu-id="a94d1-118">Använd cmdlet:en `Get-Credential` och mata in resultaten i parametern `Credential`.</span><span class="sxs-lookup"><span data-stu-id="a94d1-118">Use the `Get-Credential` cmdlet and feed the results into the `Credential` parameter.</span></span>
    - <span data-ttu-id="a94d1-119">Lägg till parametern `-OpenPorts` och ”22” som port – vi kan nu logga in med SSH på datorn.</span><span class="sxs-lookup"><span data-stu-id="a94d1-119">Add the `-OpenPorts` parameter and pass "22" as the port - this will let us SSH into the machine.</span></span>
 
    ```powershell
    New-AzureRmVm -ResourceGroupName <rgn>[Sandbox resource group name]</rgn> -Name "testvm-eus-01" -Credential (Get-Credential) -Location "East US" -Image UbuntuLTS -OpenPorts 22
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
    
1. <span data-ttu-id="a94d1-120">Det här kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="a94d1-120">This will take a few minutes to complete.</span></span> <span data-ttu-id="a94d1-121">När detta sker kan du fråga och tilldela objektet från den virtuella datorn till en variabel (`$vm`).</span><span class="sxs-lookup"><span data-stu-id="a94d1-121">Once it does, you can query it and assign the VM object to a variable (`$vm`).</span></span>

    ```powershell
    $vm = Get-AzureRmVM -Name "testvm-eus-01" -ResourceGroupName <rgn>[Sandbox resource group name]</rgn>
    ```
    
1. <span data-ttu-id="a94d1-122">Hämta sedan värdet för att dumpa information om den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="a94d1-122">Then query the value to dump out the information about the VM:</span></span>

    ```powershell
    $vm
    ```

    <span data-ttu-id="a94d1-123">Du bör se något liknande följande:</span><span class="sxs-lookup"><span data-stu-id="a94d1-123">You should see something like:</span></span>

    ```output
    ResourceGroupName : <rgn>[Sandbox resource group name]</rgn>
    Id                : /subscriptions/xxxxxxxx-xxxx-aaaa-bbbb-cccccccccccc/resourceGroups/<rgn>[Sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/testvm-eus-01
    VmId              : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    Name              : testvm-eus-01
    Type              : Microsoft.Compute/virtualMachines
    Location          : eastus
    Tags              : {}
    HardwareProfile   : {VmSize}
    NetworkProfile    : {NetworkInterfaces}
    OSProfile         : {ComputerName, AdminUsername, LinuxConfiguration, Secrets}
    ProvisioningState : Succeeded
    StorageProfile    : {ImageReference, OsDisk, DataDisks}
    ```
    
1. <span data-ttu-id="a94d1-124">Du kan få åtkomst till komplexa objekt via en punktsyntax (”.”).</span><span class="sxs-lookup"><span data-stu-id="a94d1-124">You can reach into complex objects through a dot (".") syntax.</span></span> <span data-ttu-id="a94d1-125">Till exempel, för att visa egenskaperna i objektet `VMSize` som är associerade med området HardwareProfile kan du skriva:</span><span class="sxs-lookup"><span data-stu-id="a94d1-125">For example, to see the properties in the `VMSize` object associated with the HardwareProfile section you can type:</span></span>

    ```powershell
    $vm.HardwareProfile
    ```

1. <span data-ttu-id="a94d1-126">Eller för att få information om en av diskarna:</span><span class="sxs-lookup"><span data-stu-id="a94d1-126">Or to get information on one of the disks:</span></span>

    ```powershell
    $vm.StorageProfile.OsDisk
    ```

1. <span data-ttu-id="a94d1-127">Du kan även skicka objekt från virtuella datorer till andra cmdlet:ar.</span><span class="sxs-lookup"><span data-stu-id="a94d1-127">You can even pass the VM object into other cmdlets.</span></span> <span data-ttu-id="a94d1-128">Till exempel, detta kommer att hämta din virtuella dators offentliga IP-adress:</span><span class="sxs-lookup"><span data-stu-id="a94d1-128">For example, this will retrieve the public IP address of your VM:</span></span>

    ```powershell
    $vm | Get-AzureRmPublicIpAddress
    ```

1. <span data-ttu-id="a94d1-129">Med IP-adressen kan du ansluta till den virtuella datorn med SSH.</span><span class="sxs-lookup"><span data-stu-id="a94d1-129">With the IP address, you can connect to the VM with SSH.</span></span> <span data-ttu-id="a94d1-130">Till exempel om du använde användarnamnet ”bob”, och IP-adressen är ”205.22.16.5” ansluter kommandot till Linux-datorn:</span><span class="sxs-lookup"><span data-stu-id="a94d1-130">For example, if you used the username "bob", and the IP address is "205.22.16.5", then this command would connect to the Linux machine:</span></span>

```powershell
ssh bob@205.22.16.5
```

## <a name="delete-a-vm"></a><span data-ttu-id="a94d1-131">Ta bort en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a94d1-131">Delete a VM</span></span>

<span data-ttu-id="a94d1-132">Nu ska vi ta bort den virtuella datorn, bara för att testa några fler kommandon.</span><span class="sxs-lookup"><span data-stu-id="a94d1-132">Just to try out some more commands, let's delete the VM.</span></span> <span data-ttu-id="a94d1-133">Först ska vi stänga av den.</span><span class="sxs-lookup"><span data-stu-id="a94d1-133">We'll shut it down first.</span></span>

```powershell
Stop-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

<span data-ttu-id="a94d1-134">Nu ska vi ta bort den virtuella datorn med cmdlet:en `Remove-AzureRmVM` :</span><span class="sxs-lookup"><span data-stu-id="a94d1-134">Now, let's delete the VM with the `Remove-AzureRmVM` cmdlet:</span></span>

```powershell
Remove-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

<span data-ttu-id="a94d1-135">Testa det här kommandot för att lista alla resurser i resursgruppen:</span><span class="sxs-lookup"><span data-stu-id="a94d1-135">Try this command to list all the resources in your resource group:</span></span>

```powershell
Get-AzureRmResource -ResourceGroupName $vm.ResourceGroupName | ft
```

<span data-ttu-id="a94d1-136">Du bör se en massa resurser (diskar, virtuella nätverk osv.) som fortfarande finns.</span><span class="sxs-lookup"><span data-stu-id="a94d1-136">You should see a bunch of resources (disks, virtual networks, etc.) that all still exist.</span></span> 

```output
Microsoft.Compute/disks
Microsoft.Network/networkInterfaces
Microsoft.Network/networkSecurityGroups
Microsoft.Network/publicIPAddresses
Microsoft.Network/virtualNetworks
```

<span data-ttu-id="a94d1-137">Detta beror på att `Remove-AzureRmVM` kommandot _endast tar bort den virtuella datorn_.</span><span class="sxs-lookup"><span data-stu-id="a94d1-137">This is because the `Remove-AzureRmVM` command _just deletes the VM_.</span></span> <span data-ttu-id="a94d1-138">Den tar inte bort någon annan resurs.</span><span class="sxs-lookup"><span data-stu-id="a94d1-138">It doesn't cleanup any of the other resources!</span></span> <span data-ttu-id="a94d1-139">Nu skulle vi vanligtvis bara ta bort resursgruppen och var klara.</span><span class="sxs-lookup"><span data-stu-id="a94d1-139">At this point, we'd likely just delete the Resource Group itself and be done with it.</span></span> <span data-ttu-id="a94d1-140">Men vi ska rensa den manuellt som övning.</span><span class="sxs-lookup"><span data-stu-id="a94d1-140">However, let's just run through the exercise to clean it up manually.</span></span> <span data-ttu-id="a94d1-141">Du bör se ett mönster i kommandona.</span><span class="sxs-lookup"><span data-stu-id="a94d1-141">You should see a pattern in the commands.</span></span>

1. <span data-ttu-id="a94d1-142">Ta bort nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a94d1-142">Delete the Network Interface.</span></span>

    ```powershell
    $vm | Remove-AzureRmNetworkInterface –Force
    ```
    
1. <span data-ttu-id="a94d1-143">Ta bort hanterade OS-diskar och lagringskonton</span><span class="sxs-lookup"><span data-stu-id="a94d1-143">Delete the managed OS disks and storage account</span></span>

    ```powershell
    Get-AzureRmDisk -ResourceGroupName $vm.ResourceGroupName -DiskName $vm.StorageProfile.OSDisk.Name | Remove-AzureRmDisk -Force
    ```

1. <span data-ttu-id="a94d1-144">Ta sedan bort det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="a94d1-144">Next, delete the virtual network.</span></span>

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmVirtualNetwork -Force
    ```

1. <span data-ttu-id="a94d1-145">Ta bort nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="a94d1-145">Delete the network security group.</span></span>

    ```powershell
    Get-AzureRmNetworkSecurityGroup -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmNetworkSecurityGroup -Force
    ```

1. <span data-ttu-id="a94d1-146">Och till sist, den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="a94d1-146">And finally, the public IP address.</span></span>

    ```powershell
    Get-AzureRmPublicIpAddress -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmPublicIpAddress -Force
    ```

<span data-ttu-id="a94d1-147">Nu bör vi ha fått med alla resurser. Kontrollera resursgruppen för att vara på den säkra sidan.</span><span class="sxs-lookup"><span data-stu-id="a94d1-147">We should have caught all the created resources; check the resource group just to be sure.</span></span> <span data-ttu-id="a94d1-148">Vi utförde en mängd manuella kommandon här, men en bättre metod hade varit att skriva ett _skript_ så att vi kunde återanvända logiken senare för att skapa eller ta bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a94d1-148">We did a lot of manual commands here but a better approach would have been to write a _script_ so we could reuse this logic later to create or delete a VM.</span></span> <span data-ttu-id="a94d1-149">Låt oss titta på skript med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a94d1-149">Let's look at scripting with PowerShell.</span></span>