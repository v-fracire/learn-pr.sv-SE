Du kan få bättre prestanda, lägre kostnader och ökad hanterbarhet genom att välja rätt lagringslösning. Här får du använda det du har lärt dig om data i onlinebutiksscenariot och hitta bästa möjliga Azure-tjänst för varje datamängd. 

## <a name="product-catalog-data"></a>Data i produktkatalogen

**Dataklassificering:** halvstrukturerade på grund av behovet av att utöka eller ändra schemat för nya produkter

**Åtgärder:**

- Kunderna behöver göra många läsåtgärder och köra frågor mot många olika fält i databasen.
- I verksamheten behövs många skrivåtgärder eftersom lagret hela tiden förändras.

**Latens och dataflöde:** stort dataflöde och korta svarstider

**Transaktionsstöd:** krävs

### <a name="recommended-service-azure-cosmos-db"></a>Rekommenderad tjänst: Azure Cosmos DB

Azure Cosmos DB har i sin struktur stöd för halvstrukturerade data, eller NoSQL-data. Därför är det inga problem med nya fält, som fältet Bluetooth-aktiverad eller andra fält du kan behöva i framtiden.

Azure Cosmos DB stödjer SQL för frågor och varje egenskap indexeras som standard. Du kan skapa frågor så att dina kunder kan filtrera en egenskap i katalogen.

Azure Cosmos DB är även ACID-kompatibelt, så du kan vara säker på att dina transaktioner utförs enligt dessa strikta krav.

Dessutom kan du replikera dina data var som helst i världen med en enda knapptryckning i Azure Cosmos DB. Så om din onlinebutik har många användare i USA, Frankrike och Storbritannien kan du replikera dina data till dessa datacenter för att minska svarstiderna, eftersom dessa data fysiskt är närmare dina användare. 

Även med data som replikeras över hela världen kan du välja en av fem konsekvensnivåer. Du kan bestämma hur du vill kompromissa gällande konsekvens, tillgänglighet, svarstid och dataflöde genom att välja rätt konsekvensnivå. Du kan skala upp för att hantera högre kundefterfrågan under perioder med hög belastning eller skala ned under lugnare perioder för att minska kostnaderna.

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Azure SQL Database vore ett utmärkt alternativ för den här datamängden, om det inte var för behovet av att utöka schemat från dag till dag baserat på nya produkter. Alla data måste följa ett schema i Azure SQL Database. Azure SQL Database kan ge många av fördelarna med Azure Cosmos DB, men den kan inte hantera heterogena data. 

Andra Azure-tjänster, som Azure Table Storage, Azure HBase i HDInsight och Azure Redis Cache, kan också lagra NoSQL-data. I det här scenariot är Azure Cosmos DB ett bättre alternativ eftersom användarna vill fråga på flera fält. Azure Cosmos DB indexerar alla fält som standard medan de andra tjänsterna har begränsingar för de data som de kan indexera. Frågor i fält som inte har indexerats leder till försämrad prestanda.

## <a name="photos-and-videos"></a>Foton och videor

**Dataklassificering:** ostrukturerade

**Åtgärder:**

- Foton och videor behöver bara hämtas per ID.
- Kunder kräver ett stort antal läsåtgärder med kort svarstid.
- Data skapas och uppdateras mer sällan och svarstiderna kan vara längre än för läsåtgärder.

**Latens och dataflöde:** för hämtningar per ID krävs korta svarstider och stort dataflöde. Data kan skapas och uppdateras med längre svarstider än för läsåtgärderna.

**Transaktionsstöd:** krävs inte

### <a name="recommended-service-azure-blob-storage"></a>Rekommenderad tjänst: Azure Blob Storage

Azure Blob Storage har stöd för lagring av filer som foton och videor. Dessutom fungerar det med Azure Content Delivery Network (CDN) genom att det mest använda innehållet cachelagras på gränsservrar. Azure CDN minskar svarstiderna när bilderna ska visas för användarna.

När du använder Azure Blob Storage kan du också flytta bilder från frekvent lagringsnivå till lågfrekvent lagring eller arkivlagringsnivå, så att du minskar kostnaderna och kan fokusera dataflödet på de bilder och videor som visas oftast.

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Du kan ladda upp bilder till Azure App Service så att samma server som kör appen även används till att visa dina bilder. Lösningen skulle fungera om du inte har så många filer. Men om du har massor av filer, och en global publik, får du bättre prestanda genom att använda Azure Blob Storage med Azure CDN.

## <a name="business-data"></a>Affärsdata

**Dataklassificering:** strukturerade

**Åtgärder:** skrivskyddade komplexa analysfrågor över flera databaser

**Latens och dataflöde:** viss svarstid i resultaten kan förväntas eftersom frågorna är komplexa.

**Transaktionsstöd:** krävs

### <a name="recommended-service-azure-sql-database"></a>Rekommenderad tjänst: Azure SQL Database

Affärsanalytiker kör ofta frågor mot sina affärsdata och de kan oftare SQL än andra frågespråk. Azure SQL Database kan användas som lösning själv, men genom att sammankoppla den med Azure Analysis Services kan dataanalytiker att skapa en semantisk modell över data i SQL-databasen. Dataanalytikerna kan sedan dela den med användare i verksamheten, så att allt de behöver göra är att ansluta till modellen från vilket BI-verktyg som helst och omedelbart utforska data och få insikter. 

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Azure SQL Data Warehouse stöder OLAP-lösningar och SQL-frågor. Men din affärsanalytiker måste köra frågor mellan databaser, vilket SQL Data Warehouse inte har stöd för.

Azure Analysis Services kan användas utöver Azure SQL Database. Men dina affärsanalytiker är mer vana vid SQL än Power BI. Därför vill de ha en databas med stöd för SQL-frågor, något som Azure Analysis Services inte har. Dessutom är de finansiella data du lagrar i företagets datauppsättning relationsbaserad och flerdimensionell. Azure Analysis Services stöder tabelldata som lagrats på själva tjänsten, men inte flerdimensionella data. Om du vill analysera flerdimensionella data i Azure Analysis Services kan du köra frågor direkt mot SQL Database.

Azure Stream Analytics är ett bra när du vill analysera data och omvandla dem till användbara insikter, men fokus ligger på realtidsdata som strömmar in. I det här scenariot tittar våra affärsanalytiker bara på historiska data.

## <a name="summary"></a>Sammanfattning

Varje datatyp har olika lagringskrav och det är ditt jobb att ta reda på vilken lösning som är bäst. Du bör alltid överväga datakategori, vilka åtgärder som utförs, förväntade svarstider och behovet av transaktionsstöd.