Microsoft Cognitive Services är en omfattande uppsättning intelligenta tjänster som vi kan använda för att utöka våra appar. Vi utforskat en liten del av tjänsten API för textanalys för att få generell information om text. Vi använde tjänsten för att analysera textfeedback från kunder efter attityd. Vi har skapat en lösning i Azure Functions för att sortera dessa textmeddelanden i olika buckets eller köer för vidare bearbetning.

När du vet hur du anropar REST API kan du enkelt integrera dessa intelligenta tjänster i dina lösningar. Alla följer ett liknande mönster:

- Registrera dig för en åtkomstnyckel
- Utforska i API-testkonsolen
- Formulera begäranden med åtkomstnyckeln och rätt region.
- Publicera begäranden från din lösning och parsa svaren i analyssyfte.

Vi har lagt till den här intelligensen i serverlös logik som skapats i Azure Functions. Du kan enkelt anropa dessa tjänster från andra typer av appar. Det finns många klientbibliotek, självstudier och snabbstarter som hjälper dig igång.

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>Förslag på ytterligare förbättringar av vår lösning

Här följer några tips om du vill lära dig mer. 

- Testa lösningen med fler textexempel och se om de tröskelvärden som vi angav för att kategorisera attitydpoäng i positiva, negativa och neutrala är lämpliga. 
- Lägg till en annan funktion i din funktionsapp som läser meddelanden från [!INCLUDE [negative-q](./q-name-negative.md)]-kön och anropar API för textanalys för att hitta nyckelfraser i texten.
- Vår indatakö innehåller feedback som råtext. I verkligheten skulle vi associera feedbacken med någon form av användar-ID, till exempel e-postadress, kontonummer etc. Därför förbättrar vi indataköns objekt till JSON-dokument som innehåller ID-fält och text. Använd sedan detta ID när du arbetar med textmeddelandet.
 - För närvarande är vår lösning ”hårdkodad” på engelska. Tänk på vilka ändringar du gör så att de kan hantera text på alla språk som stöds av API för textanalys.  

Nu när du vet hur du anropar ett par av dessa Cognitive Services-API:er, kan vi ta en titt på några av de andra tjänsterna och se hur du kan använda dem i dina lösningar. 

## <a name="further-reading"></a>Ytterligare läsning

- [Översikt över Textanalys](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Identifiera attityd i Textanalys](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Dokumentation om Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)

## <a name="clean-up-resources"></a>Rensa resurser

*Resurser* i Azure avser funktionsappar, funktioner, lagringskonton och så vidare. Dessa grupperas i *resursgrupper*. Du kan ta bort allt innehåll i en grupp genom att ta bort gruppen.

Du skapade resurser för att kunna slutföra den här modulen. Det är möjligt att du debiteras för de här resurserna beroende på din [kontostatus](https://azure.microsoft.com/account/) och dina [tjänstpriser](https://azure.microsoft.com/pricing/). Om du inte behöver resurserna längre så visar vi hur du tar bort dem här:

1. Gå till sidan **Resursgrupp** i Azure Portal.

   För att gå till den sidan från sidan för funktionsappar väljer du fliken **Översikt** och länken under **Resursgrupp**.

   För att komma till den sidan från instrumentpanelen väljer du **Resursgrupper** och sedan väljer du den resursgrupp som du använde för den här modulen. 

> [!NOTE]
> Standardnamnet på den resursgrupp som vi föreslog för den här modulen var [!INCLUDE [resource-group-name](./rg-name.md)], men det är möjligt att du har använt ett annat namn.

2. Granska listan över de resurser som ingår på sidan **Resursgrupp** och kontrollera att det är dem som du vill ta bort.

3. Välj **Ta bort resursgrupp** och följ instruktionerna.

   Borttagningen kan ta några minuter. När du är färdig visas ett meddelande i några sekunder. Du kan även välja klockikonen längst upp på sidan för att se meddelandet.

## <a name="further-reading"></a>Ytterligare läsning

Även om detta inte är avsett att vara en fullständig förteckning, kan följande resurser som rör de ämnen som beskrivs i den här modulen vara intressanta.

 * [Azure Functions-dokumentation](https://docs.microsoft.com/azure/azure-functions/)

* [The Azure Functions Challenge](https://aka.ms/afc)

* [Azure Serverless Computing Cookbook](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)

 * [Använda Queue Storage från Node.js](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [Introduktion till Azure Cosmos DB: SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [En teknisk översikt över Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [Dokumentation om Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
