Nu ska vi uppdatera vår funktion implementering för att anropa API för textanalys-tjänsten och få tillbaka ett sentimentpoäng.

1. Välj vår funktion [!INCLUDE [func-name-discover](./func-name-discover.md)], i vårt function-app i portalen.

1. Expandera den **visa filer** menyn till höger på skärmen.

1. Under den **visa filer** fliken **index.js** att öppna kodfilen i redigeraren.

1. Ersätt hela innehållet i **index.js** med följande JavaScript och **spara**.

[!code-javascript[](../code/discover-sentiment-sort.js?highlight=7)]

Låt oss titta på vad som händer i den här koden:

- För att anropa tjänsten text analytics, ange `accessKey`, markerade i kodfragmentet, till den nyckel som du sparade tidigare.
- Uppdatera `uri` till den region som du fick din åtkomstnyckel om regionen skiljer sig från *westus* visas i det här exemplet.
- Längst ned i kodfilen vi har definierat en `documents` matris. Den här matrisen är nyttolasten som vi skickar till Text Analytics-tjänsten.
- Den `documents` matris innehåller en enda post i det här fallet, vilket är kömeddelandet som vår funktion som utlöses av. Även om vi har bara ett dokument i vår matris, betyder det att vår lösning bara kan hantera ett meddelande i taget. Azure Functions-runtime hämtar och bearbetar meddelanden i batchar, anropa flera instanser av vår funktion *parallellt*. För närvarande batch standardstorleken är 16 och den maximala batchstorleken är 32.
- Den `id` måste vara unikt inom matrisen. Den `language` egenskap anger språket i dokumentet.
- Vi sedan anropa vår metod `get_sentiments`, som använder HTTPS-modulen för att göra anrop till API för textanalys. Observera att vi skickar vår prenumeration eller åtkomst, nyckel i rubriken för varje begäran.
- När tjänsten returnerar vår `response_handler` kallas och vi logga svaret i konsolen med hjälp av `context.log`


## <a name="try-it-out"></a>Prova

Innan vi tittar på sortering i köer, låt oss ta vad vi har för ett prov kört.

1. Med vår funktion [!INCLUDE [func-name-discover](./func-name-discover.md)], valt i Funktionsappar-portalen, klicka på menyalternativet Test längst till vänster så att det expanderas.

1. Välj den **testa** menyn objekt och kontrollera att du har panelen test för att öppna. Följande skärmbild visar hur den ska se ut.

    ![Skärmbild som visar funktionen Test panelen expanderas.](../media/test-panel-open-small.png)

1. Lägg till en textsträng i frågans brödtext som visas i skärmbilden.

1.  Klicka på **kör** längst ned på panelen test.

1. Kontrollera att den **loggar** fliken utökas längst ned till vänster på skärmen huvudsakliga under Kodredigeraren.

1. Kontrollera att den **loggar** fliken visas logginformation som funktionen har slutförts. I fönstret visas också svaret från Text Analytics API-anrop.

![Skärmbild som visar testa panelen och resultatet av testet lyckats.](../media/sentiment-response-log1.png)

Grattis! Den [!INCLUDE [func-name-discover](./func-name-discover.md)] fungerar som avsett. I det här exemplet vi skickas i ett mycket upbeat meddelande och tog emot ett resultat på via 0,98. Försök att ändra meddelandet till något mindre optimistisk, köra testet och notera svaret.

## <a name="add-a-message-to-the-queue"></a>Lägga till ett meddelande i kön

Vi upprepar du testet. Den här gången istället för att använda testfönstret i portalen, vi faktiskt placera ett meddelande i indatakön och titta på vad som händer.

1. Navigera till din resursgrupp i den **resursgrupper** i portalen.

1. Välj den resursgrupp som används i den här lektionen.

1. I den **resursgrupp** panelen som visas, leta upp posten Storage-konto och välj den.

    ![Skärmbild lagringskonto har valts i fönstret resursgrupp.](../media/select-storage-account.png)

1. Välj **Lagringsutforskaren (förhandsversion)** menyn till vänster i Storage-konto-fönstret.  Den här åtgärden öppnar Azure Storage Explorer i portalen. Skärmen bör se ut som på följande skärmbild i det här skedet.

![Skärmbild av storage explorer visar våra storage-konto, med inga köer för närvarande.](../media/sa-no-queue.png)

Som du ser vi inte har inga köer än i det här lagringskontot, så Låt oss fixa.

5. Om du kommer ihåg från tidigare i den här lektionen vi valde namnet kön som är associerade med vår utlösare **ny feedback q**. Högerklicka på den **köer** objektet i storage explorer och välj *Skapa kö*.

1. Ange i dialogrutan som öppnas **ny feedback q** och klicka på **OK**. Nu har vi vår inkommande kö.

1. Välj den nya kön i den vänstra menyn för att se data explorer för den här kön. Som förväntat, är kön tom. Nu ska vi lägga till ett meddelande till kön med den **Lägg till meddelande** kommandot överst i fönstret.

1. I den **Lägg till meddelande** dialogrutan Ange ”det här meddelandet kommer ifrån vår indatakö ny feedback q” i den **meddelandetext** fältet och klickar på **OK** längst ned i dialogrutan.

1. Se meddelandet liknar meddelandet i följande skärmbild i datautforskaren.
    ![Skärmbild av storage explorer visar våra storage-konto, ett meddelande som vi skapade i kön.](../media/message-in-input-queue.png)

1. Efter några sekunder klickar du på **uppdatera** att uppdatera vyn för kön. Observera att kön är tom igen. Något måste ha läs meddelandet från kön.

1. Gå tillbaka till våra funktionen i portalen och öppna den **övervakaren** fliken. Välj det senaste meddelandet i listan. Observera att vår funktion bearbetas kömeddelandet som vi har publicerat den nya-feedback-q.

![Skärmbild av Monitor-instrumentpanel som visar en post som talar om för oss att vår funktion bearbetas kömeddelandet som vi lade upp ny feedback q.](../media/message-in-monitor.png)

Vi gjorde en fullständig serveranrop av skicka något i vår kö och sedan se funktionen bearbeta dem i det här testet.

Vi gör framsteg med vår lösning! Vår funktion är nu göra ett beskrivande. Det är tar emot text från våra inkommande kö och sedan anropa till Text Analytics API-tjänsten för att få ett sentimentpoäng.  Vi har också lärt dig hur du testar vår funktion via Azure portal och Storage Explorer. I nästa övning ser vi hur enkelt det är att skriva till köer med hjälp av utdatabindningar.