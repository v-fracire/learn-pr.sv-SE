Vi underskattade hur stora en del av uppladdningarna skulle bli, och uppladdningsdisken har nästan slut på utrymme. Du väljer att dubbla utrymmet och uppgraderar det från 64 GB till 128 GB.

## <a name="resize-the-data-disk"></a>Ändra storlek på datadisk

För att ändra storleken på en disk behöver vi diskens ID eller namn. I det här fallet känner vi redan till namnet (uploadDataDisk1). Men om vi inte kommer ihåg det eller om det skapades av någon annan kan vi hämta en lista över diskar med hjälp av kommandot `az disk list`.

1. Börja med att hämta en lista över hanterade diskar i resursgruppen. Detta kan inkludera andra diskar om du har flera virtuella datorer i en enda resursgrupp. I vårt exempel ska det bara finnas vår webbserver.

    ```azurecli
    az disk list \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

1. Vi stoppar och frigör den virtuella datorn med hjälp av `az vm deallocate`. 

    ```azurecli
    az vm deallocate --name support-web-vm01
    ```
1. Nu kan vi ändra storlek på disken med kommandot `az disk update`.

    ```azurecli
    az disk update --name uploadDataDisk1 --size-gb 128
    ```
    
1. Starta om den virtuella datorn när du utfört storleksändringen.

    ```azurecli
    az vm start --name support-web-vm01
    ```

## <a name="expand-the-disk-partition"></a>Expandera diskpartitionen

Det sista steget är att berätta för operativsystemet om det tillgängliga utrymmet. Den här processen är identisk för expansioner av lokala diskar, precis som de partitionerings- och formateringssteg som vi utförde tidigare. 

1. Först hämtar vi den virtuella datorns offentliga IP-adress. Eftersom den startades om har den förmodligen ändrats. Den här gången provar vi en annan metod och använder `az vm show` med ett filter för att returnera den offentliga IP-adressen.

    > [!TIP]
    > IP-adresser är dynamiska som standard. Azure DNS kompenserar automatiskt för IP-ändringen, eller så kan du ändra beteendet genom att använda statiska IP-adresser.

    ```azurecli
    az vm show --name support-web-vm01 -d --query [publicIps] --o tsv
    ```
    
1. SSH till Linux-datorn. Du behöver ange rätt IP-adress.

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. Avmontera disken. Det gällde alltså `/dev/sdc1`.

    ```bash
    sudo umount /dev/sdc1
    ```

1. Starta `parted` i ett upphöjt gränssnitt

    ```bash
    sudo parted /dev/sdc
    ```
    
1. Expandera partitionen med kommandot `resizepart`. Ange partition 1 och den nya storleken (128 GB).

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [64GB]? 128GB
    ```

    > [!WARNING]
    > Var försiktig med storleken. När du ändrar storlek på partitionen går det även att minska en partition, vilket förmodligen skulle leda till dataförlust.
    
1. Avsluta verktyget genom att skriva `quit`.

1. Partitioneringsverktyget _återmonterar_ enheten automatiskt. Avmontera den därför igen så att den kan formateras.

    ```bash
    sudo umount /dev/sdc1
    ```
    
1. Verifiera partitionskonsekvensen med `e2fsck`. Det här steget är absolut nödvändigt, men det är en bra idé närhelst du ändrar storlek på en diskvolym.

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. Ändra storlek på filsystemet med `resize2fs`.

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. Slutligen monterar du tillbaka enheten på monteringspunkten.

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```

Verifiera att operativsystemdisken har bytt storlek genom att använda `df -h`. Det bör nu stå att enheten är på 128 GB.
