Azure lagrar dina VHD-avbildningar som sidblobar på ett Azure Storage-konto. Med hanterade diskar tar Azure hand om hanteringen av lagringsutrymmet åt dig – det är en av de bästa anledningarna att välja hanterade diskar.

När du skapar en virtuell dator väljer den en storlek för OS-disken. Den angivna storleken baseras på den bild du väljer. På Linux är det ofta runt 30 GB och på Windows cirka 127 GB.

Du kan lägga till datadiskar för att ge ytterligare lagringsutrymme men du kan också vilja expandera en befintlig disk – kanske kan ett äldre program inte dela sina data på olika enheter eller du kanske migrerar en fysisk dators disk till Azure och behöver en större OS-enhet.

> [!NOTE]
> Du kan bara ändra storleken till _större_. Just nu stöds inte att minska storleken på hanterade diskar.

Om du ändrar storleken på disken kan det även ändra diskens nivå (t.ex. från P10 till P20). Ha detta i åtanke – det kan vara fördelaktigt för prestandauppgraderingar men det kostar också mer när du uppgraderar till premiumnivåerna.

## <a name="vm-size-vs-disk-size"></a>VM-storlek jämfört med diskstorlek

Den VM-storlek du väljer när du skapar den virtuella datorn avgör hur många resurser som kan tilldelas. För lagringsutrymmet styr storleken antalet diskar du kan lägga till på den virtuella datorn och maxstorleken för varje disk. 

Som nämndes i den tidigare kursdelen stöder vissa VM-storlekar bara standardlagringsenheter – med begränsad I/O-prestanda.

Om du upptäcker att du behöver mer lagring än vad storleken på den virtuella datorn tillåter kan du ändra dess storlek. Vi behandlar det ämnet i modulen **Introduktion till Azure Virtual Machines**.

## <a name="expanding-a-disk-using-the-azure-cli"></a>Expandera en disk med Azure CLI

> [!WARNING]
> Se alltid till att du säkerhetskopierar dina data innan du utför en storleksändring!

Åtgärder på virtuella hårddiskar kan inte utföras när den virtuella datorn körs. Det första steget är att stoppa och frigöra den virtuella datorn med `az vm deallocate`, med den virtuella datorns och resursgruppens namn.

Att frigöra en virtuell dator, till skillnad från att bara _stoppa_ en virtuell dator frigör de associerade datorresurserna och låter Azure göra konfigurationsändringar av den virtualiserade maskinvaran.

```azurecli
az vm deallocate --resource-group <resource-group-name> --name <vm-name>
```

För att ändra storlek på en disk använder du sedan `az disk update`, vilket överför disknamnet, resursgruppens namn och den nya begärda storleken. När du expanderar en hanterad disk mappas den angivna storleken till den närmaste diskstorleken.

```azurecli
az disk update \
    --resource-group <resource-group-name> \
    --name <disk-name> \
    --size-gb 200
```

Starta slutligen den virtuella datorn igen med `az vm start`:

```azurecli
az vm start --resource-group <resource-group-name> --name <vm-name>
```

## <a name="expanding-a-disk-using-the-azure-portal"></a>Expandera en disk med Azure Portal

Att expandera en disk med Azure Portal är ännu enklare.

1. Stoppa den virtuella datorn med knappen **Stopp** i verktygsfältet i **översiktsvyn** på den virtuella datorn.

1. Klicka på **Diskar** i avsnittet **Inställningar**.

1. Välj den datadisk du vill ändra storlek på.

    ![Skärmbild som visar avsnittet med diskar på en virtuell dator med den virtuella hårddisk som vi vill redigera markerad](../media/5-portal-disks.png)

1. Ange en storlek _större_ än den aktuella storleken i diskinformationen. Du kan även ändra från Premium till Standard (eller tvärtom) här. De här inställningarna justerar dina prestanda, som visas i avsnittet för förväntade IOPS.

    ![Skärmbild som visar den virtuella hårddiskens redigeringsskärm med fältet för ny storlek markerat](../media/5-resize-disk.png)

1. Klicka på **Spara** för att spara ändringarna.

1. Starta om den virtuella datorn.


### <a name="expanding-the-partition"></a>Expandera partitionen

Precis som när du lägger till en ny datadisk lägger en expanderad disk inte till något användbart utrymme förrän du expanderar partitionen och filsystemet. Det här måste göras med OS-verktygen som är tillgängliga på den virtuella datorn. 

I Windows skulle vi använda verktyget Disk Manager eller kommandoradsverktyget `diskpart`.

I Linux använder du `parted` och `resize2fs`.

Vi provar några av dess kommandon.