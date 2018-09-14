Så småningom måste vara värd för vår kod någonstans i molnet. Den Azure-teknik vi använder för att göra detta är Azure Functions.

I den här föreläsning du lära dig:

- Så här skapar du en Azure-Funktionsapp och `JavaScript` Http-utlösare.
- Så här att köra och felsöka en Azure-funktion lokalt.

## <a name="create-an-azure-function-project"></a>Skapa ett Azure Function-projekt

Ett Azure Function-projekt är en behållare för flera funktioner. Functions har utlösts på olika sätt; Vi kommer att utlösa våra funktioner genom att göra en HTTP-begäran.

Det finns många sätt att skapa Azure Functions; Vi ska använda Visual Studio Code med tillägget Azure Functions som vi tidigare installerade i den här modulen.

Nu ska vi först skapa vår Function-projekt.

1. Typ <kbd>Ctrl</kbd>+<kbd>P</kbd> öppna kommandopaletten, och sedan söka efter och välj `Azure Functions: Create New Project...`

   > **OBS!**
   >
   > Programmet kan begära att om du vill skriva över vissa filer som `.gitignore`, svar **inga** till alla!

   ![Skapa nytt projekt](/media-drafts/7.create-new-project.png)

2. Välj den mapp där du vill skapa funktionsappen (det första alternativet är den aktuella mappen)

   ![Välj mapp](/media-drafts/7.select-folder.png)

3. Välj `JavaScript` som önskat språk.

   ![Välj språk](/media-drafts/7.select-language.png)

4. Om ovanstående slutförts bör du se den nedan filer som skapas i mappen

   ![App som skapats](/media-drafts/7.app-created.png)

## <a name="create-an-azure-function"></a>Skapa en Azure-funktion

Sedan upp nu ska vi skapa Azure-funktionen, typer av kod som svarar på en HTTP-begäran.

Igen ska vi använda i Visual Studio Code-tillägg.

1.  Typ <kbd>Ctrl</kbd>+<kbd>P</kbd> öppna kommandopaletten, och sedan söka efter och välj `Azure Functions: Create Function...`

    ![Skapa ny funktion](/media-drafts/7.create-function.png)

2.  Välj den mapp där du skapade function-projekt tidigare (det första alternativet är den aktuella mappen)

    ![Välj mapp](/media-drafts/7.select-current-project.png)

3.  Vi skapar en _HTTP-utlösare_, så markerar du det alternativet.

    ![Välj mapp](/media-drafts/7.select-trigger.png)

4.  Skriv i `MojifyImage` som namnet på den funktion som du vill skapa

    ![Välj namn](/media-drafts/7.choose-function-name.png)

5.  Välj `Anonymous` som autentiseringsnivån

    > Obs: genom att välja `Anonymous` funktionen är öppet för världen och osäkert.

    ![Välj namn](/media-drafts/7.choose-auth-level.png)

## <a name="run-the-function-locally"></a>Kör funktionen lokalt

När dessa kommandon för att slutföra du har konverterat starter-projektet till en function-projekt med en _HTTP-utlösare_ anropade funktionen `MojifyImage`.

Att se till att allt fungerar som du kan köra appen lokalt, t.ex:

```bash
func host start
```

Detta startar funktionens körning lokalt. Om allt fungerar korrekt så småningom bör du se något som liknar den nedan ut till konsolen:

```
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

> **Obs!**
>
> Detta är en av fördelarna med att installera Azure Functions-körningen, det gör att vi kan köra funktionen lokalt med hjälp av samma underliggande teknik som används för att köra funktionen i produktion.

För att säkerställa att funktionen fungerar som den ska, finns i URL: en ut på konsolen, då skrivs i webbläsarfönstret:

![Fungerande Funktionsapp](/media-drafts/7.default-function-app-working.png)

## <a name="debug-the-function-locally"></a>Felsöka funktionen lokalt

> **Viktigt**
>
> Kontrollera att du har avslutat den `func host start` kommandot du körde innan du försöker att felsöka den med hjälp av Visual Studio Code.

Dessutom kan vi köra _och felsöka_ programmet i visual studio-kod.

Lägg till en brytpunkt till den `index.js` filen överst i JavaScript-funktion som detta:

![Lägg till brytpunkt](/media-drafts/7.add-breakpoint.png)

Att köra funktionen i debug redigeringsläge första går du till menyn debug <kbd>Cmd<kbd> + <kbd>SKIFT<kbd> + <kbd>D<kbd>.

Välj `Attach to JavaScript Functions` från debug-konfiguration innan du slutligen trycker du på den gröna triangeln att starta felsökningssessionen.

> **Obs!**
>
> Den `Attach to JavaScript Functions` debug-konfiguration har lagts till automatiskt när du skapade Function-projekt.

Du kan också trycka på <kbd>F5<kbd> från var som helst i programmet; den här lösningen körs senast debug-konfiguration.

Den `func host start` kommandot körs för dig; en terminal ska öppna med så småningom samma utdata som ovan:

```
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

Eftersom vi felsöker du också se debug menyraden, t.ex:

![Menyraden för felsökning](/media-drafts/7.debug-menu-bar.png)

Nu när du besöker URL: en ovan den bryts vid brytpunkten du angav och du kan gå igenom funktionen.

<!-- TODO Find Link -->

Du kan [mer](https://code.visualstudio.com/docs/editor/debugging) om felsökning i Visual Studio Code
