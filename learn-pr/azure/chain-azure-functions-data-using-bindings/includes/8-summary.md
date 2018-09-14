Den här modulen var allt om att integrera data och tjänster i dina funktioner. Vi igång med en snabb genomgång av bindningstyper som visas när du lägger till dem till en funktion. Vi har tittat sedan vid läsning av data från ett Azure Cosmos DB med hjälp av en indatabindning. Plattformen tar hand om hanteringen anslutningssträngar och vi såg du hur enkelt det är att läsa data i vår kod med hjälp av bindningen. Slutligen fokuserar vi vår kännedom om hur du skriver olika datakällor med hjälp av utdatabindningar. Den här resan summeras i följande tabell.

[!INCLUDE [summary table](./summary-table.md)]

Du kan använda de metoder som du har lärt dig här att lägga till och testa bindningar i dina funktioner. Här är några intressanta idéer att få mer praxis med bindningar och bygga vidare på det du har lärt dig här.

* Skapa en annan funktion för att läsa från Blob Storage och andra indatabindningar som vi inte har använt i den här modulen.

* Skapa en annan funktion att skriva till flera mål med hjälp av andra typer av stöds utdata-bindning.

* I den sista enheten vi introducerade en kö och skickade meddelanden till den med en utdatabindning. I nästa steg, Överväg att lägga till en annan funktion som läser in meddelanden i kön och skriver ut den **MEDDELANDETEXT** till konsolen med `Console.Log()`.

Som vi såg i den här modulen erbjuder Azure-portalen enkel att använda funktioner för att börja skapa funktioner och koppla dem till data och andra tjänster.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="further-reading"></a>Mer information

Även om detta inte är avsedd att vara en fullständig förteckning, är följande några resurser som rör de ämnen som täcks i den här modulen som kan vara intressant.

 * [Azure Functions-dokumentation](https://docs.microsoft.com/azure/azure-functions/)

* [Azure Functions-utmaningen](https://aka.ms/afc)

* [Azure serverlös databehandling Cookbook](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)

 * [Använda Queue Storage från Node.js](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [Introduktion till Azure Cosmos DB: SQL-API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [En teknisk översikt över Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [Dokumentation om Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
