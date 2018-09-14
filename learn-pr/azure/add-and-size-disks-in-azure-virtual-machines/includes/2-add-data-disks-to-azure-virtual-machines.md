Precis som alla andra datorer Använd virtuella datorer i Azure diskar som en plats för att lagra ett operativsystem, program och data. Dessa diskar kallas virtuella hårddiskar (VHD).

Anta att du har skapat en virtuell dator (VM) i Azure, vilket ska vara värd för databasen för fallet historik som din Advokatbyrå förlitar sig på. En väl utformad diskkonfigurationen är nyckeln till bra prestanda och flexibilitet för SQL Server.

I den här enheten, får du lära dig hur du väljer rätt konfigurationsvärden för dina diskar och hur du kopplar de diskarna till en virtuell dator.

## <a name="how-disks-are-used-by-vms"></a>Hur diskar som används av virtuella datorer

Virtuella datorer använder diskar för tre olika syften:

- **Operativsystemets lagringsutrymme**. Varje virtuell dator innehåller en disk som innehåller operativsystemet. Den här enheten är registrerad som en SATA-enhet och märkta som C:-enheten i Windows och monteras på ”/” i Unix-liknande operativsystem. Den har en maxkapacitet på 2 048 GB (Gigabyte) och dess innehåll hämtas från VM-avbildning som du använde för att skapa den virtuella datorn.
- **Temporär lagring**. Varje virtuell dator innehåller en tillfällig virtuell Hårddisk som används för sidan och växling filer. Data på den här enheten kan gå förlorade under en underhållshändelse eller omdistributionen. Enheten är märkta som D: på en virtuell Windows-dator som standard. Använd inte den här enheten för att lagra viktiga data som du inte vill förlora.
- **Datalagring**. En datadisk är andra disk som är ansluten till en virtuell dator. Du kan använda datadiskar för att lagra filer, databaser och andra data som du behöver för att bevara mellan omstarter. Vissa VM-avbildningar omfattar datadiskar som standard. Du kan också lägga till dina egna datadiskar upp till det maximala antal som anges av storleken på den virtuella datorn. Varje datadisk är registrerad som en SCSI-enhet och har en maxkapacitet på 4 095 GB. Du kan välja enhetsbeteckningar eller monteringspunkter för dina enheter.

## <a name="storing-vhd-files"></a>Lagra VHD-filer

I Azure, virtuella hårddiskar lagras i ett Azure storage-konto som **sidblobbar**.

I den här tabellen visas de olika typerna av lagringskonton och vilka objekt som kan användas med var och en.

|**Typ av lagringskonto**|**Allmän standard**|**Allmän premium**|**Blob Storage, frekvent och lågfrekvent åtkomstnivå**|
|-----|-----|-----|-----|
|**Tjänster som stöds**| Azure Blob storage, Azure Files, Azure Queue storage | Blob Storage | Blob Storage|
|**Typer av blobbar som stöds**|Blockblobbar, sidblobbar och tilläggsblobbar | Sidblobbar | Blockblobbar och tilläggsblobbar|

Båda Allmänt standard och premium storage stöd för sidblobar. Välj ett standardlagringskonto om kostnaden är viktigast. Premium storage kostar mer, men kommer också att leverera mycket högre i/o-åtgärder per sekund (IOPS). Om dataprestanda är ett krav för den virtuella datorn, föredra premiumlagring.

## <a name="attach-data-disks-to-vms"></a>Koppla datadiskar till virtuella datorer

Du kan lägga till datadiskar till en virtuell dator när som helst genom att koppla dem till den virtuella datorn. Ansluta en disk associerar VHD-filen med den virtuella datorn. 

Den virtuella Hårddisken kan inte tas bort från lagring när den är ansluten.

### <a name="attach-an-existing-data-disk-to-an-azure-vm"></a>Koppla en befintlig datadisk till en Azure-dator

Du kanske redan har en virtuell Hårddisk som lagrar data som du vill använda i din Azure-VM. I vårt lag fast scenario, till exempel har kanske du redan konverterat fysiska diskar för virtuella hårddiskar lokalt. I det här fallet kan du använda PowerShell `Add-AzureRmVhd` cmdlet för att överföra den till lagringskontot. Denna cmdlet är optimerad för att överföra VHD-filer och kan slutföra överföringen snabbare än andra metoder. Överföringen har slutförts med flera trådar för bästa prestanda. När den virtuella Hårddisken har överförts, kopplar du den till en befintlig virtuell dator som en datadisk. Det hanterar ett utmärkt sätt att distribuera alla typer av data till Azure virtuella datorer. Data är automatiskt finns i den virtuella datorn och behöver inte att partitionera och formatera den nya disken.

### <a name="attach-a-new-data-disk-to-an-azure-vm"></a>Koppla en ny datadisk till en Azure virtuell dator

Du kan använda Azure-portalen för att lägga till en ny, tom datadisk till en virtuell dator. 

Den här processen skapar en **VHD** filen som en sidblobb i storage-konto som du anger och koppla den VHD-filen som en datadisk till den virtuella datorn.

Innan du kan använda den nya virtuella Hårddisken för att lagra data, måste du initiera, partitionera och formatera den nya disken. Vi ska öva på de här stegen i nästa övning.

I fysiska, lokala servrar kan lagra du data på fysiska hårddiskar. Du kan lagra data i en Azure--dator (VM) på virtuella hårddiskar (VHD). Dessa VHD lagras som sidblobar i Azure storage-konton. När du migrerar din Advokatbyrå fallet historik-databas till Azure måste du skapa virtuella hårddiskar där databasfilerna ska sparas.
