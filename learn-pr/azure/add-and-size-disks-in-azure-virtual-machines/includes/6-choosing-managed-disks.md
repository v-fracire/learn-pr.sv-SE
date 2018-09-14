Program och tjänster behöver ofta att skala med begäran. Det innebär skala ut eller skapa ytterligare instanser för virtuella datorer. Skapa diskar för nya instanser placera manuellt belastning på Azure-administratörer. Vi kan i stället använda **hanterade diskar** att minska administrationen som krävs för att skapa nya diskar.

Hanterad disklagring för referensen för du ”i bakgrunden”. Du anger disken typ och storleken på disken som du behöver och Azure skapar och hanterar disken åt dig. 

När du använder hanterade diskar, en annan disktypen är tillgängliga för dig att använda kallas **standard SSD**.

## <a name="standard-ssds"></a>Standard SSD:er

Om du kommer ihåg med ohanterade diskar, du kan välja mellan två typer av disk: standard-hårddiskar (HDD) och premium SSD-enheter (solid-state drive).

När du använder hanterade diskar kan du välja en tredje typ av disk: standard SSD-enheter. Den här disktyp varandra standard hårddiskar och premium SSD ur en prestanda och kostnader.

Du kan använda standard SSD-enheter med alla VM-storlekar, inklusive VM-storlekar som inte har stöd för premiumlagring. I själva verket är använda standard SSD: er det enda sättet att använda SSD: er med dessa virtuella datorer.

## <a name="managed-disks-in-availability-sets"></a>Hanterade diskar i tillgänglighetsuppsättningar

Ibland kan du använda flera virtuella datorer som värd för ditt program för att hantera ökad belastning.

Du kan placera dina virtuella datorer i en **tillgänglighetsuppsättning** att minska risken för kunder som har ett avbrott. Med hjälp av en tillgänglighetsuppsättning, är dina virtuella datorer fördelade över feldomäner. En feldomän är en logisk grupp av underliggande maskinvara som delar samma strömkälla och nätverksswitch, ungefär som ett rack i ett lokalt datacenter. När du skapar virtuella datorer i en tillgänglighetsuppsättning distribuerar Azure-plattformen automatiskt dina virtuella datorer mellan dessa feldomäner. Den här separationen i Azure-datacentret garanterar att det finns ingen enskild felpunkt för alla virtuella datorer i tillgänglighetsuppsättningen.

När du använder hanterade diskar ser automatiskt Azure till att varje virtuell Hårddisk placeras i samma feldomän som den virtuella datorn. Garantin tar bort risken att konfigurationen av storage-kontot kan innehålla en enskild felpunkt. Det går inte att garantera den här separationen när du använder ohanterade diskar.

## <a name="choosing-managed-disks"></a>Välja hanterade diskar

Det finns några aspekter som kan påverka beslutet att skapa dina diskar för virtuella datorer som hanterade diskar i stället för ohanterade diskar.

**Vill du styra storage-konton i din prenumeration?**

Hanterade diskar är enkla att administrera, men om du anser att du kan optimera lagring eller hantera kostnaderna bättre med behåller kontrollen över storage-konton.

**Behöver du använda standard SSD-enheter?** 

Om du vill använda SSD: er med en VM-storlek som inte har stöd för premium storage kan behöva du väljer hanterade diskar.

**Är de virtuella datorerna i en tillgänglighetsuppsättning?** 

Vi rekommenderar starkt att använda hanterade diskar för att optimera flexibilitet om dina virtuella datorer är medlemmar i en tillgänglighetsuppsättning.

## <a name="migrating-to-managed-disks"></a>Migrera till managed disks

Du kan konvertera din ohanterade diskar till hanterade diskar när som helst med hjälp av någon av följande två metoder:

- Med Azure portal. På sidan diskar i den virtuella datorns inställningar hittar du den **migrera till managed disks** verktyget. Den här processen kommer stoppa den virtuella datorn, migrera diskarna och starta sedan om den virtuella datorn för dig. Du får ett avbrott i tjänsten från den virtuella datorn medan konverteringen sker.
- Använda Azure PowerShell. Du kan använda den `ConvertTo-AzureRmVMManagedDisk` cmdlet för att utföra migreringen. Om du väljer den här metoden måste du stoppa och starta den virtuella datorn själv.
  
> [!Note]
> Tänk på att migrera från ohanterade diskar till hanterade diskar inte kan ångras. Dessutom tas den ursprungliga lagringskonton som används av den virtuella datorn inte bort automatiskt. Du måste ta bort de ursprungliga Virtuella hårddiskblobbarna för att undvika kostnader uppkommer. 

Hanterade diskar minska det administrativa arbetet och har vissa funktioner, till exempel standard SSD: er som inte är tillgängliga för ohanterade diskar.
