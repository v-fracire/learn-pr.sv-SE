Vi vill alla veta vad våra kunder tycker om vårt varumärke, vår produkt och vårt budskap. Hur ändrar sig deras åsikt med tiden? Genom att analysera det de skriver efter sentiment kan vi få viss vägledning om detta. Sentimentanalys hjälper till att besvara frågan: *Vad vill våra kunder faktiskt ha?* Det används för att analysera tweets och annat innehåll från sociala medier, kundrecensioner och e-postmeddelanden.

![Sentiment som extraheras från text och visas på en mätare från negativt till positivt.](../media/sentiment-analysis.png)

 En populär metod för sentimentanalys är att träna  maskininlärningsmodeller som identifierar sentiment. Den processen är dock komplex. Den kräver märkta träningsdata av hög kvalitet, skapande av funktioner från dessa data, träning av en klassificerare och sedan användning av klassificeraren för att förutsäga sentiment i nya texter. Det är inte alla företag som har budget och expertis nog att investera i att skapa AI-lösningar från grunden. Det har dock Microsoft och andra företag som investerar i toppmodern forskning inom dessa områden. Som utvecklare kan vi dra nytta av deras resultat via de API:er, SDK:er och plattformar som de lanserar. Microsoft Cognitive Services är ett sådant erbjudande.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

Microsoft Cognitive Services består av en uppsättning API:er, SDK:er och tjänster. Målet är att hjälpa utvecklare att göra apparna mer intelligenta, engagerande och synliga.

Microsoft Cognitive Services erbjuder intelligenta algoritmer inom syn, tal, språk, kunskap och sökning. Om du vill se vad som erbjuds kan du titta i [Cognitive Services-katalogen](https://azure.microsoft.com/services/cognitive-services/directory/). Du kan prova varje tjänst utan kostnad. När du bestämmer dig för att integrera en eller flera av dessa tjänster i dina program registrerar du dig för en betald prenumeration. Den tjänst vi använder i den här modulen är API för textanalys, så vi går igenom lite mer om den.

## <a name="text-analytics-api"></a>API för textanalys

API för textanalys är en Cognitive Service som hjälper dig att extrahera information från text. Via den här tjänsten kan du identifiera språk, upptäcka sentiment, extrahera nyckelfraser och identifiera välkända entiteter från text. 

I den här enheten går vi igenom delen om sentimentanalys i det här API:et. Internt använder tjänsten en klassificeringsalgoritm för maskininlärning för att generera sentimentpoäng mellan 0 och 1. Poäng nära 1 visar en positiv attityd, medan en poäng nära 0 indikerar en negativ attityd. En poäng nära 0,5 anger inget sentiment eller ett neutralt yttrande. Du behöver inte bekymra dig om implementeringen av algoritmen. Du fokuserar på att använda tjänsten genom att göra anrop till den från din app. Som vi snart ser strukturerar du en **POST**-begäran, skickar den till `/sentiment`slutpunkten och får ett JSON-svar som visar en *sentimentpoäng*.

Först experimenterar vi med API för textanalys med hjälp av en API-onlinetestkonsol. När vi är bekväma med API:et använder vi det i ett scenario för att identifiera sentiment i meddelanden så att vi kan sortera dem för vidare bearbetning.