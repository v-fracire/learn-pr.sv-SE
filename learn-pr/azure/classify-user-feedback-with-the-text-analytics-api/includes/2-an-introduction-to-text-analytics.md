Att inse bara, vi alla vill veta vad kunderna tycker om vårt varumärke, våra produkt, vårt meddelande. Hur ändrar sig över tid? Viss vägledning kan låsa upp om du letar efter attityd i de skriver. Attitydanalys som hjälper dig att besvara frågan: *vad våra kunder vill?* Den används för att analysera tweets och andra sociala medieinnehåll, Kundrecensioner och e-postmeddelanden.

 Ett populärt sätt att attitydanalys är att träna machine learning-modeller som identifierar sentiment. Den här processen är dock komplexa. Det innebär att med god kvalitet träningsdata som kallas, skapa funktioner från dessa data, tränar en klassificerare och sedan använda klassificeraren för att förutsäga känsla av nya typer av text. Inte alla företag har pengar och expertis att investera i att skapa AI-lösningar från grunden. Testningskostnader Microsoft och andra företag kan göra och investera i den senaste forskningen inom följande områden. Som utvecklare får vi för att dra nytta av resultaten via API: er, SDK: er och plattformar som de skickar. Microsoft Cognitive Services är ett erbjudande.

![Sentiment som extraheras från text och visas på en mätare från negativt till positivt.](../media/sentiment-analysis.png)

## <a name="microsoft-cognitive-services"></a>Microsoft Cognitive Services

Microsoft har levererade en uppsättning API: er, SDK: er och tjänster under banderoll av *Microsoft Cognitive Services* ett tag. Målet är att hjälpa utvecklare att göra apparna mer intelligenta, engagerande och synliga.

Microsoft Cognitive Services erbjuder intelligenta algoritmer i visuellt innehåll, tal, språk, kunskap och sökning. Om du vill se vad som finns på erbjudandet, Kolla in den [Cognitive Services-katalog](https://azure.microsoft.com/services/cognitive-services/directory/). Du kan prova varje tjänst utan kostnad. När du vill integrera en eller flera av dessa tjänster i dina program kan registrera dig för en betald prenumeration. Tjänsten vi använder under hela den här modulen är Textanalys, så vi dig mer om den.

## <a name="text-analytics-api"></a>API för textanalys

API för textanalys är en Cognitive tjänst som hjälper dig extrahera information från text.  Via tjänsten kan du identifiera språk, identifiera känsla, extrahera nyckelfraser och identifiera välkända entiteter från text. Om du är otålig och vill prova tjänsten, gå på till de [textanalys-demo](https://azure.microsoft.com/services/cognitive-services/text-analytics/) i officiella dokumentationen.

I den här lektionen får vi veta sentiment analysis en del av detta API. Under försättsbladen använder tjänsten machine learning-klassificeringsalgoritm för att generera ett sentimentpoäng mellan 0 och 1.  Poäng närmare 1 visar positiv attityd medan resultat närmare 0 anger negativ känsla. En poäng nära 0,5 anger ingen åsikter eller en neutral-instruktion. Du behöver inte bekymra dig om implementering av algoritmen. Du fokusera på att använda tjänsten genom att göra anrop till den från din app.  Som vi ser snart du strukturera en **POST** begäran och skicka det till den `/sentiment` slutpunkt och få ett JSON-svar som talar om en *sentimentresultatet*.

Vi ska först experimentera med API för textanalys med hjälp av en online API testkonsolen. När vi känner dig bekväm med API: et, använder vi den i ett scenario för att identifiera sentiment i meddelanden så att vi kan sortera dem för vidare bearbetning.