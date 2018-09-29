<span data-ttu-id="70768-101">I den föregående övningen utförde vi följande uppgifter med hjälp av Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="70768-101">In the previous exercise, we performed the following tasks using the Azure portal:</span></span>

- <span data-ttu-id="70768-102">Visa status för OS-diskcache</span><span class="sxs-lookup"><span data-stu-id="70768-102">View OS disk cache status</span></span>
- <span data-ttu-id="70768-103">Ändra inställningarna för cachelagring för OS-disken</span><span class="sxs-lookup"><span data-stu-id="70768-103">Change the cache settings of the OS disk</span></span>
- <span data-ttu-id="70768-104">Lägga till en datadisk i en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="70768-104">Add a data disk to the VM</span></span>
- <span data-ttu-id="70768-105">Ändra typ av cachelagring på en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="70768-105">Change caching type on a new data disk</span></span>

<span data-ttu-id="70768-106">Nu övar vi på dessa åtgärder med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70768-106">Let's practice these operations using Azure PowerShell.</span></span> 

> [!NOTE]
> <span data-ttu-id="70768-107">Vi ska använda Azure PowerShell, men du kan också använda Azure CLI som ger liknande funktionalitet som ett konsolbaserat verktyg.</span><span class="sxs-lookup"><span data-stu-id="70768-107">We're going to use Azure PowerShell, but you could also use the Azure CLI which provides similar functionality as a console-based tool.</span></span> <span data-ttu-id="70768-108">Det kan köras på Mac OS, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="70768-108">It runs on macOS, Linux, and Windows.</span></span> <span data-ttu-id="70768-109">Om du vill veta mer om Azure CLI kan du kolla in modulen **Hantera virtuella datorer med Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="70768-109">If you are interested in learning more about the Azure CLI, check out the **Manage Virtual Machines with the Azure CLI** module.</span></span>

<span data-ttu-id="70768-110">Vi använder den virtuella dator som vi skapade i den föregående övningen.</span><span class="sxs-lookup"><span data-stu-id="70768-110">We're going to use the VM we created in the previous exercise.</span></span> <span data-ttu-id="70768-111">Åtgärderna i den här labben förutsätter att:</span><span class="sxs-lookup"><span data-stu-id="70768-111">The operations in this lab assume:</span></span>

- <span data-ttu-id="70768-112">Vår virtuella dator finns och heter **fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="70768-112">Our VM exists and is called **fotoshareVM**</span></span>
- <span data-ttu-id="70768-113">Vår virtuella dator finns i en resursgrupp som heter **<rgn>[Sandbox-resursgruppnamn]</rgn>**</span><span class="sxs-lookup"><span data-stu-id="70768-113">Our VM lives in a resource group called **<rgn>[sandbox resource group name]</rgn>**</span></span>

<span data-ttu-id="70768-114">Om du har använt andra namn ersätter du de här värdena med dina.</span><span class="sxs-lookup"><span data-stu-id="70768-114">If you've gone with a different set of names, replace these values with yours.</span></span>

<span data-ttu-id="70768-115">Här är det aktuella tillståndet för våra VM-diskar från föregående övning:</span><span class="sxs-lookup"><span data-stu-id="70768-115">Here's the current state of our VM disks from the last exercise:</span></span>

![Skärmbild på våra OS- och datadiskar som båda är inställda på skrivskyddad cachelagring.](../media/disks-final-config-portal.PNG)

<span data-ttu-id="70768-117">Vi har använt portalen för att ange fältet **HOST CACHING** (Cachelagring för värd) för både för OS- och datadiskar.</span><span class="sxs-lookup"><span data-stu-id="70768-117">We used the portal to set the **HOST CACHING** field for both the OS and data disks.</span></span> <span data-ttu-id="70768-118">Tänk på detta inledande tillstånd när vi går igenom följande steg.</span><span class="sxs-lookup"><span data-stu-id="70768-118">Keep this initial state in mind as we work through the following steps.</span></span>

### <a name="set-up-some-variables"></a><span data-ttu-id="70768-119">Konfigurera några variabler</span><span class="sxs-lookup"><span data-stu-id="70768-119">Set up some variables</span></span>

<span data-ttu-id="70768-120">Först ska vi lagra vissa resursnamn så att vi kan använda dem senare.</span><span class="sxs-lookup"><span data-stu-id="70768-120">First, let's store some resource names so we can use them later.</span></span>

1. <span data-ttu-id="70768-121">Använd Azure Cloud Shell-terminalen till höger för att köra följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="70768-121">Use the Azure Cloud Shell terminal on the right to run the following PowerShell commands:</span></span>

    > [!NOTE]
    > <span data-ttu-id="70768-122">Växla Cloud Shell-sessionen till **PowerShell** innan du testar kommandona, om du inte har gjort det redan.</span><span class="sxs-lookup"><span data-stu-id="70768-122">Switch your Cloud Shell session to **PowerShell** before trying these commands, if it isn't already.</span></span>
    
    ```powershell
    $myRgName = "<rgn>[sandbox resource group name]</rgn>"
    $myVMName = "fotoshareVM"
    ```
    
    > [!TIP]
    > <span data-ttu-id="70768-123">Du måste ange dessa variabler igen om tidsgränsen uppnås för Cloud Shell-sessionen. Om möjligt bör du därför gå igenom hela den labben i en enda session.</span><span class="sxs-lookup"><span data-stu-id="70768-123">You'll have to set these variables again if your Cloud Shell session times out. So, if possible, work through this entire lab in a single session.</span></span>
    
### <a name="get-info-about-our-vm"></a><span data-ttu-id="70768-124">Få information om vår virtuella dator</span><span class="sxs-lookup"><span data-stu-id="70768-124">Get info about our VM</span></span>

1. <span data-ttu-id="70768-125">Kör följande kommando för att hämta egenskaperna för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="70768-125">Run the following command to get back the properties of our VM:</span></span>

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
    ```
    
1. <span data-ttu-id="70768-126">Vi lagrar svaret i variabeln `$myVM`.</span><span class="sxs-lookup"><span data-stu-id="70768-126">We store the response in our `$myVM` variable.</span></span> <span data-ttu-id="70768-127">Vi kan skicka utdata till `select-object`-cmdleten för att filtrera visningen efter specifika egenskaper:</span><span class="sxs-lookup"><span data-stu-id="70768-127">We can pipe the output into the `select-object` cmdlet to filter the display to specific properties:</span></span>

    ```powershell
    $myVM | select-object -property ResourceGroupName, Name, Type, Location
    ```
    
1. <span data-ttu-id="70768-128">Du bör få något som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="70768-128">You should get something like the following.</span></span>

    ```output
    ResourceGroupName Name        Type                              Location
    ----------------- ----        ----                              --------
    <rgn>[sandbox resource group name]</rgn> fotoshareVM Microsoft.Compute/virtualMachines eastus
    ```
    
### <a name="view-os-disk-cache-status"></a><span data-ttu-id="70768-129">Visa status för OS-diskcache</span><span class="sxs-lookup"><span data-stu-id="70768-129">View OS disk cache status</span></span>

1. <span data-ttu-id="70768-130">Vi kan kontrollera inställningen cachelagring via `StorageProfile`-objektet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70768-130">We can check the caching  setting through  the `StorageProfile` object, as follows:</span></span>

    ```powershell
    $myVM.StorageProfile.OsDisk.Caching
    ```

    ```output
    ReadOnly
    ```
   
1. <span data-ttu-id="70768-131">Vi ändrar tillbaka det till standardinställningen för en OS-disk, som är _ReadWrite_.</span><span class="sxs-lookup"><span data-stu-id="70768-131">Let's change it back to the default for an OS disk which is _ReadWrite_.</span></span>

### <a name="change-the-cache-settings-of-the-os-disk"></a><span data-ttu-id="70768-132">Ändra inställningarna för cachelagring för OS-disken</span><span class="sxs-lookup"><span data-stu-id="70768-132">Change the cache settings of the OS disk</span></span>

1. <span data-ttu-id="70768-133">Vi kan ange värdet för cachelagringstyp med hjälp av samma `StorageProfile`-objekt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70768-133">We can set the value for the cache type using the same `StorageProfile` object, as follows:</span></span>

    ```powershell
    $myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
    ```
    
    <span data-ttu-id="70768-134">Det här kommandot körs snabbt, vilket bör innebär att det utför arbete lokalt.</span><span class="sxs-lookup"><span data-stu-id="70768-134">This command runs fast, which should tell you it's doing something locally.</span></span> <span data-ttu-id="70768-135">Kommandot ändrar bara egenskapen för `myVM`-objektet.</span><span class="sxs-lookup"><span data-stu-id="70768-135">The command only changes the property on the `myVM` object.</span></span> <span data-ttu-id="70768-136">Som följande skärmbild visar kommer cachelagringsvärdet inte att ha ändrats på den virtuella datorn om du uppdaterar variabeln `$myVM` genom att tilldela om den med hjälp av `Get-AzureRmVM`-cmdleten.</span><span class="sxs-lookup"><span data-stu-id="70768-136">As the following screenshot shows, if you refresh the `$myVM` variable by reassigning it using the `Get-AzureRmVM` cmdlet, the caching value won't have changed on the VM.</span></span>

1. <span data-ttu-id="70768-137">För att göra ändringen i på själva den virtuella datorn anropar du `Update-AzureRmVM` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="70768-137">To make the change on the VM itself, call `Update-AzureRmVM`, as follows:</span></span>

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
    <span data-ttu-id="70768-138">Observera att det här anropet tar en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="70768-138">Notice that this call takes a while to complete.</span></span> <span data-ttu-id="70768-139">Det beror på att vi uppdaterar den faktiska virtuella datorn, och Azure startar om den virtuella datorn för att göra ändringen.</span><span class="sxs-lookup"><span data-stu-id="70768-139">That's because we're updating the actual VM, and Azure restarts the VM  to make the change.</span></span>

    ```output
    RequestId IsSuccessStatusCode StatusCode ReasonPhrase
    --------- ------------------- ---------- ------------
                             True         OK OK
    ```
    
1. <span data-ttu-id="70768-140">Om du uppdaterar variabeln `$myVM` igen visas ändringen för objektet.</span><span class="sxs-lookup"><span data-stu-id="70768-140">If you refresh the `$myVM` variable again, you'll see the change on the object.</span></span> <span data-ttu-id="70768-141">Om du tittar på disken i portalen ser du ändringen även där.</span><span class="sxs-lookup"><span data-stu-id="70768-141">Looking at the disk in the portal, you'd also see the change there.</span></span> 

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
    $myVM.StorageProfile.OsDisk.Caching
    ```
    
    ```output
    ReadWrite
    ```
    
### <a name="list-data-disk-info"></a><span data-ttu-id="70768-142">Lista information om datadisk</span><span class="sxs-lookup"><span data-stu-id="70768-142">List data disk info</span></span>

1. <span data-ttu-id="70768-143">För att se vilka datadiskar vi har på den virtuella datorn kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="70768-143">To see what data disks we have on our VM, run the following command:</span></span>

    ```powershell
    $myVM.StorageProfile.DataDisks
    ```
    
    ```output
    Name            : fotosharesVM-data
    DiskSizeGB      : 1023
    Lun             : 0
    Caching         : ReadOnly
    CreateOption    : Attach
    SourceImage     :
    VirtualHardDisk :
    ```
    
<span data-ttu-id="70768-144">Vi har endast en datadisk för tillfället.</span><span class="sxs-lookup"><span data-stu-id="70768-144">We have only one data disk at the moment.</span></span> <span data-ttu-id="70768-145">Fältet `Lun` är viktigt.</span><span class="sxs-lookup"><span data-stu-id="70768-145">The `Lun` field is important.</span></span> <span data-ttu-id="70768-146">Det är det unika **L**ogical **U**nit **N**umber (LUN).</span><span class="sxs-lookup"><span data-stu-id="70768-146">It's the unique **L**ogical **U**nit **N**umber.</span></span> <span data-ttu-id="70768-147">När vi lägger till en till datadisk ger vi den ett unikt `Lun`-värde.</span><span class="sxs-lookup"><span data-stu-id="70768-147">When we add another data disk, we'll give it a unique `Lun` value.</span></span>

### <a name="add-a-new-data-disk-to-our-vm"></a><span data-ttu-id="70768-148">Lägga till en ny datadisk till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="70768-148">Add a new data disk to our VM</span></span>

1. <span data-ttu-id="70768-149">För enkelhetens skull sparar vi vårt nya disknamn:</span><span class="sxs-lookup"><span data-stu-id="70768-149">For convenience, we'll store our new disk name:</span></span>

    ```powershell
    $newDiskName = "fotoshareVM-data2"
    ```
    
1. <span data-ttu-id="70768-150">Kör följande `Add-AzureRmVMDataDisk`-kommando för att definiera en ny tom 1 GB-disk:</span><span class="sxs-lookup"><span data-stu-id="70768-150">Run the following `Add-AzureRmVMDataDisk` command to define a new empty 1 GB data disk:</span></span>

    ```powershell
    Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
    ```
    <span data-ttu-id="70768-151">Du får ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="70768-151">You'll get a response like:</span></span>

    ```output
    ResourceGroupName  : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx
    Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxx-xxxxxxx/resourceGroups/<rgn>[sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/fotoshareVM
    VmId               : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
    Name               : fotoshareVM
    Type               : Microsoft.Compute/virtualMachines
    Location           : eastus
    Tags               : {}
    DiagnosticsProfile : {BootDiagnostics}
    HardwareProfile    : {VmSize}
    NetworkProfile     : {NetworkInterfaces}
    OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
    ProvisioningState  : Succeeded
    StorageProfile     : {ImageReference, OsDisk, DataDisks}
        ```
    
1. We've given this disk a `Lun` value of `1` because it's not taken. We defined the disk we want to create, so it's time to run `Update-AzureRmVM` to make the actual change:

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
1. <span data-ttu-id="70768-152">Vi tar en titt på vår datadiskinfo igen:</span><span class="sxs-lookup"><span data-stu-id="70768-152">Let's look at our data disk info again:</span></span>

    ```powershell
    $myVM.StorageProfile.DataDisks
    ```
    
    ```output
    Name            : fotosharesVM-data
    DiskSizeGB      : 1023
    Lun             : 0
    Caching         : ReadOnly
    CreateOption    : Attach
    SourceImage     :
    VirtualHardDisk :
    
    Name            : fotoshareVM-data2
    DiskSizeGB      : 1
    Lun             : 1
    Caching         : None
    CreateOption    : Empty
    SourceImage     :
    VirtualHardDisk :
    ```

<span data-ttu-id="70768-153">Nu har vi två diskar.</span><span class="sxs-lookup"><span data-stu-id="70768-153">We now have two disks.</span></span> <span data-ttu-id="70768-154">Vår nya disk har en `Lun` av `1` och standardvärdet för `Caching` är `None`.</span><span class="sxs-lookup"><span data-stu-id="70768-154">Our new disk has a `Lun` of `1` and the default value for `Caching` is `None`.</span></span> <span data-ttu-id="70768-155">Låt oss ändra det värdet.</span><span class="sxs-lookup"><span data-stu-id="70768-155">Let's change that value.</span></span>

### <a name="change-cache-settings-of-new-data-disk"></a><span data-ttu-id="70768-156">Ändra inställningar för cachelagring för en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="70768-156">Change cache settings of new data disk</span></span>

1. <span data-ttu-id="70768-157">Vi ändrar egenskaperna för en ny virtuell datordatadisk med cmdleten `Set-AzureRmVMDataDisk` enligt följande:</span><span class="sxs-lookup"><span data-stu-id="70768-157">We modify properties of a virtual machine data disk with the `Set-AzureRmVMDataDisk` cmdlet, as follows:</span></span>

    ```powershell
    Set-AzureRmVMDataDisk -VM $myVM -Lun "1" -Caching ReadWrite
    ```
    
1. <span data-ttu-id="70768-158">Som alltid checkar vi in ändringarna med `Update-AzureRmVM`:</span><span class="sxs-lookup"><span data-stu-id="70768-158">As always, commit the changes with `Update-AzureRmVM`:</span></span>

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
<span data-ttu-id="70768-159">Här är en vy från portalen för det vi har gjort i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="70768-159">Here's a view from the portal of what we've accomplished in this exercise.</span></span> <span data-ttu-id="70768-160">Vår virtuella dator har nu två datadiskar, och vi har justerat alla inställningar för **HOST CACHING** (Cachelagring för värd).</span><span class="sxs-lookup"><span data-stu-id="70768-160">Our VM now has two data disks, and we've adjusted all **HOST CACHING** settings.</span></span> <span data-ttu-id="70768-161">Vi gjorde allt detta med bara några få kommandon.</span><span class="sxs-lookup"><span data-stu-id="70768-161">We did all of that with just a few commands.</span></span> <span data-ttu-id="70768-162">Det är kraften hos Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70768-162">That's the power of Azure PowerShell.</span></span>

![Skärmbild av Azure Portal som visar avsnittet Diskar i bladet för den virtuella datorn med två datadiskar.](../media/disks-final-config-portal2.png)
