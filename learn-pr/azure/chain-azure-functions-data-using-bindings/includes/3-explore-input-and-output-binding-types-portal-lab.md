Följande är en övergripande illustration av vad vi ska skapa i den här övningen.

![Illustration av HTTP-standardutlösare, som visar HTTP-begäran och -svar samt respektive bindningsparametrar, req och res](../media/3-default-http-trigger-visual-small.PNG)

Vi skapar en funktion som startar när den får en HTTP-begäran och svarar på varje begäran genom att skicka tillbaka ett meddelande. Parametrarna `req` och `res` är utlösarbindning respektive utdatabindning.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-function-app"></a>Skapa en funktionsapp

Vi ska skapa en funktionsapp som vi sedan använder genom hela modulen. I en funktionsapp kan du gruppera funktioner som en logisk enhet så att det blir enklare att hantera, distribuera och dela resurser.

1. Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du har aktiverat sandbox-miljön med.

1. Välj knappen **Skapa en resurs** längst upp till vänster i Azure Portal och sedan **Compute** > **Funktionsapp**.

1. Ange egenskaperna för funktionsappen enligt följande:

    | Egenskap     | Föreslaget värde  | Beskrivning  |
    |--------------|------------------|--------------|
    | **Appens namn** | Globalt unikt namn | Namn som identifierar din nya funktionsapp. Giltiga tecken är `a-z`, `0-9` och `-`.  | 
    | **Prenumeration** | Din prenumeration | Prenumerationen som den nya funktionsappen skapas under. | 
    | **Resursgrupp**|  Välj **Använd befintlig** och därefter _<rgn>[Sandbox-resursgruppnamn]</rgn>_ | Namnet på resursgruppen som funktionsappen ska skapas i. | 
    | **Operativsystem** | Windows | Det operativsystem som är värd för funktionsappen.  |
    | **Värd** |   Förbrukningsplan | Värdplan som definierar hur resurser allokeras till din funktionsapp. I standardinställningen **Förbrukningsplan** läggs resurser till dynamiskt när de krävs av funktionerna. För den här typen av serverlösa värdtjänster betalar du bara för den tid som dina funktioner körs.   |
    | **Lagringskonto** |  Globalt unikt namn |  Namnet på det nya lagringskonto som ska användas av funktionsappen. Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. Den här dialogrutan fyller i fältet med ett unikt namn som härleds från namnet du gav appen. Du kan använda ett annat namn eller ett befintligt konto om du vill. |
    | **Plats** | Välj från listan | Välj en av de närmaste tillgängliga platserna som anges ovan. |
    
    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. Välj **Skapa** för att etablera och distribuera funktionsappen.

1. Välj meddelandeikonen i det övre högra hörnet av portalen och titta efter ett meddelande om att **Distribution pågår** som liknar nedanstående meddelande.

    ![Meddelande om att funktionsappens distribution pågår](../media/3-func-app-deploy-progress-small.PNG)

1. Distributionen kan ta lite tid. Stanna kvar i meddelandehubben och håll utkik efter ett meddelande om att **Distributionen är klar** som liknar följande meddelande.

    ![Meddelande om att funktionsappens distribution är slutförd](../media/3-func-app-deploy-success-small.PNG)

 1. När funktionsappen har distribuerats går du till **Alla resurser** i portalen. Appen visas i listan med typen **Apptjänst** och har det namn du gav den. Välj funktionsappen från listan för att öppna den.

    >[!TIP]
    >Om du har problem med att hitta dina funktionsappar i portalen kan du få reda på hur du [lägger till funktionsappar till dina favoriter i portalen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).
    
## <a name="create-a-function"></a>Skapa en funktion

Nu när vi har en funktionsapp är det dags att skapa en funktion. En funktion aktiveras via en utlösare. Vi använder en HTTP-utlösare i den här modulen.

1. Expandera din nya funktionsapp och hovra sedan över samlingen med funktioner och välj knappen Lägg till (**+**) bredvid **Funktioner**. Den här åtgärden startar funktionsskapandeprocessen. Följande animering illustrerar den här åtgärden.

    ![Animering av plustecknet som visas när användaren för muspekaren över menyalternativet för funktioner.](../media/3-func-app-plus-hover-small.gif)

1. På sidan **Kom igång snabbt** väljer du **Anpassad funktion** under avsnittet **Kom igång på egen hand**.

1. Då visas en lista med alla mallar. Hitta mallen **HTTP-utlösare** och välj JavaScript som språk.

    ![Skärmbild av rutan för att skapa HTTP-funktion med länk till JavaScript](../media/3-http-function.png)

1. På bladet **Ny funktion** kan du ändra namn om du vill, lämna **Auktorisationsnivån** som _Funktion_ och klicka på **Skapa**.

1. I den nya funktionen klickar du på **</> Hämta funktionswebbadress** längst upp till höger och väljer **Standard (funktionsnyckel)**. Sedan väljer du **Kopiera**.

1. Klistra in den funktions-URL som du kopierade i adressfältet på den nya fliken i din webbläsare.

1. Lägg till frågesträngsvärdet `&name=Azure` i slutet av den här webbadressen och tryck på Retur på tangentbordet för att utföra begäran. Du bör se ett svar som liknar följande svar returnerat av funktionen som visas i webbläsaren.  

    ```output
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Hello Azure</string>
    ```

Som du ser i den här övningen hittills måste du välja en utlösartyp när du skapar en funktion. Varje funktion har endast en utlösare. I det här exemplet använder vi en HTTP-utlösare, vilket betyder att vår funktion startar när den får en HTTP-begäran. Standardimplementeringen, som visas i följande skärmbild i JavaScript, svarar med värdet för ett *parameternamn* som det får i frågesträngen eller brödtexten för begäran. Om ingen sträng har angetts svarar funktionen med ett meddelande som frågar den som anropar att ange ett namnvärde.

![JavaScript-standardimplementering av en HTTP-utlöst Azure-funktion](../media/3-default-http-trigger-implementation-small.PNG)

All denna kod finns i **index.js**-filen i den här funktionens mapp. Nu ska vi titta kort på funktionens andra fil, konfigurationsfilen **function.json**. Dessa konfigurationsdata visas i följande JSON-lista.

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

Som du ser har funktionen en bindning med namnet **req** av typen `httpTrigger` och en utdatabindning med namnet **res** av typen `HTTP`. I föregående kod för vår funktion såg vi hur vi fick åtkomst till nyttolasten för den inkommande HTTP-begäran genom parametern **req**. Och på samma sätt skickade vi ett HTTP-svar bara genom att ange parametern **res**. Bindningar tar verkligen hand om en del tidskrävande jobb åt oss.

>[!TIP]
>Du kan se **index.js** och **function.json** genom att expandera menyn **Visa filer** till höger på funktionspanelen i Azure Portal.  

### <a name="explore-binding-types"></a>Utforska bindningstyper

1. Observera under funktionsposten att det finns en uppsättning menyalternativ, enligt följande skärmbild.

    ![Skärmbild som visar menyalternativ under en funktion på bladet Funktionsappar.](../media/3-func-menu-small.PNG)

1. Välj menyalternativet Integrera för att öppna integreringsfliken för vår funktion. Om du har följt med i den här kursdelen bör integreringsfliken se ut som i följande skärmbild.

    ![Skärmbild som visar gränssnittet eller fliken för integrering.](../media/3-func-integrate-tab-small.PNG)

    > [!NOTE]
    > Observera att vi redan har definierat en utlösar- och utdatabindning, enligt den här skärmbilden. Du ser även att vi inte kan lägga till mer än _en_ utlösare. Om vi vill ändra utlösare för funktionen måste vi faktiskt ta bort utlösaren och sedan skapa en ny. Men områdena **Indata** och **Utdata** i det här gränssnittet visar ett plustecken (+) för att lägga till flera bindningar så att vi kan acceptera fler än ett indatavärde och skapa fler än ett utdatavärde.

1. Välj **+ Nya indata** i kolumnen **Indata**. En lista över möjliga indatabindningstyper visas enligt följande skärmbild.

    ![Skärmbild som visar listan över möjliga indatabindningar.](../media/3-func-input-bindings-selector-small.PNG)

   Ta en stund och tänk över var och en av dessa indatabindningar och hur du kan använda dem i en lösning. Det finns många att välja mellan. Den här listan kan till och med ha ändrats när du läser den här modulen, när vi fortsätter att stödja fler datakällor.

1. Vi ska gå tillbaka till att lägga till indatabindningar senare i modulen men för tillfället väljer vi **Avbryt** att stänga den här listan.

1. Välj **+ Nya utdata** i kolumnen **Utdata**. En lista över möjliga utdatabindningstyper visas enligt följande skärmbild.\

    ![Skärmbild som visar listan över möjliga utdatabindningar.](../media/3-func-output-bindings-selector-small.PNG)

   Som du ser har du flera typer av utdatabindningar till ditt förfogande. Vi ska gå tillbaka till att lägga till indatabindningar senare i modulen men för tillfället väljer vi **Avbryt** för att stänga den här listan.

Hittills har vi gått igenom hur du skapar en funktionsapp och hur du lägger till en funktion i den. Vi har sett en enkel funktion i en app som körs när en HTTP-begäran skickas till den. Vi har också utforskat Azure-portalgränssnittet och de typer av indata- och utdatabindningar som är tillgängliga för våra funktioner. I nästa kursdel använder vi en indatabindning för att läsa text från en databas.