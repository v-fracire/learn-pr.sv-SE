Du kan få bättre prestanda genom at välja rätt lagringslösning. Här får du använda det du har lärt dig om data i onlinebutiksscenariot och hitta bästa möjliga Azure-tjänst för varje datamängd. 

## <a name="product-catalog-data"></a>Data i produktkatalogen

Dataklassificering: halvstrukturerade

Åtgärder:

* Kunderna behöver göra många läsåtgärder och köra frågor mot många olika fält i databasen.
* I verksamheten behövs många skrivåtgärder eftersom lagret hela tiden förändras.

Latens och dataflöde: stort dataflöde och korta svarstider

Transaktionsstöd: krävs

### <a name="recommended-service-azure-cosmos-db"></a>Rekommenderad tjänst: Azure Cosmos DB

Azure Cosmos DB har i sin struktur stöd för halvstrukturerade data, eller NoSQL-data. Därför är det inga problem med nya fält, som fältet Bluetooth Enabled eller andra fält du kan behöva i framtiden.

När det gäller åtgärder så har Azure Cosmos DB SQL-frågestöd och alla egenskaper indexeras som standard, så du kan skapa frågor som matchar kundernas filtreringsbehov för nästan allt.

Du kan konfigurera svarstider och dataflöden i Azure Cosmos DB, så du kan skala upp för att hantera en högre efterfrågan under rusningstid i butiken och skala ned för att spara pengar när efterfrågan är lägre. Eftersom Azure Cosmos DB indexerar alla egenskaper som standard kan du även låta kunderna köra frågor mot samtliga fält.

Azure Cosmos DB är även ACID-kompatibelt, så du kan vara säker på att dina transaktioner utförs enligt dessa strikta krav.

Dessutom kan du replikera dina data var som helst i världen med en enda knapptryckning i Azure Cosmos DB. Så om din onlinebutik har många användare i USA, Frankrike och England kan du replikera dina data till dessa datacenter för att minska svarstiderna, eftersom data fysiskt är närmare dina användare. Och trots att data replikeras över hela världen kan du välja mellan fem konsekvensnivåer och balansera konsekvens, tillgänglighet, svarstider och dataflöden.

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Andra Azure-tjänster, som Azure Table Storage, Azure HBase i HDInsight och Azure Redis Cache, kan också lagra NoSQL-data. Eftersom användarna i det här scenariot vill köra frågor mot många olika fält passar Azure Cosmos DB bättre än Azure Table Storage, Azure Redis Cache och Azure HBase i HDInsight. Azure Cosmos DB indexerar alla fält som standard medan de andra har begränsningar i indexeringen, så att det inte går att köra frågor mot alla fält i databasen.

## <a name="photos-and-videos"></a>Foton och videor

Dataklassificering: ostrukturerade

Åtgärder:

* Foton och videor behöver bara hämtas per ID.
* Data skapas och uppdateras mer sällan och svarstiderna kan vara längre än för läsåtgärder.

Latens och dataflöde: för hämtningar per ID krävs korta svarstider och stort dataflöde. Data kan skapas och uppdateras med längre svarstider än för läsåtgärderna.

Transaktionsstöd: krävs inte

## <a name="recommended-service-azure-blob-storage"></a>Rekommenderad tjänst: Azure BLOB Storage

Azure BLOB Storage har stöd för lagring av filer som foton och videor. Dessutom fungerar det med Azure Content Delivery Network (CDN) genom att det mest använda innehållet cachelagras på gränsservrar, vilket minskar svarstiderna när bilder ska visas för användarna.

När du använder Azure BLOB Storage kan du också flytta bilder från frekvent lagringsnivå till lågfrekvent lagring eller arkivlagring, så att du minskar kostnaderna och kan fokusera dataflödet på de bilder och videor som visas oftast.

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Du kan ladda upp bilder till Azure App Services så att samma server som kör appen även används till att visa dina bilder. Det här skulle fungera om du inte har så många bilder, men om du har massor av filer och en global publik får du bättre prestanda genom att använda Azure BLOB Storage med Azure Content Delivery Network.

## <a name="business-data"></a>Affärsdata

Dataklassificering: strukturerade

Åtgärder: Skrivskyddade komplexa analysfrågor över flera databaser

Latens och dataflöde: Viss svarstid i resultaten kan förväntas eftersom frågorna är komplexa.

Transaktionsstöd: krävs

### <a name="recommended-service-azure-sql-database"></a>Rekommenderad tjänst: Azure SQL Database

Affärsanalytiker kör ofta frågor mot sina affärsdata, och de kan oftare SQL än andra frågespråk. Du kan använda Azure SQL Database som fristående lösning, men om du dessutom använder Azure Analysis Services kan dataanalytiker skapa en semantisk modell över data i Azure SQL Database och sedan dela den med användare i verksamheten. Då kan de ansluta till modellen från valfritt BI-verktyg och omedelbart utforska data och få insikter. 

### <a name="why-not-other-azure-services"></a>Varför inte andra Azure-tjänster?

Azure SQL Data Warehouse har stöd för OLAP-lösningar och SQL-frågor. Dina affärsanalytiker måste dock köra frågor över flera databaser, och det finns det inte stöd för i Azure SQL Data Warehouse.

Azure Analysis Services kan användas tillsammans med Azure SQL Database. Dina affärsanalytiker är dock mer vana vid SQL än att arbeta med Power BI, så de vill ha en databas med stöd för SQL-frågor och det stödet saknas i Azure Analysis Services. Dessutom är de finansiella data du lagrar i datamängden relationsbaserade och flerdimensionella till sin natur. Azure Analysis Services har stöd för tabelldata som lagras i själva tjänsten, men inte för flerdimensionella data. Om du vill analysera flerdimensionella data i Azure Analysis Services kan du köra frågor direkt mot Azure SQL Database.

Azure Stream Analytics är ett bra när du vill analysera data och omvandla dem till användbara insikter, men fokus ligger på realtidsdata som strömmar in. I det här fallet tittar våra affärsanalytiker bara på historiska data.

## <a name="summary"></a>Sammanfattning

Varje datakategori har olika lagringskrav och det är ditt jobb att ta reda på vilken lösning som är bäst. Du bör alltid överväga datakategorin, vilka åtgärder som utförs, svarstider och behovet av transaktionsstöd.
