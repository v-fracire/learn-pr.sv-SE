Vår virtuella Linux-dator har distribuerats och körs men den har inte konfigurerats att utföra arbete. Nu ska vi ansluta till den med SSH och konfigurera Apache, så att vi har en aktiv server.

## <a name="connect-to-the-vm-with-ssh"></a>Ansluta till den virtuella datorn med SSH

Om du vill ansluta till en virtuell Azure-dator med en SSH-klient behöver du:

- SSH-klientprogramvaran (finns på de flesta moderna operativsystem)
- Den offentliga IP-adressen för den virtuella datorn (eller privat om den virtuella datorn är konfigurerad för att ansluta till nätverket)

### <a name="get-the-public-ip-address"></a>Hämta den offentliga IP-adressen

1. Se till att [översiktspanelen](https://portal.azure.com?azure-portal=true) för den virtuella dator du skapade tidigare är öppen i **Azure Portal**. Du hittar den virtuella datorn under **Alla resurser** om du behöver öppna den. Översiktspanelen har mycket information om den virtuella datorn.

    - Du kan se om den virtuella datorn körs
    - Stoppa eller starta om den
    - Hämta den offentliga IP-adressen för att ansluta till den virtuella datorn
    - Visa aktiviteten för CPU, disk och nätverk

1. Klicka på knappen **Anslut** högst upp i fönstret.

1. På bladet **Anslut till virtuell dator** noterar du inställningarna för **IP-adress** och **portnummer**. På fliken **SSH** fliken finns också kommandot som du behöver köra lokalt för att ansluta till den virtuella datorn. Kopiera detta till Urklipp.

<!-- TODO: This will be necessary if we ever have inline portal integration. 

### Open Azure Cloud Shell

Let's use Cloud Shell in the Azure portal. If you generated the SSH key locally, you need to use your local session since the private key won't be in your storage account:

1. Switch back to the **Dashboard** by clicking the **Dashboard** button in the Azure sidebar.

1. Open Cloud Shell by clicking the **shell** button in the top toolbar.

    ![Screenshot of the Azure portal top navigation bar with the Azure Cloud Shell button highlighted.](../media/6-cloud-shell.png)

1. Select **Bash** as the shell type. PowerShell is also available if you are a Windows administrator.

-->

## <a name="connect-with-ssh"></a>Ansluta med SSH

1. Klistra in den kommandorad som du fick från SSH-fliken i Azure Cloud Shell. Det bör se ut ungefär så här. Men den har en annan IP-adress (och kanske ett annat användarnamn om du inte använde **jim**!):

    ```bash
    ssh jim@137.117.101.249
    ```

1. Det här kommandot öppnar en Secure Shell-anslutning och placerar dig i en traditionell kommandotolk för Linux.

1. Prova att köra några Linux-kommandon
    - `ls -la /` för att visa diskens rot
    - `ps -l` för att visa alla processer som körs
    - `dmesg` för att lista alla kernelmeddelanden
    - `lsblk` för att lista alla blockenheter – här ser du dina enheter

Mer intressant att observera i listan över enheter är vad som _saknas_. Observera att vår **Data**-enhet (`sdc`) finns men har inte monterats i filsystemet. Azure har lagt till en VHD men har inte initierat den.

## <a name="initialize-data-disks"></a>Initiera datadiskar

Alla ytterligare enheter som du skapar från början måste initieras och formateras. Processen för att gör detta är identisk med en fysisk disk:

1. Först identifierar du disken. Vi gjorde det ovan. Du kan också använda `dmesg | grep SCSI` som listar alla meddelanden från kärnan för SCSI-enheter.

1. När du känner till enheten (`sdc`) måste du initiera, vilket du kan göra med `fdisk`. Du måste köra kommandot med `sudo` och ange den disk som du vill partitionera:

    ```bash
    sudo fdisk /dev/sdc
    ```
1. Använd kommandot `n` för att lägga till en ny partition. I det här exemplet väljer vi också **p** för en primär partition och accepterar resten av standardvärdena. Utdatan blir något som liknar följande exempel:   

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

1. Skriv ut partitionstabellen med kommandot `p`. Det bör se ut ungefär så här:

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

1. Skriv ändringarna med kommandot `w`. Detta avslutar verktyget.

1. Nu måste vi skriva ett filsystem till partitionen med kommandot `mkfs`. Vi måste ange filsystemstypen och enhetsnamnet som vi har fått från `fdisk`-utdatan:
    - Skicka `-t ext4` för att skapa ett _ext4_-filsystem.
    - Enhetsnamnet är `/dev/sdc`.

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    Kommandot tar några minuter att slutföra.

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

1. Skapa därefter en katalog som vi ska använda som monteringspunkt. Vi antar att vi har en `data`-mapp:

    ```bash
    sudo mkdir /data
    ```
1. Använd slutligen `mount` för att koppla disken till monteringspunkten:

    ```bash
    sudo mount /dev/sdc1 /data
    ```
    Du bör kunna använda `lsblk` för att se den monterade enheten nu:
    
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

1. Tryck på **G** för att gå till den sista raden i filen.

1. Tryck på **I** för att öppna infogningsläget. Läget bör visas längst ned på skärmen.

1. Tryck på **END**-tangenten för att gå till slutet på raden. Alternativt kan du använda piltangenterna. Tryck på **RETUR** om du vill gå till en ny rad.

1. Skriv följande rad i redigeraren. Värdena kan avgränsas med blanksteg eller tabb. Mer information om varje kolumn finns i dokumentationen:

    ```output
    UUID=<uuid-goes-here>    /data    ext4    defaults,nofail    1    2
    ```
1. Tryck på **ESC** och skriv sedan **:w!** för att skriva filen och **:q** för att avsluta redigeraren.

1. Slutligen ska vi kontrollera att posten är korrekt genom att be operativsystemet att uppdatera monteringspunkterna:

    ```bash
    sudo mount -a
    ```

    Om det returnerar ett fel redigerar du filen för att hitta problemet.

> [!TIP]
> Vissa Linux-kärnor stöder TRIM för att ta bort oanvända block på diskar. Den här funktionen finns på Azure-diskar och kan du kan spara pengar om du skapar stora filer och sedan tar bort dem. I vår dokumentation finns information om hur du [aktiverar den här funktionen](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure).

## <a name="install-software-onto-the-vm"></a>Installera programvara på den virtuella datorn

Det finns flera alternativ för att installera programvara på den virtuella datorn. Först kan du, som nämnt, använda `scp` till att kopiera lokala filer från din dator till den virtuella datorn. Då kan du kopiera över de data eller anpassade program som du vill köra.

Du kan även installera programvara via Secure Shell. Azure-datorer är som standard anslutna till Internet. Du kan använda standardkommandon för att installera populära programvarupaket direkt från standardlagringsplatser. Vi använder den här metoden för att installera Apache.

### <a name="install-the-apache-web-server"></a>Installera Apache-webbservern

Apache är tillgänglig i Ubuntus standardlagringsplatser för programvara, så vi installerar det med hjälp av konventionella pakethanteringsverktyg:

1. Starta genom att uppdatera det lokala paketindexet för att spegla de senaste överordnade ändringarna:

    ```bash
    sudo apt-get update
    ```
    
1. Installera sedan Apache:

    ```bash
    sudo apt-get install apache2
    ```

1. Det bör starta automatiskt och statusen kan kontrolleras med `systemctl`:

    ```bash
    sudo systemctl status apache2
    ```

    Något sådant här bör returneras:

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

1. Slutligen kan vi försöka hämta standardsidan via den offentliga IP-adressen. En standardsida bör returneras.

    ![Skärmbild av en webbläsare som visar Apaches standardwebbsida för IP-adressen till den nya virtuella Linux-datorn.](../media/6-apache-works.png)

Som du ser kan du med SSH arbeta med den virtuella Linux-datorn precis som en lokal dator. Du kan administrera den här virtuella datorn precis som andra Linux-datorer: installera programvara, konfigurera roller, justera funktioner och andra dagliga uppgifter. Men det är en manuell process. Om vissa program alltid behöver installeras kan du automatisera processen med hjälp av skript.
