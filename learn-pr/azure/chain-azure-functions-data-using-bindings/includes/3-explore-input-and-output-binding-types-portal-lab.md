Följande är en övergripande bild av vad vi ska skapa i den här övningen.

![Visuell representation av standard HTTP-utlösare, som visar HTTP-begäran och svar samt respektive req och res bindande parametrar.](../media-draft/default-http-trigger-visual-small.PNG)

Vi ska skapa en funktion som startar när den får en HTTP-begäran och svarar på varje begäran genom att skicka tillbaka ett meddelande. Parametrarna `req` och `res` är utlösaren bindningen och utdata bindning respektive. Dags att sätta igång!

### <a name="create-a-function-app"></a>Skapa en funktionsapp

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Nu ska vi skapa en funktionsapp som vi använder under hela den här hela modulen. En funktionsapp kan du gruppera funktioner som en logisk enhet för enklare hantering, distribution och dela resurser.

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).
1. Välj den **skapa en resurs** knappen hittades på det övre vänstra hörnet i Azure-portalen, sedan väljer **Compute** > **Funktionsapp**.
1. Ange funktionen egenskaper enligt följande:

    | Egenskap      | Föreslaget värde  | Beskrivning                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Appens namn** | Globalt unikt namn | Namn som identifierar din nya funktionsapp. Giltiga tecken är `a-z`, `0-9` och `-`.  | 
    | **Prenumeration** | Din prenumeration | Prenumerationen som den nya funktionsappen skapas under. | 
    | **Resursgrupp**|  Välj **Använd befintlig** och välj <rgn>[Sandbox resursgruppens namn]</rgn> | Namnet på resursgruppen som funktionsappen ska skapas i. | 
    | **OS** | Windows | Det operativsystem som är värd för funktionsappen.  |
    | **Som är värd för** |   Förbrukningsplan | Värdplan som definierar hur resurser allokeras till din funktionsapp. I **standardförbrukningsplanen** läggs resurser till dynamiskt när de behövs i funktionerna. För den här typen av [serverlösa](https://azure.microsoft.com/overview/serverless-computing/) värdtjänster betalar du bara för den tid som dina funktioner körs.   |
    | **Plats** | Välj listan | Välj en [plats](https://azure.microsoft.com/regions/) nära dig eller nära andra tjänster som kommer att användas i dina funktioner. |
    | **Lagringskonto** |  Globalt unikt namn |  Namnet på det nya lagringskonto som ska användas av funktionsappen. Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. Den här dialogrutan fylls fältet med ett unikt namn som härleds från namnet du gav appen. Dock passa på att använda ett annat namn eller även ett befintligt konto. |


3. Välj **Skapa** för att etablera och distribuera funktionsappen.

4. Välj meddelandeikonen i det övre högra hörnet i portalen och titta efter en **distribution pågår** meddelande som liknar följande meddelande.

![Meddelande om att funktionen app-distribution pågår](../media-draft/func-app-deploy-progress-small.PNG)

5. Distributionen kan ta lite tid. Så Håll dig i meddelandehubben och håll utkik efter en **distributionen lyckades** meddelande som liknar följande meddelande.

![Meddelande om att funktionen app-distribution har slutförts](../media-draft/func-app-deploy-success-small.PNG)

6. Grattis! Du har skapat och distribuerat din funktionsapp. Välj **Gå till resurs** att visa den nya funktionsappen.

>[!TIP]
>Om du har problem med att hitta dina funktionsappar i portalen, Lär dig hur du [lägga till Funktionsappar i dina Favoriter på portalen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

### <a name="create-a-function"></a>Skapa en funktion

Nu när vi har en funktionsapp är det dags att skapa en funktion. En funktion aktiveras via en utlösare. I den här modulen använder vi en HTTP-utlösare.

1. Expandera din nya funktionsapp och sedan hovra över samlingen funktioner och välj Lägg till (**+**) bredvid knappen **Functions**. Den här åtgärden startar funktionen skapandeprocessen. Följande animeringen illustrerar den här åtgärden.

![Animering av på plustecknet som visas när användaren för muspekaren över menyalternativet funktioner.](../media-draft/func-app-plus-hover-small.gif)

2. I den **Kom igång snabbt** väljer **WebHook + API**, Välj ett språk för funktionen och klicka på **skapa den här funktionen**.

3. En funktion skapas i ditt valda språk med hjälp av mallen för en HTTP-utlöst funktion. I den här övningen ska skapa vi en JavaScript-funktion.

### <a name="try-it-out"></a>Prova

Nu ska vi testa vad vi har hittills genom att göra följande:

1. I den nya funktionen klickar du på **</> Hämta funktionswebbadress** längst upp till höger och väljer **Standard (funktionsnyckel)**. Sedan klickar du på **Kopiera**.

2. Klistra in funktionens URL som du kopierade i webbläsarens adressfält. Lägg till frågesträngvärdet `&name=<yourname>` i slutet av den här webbadressen och tryck på knappen `Enter` på tangentbordet för att utföra begäran. Du bör se ett svar som liknar följande svar returnerades av funktionen visas i webbläsaren.  

Bra jobbat! Du har nu lagt till en HTTP-utlöst funktion till din funktionsapp och testats för att kontrollera att den fungerar som förväntat!

![Skärmbild av svarsmeddelandet av ett genomfört anrop till vår funktion.](../media-draft/default-http-trigger-response-small.PNG)

Du måste välja en Utlösartyp när du skapar en funktion som du ser i den här övningen hittills. Varje funktion har en och endast en utlösare. I det här exemplet använder vi en HTTP-utlösare, vilket innebär att vår funktion startar när den får en HTTP-förfrågan. Standardimplementeringen som visas i följande skärmbild i JavaScript, svarar med värdet för en parameter *namn* it som tas emot i frågesträngen eller brödtexten i begäran. Om ingen sträng har angetts, svarar funktionen med ett meddelande som ber den ringer för att ange ett namnvärde.

![Skärmbild av JavaScript standardimplementering av en Azure HTTP-utlöst funktion.](../media-draft/default-http-trigger-implementation-small.PNG)

All den här koden är i den *index.js* fil i mappen för den här funktionen. Nu ska vi titta kort på funktionen användarens andra filer i *function.json* konfigurationsfilen. Den här konfigurationsdata visas i följande JSON-lista.

```json
{
  "disabled": false,
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
    }
  ]
}
```

Som du ser den här funktionen har en utlösare-bindning med namnet **req** av typen `httpTrigger` och en utdatabindning som heter **res** av typen `HTTP`. I föregående kod för vår funktion, som vi såg hur vi nås nyttolasten för inkommande HTTP-begäran via vår **req** parametern. På samma sätt kan vi har skickat ett HTTP-svar genom att ange vår **res** parametern. Bindningar verkligen ta hand om några av tunga jobb för oss!

>[!TIP]
>Du kan se index.js och function.json genom att expandera den **visa filer** menyn till höger på panelen funktion i Azure-portalen.  

### <a name="explore-binding-types"></a>Utforska bindningstyper

1. Observera under registerposten funktionen det finns en uppsättning menyobjekt som visas i följande skärmbild.

![Skärmbild som visar menyalternativ under en funktion i bladet Funktionsappar.](../media-draft/func-menu-small.PNG)

2. Välj menyalternativet integrera för att öppna fliken integration för vår funktion. Om du har följt tillsammans med den här enheten, bör fliken integrera likna mycket på följande skärmbild.

![Skärmbild som visar integrera Användargränssnittet eller flik.](../media-draft/func-integrate-tab-small.PNG)

Observera att vi har redan definierats en utlösare och en utdatabindning som visas i den här skärmbilden. Du kan också se att vi inte kan lägga till mer än en utlösare. Om du vill ändra en utlösare för vår funktionen skulle vi i själva verket har först ta bort utlösaren och skapa en ny.

Å andra sidan i **indata** och **utdata** avsnitt i det här formuläret visar en plus `+` logga för att lägga till flera bindningar.

3. Välj **+ ny indata** under den **indata** kolumn. En lista över alla typer av möjliga indatabindning visas enligt följande skärmbild.

![Skärmbild som visar listan över möjliga indatabindningar.](../media-draft/func-input-bindings-selector-small.PNG)

Ta en stund att tänka på var och en av dessa indatabindningar och hur du kan använda dem i en lösning. Det finns många att välja bland. Den här listan kan även ha ändrats av när du läser den här modulen när vi fortsätter att stödja fler datakällor.

4. Välj **Avbryt** att stänga den här listan.

5. Välj **+ nya utdata** under den **utdata** kolumn. En lista över alla typer av eventuella utdata-bindning visas enligt följande skärmbild.

![Skärmbild som visar listan över möjliga utdatabindningar.](../media-draft/func-output-bindings-selector-small.PNG)

Igen, du har många olika alternativ här, som visas av behovet av att en rullningslist till höger i den här skärmbilden.

>[!TIP]
>Om du vill veta mer om de bindningar som stöds, Kolla in den [listan över stöds bindningar](https://docs.microsoft.com/azure/azure-functions/functions-versions) i Azure Functions-dokumentationen.

Hittills har vi lärt dig hur du skapar en funktionsapp och lägga till en funktion i den. Vi har sett en enkel funktion i åtgärd som körs när en HTTP-begäran skickas till den. Vi har också utforskat av portalens användargränssnitt och typer av indata och utdata-bindning som är tillgängliga för våra funktioner. I nästa enhet använder vi en indatabindning för att läsa text från en en-databas.