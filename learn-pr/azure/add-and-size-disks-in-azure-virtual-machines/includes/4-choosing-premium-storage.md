Vissa program har större krav på datalagring än andra. Microsoft Exchange-servrar och SharePoint-servrar måste till exempel ha högpresterande diskar för att kunna köras på ett optimalt sätt.

Nu ska vi gå tillbaka till scenariot för att migrera databasen med fallhistorik för din advokatbyrå till Azure. Du vill se till att dina advokater och andra anställda kan komma åt informationen så snabbt som möjligt. Företaget växer och du vill att din databas ska kunna hantera en ökad efterfrågan.

Ett sätt som du kan maximera prestandan för virtuella datorer på i Azure, är att använda Premium Storage för virtuella hårddiskar (VHD). Premium Storage ger snabbare indata och utdata (I/O) än Standard Storage. Snabbare disk-I/O ger bättre prestanda för hårddiskintensiva program.

Vi ska jämföra prestandanivåerna för lagring och lära oss mer om Premium Storage-konton.

## <a name="how-premium-storage-differs-from-standard-storage"></a>Hur skiljer sig Premium Storage från Standard Storage?

I Azure implementeras Premium Storage på solid state-hårddiskar (SSD). SSD:er har högre I/O-prestanda än hårddiskar (HDD:er) och kan vara mer tillförlitliga eftersom de inte har några rörliga delar. Ett läs- eller skrivhuvud behöver inte flyttas till rätt plats på en disk för att kunna hitta de data som efterfrågas. 

Standard Storage implementeras på HDD:er. Standard Storage kostar mindre. Välj Standard Storage för att styra kostnader och välj Premium Storage när snabb I/O-prestanda krävs.

Du kan också välja att använda en blandning av Standard Storage och Premium Storage per disk på en enskild virtuell dator.

> [!NOTE]
> Om du använder hanterade diskar har du ett tredje alternativ: Standard SSD:er. Standard SSD:er ligger mittemellan Standard HDD:er och Premium SSD:er i både prestanda och pris, men de går inte att använda på ohanterade diskar. Du kommer lära dig mer om Standard SSD:er senare i den här modulen.

## <a name="premium-storage-accounts"></a>Premium Storage-konton

I Azure lagras virtuella hårddiskar på ett Azure-lagringskonto för generell användning som sidblobbar. Om du vill använda Premium Storage för dina VHD:er måste du lagra dem på ett **Premium Storage-konto**. Du väljer prestandanivån för ett lagringskonto när du skapar det och du kan inte ändra den här inställningen senare.

> [!IMPORTANT]
> Premium Storage kanske inte finns tillgängligt i alla Azure-regioner. Du kan kontrollera tillgängligheten för din region i [Produkttillgänglighet per region](https://azure.microsoft.com/en-us/global-infrastructure/services/).

### <a name="data-replication-and-premium-storage-accounts"></a>Datareplikering och Premium Storage-konton

Datan på ditt Microsoft Azure-lagringskonto replikeras alltid för att säkerställa hållbarhet och hög tillgänglighet. Azure Storage-replikering kopierar dina data så att de är skyddade mot planerade och oplanerade händelser som tillfälliga maskinvarufel, elavbrott, naturkatastrofer och så vidare. Du kan välja att replikera dina data inom samma datacenter, över zonindelade datacentra inom samma region, och till och med i olika regioner. Det finns fyra typer av replikering:

- **Lokalt redundant lagring (LRS)** – Azure replikerar datan inom samma Azure-datacenter. Datan förblir tillgänglig om en nod slutar att fungera. Men om hela datacentret slutar att fungera, kanske datan inte är tillgänglig.
- **Geo-redundant lagring (GRS)** – Azure replikerar datan till en sekundär region som ligger hundratals kilometer från den primära regionen. Om ditt lagringskonto har GRS aktiverat, är dina data beständiga även om det sker ett fullständig regionalt strömavbrott eller en katastrof där den primära regionen inte återställas.
- **Geo-redundant lagring med läsåtkomst (RA-GRS)** – Azure tillhandahåller skrivskyddad åtkomst till data på den sekundära platsen och geo-replikering mellan två regioner. Om ett datacenter slutar att fungera förblir datan läsbar, men den kan inte ändras.
- **Zonredundant lagring (ZRS)** – Azure replikerar dina data synkront över tre lagringskluster i en enda region. Varje lagringskluster är fysiskt avgränsat från de andra och ligger i sin egen tillgänglighetszon (AZ). Med den här typen av replikering har du fortfarande åtkomst till och kan hantera dina data även om en zon blir otillgänglig.

Standard Storage-konton stöder alla typer av replikering, men Premium Storage-konton har endast stöd för lokalt redundant lagring (LRS). Eftersom de virtuella datorerna körs i en enda region, är den här begränsningen normalt inte något problem vid VHD-lagring.

## <a name="migrating-from-standard-storage-to-premium-storage"></a>Migrera från Standard Storage till Premium Storage

Det är bäst att skapa virtuella hårddiskar i rätt typ av lagringskonto direkt. Men kraven förändras ibland, efterfrågan ökar eller du upptäcker att du har valt fel typ. Överväg att flytta till Premium Storage i sådana situationer.

Innan du påbörjar en migrering bör du fundera på om det kommer att medföra något driftstopp när den virtuella datorn inte är tillgänglig för användarna.

> [!NOTE]
> Fullständig information om migrering från Standard till Premium Storage finns i [Migrera till Azure Premium Storage (ohanterade diskar)](https://docs.microsoft.com/azure/storage/common/storage-migration-to-premium-storage).

När du skapar en virtuell dator i Azure måste du hitta rätt balans mellan pris och prestanda. Premium Storage är ett bra val för hårddiskintensiva program som databaser, eftersom den har stöd för snabbare I/O än Standard Storage.
