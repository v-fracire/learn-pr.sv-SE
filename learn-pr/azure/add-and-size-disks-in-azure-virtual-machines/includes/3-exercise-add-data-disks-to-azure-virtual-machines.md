<span data-ttu-id="aa2f2-101">Vi håller på att öka vår arbetsbelastning på vår advokatbyrå och du har fått i uppdrag att etablera en ny Linux-webbserver för att lagra viktiga dokument från många olika källor - klienter, konkurrenter och polismyndigheter.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-101">Our law firm is expanding it's case load and you have been tasked with putting up a new Linux web server to store critical documents from a variety of sources - clients, other law firms, and law enforcement offices.</span></span> <span data-ttu-id="aa2f2-102">Vår server kan acceptera överföringar och måste lagra dem på disken.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-102">Our server can accept uploads and needs to store them on disk.</span></span>

> [!TIP]
> <span data-ttu-id="aa2f2-103">Den här övningen använder Linux som exempel, men den grundläggande processen för att skapa virtuella datorer och lägga till diskar är densamma för Windows.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-103">This exercise uses Linux as the example, but the basic process of creating VMs and adding disks is the same for Windows.</span></span> <span data-ttu-id="aa2f2-104">Den viktigaste skillnaden är partitionering och formatering av disken, vilket skulle ske i en fjärrskrivbordssession med de inbyggda diskhanteringsverktygen.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-104">The primary difference would be in partitioning and formatting the disk - that would be done in a Remote Desktop session using the built-in Disk Management tools.</span></span>

<span data-ttu-id="aa2f2-105">Målet med den här övningen är att skapa en virtuell Linux-dator (VM) och ansluta en ny virtuell hårddisk (VHD) med namnet ”uploadDataDisk1” för att lagra katalogen ”uploads”.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-105">The goal of the exercise is to create a Linux virtual machine (VM) and attach a new virtual hard disk (VHD) named "uploadDataDisk1" to store the "uploads" directory.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm"></a><span data-ttu-id="aa2f2-106">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="aa2f2-106">Create a Linux VM</span></span>

<span data-ttu-id="aa2f2-107">Nu ska vi skapa en virtuell Linux-dator som värd för vår webbserver med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-107">Let's create a Linux VM to host our web server using the Azure CLI.</span></span>

1. <span data-ttu-id="aa2f2-108">Börja med att ställa in vissa standardvärden för den här sessionen.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-108">Start by setting some defaults for this session.</span></span> <span data-ttu-id="aa2f2-109">Det första du måste bestämma är _platsen_ där du vill placera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-109">The first thing you need to decide is the _location_ to place the VM.</span></span> <span data-ttu-id="aa2f2-110">Detta bör vara nära dina klienter.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-110">Ideally this would be close to your clients.</span></span> <span data-ttu-id="aa2f2-111">Välj i så fall den närmaste regionen från platserna som är tillgängliga i Azure-sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-111">In this case, select the closest region from the locations available to the Azure sandbox.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="aa2f2-112">Använd kommandot `az configure` för att ange den förvalda platsen du vill använda.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-112">Use the `az configure` command to set the default location you want to use.</span></span> <span data-ttu-id="aa2f2-113">Ersätt ”eastus” med platsen.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-113">Replace "eastus" with the location.</span></span>

    ```azurecli
    az configure --defaults location=eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="aa2f2-114">Ange standardvärdet för resursgruppen till den förkonfigurerade resursgruppen som skapats för sandbox-miljön för Azure: **<rgn>[Resursgrupp för sandbox-miljö]</rgn>**</span><span class="sxs-lookup"><span data-stu-id="aa2f2-114">Set the default resource group value to the pre-configured resource group created for the Azure sandbox: **<rgn>[sandbox resource group]</rgn>**</span></span>

    ```azurecli
    az configure --defaults group="<rgn>[sandbox Resource Group]</rgn>"
    ```

1. <span data-ttu-id="aa2f2-115">Använd sedan kommandot `vm create` för att skapa en ny virtuell Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-115">Next, use the `vm create` command to create a new Ubuntu Linux VM.</span></span>
    - <span data-ttu-id="aa2f2-116">Ge den namnet ”support-web-vm01”.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-116">Name it "support-web-vm01".</span></span>
    - <span data-ttu-id="aa2f2-117">Ange **storleken** till _Standard_DS2_v2_.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-117">Set the **size** to _Standard_DS2_v2_.</span></span>
    - <span data-ttu-id="aa2f2-118">Ange **admin-username** till ”azureuser” (eller vad du föredrar).</span><span class="sxs-lookup"><span data-stu-id="aa2f2-118">Set the **admin-username** to "azureuser" (or whatever you prefer).</span></span>
    - <span data-ttu-id="aa2f2-119">Generera SSH-nycklar med parametern `--generate-ssh-keys`.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-119">Generate SSH keys with the `--generate-ssh-keys` parameter.</span></span>

    ```azurecli
    az vm create --name support-web-vm01 \
        --image UbuntuLTS \
        --size Standard_DS2_v2 \
        --admin-username azureuser \
        --generate-ssh-keys
    ```
    
    > [!TIP]
    > <span data-ttu-id="aa2f2-120">Det kan ta några minuter att skapa den virtuella datorn och distribuera den i Azure.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-120">Creating your VM and deploying it in Azure can take a few minutes.</span></span> <span data-ttu-id="aa2f2-121">Du kan övervaka förloppet i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-121">You can watch the progress in the Cloud Shell.</span></span>
    
1. <span data-ttu-id="aa2f2-122">Detta resulterar i ett JSON-block med information om den skapade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-122">This will result in a JSON block with the details about the created VM.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/xxx/resourceGroups/<rgn>[sandbox resource group]</rgn>/providers/Microsoft.Compute/virtualMachines/support-web-vm01",
        "location": "eastus",
        "macAddress": "00-0D-3A-18-DE-B4",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "40.76.193.249",
        "resourceGroup": "<rgn>[sandbox resource group]</rgn>",
        "zones": ""
    }
    ```
    <span data-ttu-id="aa2f2-123">Anteckna **publicIpAddress**i utdatan.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-123">Note of the **publicIpAddress** in your output.</span></span> <span data-ttu-id="aa2f2-124">På så vis kan vi komma åt den virtuella datorn via en fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-124">This is how we can access the VM remotely.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa2f2-125">Vi använder den här virtuella datorn för att visa hur diskhantering går till.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-125">We are using this VM to learn how to manage disks.</span></span> <span data-ttu-id="aa2f2-126">Om vi verkligen skulle använda den som webbserver hade vi behövt öppna ytterligare portar med kommandot `az vm open-port` och installera programvara för webbservern.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-126">If we were truly going to use it as a web server, we'd want to open up additional ports with the `az vm open-port` command, and install web server software.</span></span> <span data-ttu-id="aa2f2-127">Dessa uppgifter som ligger utanför omfånget för den här modulen, men du kan gå igenom modulen **Hantera virtuella datorer med Azure CLI** för att lära dig detta.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-127">These tasks are outside the scope of this module, but you can go through the **Manage virtual machines with the Azure CLI** module to learn how to do these steps.</span></span> 

## <a name="add-an-empty-data-disk-to-our-vm"></a><span data-ttu-id="aa2f2-128">Lägga till en tom datadisk till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="aa2f2-128">Add an empty data disk to our VM</span></span>

<span data-ttu-id="aa2f2-129">Vi kommer att kalla den disk som lagrar katalogen ”överföringar” åt din web server för ”uploadDataDisk1”.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-129">We're going to name the disk that stores the "uploads" directory for your web server "uploadDataDisk1".</span></span> 

> [!TIP]
> <span data-ttu-id="aa2f2-130">Du kan lägga till datadiskar när den virtuella datorn skapas med hjälp av parametern `--data-disk-sizes-gb` för kommandot `vm create`.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-130">You can add data disks when the VM is created using the `--data-disk-sizes-gb` parameter on the `vm create` command.</span></span>

1. <span data-ttu-id="aa2f2-131">Lägg till en ny tom disk på servern med kommandot `vm disk attach`.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-131">Add a new empty disk to the server with the `vm disk attach` command.</span></span>
    - <span data-ttu-id="aa2f2-132">Döp den ”uploadDataDisk1”</span><span class="sxs-lookup"><span data-stu-id="aa2f2-132">Name it "uploadDataDisk1"</span></span>
    - <span data-ttu-id="aa2f2-133">Ställ in den på 64 GB.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-133">Set it to be 64 GB.</span></span>
    - <span data-ttu-id="aa2f2-134">Ange **sku** till ”_Premium_LRS_” så vi använder premium storage med lokal redundans.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-134">Set the **sku** to "_Premium_LRS_" so we use premium storage with local redundancy.</span></span>

    ```azurecli
    az vm disk attach \
        --vm-name support-web-vm01 \
        --disk uploadDataDisk1 \
        --size-gb 64 \
        --sku Premium_LRS \
        --new
    ```
    
<span data-ttu-id="aa2f2-135">Vi har nu definierat en disk med namnet ”uploadDataDisk1”.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-135">We've now defined a disk named "uploadDataDisk1" .</span></span> <span data-ttu-id="aa2f2-136">För att använda disken måste vi partitionera och formatera den.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-136">To use the disk, we'll need to partition and format it.</span></span>

## <a name="initialize-data-disks"></a><span data-ttu-id="aa2f2-137">Initiera datadiskar</span><span class="sxs-lookup"><span data-stu-id="aa2f2-137">Initialize data disks</span></span>

<span data-ttu-id="aa2f2-138">Alla ytterligare enheter som du skapar från början måste initieras och formateras.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-138">Any additional drives you create from scratch will need to be initialized and formatted.</span></span> <span data-ttu-id="aa2f2-139">Processen för att gör detta är identisk med en fysisk disk:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-139">The process for doing this is identical to a physical disk:</span></span>

1. <span data-ttu-id="aa2f2-140">Först SSH till Linux-servern med hjälp av den offentliga IP-adress som du fick tillbaka från det ursprungliga svaret när den virtuella datorn skapades.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-140">First, SSH into the Linux server using the public IP address you got back from the original VM creation response.</span></span> <span data-ttu-id="aa2f2-141">Om du inte har det, använder du följande kommando för att hämta IP-adressen:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-141">If you don't have that, you can use the following command to get the IP address:</span></span>

    ```azurecli
    az vm list-ip-addresses -n support-web-vm01
    ```

1. <span data-ttu-id="aa2f2-142">Använda SSH med den offentliga IP-adressen och användarnamnet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-142">Use SSH with the public IP and the username you created.</span></span>

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. <span data-ttu-id="aa2f2-143">Identifiera därefter disken med kommandot `lsblk` för att lista alla blockenheter – här visas dina enheter.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-143">Next, identify the disk with the `lsblk` command to list all the block devices - here you will see your drives.</span></span>

    ```bash
    lsblk
    ```

    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk
    └─sdb1   8:17   0   14G  0 part /mnt
    sr0     11:0    1  628K  0 rom
    sdc      8:32   0   64G  0 disk
    sda      8:0    0   30G  0 disk
    └─sda1   8:1    0   30G  0 part /
    ```

<span data-ttu-id="aa2f2-144">Leta efter disken på 64 GB som vi skapade. Här kan du se att den är **sdc** och ej monterad, eftersom den inte har startats.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-144">Look for the 64 GB drive we created - here you can see it's **sdc** and it's not mounted - this is because it's not been initialized.</span></span>

1. <span data-ttu-id="aa2f2-145">När du känner till enheten (**sdc**) måste du initiera, vilket du kan göra med `fdisk`.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-145">Once you know the drive (**sdc**) you need to initialize, you can use `fdisk` to do that.</span></span> <span data-ttu-id="aa2f2-146">Du måste köra kommandot med `sudo` och ange den disk som du vill partitionera:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-146">You will need to run the command with `sudo` and supply the disk you want to partition:</span></span>

    ```bash
    sudo fdisk /dev/sdc
    ```

1. <span data-ttu-id="aa2f2-147">Lägg till en ny partition genom att använda kommandot <kbd>n</kbd>.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-147">Use the <kbd>n</kbd> command to add a new partition.</span></span> <span data-ttu-id="aa2f2-148">I det här exemplet väljer vi också <kbd>p</kbd> för en primär partition och accepterar resten av standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-148">In this example, we also choose <kbd>p</kbd> for a primary partition and accept the rest of the default values.</span></span> <span data-ttu-id="aa2f2-149">Dina utdata kommer att likna vad du ser i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-149">The output will be similar to the following example:</span></span>   

    ```output
    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0x3b44089f.
    
    Command (m for help): n
    Partition type
       p   primary (0 primary, 0 extended, 4 free)
       e   extended (container for logical partitions)
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-268435455, default 2048):
    Last sector, +sectors or +size{K,M,G,T,P} ...
    Created a new partition 1 of type 'Linux' and of size 64 GiB.    
    ```

1. <span data-ttu-id="aa2f2-150">Skriv ut partitionstabellen med kommandot <kbd>p</kbd>.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-150">Print the partition table with the <kbd>p</kbd> command.</span></span> <span data-ttu-id="aa2f2-151">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-151">It should look something like this:</span></span>

    ```output
    Disk /dev/sdc: 64 GiB ...
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0x3b44089f
    
    Device     Boot Start ... Size Id Type
    /dev/sdc1        2048 ... 64G 83 Linux
    ```

1. <span data-ttu-id="aa2f2-152">Skriv ändringarna med kommandot <kbd>w<kbd>.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-152">Write the changes with the <kbd>w<kbd> command.</span></span> <span data-ttu-id="aa2f2-153">Detta avslutar verktyget.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-153">This will exit the tool.</span></span>

1. <span data-ttu-id="aa2f2-154">Nu måste vi skriva ett filsystem till partitionen med kommandot `mkfs`.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-154">Next, we need to write a file system to the partition with the `mkfs` command.</span></span> <span data-ttu-id="aa2f2-155">Vi måste ange filsystemstypen och enhetsnamnet som vi har fått från `fdisk`-utdatan:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-155">We will need to specify the file system type and device name that we got from the `fdisk` output:</span></span>
    - <span data-ttu-id="aa2f2-156">Skicka `-t ext4` för att skapa ett _ext4_-filsystem.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-156">Pass `-t ext4` to create an _ext4_ filesystem.</span></span>
    - <span data-ttu-id="aa2f2-157">Enhetsnamnet är `/dev/sdc1`.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-157">The device name is `/dev/sdc1`.</span></span>

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    <span data-ttu-id="aa2f2-158">Det kan ta ett par minuter innan kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-158">This command will take a minute or so to complete.</span></span>

    ```output
    mke2fs 1.42.13 (17-May-2015)
    Discarding device blocks: done
    Creating filesystem with 16777088 4k blocks and 4194304 inodes
    ...
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```
    
1. <span data-ttu-id="aa2f2-159">Skapa därefter en katalog som vi ska använda som monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-159">Next, create a directory we will use as our mount point.</span></span> <span data-ttu-id="aa2f2-160">Vi antar att vi har en `uploads`-mapp:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-160">Let's assume we will have a `uploads` folder:</span></span>

    ```bash
    sudo mkdir /uploads
    ```
1. <span data-ttu-id="aa2f2-161">Använd slutligen `mount` för att koppla disken till monteringspunkten:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-161">Finally, use `mount` to attach the disk to the mount point:</span></span>

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```
    <span data-ttu-id="aa2f2-162">Du bör kunna använda `lsblk` för att se den monterade enheten nu:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-162">You should be able to use `lsblk` to see the mounted drive now:</span></span>
    
    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk
    └─sdb1   8:17   0   14G  0 part /mnt
    sr0     11:0    1  628K  0 rom
    sdc      8:32   0   64G  0 disk
    └─sdc1   8:33   0   64G  0 part /uploads
    sda      8:0    0   30G  0 disk
    └─sda1   8:1    0   30G  0 part /
    ```
    
### <a name="mounting-the-drive-automatically"></a><span data-ttu-id="aa2f2-163">Montera enheten automatiskt</span><span class="sxs-lookup"><span data-stu-id="aa2f2-163">Mounting the drive automatically</span></span>

<span data-ttu-id="aa2f2-164">För att säkerställa att enheten monteras automatiskt efter en omstart måste den läggas till i filen `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-164">To ensure that the drive is mounted automatically after a reboot, it must be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="aa2f2-165">Vi rekommenderar även starkt att UUID (Universally Unique Identifier) används i `/etc/fstab` för att referera till enheten i stället för bara enhetsnamnet (t.ex. `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="aa2f2-165">It is also highly recommended that the UUID (universally unique identifier) is used in `/etc/fstab` to refer to the drive rather than just the device name (such as `/dev/sdc1`).</span></span> <span data-ttu-id="aa2f2-166">Om operativsystemet upptäcker ett diskfel vid start och använder UUID undviker du att den felaktiga disken monteras på en viss plats.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-166">If the OS detects a disk error during boot, using the UUID avoids the incorrect disk being mounted to a given location.</span></span> <span data-ttu-id="aa2f2-167">Återstående datadiskar tilldelas sedan samma enhets-ID:n.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-167">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="aa2f2-168">Du kan hitta UUID för den nya enheten med verktyget `blkid`:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-168">To find the UUID of the new drive, use the `blkid` utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="aa2f2-169">Något sådant här returneras:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-169">It will return something like:</span></span>

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. <span data-ttu-id="aa2f2-170">Kopiera UUID för enheten `/dev/sdc1` och öppna filen `/etc/fstab` i en textredigerare:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-170">Copy the UUID for the `/dev/sdc1` drive and open the `/etc/fstab` file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> <span data-ttu-id="aa2f2-171">Felaktig redigering av filen `/etc/fstab` kan leda till att systemet inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-171">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="aa2f2-172">Om du är osäker läser du distributionens dokumentation för att få information om hur du redigerar filen på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-172">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="aa2f2-173">Vi rekommenderar även att en säkerhetskopia av filen skapas innan den redigeras när du arbetar med produktionssystem.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-173">It is also recommended that a backup of the file is created before editing when you are working with production systems.</span></span>

1. <span data-ttu-id="aa2f2-174">Tryck på <kbd>G</kbd> för att gå till den sista raden i filen.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-174">Press <kbd>G</kbd> to move to the last line in the file.</span></span>

1. <span data-ttu-id="aa2f2-175">Tryck på <kbd>I</kbd> för att öppna infogningsläget.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-175">Press <kbd>I</kbd> to enter INSERT mode.</span></span> <span data-ttu-id="aa2f2-176">Läget bör visas längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-176">It should indicate the mode at the bottom of the screen.</span></span>

1. <span data-ttu-id="aa2f2-177">Tryck på <kbd>END</kbd>-tangenten för att gå till slutet på raden.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-177">Press the <kbd>END</kbd> key to move to the end of the line.</span></span> <span data-ttu-id="aa2f2-178">Alternativt kan du använda piltangenterna.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-178">Alternatively, you can use the arrow keys.</span></span> <span data-ttu-id="aa2f2-179">Tryck på <kbd>RETUR</kbd> om du vill gå till en ny rad.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-179">Press <kbd>ENTER</kbd> to move to a new line.</span></span>

1. <span data-ttu-id="aa2f2-180">Skriv följande rad i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-180">Type the following line into the editor.</span></span> <span data-ttu-id="aa2f2-181">Värdena kan avgränsas med blanksteg eller tabb.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-181">The values can be space or tab separated.</span></span> <span data-ttu-id="aa2f2-182">Mer information om varje kolumn finns i dokumentationen:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-182">Check the documentation for more information on each of the columns:</span></span>

    ```output
    UUID=<uuid-goes-here>    /uploads    ext4    defaults,nofail    1    2
    ```
1. <span data-ttu-id="aa2f2-183">Tryck på **ESC** och skriv sedan **:w!**</span><span class="sxs-lookup"><span data-stu-id="aa2f2-183">Press **ESC**, then type **:w!**</span></span> <span data-ttu-id="aa2f2-184">för att skriva filen och **:q** för att avsluta redigeraren.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-184">to write the file and **:q** to quit the editor.</span></span>

1. <span data-ttu-id="aa2f2-185">Slutligen ska vi kontrollera att posten är korrekt genom att be operativsystemet att uppdatera monteringspunkterna:</span><span class="sxs-lookup"><span data-stu-id="aa2f2-185">Finally, let's check to make sure the entry is correct by asking the OS to refresh the mount points:</span></span>

    ```bash
    sudo mount -a
    ```

    <span data-ttu-id="aa2f2-186">Om det returnerar ett fel redigerar du filen för att hitta problemet.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-186">If it returns an error, edit the file to find the problem.</span></span>

> [!TIP]
> <span data-ttu-id="aa2f2-187">Vissa Linux-kärnor stöder TRIM för att ta bort oanvända block på diskar.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-187">Some Linux kernels support TRIM to discard unused blocks on disks.</span></span> <span data-ttu-id="aa2f2-188">Den här funktionen finns på Azure-diskar och kan du kan spara pengar om du skapar stora filer och sedan tar bort dem.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-188">This feature is available on Azure disks and can save you money if you create large files and then delete them.</span></span> <span data-ttu-id="aa2f2-189">I vår dokumentation finns information om hur du [aktiverar den här funktionen](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure).</span><span class="sxs-lookup"><span data-stu-id="aa2f2-189">Learn how to [turn this feature on](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure) in our documentation.</span></span>

<span data-ttu-id="aa2f2-190">Nu när du har sett hur enkelt det är att lägga till diskar på dina virtuella datorer är det dags att titta på de olika typerna av diskar som du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-190">Now that you've seen how easy it is to add disks to your VMs, let's explore a bit more about the types of disks you might create.</span></span> <span data-ttu-id="aa2f2-191">I synnerhet valet mellan Standard- och Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="aa2f2-191">In particular the choice between Standard and Premium storage.</span></span>