[!INCLUDE [0-vm-note](0-vm-note.md)]

<span data-ttu-id="cc1ba-101">I den här övningen ansluter du din chattrobot till QnA Maker-kunskapsbasen du skapade tidigare, så att chattroboten kan föra en intelligent konversation.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-101">In this unit, you will connect your bot to the QnA Maker knowledge base you built earlier so that the bot can carry on an intelligent conversation.</span></span> <span data-ttu-id="cc1ba-102">När du ansluter till kunskapsbasen hämtas en del information från QnA Maker-portalen. Den kopieras till Azure-portalen, chattrobotens kod uppdateras och sedan distribueras chattroboten till Azure igen.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-102">Connecting to the knowledge base involves retrieving some information from the QnA Maker portal, copying it into the Azure portal, updating the bot code, and then redeploying the bot to Azure.</span></span>

1. <span data-ttu-id="cc1ba-103">Gå tillbaka till QnA Maker-portalen på https://www.qnamaker.ai i VM-webbläsaren och välj kontoknappen i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-103">Return to the QnA Maker portal at https://www.qnamaker.ai in the VM browser and select the account button in the upper-right corner.</span></span>
1. <span data-ttu-id="cc1ba-104">Välj **Hantera slutpunktsnycklar** från listrutan.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-104">Select **Manage endpoint keys** from the menu that drops down.</span></span>
1. <span data-ttu-id="cc1ba-105">Välj **Visa** för att visa den **primära** slutpunktsnyckeln och på **Kopiera** för att kopiera den till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-105">Select **Show** to show the **Primary** endpoint key and **Copy** to copy it to the clipboard.</span></span> <span data-ttu-id="cc1ba-106">Klistra in den i en textfil så att du enkelt kan få tag på den om en stund.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-106">Paste it into a text file, so you can easily retrieve it in a moment.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc1ba-107">Beroende på dina webbläsarinställningar kanske du måste tillåta cookies från tredje part att slutföra den här enheten.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-107">Depending on your browser settings, you may need to allow third-party cookies to complete this unit.</span></span>

1. <span data-ttu-id="cc1ba-108">Välj **Mina kunskapsbaser** i menyn längst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-108">Select **My knowledge bases** in the menu at the top of the page.</span></span>
1. <span data-ttu-id="cc1ba-109">Välj **Visa kod** för kunskapsbasen du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-109">Then, select **View Code** for the knowledge base that you created earlier.</span></span>

1. <span data-ttu-id="cc1ba-110">Kopiera kunskapsbasens ID från den första raden och värdnamnet från den andra raden.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-110">Copy the knowledge base ID from the first line and the host name from the second line.</span></span> <span data-ttu-id="cc1ba-111">Klistra in även dem i en textfil.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-111">Paste them into a text file, as well.</span></span> <span data-ttu-id="cc1ba-112">Stäng sedan dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-112">Then, close the dialog.</span></span> <span data-ttu-id="cc1ba-113">Ta **inte** med prefixet ”https://” i värdnamnet som du kopierar.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-113">**Do not** include the "https://" prefix in the host name that you copy.</span></span>

    ![Skärmbild av QnA Maker-portalen som visar ett exempel på en HTTP-begäran, med namnet på slutpunktens kunskapsbas-ID och värdnamn markerade.](../media/6-copy-endpoint-info.png)

1. <span data-ttu-id="cc1ba-115">Återgå till din Web App Bot i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-115">Return to the Web App Bot in the Azure portal.</span></span> <span data-ttu-id="cc1ba-116">Välj **Programinställningar** i menyn till vänster och rulla nedåt tills du hittar programinställningarna QnAKnowledgebaseId, QnAAuthKey och QnAEndpointHostName.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-116">Select **Application settings** in the menu on the left, and scroll down until you find application settings named "QnAKnowledgebaseId," "QnAAuthKey," and "QnAEndpointHostName."</span></span> <span data-ttu-id="cc1ba-117">Klistra in kunskapsbasens ID och värdnamn som du nyss hämtade och slutpunktsnyckeln du har hämtat i de här fälten.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-117">Paste the knowledge base ID and host name you just obtained and the endpoint key obtained previously into these fields.</span></span> <span data-ttu-id="cc1ba-118">Välj **Spara** högst upp.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-118">Then, select **Save** at the top.</span></span>

    ![Skärmbild av Azure-portalen som visar robotbladet och programinställningar med menyalternativet Programinställningar och lämplig inställning av nycklar markerade.](../media/6-enter-app-settings.png)

1. <span data-ttu-id="cc1ba-120">Gå tillbaka till **Visual Studio Code** och ersätt innehållet i **app.js** med koden nedan.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-120">Return to **Visual Studio Code** and replace the contents of **app.js** with the code below.</span></span> <span data-ttu-id="cc1ba-121">Spara sedan filen.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-121">Then, save the file.</span></span>

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
    > <span data-ttu-id="cc1ba-122">Anrop för att skapa en `QnAMakerDialog`-instans på rad 30.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-122">The call to create a `QnAMakerDialog` instance on line 30.</span></span> <span data-ttu-id="cc1ba-123">Det här skapar en dialogruta som integrerar en robot som skapats med Azure Bot Service med en kunskapsbas som skapats med Microsoft QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-123">This creates a dialog that integrates a bot built with the Azure Bot Service with a knowledge base built via Microsoft QnA Maker.</span></span>

1. <span data-ttu-id="cc1ba-124">Välj knappen **Källkodskontroll** i aktivitetsfältet i Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-124">Select the **Source Control** button in the activity bar in Visual Studio Code.</span></span>
1. <span data-ttu-id="cc1ba-125">Hovra över filen **app.js** och välj knappen __+__ för att mellanlagra ändringar av filen för nästa incheckning.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-125">Hover over the **app.js** file and select the __+__ button to stage that file's changes for the next commit.</span></span>
1. <span data-ttu-id="cc1ba-126">Skriv ”Connected to knowledge base” i meddelanderutan och klicka på bockmarkeringen för att bekräfta ändringarna.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-126">Type "Connected to knowledge base" into the message box, and select the check mark to commit your changes.</span></span>

    > [!Warning]
    > <span data-ttu-id="cc1ba-127">Om du upptäcker ändringar hos en **package.json**-fil måste du se till att du inte använder dem i din allokering.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-127">If you see changes to a **package.json** file ensure you do NOT include them in your commit.</span></span> <span data-ttu-id="cc1ba-128">Din allokering ska bara innehålla dina ändringar till **app.js**.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-128">Your commit should only include your changes to **app.js**.</span></span>

1. <span data-ttu-id="cc1ba-129">Markera ellipsen (__...__) och använd kommandot **Publish Branch** (Publicera gren) till att skicka ändringarna till fjärrdatabasen (och därmed till Azure Web App).</span><span class="sxs-lookup"><span data-stu-id="cc1ba-129">Then, select the ellipsis (__...__) button and use the **Publish Branch** command to push these changes to the remote repository and the Azure Web App.</span></span>

1. <span data-ttu-id="cc1ba-130">Gå tillbaka till din Web App Bot i Azure-portalen och välj **Testa i webbchatt** till vänster för att öppna testkonsolen.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-130">Return to the web app bot in the Azure portal, and select **Test in Web Chat** on the left to open the test console.</span></span> <span data-ttu-id="cc1ba-131">Skriv ”What's the most popular software programming language in the world?”</span><span class="sxs-lookup"><span data-stu-id="cc1ba-131">Type "What's the most popular software programming language in the world?"</span></span> <span data-ttu-id="cc1ba-132">i fältet längst ned i chattfönstret och tryck på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-132">into the box at the bottom of the chat window and press **Enter**.</span></span> <span data-ttu-id="cc1ba-133">Kontrollera att roboten svarar.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-133">Confirm that the bot responds.</span></span>

<span data-ttu-id="cc1ba-134">Grattis!</span><span class="sxs-lookup"><span data-stu-id="cc1ba-134">Congratulations!</span></span> <span data-ttu-id="cc1ba-135">Din robot är ansluten till kunskapsbasen och kan svara på frågor.</span><span class="sxs-lookup"><span data-stu-id="cc1ba-135">Your bot is connected to the knowledge base and can respond to questions.</span></span>
