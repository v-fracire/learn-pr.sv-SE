Din server måste ha tillräckligt med resurser för att hantera dagliga begäran. En typisk strategi är att välja en VM-storlek vid skapandet som är tillräcklig för typiska arbetsbelastningar och sedan ändra storlek när efterfrågan förändras.

I scenariot med leksakerna skulle den här strategin vara användbar för att hantera resurser för din tillväxt på medellång sikt. Du kan öka storleken på den virtuella datorn för att hantera ökad efterfrågan när företaget växer.

## <a name="what-is-virtual-machine-size"></a>Vad är storleken på den virtuella datorn?

_Storleken_ på en virtuell dator är ett mått på dess processor, minne, disk och förväntad nätverksbandbredd. Virtuella datorer är tillgängliga i ett förbestämt antal storlekar. Till exempel har storleken **Standard_F32s_v2** 32 virtuella processorer, 64 GiB minne, en lokal SSD på 256 GiB och en förväntad nätverksbandbredd på 14 000 Mbit/s.

När du skapar en ny virtuell dator i Azure måste du välja en storlek. Större storlekar kostar mer. Målet är att välja en storlek som kan hantera din arbetsbelastning utan att konfigurera mer kraft än du behöver.

## <a name="what-is-virtual-machine-type"></a>Vad är en typ av virtuell dator?

Den virtuella datorns _typ_ är arbetsbelastningen som den virtuella datorn har optimerats för. Vissa virtuella datorer är till exempel avsedda för processorintensiva uppgifter som att vara värd för en webbserver. Andra är avsedda för lagringsfokuserade jobb som att köra en databas.

Det finns _typer_ som motsvara varje kärnkomponent för maskinvaran i en modern dator: **beräkning**, **minne**, **lagring** och **GPU**. Det finns också en typ för **generell användning** om du behöver en balanserad kombination av resurser. Följande tabell visar typerna och VM-storlekarna som är en del av varje typ tillsammans med en kort beskrivning av målarbetsbelastningen.

|Typ|Storlekar|Beskrivning|
|---|---|---|
|Generell användning|B, Ds_v3, D_v3, vissa DS_v2, vissa D_v2, A_v2|Datorer för generell användning har ett balanserat förhållande av CPU-till-minne. Datorer för generell användning är bra för testning eller utveckling för servrar, även små till medelstora databaser eller webbservrar med låg till medelhög trafik.|
|Beräkningsoptimerad|Fs_v2, Fs, F|Virtuella datorer som är beräkningsoptimerade har ett högre förhållande mellan processor och minne än datorer för generell användning, för uppgifter som kräver extra processorkraft, till exempel programservrar, nätverksenheter eller webbservrar med medelhög trafik.|
|Minnesoptimerad|Es_v3, E_v3, M, GS, G, vissa DS_v2, vissa D_v2|Minnesoptimerade virtuella datorer är avsedda att ha ett högt förhållande minne-till-CPU. Sådana datorer är bra för relationsdatabasservrar, servrar som kräver eller utför mycket cachelagring eller servrar som utför minnesinterna analyser.|
|Lagringsoptimerad|Ls|De har virtuella datorerna är konfigurerade för högt diskgenomflöde och I/O-åtgärder som passar databaser för stordata, SQL och NoSQL.|
|GPU|NV, NC, NC_v2, NC_v3, ND|Virtuella GPU-datorer är specialiserade på uppgifter som tung grafikåtergivning eller videoredigering och även träning av modeller och inferensjobb med djupinlärning (ND-serien). Du kan välja en eller flera GPU:er för sådana datorer.|
|Databehandling med höga prestanda|H|De snabbaste och kraftfullaste processorerna finns i dessa virtuella datorer. Du kan även lägga till nätverksgränssnitt för stora dataflöden (RDMA).|

## <a name="clusters"></a>Kluster

Maskinvaran för den fysiska servern i Azure-regioner grupperas ihop i kluster. Varje kluster kan stödja flera olika virtuella datorstorlekar baserat på den fysiska maskinvaran.

När du skapar en virtuell dator och väljer en viss storlek etableras den virtuella datorn till ett lämpligt maskinvarukluster för den storleken. Trots att du kan ändra storlek på virtuella datorer när du har skapat dem kan alternativen för storleksändring begränsas av maskinvaruklustret som har valts för den ursprungliga storleken.

## <a name="what-is-vertical-scaling"></a>Vad är vertikal skalning?

_Vertikal skalning_ är processen för att ändra _storlek_ på en virtuell dator. Du kan _skala upp_ genom att välja en kraftfullare storlek för att hantera ökad efterfrågan eller _skala ned_ för att allokera färre resurser och minska kostnaderna. Följande bild visar ett exempel på att ändra storleken för en virtuell dator.

![En bild som visar upp- och nedskalning av en virtuell dator för att ändra prestandan.](../media/2-ScaleUpDown.png)

Du kan ändra storlek för en virtuell dator med hjälp av Azure-portalen, Azure PowerShell eller Azure-CLI.

### <a name="resize-in-the-portal"></a>Ändra storlek i portalen

I Azure-portalen kan du ändra storlek på en virtuell dator genom att välja den virtuella datorn, klicka på posten **Storlek** och välja en post i bladet **Välj en storlek**. 

Om den virtuella datorn körs vid den aktuella tidpunkten beror de storlekar du kan välja bland på vilka storlekar som är tillgängliga i din region. Du ser endast alternativ för att ändra storlek som är kompatibel med samma maskinvarukluster som den virtuella datorn körs på. Det kallas ibland för en *storleksfamilj*. Om du väljer en ny storlek medan den virtuella datorn körs startas din VM om automatiskt för att tillämpa den nya storleken.

Om storleken du letar efter inte visas i portalen när den virtuella datorn körs kan du stänga av den virtuella datorn för att se fler alternativ. När datorn är i tillståndet **Stoppad (frigjord)** kan du välja storlekar från annan maskinvara i samma region.

### <a name="resize-with-powershell"></a>Ändra storlek med PowerShell

Du kan använda PowerShell för att utföra vertikal skalning interaktivt eller med skript. Skript är bra för avancerade scenarier, till exempel om du vill ändra storlek på flera virtuella datorer samtidigt. De är också praktiskt om du vill ändra storleken under icke-arbetstid för att undvika störningar för användaren.

Följande cmdlet visar en lista över storlekar för virtuella datorer i samma storleksfamilj som den aktuella maskinvaran:

```PowerShell
Get-AzureRmVMSize -ResourceGroupName "myResourceGroup" -VMName "MyVM"
```

Om önskad storlek visas använder du följande cmdlet för att ändra storlek på den virtuella datorn:

```PowerShell
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "MyVM"
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
```

Om den önskade storleken inte visas när datorn körs använder du följande kommandon för att frigöra den virtuella datorn, ändra storlek på datorn och starta den igen:

```PowerShell
Stop-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "MyVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "MyVM"
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "MyVM"
```

Storleken på virtuella datorer i Azure kan ändras efter behov för att öka prestanda eller minska kostnaderna. Det är praktiskt att utföra storleksändringen manuellt, antingen med portalen eller ett skript, för att hantera gradvis företagstillväxt eller när du vet om en förändring av efterfrågan i förväg. I scenariot med leksaksaffären kunde de skala upp före semestern för att hantera efterfrågantopparna och sedan skala ned efteråt.
