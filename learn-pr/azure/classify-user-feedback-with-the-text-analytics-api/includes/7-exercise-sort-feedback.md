Varje funktion måste ha en enda utlösarbindning. Den definierar hur vår kod utlöses för körning. Förutom en utlösare kan vi definiera bindningar som ansluter oss till datakällor. Om du minns från vårt diagram över lösningen vill vi skicka meddelanden till tre köer. Så vi ska definiera anslutningarna som utdatabindningar i vår funktion. Vi skulle kunna skapa bindningarna via användargränssnittet för **utdatabindning**. Men för att spara tid ska vi redigera konfigurationsfilen direkt.

1. Välj vår funktion, [!INCLUDE [func-name-discover](./func-name-discover.md)], i funktionappsportalen.

1. Utöka menyn **Visa filer** till höger på skärmen.

1. Under fliken **Visa filer** väljer du **function.json** för att öppna konfigurationsfilen i redigeraren.

1. Ersätt hela innehållet i konfigurationsfilen med följande JSON. 

[!code-json[](../code/function.json)]

Vi har lagt till tre nya bindningar till konfigurationsfilen.

- Varje ny bindning är av typen `queue`. De här bindningarna är för de tre köerna som vi ska fylla i våra feedbackmeddelanden till när vi vet feedbackens sentiment.
- Varje bindning har en riktning som definieras som `out`, eftersom vi ska publicera meddelanden till dessa köer.
- Varje bindning använder samma anslutning till vårt lagringskonto.
- Varje bindning har en unik `queueName` och `name`.

Att publicera ett meddelande till kön är lika enkelt som, vi säger `context.bindings.negativeFeedbackQueueItem = "<message>"`.

## <a name="update-implementation-to-sort-feedback-into-queues-based-on-sentiment-score"></a>Uppdatera implementering för sortera feedback i köer baserat på sentimentpoäng

Målet med vår feedbacksortering är att sortera feedback i tre buckets, positiv, neutral och negativ. Hittills har vi vår indatakö, vår kod för att anropa API för textanalys och vi har definierat våra utdataköer. I den här delen har vi lagt till logiken för att flytta meddelanden till köerna baserat på sentiment.

1. Gå till vår funktion, [!INCLUDE [func-name-discover](./func-name-discover.md)], och öppna `index.js` i kodredigeraren igen.

1. Ersätt implementeringen med följande uppdatering.
[!code-javascript[](../code/discover-sentiment+sort.js?highlight=25-48)]

Vi har lagt till den markerade koden i vår implementering. Koden parsar svaret från den kognitiva tjänsten API för textanalys. Baserat på sentimentpoängen vidarebefordras meddelandet till en av de tre utdataköerna. Koden för att publicera meddelandet är bara att ange rätt bindningsparameter.

## <a name="try-it-out"></a>Prova

För att testa den uppdaterade implementeringen går vi tillbaka till Storage Explorer. 

1. Gå till din resursgrupp i delen **Resursgrupper** i portalen.

1. Välj resursgruppen som används i den här lektionen.

1. I panelen **Resursgrupp** som öppnas letar du reda på lagringskontot och markerar det.
![Skärmbild av valt lagringskonto i fönstret Resursgrupp.](../media-draft/select-storage-account.png)

1. Välj **Storage Explorer (förhandsversion)** på den västra menyn i lagringskontots huvudfönster.  Den här åtgärden öppnar Azure Storage Explorer i portalen. Skärmen bör likna följande skärmbild i det här stadiet.
![Skärmbild av Storage Explorer som visar vårt lagringskonto med en kö.](../media-draft/storage-explorer-menu-inputq.png)

Det finns en listad kö under samlingen **Köer**. Kön är [!INCLUDE [input-q](./q-name-input.md)], indatakön vi definierade i det föregående testavsnittet i modulen.

1. Välj [!INCLUDE [input-q](./q-name-input.md)] på menyn till höger så ser du Datautforskaren för den här kön. Som förväntat fanns det inga data i kön. Vi lägger till ett meddelande till kön med kommandot **Lägg till meddelande** överst i fönstret. 

1. I dialogrutan **Lägg till meddelande** skriver du ”Jag har kul med den här övningen!” i **meddelandetextfältet** och klickar på **OK** längst ned i dialogrutan. 

1. Meddelandet visas i datafönstret för [!INCLUDE [input-q](./q-name-input.md)]. Efter några sekunder klickar du på **Uppdatera** överst i datavyn för att uppdatera vyn av kön. Lägg märke till att meddelandet försvinner efter en stund. Men vart tog det vägen?

1. Högerklicka på samlingen **KÖER** på menyn till vänster. Observera ett en *ny* kö visas.
![Skärmbild av Storage Explorer som visar att en ny kö har skapats i samlingen. Kön har ett meddelande.](../media-draft/sa-new-output-q.png)

Kön [!INCLUDE [positive-q](./q-name-positive.md)] skapades automatiskt när ett meddelande publicerades till den för första gången. Med Azure Functions-köutdatabindningar måste du inte manuellt skapa utdatakön innan du publicerar till den! Nu när vi ser att ett inkommande meddelande har sorterats av funktionen till [!INCLUDE [positive-q](./q-name-positive.md)] ska vi se var följande meddelande hamnar.

5. Med samma steg som ovan ska du lägga till följande meddelanden i [!INCLUDE [input-q](./q-name-input.md)].

- ”Jag gillar broccoli!”
- ”Microsoft är ett företag”

6. Klicka på **Uppgradera** tills [!INCLUDE [input-q](./q-name-input.md)] är tom igen. Den här processen kan ta en stund och kräver flera uppdateringar.

1. Högerklicka på samlingen **KÖER** och lägg märke till att två köer till visas. Köerna heter [!INCLUDE [neutral-q](./q-name-neutral.md)] och [!INCLUDE [negative-q](./q-name-negative.md)]. Det kan ta några sekunder, så fortsätt att uppdatera samlingen **KÖER** till du ser de nya köerna. Din kölista bör se ut ungefär så här när du är klar.

![Skärmbild av Storage Explorer som visar fyra köer i samlingen KÖER.](../media-draft/sa-final-q-list.png)

Klicka på varje kö i listan för att se om de har meddelanden. Om du har lagt till de föreslagna meddelandena bör du se ett meddelande i [!INCLUDE [positive-q](./q-name-positive.md)], [!INCLUDE [neutral-q](./q-name-neutral.md)] och [!INCLUDE [negative-q](./q-name-negative.md)].

Grattis! Nu har vi en fungerande feedbacksortering! När meddelanden hamnar i indatakön använder vår funktion tjänsten API för textanalys för att hämta en sentimentpoäng. Baserat på den poängen vidarebefordrar funktionen meddelandena till rätt kö. Även om det verkar som funktionen endast bearbetar ett köobjekt i taget läser Azure Functions-körmiljön faktiskt batchar med köobjekt och startar andra instanser av funktionen för att bearbeta dem parallellt. 