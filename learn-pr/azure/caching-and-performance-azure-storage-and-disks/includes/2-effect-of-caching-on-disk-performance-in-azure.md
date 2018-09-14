Vi ska titta på de faktorer som påverkar diskprestanda i Azure och hur cachelagring hjälpa dig att optimera prestanda.

### <a name="disk-caching"></a>Diskcachelagring

En cache är en särskild komponent på en dator som lagrar data så att den kan nås snabbare, vanligtvis i minnet. Data i ett cacheminne är ofta data som tidigare har lästs eller är data som kommer från en tidigare beräkning. Målet är att komma åt data snabbare än komma från disken.

Cachelagring använder specialiserade och ibland dyra, tillfälligt lagringsutrymme som har snabbare läsa och/eller skriva prestanda än permanent lagring. Eftersom den här cachelagring är ofta begränsad, behöver beslut besluta vad dataåtgärder får mest cachelagring. Men även där cacheminnet kan göras allmänt tillgänglig, som i Azure, det är fortfarande är viktigt att veta arbetsbelastningmönster för varje disk innan du bestämmer dig på vilken typ av cachelagring att använda.

**Läsa cachelagring** försöker göra Datahämtningen snabbare. Data läses från snabbare cachen i stället för att läsa från permanent lagring. Dataläsning träffar på följande villkor:

- Data har lästs innan och finns i cacheminnet.
- Cacheminnet är tillräckligt stor för att rymma alla dessa data.

Det är viktigt att notera att läscachelagring fungerar när det finns några _förutsägbarhet_ till den skrivskyddade kö, till exempel en uppsättning sekventiella läsåtgärder. För slumpmässiga i/o, där de data som du försöker komma åt spridda över storage, caching blir för liten eller ingen förmånen och kan även minska diskprestanda.

**Skrivcache** försöker att snabba upp skriva data till lagring. Appen kan med hjälp av en skrivcache, Överväg att data sparas. I verkligheten kan data är i kö i ett cacheminne, väntar på att skrivas till permanent lagring. Den här mekanismen kan vara en potentiell felpunkt, t.ex. om systemet stängs ned innan det cachelagrade data rensas eftersom du kan föreställa dig till disk. Vissa system, till exempel SQL Server, hantera skriva cachelagrade data till beständig disklagring själva.

### <a name="azure-disk-caching"></a>Azure diskcachelagring

Det finns två typer av diskcachelagringstypen den problem disklagring:

- Azure storage cachelagring
- Diskcachelagring för Azure-dator (VM)

Cachelagring av Azure storage tillhandahåller cachetjänster för Azure Blob storage, Azure Files och annat innehåll i Azure. Konfiguration av dessa typer av cache är utanför omfattningen för den här modulen.

Azure-dator diskcachelagring handlar om att optimera Läs- och skrivåtkomst till virtuell hårddisk (VHD)-filer som bifogas virtuella Azure-datorer. Vi att fokusera på diskcachelagring i den här modulen.

### <a name="azure-virtual-machine-disk-types"></a>Azure VM-disktyper

Det finns tre typer av diskar som används med virtuella Azure-datorer:

- **OS-disken**: när du skapar en Azure-dator, bifogar Azure automatiskt en virtuell Hårddisk för operativsystemet (OS). Den virtuella Hårddisken lagras som en sidblobb i Azure storage.
- **Temporär disk**: när du skapar en Azure VM, Azure också lägger automatiskt till en tillfällig disk. Den här disken används för data, till exempel sida och växling. Data på den här disken kan gå förlorade under underhåll eller en VM redeploy. Därför inte använda det för permanent datalagring, till exempel databasfiler eller transaktionsloggar.
- **Datadiskar**: en datadisk är en virtuell Hårddisk som är kopplad till en virtuell dator för att lagra programdata eller andra data som du behöver.

OS och datadiskar dra nytta av virtuell Azure-dator diskcachelagring. Cachestorleken för en virtuell datordisk beror på Virtuella datorinstansens storlek och antalet diskar som monterats på den virtuella datorn. Cachelagring kan aktiveras för endast upp till fyra datadiskar.

## <a name="cache-options-for-azure-vms"></a>Cachealternativ för virtuella Azure-datorer

Det finns tre vanliga alternativ för cachelagring av VM-disk:

- **Läs/Skriv** – återskrivningscache. Använd det här alternativet endast om ditt program hanterar korrekt skriva cachelagrade data till beständiga diskar vid behov.
- **Skrivskyddad** -läsningar utförs från cachen.
- **Ingen** -ingen cachelagring. Välj det här alternativet för lässkyddad och skrivintensiv diskar. Loggfiler är bra eftersom de skrivintensiv åtgärder.

Inte alla cachelagring alternativet är tillgängligt för varje typ av disk. I följande tabell visas cachelagringsalternativ för varje typ av disk:

| |**Skrivskyddad**  |**Läs/Skriv**  |**Ingen**  |
|---------|---------|---------|---------|
|OS-disk     |   ja      |   Ja (standard)     |   ja      |
|Datadisk     |   Ja (standard)      |  ja       |  ja       |
|Temporär disk     |  nej       |   nej      |   nej      |

> [!NOTE]
> Diskcachelagringstypen alternativ kan inte ändras för L-serien och virtuella datorer i B-serien.

## <a name="performance-considerations-for-azure-vm-disk-caching"></a>Prestandaöverväganden för cachelagring av Virtuella Azure-disk

Så, hur kan din cacheinställningarna påverkar prestandan för dina arbetsbelastningar som körs på virtuella Azure-datorer?

### <a name="os-disk"></a>OS-disk

För en virtuell dator OS-disk är standardbeteendet att använda cache med läs-/ skrivbehörighet. Om du har program som lagrar datafiler på OS-disken och programmen har massor av slumpmässig läsning/skrivning åtgärder till datafiler, Överväg att flytta filerna till en datadisk som har cachelagring inaktiverad. Varför är som? Om skrivskyddade kön inte innehåller sekventiella läsåtgärder, blir cachelagring, för liten eller ingen-förmånen. Att underhålla cachen som om data som var sekventiella, kan minska diskprestanda.

### <a name="data-disks"></a>Datadiskar

För resultatdrivna program, bör du använda datadiskar i stället för OS-disken. Med hjälp av separata diskar kan du konfigurera lämplig cacheinställningarna för varje disk.

Till exempel kör SQL Server på Azure Virtual Machines, aktivera **skrivskyddad** cachelagring på datadiskar (för vanliga och TempDB-data) kan medföra betydande prestandaförbättringar. Loggfiler, å andra sidan är bra kandidater för datadiskar med ingen cachelagring.

> [!WARNING]
> Om du ändrar cache-inställningen för en Azure-disk frånkopplas och återansluts sedan måldisken. Om det är ingen operativsystemdisk startas den virtuella datorn. Stoppa alla program/tjänster som kan påverkas av den här avbrott innan du ändrar inställningen för disk-cache.
