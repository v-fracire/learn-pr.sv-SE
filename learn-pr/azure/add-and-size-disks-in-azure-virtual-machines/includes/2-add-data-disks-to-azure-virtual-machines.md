Precis som alla andra datorer använder virtuella Azure-datorer diskar för att lagra operativsystemet, programmen och data. Dessa diskar kallas för virtuella hårddiskar (VHD).

Vi antar att du har skapat en virtuell dator i Azure, som ska vara värd för databasen med fallhistorik som din advokatbyrå använder. En väl utformad diskkonfiguration är nyckeln till bra prestanda och flexibilitet för SQL Server.

I den här kursdelen får du lära dig att välja rätt konfigurationsvärden för dina diskar och hur du kopplar de diskarna till en virtuell dator.

## <a name="how-disks-are-used-by-vms"></a>Hur diskar används av virtuella datorer

Virtuella datorer använder diskar i tre olika syften:

- **Lagring av operativsystem**. Varje virtuell dator innehåller en disk som lagrar operativsystemet. Den här enheten är registrerad som en SATA-enhet och märkt som C:-enheten i Windows. Den monteras på ”/” i Unix-liknande operativsystem. Den har en maximal kapacitet på 2 048 gigabyte (GB) och dess innehåll hämtas från den virtuella datoravbildning som du använde för att skapa den virtuella datorn.
- **Tillfällig lagring**. Varje virtuell dator innehåller också en tillfällig virtuell hårddisk som används för sidor och växlingsfiler. Data på den här enheten kan gå förlorade vid underhåll eller omdistribution. Enheten är märkt som D: på en virtuell Windows-dator som standard. Använd inte den här enheten för att lagra viktiga data som du inte vill förlora.
- **Datalagring**. En datadisk är alla andra diskar som är anslutna till en virtuell dator. Du kan använda datadiskar för att lagra filer, databaser och alla andra data som du behöver bevara mellan omstarter. Vissa virtuella datoravbildningar innefattar datadiskar som standard. Du kan också lägga till dina fler datadiskar upp till det maximala antal som anges av storleken på den virtuella datorn. Varje datadisk är registrerad som en SCSI-enhet och har en maximal kapacitet på 4 095 GB. Du kan välja enhetsbeteckningar eller monteringspunkter för dina enheter.

## <a name="storing-vhd-files"></a>Lagra VHD-filer

I Azure lagras virtuella hårddiskar på ett Azure-lagringskonto som **sidblobbar**.

I den här tabellen visas de olika typerna av lagringskonton och vilka objekt som kan användas med vart och ett.

|**Typ av lagringskonto**|**Allmän standard**|**Allmän premium**|**Blobblagring, frekvent och lågfrekvent åtkomstnivå**|
|-----|-----|-----|-----|
|**Tjänster som stöds**| Azure-blobblagring, Azure Files, Azure-kölagring | Blobblagring | Blobblagring|
|**Typer av blobbar som stöds**|Blockblobbar, sidblobbar och tilläggsblobbar | Sidblobbar | Blockblobbar och bilageblobbar|

Premium- och allmän standardlagring har stöd för sidblobar. Välj ett standardlagringskonto om kostnaden är viktigast. Premium storage kostar mer, men kommer också att leverera mycket högre i/o-åtgärder per sekund (IOPS). Om dataprestanda är ett krav för den virtuella datorn är premiumlagring bättre.

## <a name="attach-data-disks-to-vms"></a>Koppla datadiskar till virtuella datorer

Du kan lägga till datadiskar till en virtuell dator när som helst genom att koppla dem till den virtuella datorn. När du ansluter en disk associeras VHD-filen med den virtuella datorn. 

Den virtuella hårddisken kan inte tas bort från lagringen när den är ansluten.

### <a name="attach-an-existing-data-disk-to-an-azure-vm"></a>Koppla en befintlig datadisk till en virtuell Azure-dator

Du kanske redan har en virtuell hårddisk som lagrar data som du vill använda i din virtuella Azure-dator. I vårt scenario med advokatbyrån kanske du redan har konverterat dina fysiska diskar till virtuella hårddiskar lokalt. I det här fallet kan du använda PowerShell-cmdleten `Add-AzureRmVhd` till att överföra den till lagringskontot. Denna cmdlet är optimerad för att överföra VHD-filer och kan slutföra överföringen snabbare än andra metoder. Överföringen slutförs med hjälp av flera trådar för att få bästa prestanda. När den virtuella hårddisken har överförts, kopplar du den till en befintlig virtuell dator som en datadisk. Det här är en utmärkt metod för att distribuera alla typer av data till virtuella Azure-datorer. Datan finns automatiskt i den virtuella datorn och du behöver inte partitionera eller formatera den nya disken.

### <a name="attach-a-new-data-disk-to-an-azure-vm"></a>Koppla en ny datadisk till en virtuell Azure-dator

Du kan använda Azure Portal för att lägga till en ny, tom datadisk till en virtuell dator. 

Processen skapar en **.vhd**-fil som en sidblobb i lagringskontot som du anger, samt kopplar .vhd-filen som en datadisk till den virtuella datorn.

Innan du kan använda den nya virtuella hårddisken för att lagra data, måste du initiera, partitionera och formatera den nya disken. Vi ska öva på de här stegen i nästa övning.

I fysiska, lokala servrar lagrar du data på fysiska hårddiskar. Du lagrar data på en virtuell Azure-dator på virtuella hårddiskar (VHD:er). Dessa virtuella hårddiskar lagras på Azure-lagringskonton som sidblobbar. När du exempelvis migrerar din advokatbyrås databas med fallhistorik till Azure, måste du skapa virtuella hårddiskar där databasfilerna ska sparas.
