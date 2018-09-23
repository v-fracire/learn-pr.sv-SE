Anta att du vill skapa en enkel söktjänst för bokmärken. Tjänsten är inledningsvis skrivskyddad. Om användare vill hitta en post skickar de en begäran med ID:t för posten och du returnerar URL:en. Flödet beskrivs i flödesschemat nedan:

![Flödesschema som visar processen med att hitta ett bokmärke i vår Cosmos-DB-serverdel. När Azure-funktionen tar emot en begäran med ett bokmärkes-ID kontrollerar den först om begäran är giltig. Om den är ogiltig skapas ett felmeddelande. Om begäran är giltig kontrollerar funktionen om bokmärkes-ID:t förekommer i Cosmos DB. Om det saknas skapas ett felmeddelande. Om bokmärkes-ID:t hittades skapas ett lyckat svar.](../media/5-find-bookmark-flow-small.png)

När användare skickar dig en begäran med lite text försöker du hitta en post i din serverdelsdatabas som innehåller den här texten som en nyckel eller ett ID. Du returnerar ett resultat som anger huruvida du hittade posten eller inte.

Du måste lagra data någonstans. I det här flödesschemat visas datalagret som en Azure Cosmos DB-instans. Men hur ansluter du till databasen från en funktion och läser in data? I en värld av funktioner konfigurerar du en *indatabindning* för jobbet.  Det är enkelt att konfigurera en bindning via Azure Portal. Som du snart kommer att se behöver du inte skriva kod för sådana uppgifter som att öppna en lagringsanslutning. Azure Functions-körningen och bindningar tar hand om de här aktiviteterna åt dig.

## <a name="create-an-azure-cosmos-db-account"></a>Skapa ett Azure Cosmos DB-konto 

> [!NOTE]
> Den här enheten är inte avsedd att vara en självstudiekurs om Azure Cosmos DB. Det finns en fullständig utbildningsväg på Cosmos DB om du vill veta mer efter det att du har slutfört den här modulen.

### <a name="create-a-database-account"></a>Skapa ett databaskonto

Ett databaskonto är en container för hantering av en eller flera databaser. Innan vi kan skapa en databas behöver vi skapa ett databaskonto.

1. Kontrollera att du är inloggad på [Azure Portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Välj knappen **Skapa en resurs** längst upp till vänster i Azure-portalen och välj sedan **Databaser** > **Azure Cosmos DB**.

1. På sidan **Nytt konto** anger du inställningarna för det nya Azure Cosmos DB-kontot.

    | Inställning | Värde | Beskrivning |
    |---------|-------|-------------|
    | ID |*Ange ett unikt namn*|Ange ett unikt namn som identifierar Azure Cosmos DB-kontot. Eftersom `documents.azure.com` läggs till det ID du anger för att skapa din URI ska du använda ett unikt men identifierbart ID.<br><br>Ditt ID får bara innehålla gemener, siffror och bindestreck och måste innehålla mellan 3 och 50 tecken. |
    | API |SQL|API:et avgör vilken typ av konto som skapas. Azure Cosmos DB innehåller fem API:er för ditt program: SQL (dokumentdatabas), Gremlin (grafdatabas), MongoDB (dokumentdatabas), Azure-tabell och Cassandra, där var och en för närvarande kräver ett separat konto. <br><br>Välj **SQL**. För närvarande fungerar Azure Cosmos DB-utlösaren, indatabindningar och utdatabindningar endast med SQL API- och Graph API-konton. |
    | Prenumeration | Concierge-prenumeration | Välj den Azure-prenumeration som ska användas för det här Azure Cosmos DB-kontot. |
    Resursgrupp|Använd befintlig<br><br>Välj sedan **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**. | Vi väljer **Använd befintlig**, eftersom vi vill gruppera alla resurser som har skapats för den här modulen under den kostnadsfria sandbox-resursgruppen. |
    | Plats | Fylls i automatiskt när **Använd befintlig** har angetts. | Välj den geografiska plats som ska vara värd för ditt Azure Cosmos DB-konto. Använd den plats som finns närmast dina användare så att de får så snabb åtkomst till data som möjligt. I den här övningen bestäms platsen i förväg för oss som platsen för den befintliga resursgruppen.|
    
    Lämna andra fält på bladet **Nytt konto** på standardnivåvärdena eftersom vi använder dem i den här modulen.  Detta omfattar **Aktivera georedundans**, **Aktivera multimaster**, **Virtuella nätverk**.

1. Välj **Skapa** för att etablera och distribuera databaskontot.

1. Distributionen kan ta lite tid. Så vänta tills ett **Distributionen lyckades**-meddelande visas i meddelandehubben innan du fortsätter.

    ![Meddelande om att databaskontots distribution har slutförts](../media/5-db-deploy-success.PNG)

1. Välj **Gå till resurs** och navigera till databaskontot i portalen. Vi ska lägga till en samling i databasen sedan.

### <a name="add-a-collection"></a>Lägga till en samling

I Cosmos DB innehåller en *container* godtyckliga användargenererade entiteter. För SQL- och MongoDB API-konton mappas en container till en *samling*. Vi lagrar dokument i en samling.

Låt oss skapa en databas och en samling med hjälp av Datautforskaren i Azure Portal.

1. Välj **Datautforskaren** > **Ny samling**.

2. Ange inställningarna för den nya samlingen under **Lägg till samling**.

    >[!TIP]
    >Området **Lägg till samling** visas längst till höger. Du kan behöva rulla till höger för att se den.

    |Inställning|Föreslaget värde|Beskrivning
    |---|---|---|
    |Databas-ID|[!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]| Databasnamn måste innehålla mellan 1 och 255 tecken och får inte innehålla /, \\, #, ? eller avslutande blanksteg.<br><br>Du kan välja att ange vad du vill här, men vi rekommenderar [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)] som namn på den nya databasen och så kommer vi att kalla den i den här övningen. |
    |Samlings-ID|[!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)]|Ange [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] som namnet på din nya samling. Samma teckenkrav gäller för samlings-ID:n som för databasnamn.|
    |Lagringskapacitet| Fast (10 GB)|Använd standardvärdet **Fast (10 GB)**. Det här värdet är databasens lagringskapacitet.|
    |Dataflöde|1 000 RU|Ändra genomflödet till 1 000 begäransenheter per sekund (RU/s). Lagringskapaciteten måste anges till **Fast (10 GB)** för att det ska gå att ställa in dataflöde på 400 RU/s. Du kan skala upp prestanda senare om du vill minska svarstiden.|
        
3. Klicka på **OK**. Datautforskaren visar den nya databasen och samlingen. Så, nu har vi en databas. Vi har definierat en samling inne i databasen. Därefter lägger vi till vissa data, även kallat dokument.

### <a name="add-test-data"></a>Lägga till testdata

Vi har definierat en samling i databasen som kallas [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)]. Vi vill lagra en URL och ett ID i varje dokument, t.ex. en lista över webbsidesbokmärken. 

Du lägger nu till data i din nya samling med hjälp av Datautforskaren.

1. Den nya databasen visas på panelen Samlingar i Datautforskaren. Expandera databasen [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)], expandera samlingen [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)], välj **Dokument** och sedan **Nytt dokument**.
  
2. Ersätt standardinnehållet i det nya dokumentet med följande JSON.

     ```json
     {
         "id": "docs",
         "URL": "https://docs.microsoft.com/azure"
     }
     ```

3. När du har lagt till JSON på fliken **Dokument** väljer du **Spara**.

    Observera att det finns fler egenskaper än de som vi har lagt till. Alla börjar med ett understreck (_rid, _self, _etag, _attachments, _ts). Det här är egenskaper som genereras av systemet för att underlätta hanteringen av dokumentet.

    |Egenskap  |Beskrivning  |
    |---------|---------|
    | `_rid`     |     Resurs-ID:t är en unik identifierare som också är hierarkisk per resursstacken på resursmodellen. Den används internt för placering och navigering i dokumentresursen.    |
    | `_self`     |   Den unika adresserbara URI:n för resursen.      |
    | `_etag`     |   Krävs för optimistisk samtidighetskontroll.     |
    | `_attachments`     |  Den adresserbara sökvägen till resursen för bifogade filer.       |
    | `_ts`     |    Tidsstämpeln för den senaste uppdateringen av den här resursen.    |
     
4. Nu ska vi lägga till några fler dokument i samlingen. Skapa ytterligare fyra dokument med följande innehåll. Kom ihåg att spara ditt arbete.

    ```json
    {
        "id": "portal",
        "URL": "https://portal.azure.com"
    }
    ```
    
    ```json
    {
        "id": "learn",
        "URL": "https://docs.microsoft.com/learn"
    }
    ```
    
    ```json
    {
        "id": "marketplace",
        "URL": "https://azuremarketplace.microsoft.com/marketplace/apps"
    }
    ```
    
    ```json
    {
        "id": "blog",
        "URL": "https://azure.microsoft.com/blog"
    }
    ```
    
1. När du är klar bör samlingen se ut som följande:

    ![SQL API-användargränssnittet i portalen visar en lista över poster som du har lagt till i din bokmärkessamling](../media/5-db-bookmark-coll.PNG)
    
Nu har du ett antal poster i din bokmärkessamling. Så här fungerar vårt scenario. Om en begäran tas emot med, t.ex. ”id = docs”, söker du upp detta ID i din bokmärkessamling och returnerar URL:en `https://docs.microsoft.com/azure`. Vi ska skapa en Azure-funktion som söker upp värden i den här samlingen.

## <a name="create-your-function"></a>Skapa din funktion

1. Gå till den funktionsapp som du skapade i föregående enhet.

1. Expandera funktionsappen, hovra sedan över samlingen med funktioner och välj sedan knappen **Lägg till** (**+**) bredvid **Funktioner**.  
   Den här åtgärden startar funktionsskapandeprocessen. Följande animering illustrerar åtgärden:

   ![Animering av det plustecken som visas när du hovrar över funktionsmenyalternativet](../media/3-func-app-plus-hover-small.gif)

   På sidan visas en fullständig uppsättning med utlösare som stöds. 
   
1. Välj **HTTP-utlösare**, vilket är den första posten i följande skärmbild:

    ![Skärmbild som visar en del av utlösarmallens valgränssnitt, där TTP-utlösaren visas först, i den övre vänstra delen av bilden.](../media/5-trigger-templates-small.png)

1. Fyll i dialogrutan **Ny funktion** som visas till höger med följande värden.

    | Fält | Värde |
    |----------|--------|
    | Språk | **JavaScript** |
    | Namn     | [!INCLUDE [func-name-find](./func-name-find.md)] |
    | Auktoriseringsnivå | **Funktion** |
    
1. Skapa den nya funktionen genom att välja **Skapa**.  
    Detta öppnar filen *index.js* i kodredigeraren och en standardimplementering av den HTTP-utlösta funktionen visas.

### <a name="verify-the-function"></a>Verifiera funktionen

Du kan verifiera vad vi har gjort hittills genom att testa vår nya funktion enligt följande:

1. Klicka på **Hämta funktionswebbadress** längst upp till höger i den nya funktionen och välj **Standard (funktionsnyckel)**. Klicka sedan på **Kopiera**.

1. Klistra in den funktions-URL som du kopierade i adressfältet till din webbläsare. Lägg till frågesträngsvärdet `&name=<yourname>` i slutet av den här webbadressen och kör begäran genom att trycka på **Retur**. Du bör få ett svar från Azure-funktionen direkt i webbläsaren.  

Nu när vår grundfunktion fungerar ska vi koncentrera oss på att läsa data från vår Azure Cosmos DB eller, i vårt scenario, vår [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)]-samling.

## <a name="add-an-azure-cosmos-db-input-binding"></a>Lägg till en Azure Cosmos DB-indatabindning

Du måste definiera en indatabindning om du vill kuna läsa data från databasen. Som du ser kan du konfigurera en bindning som kan kommunicera med din databas i bara några få steg.

1. Öppna integrationsfliken genom att välja **Integrera** i rutan till vänster.  
   Den mall som du använde skapade en HTTP-utlösare och en HTTP-utdatabindning. Lägg nu till din nya Azure Cosmos DB-indatabindning. 

2. Välj **Nya indata** i kolumnen **Indata**.  
   En lista över alla möjliga indatabindningstyper visas.

3. Välj **Azure Cosmos DB** i listan och välj sedan **Välj**.  
   Den här åtgärden öppnar konfigurationssidan för Azure Cosmos DB-indata. Nu ska du konfigurera en anslutning till databasen.

4. Välj **Ny** bredvid rutan **Azure Cosmos DB-kontoanslutning**.  
   Den här åtgärden öppnar fönstret **Anslutning**, som redan innehåller **Azure Cosmos DB-kontot** och din Azure-prenumeration har valts. Det enda som återstår är att välja ett databaskonto-ID.

5. I avsnittet Skapa ett databaskonto var du tvungen att ange ett ID-värde. Leta upp det här värdet i listrutan **Databaskonto** och klicka sedan på **Välj**.

6. En ny anslutning till databasen har konfigurerats och visas i fältet **Azure Cosmos DB-kontoanslutning**. Om du är nyfiken på vad som faktiskt finns bakom den här abstrakta namnet klickar du på *Visa värde*, varvid anslutningssträngen visas.

Du vill leta upp ett bokmärke med ett specifikt ID, så låt oss koppla det ID som vi tar emot till bindningen.

7. I fältet **Dokument-ID (valfritt)** anger du `{id}`. Den här syntaxen kallas för ett *bindningsuttryck*. Funktionen utlöses av en HTTP-begäran som använder en frågesträng för att ange vilket ID som ska sökas upp. Eftersom ID:n är unika i vår samling returnerar bindningen antingen 0 (hittades inte) eller 1 (hittades) dokument.

8. Fyll noggrant i de återstående fälten på den här sidan med hjälp av värdena i tabellen nedan. Du kan när som helst klicka på informationsikonen till höger om varje fältnamn för att få mer information om syftet med varje fält.

    |Inställning  |Värde  |Beskrivning  |
    |---------|---------|---------|
    |Dokumentparameternamn     |  **bokmärke**       |  Det namn som används för att identifiera den här bindningen i koden.      |
    |Databasnamn     |  [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]       | Databas att arbeta med. Det här värdet är det databasnamn som vi konfigurerade tidigare i den här lektionen.        |
    |Samlingsnamn     |  [!INCLUDE [cosmos-db-name](./cosmos-coll-name.md)]        | Den samling som vi ska läsa data från. Den här inställningen definierades tidigare i lektionen. |
    |SQL-fråga (valfritt)    |   lämna tomt       |   Vi hämtar endast ett dokument i taget baserat på ID:t. Filtrering efter fältet dokument-ID är alltså bättre än att använda en SQL-fråga i den här instansen. Vi kan skapa en SQL-fråga för att returnera en post (`SELECT * from b where b.ID = {id}`). Frågan skulle verkligen returnera ett dokument, men den skulle returnera det i en dokumentsamling. Vår kod skulle behöva ändra en samling i onödan. Använda SQL-frågemetoden när du vill hämta flera dokument.   |
    |Partitionsnyckel (valfritt)     |   lämna tomt      |  Vi kan acceptera standardvärdena här.       |
    
9. Välj **Spara** så att alla ändringar i den här bindningskonfigurationen sparas. 

Nu när du har definierat din bindning är det dags att använda den i din funktion.

## <a name="update-function-implementation"></a>Uppdatera funktionsimplementeringen

1. Välj din funktion [!INCLUDE [func-name-find](./func-name-find.md)] så öppnas *index.js* i kodredigeraren. Du har lagt till en indatabindning att läsa från databasen, så det är dags att uppdatera logiken så att du kan använda den här bindningen.

2. Ersätt all kod i *index.js* med koden från följande kodavsnitt och välj **Spara**.

   [!code-javascript[](../code/find-bookmark-single.js)]

En inkommande HTTP-begäran utlöser funktionen, och en `id`-frågeparameter skickas till Cosmos DB-indatabindningen. Om databasen hittar ett dokument som matchar detta ID så konfigureras parametern `bookmark` för det påträffade dokumentet. I så fall kan vi skapa ett svar som innehåller det URL-värde som påträffades i det bokmärkta dokumentet. Om inget dokument som matchar den här nyckeln hittades, så svarar vi med en nyttolast och statuskod som meddelar användaren de dåliga nyheterna.

## <a name="try-it-out"></a>Prova

1. Välj **Hämta funktionswebbadress** längst upp till höger och välj **Standard (funktionsnyckel)**. Sedan kopierar du funktionens URL genom att klicka på **Kopiera**.

2. Klistra in den funktions-URL som du kopierade i adressfältet till din webbläsare. Lägg till frågesträngsvärdet `&id=docs` i slutet av den här webbadressen och genomför begäran genom att trycka på tangenten `Enter` på tangentbordet. Nu bör du se ett svar som innehåller en URL till resursen.

3. Ersätt `&id=docs` med `&id=missing` och observera svaret.

4. Ersätt den tidigare frågesträngen med `&id=` och observera svaret.

    >[!TIP]
    >Du kan även testa funktionen med hjälp av fliken **Testa** i funktionsportalens användargränssnitt. Du kan lägga till en frågeparameter eller ange en brödtext i begäran och få samma resultat som beskrivs i föregående steg.
    
I den här enheten skapade vi vår första indatabindning manuellt för att kunna läsa från en Azure Cosmos DB-databas. Den mängd kod som vi skriver för att söka i vår databas och läsa data var minimal, tack vare bindningar. Vi gjorde det mesta av vårt arbete med att konfigurera bindningen deklarativt och plattformen tog hand om resten.  

I nästa enhet lägger vi till flera data i vår bokmärkessamling via en Azure Cosmos DB-utdatabindning.