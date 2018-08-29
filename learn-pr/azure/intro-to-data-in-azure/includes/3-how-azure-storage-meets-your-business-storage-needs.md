När du tittar på fördelarna med Azure Storage kommer du att se att Azure erbjuder de bästa alternativen för lagring av din utbildningsportal. Nu ska vi utforska fördelarna och alternativen som är tillgängliga med Azure Storage mer i detalj för att se hur Azure kan uppfylla dina affärsbehov.

## <a name="how-azure-storage-can-meet-your-business-storage-needs"></a>Så här kan Azure Storage uppfylla lagringsbehoven på ditt företag

Azure Storage tillhandahåller flera alternativ som är anpassade efter de specifika typerna av datalagring.

### <a name="azure-sql-database"></a>Azure SQL Database

**Azure SQL Database** är en robust helt hanterad molnbaserad relationsdatabas som lagrar alla dina data. Du kan använda den här funktionen för att lagra data som du ofta använder och uppdaterar, till exempel personlig och utbildningsrelaterad information om din personal. Du kan också migrera dina befintliga SQL Server-databaser utan att ändra dina program.

![AzureSQL](../images/Azure_SQL.png)

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Den prisbelönta Azure-funktionen Azure Cosmos DB är en globalt distribuerad databastjänst. Funktionen stöder schemalösa data för utveckling av dynamiska program som *alltid är aktiva*, vilket ger stöd för data som ändras kontinuerligt. Du kan använda den här funktionen för att lagra data som uppdateras och underhålls baserat på indata från användare i hela världen.

![CosmosDB](../images/Azure_cosmos_db.png)

### <a name="azure-blob-storage"></a>Azure Blob Storage

Med Azure Blob Storage kan du strömma stora video- eller ljudfiler direkt till användarens webbläsare var som helst i världen. Blob Storage används också för att lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering. Azure Blob Storage kan lagra upp till 8 TB data för fillagring för virtuella datorer.

![AzureBlob](../images/Azure_blob.png)

### <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

Med funktionen Data Lake i Azure Storage kan du analysera dataanvändningen och skapa rapporter baserat på resultatet. En datasjö är en stor databas som lagrar både strukturerade och ostrukturerade data.

**Azure Data Lake Storage Gen2** kombinerar skalbarheten och kostnadsfördelarna med objektlagring med tillförlitligheten och prestanda hos funktionerna i Big Data-filsystem.

![Data_lake_Store_concept](../images/Data_lake_store_concept.png)

### <a name="azure-files"></a>Azure Files

Azure Files erbjuder fullständigt hanterade filresurser i molnet. Program som körs i Azure kan enkelt dela filer mellan virtuella datorer. Du kan använda Azure-filresurser på samma gång för molnet eller lokala distributioner av Windows, Linux och macOS.

![Azure_Files](../images/Azure_Files.png)

### <a name="azure-queue"></a>Azure Queue

Azure Queue Storage är en tjänst för lagring av stora mängder meddelanden som kan nås var som helst i världen. Ett enskilt kömeddelande har en storlek på upp till 64 kB och en kö kan innehålla flera miljoner meddelanden.

Kön används främst:

- För att skapa en lista med kvarvarande arbetsuppgifter och för att skicka meddelanden mellan olika Azure-webbservrar.
- För att belastningsutjämna mellan olika webbservrar/infrastrukturer och hantera trafiktoppar.
- För att säkerställa återhämtning efter komponentfel när flera användare kommer åt dina data samtidigt.

![Azure_Queue](../images/Azure_Queue.png)

### <a name="azure-standard-storage"></a>Azure Standard Storage

Virtuella datorer i Azure använder diskar för att lagra ett operativsystem, program och data. Azure Standard Storage tillhandahåller tillförlitligt, billigt diskstöd för virtuella datorer som kör arbetsbelastningar som inte är verksamhetskritiska. Med Standard Storage lagras data på hårddiskar (HDD).

När du arbetar med virtuella datorer kan du använda SSD-standarddiskar och HDD-standarddiskar för mindre kritiska arbetsbelastningar, och SSD-premiumdiskar för verksamhetskritiska produktionsprogram. Azures diskar har konsekvent levererat tillförlitlighet i företagsklass, med en branschledande årlig felfrekvens på 0 %.

![Azure_disk](../images/Azure_disks.png)

### <a name="storage-tiers"></a>Lagringsnivåer

Azure Storage erbjuder tre lagringsnivåer för lagring av blobbobjekt:

1. **Frekvent lagringsnivå** – Azures frekventa lagringsnivå är optimerad för att lagra data som används ofta. 
1. **Lågfrekvent lagringsnivå** – Azures lågfrekventa lagringsnivå är optimerad för att lagra data som inte används ofta och som lagras i minst 30 dagar.
1. **Arkivlagringsnivå** – Arkivlagringsnivån i Azure är optimerad för att lagra data som används sällan och som lagras i minst 180 dagar med flexibla svarstidskrav. Arkivlagring i Azure är perfekt för lagring av äldre versioner av dina data så att du kan hämta dem om det krävs för granskning eller vid andra aktiviteter som inträffar mer sällan.

![Archive_Tier](../images/Archive_Storage_Tier.png)

### <a name="azure-storage-encryptionreplication"></a>Kryptering/replikering i Azure Storage

Azure Storage tillhandahåller säkerhet och hög tillgänglighet för dina data via krypterings- och replikeringsfunktioner.

#### <a name="encryption-for-storage-services"></a>Kryptering för lagringstjänster

Följande typer av kryptering är tillgängliga för dina resurser:

1. **Azure Storage Service Encryption (SSE)** tillhandahåller kryptering i vila och hjälper dig att skydda dina data på ett sätt som uppfyller din organisations säkerhet och regelefterlevnad. Azure SSE krypterar data innan de lagras och dekrypterar data innan de hämtas. Krypteringen och dekrypteringen är transparent för användaren.
1. **Kryptering på klientsidan**, där data redan har krypterats av klientbiblioteken. Azure lagrar data i krypterat tillstånd i vila, vilka sedan dekrypteras när de hämtas.

    Den här krypteringsfunktionen säkerställer att dina data uppfyller globala skyddsstandarder. Det är lämpligt att lagra känslig information som till exempel privata och finansiella data.

#### <a name="replication-for-storage-availability"></a>Replikering för lagringstillgänglighet

Replikeringstypen ställs in när du skapar ett lagringskonto. Replikeringsfunktionen garanterar att dina data alltid är tillgängliga. Azure Storage tillhandahåller regionala och geografiska replikeringar för att skydda data mot naturkatastrofer och andra lokala katastrofer som bränder eller översvämningar.
