Den här modulen handlade om att integrera data och tjänster i dina funktioner. Vi började med en snabb genomgång av de bindningstyper som visas när du lägger till dem i en funktion. Vi tittade sedan på att läsa in data från en Azure Cosmos DB med hjälp av en indatabindning. Plattformen tar hand om hanteringen av anslutningssträngar och vi såg hur enkelt det är att läsa data i vår kod med hjälp av bindningen. Slutligen fokuserade vi på hur man skriver till olika datakällor med hjälp av utdatabindningar. Den här processen sammanfattas i följande tabell:

[!INCLUDE [summary table](./summary-table.md)]

Du kan använda de metoder som du har lärt dig här för att lägga till och testa bindningar i dina funktioner. Här är några intressanta idéer för att få mer erfarenhet av bindningar och bygga vidare på det du har lärt dig här.

* Skapa ännu en funktion för att läsa från Blob Storage och andra indatabindningar som vi inte har använt i den här modulen.

* Skapa ännu en funktion som skriver till flera mål med hjälp av andra typer av utdatabindningar som stöds.

* I den föregående enheten introducerade vi en kö och skickade meddelanden till den med en utdatabindning. I nästa steg bör du överväga att lägga till ännu en funktion som läser in meddelanden i kön och skriver ut **MEDDELANDETEXTEN** till konsolen med `Console.Log()`.

Som vi såg i den här modulen erbjuder Azure-portalen lättanvända funktioner för att börja skapa funktioner och koppla dem till data och andra tjänster.

Om du vill utföra liknande serverfri integrering med visuella arbetsflöden och lite eller ingen anpassad kod kan du kolla in Azure Logic Apps.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>Ytterligare resurser

Även om detta inte är avsett att vara en fullständig förteckning, är följande några resurser som rör de ämnen som tas upp i den här modulen och som kan vara intressanta för dig:

* [Azure Functions-dokumentation](https://docs.microsoft.com/azure/azure-functions/)
* [Azure Functions-utmaningen](https://aka.ms/afc)
* [Azure serverfri databehandling – Cookbook](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)
* [Använda Queue Storage från Node.js](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)
* [Introduktion till Azure Cosmos DB: SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)
* [En teknisk översikt över Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)
* [Dokumentation om Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
