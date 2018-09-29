<span data-ttu-id="95824-101">Azure lagrar dina VHD-avbildningar som sidblobar på ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="95824-101">Azure stores your VHD images as page blobs in an Azure Storage account.</span></span> <span data-ttu-id="95824-102">Med hanterade diskar tar Azure hand om hanteringen av lagringsutrymmet åt dig – det är en av de bästa anledningarna att välja hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="95824-102">With managed disks, Azure takes care of managing the storage on your behalf - it's one of the best reasons to choose managed disks.</span></span>

<span data-ttu-id="95824-103">När du skapar en virtuell dator väljer den en storlek för OS-disken.</span><span class="sxs-lookup"><span data-stu-id="95824-103">When you create the VM, it chooses a size for the OS disk.</span></span> <span data-ttu-id="95824-104">Den angivna storleken baseras på den bild du väljer.</span><span class="sxs-lookup"><span data-stu-id="95824-104">The specific size is based on the image you select.</span></span> <span data-ttu-id="95824-105">På Linux är det ofta runt 30 GB och på Windows cirka 127 GB.</span><span class="sxs-lookup"><span data-stu-id="95824-105">On Linux, it's often around 30 GB, and on Windows about 127 GB.</span></span>

<span data-ttu-id="95824-106">Du kan lägga till datadiskar för att ge ytterligare lagringsutrymme men du kan också vilja expandera en befintlig disk – kanske kan ett äldre program inte dela sina data på olika enheter eller du kanske migrerar en fysisk dators disk till Azure och behöver en större OS-enhet.</span><span class="sxs-lookup"><span data-stu-id="95824-106">You can add data disks to provide for additional storage space, but you may also wish to expand an existing disk - perhaps a legacy application cannot split its data across drives, or you are migrating a physical PC's drive to Azure and need a larger OS drive.</span></span>

> [!NOTE]
> <span data-ttu-id="95824-107">Du kan bara ändra storleken till _större_.</span><span class="sxs-lookup"><span data-stu-id="95824-107">You can only resize a disk to a _larger_ size.</span></span> <span data-ttu-id="95824-108">Just nu stöds inte att minska storleken på hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="95824-108">Shrinking managed disks is not supported today.</span></span>

<span data-ttu-id="95824-109">Om du ändrar storleken på disken kan det även ändra diskens nivå (t.ex. från P10 till P20).</span><span class="sxs-lookup"><span data-stu-id="95824-109">Changing the size of the disk can also change the level of the disk (for example from P10 to P20).</span></span> <span data-ttu-id="95824-110">Ha detta i åtanke – det kan vara fördelaktigt för prestandauppgraderingar men det kostar också mer när du uppgraderar till premiumnivåerna.</span><span class="sxs-lookup"><span data-stu-id="95824-110">Keep this in mind - this can be beneficial for performance upgrades, but will also cost more as you move up the premium tiers.</span></span>

## <a name="vm-size-vs-disk-size"></a><span data-ttu-id="95824-111">VM-storlek jämfört med diskstorlek</span><span class="sxs-lookup"><span data-stu-id="95824-111">VM size vs. Disk size</span></span>

<span data-ttu-id="95824-112">Den VM-storlek du väljer när du skapar den virtuella datorn avgör hur många resurser som kan tilldelas.</span><span class="sxs-lookup"><span data-stu-id="95824-112">The VM size you choose when you create your VM will determine how many resources it can allocate.</span></span> <span data-ttu-id="95824-113">För lagringsutrymmet styr storleken antalet diskar du kan lägga till på den virtuella datorn och maxstorleken för varje disk.</span><span class="sxs-lookup"><span data-stu-id="95824-113">For storage, the size will control the number of disks you can add to the VM and the max size of each disk.</span></span> 

<span data-ttu-id="95824-114">Som nämndes i den tidigare kursdelen stöder vissa VM-storlekar bara standardlagringsenheter – med begränsad I/O-prestanda.</span><span class="sxs-lookup"><span data-stu-id="95824-114">As mentioned in the previous unit, some VM sizes only support Standard storage drives - limiting the I/O performance.</span></span>

<span data-ttu-id="95824-115">Om du upptäcker att du behöver mer lagring än vad storleken på den virtuella datorn tillåter kan du ändra dess storlek.</span><span class="sxs-lookup"><span data-stu-id="95824-115">If you find that you need more storage than what your VM size allows for, you can change the VM size.</span></span> <span data-ttu-id="95824-116">Vi behandlar det ämnet i modulen **Introduktion till Azure Virtual Machines**.</span><span class="sxs-lookup"><span data-stu-id="95824-116">We cover that topic in the **Introduction to Azure Virtual Machines** module.</span></span>

## <a name="expanding-a-disk-using-the-azure-cli"></a><span data-ttu-id="95824-117">Expandera en disk med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="95824-117">Expanding a disk using the Azure CLI</span></span>

> [!WARNING]
> <span data-ttu-id="95824-118">Se alltid till att du säkerhetskopierar dina data innan du utför en storleksändring!</span><span class="sxs-lookup"><span data-stu-id="95824-118">Always make sure that you back up your data before performing disk resize operations!</span></span>

<span data-ttu-id="95824-119">Åtgärder på virtuella hårddiskar kan inte utföras när den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="95824-119">Operations on VHDs cannot be performed with the VM running.</span></span> <span data-ttu-id="95824-120">Det första steget är att stoppa och frigöra den virtuella datorn med `az vm deallocate`, med den virtuella datorns och resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="95824-120">The first step is to stop and deallocate the VM with `az vm deallocate` supplying the VM name and resource group name.</span></span>

<span data-ttu-id="95824-121">Att frigöra en virtuell dator, till skillnad från att bara _stoppa_ en virtuell dator frigör de associerade datorresurserna och låter Azure göra konfigurationsändringar av den virtualiserade maskinvaran.</span><span class="sxs-lookup"><span data-stu-id="95824-121">Deallocating a VM, unlike just _stopping_ a VM releases the associated computing resources and allows Azure to make configuration changes to the virtualized hardware.</span></span>

```azurecli
az vm deallocate --resource-group <resource-group-name> --name <vm-name>
```

<span data-ttu-id="95824-122">För att ändra storlek på en disk använder du sedan `az disk update`, vilket överför disknamnet, resursgruppens namn och den nya begärda storleken.</span><span class="sxs-lookup"><span data-stu-id="95824-122">Next, to resize a disk, you use `az disk update`, passing the disk name, resource group name, and newly requested size.</span></span> <span data-ttu-id="95824-123">När du expanderar en hanterad disk mappas den angivna storleken till den närmaste diskstorleken.</span><span class="sxs-lookup"><span data-stu-id="95824-123">When you expand a managed disk, the specified size is mapped to the nearest managed disk size.</span></span>

```azurecli
az disk update \
    --resource-group <resource-group-name> \
    --name <disk-name> \
    --size-gb 200
```

<span data-ttu-id="95824-124">Starta slutligen den virtuella datorn igen med `az vm start`:</span><span class="sxs-lookup"><span data-stu-id="95824-124">Finally, you start the VM again with `az vm start`:</span></span>

```azurecli
az vm start --resource-group <resource-group-name> --name <vm-name>
```

## <a name="expanding-a-disk-using-the-azure-portal"></a><span data-ttu-id="95824-125">Expandera en disk med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="95824-125">Expanding a disk using the Azure portal</span></span>

<span data-ttu-id="95824-126">Att expandera en disk med Azure Portal är ännu enklare.</span><span class="sxs-lookup"><span data-stu-id="95824-126">Expanding a disk using the Azure portal is even easier.</span></span>

1. <span data-ttu-id="95824-127">Stoppa den virtuella datorn med knappen **Stopp** i verktygsfältet i **översiktsvyn** på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="95824-127">Stop the VM using the **Stop** button in the toolbar on the **Overview** view of the VM.</span></span>

1. <span data-ttu-id="95824-128">Klicka på **Diskar** i avsnittet **Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="95824-128">Click **Disks** in the **Settings** section.</span></span>

1. <span data-ttu-id="95824-129">Välj den datadisk du vill ändra storlek på.</span><span class="sxs-lookup"><span data-stu-id="95824-129">Select the data disk you want to resize.</span></span>

    ![Skärmbild som visar avsnittet med diskar på en virtuell dator med den virtuella hårddisk som vi vill redigera markerad](../media/5-portal-disks.png)

1. <span data-ttu-id="95824-131">Ange en storlek _större_ än den aktuella storleken i diskinformationen.</span><span class="sxs-lookup"><span data-stu-id="95824-131">In the disk details, type a size _larger_ than the current size.</span></span> <span data-ttu-id="95824-132">Du kan även ändra från Premium till Standard (eller tvärtom) här.</span><span class="sxs-lookup"><span data-stu-id="95824-132">You can also change from Premium to Standard (or vice-versa) here.</span></span> <span data-ttu-id="95824-133">De här inställningarna justerar dina prestanda, som visas i avsnittet för förväntade IOPS.</span><span class="sxs-lookup"><span data-stu-id="95824-133">These settings will adjust your performance as shown in the predicted IOPS section.</span></span>

    ![Skärmbild som visar den virtuella hårddiskens redigeringsskärm med fältet för ny storlek markerat](../media/5-resize-disk.png)

1. <span data-ttu-id="95824-135">Klicka på **Spara** för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="95824-135">Click **Save** to save the changes.</span></span>

1. <span data-ttu-id="95824-136">Starta om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="95824-136">Restart the VM.</span></span>


### <a name="expanding-the-partition"></a><span data-ttu-id="95824-137">Expandera partitionen</span><span class="sxs-lookup"><span data-stu-id="95824-137">Expanding the partition</span></span>

<span data-ttu-id="95824-138">Precis som när du lägger till en ny datadisk lägger en expanderad disk inte till något användbart utrymme förrän du expanderar partitionen och filsystemet.</span><span class="sxs-lookup"><span data-stu-id="95824-138">Just like adding a new data disk, an expanded disk won't add any usable space until you expand the partition and filesystem.</span></span> <span data-ttu-id="95824-139">Det här måste göras med OS-verktygen som är tillgängliga på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="95824-139">This must be done using the OS tools available to the VM.</span></span> 

<span data-ttu-id="95824-140">I Windows skulle vi använda verktyget Disk Manager eller kommandoradsverktyget `diskpart`.</span><span class="sxs-lookup"><span data-stu-id="95824-140">On Windows, we would use the Disk Manager tool or the `diskpart` command line tool.</span></span>

<span data-ttu-id="95824-141">I Linux använder du `parted` och `resize2fs`.</span><span class="sxs-lookup"><span data-stu-id="95824-141">On Linux, you will use `parted` and `resize2fs`.</span></span>

<span data-ttu-id="95824-142">Vi provar några av dess kommandon.</span><span class="sxs-lookup"><span data-stu-id="95824-142">Let's try out some of these commands.</span></span>