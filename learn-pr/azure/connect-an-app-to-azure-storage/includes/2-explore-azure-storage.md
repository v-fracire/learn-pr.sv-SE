Vi börjar med att ta en snabb titt på Azure Storage-tjänster, dataformat och konton. 

Microsoft Azure Storage är en hanterad tjänst som erbjuder pålitlig, säker och skalbar lagring i molnet. Vi ska gå igenom termerna.

| | |
|-|-|
| **Pålitlig** | Redundans garanterar att dina data är säkra i händelse av tillfälliga maskinvarufel. Du kan också replikera data i datacenter eller geografiska regioner för ytterligare skydd mot lokala allvarliga händelser eller naturkatastrofer. Data som replikeras på det här sättet förblir mycket tillgängliga vid ett oväntat avbrott. |
| **Skydda** | Alla data som skrivs till Azure Storage krypteras av tjänsten. Med Azure Storage får du detaljerad kontroll över vem som har tillgång till dina data. |
| **Skalbarhet** | Azure Storage är utformat för att vara mycket skalbart för att uppfylla krav på datalagring och prestanda för dagens program. |
| **Hanterade** | Microsoft Azure tar hand om underhåll och hanterar kritiska problem åt dig. |

En enda Azure-prenumeration kan ha upp till 200 lagringskonton, och var och en kan innehålla 500 TB data. Om du har ett affärsärende kan du prata med Azure Storage-teamet och få godkännande för upp till 250 lagringskonton i en prenumeration, vilket utökar din maxlagring upp till 125 petabyte!

## <a name="azure-data-services"></a>Azure-datatjänster

Azure Storage innehåller fyra typer av data:

- **Blobbar**: En mycket skalbar objektlagring för textdata och binära data.
- **Files**: Hanterade filresurser för distributioner i molnet eller lokalt.
- **Köer**: Ett meddelandearkiv för tillförlitliga meddelandefunktioner mellan programkomponenter.
- **Tabeller**: En NoSQL-lagring av schemalös lagring av strukturerade data. Den här tjänsten har ersatts av Cosmos DB och kommer inte att beskrivas här.

Alla datatyper i Azure Storage är åtkomliga från hela världen via HTTP eller HTTPS. Microsoft tillhandahåller SDK:er för Azure Storage på många olika språk samt en REST API. Du kan även visuellt utforska dina data direkt i Azure-portalen.

### <a name="blob-storage"></a>Blob Storage

Azure Blob Storage är en lösning för objektlagring som är optimerat för att lagra stora mängder ostrukturerade data, exempelvis text eller binära data. Blob Storage är perfekt för att:

- Leverera bilder eller dokument direkt till en webbläsare, inklusive en fullständigt statiska webbplatser.
- Lagra filer för distribuerad åtkomst.
- Direktuppspelning av video och ljud.
- Lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering.
- Lagra data för analys av en tjänst som kan vara lokal eller Azure-värdbaserad.

Azure Storage stöder tre typer av blobar:

| Blobtyp | Beskrivning |
|-----------|-------------|
| **Blockblobar** | Blockblobar används till att lagra text- eller binära filer med en storlek på upp till ~5 TB (50 000 block på 100 MB). I första hand används blockblobar för lagring av filer som läses från början till slut, som mediefiler eller bildfiler för webbplatser. De kallas för blockblobar eftersom filer som är större än 100 MB måste laddas upp som små block, som därefter konsolideras (eller checkas in) i den slutgiltiga bloben. |
| **Sidblobar** | Sidblobar används för att lagra filer med slumpmässig åtkomst upp till 8 TB. Sidblobar används främst för stödlagring av VHD:erna som används för att tillhandahålla tåliga diskar för Azure Virtual Machines (Azure VMs). De kallas för sidblobar eftersom de erbjuder slumpmässig läs-/skrivåtkomst till sidor på 512 bytes. |
| **Tilläggsblobar** | Tilläggsblobar består av block precis som blockblobarna, men är optimerade för tilläggsåtgärder. De används ofta för att logga information från en eller flera källor till samma blob. Du kan till exempel skriva all din spårningsloggning till samma tilläggsblob för ett program som körs på flera virtuella datorer. En enda tilläggsblob kan ha upp till 195 GB. |

### <a name="files"></a>Files

Med Azure Files kan du konfigurera nätverksfilresurser med hög tillgänglighet som kan nås via SMB-standardprotokollet (Server Message Block). Det innebär att flera virtuella datorer kan dela samma filer med både läs- och skrivbehörighet. Du kan också läsa filerna med hjälp av REST-gränssnittet eller klientbiblioteken för lagring. Du kan även associera en unik URL till en fil för att ge detaljerad åtkomst till en privat fil för en angiven tidsperiod. Filresurser kan användas för många vanliga scenarier:

- Lagra delade konfigurationsfiler för virtuella datorer, verktyg och hjälpmedel så att alla använder samma version.
- Loggfiler, till exempel diagnostik, mått och kraschdumpar.
- Delade data mellan lokala program och virtuella Azure-datorer för att tillåta migrering av appar till molnet under en viss tidsperiod.

### <a name="queues"></a>Köer

Azure-kötjänsten används för att lagra och hämta meddelanden. Kömeddelanden kan vara upp till 64 kB och en kö kan innehålla miljontals meddelanden. Köer används vanligtvis för att lagra listor med meddelanden som ska bearbetas asynkront.

Du kan använda köer för att löst koppla ihop olika delar av programmet. Vi kan till exempel utföra bildbearbetning på fotona som användarna har laddat upp. Vi kanske vill erbjuda någon slags ansiktsavkänning eller taggningsfunktion så att människor kan söka igenom alla bilder som de har lagrat i tjänsten. Vi skulle kunna använda köer för att skicka meddelanden till vår bildbehandlingstjänst för att informera om att nya bilder har laddats upp och är redo för bearbetning. Den här sortens arkitektur skulle göra det möjligt för dig att utveckla och uppdatera varje del av tjänsten oberoende.

## <a name="azure-storage-accounts"></a>Azure-lagringskonton

För att få åtkomst till några av tjänsterna från ett program måste du skapa ett _lagringskonto_. Azure-lagringskontot tillhandahåller en unik namnrymd i Azure där du kan lagra och få åtkomst till dina dataobjekt. Ett lagringskonto innehåller alla blobar, filer, köer, tabeller och VM-diskar som du skapar med det kontot.

### <a name="creating-a-storage-account"></a>Skapa ett lagringskonto

Du kan skapa ett Azure Storage-konto med hjälp av Azure-portalen, Azure PowerShell eller Azure CLI. Azure Storage har tre olika kontoalternativ med olika priser och olika funktioner som stöds.

> [!div class="mx-tableFixed"]
> | Kontotyp | Beskrivning |
> |--------------|-------------|
> | **General-purpose v2 (GPv2)** | GPv2-konton (General-purpose v2) är lagringskonton som stöder alla de senaste funktionerna för blobar, filer, köer och tabeller. Prisstrukturen för GPv2-konton har utformats för att erbjuda de lägsta gigabytepriserna. |
> | **General-purpose v1 (GPv1)** | GPv1-konton (General-purpose v1) ger åtkomst till alla Azure Storage-tjänster, men erbjuder kanske inte de senaste funktionerna eller det lägsta gigabytepriset. Lågfrekvent lagring och arkivlagring stöds till exempel inte i GPv1. Transaktionspriserna är lägre för GPPv1, så arbetsbelastningar med hög omsättning eller många läsåtgärder kan ha nytta av den här kontotypen. |
> | **Blob Storage-konton** | Blob Storage-konton är en äldre kontotyp som stöder samma funktioner för blockblobbar som GPv2, men stödet är begränsat till blockblobbar och tilläggsblobbar. Prissättningen liknar den som används för GPv2-konton. |

Om du vill veta mer om att skapa lagringskonton ska du gå igenom modulen **Skapa ett Azure Storage-konto** i utbildningsportalen.