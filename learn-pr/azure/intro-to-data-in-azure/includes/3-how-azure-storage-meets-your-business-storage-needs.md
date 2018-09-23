När du tittar på fördelarna med Azures datalagring kommer du se att Azure har de bästa alternativen för lagring av din utbildningsportal. Nu ska vi närmare utforska fördelarna och alternativen för att se hur det passar dina affärsbehov.

## <a name="how-azure-data-storage-can-meet-your-business-storage-needs"></a>Så kan Azures datalagring uppfylla lagringsbehoven för ditt företag

Azure har flera lagringsalternativ som är anpassade för olika typer av datalagring. Låt oss ta en snabb titt på några av dem.

:::row:::
  :::column:::
    ![Azure SQL Database](../media/3-azure-sql-db.png)
  :::column-end:::
    :::column span="3"::: **Azure SQL Database**

**Azure SQL Database** är en robust, helt hanterad och molnbaserad relationsdatabas. Du kan använda den här funktionen till att lagra data som du använder och uppdaterar ofta, till exempel personlig och utbildningsrelaterad information om din personal. Du kan också migrera befintliga SQL Server-databaser utan att ändra dina program. Följande bild visar de datatyper från scenariot med onlineutbildningsportalen som lagras i en Azure SQL-databas.

![En bild som visar Azure SQL när det används för att lagra elevinformation, till exempel avskrifter, certifieringar och studiematerial.](../media/3-Azure_SQL.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Cosmos DB](../media/3-cosmos-db.png)
  :::column-end:::
    :::column span="3"::: **Azure Cosmos DB**

Azure Cosmos DB är en globalt distribuerad databastjänst. Den har stöd för data utan schema så att du kan skapa mycket dynamiska program som **alltid är aktiva**, vilket ger stöd för data som ändras kontinuerligt. Du kan använda den här funktionen för att lagra data som uppdateras och underhålls av användare i hela världen. Följande bild visar ett exempel på en Azure Cosmos DB-databas som används för att lagra data som används av personer som finns i hela världen.

![En bild som visar användningen av Azure Cosmos DB i onlineutbildningsscenariot för att lagra kurskatalogen. Azure Cosmos DB är ett bra val här eftersom katalogen uppdateras av administratörer och används av studenter över hela världen.](../media/3-Azure_cosmos_db.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Blob Storage](../media/3-azure-blob-storage.png)
  :::column-end:::
    :::column span="3"::: **Azure Blob storage**

Med Azure Blob Storage kan du strömma stora video- eller ljudfiler direkt till användarens webbläsare var som helst i världen. Blob Storage används också till att lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering. Den har möjlighet att lagra upp till 8 TB data för virtuella datorer. Följande bild visar ett exempel på användning av Azure Blob Storage.

![En bild som visar Azure Blob Storage som används för att lagra och strömma video- eller ljudfiler.](../media/3-Azure_blob.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Data Lake Storage Gen2](../media/3-azure-data-lake.png)
  :::column-end:::
    :::column span="3"::: **Azure Data Lake Storage Gen2**

Med Data Lake-funktionen kan du analysera dataanvändningen och skapa rapporter. Data Lake är en stor databas för både strukturerade och ostrukturerade data.

**Azure Data Lake Storage Gen2** kombinerar objektlagringens skalbarhet och kostnadsfördelar med Big Data-filsystemens tillförlitlighet och prestanda. Följande bild visar hur Azure Data Lake lagrar dina affärsdata och gör dem tillgängliga för analys.

![En bild som visar Azure Data Lakes roll i förbereda och lagra dina data för användning i analysverktyg. Azure Data Lake kan hantera en mängd olika indatatyper, till exempel relationsdatabaser, video- eller sensordata.](../media/3-Data_lake_store_concept.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Files](../media/3-azure-files.png)
  :::column-end:::
    :::column span="3"::: **Azure Files**

Med Azure Files får du helt hanterade filresurser i molnet. Program som körs i Azure kan enkelt dela filer mellan virtuella datorer. Du kan använda Azure-filresurser på samma gång för molnet eller lokala distributioner av Windows, Linux och macOS. Följande bild visar hur Azure Files används för att dela data mellan två geografiska platser. Azure Files använder protokollet Server Message Block (SMB), vilket garanterar att data krypteras i vila och under överföring.

![En bild som visar fildelningsfunktionerna i Azure Files. ](../media/3-Azure_Files.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Queue](../media/3-azure-queue.png)
  :::column-end:::
    :::column span="3"::: **Azure Queue**

Azure Queue Storage är en tjänst för lagring av stora mängder meddelanden som kan nås var som helst i världen. För att sätta det i perspektiv har ett enskilt kömeddelande en storlek på upp till 64 kB och en kö kan innehålla flera miljoner meddelanden.

Oftast finns det en eller flera avsändarkomponenter och en eller flera mottagarkomponenter. Avsändarkomponenter lägger till meddelanden till kön, medan mottagarkomponenter hämtar meddelanden som ligger först i kön för bearbetning. Följande bild visar flera avsändarappar som lägger till meddelanden i Azure-kön och en mottagarapp som hämtar meddelandena.

![En bild som visar den övergripande arkitekturen i Azure Queue Storage](../media/3-Azure_Queue.png)

Du kan använda Queue Storage för att:

- Skapa en lista med kvarvarande arbetsuppgifter och för att skicka meddelanden mellan olika Azure-webbservrar.
- Fördela belastningen mellan olika webbservrar/infrastrukturer och för att hantera trafiktoppar.
- Säkerställa återhämtning efter komponentfel när flera personer använder dina data samtidigt.

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Standard Storage](../media/3-azure-standard-storage.png)
  :::column-end:::
    :::column span="3"::: **Azure Standard Storage**

Virtuella datorer i Azure använder diskar till att lagra operativsystem, program och data. Azure Standard Storage ger ett tillförlitligt och kostnadseffektivt diskstöd för virtuella datorer som kör arbetsbelastningar som inte är verksamhetskritiska. Med Standard Storage lagras data på hårddiskar (HDD).

När du arbetar med virtuella datorer kan du använda SSD-standarddiskar och HDD-standarddiskar för mindre kritiska arbetsbelastningar, och SSD-premiumdiskar för verksamhetskritiska produktionsprogram. Azures diskar har konsekvent levererat tillförlitlighet i företagsklass, med en branschledande årlig felfrekvens på 0 %. Följande bild visar en virtuell Azure-dator som använder separata diskar för att lagra olika data.

![En bild som visar två diskar i en virtuell dator, en som lagrar operativsystemet och en som lagrar data.](../media/3-Azure_disks.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Lagringsnivåer](../media/3-storage-tiers.png)
  :::column-end:::
    :::column span="3"::: **Lagringsnivåer**

Azure erbjuder tre lagringsnivåer för lagring av blobbobjekt:

1. **Frekvent lagringsnivå**: optimerad för att lagra data som används ofta.

1. **Lågfrekvent lagringsnivå**: optimerad för data som inte används ofta och som har lagrats i minst 30 dagar.

1. **Arkivlagringsnivå**: för data som används sällan och som lagrats i minst 180 dagar med flexibla svarstidskrav.

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Kryptering och replikering](../media/3-azure-storage-encryption.png)
  :::column-end:::
    :::column span="3"::: **Kryptering och replikering**

Azure har funktioner för kryptering och replikering som ger hög säkerhet och tillgänglighet för dina data.

#### <a name="encryption-for-storage-services"></a>Kryptering för lagringstjänster

Följande krypteringstyper är tillgängliga för dina resurser:

1. **Azure Storage Service Encryption (SSE)** för vilande data hjälper dig att skydda dina data på ett sätt som uppfyller organisationens krav på säkerhet och regelefterlevnad. Det krypterar data innan de lagras och dekrypterar data innan de hämtas. Krypteringen och dekrypteringen är transparent för användaren.

1. **Kryptering på klientsidan** är när data redan har krypterats av klientbiblioteken. Azure lagrar data i krypterat tillstånd i vila och dekrypterar sedan när data hämtas.

#### <a name="replication-for-storage-availability"></a>Replikering för tillgänglighet

Replikeringstypen ställs in när du skapar ett lagringskonto. Replikeringsfunktionen garanterar att dina data alltid är tillgängliga. Azure ger regionbaserad och geografisk replikering som skyddar dina data mot naturkatastrofer och andra lokala katastrofer som bränder eller översvämningar.

  :::column-end:::
:::row-end:::