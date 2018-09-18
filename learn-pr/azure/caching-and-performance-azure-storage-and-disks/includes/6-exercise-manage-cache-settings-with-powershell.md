
<span data-ttu-id="3d9ea-101">I den föregående övningen utförde vi följande uppgifter med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-101">In the previous exercise, we performed the following tasks using the Azure portal.</span></span>

- <span data-ttu-id="3d9ea-102">Visa status för OS-diskcache</span><span class="sxs-lookup"><span data-stu-id="3d9ea-102">View OS disk cache status</span></span>
- <span data-ttu-id="3d9ea-103">Ändra inställningarna för cachelagring för OS-disken</span><span class="sxs-lookup"><span data-stu-id="3d9ea-103">Change the cache settings of the OS disk</span></span>
- <span data-ttu-id="3d9ea-104">Lägga till en datadisk i en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3d9ea-104">Add a data disk to the VM</span></span>
- <span data-ttu-id="3d9ea-105">Ändra typ av cachelagring på en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="3d9ea-105">Change caching type on a new data disk</span></span>

<span data-ttu-id="3d9ea-106">Nu övar vi på dessa åtgärder med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-106">Let's practice these operations using Azure PowerShell.</span></span> <span data-ttu-id="3d9ea-107">Vi använder den virtuella dator som vi skapade i den föregående övningen.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-107">We're going to use the VM we created in the previous exercise.</span></span> <span data-ttu-id="3d9ea-108">Åtgärderna i den här labben förutsätter att:</span><span class="sxs-lookup"><span data-stu-id="3d9ea-108">The operations in this lab assume:</span></span>

- <span data-ttu-id="3d9ea-109">Vår virtuella dator finns och heter **fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="3d9ea-109">Our VM exists and is called **fotoshareVM**</span></span>
- <span data-ttu-id="3d9ea-110">Vår virtuella dator finns i en resursgrupp som heter **fotoshare-rg**</span><span class="sxs-lookup"><span data-stu-id="3d9ea-110">Our VM lives in a resource group called **fotoshare-rg**</span></span>

<span data-ttu-id="3d9ea-111">Om du har använt andra namn ersätter du bara de här värdena med dina.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-111">If you've gone with a different set of names, just replace these values with yours.</span></span> 

<span data-ttu-id="3d9ea-112">Här är det aktuella tillståndet för våra VM-diskar från föregående övning.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-112">Here's the current state of our VM disks from the last exercise.</span></span> 

![Skärmbild på våra OS- och datadiskar som båda är inställda på skrivskyddad cachelagring.](../media-draft/disks-final-config-portal.PNG)

<span data-ttu-id="3d9ea-114">Vi har använt portalen för att ange fältet \***HOST CACHING** (Cachelagring för värd) för både för OS- och datadiskar.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-114">We used the portal to set the \***HOST CACHING** field for both the OS and data disks.</span></span> <span data-ttu-id="3d9ea-115">Tänk på detta inledande tillstånd när vi går igenom följande steg.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-115">Keep this initial state in mind as we work through the following steps.</span></span> 

### <a name="set-up-some-variables"></a><span data-ttu-id="3d9ea-116">Konfigurera några variabler</span><span class="sxs-lookup"><span data-stu-id="3d9ea-116">Set up some variables</span></span>
<span data-ttu-id="3d9ea-117">Först ska vi lagra vissa resursnamn så att vi kan använda dem senare.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-117">First, let's  store some resource names so we can use them later.</span></span>

<span data-ttu-id="3d9ea-118">Använd Azure Cloud Shell-terminalen till höger för att köra följande Powershell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-118">Use the Azure Cloud Shell terminal on the right to run the following Powershell commands.</span></span> 

```powershell
$myRgName = "fotoshare-rg"
$myVMName = "fotoshareVM"
```

> [!TIP]
> <span data-ttu-id="3d9ea-119">Du måste ange dessa variabler igen om tidsgränsen uppnås för Cloud Shell-sessionen. Om möjligt bör du därför gå igenom hela den labben i en enda session.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-119">You'll have to set these variables again if your Cloud Shell session times out. So, if possible, work through this entire lab in a single session.</span></span> 

### <a name="get-info-about-our-vm"></a><span data-ttu-id="3d9ea-120">Få information om vår virtuella dator</span><span class="sxs-lookup"><span data-stu-id="3d9ea-120">Get info about our VM</span></span>

<span data-ttu-id="3d9ea-121">Kör följande kommando för att hämta egenskaperna för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-121">Run the following command to get back the properties of our VM.</span></span>
 
```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVMName
```
<span data-ttu-id="3d9ea-122">Vi lagrar svaret i variabeln `$myVM`.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-122">We store the response in our `$myVM` variable.</span></span> <span data-ttu-id="3d9ea-123">Vi kan köra följande kommando för att bara visa de egenskaper som vi anger här.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-123">We can run the following command to just show us the properties we specify here.</span></span>

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

<span data-ttu-id="3d9ea-124">Som följande skärmbild visar är den här virtuella datorn verkligen den virtuella dator som vi letar efter.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-124">As the following screenshot shows, this VM is indeed the VM we're after.</span></span> <span data-ttu-id="3d9ea-125">Därför går vi vidare.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-125">So, let's move on.</span></span> 

![PowerShell-konsolen som visar resultatet av senaste 4 kommandona som vi körde.](../media-draft/ps-commands-1.PNG)

### <a name="view-os-disk-cache-status"></a><span data-ttu-id="3d9ea-127">Visa status för OS-diskcache</span><span class="sxs-lookup"><span data-stu-id="3d9ea-127">View OS disk cache status</span></span>

<span data-ttu-id="3d9ea-128">Vi kan kontrollera inställningen cachelagring via `StorageProfile`-objektet på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-128">We can check the caching  setting through  the `StorageProfile` object as follows.</span></span>

```powershell
$myVM.StorageProfile.OsDisk.Caching
```
<span data-ttu-id="3d9ea-129">I det här exemplet är det aktuella värdet **Inget**.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-129">In this example, the current value is **None**.</span></span> <span data-ttu-id="3d9ea-130">Vi ändrar tillbaka det till standardinställningen för en OS-disk.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-130">Let's change it back to the default for an OS disk.</span></span>

![PowerShell-konsolen som visar vår OS-disk med cachelagringsvärdet ”Inget”.](../media-draft/ps-oscaching-none.PNG)

### <a name="change-the-cache-settings-of-the-os-disk"></a><span data-ttu-id="3d9ea-132">Ändra inställningarna för cachelagring för OS-disken</span><span class="sxs-lookup"><span data-stu-id="3d9ea-132">Change the cache settings of the OS disk</span></span>

<span data-ttu-id="3d9ea-133">Vi kan ange värdet för cachelagringstyp med hjälp av samma StorageProfile-objekt på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-133">We can set the value for the cache type using the same StorageProfile\` object as follows.</span></span>
 
```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

<span data-ttu-id="3d9ea-134">Det här kommandot körs snabbt, vilket bör innebär att det utför arbete lokalt.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-134">This command runs fast, which should tell you it's doing something locally.</span></span> <span data-ttu-id="3d9ea-135">Kommandot ändrar bara egenskapen för myVM-objektet.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-135">The command just changes the property on the myVM object.</span></span> <span data-ttu-id="3d9ea-136">Som följande skärmbild visar kommer cachelagringsvärdet inte att ha ändrats på den virtuella datorn om du uppdaterar variabeln `$myVM`.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-136">As the following screenshot shows, if you refresh the `$myVM` variable,  the caching value won't have changed on the VM.</span></span>

![PowerShell-konsolen som visar att när ”myVM”-objektet uppdateras så återställs cachelagringsvärdet till ”Inget” eftersom vi i själva verket inte uppdaterade den virtuella datorn.](../media-draft/ps-commands-2.PNG)

<span data-ttu-id="3d9ea-138">För att göra ändringen i på själva den virtuella datorn anropar du `Update-AzureRmVM` på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-138">To  make the change on the VM itself, call `Update-AzureRmVM` as follows.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="3d9ea-139">Observera att det här anropet tar en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-139">Notice that this call takes a while to complete.</span></span> <span data-ttu-id="3d9ea-140">Det beror på att vi uppdaterar den faktiska virtuella datorn, och Azure startar om den virtuella datorn för att göra ändringen.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-140">That's because we're updating the actual VM and Azure restarts the VM  to make the change.</span></span>

![PowerShell-konsolen som visar vår OS-disk med cachelagringsvärdet ”Inget”.](../media-draft/ps-oscaching-rw.PNG)

<span data-ttu-id="3d9ea-142">Om du uppdaterar variabeln `$myVM` igen visas ändringen för objektet.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-142">If you refresh the `$myVM` variable again, you'll see the change on the object.</span></span> <span data-ttu-id="3d9ea-143">Om du tittar på disken i portalen ser du ändringen även där.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-143">Looking at the disk in the portal, you'd also see the change there.</span></span> <span data-ttu-id="3d9ea-144">Nu går vi vidare till att skapa en ny datadisk.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-144">Ok, let's move on to creating a new data disk.</span></span>  

### <a name="list-data-disk-info"></a><span data-ttu-id="3d9ea-145">Lista information om datadisk</span><span class="sxs-lookup"><span data-stu-id="3d9ea-145">List data disk info</span></span>

<span data-ttu-id="3d9ea-146">För att se vilka datadiskar vi har på den virtuella datorn kör du följande kommando.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-146">To see what data disks we have on our VM, run the following command.</span></span> 

```powershell
$myVM.StorageProfile.DataDisks
```

<span data-ttu-id="3d9ea-147">Vi har endast en datadisk för tillfället.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-147">We've only one data disk at the moment.</span></span> <span data-ttu-id="3d9ea-148">Fältet `Lun` är viktigt.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-148">The `Lun` field is important.</span></span> <span data-ttu-id="3d9ea-149">Det är det unika **L**ogical **U**nit **N**umber (LUN).</span><span class="sxs-lookup"><span data-stu-id="3d9ea-149">It's the unique **L**ogical **U**nit **N**umber.</span></span> <span data-ttu-id="3d9ea-150">När vi lägger till en till datadisk ger vi den ett unikt `Lun`-värde.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-150">When we add another data disk, we'll give it a unique `Lun` value.</span></span> 

### <a name="add-a-new-data-disk-to-our-vm"></a><span data-ttu-id="3d9ea-151">Lägga till en ny datadisk till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="3d9ea-151">Add a new data disk to our VM</span></span> 

<span data-ttu-id="3d9ea-152">För enkelhetens skull sparar vi vårt nya disknamn.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-152">For convenience, we'll store our new disk name.</span></span>

```powershell
$newDiskName = "fotoshareVM-data2"
```

<span data-ttu-id="3d9ea-153">Kör följande `Add-AzureRmVMDataDisk`-kommando för att definiera en ny disk.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-153">Run the following `Add-AzureRmVMDataDisk` command to define a new disk.</span></span> 

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

<span data-ttu-id="3d9ea-154">Vi har gett den här disken LUN-värdet 1 eftersom det inte är upptaget.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-154">We've given this disk a Lun value of 1, since that's not taken.</span></span> <span data-ttu-id="3d9ea-155">Vi har definierat den disk som vi vill skapa, så det är dags att köra `Update-AzureRmVM` för att göra den faktiska ändringen.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-155">We defined the disk we want to create, so it's time to run `Update-AzureRmVM` to make the actual change.</span></span> 

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="3d9ea-156">Vi tar en titt på vår datadiskinfo igen.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-156">Let's look at our data disk info again.</span></span>

```powershell
$myVM.StorageProfile.DataDisks
```

![PowerShell-konsolen som visar våra två datadiskar.](../media-draft/2-data-disks-part1.png)

<span data-ttu-id="3d9ea-158">Nu har vi två diskar.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-158">We now have two disks.</span></span> <span data-ttu-id="3d9ea-159">Vår nya disk har **LUN**-värdet 1 och standardvärdet för **Cachelagring**, som är **Inget**.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-159">Our new disk has a **Lun** of 1 and the default value for **Caching** is **None**.</span></span> <span data-ttu-id="3d9ea-160">Låt oss ändra det värdet.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-160">Let's change that value.</span></span>

### <a name="change-cache-settings-of-new-data-disk"></a><span data-ttu-id="3d9ea-161">Ändra inställningar för cachelagring för en ny datadisk</span><span class="sxs-lookup"><span data-stu-id="3d9ea-161">Change cache settings of new data disk</span></span>

<span data-ttu-id="3d9ea-162">Vi ändrar egenskaperna för en ny virtuell datordatadisk med cmdleten `Set-AzureRmVMDataDisk` enligt följande.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-162">We modify properties of a virtual machine data disk with the `Set-AzureRmVMDataDisk` cmdlet as follows.</span></span>

```powershell
Set-AzureRmVMDataDisk -Lun "1" -Caching ReadWrite
```

<span data-ttu-id="3d9ea-163">Som alltid checkar vi in ändringarna med `Update-AzureRmVM`.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-163">As always, commit the changes with `Update-AzureRmVM`.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="3d9ea-164">Här är en vy från portalen för det vi har gjort i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-164">Here's a view from the portal of what we've accomplished in this exercise.</span></span> <span data-ttu-id="3d9ea-165">Vår virtuella dator har nu två datadiskar, och vi har justerat alla inställningar för **HOST Caching** (Cachelagring för värd).</span><span class="sxs-lookup"><span data-stu-id="3d9ea-165">Our VM now has two data disks and we've adjust all **HOST Caching** settings.</span></span> <span data-ttu-id="3d9ea-166">Vi gjorde allt detta med bara några få kommandon.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-166">We did all of that with just a few commands.</span></span> <span data-ttu-id="3d9ea-167">Det är kraften hos Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3d9ea-167">That's the power of Azure PowerShell.</span></span>

![PowerShell-konsolen som visar våra två datadiskar.](../media-draft/disks-final-config-portal2.png)
