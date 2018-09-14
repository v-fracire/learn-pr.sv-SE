I vår senaste övningen implementerat vi ett scenario för att leta upp bokmärken i en Azure Cosmos DB-databas. Vi har konfigurerat en indatabindning att läsa data från våra bokmärken samling. Men bara läsning av data är tråkiga, så Låt oss göra mer. Nu ska vi Expandera scenario om du vill inkludera skrivning. Överväg följande flödesschema.

![Flow-diagram som visar processen att lägga till ett bokmärke i vår Cosmos-DB backend-server](../media-draft/add-bookmark-flow-small.png)

I det här scenariot får vi begäranden för att lägga till bokmärken i vår lista. Begäranden skickas i önskad nyckel eller ID: T, tillsammans med bokmärkets URL. Som du ser i diagrammet flödet svarar vi med ett fel om nyckeln redan finns i vår backend-server.

Om nyckeln som skickades till oss *inte* hittas lägger vi till det nya bokmärket till vår databas. Vi kan stoppa det, men låt oss göra lite mer.

Lägg märke till ytterligare ett steg i flödesschemat? Så här långt vi inte gjort mycket med data som vi tar emot när det gäller bearbetning. Vi flytta vad vi tar emot till en databas. I en verklig lösning är det dock möjligt att vi skulle förmodligen bearbeta data på något sätt. Vi kan välja att göra all bearbetning i samma funktion, men i den här övningen som vi visar ett mönster som avlastar vidare bearbetning till en annan komponent eller affärslogik.

Vad kan vara ett bra exempel på den här avlasta arbete i vårt scenario bokmärken? Bra vad händer om vi skicka nytt bokmärke till en QR-kod generation tjänst? Tjänsten skulle, i sin tur genererar en QR-kod för URL: en, lagra avbildningen i blob storage och lägga till bildens qr-adressen till posten i vår bokmärken samling. Anropa en tjänst för att skapa en qr-avbildning tar tid så snarare än att vänta tills resultatet, vi lämnar den för en funktion och låt den tar hand om detta asynkront.

Precis som Azure Functions har stöd för indatabindningar för integrering av olika källor, har också en uppsättning utdata bindningar mallar för att göra det lättare för dig att skriva data till datakällor. Utdatabindningar också har konfigurerats i den *function.json* fil.  Som vi ser i den här övningen, kan vi konfigurera vår funktion för att arbeta med flera datakällor och tjänster.

> [!IMPORTANT]
> Den här övningen bygger på den här övningen i den sista enheten, nämligen den använder samma Azure Cosmos DB-databas och indatabindning. Om du inte har arbetat med den enheten, rekommenderar vi gör det innan du fortsätter med den här övningen.

## <a name="create-an-httptriggered-function"></a>Skapa en HTTP_triggered-funktion

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

2. Gå till funktionsappen som du skapade i den här modulen i Azure-portalen.

3. Expandera funktionsappen, och sedan hovra över samlingen funktioner och välj Lägg till (**+**) bredvid knappen **Functions**. Den här åtgärden startar funktionen skapandeprocessen. Följande animeringen illustrerar den här åtgärden.

![Animering av på plustecknet som visas när användaren för muspekaren över menyalternativet funktioner.](../media-draft/func-app-plus-hover-small.gif)

4. Sidan visar oss den aktuella uppsättningen stöds utlösare. Välj **HTTP-utlösare**, vilket är den första posten i följande skärmbild.

![Skärmbild av en del av utlösaren mall markeringen Användargränssnittet med HTTP-utlösaren visas först i upp till vänster i avbildningen.](../media-draft/trigger-templates-small.PNG)

5. Fyll i **ny funktion** dialogrutan som visas till höger med hjälp av följande värden.

|Fält  |Värde  |
|---------|---------|
|Språk     | **JavaScript**        |
|Namn     |   [!INCLUDE [func-name-add](./func-name-add.md)]     |
| Auktoriseringsnivå | **Funktionen** |

5. Välj **skapa** att skapa vår funktion som öppnas filen index.js i kodredigeraren och visar en standardimplementering av HTTP-utlöst funktion.

I den här övningen, vi får snabbare saker med hjälp av den *kod* och *configuration* från den föregående enheten som en startpunkt.

6. Ersätt all kod i index.js med kod i följande fragment och klicka på **spara** att spara ändringen. 

[!code-javascript[](../code/find-bookmark-single.js)]

Om den här koden ser bekant ut är eftersom den är implementeringen av våra [!INCLUDE [func-name-find](./func-name-find.md)] funktion. Som förväntat, fungerar inte funktionen tills vi definierar bindningarna som samma.  

7. Öppna den *function.json* fil från den [!INCLUDE [func-name-find](./func-name-find.md)] funktion. Du hittar det genom att öppna den **visa filer** menyn till höger om Kodredigeraren.

8. Kopiera hela innehållet i den här filen.

9. Öppna den *function.json* fil från den [!INCLUDE [func-name-add](./func-name-add.md)] funktion.

10. Ersätt innehållet i den här filen med innehåll som du kopierade från den *function.json* fil som är associerad med den [!INCLUDE [func-name-find](./func-name-find.md)] funktion. När du är klar bör din function.json innehåller följande JSON.

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "documentDB",
      "name": "bookmark",
      "databaseName": "func-io-learn-db",
      "collectionName": "Bookmarks",
      "connection": "unit3test_DOCUMENTDB",
      "direction": "in",
      "id": "{id}"
    }
  ],
  "disabled": false
}
```

11. Se till att **spara** alla ändringar.

I föregående steg, vi har konfigurerat bindningar för vår nya funktionen genom att kopiera bindningsdefinitionerna från en annan. Vi kan naturligtvis skapas en ny bindning via Användargränssnittet, men det är bra att förstå att detta alternativ är tillgängliga för dig.

## <a name="try-it-out"></a>Prova

1. Som vanligt, klickar du på **<> / hämta Funktionswebbadress** längst upp till höger, Välj **standard (funktionsnyckel)**, och klicka sedan på **kopia** att kopiera funktionen är URL: en.

2. Klistra in funktionens URL som du kopierade i webbläsarens adressfält. Lägg till frågesträngvärdet `&id=docs` i slutet av den här webbadressen och tryck på knappen `Enter` på tangentbordet för att utföra begäran. Alla ska, bör du se ett svar som innehåller en URL till den här resursen.

Därför var finns vi på? Dessutom har hittills vi egentligen bara replikerat det vi gjorde i den föregående övningen. Men det är ok. Vi kopierar det vi gjorde i den senaste testmiljön som fungerar som en startpunkt för denna. Vi arbetar på den nya grejer därefter nämligen skrivning till databasen. För att vi behöver en *utdatabindning*.

## <a name="define-azure-cosmos-db-output-binding"></a>Definiera Azure Cosmos DB-utdatabindning

Snarare än definierar en ny utdatabindning genom att gå via användargränssnittet, ska vi skapa den här bindningen genom att uppdatera konfigurationsfilen *function.json*, manuellt. 

1. Öppna den **function.json** -filen för den här funktionen i redigeraren genom att välja den i den **visa filer** lista.

2. Kopiera bindningen med namnet `bookmark` i filen.

3. Placera markören direkt efter den avslutande klammer, precis före den avslutande hakparentesen. Lägg till ett kommatecken `,` och klistra in kopian av bindningen här. Din *function.json* config bör nu se ut så här.

[!code-json[](../code/config-new-entry.json?highlight=22-31)]

4. Redigera bindning som vi klistrade in, med följande ändringar.


|Egenskap   |Gammalt värde  |Nytt värde  |
|---------|---------|---------|
|name (namn)     |   Bokmärke      |  **newbookmark**       |
|Riktning     |   i      |   **out**      |
|id     |      {id}   |   **ta bort den här egenskapen. Det finns inte för utdata-bindning.**      |

När du gör dessa ändringar kommer avslutas att med en fil som ser ut som följande JSON.

[!code-json[](../code/config-q-complete.json?highlight=22-30)]

Det var bara en demonstration av hur du även kan skapa bindningar direkt i konfigurationsfilen. I det här exemplet är det rimligt eftersom vi återanvänder egenskaper från en annan bindning, nämligen den `databaseName`, `collectionName` och `connection` att vi redan konfigurerats för vår Cosmos-DB-indatabindning.

> [!NOTE]
> Det faktiska värdet för `connection` i ovanstående JSON-filen kan det namn som din anslutning angavs när den skapades.

Innan vi uppdaterar vår kod vi lägger till en mer bindning som gör att vi kan skicka meddelanden till en kö.

## <a name="define-azure-queue-storage-output-binding"></a>Definiera Azure Queue Storage-utdatabindning

Azure Queue storage är en tjänst för att lagra meddelanden som kan nås från var som helst i världen. Ett enskilt meddelande som kan vara upp till 64 KB och en kö kan innehålla miljontals meddelanden upp till den totala kapacitetsgränsen för det lagringskonto som den definieras. I följande diagram visas på en hög nivå hur en kö ska användas i vårt scenario.

![Diagram som visar konceptet med en lagringskö och två funktioner push-överföra och dyker meddelanden till kön.](../media-draft/q-logical-small.png)

Här ser vi som våra nya funktionen [!INCLUDE [func-name-add](./func-name-add.md)], lägger till meddelanden till en kö. En annan funktion, till exempel en fiktiva funktion kallas *gen streckkod*, kommer popup-meddelanden från samma kö och bearbetar begäran.  Eftersom vi skriver, eller *push*, meddelanden till kön från [!INCLUDE [func-name-add](./func-name-add.md)], lägger vi till en ny utdatabindning till vår lösning. Nu ska vi skapa bindningen i användargränssnittet för den här gången.

1. Välj **integrera** i funktionen menyn till vänster och öppna fliken integration.

2. Välj **+ nya utdata** under den **utdata** kolumn. En lista över alla typer av eventuella utdata-bindning visas.

3. Klicka på **Azure Queue Storage** i listan och sedan den **Välj** knappen. Den här åtgärden öppnar konfigurationssidan för Azure Queue Storage-utdata.

Nu ska ange vi en anslutning till lagringskonto. Det här är där våra kö kommer att finnas.

4. I fältet med namnet **lagringskontoanslutning** på den här sidan, klickar du på *nya* till höger om fältet tomt. Den här åtgärden öppnar den **Lagringskonto** dialogruta för filval. 

5. När vi igång den här modulen och skapat vårt funktionsapp, skapades även ett storage-konto när som helst. Den visas i den här dialogrutan, så gå vidare och välja den. Den **lagringskontoanslutning** fylls med namnet på en anslutning. Om du vill visa Anslutningssträngens värde klickar du på **Visa värde**.

6. Även om vi kan lämna övriga fält på den här sidan med sina standardvärden, låt oss ändra följande för att låna ut mer begripligt egenskaper.


|Egenskap  |Gammalt värde  |Nytt värde  | Beskrivning |
|---------|---------|---------|---------|
|Könamn     |    utkö     |  **bookmarks-post-process**      | Det här är namnet på den kö som vi använder för att placera bokmärken till så att de kan bearbetas ytterligare genom att en annan funktion. |
| Meddelandeparameternamn    |  outputQueueItem       |   **newMessage**      | Det här är bindningsegenskapen som vi ska använda i kod. |


7. Kom ihåg att klicka på **spara** att spara dina ändringar.

## <a name="update-function-implementation"></a>Uppdatera funktionen implementering

Nu har vi alla våra bindningar har ställt in för den [!INCLUDE [func-name-add](./func-name-add.md)] funktion. Är det dags att använda dem i vår funktion.

1.  Klicka på vår funktion [!INCLUDE [func-name-add](./func-name-add.md)], så att du öppnar *index.js* i kodredigeraren.

2. Ersätt all kod i index.js med koden från följande kodavsnitt.

[!code-javascript[](../code/add-bookmark.js)]

Nu ska vi analys på detaljnivå vad den här koden gör.

* Eftersom den här funktionen ändras våra data kan vi förväntar oss HTTP-begäran till ett INLÄGG och bokmärke data som ska ingå i begärandetexten.
* Vår Cosmos DB-indatabindning försöker hämta ett dokument eller ett bokmärke, med hjälp av den `id` som vi tar emot. Om den hittar en post i `bookmark` objekt anges. Den `if(bookmark)` villkoret kontrollerar om en post hittades.
* Att lägga till i databasen är en enkel som den `context.bindings.newbookmark` bindningsparametern för den nya posten för bokmärket, som vi har skapat som en JSON-sträng.
* Skicka meddelanden till vår kön är lika enkelt som den `context.bindings.newmessage parameter`.

> [!NOTE]
> Den enda aktivitet som vi utförde var att skapa en kö-bindning. Vi skapat aldrig kön uttryckligen. Du bevittnat kraften hos bindningar! Eftersom följande står det skapas automatiskt i kön för dig om det inte finns!

![Skärmbild som anropar sig att kön automatiskt-skapas.](../media-draft/q-auto-create-small.png)

Därför är det – Låt oss se vårt arbete i praktiken i nästa avsnitt.

## <a name="try-it-out"></a>Prova

Nu när vi har flera utdatabindningar blir våra tester lite svårare. Medan vi kunde innehåll när du vill testa genom att skicka en HTTP-förfrågan och en frågesträng i föregående labs, kommer vi vill utföra en HTTP Post nu. Vi måste också kontrollera om meddelanden gör den till en kö.

1.  Med vår funktion [!INCLUDE [func-name-add](./func-name-add.md)], valt i Funktionsappar-portalen, klicka på menyalternativet Test längst till vänster så att det expanderas.

2. Välj den **testa** menyn objekt och kontrollera att du har panelen test för att öppna. Följande skärmbild visar hur den ska se ut. 

![Skärmbild som visar funktionen Test panelen expanderas.](../media-draft/test-panel-open-small.png)

> [!IMPORTANT]
> Se till att **POST** väljs i listrutan för HTTP-metod.

3. Ersätt innehållet i begärandetexten med följande JSON-nyttolasten.

```json
  {
      "id": "docs",
      "url": "https://docs.microsoft.com/azure"
  }
  ```

4. Klicka på **kör** längst ned på panelen test. 

5. Kontrollera att den *utdata* visas i fönstret i ”bokmärke finns redan”. meddelandet som visas i följande diagram. 

![Skärmbild som visar testa panelen och resultatet av ett misslyckade test.](../media-draft/test-exists-small.png)

6. Ersätt nu begärandetexten med följande nyttolasten. 

```json
  {
      "id": "github",
      "URL": "https://www.github.com"
  }
  ```
7. Klicka på **kör** längst ned på panelen test.

8. Kontrollera den att *utdata* rutan visar meddelandet ”bokmärke har lagts till” som du ser i följande diagram.

![Skärmbild som visar testa panelen och resultatet av testet lyckats.](../media-draft/test-success-small.png)

Grattis! Den [!INCLUDE [func-name-add](./func-name-add.md)] fungerar som avsett, men vad gäller för den kö-åtgärd som vi hade koden? Bra, nu ska vi se om något har skrivits till en kö.

### <a name="verify-that-a-message-is-written-to-our-queue"></a>Kontrollera att ett meddelande skrivs till vår kö

Azure Queue Storage-köer finns i ett lagringskonto. Du har valt lagringskontot i den här övningen redan när du skapar utdata-bindning. 

1. I huvudsakliga sökrutan i Azure-portalen, Skriv *lagringskonton* och i sökresultaten väljer **lagringskonton** under den *Services* kategori. Detta illustreras i följande skärmbild. 

![Skärmbild som visar sökresultatet för Storage-konto i den huvudsakliga sökrutan.](../media-draft/search-for-sa-small.png)

2. I listan över konton som ska returneras, väljer du det lagringskonto som du använde för att skapa den **newmessage** utdatabindning. Inställningarna för lagringskontot visas i huvudfönstret i portalen.

3. Välj den **köer** objekt från listan över tjänster. Då visas en lista över köer som värd för det här lagringskontot. Kontrollera att den **bokmärken-post-process** kön finns, enligt följande skärmbild.

![Skärmbild som visar vår kö i listan över köer som värd för det här lagringskontot](../media-draft/q-in-list-small.png)

4. Klicka på **bokmärken-post-process** att öppna kön. De meddelanden som finns i kön visas i en lista. Om allt gått enligt planen meddelandet som vi har publicerats när vi har lagt till ett bokmärke i vår databas bör finnas i kön och ser ut följande post. 

![Skärmbild som visar vår meddelandet i kön](../media-draft/message-in-q-small.png)

I det här exemplet ser du att meddelandet har fått ett unikt ID och **MEDDELANDETEXT** fält visar våra bokmärke i JSON-Anslutningssträngens format.

5. Du kan testa funktionen ytterligare genom att ändra begärandetexten i panelen Test med nya id-url-datauppsättningar och köra funktionen. Titta på den här kön för att se fler meddelanden som tas emot. Du kan också leta i databasen för att kontrollera nya poster har lagts till. 

I den här övningen expanderat vi vår kunskap om bindningar till utdatabindningar, skriva data till vår Azure Cosmos DB. Vi har gått vidare och lagt till en annan utgående bindning för att skicka meddelanden till en Azure-kö. Detta visar den verkliga kraften i bindningar för att forma och flytta data från inkommande källor till en mängd olika mål. Vi har inte skrivits någon databaskod eller var tvungen att hantera anslutningssträngar själva. I stället vi har konfigurerat bindningar deklarativt och låta plattformen som tar hand om säkra anslutningar, skala vår funktion och skalning av våra anslutningar.