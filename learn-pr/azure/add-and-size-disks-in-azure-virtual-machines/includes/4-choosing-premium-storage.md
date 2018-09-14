Vissa program placera större krav på datalagring än andra. Microsoft Exchange-servrar och SharePoint-servrar måste till exempel högpresterande diskar så att de körs bäst.

Nu ska vi gå tillbaka till scenariot för att migrera databasen fallet historik för din Advokatbyrå till Azure. Du vill se till att din advokater och andra anställda kan komma åt ärendeinformation så snabbt som möjligt. Företaget växer, och du vill att din databas för att hantera ökad efterfrågan.

Ett sätt som du kan maximera prestanda för virtuella datorer (VM) i Azure är att använda premiumlagring för virtuella hårddiskar (VHD). Premium storage tillhandahåller snabbare indata och utdata (I/O) än standardlagring. Snabbare disk-i/o resulterar i bättre prestanda för disk-intensiva program.

Nu ska vi jämför prestandanivåer för lagring och Läs mer om premium storage-konton.

## <a name="how-premium-storage-differs-from-standard-storage"></a>Hur skiljer sig premium-lagring från standardlagring

I Azure implementeras premium-lagring på solid state-hårddiskar (SSD). SSD-enheter har högre i/o-prestanda än hårddiskar (HDD) och kan vara mer tillförlitliga eftersom de har inga rörliga delar. En läs- eller skrivtransaktion huvud har inte att flytta till rätt plats på en disk för att hitta de data som begärdes. 

Standardlagring implementeras på hårddiskar. Standardlagring faktureras med en lägre hastighet. Välj standardlagring kontrollera kostnader och välj premiumlagring när snabba i/o-prestanda krävs.

Du kan också välja att använda en blandning av standard och premium-lagring per disk i en enda virtuell dator.

> [!NOTE]
> Om du använder hanterade diskar måste du ha ett tredje alternativ: standard SSD-enheter. Standard SSD-enheter är mellanliggande i både prestanda och pris mellan standard HDD- och premium SSD-enheter men är inte tillgängliga för ohanterade diskar. Du kommer lära dig mer om standard SSD-enheter senare i den här modulen.

## <a name="premium-storage-accounts"></a>Premium storage-konton

Virtuella hårddiskar lagras i Azure-lagringskonton som sidblobar. Om du vill använda premium-lagring för dina VHD: er, måste du lagra virtuella hårddiskar i en **premiumlagringskonto**. Du kan välja prestandanivån för en storage-konto när du har skapat den, och du kan inte ändra den här inställningen senare.

> [!IMPORTANT]
> Premium-lagring kanske inte tillgänglig i alla Azure-regioner. Du kan kontrollera tillgängligheten för din region [produkttillgänglighet per region](https://azure.microsoft.com/en-us/global-infrastructure/services/).

### <a name="data-replication-and-premium-storage-accounts"></a>Data replikering och premium storage-konton

Data i ditt Microsoft Azure-lagringskonto replikeras alltid för att säkerställa hållbarhet och hög tillgänglighet. Azure Storage replikering kopierar dina data så att den är skyddad från planerade och oplanerade händelser som tillfälliga maskinvarufel, power meddelanden, massiv naturkatastrofer och så vidare. Du kan välja att replikera dina data inom samma Datacenter över zonindelad datacenter inom samma region och i olika regioner. Det finns fyra typer av replikering:

- **Lokalt redundant lagring (LRS)** – Azure replikerar data inom samma Azure-datacenter. Data förblir tillgängliga om en nod misslyckas. Men om det inte går att ett helt datacenter, kanske data inte tillgänglig.
- **GEO-redundant lagring (GRS)** – Azure replikerar data till en sekundär region som ligger hundratals mil bort från den primära regionen. Om ditt lagringskonto har GRS aktiverat, sedan dina data är beständiga även om det finns en fullständig regionalt strömavbrott eller en katastrof där den primära regionen inte återställas.
- **Läsåtkomst till geografiskt redundant lagring (RA-GRS)** – Azure tillhandahåller skrivskyddad åtkomst till data i den sekundära platsen och geo-replikering mellan två regioner. Om det inte går att ett datacenter, data förblir läsbara men kan inte ändras.
- **Zonredundant lagring (ZRS)** – Azure replikerar dina data synkront i tre lagringskluster i en enda region. Varje lagringskluster är fysiskt avgränsade från de andra och ligger i sin egen tillgänglighetszon (AZ). Med den här typen av replikering kan du fortfarande åtkomst till och hantera dina data i händelse av att en zon blir otillgänglig.

Standard storage-konton stöder alla typer av replikering, men premium storage-konton har endast stöd för lokalt redundant lagring (LRS). Eftersom själva de virtuella datorerna körs i en enda region, inte den här begränsningen normalt ett problem för VHD-lagring.

## <a name="migrating-from-standard-storage-to-premium-storage"></a>Migrera från standardlagring till premium storage

Det är bäst att skapa virtuella hårddiskar i rätt typ av storage-konto i första hand. Men kraven förändras ibland, efterfrågan ökar eller du upptäcker att du har valt fel typ. Överväg att flytta till premium storage i sådana situationer.

Överväg att det ska se ut vissa avbrott under vilken den virtuella datorn blir inte tillgängliga för användarna innan du påbörjar en migrering.

> [!NOTE]
> Fullständig information om migrering från standard till premium storage finns [migrera till Azure Premium Storage (ohanterade diskar)](https://docs.microsoft.com/azure/storage/common/storage-migration-to-premium-storage).

När du skapar en virtuell dator i Azure kan behöva du hitta rätt balans mellan pris och prestanda. Premium storage är ett bra val för disk-intensiva program som databaser, eftersom den har stöd för i/o för snabbare än standardlagring.
