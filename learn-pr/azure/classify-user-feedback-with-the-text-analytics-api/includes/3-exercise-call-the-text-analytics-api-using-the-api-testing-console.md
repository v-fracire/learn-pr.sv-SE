Om du vill se API för textanalys i praktiken vi gör några anrop med hjälp av inbyggda API testning Konsolverktyget finns i referensdokumentationen online. Innan vi gör det behöver vi en åtkomstnyckel för att göra dessa anrop.

## <a name="create-an-access-key"></a>Skapa en åtkomstnyckel

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Varje anrop till API för textanalys krävs en prenumerationsnyckel. Kallas ofta en åtkomstnyckel, används det för att verifiera att du har åtkomst för att anropa. Vi använder Azure-portalen för att hämta en nyckel. 

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. Klicka på **skapa en resurs**

1. Under Azure Marketplace, väljer **AI + Maskininlärning** att visa en lista över tillgängliga API: er. Klicka på **se alla** uppe till höger på sidan för att se hela listan med Cognitive Services API: er.

1. Hitta **textanalys** i listan med Cognitive Services och välj den.
    ![Skärmbild av AI och Machine Learning fönster som visar Text Analytics API som valts i listan.](../media/select-text-analytics.PNG)

1. I Skapa-sidan som öppnas anger du följande värden i varje fält.

|Egenskap  | Värde  | Beskrivning  |
|---------|---------|---------|
|Namn     |    MyTextAnalyticsAPIAccount     |  Namnet på Cognitive Services-konto. Vi rekommenderar att du använder ett beskrivande namn. Giltiga tecken är `a-z`, `0-9` och `-`.    |
|Prenumeration     |  Din prenumeration       |   Prenumerationen under där den här nya Cognitive Services API-konto med **Textanalys** har skapats.      |
|Plats     |  Västra USA       |  Välj en [region](https://azure.microsoft.com/regions/) nära dig.       |
|Prisnivå     | **F0 kostnadsfritt**     |   Kostnaden för Cognitive Services-kontot beror på den faktiska användningen och de alternativ du väljer. Vi rekommenderar att du väljer den kostnadsfria nivån för våra syften här.      |
|Resursgrupp     |  Välj **Använd befintlig** och välj <rgn>[Sandbox resursgruppens namn]</rgn>       |  Namn för den nya resursgruppen där du vill skapa ett Cognitive Services Text Analytics API-konto.       |

Här visas en skärmbild av vad de **skapa** sidan ser ut när du är klar.

![Skärmbild av användargränssnittet för att skapa ett Text Analytics-konto med alla fält som fylls med värden som föreslås i föregående tabell.](../media/create-text-analytics-account.PNG)

1. Välj **skapa** längst ned på sidan för att starta process för att skapa kontot.  Håll utkik efter ett meddelande om att distributionen pågår. Du får sedan ett meddelande om att konton som har distribuerats korrekt i resursgruppen.

![Distributionen lyckades-meddelande med en knappen Gå till resurs och en PIN-kod för instrumentpanelen knappen.](../media/deploy-resource-group-success.PNG)

Nu när vi har vår Cognitive Services-konto kan vi hitta åtkomstnyckeln så att vi kan börja med att anropa API.

1. Klicka på den **gå till resurs** knappen på den *distributionen lyckades* meddelande. Den här åtgärden öppnar kontot Snabbstart.

1. Välj den **nycklar** menyalternativet på menyn till vänster eller i den *ta dina nycklar* avsnittet i snabbstarten.  Den här åtgärden öppnar den **hantera nycklar** sidan.

1. Kopiera en av nycklarna med hjälp av kopieringsknappen.

![Hantera nycklar användargränssnitt som visar namnet på Cognitive Services-konto och nyckel 1 och 2 för nyckel-poster.](../media/manage-keys.PNG)

> [!IMPORTANT]
> Alltid skydda dina åtkomstnycklar och dela dem aldrig.

1. Store den här nyckeln för resten av den här modulen. Vi använder den inom kort att göra API-anrop från testkonsolen och i resten DENOD modulen.

Nu när vi har vår nyckel kan vi gå vidare till testkonsolen och dra API: et för en skapa.

## <a name="call-the-api-from-the-testing-console"></a>Anropa API från testkonsolen

1. Navigera till den [Textanalys](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7?azure-portal=true) referensdokumentation i webbläsaren.

    Denna sida visas en meny till vänster och innehåll till höger. På menyn visas POST-metoder som du kan anropa i API för textanalys. De här slutpunkterna är **identifiera språk**, **entiteter**, **Nyckelfraser**, och **Sentiment**.  Vi behöver göra några saker för att anropa en av dessa åtgärder.

    - Välj den metod som vi vill ringa upp.
    - Välj en testning konsol som matchar regionen eller plats som vi valde tidigare i den här lektionen.
    - Lägg till åtkomstnyckel som vi har sparat tidigare i kursen för varje anrop.

1. Längst fram den vänstra menyn och välj **Sentiment**. Det här alternativet öppnas Sentiment dokumentationen till höger. Så som visas i dokumentationen, kommer vi att göra ett REST-anrop i följande format:

    `https://[location].api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

Vi ska skicka in våra prenumerationsnyckel och åtkomstnyckel i den **ocp-Apim-Subscription-Key** rubrik.

## <a name="make-some-api-calls"></a>Göra vissa API-anrop

1. Välj en region i listan på den här sidan som matchar den plats som vi valt när du skapar våra Cognitive Services-konto tidigare i den här lektionen.  Exempel: om vi valde *västra USA* tidigare när du skapar kontot, välj sedan det här på följande sätt.
    ![Skärmbild av API för textanalys referensplats med Sentiment menyalternativet valts, följt av västra USA.](../media/select-testing-console-region.png)

1. Nästa sida som öppnas är en live, interaktiva, API-konsol.  Klistra in den åtkomstnyckel som du sparade tidigare i fältet på sidan med etiketten **Ocp-Apim-Subscription-Key**. Observera att nyckeln har skrivits automatiskt till fönstret HTTP-begäran som ett värde för sidhuvudet.

1. Bläddra längst ned på sidan och klicka på **skicka**. Lås oss vad som händer genom att titta på den här skärmen i detalj.

I avsnittet rubriker i användargränssnittet ange vi nyckeln åtkomst eller en prenumeration i huvudet i begäran.

![Skärmbild av avsnittet rubriker.](../media/2-marker.PNG)

Nu ska vi har begärandetexten avsnittet som har en **dokument** matris. Alla dokument i matrisen som tre egenskaper. Egenskaperna är *”language”*, *”id”*, *”text”*. Den *”id”* är ett tal i det här exemplet, men kan vara allt du vill så länge det är unikt i matrisen dokument. I det här exemplet låter vi också i dokument som skrivits i tre olika språk. Över 15 språk stöds i funktionen Sentiment i API för textanalys. Mer information finns i [språk som stöds i API för textanalys](https://docs.microsoft.com//azure/cognitive-services/text-analytics/text-analytics-supported-languages). Den maximala storleken för ett enskilt dokument är 5 000 tecken och en begäran kan ha upp till 1 000 dokument.

![Skärmbild av Begärandetext avsnittet](../media/3-marker.PNG)

Den fullständiga begäran, inklusive rubrikerna och begärans-URL visas i nästa avsnitt. I det här exemplet ser du att begäranden dirigeras till en URL som börjar med `westus`. URL: en skiljer sig beroende på den region som du har valt.

![Avsnittet tal fyra. ](../media/4-marker.PNG)
 ![Avsnittet nummer fem.](../media/5-marker.PNG)

Sedan har vi information om svaret. I det här exemplet ser vi att begäran har en lyckad och en kod `200` returnerades. Vi kan också se att hälsoverifieringskoden tog 38 ms.

![Avsnittet tal fem.](../media/6-marker.PNG)

Slutligen kan se vi svaret på begäran. Svaret innehåller insight Textanalys hade om våra dokument. En matris med dokument returneras till oss, utan att den ursprungliga texten. Vi få tillbaka en *”id”* och *”poäng”* för varje dokument. API: et returnerar ett numeriskt värde mellan 0 och 1. Resultat nära 1 visar positiv attityd medan resultat nära 0 indikerar negativ attityd. Ett resultat på 0,5 anger bristen på sentiment, en neutral-instruktion. I det här exemplet har vi två ganska positivt dokument och ett negativt dokument.

![Avsnittet tal fem.](../media/7-marker.PNG)

Grattis! Du har gjort ditt första anrop till API för textanalys utan att behöva skriva en rad med kod. Passa på att stanna kvar i konsolen och prova fler anrop. Här följer några förslag:

- Ändra dokument i avsnittsnummer 2 och se vad API: et returnerar.
- Prova de andra metoderna **identifiera språk**, **entiteter** och **Nyckelfraser**, med hjälp av samma prenumerationsnyckeln.
- Försök att göra ett anrop från en annan region med din prenumeration och se vad som händer.

API-testkonsolen är ett bra sätt att utforska funktionerna i den här API: et. Nu när du har utforskat själv, ska vi gå vidare till att placera den här intelligensen i ett verkligt scenario.