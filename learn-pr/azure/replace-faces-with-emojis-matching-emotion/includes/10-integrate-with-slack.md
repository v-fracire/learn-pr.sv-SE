Vi har skapat alla nödvändiga Azure functions och vi kan köra dem lokalt.

I den här lektionen vi Anslut våra Azure Functions till Slack och skapa en Slack _snedstreck_ kommando som anropar våra Azure-funktion och visar den resulterande mojified bilden i fönstret slack.

I slutet av den här föreläsning lär du dig:

- Hur du skapar en Slack-app och ansluter ett snedstreck-kommando till en Azure Function-slutpunkt.
- Hur du testar snedstreck kommandot lokalt, utan att distribuera till molnet.

## <a name="using-ngrok-to-develop-with-slash-commands"></a>Med hjälp av ngrok för att utveckla med snedstreck kommandon

Vår Azure functions är för närvarande endast som körs lokalt. slack körs dock i molnet, på internet.

När vi ställer in en slack _snedstreck_ kommandot i nästa avsnitt, det kommer att be om en offentlig URL som anropas när någon kör ett kommando med snedstreck.

Vi vill ge dem vår `RespondToSlackCommand` URL, men som endast finns på den lokala datorn.

Hur kan du ge tjänster i molnåtkomst till resurser på din lokala dator för utveckling?

En bra lösning i den här conundrum är `ngrok` det här är ett verktyg som skapar säkra tunnlar till den lokala datorn från internet.

1. Gå till https://ngrok.com/download
2. Följ instruktionerna för att ladda ned och installera den `ngrok` paketet till den lokala datorn.
3. Kör `ngrok http 7071` i en terminal.
   ![ngrok](/media-drafts/9.ngrok.png)
4. Detta skriver ut en offentlig URL som slutar på `ngrok.io` t.ex. `http://bbade778.ngrok.io`, Skriv ned det du behöver den i nästa avsnitt.

> **Obs!**
>
> Detta `http://*.ngrock.io` URL som du kan använda samma sätt som du använde `http://localhost:7071`, prova med en avbildning som det. `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`

## <a name="create-a-slack-app"></a>Skapa en slack-app

Ett snedstreck kommando finns som en del av en slack-app, en slack _bot_.

Besök [skapa Slack-App](https://api.slack.com/apps/new)

Välj en `App Name` och associera det med den `Workspace` du skapade i början av den här självstudien och tryck sedan på `Create App`, t.ex:

![Skapa Slack-App](/media-drafts/9.create-slack-app.png)

När du som har skapats måste vi sedan gå till vår slack app och skapar ett snedstreck-kommando.

## <a name="create-a-slash-command"></a>Skapa ett snedstreck-kommando

Klicka på den `Slash Commands` menyalternativ.

![Snedstreck kommandon](/media-drafts/9.slash-commands.png)

Klicka på `Create New Command`

- Ange det namn som du vill använda för din snedstreck kommandot i det `command` fält.
- Ange ngrok offentliga URL: en till den `Request URL` fält

  > **VIKTIGT**
  >
  > Kom ihåg att lägga till `/api/RespondToSlackCommand` till URL

- Lägg till en kort beskrivning och användning-tipset.
- Tryck på `Save`

![Snedstreck kommandon](/media-drafts/9.create-slash-command.png)

När du har sparat en skärm visas t.ex:

![Snedstreck kommandon lyckades](/media-drafts/9.create-slash-commands-success.png)

## <a name="install-the-slack-app-to-the-workspace"></a>Installera slack-appen till arbetsytan

Innan vi kan använda vår slack app i vår arbetsyta, som vi behöver installera den.

1. Välj den `Basic Information` menyalternativ

2. Expandera den `Install your app to your workspace` alternativet och tryck på den `Install app to workspace` knappen.

   ![Installera appen till arbetsytan](/media-drafts/9.install-app-to-workspace.png)

3. Du kan bli ombedd att autentisera ditt program t.ex:

   ![Autentisera appen till arbetsytan](/media-drafts/9.authenticate-slack-app.png)

## <a name="try-it-out"></a>Prova

Nu när snedstreck-kommandot har skapats och är anslutna till vår som körs lokalt instans av Azure Functions via länken ngrok ska vi testa den.

1. Gå till arbetsytan slack som du har associerat med din slack-app.
2. Gå till alla chat-fönstret och skriv `/mojify` (eller det namn du gav appen)
3. Om allt har fungerat korrekt, bör du se `mojify` som ett alternativ

   ![Kontrollera Mojify](/media-drafts/9.slack-check-mojify.png)

4. Skriv nu `/mojify` och Lägg till en bild-URL, tryck på RETUR, t.ex:

   ![Typ Mojify](/media-drafts/9.slack-type-mojify.png)
