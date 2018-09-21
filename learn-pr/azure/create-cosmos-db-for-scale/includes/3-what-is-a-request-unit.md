Nästa steg är att se över dataflödet i vår databas. Tillräckligt dataflöde är viktigt så att du säkert kan hantera den transaktionsvolym som dina affärsbehov kräver. Dataflödesbehovet behöver inte vara statiskt. Du kanske till exempel skapar en shoppingwebbplats som behöver skalas ut under reor eller storhelger. Vi börjar med att läsa mer om enheter för programbegäran och hur du kan beräkna krav på dataflöde.

## <a name="what-is-database-throughput"></a>Vad är dataflöden i databaser? 

Dataflödet är antalet läsningar och skrivningar som databasen kan utföra under en sekund.

Om du vill skala ut dataflödet strategiskt måste du uppskatta behovet genom att uppskatta hur många läsningar och skrivningar som kommer att utföras vid olika tidpunkter och för olika dokumentstorlekar. Med en riktig uppskattning håller du användarna nöjda även när efterfrågan är som störst. Med en felaktig uppskattning kan förfrågningar nedgraderas, åtgärder kan behöva vänta och göras om och svarstiderna kan öka vilket ger missnöjda kunder.

## <a name="what-is-a-request-unit"></a>Vad är en enhet för programbegäran?

Azure Cosmos DB mäter dataflöden i något som kallas **enhet för programbegäran (RU)**. Den här enheten mäts per sekund, så måttenheten är **enhet för programbegäran per sekund (RU/s)**. Du måste reservera antalet RU/s som du vill att Azure Cosmos DB ska etablera i förväg, så att den belastning du har uppskattat kan hanteras. Du kan när som helst skala upp eller ned dina RU/s om efterfrågan ändras.

## <a name="request-unit-basics"></a>Översikt över enheter för programbegäran

En enskild enhet för programbegäran, 1 RU, är lika med den ungefärliga kostnaden för att utföra en enda GET-begäran för ett dokument på 1 kB med dokumentets ID. Att utföra en GET med ett dokuments ID är ett effektivt sätt att hämta ett dokument, och därmed är kostnaden liten. Att skapa, ersätta eller ta bort samma objekt kräver ytterligare bearbetning av tjänsten, och därför går det åt flera enheter för programbegäran.

Antalet enheter för programbegäran som går åt till en åtgärd varierar med dokumentets storlek, antal egenskaper i dokumentet och vilken åtgärd som utförs. Dessutom beror antalet på en del andra avancerade begrepp som konsekvens och indexeringsprinciper.

I följande tabell visas antalet enheter för programbegäran som behövs för objekt i tre olika storlekar (1 kB, 4 kB och 64 kB) på två olika prestandanivåer (500 läsningar/s + 100 skrivningar/s och 500 läsningar/s + 500 skrivningar/s). I det här exemplet är datakonsekvensen inställd på **Session** och indexeringsprincipen på **Ingen**.

| Objektstorlek | Läsningar per sekund | Skrivningar per sekund | Enheter för programbegäran
| --- | --- | --- | --- |
| 1 kB | 500 | 100 | (500 * 1) + (100 * 5) = 1 000 RU/s
| 1 kB | 500 | 500 | (500 * 1) + (500 * 5) = 3 000 RU/s
| 4 kB | 500 | 100 | (500 * 1,3) + (100 * 7) = 1 350 RU/s
| 4 kB | 500 | 500 | (500 * 1,3) + (500 * 7) = 4 150 RU/s
| 64 kB | 500 | 100 | (500 * 10) + (100 * 48) = 9 800 RU/s
| 64 kB | 500 | 500 | (500 * 10) + (500 * 48) = 29 000 RU/s
 
Som du ser måste du reservera fler enheter för programbegäran ju större objektet är och ju fler läsningar och skrivningar som behövs. När du ska uppskatta dataflödesbehovet i ett program så är [Kapacitetsplaneraren](https://www.documentdb.com/capacityplanner) i Azure Cosmos DB ett onlineverktyg där du kan ladda upp ett JSON-exempeldokument och ställa in hur många åtgärder du behöver utföra per sekund. Sedan får du ett uppskattat antal att reservera.

## <a name="exceeding-throughput-limits"></a>Överskrida dataflödesgränser

Om du inte reserverar tillräckligt med enheter för programbegäran och försöker läsa eller skriva mer data än det etablerade dataflödet tillåter kommer din begäran att begränsas. När en begäran begränsas måste den göras om efter ett angivet intervall. Om du använder .NET SDK görs begäran om automatiskt efter tiden som anges i rubriken retry-after.

## <a name="creating-an-account-built-to-scale"></a>Skapa ett konto som är gjort för att skalas om

Du kan när som helst ändra antalet enheter för programbegäran som är etablerade för en databas. När volymerna är stora kan du skala upp för att tillgodose det högre behovet, och sedan minska det etablerade dataflödet för att minska kostnaderna när behovet är litet.

När du skapar ett konto kan du etablera som minst 400 RU/s eller som mest 250 000 RU/s i portalen. Om du behöver ännu större dataflöde kan du starta ett ärende i Azure Portal. Vi rekommenderar ett inledande dataflöde på 1 000 RU/s för nästan alla konton, eftersom det är det minsta värdet där din databas skalar om automatiskt om du behöver mer än 10 GB lagringsutrymme. Om du ställer in dataflödet på mindre än 1 000 RU/s kommer databasen inte att skala upp till mer än 10 GB såvida du inte etablerar om databasen och anger en partitionsnyckel. Partitionsnycklar används för snabba datasökningar i Azure Cosmos DB och gör att du kan skala om databasen automatiskt när det behövs. Partitionsnycklar diskuteras lite senare i modulen.

## <a name="summary"></a>Sammanfattning

Nu förstår du hur man beräknar och anpassar dataflödet för en Azure Cosmos DB med hjälp av enheter för programbegäran och kan göra ett lämpligt val när du skapar en ny Azure Cosmos DB-samling. Du kan när som helst ändra antalet enheter för programbegäran, men om du ställer in värdet 1 000 RU/s när du skapar ett konto ser du till att databasen kan skalas om senare.