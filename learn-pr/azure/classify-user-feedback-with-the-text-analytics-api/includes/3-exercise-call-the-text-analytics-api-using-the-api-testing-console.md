För att se API för textanalys i praktiken gör vi några anrop med hjälp av det inbyggda API-testkonsolverktyget som finns i referensdokumentationen online. Innan vi gör detta måste vi ha en åtkomstnyckel för att kunna göra dessa anrop. 

## <a name="create-an-access-key"></a>Skapa en åtkomstnyckel

För varje anrop till API för textanalys krävs en prenumerationsnyckel. Det kallas ofta en åtkomstnyckel och den används för att verifiera att du har åtkomst för anropet. Vi använder Azure Portal till att hämta en nyckel. 

1. Logga in på Azure Portal på [https://portal.azure.com](https://portal.azure.com?azure-portal=true) med ditt Azure-konto.

1.  Klicka på **+ Skapa en resurs**

1.  Under Azure Marketplace väljer du **AI + Machine Learning** för att visa en lista med tillgängliga API:er. Klicka på **Se alla** uppe till höger på sidan för att se hela listan med API:er för Cognitive Services. 

1. Leta reda på **Textanalys** i listan med Cognitive Services och välj den. 
![Skärmbild av fönstret AI och Machine Learning som visar den API för textanalys som valts i listan.](../media-draft/select-text-analytics.PNG)

1. På sidan Skapa som öppnas anger du följande värden i fälten.


|Egenskap  | Värde  | Beskrivning  |
|---------|---------|---------|
|Namn     |    MyTextAnalyticsAPIAccount     |  Namnet på Cognitive Services-kontot. Vi rekommenderar att du använder ett beskrivande namn. Giltiga tecken är `a-z`, `0-9` och `-`.    |
|Prenumeration     |  Din prenumeration       |   Prenumerationen där det här nya Cognitive Services API-kontot med **API för textanalys** skapas.      |
|Plats     |  USA, västra       |  Välj en [region](https://azure.microsoft.com/regions/) nära dig.       |
|Prisnivå     | **F0 kostnadsfritt**     |   Kostnaden för ditt Cognitive Services-konto beror på den faktiska användningen och de alternativ som du väljer. Vi rekommenderar att du väljer den kostnadsfria nivån för våra syften här.      |
|Resursgrupp     |  Skapa en ny resursgrupp och kalla den [!INCLUDE [resource-group-note](./rg-name.md)]       |  Namnet på den nya resursgruppen där du vill skapa ett Cognitive Services-konto för API för textanalys.       |


[!INCLUDE [resource-group-note](./rg-notice.md)]

Här visas en skärmbild av hur sidan **Skapa** ser ut när du är klar.

![Skärmbild av användargränssnittet för att skapa ett textanalyskonto med alla fält ifyllda med de värden som föreslogs i föregående tabell.](../media-draft/create-text-analytics-account.PNG)

6. Välj **Skapa** längst ned på sidan för att starta processen att skapa kontot.  Vänta på ett meddelande om att distributionen pågår. Du får sedan ett meddelande om att kontot har distribuerats till resursgruppen.

![Meddelandet om att distributionen är klar med knapparna Gå till resurs och Fäst på instrumentpanelen.](../media-draft/deploy-resource-group-success.PNG)

Nu när vi har vårt Cognitive Services-konto kan vi leta reda på åtkomstnyckeln. Därefter ska vi börja anropa API:n. 

7. Klicka på knappen **Gå till resurs** i meddelandet *Distributionen lyckades*. Den här åtgärden öppnar kontot Snabbstart. 

1. Välj menyalternativet **Nycklar** i menyn till vänster, eller i avsnittet *Hämta dina nycklar* i snabbstarten.  Åtgärden öppnar sidan **Hantera nycklar**.

1. Kopiera en av nycklarna med hjälp av kopieringsknappen. 

![Användargränssnittet för att hantera nycklar visar namnet på Cognitive Services-kontot samt posterna Nyckel 1 och Nyckel 2.](../media-draft/manage-keys.PNG)

> [!IMPORTANT]
> Se alltid till att dina åtkomstnycklar är skyddade och dela dem aldrig. 

10. Lagra nyckeln för resten av den här modulen. Vi kommer att använda den inom kort för att göra API-anrop från testkonsolen och i resten av modulen.

Nu när vi har vår nyckel kan vi gå vidare till testkonsolen och börja använda API:n.

## <a name="call-the-api-from-the-testing-console"></a>Anropa API:n från testkonsolen

1. Gå till referensdokumentationen för [API för textanalys](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7?azure-portal=true) i webbläsaren.

På startsidan visas en meny till vänster och innehåll till höger. I menyn visas de POST-metoder som du kan anropa i API för textanalys. De här slutpunkterna är **Identifiera språk**, **Entiteter**, **Nyckelfraser** och **Attityd**.  Vi behöver göra några saker för att kunna anropa en av dessa åtgärder.
- Välja den metod som vi vill anropa.
- Välja en testkonsol som matchar regionen eller platsen som vi valde tidigare i den här lektionen. 
- Lägga till den åtkomstnyckel som vi sparade tidigare i lektionen i varje anrop.

2. Välj **Attityd** i den vänstra menyn. Det här alternativet öppnar attityddokumentationen till höger. Så som visas i dokumentationen kommer vi att göra ett REST-anrop i följande format:

`https://[location].api.cognitive.microsoft.com/text/analytics/v2.0/sentiment` 

Vi ska skicka in vår prenumerationsnyckel eller åtkomstnyckel i rubriken **ocp-Apim-Subscription-Key**.

## <a name="make-some-api-calls"></a>Göra vissa API-anrop

1. Välj en region i listan på den här sidan som matchar den plats som vi valde när vi skapade vårt Cognitive Services-konto tidigare i den här lektionen.  Om vi exempelvis valde *USA, västra* tidigare när vi skapade kontot, väljer du det här på följande sätt.
![Skärmbild av en referensplats för API för textanalys med menyalternativet Attityd valt, följt av USA, västra.](../media-draft/select-testing-console-region.png)

1.  Nästa sida som öppnas är en interaktiv API-konsol.  Klistra in den åtkomstnyckel som du sparade tidigare i fältet på sidan med etiketten **Ocp-Apim-Subscription-Key**. Observera att nyckeln skrivs automatiskt till fönstret HTTP-begäran som ett värde för rubriken.

1. Bläddra längst ned på sidan och klicka på **Skicka**. Låt oss se vad som händer genom att titta på den här skärmen i detalj.

I avsnittet Rubriker i användargränssnittet anger vi åtkomst eller prenumeration samt nyckel i rubriken för begäran.

![Skärmbild av rubrikavsnittet.](../media-draft/2-marker.PNG)

Därefter har vi avsnittet med begärandetext som innehåller en **dokument**matris. Alla dokument i matrisen har tre egenskaper. Egenskaperna är *”language”*, *”id”* och *”text”*. Egenskapen *”id”* är ett tal i det här exemplet, men kan vara allt du vill så länge det är unikt i dokumentmatrisen. I det här exemplet använder vi också dokument som skrivits på tre olika språk. Över 15 språk stöds i attitydfunktionen i API för textanalys. Mer information finns i [Språk som stöds i API för textanalys](https://docs.microsoft.com//azure/cognitive-services/text-analytics/text-analytics-supported-languages). Den maximala storleken för ett enskilt dokument är 5 000 tecken och en begäran kan innehålla upp till 1 000 dokument. 

![Skärmbild av avsnittet Begärandetext](../media-draft/3-marker.PNG)

En fullständig begäran, inklusive rubriker och begärans-URL visas i nästa avsnitt. I det här exemplet ser du att begäranden dirigeras till en URL som börjar med `westus`. URL:en ser olika ut beroende på vilken region som du har valt.  

![Avsnitt nummer fyra.](../media-draft/4-marker.PNG) 
![Avsnitt nummer fem.](../media-draft/5-marker.PNG) 

Därefter har vi information om svaret. I det här exemplet ser vi att begäran lyckades och att koden `200` returnerades. Vi kan också se att processen tog 38 msek.

![Avsnitt nummer fem.](../media-draft/6-marker.PNG)  

Slutligen kan se vi svaret på vår begäran. Svaret innehåller den insikt som API för textanalys hade om våra dokument. En matris med dokument returneras till oss, utan den ursprungliga texten. Vi får tillbaka ett *”id”* och en *”poäng”* för varje dokument. API:n returnerar en numerisk poäng mellan 0 och 1. Poäng nära 1 visar en positiv attityd, medan en poäng nära 0 indikerar en negativ attityd. Ett resultat på 0,5 anger en brist på attityd, dvs. ett neutralt uttryck. I det här exemplet har vi två ganska positiva dokument och ett negativt dokument. 
![Avsnitt nummer fem.](../media-draft/7-marker.PNG)  

Grattis! Du har gjort ditt första anrop till API för textanalys utan att behöva skriva en enda rad med kod. Passa på att stanna kvar i konsolen och prova fler anrop. Här följer några förslag:

- Ändra dokumenten i avsnitt nummer 2 och se vad API:n returnerar. 
- Prova de andra metoderna **Identifiera språk**, **Entiteter** och **Nyckelfraser**, med hjälp av samma prenumerationsnyckel.
- Testa att göra ett anrop från en annan region med din prenumeration och se vad som händer. 

API-testkonsolen är ett bra sätt att utforska funktionerna i den här API:n. Nu när du har utforskat på egen hand, ska vi gå vidare till att använda den här kunskapen i ett verkligt scenario.