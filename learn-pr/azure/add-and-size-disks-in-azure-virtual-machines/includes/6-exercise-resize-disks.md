<span data-ttu-id="7f29d-101">Vi underskattade hur stora en del av uppladdningarna skulle bli, och uppladdningsdisken har nästan slut på utrymme.</span><span class="sxs-lookup"><span data-stu-id="7f29d-101">We underestimated how big some of the uploads would be and our upload disk is running out of space.</span></span> <span data-ttu-id="7f29d-102">Du väljer att dubbla utrymmet och uppgraderar det från 64 GB till 128 GB.</span><span class="sxs-lookup"><span data-stu-id="7f29d-102">You decide to double the space and upgrade it from 64 GB to 128 GB.</span></span>

## <a name="resize-the-data-disk"></a><span data-ttu-id="7f29d-103">Ändra storlek på datadisk</span><span class="sxs-lookup"><span data-stu-id="7f29d-103">Resize the data disk</span></span>

<span data-ttu-id="7f29d-104">För att ändra storleken på en disk behöver vi diskens ID eller namn.</span><span class="sxs-lookup"><span data-stu-id="7f29d-104">To resize a disk, we need the ID or name of the disk.</span></span> <span data-ttu-id="7f29d-105">I det här fallet känner vi redan till namnet (uploadDataDisk1).</span><span class="sxs-lookup"><span data-stu-id="7f29d-105">In this case we already know the name (uploadDataDisk1).</span></span> <span data-ttu-id="7f29d-106">Men om vi inte kommer ihåg det eller om det skapades av någon annan kan vi hämta en lista över diskar med hjälp av kommandot `az disk list`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-106">But in case we didn't remember that, or it was created by someone else we can get a list of disks using the `az disk list` command.</span></span>

1. <span data-ttu-id="7f29d-107">Börja med att hämta en lista över hanterade diskar i resursgruppen. Detta kan inkludera andra diskar om du har flera virtuella datorer i en enda resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f29d-107">Start by getting a list of the managed disks in the resource group; this might include other disks if you have multiple VMs in a single resource group.</span></span> <span data-ttu-id="7f29d-108">I vårt exempel ska det bara finnas vår webbserver.</span><span class="sxs-lookup"><span data-stu-id="7f29d-108">For our example, there should just be our web server.</span></span>

    ```azurecli
    az disk list \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

1. <span data-ttu-id="7f29d-109">Vi stoppar och frigör den virtuella datorn med hjälp av `az vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-109">Next, stop and deallocate the VM using `az vm deallocate`.</span></span> 

    ```azurecli
    az vm deallocate --name support-web-vm01
    ```
1. <span data-ttu-id="7f29d-110">Nu kan vi ändra storlek på disken med kommandot `az disk update`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-110">Now we can resize the disk with the `az disk update` command.</span></span>

    ```azurecli
    az disk update --name uploadDataDisk1 --size-gb 128
    ```
    
1. <span data-ttu-id="7f29d-111">Starta om den virtuella datorn när du utfört storleksändringen.</span><span class="sxs-lookup"><span data-stu-id="7f29d-111">Once the resize operation has completed, restart the VM.</span></span>

    ```azurecli
    az vm start --name support-web-vm01
    ```

## <a name="expand-the-disk-partition"></a><span data-ttu-id="7f29d-112">Expandera diskpartitionen</span><span class="sxs-lookup"><span data-stu-id="7f29d-112">Expand the disk partition</span></span>

<span data-ttu-id="7f29d-113">Det sista steget är att berätta för operativsystemet om det tillgängliga utrymmet.</span><span class="sxs-lookup"><span data-stu-id="7f29d-113">The final step is to tell the OS about the available space.</span></span> <span data-ttu-id="7f29d-114">Den här processen är identisk för expansioner av lokala diskar, precis som de partitionerings- och formateringssteg som vi utförde tidigare.</span><span class="sxs-lookup"><span data-stu-id="7f29d-114">Just like the partitioning and format steps we did earlier, this process is identical for on-premise disk expansions.</span></span> 

1. <span data-ttu-id="7f29d-115">Först hämtar vi den virtuella datorns offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7f29d-115">First, get the public IP address of your VM.</span></span> <span data-ttu-id="7f29d-116">Eftersom den startades om har den förmodligen ändrats.</span><span class="sxs-lookup"><span data-stu-id="7f29d-116">Since it was rebooted, it has likely changed.</span></span> <span data-ttu-id="7f29d-117">Den här gången provar vi en annan metod och använder `az vm show` med ett filter för att returnera den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="7f29d-117">Let's try a different approach this time and use `az vm show` with a filter to return the public IP address.</span></span>

    > [!TIP]
    > <span data-ttu-id="7f29d-118">IP-adresser är dynamiska som standard.</span><span class="sxs-lookup"><span data-stu-id="7f29d-118">IP addresses are dynamic by default.</span></span> <span data-ttu-id="7f29d-119">Azure DNS kompenserar automatiskt för IP-ändringen, eller så kan du ändra beteendet genom att använda statiska IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="7f29d-119">Azure DNS will automatically compensate for the IP change, or you can alter the behavior by using static IP addresses.</span></span>

    ```azurecli
    az vm show --name support-web-vm01 -d --query [publicIps] --o tsv
    ```
    
1. <span data-ttu-id="7f29d-120">SSH till Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="7f29d-120">SSH into the Linux machine.</span></span> <span data-ttu-id="7f29d-121">Du behöver ange rätt IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7f29d-121">You will need to supply your correct IP address.</span></span>

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. <span data-ttu-id="7f29d-122">Avmontera disken.</span><span class="sxs-lookup"><span data-stu-id="7f29d-122">Unmount the disk.</span></span> <span data-ttu-id="7f29d-123">Det gällde alltså `/dev/sdc1`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-123">Recall that it was `/dev/sdc1`.</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

1. <span data-ttu-id="7f29d-124">Starta `parted` i ett upphöjt gränssnitt</span><span class="sxs-lookup"><span data-stu-id="7f29d-124">Launch `parted` in an elevated shell</span></span>

    ```bash
    sudo parted /dev/sdc
    ```
    
1. <span data-ttu-id="7f29d-125">Expandera partitionen med kommandot `resizepart`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-125">Expand the partition with the `resizepart` command.</span></span> <span data-ttu-id="7f29d-126">Ange partition 1 och den nya storleken (128 GB).</span><span class="sxs-lookup"><span data-stu-id="7f29d-126">Enter partition 1 and the new size (128GB).</span></span>

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [64GB]? 128GB
    ```

    > [!WARNING]
    > <span data-ttu-id="7f29d-127">Var försiktig med storleken.</span><span class="sxs-lookup"><span data-stu-id="7f29d-127">Be careful about the size.</span></span> <span data-ttu-id="7f29d-128">När du ändrar storlek på partitionen går det även att minska en partition, vilket förmodligen skulle leda till dataförlust.</span><span class="sxs-lookup"><span data-stu-id="7f29d-128">Resizing the partition allows you to shrink a partition too, and that will likely result in data loss.</span></span>
    
1. <span data-ttu-id="7f29d-129">Avsluta verktyget genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-129">Exit the tool by typing `quit`.</span></span>

1. <span data-ttu-id="7f29d-130">Partitioneringsverktyget _återmonterar_ enheten automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7f29d-130">The partition tool will automatically _remount_ the drive.</span></span> <span data-ttu-id="7f29d-131">Avmontera den därför igen så att den kan formateras.</span><span class="sxs-lookup"><span data-stu-id="7f29d-131">So unmount it again so we can format it.</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```
    
1. <span data-ttu-id="7f29d-132">Verifiera partitionskonsekvensen med `e2fsck`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-132">Verify the partition consistency with `e2fsck`.</span></span> <span data-ttu-id="7f29d-133">Det här steget är absolut nödvändigt, men det är en bra idé närhelst du ändrar storlek på en diskvolym.</span><span class="sxs-lookup"><span data-stu-id="7f29d-133">This step is absolutely necessary but is a good idea any time you are changing sizes on a disk volume.</span></span>

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. <span data-ttu-id="7f29d-134">Ändra storlek på filsystemet med `resize2fs`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-134">Resize the filesystem with `resize2fs`.</span></span>

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. <span data-ttu-id="7f29d-135">Slutligen monterar du tillbaka enheten på monteringspunkten.</span><span class="sxs-lookup"><span data-stu-id="7f29d-135">Finally, mount the drive back to the mount point.</span></span>

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```

<span data-ttu-id="7f29d-136">Verifiera att operativsystemdisken har bytt storlek genom att använda `df -h`.</span><span class="sxs-lookup"><span data-stu-id="7f29d-136">To verify the OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="7f29d-137">Det bör nu stå att enheten är på 128 GB.</span><span class="sxs-lookup"><span data-stu-id="7f29d-137">It should now show that the drive is 128 GB.</span></span>
