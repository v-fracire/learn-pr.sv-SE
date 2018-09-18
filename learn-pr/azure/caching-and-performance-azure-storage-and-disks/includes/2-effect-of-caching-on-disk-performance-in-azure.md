Vi ska titta på de faktorer som påverkar diskprestanda i Azure och hur cachelagring kan hjälpa dig att optimera prestanda. 

### <a name="disk-caching"></a>Diskcachelagring

En cache är en särskild komponent i en dator som lagrar data så att de kan nås snabbare, vanligtvis i minnet. Data i ett cacheminne är ofta data som tidigare har lästs eller data som kommer från en tidigare beräkning. Målet är att komma åt data snabbare än om de läses från disken.

Cachelagring använder specialiserad och ibland dyr tillfällig lagring som har snabbare prestanda för läsning och/eller skrivning än vad permanent lagring har. Eftersom den här cachelagringen ofta är begränsad behöver man bestämma vilka dataåtgärder som gagnas mest av cachelagring. Men även där cacheminne kan göras mycket tillgängligt, till exempel i Azure, är det fortfarande viktigt att känna till arbetsbelastningmönstren för varje disk innan du bestämmer vilken typ av cachelagring som ska användas.

**Cachelagring för läsning** försöker göra datahämtning snabbare. I stället för att läsa från permanent lagring läses data från det snabbare cacheminnet. Dataläsning träffar cacheminnet under följande omständigheter.

- Data har lästs in förut och finns i cacheminnet.
- Cacheminnet är tillräckligt stort för att rymma alla dessa data.

Observera att cachelagring för läsning fungerar när det finns viss _förutsägbarhet_ i läsningskön, till exempel en uppsättning sekventiella läsåtgärder. För slumpmässig I/O, där de data som du läser är spridda över lagringen, finns det få eller inga fördelar med cachelagring, och det kan till och med minska diskprestanda.

**Cachelagring för skrivning** försöker göra dataskrivning till lagring snabbare. Genom att använda en cachelagring för skrivning kan appen överväga de data som ska sparas. I verkligheten placeras data i kö i ett cacheminne som väntar på att skrivas till permanent lagring. Den här mekanismen är en potentiell källa till fel om systemet stängs ned innan dessa cachelagrade data överförs till disk. Vissa system, till exempel SQL Server, hanterar själva skrivning av cachelagrade data till beständig disklagring.  

### <a name="azure-disk-caching"></a>Azure-diskcachelagring

Det finns två typer av diskcachelagring som berör disklagring:

- Azure-cachelagring
- Diskcachelagring för virtuella Azure-datorer

Azure-cachelagring tillhandahåller cachelagringstjänster för Azure Blob-lagring, Azure Files och annat innehåll i Azure. Konfiguration av dessa cachetyper ligger inte inom ramen för den här modulen.

Diskcachelagring för virtuella Azure-datorer handlar om att optimera läs- och skrivåtkomst till de virtuell hårddiskfiler (VHD) som ansluts till virtuella Azure-datorer. Vi fokuserar på diskcachelagring i den här modulen.

### <a name="azure-virtual-machine-disk-types"></a>Disklagringstyper för virtuella Azure-datorer

Det finns tre typer av diskar som används med virtuella Azure-datorer (VM).

- **OS-disk**: när du skapar en virtuell Azure-dator ansluter Azure automatiskt en virtuell hårddisk för operativsystemet (OS). Den virtuella hårddisken lagras som en sidblob i Azure-lagringen.
- **Tillfällig disk**: när du skapar en virtuell Azure-dator lägger Azure även automatiskt till en tillfällig disk. Den här disken används för data, till exempel sido- och växlingsfiler. Data på den här disken kan gå förlorade under underhåll eller vid omdistribuering av en virtuell dator. Därför bör du inte använda den för att lagra permanenta data såsom databasfiler eller transaktionsloggar.
- **Datadiskar**: en datadisk är en virtuell hårddisk som är ansluten till en virtuell dator för att lagra programdata eller andra data som du behöver bevara.

OS-diskar och datadiskar drar nytta av diskcachelagring för virtuella Azure-datorer. Cachestorleken för en virtuell datordisk beror på VM-instansens storlek och antalet diskar som monterats på den virtuella datorn. Cachelagring kan bara aktiveras för upp till fyra datadiskar.

## <a name="cache-options-for-azure-vms"></a>Cachelagringsalternativ för virtuella Azure-datorer

Det finns tre vanliga alternativ för VM-diskcachelagring.

- **Läsa/skriva** – återskrivningscache.  Använd bara det här alternativet om programmet hanterar skrivning av cachelagrade data till beständiga diskar när det behövs
- **Skrivskyddad** – läsningar utförs från cachen.
- **Ingen** – ingen cachelagring. Välj det här alternativet för lässkyddade och skrivintensiva diskar. Loggfiler är ett bra alternativ eftersom de är skrivintensiva åtgärder.

Inte alla cachelagringsalternativet är tillgängliga för alla disktyper. I följande tabell visas cachelagringsalternativen för varje disktyp:

| |**Skrivskyddad**  |**Läsa/skriva**  |**Ingen**  |
|---------|---------|---------|---------|
|OS-disk     |   ja      |   ja (standard)     |   ja      |
|Datadisk     |   ja (standard)      |  ja       |  ja       |
|Tillfällig disk     |  nej       |   nej      |   nej      |

> [!NOTE]
> Alternativen för diskcachelagring kan inte ändras för virtuella datorer i L-serien och B-serien.

## <a name="performance-considerations-for-azure-vm-disk-caching"></a>Prestandaöverväganden för diskcachelagring för virtuella Azure-datorer

Hur kan då dina cacheinställningar påverka prestanda för de arbetsbelastningar som körs på virtuella Azure-datorer?

### <a name="os-disk"></a>OS-disk

För en OS-disk för en virtuell dator är standardbeteendet att använda cachelagring i läget Läsa/skriva. Om du har program som lagrar datafiler på OS-disken och programmen utför många slumpmässiga läs-/skrivåtgärder till datafiler bör du överväga att flytta de filerna till en datadisk som har cachelagring inaktiverat. Det beror på att om läsningskön inte innehåller sekventiella läsåtgärder så finns det få eller inga fördelar med cachelagring. Arbetet med att underhålla cacheminnet som om data vore sekventiella kan minska diskprestanda.

### <a name="data-disks"></a>Datadiskar

För prestandakänsliga program bör du använda datadiskar i stället för OS-disken. Med hjälp av separata diskar kan du konfigurera lämpliga cacheinställningar för varje disk.

Exempel: om du aktiverar **Skrivskyddad** cachelagring för datadiskarna för virtuella Azure-datorer som kör SQL Server kan det resultera i avsevärda prestandaförbättringar. Loggfiler å andra sidan är bra kandidater för datadiskar utan cachelagring.

> [!WARNING]
> När du ändrar cacheinställningen för en Azure-disk så frånkopplas och återansluts måldisken. Om det är operativsystemdisken startas den virtuella datorn om. Stoppa alla program/tjänster som kan påverkas av det här avbrottet innan du ändrar inställningen för diskcachelagring.
>
>