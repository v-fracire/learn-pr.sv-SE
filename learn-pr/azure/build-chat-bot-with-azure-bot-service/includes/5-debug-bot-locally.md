[!INCLUDE [0-vm-note](0-vm-note.md)]

Som med all annan programkod du skriver måste ändringarna i robotkoden testas och felsökas lokalt innan de distribueras till en produktionsmiljö. Om du vill få hjälp med felsökning av roboten har Microsoft en Bot Framework-emulator. I den här övningen får du lära dig hur du använder Visual Studio Code och emulatorn för att felsöka robotar.

1. Kör följande kommando i Visual Studio Codes integrerade terminal för att installera [Restify](http://restify.com/), ett populärt Node.js-paket för att skapa och använda RESTful-webbtjänster:

    ```bash
    npm install restify
    ```

1. Upprepa det här steget för följande kommandon för att installera [Microsoft Bot Framework Bot Builder SDK för Node.js](https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-quickstart):

    ```bash
    npm install botbuilder
    npm install botbuilder-azure
    npm install botbuilder-cognitiveservices
    ```

1. Välj knappen **Explorer** i Visual Studio Codes aktivitetsfält. 
1. Därefter **app.js** för att öppna koden i redigeringsprogrammet. Den här filen innehåller koden som styr roboten – kod som har genererats av Azure Bot Service och hämtats från Azure-portalen.

1. Ersätt innehållet i **app.js** med följande kod och spara sedan filen.

    ```JavaScript
    "use strict";
    var builder = require("botbuilder");
    var botbuilder_azure = require("botbuilder-azure");

    var useEmulator = true;
    var userName = "";
    var yearsCoding = "";
    var selectedLanguage = "";

    var connector = useEmulator ? new builder.ChatConnector() : new botbuilder_azure.BotServiceConnector({
        appId: process.env.MicrosoftAppId,
        appPassword: process.env.MicrosoftAppPassword
    });

    var bot = new builder.UniversalBot(connector);

    bot.dialog('/', [

    function (session) {
        builder.Prompts.text(session, "Hello, and welcome to QnA Factbot! What's your name?");
    },

    function (session, results) {
        userName = results.response;
        builder.Prompts.number(session, "Hi " + userName + ", how many years have you been writing code?");
    },

    function (session, results) {
        yearsCoding = results.response;
        builder.Prompts.choice(session, "What language do you love the most?", ["C#", "Python", "Node.js", "Visual FoxPro"]);
    },

    function (session, results) {
        selectedLanguage = results.response.entity;

        session.send("Okay, " + userName + ", I think I've got it:" +
            " You've been writing code for " + yearsCoding + " years," +
            " and prefer to use " + selectedLanguage + ".");
    }]);

    var restify = require('restify');
    var server = restify.createServer();

    server.listen(3978, function() {
        console.log('test bot endpoint at http://localhost:3978/api/messages');
    });

    server.post('/api/messages', connector.listen());
    ```

1. Ange brytpunkter på raderna 20, 25 och 30 (`builder.Prompts...` anrop) genom att klicka i marginalen till vänster.

1. Välj **Felsök** i aktivitetsfältet och välj sedan den gröna pilknappen **Starta felsökning** för att starta en felsökningssession. Kontrollera att meddelandet ”test bot endpoint at http://localhost:3978/api/messages” visas i felsökningskonsolen.

    ![Skärmbild av Visual Studio Code som visar felsökningssystemet med navigeringsobjektet för felsökning och uppspelningsknappen för felsökning som används för att starta en felsökningssession markerade.](../media/5-vs-launch-debugger.png)

    Din robotkod körs nu lokalt.

1. Starta **Bot Framework Emulator** på Start-menyn.

1. Markera fältet för att **ange URL för slutpunkt**. Ange namnet och URL:en till roboten såsom de visades i felsökningskonsolen i föregående steg.

1. Lämna fälten för ID och lösenord för Microsoft-app och fälten för nationella inställningar fälten tomma och välj **ANSLUT**.

1. Välj sedan **Spara och anslut** och spara konfigurationsfilen på valfri plats.

    >[!NOTE]
    > Hädanefter kan du återansluta roboten genom att helt enkelt välja namnet på roboten under Mina robotar.

    ![Skärmbild av Bot Framework Emulator som visar den nya skärmen för robotkonfiguration knappen Spara och anslut markerad.](../media/5-new-bot-configuration.png)

1. Skriv ”hi” i rutan längst ned i emulatorn och tryck på **Retur**. Bekräfta att Visual Studio Code stannar på rad 20 i **app.js**. Välj därefter knappen **Fortsätt** i verktygsfältet för felsökning i Visual Studio Code. Du kommer då tillbaka till emulatorn där du ser robotens svar.

1. Roboten kommer att ställa ett antal frågor till dig. Besvara dem och välj **Fortsätt** i Visual Studio Code varje gång du kommer till en brytpunkt. När du är klar väljer du knappen **Stopp** i verktygsfältet för felsökning för att avsluta sessionen.

Nu har du en fullt fungerande robot och vet hur man gör för att felsöka den lokalt i Microsoft Frameworks robotemulator. Nästa steg blir att göra roboten smartare genom att ansluta till den kunskapsbas som du publicerade.