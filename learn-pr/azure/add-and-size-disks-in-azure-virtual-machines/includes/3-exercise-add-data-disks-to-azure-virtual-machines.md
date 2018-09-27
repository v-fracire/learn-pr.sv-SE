Vi håller på att öka vår arbetsbelastning på vår advokatbyrå och du har fått i uppdrag att etablera en ny Linux-webbserver för att lagra viktiga dokument från många olika källor - klienter, konkurrenter och polismyndigheter. Vår server kan acceptera överföringar och måste lagra dem på disken.

> [!TIP]
> Den här övningen använder Linux som exempel, men den grundläggande processen för att skapa virtuella datorer och lägga till diskar är densamma för Windows. Den viktigaste skillnaden är partitionering och formatering av disken, vilket skulle ske i en fjärrskrivbordssession med de inbyggda diskhanteringsverktygen.

Målet med den här övningen är att skapa en virtuell Linux-dator (VM) och ansluta en ny virtuell hårddisk (VHD) med namnet ”uploadDataDisk1” för att lagra katalogen ”uploads”.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm"></a>Skapa en virtuell Linux-dator

Nu ska vi skapa en virtuell Linux-dator som värd för vår webbserver med Azure CLI.

1. Börja med att ställa in vissa standardvärden för den här sessionen. Det första du måste bestämma är _platsen_ där du vill placera den virtuella datorn. Detta bör vara nära dina klienter. Välj i så fall den närmaste regionen från platserna som är tillgängliga i Azure-sandbox-miljön.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Använd kommandot `az configure` för att ange den förvalda platsen du vill använda. Ersätt ”eastus” med platsen.

    ```azurecli
    az configure --defaults location=eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Ange standardvärdet för resursgruppen till den förkonfigurerade resursgruppen som skapats för sandbox-miljön för Azure: **<rgn>[Resursgrupp för sandbox-miljö]</rgn>**

    ```azurecli
    az configure --defaults group="<rgn>[sandbox Resource Group]</rgn>"
    ```

1. Använd sedan kommandot `vm create` för att skapa en ny virtuell Ubuntu Linux-dator.
    - Ge den namnet ”support-web-vm01”.
    - Ange **storleken** till _Standard_DS2_v2_.
    - Ange **admin-username** till ”azureuser” (eller vad du föredrar).
    - Generera SSH-nycklar med parametern `--generate-ssh-keys`.

    ```azurecli
    az vm create --name support-web-vm01 \
        --image UbuntuLTS \
        --size Standard_DS2_v2 \
        --admin-username azureuser \
        --generate-ssh-keys
    ```
    
    > [!TIP]
    > Det kan ta några minuter att skapa den virtuella datorn och distribuera den i Azure. Du kan övervaka förloppet i Cloud Shell.
    
1. Detta resulterar i ett JSON-block med information om den skapade virtuella datorn.

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
    Anteckna **publicIpAddress**i utdatan. På så vis kan vi komma åt den virtuella datorn via en fjärranslutning.

    > [!NOTE]
    > Vi använder den här virtuella datorn för att visa hur diskhantering går till. Om vi verkligen skulle använda den som webbserver hade vi behövt öppna ytterligare portar med kommandot `az vm open-port` och installera programvara för webbservern. Dessa uppgifter som ligger utanför omfånget för den här modulen, men du kan gå igenom modulen **Hantera virtuella datorer med Azure CLI** för att lära dig detta. 

## <a name="add-an-empty-data-disk-to-our-vm"></a>Lägga till en tom datadisk till den virtuella datorn

Vi kommer att kalla den disk som lagrar katalogen ”överföringar” åt din web server för ”uploadDataDisk1”. 

> [!TIP]
> Du kan lägga till datadiskar när den virtuella datorn skapas med hjälp av parametern `--data-disk-sizes-gb` för kommandot `vm create`.

1. Lägg till en ny tom disk på servern med kommandot `vm disk attach`.
    - Döp den ”uploadDataDisk1”
    - Ställ in den på 64 GB.
    - Ange **sku** till ”_Premium_LRS_” så vi använder premium storage med lokal redundans.

    ```azurecli
    az vm disk attach \
        --vm-name support-web-vm01 \
        --disk uploadDataDisk1 \
        --size-gb 64 \
        --sku Premium_LRS \
        --new
    ```
    
Vi har nu definierat en disk med namnet ”uploadDataDisk1”. För att använda disken måste vi partitionera och formatera den.

## <a name="initialize-data-disks"></a>Initiera datadiskar

Alla ytterligare enheter som du skapar från början måste initieras och formateras. Processen för att gör detta är identisk med en fysisk disk:

1. Först SSH till Linux-servern med hjälp av den offentliga IP-adress som du fick tillbaka från det ursprungliga svaret när den virtuella datorn skapades. Om du inte har det, använder du följande kommando för att hämta IP-adressen:

    ```azurecli
    az vm list-ip-addresses -n support-web-vm01
    ```

1. Använda SSH med den offentliga IP-adressen och användarnamnet som du skapade.

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. Identifiera därefter disken med kommandot `lsblk` för att lista alla blockenheter – här visas dina enheter.

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

Leta efter disken på 64 GB som vi skapade. Här kan du se att den är **sdc** och ej monterad, eftersom den inte har startats.

1. När du känner till enheten (**sdc**) måste du initiera, vilket du kan göra med `fdisk`. Du måste köra kommandot med `sudo` och ange den disk som du vill partitionera:

    ```bash
    sudo fdisk /dev/sdc
    ```

1. Lägg till en ny partition genom att använda kommandot <kbd>n</kbd>. I det här exemplet väljer vi också <kbd>p</kbd> för en primär partition och accepterar resten av standardvärdena. Dina utdata kommer att likna vad du ser i följande exempel:   

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

1. Skriv ut partitionstabellen med kommandot <kbd>p</kbd>. Det bör se ut ungefär så här:

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

1. Skriv ändringarna med kommandot <kbd>w<kbd>. Detta avslutar verktyget.

1. Nu måste vi skriva ett filsystem till partitionen med kommandot `mkfs`. Vi måste ange filsystemstypen och enhetsnamnet som vi har fått från `fdisk`-utdatan:
    - Skicka `-t ext4` för att skapa ett _ext4_-filsystem.
    - Enhetsnamnet är `/dev/sdc`.

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    Det kan ta ett par minuter innan kommandot har slutförts.

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
    
1. Skapa därefter en katalog som vi ska använda som monteringspunkt. Vi antar att vi har en `uploads`-mapp:

    ```bash
    sudo mkdir /uploads
    ```
1. Använd slutligen `mount` för att koppla disken till monteringspunkten:

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```
    Du bör kunna använda `lsblk` för att se den monterade enheten nu:
    
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
    
### <a name="mounting-the-drive-automatically"></a>Montera enheten automatiskt

För att säkerställa att enheten monteras automatiskt efter en omstart måste den läggas till i filen `/etc/fstab`. Vi rekommenderar även starkt att UUID (Universally Unique Identifier) används i `/etc/fstab` för att referera till enheten i stället för bara enhetsnamnet (t.ex. `/dev/sdc1`). Om operativsystemet upptäcker ett diskfel vid start och använder UUID undviker du att den felaktiga disken monteras på en viss plats. Återstående datadiskar tilldelas sedan samma enhets-ID:n. Du kan hitta UUID för den nya enheten med verktyget `blkid`:

```bash
sudo -i blkid
```

Något sådant här returneras:

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. Kopiera UUID för enheten `/dev/sdc1` och öppna filen `/etc/fstab` i en textredigerare:

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> Felaktig redigering av filen `/etc/fstab` kan leda till att systemet inte kan startas. Om du är osäker läser du distributionens dokumentation för att få information om hur du redigerar filen på rätt sätt. Vi rekommenderar även att en säkerhetskopia av filen skapas innan den redigeras när du arbetar med produktionssystem.

1. Tryck på <kbd>G</kbd> för att gå till den sista raden i filen.

1. Tryck på <kbd>I</kbd> för att öppna infogningsläget. Läget bör visas längst ned på skärmen.

1. Tryck på <kbd>END</kbd>-tangenten för att gå till slutet på raden. Alternativt kan du använda piltangenterna. Tryck på <kbd>RETUR</kbd> om du vill gå till en ny rad.

1. Skriv följande rad i redigeraren. Värdena kan avgränsas med blanksteg eller tabb. Mer information om varje kolumn finns i dokumentationen:

    ```output
    UUID=<uuid-goes-here>    /uploads    ext4    defaults,nofail    1    2
    ```
1. Tryck på **ESC** och skriv sedan **:w!** för att skriva filen och **:q** för att avsluta redigeraren.

1. Slutligen ska vi kontrollera att posten är korrekt genom att be operativsystemet att uppdatera monteringspunkterna:

    ```bash
    sudo mount -a
    ```

    Om det returnerar ett fel redigerar du filen för att hitta problemet.

> [!TIP]
> Vissa Linux-kärnor stöder TRIM för att ta bort oanvända block på diskar. Den här funktionen finns på Azure-diskar och kan du kan spara pengar om du skapar stora filer och sedan tar bort dem. I vår dokumentation finns information om hur du [aktiverar den här funktionen](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure).

Nu när du har sett hur enkelt det är att lägga till diskar på dina virtuella datorer är det dags att titta på de olika typerna av diskar som du kan skapa. I synnerhet valet mellan Standard- och Premium-lagring.