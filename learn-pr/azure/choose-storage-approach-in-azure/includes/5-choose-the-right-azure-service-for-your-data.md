Välja rätt lagringslösningen kan leda till bättre prestanda, lägre kostnader och bättre hanterbarhet. Här kan ska du använda vad du har lärt dig om data i ditt scenario för onlinebutiker och hitta bästa Azure-tjänsten för varje datauppsättning. 

## <a name="product-catalog-data"></a>Data i produktkatalogen

**Dataklassificering:** halvstrukturerade på grund av behovet av att utöka eller ändra schemat för nya produkter

**Åtgärder:**

- Kunder kräver ett stort antal läsåtgärder, med möjlighet att fråga på många fält i databasen.
- Verksamheten kräver ett stort antal skrivåtgärder att spåra den ständigt föränderliga lagerplatser.

**Latens och dataflöde:** stort dataflöde och korta svarstider

**Transaktionsstöd:** krävs

### <a name="recommended-service-azure-cosmos-db"></a>Rekommenderad tjänst: Azure Cosmos DB

Azure Cosmos DB har i sin struktur stöd för halvstrukturerade data, eller NoSQL-data. Stöd för nya fält, till exempel ”Bluetooth-aktiverade”-fältet, eller alla nya fält som du behöver i framtiden, så är ett beroende med Azure Cosmos DB.

Azure Cosmos DB stöder SQL för frågor och varje egenskap indexeras som standard. Du kan skapa frågor så att dina kunder kan filtrera en egenskap i katalogen.

Azure Cosmos DB är även ACID-kompatibelt, så du kan vara säker på att dina transaktioner utförs enligt dessa strikta krav.

Dessutom kan du replikera dina data var som helst i världen med en enda knapptryckning i Azure Cosmos DB. Så om din e-handelswebbplats har användare som koncentrerade i USA, Frankrike och England, kan du replikera dina data till dessa Datacenter för att minska svarstiden, som du har fysiskt flyttas data närmare dina användare. 

Även med data som replikeras över hela världen, kan du välja från en av fem konsekvensnivåer. Du kan fastställa rätt balans mellan konsekvens, tillgänglighet, svarstid och dataflöde att genom att välja rätt konsekvensnivå. Du kan skala upp för att hantera högre kundefterfrågan under perioder med hög belastning eller skala ned under lugnare perioder för att minska kostnaderna.

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Azure SQL Database är ett utmärkt alternativ för den här datamängden har den inte för behovet av att utöka schemat ad-hoc för nya produkter. Alla data måste följa ett schema i Azure SQL Database. Azure SQL Database kan ge många av fördelarna med Azure Cosmos DB, men den kan inte hantera heterogena data. 

Andra Azure-tjänster, som Azure Table Storage, Azure HBase i HDInsight och Azure Redis Cache, kan också lagra NoSQL-data. I det här scenariot kan användarna fråga på flera fält, så att Azure Cosmos DB är ett bättre alternativ. Azure Cosmos DB indexerar alla fält som standard medan de andra tjänsterna är begränsad i de indexerar data och frågor på icke-indexerade fält blir nedsatt prestanda.

## <a name="photos-and-videos"></a>Foton och videor

**Dataklassificering:** ostrukturerade

**Åtgärder:**

- Behöver bara hämtas efter-ID.
- Kunder kräver ett stort antal läsåtgärder med kort svarstid.
- Data skapas och uppdateras mer sällan och svarstiderna kan vara längre än för läsåtgärder.

**Latens och dataflöde:** för hämtningar per ID krävs korta svarstider och stort dataflöde. Data kan skapas och uppdateras med längre svarstider än för läsåtgärderna.

**Transaktionsstöd:** krävs inte

### <a name="recommended-service-azure-blob-storage"></a>Rekommenderad tjänst: Azure Blob Storage

Azure Blob Storage har stöd för lagring av filer som foton och videor. Fungerar med Azure CDN Content Delivery Network () genom cachelagring av de mest använda innehållet och lagra dem på edge-servrar. Azure CDN minskar svarstid i som upp dessa avbildningar till dina användare.

Du kan också flytta avbildningar från nivån för frekvent till lågfrekvent nivå eller arkivnivå lagringsnivå med hjälp av Azure Blob storage, för att minska kostnaderna och fokus dataflöde på de ofta visas bilder och videor.

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Du kan överföra dina avbildningar till Azure App Service, så att samma server som kör din app fungerar som värd för upp dina avbildningar. Den här lösningen skulle fungera om du inte har många filer. Men om du har massor av filer, och en global publik, får du bättre prestanda genom att använda Azure Blob Storage med Azure CDN.

## <a name="business-data"></a>Affärsdata

**Dataklassificering:** strukturerade

**Åtgärder:** skrivskyddade komplexa analysfrågor över flera databaser

**Latens och dataflöde:** viss svarstid i resultaten kan förväntas eftersom frågorna är komplexa.

**Transaktionsstöd:** krävs

### <a name="recommended-service-azure-sql-database"></a>Rekommenderad tjänst: Azure SQL Database

Affärsanalytiker kör ofta frågor mot sina affärsdata och de kan oftare SQL än andra frågespråk. Azure SQL Database kan användas som lösning själv, men genom att sammankoppla den med Azure Analysis Services kan dataanalytiker att skapa en semantisk modell över data i SQL-databasen. Dataanalytiker kan sedan dela den med användare i verksamheten, så att de bara behöver ansluta till modellen från valfritt business intelligence (BI)-verktyg och utforska data och få insikter direkt. 

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Azure SQL Data Warehouse stöder OLAP-lösningar och SQL-frågor. Men din affärsanalytiker måste köra frågor mellan databaser, vilket SQL Data Warehouse inte har stöd för.

Azure Analysis Services kan användas utöver Azure SQL Database. Men din affärsanalytiker är mer mycket om SQL än i att arbeta med Power BI. Så att de vill ha en databas som har stöd för SQL-frågor som inte har Azure Analysis Services. Dessutom är de finansiella data du lagrar i företagets datauppsättning relationsbaserad och flerdimensionell. Azure Analysis Services stöder tabelldata som lagras på själva tjänsten, men inte multidimensionella data. Du kan använda en direkt fråga till SQL-databasen för att analysera flerdimensionella data med Azure Analysis Services.

Azure Stream Analytics är ett bra när du vill analysera data och omvandla dem till användbara insikter, men fokus ligger på realtidsdata som strömmar in. I det här scenariot tittar affärsanalytiker på historiska data.

## <a name="summary"></a>Sammanfattning

Varje typ av data har olika krav och det är ditt jobb att ta reda på vilken lösning som är bäst. Bör du använda vilken typ av data, de åtgärder som krävs, förväntat svarstid och behovet av transaktionell.