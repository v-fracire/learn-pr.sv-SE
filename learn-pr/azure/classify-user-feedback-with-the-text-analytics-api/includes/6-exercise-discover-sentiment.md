Nu uppdaterar vi vår funktionsimplementering för att anropa API för textanalys-tjänsten och få en sentimentpoäng.

1. Välj vår funktion, [!INCLUDE [func-name-discover](./func-name-discover.md)], i funktionsappen på portalen.

1. Utöka menyn **Visa filer** till höger på skärmen.

1. Under fliken **Visa filer** väljer du **index.js** för att öppna kodfilen i redigeringsprogrammet.

1. Ersätt hela innehållet i **index.js** med följande JavaScript och **Spara**.

    [!code-javascript[](../code/discover-sentiment-sort.js?highlight=7)]

1. Uppdatera värdet för `accessKey` i koden som du klistrade in med åtkomstnyckeln för API för textanalys, som du sparade tidigare i den här modulen. 

1. Uppdatera värdet för `uri` med den region som du fick din åtkomstnyckel från.

Vi tittar på vad som händer i den här koden:

- Varje anrop till Text Analytics-tjänsten behöver åtkomst till nyckeln, som vi lägger till som `Ocp-Apim-Subscription-Key`-huvud. 
- Varje anrop görs till en regionsspecifik slutpunkt, definierad av `uri` i vår kod.
- Längst ned i kodfilen har vi definierat en `documents`-matris. Den här matrisen är den nyttolast som vi skickar till textanalystjänsten.
- `documents`-matrisen innehåller en enda post i det här fallet, vilket är det kömeddelande som utlöste vår funktion. Även om vi har bara ett dokument i matrisen betyder det inte att vår lösning bara kan hantera ett meddelande i taget. Azure Functions-körmiljön hämtar och bearbetar meddelanden i batchar och anropar flera instanser av funktionen *parallellt*. För närvarande är standardstorleken för batchar 16, och den maximala batchstorleken är 32.
- `id` måste vara unikt i matrisen. Egenskapen `language` anger språket i dokumenttexten.
- Vi anropar sedan metoden `get_sentiments`, som använder HTTPS-modulen för att göra anrop till API för textanalys. Observera att vi skickar vår prenumerationsnyckel, eller åtkomstnyckel, i huvudet för varje begäran.
- När tjänsten returnerar anropas vår `response_handler`, och vi loggar svaret till konsolen med hjälp av `context.log`


## <a name="try-it-out"></a>Prova

Innan vi tittar på att sortera i köer gör vi en testkörning av det vi har hittills.

1. Med vår funktion, [!INCLUDE [func-name-discover](./func-name-discover.md)], vald i portalen Funktionsappar, klickar du på menyalternativet **Testa** längst till höger.

1. Välj menyalternativet **Testa** och se till att testpanelen är öppen.

1. Lägg till en textsträng i begärandetexten enligt det som visas på skärmbilden.

    ![Skärmbild som visar funktionen Testpanel expanderad.](../media/test-panel-open-small.png)

1.  Klicka på **Kör** längst ned på testpanelen.

1. Kontrollera att fliken **Loggar** expanderas längst ned till vänster på huvudskärmen under kodredigeraren.

1. Kontrollera att fliken **Loggar** visar logginformation som funktionen har slutfört. I fönstret visas även svaret från API för textanalys-anropet.

    ![Skärmbild som visar testpanelen och resultatet av ett lyckat test.](../media/sentiment-response-log1.png)

Grattis! [!INCLUDE [func-name-discover](./func-name-discover.md)] fungerar som avsett. I det här exemplet skickade vi ett väldigt positivt meddelande och fick ett resultat på över 0,98. Försök att ändra meddelandet till något mindre optimistiskt. Kör sedan testet och notera svaret.

## <a name="add-a-message-to-the-queue"></a>Lägga till ett meddelande i kön

Vi upprepar testet. I stället för att använda testfönstret i portalen placerar vi den här gången ett meddelande i indatakön och ser vad som händer.

1. Gå till din resursgrupp i delen **Resursgrupper** i portalen.

1. Välj <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>, resursgruppen som används i den här enheten.

1. I panelen **Resursgrupp** som visas letar du reda på lagringskontot och markerar det.

    ![Skärmbild av valt lagringskonto i fönstret Resursgrupp.](../media/select-storage-account.png)

1. Välj **Storage Explorer (förhandsversion)** på den västra menyn i lagringskontots huvudfönster. Den här åtgärden öppnar Azure Storage Explorer i portalen. 

    ![Skärmbild av Storage Explorer som visar vårt lagringskonto, utan köer för närvarande.](../media/sa-no-queue.png)

    Som du ser har vi inte några köer i det här lagringskontot än, så vi lägger till en.

5. Som du kanske kommer ihåg från en tidigare del i den här enheten gav vi kön ett namn som är associerat med vår utlösare **new-feedback-q**. Högerklicka på objektet **Queues** (Köer) i lagringsutforskaren och välj *Skapa kö*.

1. I den dialogruta som öppnas anger du **new-feedback-q** och klickar på **OK**. Nu har vi vår indatakö.

1. Välj den nya kön på menyn till vänster så ser du datautforskaren för den här kön. Som förväntat är kön tom. Vi lägger till ett meddelande till kön med kommandot **Lägg till meddelande** överst i fönstret.

1. I dialogrutan **Lägg till meddelande** anger du ”Det här meddelandet kom från vår indatakö, new-feedback-q” i fältet **Meddelandetext** och klickar på **OK** längst ned i dialogrutan.

1. Observera meddelandet, som liknar meddelandet på följande skärmbild, i datautforskaren.
    ![Skärmbild av lagringsutforskaren som visar vårt lagringskonto med det meddelande som vi skapade i kön.](../media/message-in-input-queue.png)

1. Efter några sekunder klickar du på **Uppdatera** för att uppdatera vyn av kön. Observera att kön är **tom** igen. Något måste ha läst meddelandet från kön.

1. Gå tillbaka till vår funktion i portalen och öppna fliken **Övervaka**. Välj det senaste meddelandet i listan. Observera att vår funktion bearbetade det kömeddelande som vi hade publicerat till new-feedback-q. Resultaten kan vara fördröjda i den här loggen, så du kan behöva vänta några minuter och trycka på *Uppdatera*.

    ![Skärmbild av instrumentpanelen för övervakning som visar en post som talar om för oss att vår funktion bearbetade det kömeddelande som vi publicerade till new-feedback-q.](../media/message-in-monitor.png)

    I det här testet gjorde vi en fullständig tur och retur-resa genom att publicera något till kön och sedan se hur funktionen bearbetade det.

Vi gör framsteg med lösningen! Vår funktion utför nu något användbart. Den tar emot text från våra indatakö och anropar sedan API för textanalys-tjänsten för att få en sentimentpoäng. Vi har också lärt oss att testa funktionen via Azure-portalen och Storage Explorer. I nästa övning ser vi hur enkelt det är att skriva till köer med hjälp av utdatabindningar.