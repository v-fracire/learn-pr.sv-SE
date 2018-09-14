Låt oss börja med att ta en titt på Azure storage-tjänster, data-format och konton. 

Microsoft Azure Storage är en hanterad tjänst som tillhandahåller hållbar, säker och skalbar lagring i molnet. Lås oss dessa villkor.

| | |
|-|-|
| **Hållbar** | Redundans garanterar att dina data är säkra i händelse av tillfälliga maskinvarufel. Du kan också replikera data mellan Datacenter eller geografiska regioner för ytterligare skydd mot lokala allvarliga händelser eller naturkatastrofer. Data som replikeras på det här sättet förblir mycket tillgängliga vid ett oväntat avbrott. |
| **Skydda** | Alla data som skrivs till Azure Storage krypteras av tjänsten. Med Azure Storage får du detaljerad kontroll över vem som har tillgång till dina data. |
| **Skalbar** | Azure Storage är utformat för att vara mycket skalbart för att uppfylla krav på datalagring och prestanda för dagens program. |
| **Hanterad** | Microsoft Azure tar hand om underhåll och hanterar kritiska problem åt dig. |

En enda Azure-prenumeration kan ha upp till 200 lagringskonton, som kan innehålla 500 TB data. Om du har en affärsfall kan du kommunicera med Azure Storage-teamet och få ett godkännande för upp till 250 lagringskonton i en prenumeration, vilket gör att din maximalt lagringsutrymme upp till 125 Petabyte!

## <a name="azure-data-services"></a>Azure-datatjänster

Azure storage innehåller fyra typer av data.

- **Blobbar**: en mycket skalbar objektlagring för text eller binära data.
- **Filer**: hanterade filresurser för molnet eller lokala distributioner.
- **Köer**: ett meddelandearkiv för tillförlitliga meddelandefunktioner mellan programkomponenter.
- **Tabeller**: en NoSQL-lagring av schemalös lagring av strukturerade data. Den här tjänsten har ersatts av Cosmos DB och kommer inte beskrivs här.

Alla dessa datatyper i Azure Storage kan nås från var som helst i världen via HTTP eller HTTPS. Microsoft tillhandahåller SDK: er för Azure Storage i ett flertal språk, samt ett REST-API. Du kan också visuellt Utforska dina data direkt i Azure-portalen.

### <a name="blob-storage"></a>Blob Storage

Azure Blob storage är en lösning för lagring av objekt optimerad för att lagra stora mängder Ostrukturerade data, exempelvis text eller binära data. Blob Storage är perfekt för:

- Leverera bilder eller dokument direkt till en webbläsare, inklusive fullständig serverstatiska webbplatser.
- Lagra filer för distribuerad åtkomst.
- Direktuppspelning av video och ljud.
- Lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering.
- Lagra data för analys av en tjänst som kan vara lokal eller Azure-baserad.

Azure Storage stöder tre typer av blobbar:

| Blobtyp | Beskrivning |
|-----------|-------------|
| **Blockblob-objekt** | Blockblob-objekt används för att lagra text eller binära filer på upp till ~ 5 TB (50 000 block med 100 MB) i storlek. I första hand för blockblob-objekt är lagring av filer som läses från början till slut som mediafiler eller bildfiler för webbplatser. De har namngetts blockblobar eftersom filer som är större än 100 MB måste laddas upp som små block som är konsoliderade (eller sedan allokerat) till sista blob. |
| **Sidblob-objekt** | Sidblobar används för att lagra direktåtkomst filer på upp till 8 TB i storlek. Sidblobar används främst som datalagret för de virtuella hårddiskarna som används för att ange beständiga diskar för Azure Virtual Machines (Azure virtuella datorer). De har namngetts sidblobar eftersom de ger slumpmässig läsning/skrivning åtkomst till 512 byte-sidor. |
| **Tilläggsblobbar** | Lägg till BLOB-objekt består av block som blockblobbar, men de är optimerade för tilläggsåtgärder. De används ofta för att logga information från en eller flera källor till samma blob. Du kan till exempel skriva alla dina spårningsloggning till samma tilläggsblobb för ett program som körs på flera virtuella datorer. En enda tilläggsblobb kan vara upp till 195 GB. |

### <a name="files"></a>Filer

Azure Files kan du konfigurera filresurser i nätverk med hög tillgänglighet som kan komma åt via standardprotokollet för Server Message Block (SMB). Det innebär att flera virtuella datorer kan dela samma filer med både läs- och skrivbehörighet. Du kan också läsa filerna med hjälp av REST-gränssnittet eller klientbiblioteken för lagring. Du kan även associera en unik URL till en fil för att ge detaljerad åtkomst till en privat fil för en angiven tidsperiod. Filresurser kan användas för många vanliga scenarier:

- Lagra delade konfigurationsfiler för virtuella datorer, verktyg eller verktyg så att alla använder samma version.
- Loggfiler, till exempel diagnostik, mått och kraschdumpar.
- Delade data mellan lokala program och virtuella Azure-datorer för att tillåta migrering av appar till molnet under en viss tidsperiod.

### <a name="queues"></a>Köer

Azure-kötjänsten används för att lagra och hämta meddelanden. Kömeddelanden kan vara upp till 64 kB och en kö kan innehålla miljontals meddelanden. Köer används vanligtvis för att lagra listor med meddelanden som ska bearbetas asynkront.

Du kan använda köer för att ansluta löst olika delar av ditt program tillsammans. Vi kan exempelvis utföra bearbetning av avbildning på foton som laddats upp av våra användare. Kanske vi vill ge någon form av ansiktsigenkänning eller taggning funktion, så att personer kan söka igenom alla bilder som de har lagrat i vår tjänst. Vi kan använda köer för att skicka meddelanden till vår bildbehandlingstjänst så att den vet att nya avbildningar har laddats upp och är redo för bearbetning. Den här typen av arkitektur skulle vara möjligt att utveckla och uppdatera varje del av tjänsten oberoende av varandra.

## <a name="azure-storage-accounts"></a>Azure-lagringskonton

För att komma åt dessa tjänster från ett program måste du skapa en _lagringskonto_. Storage-konto tillhandahåller ett unikt namnområde i Azure för att lagra och komma åt dina dataobjekt. Ett lagringskonto innehåller alla blobar, filer, köer, tabeller och VM-diskar som du skapar med det kontot.

### <a name="creating-a-storage-account"></a>Skapa ett lagringskonto

Du kan skapa ett Azure storage-konto med Azure-portalen, Azure PowerShell eller Azure CLI. Azure Storage tillhandahåller tre olika kontoalternativ med olika priser och funktioner som stöds.

> [!div class="mx-tableFixed"]
> | Kontotyp | Beskrivning |
> |--------------|-------------|
> | **Generell användning v2 (GPv2)** | GPv2-konton (General-purpose v2) är lagringskonton som stöder alla de senaste funktionerna för blobar, filer, köer och tabeller. Prisstrukturen för GPv2-konton har utformats med låga gigabytepriser. |
> | **General-Purpose v1 (GPv1)** | General-Purpose v1 (GPv1)-konton ger åtkomst till alla Azure Storage-tjänster men har inte de senaste funktionerna eller det lägsta gigabytepriset. Lågfrekvent lagring och arkivlagring är två exempel på funktioner som inte stöds av GPv1. Transaktionspriserna är lägre för GPPv1, så arbetsbelastningar med hög omsättning eller många läsåtgärder kan ha nytta av den här kontotypen. |
> | **BLOB storage-konton** | En äldre kontotyp, blob storage-konton stöder samma funktioner för blockblobar som GPv2, men de är begränsade till stöder endast block och tilläggsblobbar. Prisstrukturen liknar den som används för GPv2-konton. |

Om du vill veta mer om hur du skapar lagringskonton, se till att gå igenom den **skapa ett Azure storage-konto** modul i learning-portalen.