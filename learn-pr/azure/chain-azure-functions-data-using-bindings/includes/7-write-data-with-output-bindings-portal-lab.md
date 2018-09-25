I vår senaste övning implementerade vi ett scenario för att leta upp bokmärken i en Azure Cosmos DB-databas. Vi konfigurerade en indatabindning att läsa data från vår bokmärkessamling. Men vi kan göra mer. Vi utökar scenariot till att även omfatta dataskrivning. Titta på följande flödesschema:

![Flödesschema som visar processen med att hitta ett bokmärke i vår Cosmos-DB-serverdel. När Azure-funktionen tar emot en begäran med ett bokmärkes-ID kontrollerar den först om begäran är giltig. Om den är ogiltig skapas ett felmeddelande. Om begäran är giltig kontrollerar funktionen om bokmärkes-ID:t förekommer i Cosmos DB. Om det saknas skapas ett felmeddelande. Om bokmärkes-ID:t hittades skapas ett lyckat svar.](../media/7-add-bookmark-flow-small.png)

I det här scenariot får vi begäranden om att lägga till bokmärken i vår samling. Begärandena skickas i önskad nyckel eller önskat ID tillsammans med bokmärkets URL. Som du ser i flödesschemat svarar vi med ett fel om nyckeln redan finns i vår serverdel.

Om den nyckel som skickades till oss *inte* hittas lägger vi till det nya bokmärket i vår databas. Vi skulle kunna nöja oss här, men vi ska göra lite till.

Lägger du märke till ytterligare ett steg i flödesschemat? Så här långt vi inte gjort mycket med de data som vi tar emot när det gäller bearbetning. Vi flytta det som vi tar emot till en databas. I en verklig lösning skulle vi dock förmodligen bearbeta data på något sätt. Vi kan välja att utföra all bearbetning i samma funktion, men i den här övningen visar vi ett mönster som avlastar vidare bearbetning till en annan komponent eller affärslogik.

Vad kan vara ett bra exempel på den här avlasta arbete i vårt scenario med bokmärken? Så vad händer om vi skickar nytt bokmärke till en QR-kodgenererad tjänst? Den här tjänsten skulle, i sin tur, generera en QR-kod för URL:en, lagra avbildningen i blob-minnet och lägga till bildens qr-adress till posten i vår bokmärkessamling. Att anropa en tjänst för att skapa en QR-avbildning tar tid, så i stället för att vänta på resultatet lämnar vi över den till en funktion och låter denna ta hand om detta asynkront.

Precis som Azure Functions har stöd för indatabindningar för integrering av olika källor, har den även en uppsättning mallar för utdatabindningar för att göra det lättare för dig att skriva data till datakällor. Utdatabindningar har även konfigurerats i *function.json*-filen.  Som vi kommer att se i den här övningen kan vi konfigurera vår funktion så att den fungerar med flera datakällor och tjänster.

> [!IMPORTANT]
> Den här övningen bygger vidare på den förra. Den använder samma Azure Cosmos DB-databas och indatabindning. Om du inte har arbetat igenom den här delen rekommenderar vi gör det innan du fortsätter med den här övningen.

## <a name="create-an-http-triggered-function"></a>Skapa en HTTP-utlöst funktion

1. Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

2. Navigera till den funktionsapp som du skapade i föregående avsnitt i portalen.

3. Expandera funktionsappen, hovra sedan över samlingen med funktioner och välj Lägg till (**+**) bredvid knappen **Funktioner**. Den här åtgärden startar funktionsskapandeprocessen. Följande animering illustrerar den här åtgärden.

    ![Animering av plustecknet som visas när användaren för muspekaren över menyalternativet för funktioner.](../media/3-func-app-plus-hover-small.gif)

4. Sidan visar oss den aktuella uppsättningen med utlösare som stöds. Välj **HTTP-utlösare**, vilket är den första posten i följande skärmbild.

    ![Skärmbild som visar en del av utlösarmallens valgränssnitt, där TTP-utlösaren visas först, i den övre vänstra delen av bilden.](../media/5-trigger-templates-small.PNG)


5. I rutan **Ny funktion** som visas till höger anger du nedanstående värden:

    |Fält  |Värde  |
    |---------|---------|
    |Språk     | **JavaScript**        |
    |Namn     |   [!INCLUDE [func-name-add](./func-name-add.md)]     |
    | Auktoriseringsnivå | **Funktion** |

6. Skapa den nya funktionen genom att välja **Skapa**. Detta öppnar filen **index.js** i kodredigeraren och en standardimplementering av den HTTP-utlösta funktionen visas.

    > [!NOTE]
    > I den här övningen snabbar vi upp saker och ting genom att använda *koden* och *konfigurationen* från den föregående delen som en utgångspunkt.

7. Ersätt all kod i **index.js** med koden från följande kodavsnitt och klicka på **Spara** så att ändringen sparas:

   [!code-javascript[](../code/find-bookmark-single.js)]

   Om den här koden ser bekant ut beror det på att den utgör implementeringen av vår [!INCLUDE [func-name-find](./func-name-find.md)]-funktion. Som förväntat fungerar inte funktionen förrän vi definierar samma bindningar.

1. Öppna filen **function.json** från funktionen [!INCLUDE [func-name-add](./func-name-add.md)].

11. Ersätt innehållet i filen med följande JSON:

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

12. Se till att **Spara** alla ändringar.

I föregående steg har vi konfigurerat bindningar för vår nya funktion genom att kopiera bindningsdefinitionerna från en annan. Vi kan naturligtvis skapa en ny bindning via användargränssnittet, men det är bra att förstå att det här alternativet är tillgängligt för dig.

## <a name="try-it-out"></a>Prova

1. Välj **Hämta funktionswebbadress** längst upp till höger och välj **Standard (funktionsnyckel)**. Sedan kopierar du funktionens URL genom att klicka på **Kopiera**.

2. Klistra in URL:en i adressfältet för din webbläsare. Lägg till frågesträngsvärdet `&id=docs` i slutet av den här webbadressen och tryck på Enter för att utföra begäran. Om allt går som det ska bör du se ett svar som innehåller en URL till den här resursen.

Så, var befinner vi oss? Dessutom har hittills egentligen bara replikerat det som vi gjorde i föregående övning. Men det är okej. Vi kopierar det vi som gjorde i den senaste testmiljön så att det kan fungera som en startpunkt för denna. Vi ska snart jobba på de nya momenten. Det vill säga, vi ska skriva till databasen. För detta behöver vi en *utdatabindning*.

## <a name="define-azure-cosmos-db-output-binding"></a>Definiera Cosmos-DB-utdatabindning

I stället för att definiera en ny utdatabindning genom att gå via användargränssnittet, ska du skapa den här bindningen genom att uppdatera konfigurationsfilen *function.json*, manuellt.

1. Kontrollera att filen *function.json* för [!INCLUDE [func-name-add](./func-name-add.md)] är öppen i redigeraren.

1. Kopiera bindningen `bookmark` i filen.

1. Placera markören direkt efter den avslutande klammern (}) och precis före den avslutande hakparentesen (]). Lägg till ett kommatecken (,) och klistra sedan in kopian av bindningen här. Konfigurationsfilen *function.json* bör nu se ut så här:

   [!code-json[](../code/config-new-entry.json?highlight=22-31)]

1. Redigera den bindning som du klistrade in, med följande ändringar:

    |Egenskap   |Tidigare värde  |Nytt värde  |
    |---------|---------|---------|
    |namn     |   bokmärke      |  **newbookmark**       |
    |riktning     |   in      |   **ut**      |
    |id     |      {id}   |   **ta bort den här egenskapen. Den finns inte för utdatabindningen.**      |

1. När du har gjort dessa ändringar bör din fil som ser ut som följande JSON:

    [!code-json[](../code/config-q-complete.json?highlight=22-30)]

Detta var bara en demonstration av hur du även kan skapa bindningar direkt i konfigurationsfilen. Det är klokt i detta exempel eftersom du återanvänder egenskaperna från en annan bindning. Det vill säga du ska återanvända `databaseName`, `collectionName`, och `connection` som du redan har konfigurerat för din Azure Cosmos DB-indatabindning.

> [!NOTE]
> Det faktiska värdet för `connection` i ovanstående JSON-fil blir det namn som anslutningen fick när den skapades.

Innan vi uppdaterar vår kod vi lägger till ännu en bindning som gör att vi kan skicka meddelanden till en kö.

## <a name="define-azure-queue-storage-output-binding"></a>Definiera Azure Queue Storage-utdatabindning

Azure Queue Storage är en tjänst för lagring av meddelanden som kan nås var som helst i världen. Storleken på ett enda meddelande kan vara upp till 64 KB stort och en kö kan innehålla miljontals meddelanden&mdash;, upp till den totala kapaciteten för ett lagringskonto som har definierats. I följande diagram visas på en hög nivå hur en kö används i vårt scenario:

![Diagram som visar konceptet med en lagringskö och två funktioner varav den ena push-överför och den andra stoppar in meddelanden i kön](../media/7-q-logical-small.png)

Här ser vi att den nya funktionen, [!INCLUDE [func-name-add](./func-name-add.md)], lägger till meddelanden i en kö. En annan funktion&mdash;, till exempel en fiktiv funktion med namnet *gen-qr-code*&mdash;, visar popup-meddelanden från samma kö och bearbetar begäran.  Eftersom vi skriver, eller *pushar*, meddelanden till kön från [!INCLUDE [func-name-add](./func-name-add.md)], lägger vi till en ny utdatabindning i vår lösning. Den här gången ska vi skapa bindningen i portalanvändargränssnittet.

1. Välj **Integrera** i funktionsmenyn till vänster och öppna integrationsfliken.

2. Välj **Nya utdata** i kolumnen **Utdata**.
    En lista över alla typer av möjliga utdatabindningar visas.

3. Välj **Azure Queue Storage** i listan och välj sedan **Välj**.
    Den här åtgärden öppnar konfigurationssidan för Azure Queue Storage-utdata.

   Nu ska vi konfigurera en anslutning till ett lagringskonto. Här kommer vår kö att finnas.

4. Välj **nya** till höger om fältet **lagringskontoanslutning**.
   Markeringsfönstret **Lagringskonto** öppnas.

5. När vi satte igång den här modulen och skapade vår funktionsapp, skapades även ett lagringskonto vid samma tillfälle. Det visas i det här fösntret så du kan välja det. Fältet **Lagringskontoanslutning** fylls i med namnet på en anslutning. Om du vill visa anslutningssträngens värde väljer du **Visa värde**.

6. Även om vi kan lämna övriga fält på den här sidan med sina standardvärden ska vi ändra följande för att göra egenskaperna mer begripliga:

    |Egenskap  |Tidigare värde  |Nytt värde  | Beskrivning |
    |---------|---------|---------|---------|
    |Könamn     |    utkö     |  **bookmarks-post-process**      | Det här är namnet på den kö där vi placerar bokmärken så att de kan bearbetas ytterligare via en annan funktion. |
    | Meddelandeparameternamn    |  outputQueueItem       |   **newmessage**      | Det här är den bindningsegenskap som vi ska använda i koden. |

7. Kom ihåg att välja **Spara** för att spara ändringarna.

## <a name="update-function-implementation"></a>Uppdatera funktionsimplementeringen

Nu har vi alla våra bindningar konfigurerade för funktionen [!INCLUDE [func-name-add](./func-name-add.md)]. Nu är det dags att använda dem i vår funktion.

1.  Välj vår funktion [!INCLUDE [func-name-add](./func-name-add.md)] så öppnas filen **index.js** i kodredigeraren.

2. Ersätt all kod i *index.js* med koden från följande kodavsnitt:

   [!code-javascript[](../code/add-bookmark.js)]

Nu ska vi analysera på detaljnivå vad den här koden gör:

* Eftersom den här funktionen förändrar våra data kan vi förvänta oss att HTTP-begäran är en POST och att bokmärkesdata ska ingå i begärandetexten.
* Vår Azure Cosmos DB-indatabindning försöker hämta ett dokument eller ett bokmärke med hjälp av den `id` som vi tar emot. Om den hittar en post ställs objektet `bookmark` in. Villkoret `if(bookmark)` kontrollerar om en post hittades.
* Att lägga till den i databasen är lika enkelt som att ställa in `context.bindings.newbookmark`-bindningsparametern för den nya bokmärkespost som vi har skapat som en JSON-sträng.
* Att skicka ett meddelande till vår kö är lika enkelt som att konfigurera `context.bindings.newmessage parameter`.

> [!NOTE]
> Den enda aktivitet som du utförde var att skapa en köbindning. Du skapade aldrig kön uttryckligen. Du har nu sett hur kraftfulla bindningar är! Precis som följande text anger skapas kön automatiskt åt dig om den inte finns.

![Skärmbild som visar att kön kommer att skapas automatiskt.](../media/7-q-auto-create-small.png)

Klart! Låt oss titta på vårt arbete i praktiken i nästa avsnitt.

## <a name="try-it-out"></a>Prova

Nu när vi har flera utdatabindningar blir testen lite svårare. Medan vi i tidigare övningar nöjde oss med att testa genom att skicka en HTTP-begäran och en frågesträng, vill vi nu utföra en HTTP-post. Vi måste också kontrollera om meddelandena kommer fram till kön.

1. Med vår funktion, [!INCLUDE [func-name-add](./func-name-add.md)], vald i portalen Funktionsappar väljer du menyalternativet Test längst till vänster så att det expanderas.

2. Välj menyalternativet **Testa** och se till att testpanelen är öppen. Det bör se ut ungefär som på följande skärmbild:

    ![Skärmbild som visar funktionen Testpanel expanderad.](../media/7-test-panel-open-small.png)

    > [!IMPORTANT]
    > Se till att **POST** är valt i listrutan för HTTP-metod.

3. Ersätt hela innehållet i begärandetexten med följande JSON-nyttolast:

    ```json
    {
        "id": "docs",
        "url": "https://docs.microsoft.com/azure"
    }
    ```

4. Välj **Kör** längst ned på testpanelen.

5. Kontrollera att meddelandet ”Bokmärket har lagts till” visas i fönstret **Utdata** som du ser i följande diagram:

    ![En skärmbild som visar testpanelen och resultatet av ett misslyckat test.](../media/7-test-exists-small.png)

6. Ersätt nu begärandetexten med följande nyttolast:

    ```json
    {
        "id": "github",
        "url": "https://www.github.com"
    }
    ```
7. Välj **Kör** längst ned på testpanelen.

8. Kontrollera att meddelandet ”Bokmärket har lagts till” visas i rutan *Utdata* som du ser i följande diagram.

    ![Skärmbild som visar testpanelen och resultatet av ett lyckat test.](../media/7-test-success-small.png)

Grattis! [!INCLUDE [func-name-add](./func-name-add.md)] fungerar som avsett, men vad gäller för den köåtgärd som vi hade i koden? Så nu ska vi se om något har skrivits till en kö.

### <a name="verify-that-a-message-is-written-to-the-queue"></a>Kontrollera att ett meddelande skrivs till vår kö

Azure Queue Storage-köer finns på ett lagringskonto. Du har valt lagringskontot i den här övningen redan när du skapar utdatabindningen.

1. Gå till huvudsökrutan i Azure-portalen och skriv **lagringskonton**. Välj **Lagringskonton** bland sökresultaten under kategorin **Tjänster**.

      ![Skärmbild som visar sökresultatet för lagringskontot i den huvudsakliga sökrutan.](../media/7-search-for-sa-small.png)

2. I listan över konton som ska returneras väljer du det lagringskonto som du använde för att skapa utdatabindningen **newmessage**.
   Inställningarna för lagringskontot visas i huvudfönstret i portalen.

3. Välj alternativet **Köer** från listan över **tjänster**.
   Då visas en lista över köer som värdhanteras av det här lagringskontot. Kontrollera att kön **bookmarks-post-process** finns, enligt följande skärmbild:

      ![Skärmbild som visar vår kö i listan över köer som värdhanteras av det här lagringskontot](../media/7-q-in-list-small.png)

4. Öppna kön genom att välja **bookmarks-post-process**.
   De meddelanden som finns i kön visas i en lista. Om allt gått enligt planen innehåller kön meddelandet som du har skapade när du skickade ett bokmärke till databasen. Det ska se ut som i följande exempel:

    ![Skärmbild som visar vårt meddelande i kön](../media/7-message-in-q-small.png)

   I det här exemplet ser du att meddelandet har fått ett unikt ID och fältet **MEDDELANDETEXT** visar vårt bokmärke i JSON-strängformat.

5. Du kan testa funktionen ytterligare genom att ändra begärandetexten i panelen Test med nya ID-/url-datauppsättningar och köra funktionen. Titta på den här kön för att se fler meddelanden tas emot. Du kan även leta i databasen för att kontrollera att nya poster har lagts till.

I den här övningen har vi utökat dina kunskaper om bindningar till utdatabindningar genom att skriva data till din Azure Cosmos DB. Vi har gått vidare och lagt till ännu en utgående bindning för att skicka meddelanden till en Azure-kö. Detta visar den verkliga kraften i bindningar för att hjälpa dig att forma och flytta data från inkommande källor till en mängd olika mål. Vi har inte skrivit någon databaskod eller varit tvungna att hantera anslutningssträngar själva. I stället vi har konfigurerat bindningar deklarativt och låtit plattformen ta hand om säkra anslutningar, skala vår funktion och skala våra anslutningar.