<span data-ttu-id="42f4a-101">Vår virtuella Linux-dator har distribuerats och körs men den har inte konfigurerats att utföra arbete.</span><span class="sxs-lookup"><span data-stu-id="42f4a-101">We have our Linux VM deployed and running, but it's not configured to do any work.</span></span> <span data-ttu-id="42f4a-102">Nu ska vi ansluta till den med SSH och konfigurera Apache, så att vi har en aktiv server.</span><span class="sxs-lookup"><span data-stu-id="42f4a-102">Let's connect to it with SSH and configure Apache, so we have a running web server.</span></span>

## <a name="connect-to-the-vm-with-ssh"></a><span data-ttu-id="42f4a-103">Ansluta till den virtuella datorn med SSH</span><span class="sxs-lookup"><span data-stu-id="42f4a-103">Connect to the VM with SSH</span></span>

<span data-ttu-id="42f4a-104">Om du vill ansluta till en virtuell Azure-dator med en SSH-klient behöver du:</span><span class="sxs-lookup"><span data-stu-id="42f4a-104">To connect to an Azure VM with an SSH client, you will need:</span></span>

- <span data-ttu-id="42f4a-105">SSH-klientprogramvaran (finns på de flesta moderna operativsystem)</span><span class="sxs-lookup"><span data-stu-id="42f4a-105">SSH client software (present on most modern operating systems)</span></span>
- <span data-ttu-id="42f4a-106">Den offentliga IP-adressen för den virtuella datorn (eller privat om den virtuella datorn är konfigurerad för att ansluta till nätverket)</span><span class="sxs-lookup"><span data-stu-id="42f4a-106">The public IP address of the VM (or private if the VM is configured to connect to your network)</span></span>

### <a name="get-the-public-ip-address"></a><span data-ttu-id="42f4a-107">Hämta den offentliga IP-adressen</span><span class="sxs-lookup"><span data-stu-id="42f4a-107">Get the public IP address</span></span>

1. <span data-ttu-id="42f4a-108">Se till att [översiktspanelen](https://portal.azure.com?azure-portal=true) för den virtuella dator du skapade tidigare är öppen i **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="42f4a-108">In the [Azure portal](https://portal.azure.com?azure-portal=true), ensure the **Overview** panel for the virtual machine that you created earlier is open.</span></span> <span data-ttu-id="42f4a-109">Du hittar den virtuella datorn under **Alla resurser** om du behöver öppna den.</span><span class="sxs-lookup"><span data-stu-id="42f4a-109">You can find the VM under **All Resources** if you need to open it.</span></span> <span data-ttu-id="42f4a-110">Översiktspanelen har mycket information om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42f4a-110">The overview panel has a lot of information about the VM.</span></span>

    - <span data-ttu-id="42f4a-111">Du kan se om den virtuella datorn körs</span><span class="sxs-lookup"><span data-stu-id="42f4a-111">You can see whether the VM is running</span></span>
    - <span data-ttu-id="42f4a-112">Stoppa eller starta om den</span><span class="sxs-lookup"><span data-stu-id="42f4a-112">Stop or restart it</span></span>
    - <span data-ttu-id="42f4a-113">Hämta den offentliga IP-adressen för att ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="42f4a-113">Get the public IP address to connect to the VM</span></span>
    - <span data-ttu-id="42f4a-114">Visa aktiviteten för CPU, disk och nätverk</span><span class="sxs-lookup"><span data-stu-id="42f4a-114">See the activity of the CPU, disk, and network</span></span>

1. <span data-ttu-id="42f4a-115">Klicka på knappen **Anslut** högst upp i fönstret.</span><span class="sxs-lookup"><span data-stu-id="42f4a-115">Click the **Connect** button at the top of the pane.</span></span>

1. <span data-ttu-id="42f4a-116">På bladet **Anslut till virtuell dator** noterar du inställningarna för **IP-adress** och **portnummer**.</span><span class="sxs-lookup"><span data-stu-id="42f4a-116">In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings.</span></span> <span data-ttu-id="42f4a-117">På fliken **SSH** fliken finns också kommandot som du behöver köra lokalt för att ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42f4a-117">On the **SSH** tab, you will also find the command you need to execute locally to connect to the VM.</span></span> <span data-ttu-id="42f4a-118">Kopiera detta till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="42f4a-118">Copy this to the clipboard.</span></span>

<!-- TODO: this will be necessary if we ever have inline portal integration 

### Open the Azure Cloud Shell

Let's use the Cloud Shell in the Azure Portal. If you generated the SSH key locally, you need to use your local session since the private key won't be in your storage account.

1. Switch back to the **Dashboard** by clicking the Dashboard button in the Azure sidebar.

1. Open the Cloud Shell by clicking the shell button in the top toolbar.

    ![Open the Azure Cloud Shell](../media-drafts/6-cloud-shell.png)

1. Select **Bash** as the shell type. PowerShell is also available if you are a Windows administrator.

    ![Select bash shell in the portal](../media-drafts/6-use-bash-shell.png)

-->

## <a name="connect-with-ssh"></a><span data-ttu-id="42f4a-119">Ansluta med SSH</span><span class="sxs-lookup"><span data-stu-id="42f4a-119">Connect with SSH</span></span>

1. <span data-ttu-id="42f4a-120">Klistra in den kommandorad som du fick från SSH-fliken i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="42f4a-120">Paste the command line you got from the SSH tab into the Cloud Shell.</span></span> <span data-ttu-id="42f4a-121">Det bör se ut ungefär så här. Men den har en annan IP-adress (och kanske ett annat användarnamn om du inte använde **jim**!)</span><span class="sxs-lookup"><span data-stu-id="42f4a-121">It should look something like this; however it will have a different IP address (and perhaps a different username if you didn't use **jim**!)</span></span>

    ```bash
    ssh jim@137.117.101.249
    ```

1. <span data-ttu-id="42f4a-122">Det här kommandot öppnar en SSH-anslutning och placerar dig i en traditionell skalkommandotolk för Linux.</span><span class="sxs-lookup"><span data-stu-id="42f4a-122">This command will open a secure shell connection and place you at a traditional shell command prompt for Linux.</span></span>

1. <span data-ttu-id="42f4a-123">Prova att utföra några Linux-kommandon</span><span class="sxs-lookup"><span data-stu-id="42f4a-123">Try executing a few Linux commands</span></span>
    - <span data-ttu-id="42f4a-124">`ls -la /` för att visa diskens rot</span><span class="sxs-lookup"><span data-stu-id="42f4a-124">`ls -la /` to show the root of the disk</span></span>
    - <span data-ttu-id="42f4a-125">`ps -l` för att visa alla processer som körs</span><span class="sxs-lookup"><span data-stu-id="42f4a-125">`ps -l` to show all the running processes</span></span>
    - <span data-ttu-id="42f4a-126">`dmesg` för att lista alla kernelmeddelanden</span><span class="sxs-lookup"><span data-stu-id="42f4a-126">`dmesg` to list all the kernel messages</span></span>
    - <span data-ttu-id="42f4a-127">`lsblk` för att lista alla blockenheter – här ser du dina enheter</span><span class="sxs-lookup"><span data-stu-id="42f4a-127">`lsblk` to list all the block devices - here you will see your drives</span></span>

<span data-ttu-id="42f4a-128">Mer intressant att observera i listan över enheter är vad som _saknas_.</span><span class="sxs-lookup"><span data-stu-id="42f4a-128">The more interesting thing to observe in the list of drives is what is _missing_.</span></span> <span data-ttu-id="42f4a-129">Observera att vår **Data**-enhet (`sdc`) finns men har inte monterats i filsystemet.</span><span class="sxs-lookup"><span data-stu-id="42f4a-129">Notice that our **Data** drive (`sdc`) is present but not mounted into the file system.</span></span> <span data-ttu-id="42f4a-130">Azure har lagt till en VHD men har inte initierat den.</span><span class="sxs-lookup"><span data-stu-id="42f4a-130">Azure added a VHD but didn't initialize it.</span></span>

## <a name="initialize-data-disks"></a><span data-ttu-id="42f4a-131">Initiera datadiskar</span><span class="sxs-lookup"><span data-stu-id="42f4a-131">Initialize data disks</span></span>

<span data-ttu-id="42f4a-132">Alla ytterligare enheter du skapar från början måste initieras och formateras.</span><span class="sxs-lookup"><span data-stu-id="42f4a-132">Any additional drives you create from scratch will need to be initialized and formatted.</span></span> <span data-ttu-id="42f4a-133">Processen för att gör detta är identisk med en fysisk disk.</span><span class="sxs-lookup"><span data-stu-id="42f4a-133">The process for doing this is identical to a physical disk.</span></span>

1. <span data-ttu-id="42f4a-134">Först identifierar du disken.</span><span class="sxs-lookup"><span data-stu-id="42f4a-134">First, identify the disk.</span></span> <span data-ttu-id="42f4a-135">Vi gjorde det ovan.</span><span class="sxs-lookup"><span data-stu-id="42f4a-135">We did that above.</span></span> <span data-ttu-id="42f4a-136">Du kan också använda `dmesg | grep SCSI` som listar alla meddelanden från kärnan för SCSI-enheter.</span><span class="sxs-lookup"><span data-stu-id="42f4a-136">You could also use `dmesg | grep SCSI` which will list all the messages from the kernel for SCSI devices.</span></span>

1. <span data-ttu-id="42f4a-137">När du känner till enheten (`sdc`) måste du initiera, vilket du kan göra med `fdisk`.</span><span class="sxs-lookup"><span data-stu-id="42f4a-137">Once you know the drive (`sdc`) you need to initialize, you can use `fdisk` to do that.</span></span> <span data-ttu-id="42f4a-138">Du måste köra kommandot med `sudo` och ange den disk du vill partitionera.</span><span class="sxs-lookup"><span data-stu-id="42f4a-138">You will need to run the command with `sudo` and supply the disk you want to partition.</span></span>

    ```bash
    sudo fdisk /dev/sdc
    ```
1. <span data-ttu-id="42f4a-139">Använd kommandot `n` för att lägga till en ny partition.</span><span class="sxs-lookup"><span data-stu-id="42f4a-139">Use the `n` command to add a new partition.</span></span>  <span data-ttu-id="42f4a-140">I det här exemplet väljer vi också p för en primär partition och accepterar resten av standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="42f4a-140">In this example, we also choose p for a primary partition and accept the rest of the default values.</span></span> <span data-ttu-id="42f4a-141">Utdata blir något som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="42f4a-141">The output will be similar to the following example:</span></span>   

    ```output
    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0x1f2d0c46.
    
    Command (m for help): n
    Partition type
       p   primary (0 primary, 0 extended, 4 free)
       e   extended (container for logical partitions)
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-2145386495, default 2048):
    Last sector, +sectors or +size{K,M,G,T,P} (2048-2145386495, default 2145386495):
    
    Created a new partition 1 of type 'Linux' and of size 1023 GiB.
    ```    

1. <span data-ttu-id="42f4a-142">Skriv ut partitionstabellen med kommandot `p`.</span><span class="sxs-lookup"><span data-stu-id="42f4a-142">Print the partition table with the `p` command.</span></span> <span data-ttu-id="42f4a-143">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="42f4a-143">It should look something like this:</span></span>

    ```output
    Disk /dev/sdc: 1023 GiB, 1098437885952 bytes, 2145386496 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0x1f2d0c46
    
    Device     Boot Start        End    Sectors  Size Id Type
    /dev/sdc1        2048 2145386495 2145384448 1023G 83 Linux
    ```
    
1. <span data-ttu-id="42f4a-144">Skriv ändringarna med kommandot `w`.</span><span class="sxs-lookup"><span data-stu-id="42f4a-144">Write the changes with the `w` command.</span></span> <span data-ttu-id="42f4a-145">Detta avslutar verktyget.</span><span class="sxs-lookup"><span data-stu-id="42f4a-145">This will exit the tool.</span></span>

1. <span data-ttu-id="42f4a-146">Nu måste vi skriva ett filsystem till partitionen med kommandot `mkfs`.</span><span class="sxs-lookup"><span data-stu-id="42f4a-146">Next, we need to write a file system to the partition with the `mkfs` command.</span></span> <span data-ttu-id="42f4a-147">Vi måste ange filsystemstypen och enhetsnamnet som vi har fått från `fdisk`-utdata.</span><span class="sxs-lookup"><span data-stu-id="42f4a-147">We will need to specify the file system type and device name which we got from the `fdisk` output.</span></span>
    - <span data-ttu-id="42f4a-148">Skicka `-t ext4` för att skapa ett _ext4_-filsystem.</span><span class="sxs-lookup"><span data-stu-id="42f4a-148">Pass `-t ext4` to create an _ext4_ filesystem.</span></span>
    - <span data-ttu-id="42f4a-149">Enhetsnamnet är `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="42f4a-149">The device name is `/dev/sdc`.</span></span>

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    <span data-ttu-id="42f4a-150">Kommandot tar fem minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="42f4a-150">This command will take a few minutes to complete.</span></span>

    ```output
    mke2fs 1.44.1 (24-Mar-2018)
    Discarding device blocks: done
    Creating filesystem with 268173056 4k blocks and 67043328 inodes
    Filesystem UUID: e311c905-e0d9-43ab-af63-7f4ee4ef108e
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
            4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
            102400000, 214990848
    
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (262144 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```

1. <span data-ttu-id="42f4a-151">Skapa därefter en katalog som vi använder som monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="42f4a-151">Next, create a directory we will use as out mount point.</span></span> <span data-ttu-id="42f4a-152">Anta att vi har en `data`-mapp.</span><span class="sxs-lookup"><span data-stu-id="42f4a-152">Let's assume we will have a `data` folder.</span></span>

    ```bash
    sudo mkdir /data
    ```
1. <span data-ttu-id="42f4a-153">Använd slutligen `mount` för att koppla disken till monteringspunkten.</span><span class="sxs-lookup"><span data-stu-id="42f4a-153">Finally, use `mount` to attach the disk to the mount point.</span></span>

    ```bash
    sudo mount /dev/sdc1 /data
    ```
    <span data-ttu-id="42f4a-154">Du bör kunna använda `lsblk` för att se den monterade enheten nu.</span><span class="sxs-lookup"><span data-stu-id="42f4a-154">You should be able to use `lsblk` to see the mounted drive now.</span></span>
    
    ```output
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda       8:0    0   30G  0 disk
    ├─sda1    8:1    0 29.9G  0 part /
    ├─sda14   8:14   0    4M  0 part
    └─sda15   8:15   0  106M  0 part /boot/efi
    sdb       8:16   0   16G  0 disk
    └─sdb1    8:17   0   16G  0 part /mnt
    sdc       8:32   0 1023G  0 disk
    └─sdc1    8:33   0 1023G  0 part /data
    sr0      11:0    1  628K  0 rom
    ```

### <a name="mounting-the-drive-automatically"></a><span data-ttu-id="42f4a-155">Montera enheten automatiskt</span><span class="sxs-lookup"><span data-stu-id="42f4a-155">Mounting the drive automatically</span></span>

<span data-ttu-id="42f4a-156">För att säkerställa att enheten monteras automatiskt efter en omstart måste den läggas till i filen `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="42f4a-156">To ensure that the drive is mounted automatically after a reboot, it must be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="42f4a-157">Vi rekommenderar även starkt att UUID (Universally Unique Identifier) används i `/etc/fstab` för att referera till enheten i stället för bara enhetsnamnet (t.ex. `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="42f4a-157">It is also highly recommended that the UUID (Universally Unique Identifier) is used in `/etc/fstab` to refer to the drive rather than just the device name (such as, `/dev/sdc1`).</span></span> <span data-ttu-id="42f4a-158">Om operativsystemet upptäcker ett diskfel vid start och använder UUID undviker du att den felaktiga disken monteras på en viss plats.</span><span class="sxs-lookup"><span data-stu-id="42f4a-158">If the OS detects a disk error during boot, using the UUID avoids the incorrect disk being mounted to a given location.</span></span> <span data-ttu-id="42f4a-159">Återstående datadiskar tilldelas sedan samma enhets-ID:n.</span><span class="sxs-lookup"><span data-stu-id="42f4a-159">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="42f4a-160">Du kan hitta UUID för den nya enheten med verktyget `blkid`:</span><span class="sxs-lookup"><span data-stu-id="42f4a-160">To find the UUID of the new drive, use the `blkid` utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="42f4a-161">Något sådant här returneras:</span><span class="sxs-lookup"><span data-stu-id="42f4a-161">It will return something like:</span></span>

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. <span data-ttu-id="42f4a-162">Kopiera UUID för enheten `/dev/sdc1` och öppna filen `/etc/fstab` i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="42f4a-162">Copy the UUID for the `/dev/sdc1` drive and open the `/etc/fstab` file in a text editor.</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> <span data-ttu-id="42f4a-163">Felaktig redigering av filen `/etc/fstab` kan leda till att systemet inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="42f4a-163">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="42f4a-164">Om du är osäker läser du distributionens dokumentation för att få information om hur du redigerar filen på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="42f4a-164">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="42f4a-165">Vi rekommenderar även att en säkerhetskopia av filen skapas innan den redigeras när du arbetar med produktionssystem.</span><span class="sxs-lookup"><span data-stu-id="42f4a-165">It is also recommended that a backup of the file is created before editing when you are working with production systems.</span></span>

1. <span data-ttu-id="42f4a-166">Tryck på **G** för att gå till den sista raden i filen.</span><span class="sxs-lookup"><span data-stu-id="42f4a-166">Press **G** to move to the last line in the file.</span></span>

1. <span data-ttu-id="42f4a-167">Tryck på **I** för att öppna infogningsläget.</span><span class="sxs-lookup"><span data-stu-id="42f4a-167">Press **I** to enter INSERT mode.</span></span> <span data-ttu-id="42f4a-168">Läget bör visas längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="42f4a-168">It should indicate the mode at the bottom of the screen.</span></span>

1. <span data-ttu-id="42f4a-169">Tryck på **END**-tangenten för att gå till slutet på raden.</span><span class="sxs-lookup"><span data-stu-id="42f4a-169">Press the **END** key to move to the end of the line.</span></span> <span data-ttu-id="42f4a-170">Alternativt kan du använda piltangenterna.</span><span class="sxs-lookup"><span data-stu-id="42f4a-170">Alternatively, you can use the arrow keys.</span></span> <span data-ttu-id="42f4a-171">Tryck på **RETUR** om du vill gå till en ny rad.</span><span class="sxs-lookup"><span data-stu-id="42f4a-171">Press **ENTER** to move to a new line.</span></span>

1. <span data-ttu-id="42f4a-172">Skriv följande rad i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="42f4a-172">Type the following line into the editor.</span></span> <span data-ttu-id="42f4a-173">Värdena kan avgränsas med blanksteg eller tabb.</span><span class="sxs-lookup"><span data-stu-id="42f4a-173">The values can be space or tab separated.</span></span> <span data-ttu-id="42f4a-174">Mer information om varje kolumn finns i dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="42f4a-174">Check the documentation for more information on each of the columns.</span></span>

    ```output
    UUID=<uuid-goes-here>    /data    ext4    defaults,nofail    1    2
    ```
1. <span data-ttu-id="42f4a-175">Tryck på **ESC** och skriv sedan **:w!**</span><span class="sxs-lookup"><span data-stu-id="42f4a-175">Press **ESC**, then type **:w!**</span></span> <span data-ttu-id="42f4a-176">för att skriva filen och **:q** om du vill avsluta redigeraren.</span><span class="sxs-lookup"><span data-stu-id="42f4a-176">to write the file, and **:q** to quit the editor.</span></span>

1. <span data-ttu-id="42f4a-177">Slutligen ska vi kontrollera att posten är korrekt genom att fråga operativsystemet att uppdatera monteringspunkterna.</span><span class="sxs-lookup"><span data-stu-id="42f4a-177">Finally, let's check to make sure the entry is correct by asking the OS to refresh the mount points.</span></span>

    ```bash
    sudo mount -a
    ```
    
    <span data-ttu-id="42f4a-178">Om det returnerar ett fel redigerar du filen för att hitta problemet.</span><span class="sxs-lookup"><span data-stu-id="42f4a-178">If it returns an error, edit the file to find the problem.</span></span>

> [!TIP]
> <span data-ttu-id="42f4a-179">Vissa Linux-kärnor stöder TRIM för att ta bort oanvända block på diskar.</span><span class="sxs-lookup"><span data-stu-id="42f4a-179">Some Linux kernels support TRIM to discard unused blocks on disks.</span></span> <span data-ttu-id="42f4a-180">Den här funktionen finns på Azure-diskar och kan du kan spara pengar om du skapar stora filer och sedan tar bort dem.</span><span class="sxs-lookup"><span data-stu-id="42f4a-180">This feature is available on Azure disks and can save you money if you create large files and then delete them.</span></span> <span data-ttu-id="42f4a-181">I vår dokumentation finns information om hur du [aktiverar den här funktionen](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure).</span><span class="sxs-lookup"><span data-stu-id="42f4a-181">Learn how to [turn this feature on](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure) in our documentation.</span></span>

## <a name="install-software-onto-the-vm"></a><span data-ttu-id="42f4a-182">Installera programvara på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="42f4a-182">Install software onto the VM</span></span>

<span data-ttu-id="42f4a-183">Det finns flera alternativ för att installera programvara på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42f4a-183">You have several options to install software onto the VM.</span></span> <span data-ttu-id="42f4a-184">Först kan du, som nämnt, använda `scp` till att kopiera lokala filer från din dator till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42f4a-184">First, as mentioned, you can use `scp` to copy local files from your machine to the VM.</span></span> <span data-ttu-id="42f4a-185">Då kan du kopiera data eller anpassade program du vill köra.</span><span class="sxs-lookup"><span data-stu-id="42f4a-185">This lets you copy over data, or custom applications you want to run.</span></span>

<span data-ttu-id="42f4a-186">Du kan även installera programvara via SSH.</span><span class="sxs-lookup"><span data-stu-id="42f4a-186">You can also install software through the secure shell.</span></span> <span data-ttu-id="42f4a-187">Azure-datorer är som standard anslutna till Internet.</span><span class="sxs-lookup"><span data-stu-id="42f4a-187">Azure machines are by default, Internet-connected.</span></span> <span data-ttu-id="42f4a-188">Du kan använda standardkommandon för att installera populära programvarupaket direkt från standardlagringsplatser.</span><span class="sxs-lookup"><span data-stu-id="42f4a-188">You can use standard commands to install popular software packages directly from standard repositories.</span></span> <span data-ttu-id="42f4a-189">Vi använder den här metoden för att installera Apache.</span><span class="sxs-lookup"><span data-stu-id="42f4a-189">Let's use this approach to install Apache.</span></span>

### <a name="install-apache-web-server"></a><span data-ttu-id="42f4a-190">Installera Apache-webbserver</span><span class="sxs-lookup"><span data-stu-id="42f4a-190">Install Apache web server</span></span>

<span data-ttu-id="42f4a-191">Apache är tillgänglig i Ubuntus standardlagringsplatser för programvara, så vi installerar det med hjälp av konventionella pakethanteringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="42f4a-191">Apache is available within Ubuntu's default software repositories, so we will install it using conventional package management tools.</span></span>

1. <span data-ttu-id="42f4a-192">Starta genom att uppdatera det lokala paketindexet för att spegla de senaste överordnade ändringarna.</span><span class="sxs-lookup"><span data-stu-id="42f4a-192">Start by updating the local package index to reflect the latest upstream changes.</span></span>

    ```bash
    sudo apt-get update
    ```
    
1. <span data-ttu-id="42f4a-193">Installera sedan Apache.</span><span class="sxs-lookup"><span data-stu-id="42f4a-193">Next, install Apache.</span></span>

    ```bash
    sudo apt-get install apache2
    ```
    
1. <span data-ttu-id="42f4a-194">Det bör starta automatiskt och statusen kan kontrolleras med `systemctl`:</span><span class="sxs-lookup"><span data-stu-id="42f4a-194">It should start automatically - we can check the status using `systemctl`:</span></span>

    ```bash
    sudo systemctl status apache2
    ```

    <span data-ttu-id="42f4a-195">Något sådant här bör returneras:</span><span class="sxs-lookup"><span data-stu-id="42f4a-195">This should return something like:</span></span>

    ```output
    apache2.service - The Apache HTTP Server
       Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
      Drop-In: /lib/systemd/system/apache2.service.d
               └─apache2-systemd.conf
       Active: active (running) since Mon 2018-09-03 21:00:03 UTC; 1min 34s ago
     Main PID: 11156 (apache2)
        Tasks: 55 (limit: 4915)
       CGroup: /system.slice/apache2.service
               ├─11156 /usr/sbin/apache2 -k start
               ├─11158 /usr/sbin/apache2 -k start
               └─11159 /usr/sbin/apache2 -k start
    
    test-web-eus-vm1 systemd[1]: Starting The Apache HTTP Server...
    test-web-eus-vm1 apachectl[11129]: AH00558: apache2: Could not reliably determine the server's fully qua
    test-web-eus-vm1 systemd[1]: Started The Apache HTTP Server.
    ```

1. <span data-ttu-id="42f4a-196">Slutligen kan vi försöka hämta standardsidan via den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="42f4a-196">Finally, we can try retrieving the default page through the public IP address.</span></span> <span data-ttu-id="42f4a-197">En standardsida bör returneras.</span><span class="sxs-lookup"><span data-stu-id="42f4a-197">It should return a default page.</span></span>

    ![Apache-standardwebbsidan](../media-drafts/6-apache-works.png)

<span data-ttu-id="42f4a-199">Som du ser kan du med SSH arbeta med den virtuella Linux-datorn precis som en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="42f4a-199">As you can see, SSH allows you to work with the Linux VM just like a local computer.</span></span> <span data-ttu-id="42f4a-200">Du kan administrera den här virtuella datorn precis som andra Linux-datorer: installera programvara, konfigurera roller, justera funktioner och andra dagliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="42f4a-200">You can administer this VM as you would any other Linux computer: installing software, configuring roles, adjusting features and other everyday tasks.</span></span> <span data-ttu-id="42f4a-201">Men det är en manuell process. Om vissa program alltid behöver installeras kan du automatisera processen med hjälp av skript.</span><span class="sxs-lookup"><span data-stu-id="42f4a-201">However, it's a manual process - if we always need to install some software, you might consider automating the process using scripting.</span></span>
