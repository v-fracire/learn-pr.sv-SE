Som med all annan programkod du skriver måste ändringarna i robotkoden testas och felsökas lokalt innan något kan distribueras till en produktionsmiljö. Om du vill få hjälp med felsökning av roboten har Microsoft en [Bot Framework-emulator](https://emulator.botframework.com/). I den här övningen får du lära dig hur du använder Visual Studio Code och emulatorn för att felsöka robotar.

1. Om du inte har installerat Microsoft Bot Framework-emulatorn ännu måste du göra det nu. Du kan ladda ned den från https://emulator.botframework.com/. Det finns versioner tillgängliga för Windows, macOS och Linux.

1. Kör följande kommando i Visual Studio Codes integrerade terminal för att installera [Restify](http://restify.com/) ett populärt Node.js-paket för att skapa och använda RESTful-webbtjänster:

    ```
    npm install restify
    ```

1. Upprepa det här steget för följande kommandon för att installera [Microsoft Bot Framework Bot Builder SDK för Node.js](https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-quickstart):

    ```
    npm install botbuilder
    npm install botbuilder-azure
    npm install botbuilder-cognitiveservices
    ```

1. Klicka på knappen **Utforskaren** i Visual Studio Codes aktivitetsfält. Välj sedan **app.js** för att öppna koden i redigeringsprogrammet. Den här filen innehåller koden som styr roboten – kod som har genererats av Azure Bot Service och hämtats från Azure-portalen.

    ![Öppna app.js](../media-draft/5-vs-select-index-js.png)

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

1. Ange brytpunkter på raderna 20, 25 och 30 genom att klicka i marginalen till vänster.
 
    ![Lägga till brytpunkter i app.js](../media-draft/5-vs-add-breakpoints.png)

1. Klicka på **Felsök** i aktivitetsfältet och klicka sedan på den gröna pilen för att starta en felsökningssession. Kontrollera att meddelandet ”test bot endpoint at http://localhost:3978/api/messages” visas i felsökningskonsolen.
 
    ![Starta felsökningsprogrammet](../media-draft/5-vs-launch-debugger.png)

1. Din robotkod körs nu lokalt. Starta Bot Framework-emulatorn och klicka på **Create a new bot configuration** (Skapa en ny robotkonfiguration). Ange namnet och URL:en till roboten såsom de visades i felsökningskonsolen i föregående steg. Klicka sedan på **Spara och anslut** och spara konfigurationsfilen på valfri plats.

    > Hädanefter kan du återansluta roboten genom att helt enkelt klicka på namnet på roboten under Mina robotar.

    ![Ansluta till roboten](../media-draft/5-new-bot-configuration.png)

1. Skriv ”hi” i rutan längst ned i emulatorn och tryck på **Retur**. Bekräfta att Visual Studio Code stannar på rad 20 i **app.js**. Klicka sedan på knappen **Fortsätt** i verktygsfältet för felsökning i Visual Studio Code. Du kommer då tillbaka till emulatorn där du ser robotens svar.
 
    ![Fortsätta i felsökningsprogrammet](../media-draft/5-continue-debugging.png)

1. Roboten kommer att ställa ett antal frågor till dig. Besvara dem och klicka på **Fortsätt** i Visual Studio Code varje gång du kommer till en brytpunkt. När du är klar klickar du på knappen **Stopp** i verktygsfältet för felsökning för att avsluta sessionen.

Nu har du en fullt fungerande robot och vet hur man gör för att felsöka den lokalt i Microsoft Frameworks robotemulator. Nästa steg blir att göra roboten smartare genom att ansluta till den kunskapsbas som du publicerade.