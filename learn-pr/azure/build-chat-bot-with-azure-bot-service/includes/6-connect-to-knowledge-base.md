I den här övningen ansluter du din chattrobot till QnA Maker-kunskapsbasen du skapade tidigare, så att chattroboten kan föra en intelligent konversation. När du ansluter till kunskapsbasen hämtas en del information från QnA Maker-portalen. Den kopieras till Azure Portal, chattrobotens kod uppdateras och sedan distribueras chattroboten till Azure igen.

1. Gå tillbaka till den [QnA Maker-portalen](https://www.qnamaker.ai/) och klicka på ditt namn i det övre högra hörnet. Välj **Hantera slutpunktsnycklar** från listrutan. Klicka på **Visa** för att visa den primära slutpunktsnyckeln och på **Kopiera** för att kopiera den till Urklipp. Klistra sedan in den i en textfil så att du enkelt kan få tag på den om en stund.

1. Klicka på **Mina kunskapsbaser** i menyn längst upp på sidan. Klicka sedan på **Visa kod** för kunskapsbasen du skapade tidigare.

1. Kopiera kunskapsbasens ID från den första raden och värdnamnet från den andra raden. Klistra in även dem i en textfil. Stäng sedan dialogrutan. **Inkludera inte** prefixet https:// i värdnamnet du kopierar.

    ![Skärmbild av QnA Maker-portalen som visar sedan exempel HTTP-begäran med namnet på slutpunkten kunskapsbas-ID och värd markerat.](../media/6-copy-endpoint-info.png)

1. Återgå till din Web App Bot i Azure-portalen. Klicka på **Programinställningar** i menyn till vänster och rulla nedåt tills du hittar programinställningarna QnAKnowledgebaseId, QnAAuthKey och QnAEndpointHostName. Klistra in kunskapsbasens ID och värdnamn från steg 3 och slutpunktsnyckeln från steg 1 i de här fälten. Klicka sedan på **Spara**.

    ![Skärmbild av Azure-portalen som visar roboten bladet och programinställningar information med menyalternativet för inställningar för program och lämplig inställning nycklar markering.](../media/6-enter-app-settings.png)

1. Gå tillbaka till Visual Studio Code och ersätt innehållet i **app.js** med koden nedan. Spara sedan filen.

    ```JavaScript
    var restify = require('restify');
    var builder = require('botbuilder');
    var botbuilder_azure = require("botbuilder-azure");
    var builder_cognitiveservices = require("botbuilder-cognitiveservices");

    // Setup Restify Server
    var server = restify.createServer();
    server.listen(process.env.port || process.env.PORT || 3978, function () {
        console.log('%s listening to %s', server.name, server.url);
    });

    // Create chat connector for communicating with the Bot Framework Service
    var connector = new builder.ChatConnector({
        appId: process.env.MicrosoftAppId,
        appPassword: process.env.MicrosoftAppPassword
    });

    // Listen for messages from users
    server.post('/api/messages', connector.listen());

    // Create your bot with a function to receive messages from the user
    var bot = new builder.UniversalBot(connector);

    var recognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId,
        authKey: process.env.QnAAuthKey,
        endpointHostName: process.env.QnAEndpointHostName
    });

    var basicQnAMakerDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [recognizer],
        defaultMessage: "I'm not quite sure what you're asking. Please ask your question again.",
        qnaThreshold: 0.3
    });

    bot.dialog('basicQnAMakerDialog', basicQnAMakerDialog);

    bot.dialog('/',
    [
        function (session) {
            session.replaceDialog('basicQnAMakerDialog');
        }
    ]);
    ```

    > [!Note]
    > Anropet för att skapa en `QnAMakerDialog` instans på raderna 30. Det här skapar en dialogruta som integrerar en chattrobot som skapats med Azure Bot Service med en kunskapsbas som skapats med Microsoft QnA Maker.

1. Klicka på knappen **Källkodskontroll** i aktivitetsfältet i Visual Studio Code. Skriv ”Connected to knowledge base” i meddelanderutan och klicka på bockmarkeringen för att bekräfta ändringarna. Klicka på ellipsen och använd kommandot **Publicera gren** till att skicka ändringarna till fjärrdatabasen (och därmed till Azure Web App).

1. Gå tillbaka till din Web App Bot i Azure-portalen och klicka på **Testa i webbchatt** till vänster för att öppna testkonsolen. Skriv ”What's the most popular software programming language in the world?” i fältet längst ned i chattfönstret och tryck på **Enter**. Bekräfta att roboten svarar.

Nu när roboten är ansluten till kunskapsbasen är det sista steget att testa den i verkligheten. Och vad kan vara mer verkligt än att testa den med Skype?
