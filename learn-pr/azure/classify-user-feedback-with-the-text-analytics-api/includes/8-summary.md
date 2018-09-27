Azure Cognitive Services är en omfattande uppsättning intelligenta tjänster som vi kan använda för att utöka våra appar. API för textanalys har flera åtgärder för att skapa meningsfulla insikter. Vi använde tjänsten för att läsa av attityd i textfeedback från kunder. Vi har skapat en lösning i Azure Functions för att sortera dessa textmeddelanden i olika buckets eller köer för vidare bearbetning.

När du vet hur du anropar REST API kan du enkelt integrera dessa intelligenta tjänster i dina lösningar. Alla följer ett liknande mönster:

- Registrera dig för en åtkomstnyckel.
- Utforska i API-testkonsolen.
- Formulera begäranden med åtkomstnyckeln och rätt region.
- Publicera begäranden från din lösning och parsa svaren i analyssyfte.

Vi har lagt till den här intelligensen i serverlös logik som skapats i Azure Functions. Du kan enkelt anropa dessa tjänster från andra typer av appar. Det finns många klientbibliotek, självstudier och snabbstarter som hjälper dig igång.

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>Förslag på ytterligare förbättringar av vår lösning

Här följer några tips om du vill lära dig mer.

- Testa lösningen med fler textexempel. Se om de tröskelvärden som vi angav för att kategorisera attitydpoäng i positiva, negativa och neutrala är lämpliga.
- Lägg till en annan funktion i din funktionsapp som läser meddelanden från [!INCLUDE [negative-q](./q-name-negative.md)]-kön och anropar API för textanalys för att hitta nyckelfraser i texten.
- Vår indatakö innehåller feedback som råtext. I verkligheten skulle vi associera feedbacken med någon form av användar-ID, till exempel e-postadress, kontonummer etc. Förbättra indataköns objekt till JSON-dokument som innehåller ID-fält och text. Använd sedan detta ID när du arbetar med textmeddelandet.
- För närvarande är vår lösning ”hårdkodad” på engelska. Tänk på vilka ändringar du skulle göra, så att de kan hantera text på alla språk som stöds av API för textanalys.
- Om du känner till Logic Apps kan du klicka på länken till det inbyggda anslutningsprogrammet för textanalys i avsnittet Ytterligare läsning.

Nu när du vet hur du anropar ett par av dessa Cognitive Services-API:er, ska du utforska några av de andra tjänsterna och se hur du kan använda dem i dina lösningar.

## <a name="further-reading"></a>Ytterligare läsning

- [Översikt över Textanalys](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Demo för Textanalys](https://azure.microsoft.com/services/cognitive-services/text-analytics/)
- [Identifiera attityd i Textanalys](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Dokumentation om Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)

- [Anslutningsprogram till Logic Apps för textanalys](https://docs.microsoft.com/connectors/cognitiveservicestextanalytics/)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
