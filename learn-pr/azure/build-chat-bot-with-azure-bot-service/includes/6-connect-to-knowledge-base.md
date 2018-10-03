[!INCLUDE [0-vm-note](0-vm-note.md)]

I den här övningen ansluter du din chattrobot till QnA Maker-kunskapsbasen du skapade tidigare, så att chattroboten kan föra en intelligent konversation. När du ansluter till kunskapsbasen hämtas en del information från QnA Maker-portalen. Den kopieras till Azure-portalen, chattrobotens kod uppdateras och sedan distribueras chattroboten till Azure igen.

1. Gå tillbaka till QnA Maker-portalen på https://www.qnamaker.ai i VM-webbläsaren och välj kontoknappen i det övre högra hörnet.
1. Välj **Hantera slutpunktsnycklar** från listrutan.
1. Välj **Visa** för att visa den **primära** slutpunktsnyckeln och på **Kopiera** för att kopiera den till Urklipp. Klistra in den i en textfil så att du enkelt kan få tag på den om en stund.

    > [!NOTE]
    > Beroende på dina webbläsarinställningar kanske du måste tillåta cookies från tredje part att slutföra den här enheten.

1. Välj **Mina kunskapsbaser** i menyn längst upp på sidan.
1. Välj **Visa kod** för kunskapsbasen du skapade tidigare.

1. Kopiera kunskapsbasens ID från den första raden och värdnamnet från den andra raden. Klistra in även dem i en textfil. Stäng sedan dialogrutan. Ta **inte** med prefixet ”https://” i värdnamnet som du kopierar.

    ![Skärmbild av QnA Maker-portalen som visar ett exempel på en HTTP-begäran, med namnet på slutpunktens kunskapsbas-ID och värdnamn markerade.](../media/6-copy-endpoint-info.png)

1. Återgå till din Web App Bot i Azure-portalen. Välj **Programinställningar** i menyn till vänster och rulla nedåt tills du hittar programinställningarna QnAKnowledgebaseId, QnAAuthKey och QnAEndpointHostName. Klistra in kunskapsbasens ID och värdnamn som du nyss hämtade och slutpunktsnyckeln du har hämtat i de här fälten. Välj **Spara** högst upp.

    ![Skärmbild av Azure-portalen som visar robotbladet och programinställningar med menyalternativet Programinställningar och lämplig inställning av nycklar markerade.](../media/6-enter-app-settings.png)

1. Gå tillbaka till **Visual Studio Code** och ersätt innehållet i **app.js** med koden nedan. Spara sedan filen.

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
        appPassword: process.env.MicrosoftAppPassword,
        openIdMetadata: process.env.BotOpenIdMetadata
    });

    // Listen for messages from users
    server.post('/api/messages', connector.listen());

    var tableName = 'botdata';
    var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
    var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

    // Create your bot with a function to receive messages from the user
    var bot = new builder.UniversalBot(connector);
    bot.set('storage', tableStorage);

    // Recognizer and and Dialog for preview QnAMaker service
    var previewRecognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId,
        authKey: process.env.QnAAuthKey || process.env.QnASubscriptionKey
    });

    var basicQnAMakerPreviewDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [previewRecognizer],
        defaultMessage: 'No match! Try changing the query terms!',
        qnaThreshold: 0.3
    }
    );

    bot.dialog('basicQnAMakerPreviewDialog', basicQnAMakerPreviewDialog);

    // Recognizer and and Dialog for GA QnAMaker service
    var recognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId,
        authKey: process.env.QnAAuthKey || process.env.QnASubscriptionKey, // Backward compatibility with QnAMaker (Preview)
        endpointHostName: process.env.QnAEndpointHostName
    });

    var basicQnAMakerDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [recognizer],
        defaultMessage: "I'm not quite sure what you're asking. Please ask your question again.",
        qnaThreshold: 0.3
    });

    bot.dialog('basicQnAMakerDialog', basicQnAMakerDialog);

    bot.dialog('/', //basicQnAMakerDialog);
        [
            function (session) {
                var qnaKnowledgebaseId = process.env.QnAKnowledgebaseId;
                var qnaAuthKey = process.env.QnAAuthKey || process.env.QnASubscriptionKey;
                var endpointHostName = process.env.QnAEndpointHostName;

                // QnA Subscription Key and KnowledgeBase Id null verification
                if ((qnaAuthKey == null || qnaAuthKey == '') || (qnaKnowledgebaseId == null || qnaKnowledgebaseId == ''))
                    session.send('Please set QnAKnowledgebaseId, QnAAuthKey and QnAEndpointHostName (if applicable) in App Settings. Learn how to get them at https://aka.ms/qnaabssetup.');
                else {
                    if (endpointHostName == null || endpointHostName == '')
                        // Replace with Preview QnAMakerDialog service
                        session.replaceDialog('basicQnAMakerPreviewDialog');
                    else
                        // Replace with GA QnAMakerDialog service
                        session.replaceDialog('basicQnAMakerDialog');
                }
            }
        ]);
    ```

    > [!NOTE]
    > Anrop för att skapa en `QnAMakerDialog`-instans på rad 30. Det här skapar en dialogruta som integrerar en robot som skapats med Azure Bot Service med en kunskapsbas som skapats med Microsoft QnA Maker.

1. Välj knappen **Källkodskontroll** i aktivitetsfältet i Visual Studio Code.
1. Hovra över filen **app.js** och välj knappen __+__ för att mellanlagra ändringar av filen för nästa incheckning.
1. Skriv ”Connected to knowledge base” i meddelanderutan och klicka på bockmarkeringen för att bekräfta ändringarna.

    > [!Warning]
    > Om du upptäcker ändringar hos en **package.json**-fil måste du se till att du inte använder dem i din allokering. Din allokering ska bara innehålla dina ändringar till **app.js**.

1. Markera ellipsen (__...__) och använd kommandot **Publish Branch** (Publicera gren) till att skicka ändringarna till fjärrdatabasen (och därmed till Azure Web App).

1. Gå tillbaka till din Web App Bot i Azure-portalen och välj **Testa i webbchatt** till vänster för att öppna testkonsolen. Skriv ”What's the most popular software programming language in the world?” i fältet längst ned i chattfönstret och tryck på **Retur**. Kontrollera att roboten svarar.

Grattis! Din robot är ansluten till kunskapsbasen och kan svara på frågor.
