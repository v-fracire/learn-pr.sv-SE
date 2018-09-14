Anta att vi vill skapa en enkel bokmärke lookup-tjänst. Tjänsten är readonly från början. Om en användare vill hitta en post, de skickar en begäran med ID: T för posten och returnerar vi URL: en. Diagrammet nedan beskriver flödet.

![Flow-diagram som visar processen att hitta ett bokmärke i vår Cosmos-DB backend-server](../media-draft/find-bookmark-flow-small.png)

När en användare skickar oss en begäran med lite text, försöker vi hitta en post i vår backend-databas som innehåller den här texten som en nyckel eller -Id. Vi returnera ett resultat som anger om vi hittas posten eller inte.

Vi behöver att lagra data någonstans. I det här flödesschemat visas vår datalager som ett Azure Cosmos DB. Men hur vi ansluta till databasen från en funktion och läsa data? I en värld av funktioner, konfigurerar vi en *indatabindning* för jobbet.  Det är enkelt att konfigurera en bindning via Azure Portal. Som vi ser inom kort har du inte skriva kod för uppgifter, till exempel att öppna en lagringsanslutning. Azure Functions runtime och bindningen hand tar om dessa uppgifter åt dig.

> [!IMPORTANT]
> Vi kommer att referera till vissa resurser (databasen, funktion, bindningar, kod) som vi skapar här i vårt nästa labb. Håll resurserna runt minst tills du är klar med den här kursen.

Som var fallet för den föregående modulen ska vi göra allt i Azure-portalen.

## <a name="create-an-azure-cosmos-db"></a>Skapa ett Azure Cosmos DB 

### <a name="create-a-database-account"></a>Skapa ett databaskonto

Ett databaskonto är en behållare för att hantera en eller flera databaser. Innan vi kan skapa en databas, som vi behöver skapa ett databaskonto.

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

2. Välj den **skapa en resurs** knappen hittades på det övre vänstra hörnet i Azure-portalen, sedan väljer **databaser** > **Azure Cosmos DB**.

3. På sidan **Nytt konto** anger du inställningarna för det nya Azure Cosmos DB-kontot.

    Inställning|Värde|Beskrivning
    ---|---|---
    ID|*Ange ett unikt namn*|Ange ett unikt namn som identifierar Azure Cosmos DB-kontot. Eftersom *documents.azure.com* läggs till det ID du anger för att skapa din URI ska du använda ett unikt men identifierbart ID.<br><br>Ditt ID får bara innehålla gemener, siffror och bindestreck och måste innehålla mellan 3 och 50 tecken.
    API|SQL|API:n avgör vilken typ av konto som skapas. Azure Cosmos DB innehåller fem API: er så att de passar behoven i ditt program: SQL (dokumentdatabas), Gremlin (grafdatabas), MongoDB (dokumentdatabas), Azure Table och Cassandra, där var och en för närvarande kräver ett separat konto. <br><br>Välj **SQL**. Just nu fungerar Azure Cosmos DB-utlösaren, indatabindningar och utdatabindningar bara med SQL API och Graph API-konton. 
    Prenumeration|*Din prenumeration*|Välj den Azure-prenumeration som ska användas för det här Azure Cosmos DB-kontot.
    Resursgrupp|Använd befintlig<br><br>*Välj sedan <rgn>[Sandbox resursgruppens namn]</rgn>, resursgruppen som vi skapade i en tidigare enhet för den här modulen resurser.*| Väljer vi **Använd befintlig**, eftersom vi vill gruppera alla resurser som skapats för den här modulen under samma resursgrupp.
    Plats|Fylls i automatiskt en gång **Använd befintlig** har angetts. | Välj den geografiska plats som ska vara värd för ditt Azure Cosmos DB-konto. Använd den plats som är närmast dina användare så att de får så snabb åtkomst till data som möjligt. I den här övningen bestäms före platsen för oss som platsen för den befintliga resursgruppen.

Lämna andra fält i den **nytt konto** bladet på standardnivå värden eftersom vi använder dem i den här modulen.  Det omfattar **aktivera georedundans**, **aktivera flera Master**, **virtuella nätverk**.

4. Välj **skapa** att etablera och distribuera databaskontot.

5. Distributionen kan ta lite tid. Så vänta tills en **distributionen lyckades** meddelande som liknar följande meddelande innan du fortsätter.

<!-- TODO figure out how to center these image -->

![Meddelande om att distributionen av databasen har slutförts](../media-draft/db-deploy-success.PNG)

6. Grattis! Du har skapat och distribuerat ditt databaskonto!

7. Välj **gå till resurs** att navigera till kontot i portalen. Vi ska lägga till en samling databasen sedan.

### <a name="add-a-collection"></a>Lägga till en samling

I Cosmos DB, en *behållare* innehåller godtyckliga användargenererade entiteter. I en databas har stöd för SQL-API, en dokumentorienterad API, en behållare är en *samling*. Vi lagrar dokument i en samling. Förhoppningsvis blir alla tydligare när vi skapar en samling och lägger till några dokument.

Nu ska vi använda datautforskarverktyget i Azure-portalen för att skapa en databas och samling.

1. Klicka på **Datautforskaren** > **Ny samling**.

2. I den **Lägg till samling**, anger du inställningarna för den nya samlingen.

    >[!TIP]
    >Området **Lägg till samling** visas längst till höger, du kan behöva bläddra åt höger för att se det.

    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    Databas-id|[!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]| Databasnamn måste innehålla mellan 1 och 255 tecken och får inte innehålla /, \\, #, ? eller avslutande blanksteg.<br><br>Du kan välja att ange vad du vill här, men vi rekommenderar [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)] som namn på den nya databasen och som är vad vi ska referera till i den här enheten. |
    Samlings-ID|[!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)]|Ange [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] som namn på vår nya samlingen. Samlings-ID har samma teckenkrav gäller som databasnamn.
    Lagringskapacitet| Fast (10 GB)|Använd standardvärdet för **Fast (10 GB)**. Det här värdet är databasens lagringskapacitet.
    Dataflöde|400 RU|Ändra genomflödet till 400 begäransenheter per sekund (RU/s). Lagringskapaciteten måste anges till **Fast (10 GB)** för att kunna ställa in dataflöde på 400 RU/s. Du kan skala upp dataflödet senare om du vill minska svarstiden.
    
3. Klicka på **OK**. Datautforskaren visar den nya databasen och samlingen. Så nu har vi en databas. Vi har definierat en samling inne i databasen. Därefter lägger vi till vissa data, även känt som dokument.

### <a name="add-test-data"></a>Lägg till testdata

Vi har definierat en samling i vår databas som heter [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)], så det vi tänkt lagra i samlingen? Dessutom är tanken att lagra en URL och -ID i varje dokument, t.ex. en lista över webbsida bokmärken. 

Vi lägger till data till vår nya samling med Datautforskaren.

1. Den nya databasen visas på panelen Samlingar i datautforskaren. Expandera den [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)] databasen, expandera den [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] samling, klickar du på **dokument**, och klicka sedan på **nytt dokument**.
  
2. Lägg nu till ett dokument i samlingen med följande struktur. Varje dokument är-schemalösa JSON-fil.

     ```json
     {
         "id": "docs",
         "URL": "https://docs.microsoft.com/azure"
     }
     ```

3. När du har lagt till json på fliken **Dokument** klickar du på **Spara**.

När dokumentet sparas, Observera att det finns fler egenskaper än de som vi har lagt till. Alla börjar med ett understreck (_rid, _self, _etag, _attachments, _ts). Det här är egenskaper som genereras av systemet för att hantera dokumentet. I följande tabell beskrivs kortfattat de är.

|Egenskap  |Beskrivning  |
|---------|---------|
|_rid     |     Resurs-ID (_rid) är en unik identifierare som också är hierarkiska per resurs-stacken på vilken resursmodell. Den används internt för placering och navigeringen i Dokumentresursen.    |
|_self     |   Den unika adresserbara URI för resursen.      |
|_etag     |   Krävs för optimistisk samtidighetskontroll.     |
|_attachments     |  Adresserbara sökväg för resurs för bifogade filer.       |
|_ts     |    Tidsstämpel för senaste uppdateringen av den här resursen.    |
 
4. Vi lägger till några dokument i vår samling. För var och en av följande använder den **nytt dokument** kommandot igen för att skapa en post för varje. Glöm inte att klicka på # Save ** för att samla in dina tillägg.

|id  |value (värde)  |
|---------|---------|
|portal     |  https://portal.azure.com       |
|Lär dig     |   https://docs.microsoft.com/learn |
|marketplace     |    https://azuremarketplace.microsoft.com/marketplace/apps  |
|blog | https://azure.microsoft.com/blog |

När du är klar, din samling bör se ut som följande skärmbild.

![Skärmbild av SQL API-Användargränssnittet i portal som visar en lista över poster som vi har lagt till vår bokmärken-samlingen.](../media-draft/db-bookmark-coll.PNG)

Nu har vi några poster i vår bokmärke-samlingen. Så här fungerar vårt scenario. Om en begäran tas emot med, till exempel ”id = docs”, vi Leta upp detta ID i vår bokmärken samlingen och returnerar URL: en https://docs.microsoft.com/azure. Vi gör en Azure-funktion som hämtar värden i den här samlingen.

## <a name="create-our-function"></a>Skapa vår funktion

1. Gå till funktionsappen som du skapade i den föregående enheten.

2. Expandera funktionsappen, och sedan hovra över samlingen funktioner och välj Lägg till (**+**) bredvid knappen **Functions**. Den här åtgärden startar funktionen skapandeprocessen. Följande animeringen illustrerar den här åtgärden.

![Animering av på plustecknet som visas när användaren för muspekaren över menyalternativet funktioner.](../media-draft/func-app-plus-hover-small.gif)

3. Sidan visar oss en fullständig uppsättning med stöds utlösare. Välj **HTTP-utlösare**, vilket är den första posten i följande skärmbild.

![Skärmbild av en del av utlösaren mall markeringen Användargränssnittet med HTTP-utlösaren visas först i upp till vänster i avbildningen.](../media-draft/trigger-templates-small.PNG)

4. Fyll i **ny funktion** dialogrutan som visas till höger med hjälp av följande värden.

|Fält  |Värde  |
|---------|---------|
|Språk     | **JavaScript**        |
|Namn     |   [!INCLUDE [func-name-find](./func-name-find.md)]     |
| Auktoriseringsnivå | **Funktionen** |

5. Välj **skapa** att skapa vår funktion som öppnas filen index.js i kodredigeraren och visar en standardimplementering av HTTP-utlöst funktion.

Du kan verifiera vad vi har gjort hittills genom att testa vår nya funktionen enligt följande:

1. I den nya funktionen klickar du på **</> Hämta funktionswebbadress** längst upp till höger och väljer **Standard (funktionsnyckel)**. Sedan klickar du på **Kopiera**.

2. Klistra in funktionens URL som du kopierade i webbläsarens adressfält. Lägg till frågesträngvärdet `&name=<yourname>` i slutet av den här webbadressen och tryck på knappen `Enter` på tangentbordet för att utföra begäran. Du bör se ett svar som liknar följande svar returnerades av funktionen visas i webbläsaren.  

Nu när vi har vår utan ben funktionen arbetar vi med adressfälten läsningen av data från våra Azure Cosmos DB eller i vårt scenario, vår [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] samling.

## <a name="add-a-cosmos-db-input-binding"></a>Lägg till en Cosmos DB-indatabindning

Vi vill läsa data från databasen som vi har skapat, så ange indatabindningar. Som du ser Konfigurera vi en bindning som kan kommunicera med vår databas med några få steg.

1. Välj **integrera** i funktionen menyn till vänster och öppna fliken integration.

Vi använde mallen skapats en HTTP-utlösare och en HTTP-utdatabindning för oss. Vi lägger till vår nya Azure Cosmos DB-indatabindning. 

2. Välj **+ ny indata** under den **indata** kolumn. En lista över alla typer av möjliga indatabindning visas.

3. Klicka på **Azure Cosmos DB** i listan och sedan den **Välj** knappen. Den här åtgärden öppnar konfigurationssidan för Azure Cosmos DB indata.

Nu ska ange vi en anslutning till databasen.

4. I fältet med namnet **Azure Cosmos DB-kontoanslutning** på den här sidan, klickar du på *nya* till höger om fältet tomt. Den här åtgärden öppnar den **anslutning** öppnas där du har redan **Azure Cosmos DB-konto** och Azure-prenumerationen har valt. Det enda som återstår är att välja ett databas-id för kontot.

5. I avsnittet **skapa ett databaskonto**, var du tvungen att ange ett ID-värde. Nu hitta detta värde i den *databaskonto* listrutan och klicka sedan på **Välj**.

En ny anslutning till databasen har konfigurerats och visas i den **Azure Cosmos DB-kontoanslutning** fält. Om du är nyfiken på det som faktiskt finns bakom den här abstrakta namn, klicka bara på *Visa värde* att visa anslutningssträngen.

Vi vill leta upp ett bokmärke med ett specifikt ID, så vi koppla det ID som vi tar emot till bindningen.

7. I den **dokument-ID (valfritt)** anger `{id}`. Detta kallas en *bindning uttryck*. Funktionen utlöses av en HTTP-begäran som använder en frågesträng för att ange ID för att leta upp. Eftersom ID: N är unika i vår samlingen, returnerar bindningen 0 (hittades inte) eller 1 (hittade) dokument.

8. Fyll noggrant återstående fält på den här sidan med hjälp av värdena i tabellen nedan. Du kan klicka på informationsikonen till höger om varje fältnamn mer information om syftet med varje fält när som helst.

|Inställning  |Värde  |Beskrivning  |
|---------|---------|---------|
|Dokumentparameternamn     |  **Bokmärke**       |  Namnet används för att identifiera den här bindningen i koden.      |
|Databasnamn     |  [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]       | Databasen där data ska läsas. Det här är namnet på databasen som vi angav tidigare i den här lektionen.        |
|Samlingsnamn     |  [!INCLUDE [cosmos-db-name](./cosmos-coll-name.md)]        | Samlingen som vi ska läsa data. Den här inställningen definierades tidigare i lektionen. |
|SQL-fråga (valfritt)    |   Lämna tomt       |   Vi endast hämtar ett dokument i taget baseras på ID. Så, filtrering med dokument-ID-fält är bättre än att använda en SQL-fråga i den här instansen. Vi kan skapa en SQL-fråga för att returnera en post (`SELECT * from b where b.ID = {id}`). Frågan verkligen returneras ett dokument, men den skulle gå tillbaka den i en dokumentsamling. Vår kod skulle behöva ändra en samling i onödan. Använda SQL-fråga-metoden när du vill hämta flera dokument.   |
|Partitionsnyckel (valfritt)     |   Lämna tomt      |  Vi kan acceptera standardinställningarna här.       |

9. Klicka på **spara** att spara alla ändringar i den här bindningskonfigurationen. Nu när vi har vår bindningen som har definierats, är det dags att använda den i vår funktion.

## <a name="update-function-implementation"></a>Uppdatera funktionen implementering

1. Klicka på vår funktion [!INCLUDE [func-name-find](./func-name-find.md)], så att du öppnar *index.js* i kodredigeraren. Vi har lagt till en indatabindning att läsa från vår databas, så Låt oss uppdatera logik för att använda den här bindningen.

2. Ersätt all kod i index.js med koden från följande kodavsnitt.

[!code-javascript[](../code/find-bookmark-single.js)]

När en HTTP-begäran gör våra funktionen kan utlösa, den `id` Frågeparametern skickas till vår Cosmos DB-indatabindning. Om det finns ett dokument som matchar detta ID kommer `bookmark` parametern anges till den. I så fall kan skapa vi ett svar som innehåller URL-värdet finns i dokumentet bokmärke. Om inga dokument hittades som matchar den här nyckeln, som vi svara med en nyttolast och statuskod som meddelar användaren dåliga nyheter.

## <a name="try-it-out"></a>Prova

1. Som vanligt, klickar du på **<> / hämta Funktionswebbadress** längst upp till höger, Välj **standard (funktionsnyckel)**, och klicka sedan på **kopia** att kopiera funktionen är URL: en.

2. Klistra in funktionens URL som du kopierade i webbläsarens adressfält. Lägg till frågesträngvärdet `&id=docs` i slutet av den här webbadressen och tryck på knappen `Enter` på tangentbordet för att utföra begäran. Alla ska, bör du se ett svar som innehåller en URL till den här resursen.

3. Ersätt `&id=docs` med `&id=missing` och notera svaret.

4. Ersätt tidigare frågesträngen med `&id=` och notera svaret.

>[!TIP]
>Du kan också testa funktionen med hjälp av den **testa** flik i funktionen portalens användargränssnitt. Du kan lägga till en frågeparameter eller ange bara en brödtext i begäran för att få samma resultat som beskrivs i föregående steg te.

I den här enheten skapade vi vår första indatabindning manuellt för att läsa från ett Azure Cosmos DB-databasen. Mängden kod som vi nämnt söka i vår databas och läsa data har minimal, tack vare bindningar. Vi gjorde de flesta av vårt arbete som konfigurerar bindningen deklarativt och plattformen tog hand om resten.  

I nästa enhet lägger vi till mer data till vår bokmärke samling via ett Azure Cosmos DB-utdatabindning.

> [!TIP]
> Den här enheten är inte avsedd att vara en självstudiekurs om Azure Cosmos DB. Om du vill fördjupa dig är här några resurser som hjälper dig att komma igång:
>
>* [Introduktion till Azure Cosmos DB: SQL-API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)
>
>* [En teknisk översikt över Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)
>
>* [Dokumentation om Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)