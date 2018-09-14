Microsoft Cognitive Services är en omfattande uppsättning intelligenta tjänster som vi kan använda för att utöka våra appar. API för textanalys har flera åtgärder för att skapa meningsfulla insikter. Vi använde tjänsten för att identifiera sentiment i text feedback från kunder. Vi har skapat en lösning på Azure Functions för att sortera meddelandena text i olika buckets eller köer för vidare bearbetning.

När du vet hur du anropar REST-API kan du enkelt integrera dessa intelligenta tjänster i dina lösningar. Alla följer ett liknande mönster:

- Registrera dig för en åtkomstnyckel
- Utforska i API-testkonsolen
- Formulera begäranden med åtkomstnyckeln och du rätt region.
- Publicera begäranden från din lösning och parsa svar för insikter.

Vi har lagt till den här intelligensen serverlös logik som skapats i Azure Functions. Du kan enkelt anropa dessa tjänster från andra typer av appar. Det finns många klientbibliotek, självstudier och kom igång snabbt med att komma igång.

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>Förslag på ytterligare förbättringar av vår lösning

Här följer några tips du bör tänka på om du vill ta vad vi mer.

- Testa lösningen med mer textexempel. Bestäm om de tröskelvärden som vi anger kategorisera sentimentpoäng i positivt, negativt och neutral är lämpliga.
- Överväg att lägga till en annan funktion i din funktionsapp som läser meddelanden från den [!INCLUDE [negative-q](./q-name-negative.md)] kön och anropar API för textanalys att hitta nyckelfraser i texten.
- Vår indatakö innehåller rå text feedback. I verkligheten, skulle vi associera feedback med någon form av användar-ID, till exempel e-postadress, kontonummer, och så vidare. Förbättra indatakö-objekt för att JSON-dokument som innehåller och ID fält och texten. Använd sedan detta ID när du arbetar med SMS-meddelandet.
- För närvarande vår lösning är ”hårdkodad” till engelska. Tänk på ändringarna du vill implementera så att den kan hantera text på alla språk som stöds av API för textanalys.
- Om du är bekant med Logic Apps kan du gå till länken till den inbyggda anslutningen för textanalys i avsnittet ytterligare läsning.

Nu när du vet hur du anropar någon av dessa Cognitive Services-API: er kan du utforska några av de andra tjänsterna och tänka på hur du kan använda dem i dina lösningar.

## <a name="further-reading"></a>Ytterligare läsning

- [Översikt över text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Så identifierar sentiment i textanalys](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Dokumentation om cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)

- [Text Analytics Logic Apps-Anslutningsapp](https://docs.microsoft.com/connectors/cognitiveservicestextanalytics/)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
