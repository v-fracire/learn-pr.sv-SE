Varje funktion måste ha en och endast en utlösare. Den definierar hur vår kod utlöses om du vill köra. Förutom en utlösare definierar vi bindningar som ansluter vi till datakällor. Om du kommer ihåg från vårt diagram över lösningen, som vi vill skicka meddelanden till tre köer. Därför ska vi definiera anslutningarna som utdatabindningar i vår funktion. Vi kan skapa bindningar via den **Utdatabindning** Användargränssnittet. Men om du vill spara tid, ska vi redigera konfigurationsfilen direkt.

1. Välj vår funktion [!INCLUDE [func-name-discover](./func-name-discover.md)], Funktionsappar-portalen.

1. Expandera den **visa filer** menyn till höger på skärmen.

1. Under den **visa filer** fliken **function.json** att öppna konfigurationsfilen i redigeraren.

1. Ersätt hela innehållet i konfigurationsfilen med följande JSON.

[!code-json[](../code/function.json)]

Vi har lagt till tre nya bindningar konfig.

- Varje ny bindning är av typen `queue`. Dessa bindningar finns för de tre köer som vi ska fylla med vår feedbackmeddelanden till när vi vet känslan i din feedback.
- Varje har en riktning som definierats som `out`, eftersom vi ska skicka meddelanden till dessa köer.
- Varje bindning använder samma anslutning till våra storage-konto.
- Varje bindning har en unik `queueName` och `name`.

Skicka meddelanden till en kö är lika enkelt som säger, till exempel `context.bindings.negativeFeedbackQueueItem = "<message>"`.

## <a name="update-implementation-to-sort-feedback-into-queues-based-on-sentiment-score"></a>Uppdatera implementering för att sortera feedback i köer baserat på sentimentets poäng

Målet med våra feedback bildsorteringsläget är att sortera feedback i tre buckets, positiva, neutral och negativa. Hittills har vi vår indatakön visar vår kod för att anropa API för textanalys, och vi har definierat vårt utgående köer. I det här avsnittet lägger vi till logik för att flytta meddelanden till de köer som baseras på sentiment.

1. Gå till vår funktion [!INCLUDE [func-name-discover](./func-name-discover.md)], och öppna `index.js` i kodredigeraren igen.

1. Ersätt implementeringen med följande uppdatering.

[!code-javascript[](../code/discover-sentiment+sort.js?highlight=25-48)]

Vi har lagt till den markerade koden vår implementering. Koden tolkar svaret från API för textanalys cognitive service. Baserat på attitydsresultatet meddelandet vidarebefordras till någon av eller tre utgående köer. Koden för att skicka meddelandet är bara ställer in rätt bindningsparametern.

## <a name="try-it-out"></a>Prova

Om du vill testa den uppdaterade implementeringen, ska vi gå tillbaka till Lagringsutforskaren.

1. Navigera till din resursgrupp i den **resursgrupper** i portalen.

1. Välj den resursgrupp som används i den här lektionen.

1. I den **resursgrupp** panelen att öppnas, leta upp posten Storage-konto och välj den.
    ![Skärmbild lagringskonto har valts i fönstret resursgrupp.](../media/select-storage-account.png)

1. Välj **Lagringsutforskaren (förhandsversion)** menyn till vänster i Storage-konto-fönstret.  Den här åtgärden öppnar Azure Storage Explorer i portalen. Skärmen bör se ut som på följande skärmbild i det här skedet.
    ![Skärmbild av storage explorer visar våra storage-konto, med en kö för närvarande.](../media/storage-explorer-menu-inputq.png)

Vi har en kö som visas under den **köer** samling. Den här kön är [!INCLUDE [input-q](./q-name-input.md)], inkommande kö som vi definierade i föregående avsnitt för test av modulen.

1. Välj [!INCLUDE [input-q](./q-name-input.md)] i den vänstra menyn för att se data explorer för den här kön. Som förväntat, hade inga data i kön. Nu ska vi lägga till ett meddelande till kön med den **Lägg till meddelande** kommandot överst i fönstret.

1. I den **Lägg till meddelande** dialogrutan Ange ”jag har kul med den här övningen”! i den **meddelandetext** fältet och klickar på **OK** längst ned i dialogrutan.

1. Meddelandet visas i datafönstret för [!INCLUDE [input-q](./q-name-input.md)]. Efter några sekunder klickar du på **uppdatera** överst i datavyn att uppdatera vyn för kön. Observera att meddelandet stängs efter en stund. Så var det?

1. Högerklicka på den **KÖER** samling i den vänstra menyn. Observera att en *nya* kön visas.
    ![Skärmbild av Storage Explorer som visar hur en ny kö har skapats i samlingen. Kön har ett meddelande.](../media/sa-new-output-q.png)

Kön [!INCLUDE [positive-q](./q-name-positive.md)] skapades automatiskt när ett meddelande har publicerats till den första gången. Du behöver inte skapa den utgående kön innan publicering till den manuellt med Azure Functions för utdata köbindningar! Nu när vi se ett inkommande meddelande har sorterats av vår funktion i [!INCLUDE [positive-q](./q-name-positive.md)], låt oss se där följande meddelanden mark.

1. Med hjälp av samma steg som ovan, Lägg till följande meddelanden till [!INCLUDE [input-q](./q-name-input.md)].

- ”Jag gillar broccoli”!
- ”Microsoft är ett företag”

1. Klicka på **uppdatera** tills [!INCLUDE [input-q](./q-name-input.md)] är tom igen. Den här processen kan ta en stund och kräver flera uppdateringar.

1. Högerklicka på den **KÖER** samling och notera två flera köer som visas. Köer är namngivna [!INCLUDE [neutral-q](./q-name-neutral.md)] och [!INCLUDE [negative-q](./q-name-negative.md)]. Det kan ta några sekunder, så att fortsätta uppdatera den **KÖER** insamling innan nya köer. När du är klar din kö bör se ut så här.

![Skärmbild av Storage Explorer-menyn med fyra köer i samlingen KÖER.](../media/sa-final-q-list.png)

Klicka på varje kö i listan för att se om de har meddelanden. Om du har lagt till de föreslagna meddelandena, bör du se ett meddelande i [!INCLUDE [positive-q](./q-name-positive.md)], [!INCLUDE [neutral-q](./q-name-neutral.md)], och [!INCLUDE [negative-q](./q-name-negative.md)].

Grattis! Nu har vi en fungerande feedback Bildsortering! När meddelanden tas emot i indatakön, använder våra funktionen Text Analytics API-tjänsten för att få ett sentimentpoäng. Funktionen vidarebefordrar baserat på den poängen, meddelanden till lämpliga köer. Även om det verkar som funktionen processer endast ett objekt i taget, Azure Functions-runtime faktiskt läser batchar med objekt i kö och sätta upp andra instanser av vår funktion för att bearbeta dem parallellt.