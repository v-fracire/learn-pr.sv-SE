<span data-ttu-id="8a918-101">Tillägget Azure Cosmos DB för Visual Studio Code gör det enklare att skapa konton, databaser och samlingar genom att använda kommandofönstret för att skapa resurser.</span><span class="sxs-lookup"><span data-stu-id="8a918-101">The Azure Cosmos DB extension for Visual Studio Code simplifies account, database, and collection creation by enabling you to create resources using the command window.</span></span>

<span data-ttu-id="8a918-102">I den här enheten ska du installera Azure Cosmos DB-tillägget för Visual Studio och sedan använda det för att skapa ett konto, databas och samling.</span><span class="sxs-lookup"><span data-stu-id="8a918-102">In this unit you will install the Azure Cosmos DB extension for Visual Studio, and then use it to create an account, database, and collection.</span></span>

## <a name="install-the-azure-cosmos-db-extension-for-visual-studio"></a><span data-ttu-id="8a918-103">Installera Azure Cosmos DB-tillägget för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a918-103">Install the Azure Cosmos DB extension for Visual Studio</span></span>

1. <span data-ttu-id="8a918-104">Gå till [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) och installera tillägget **Azure Cosmos DB** för Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8a918-104">Go to the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) and install the **Azure Cosmos DB** extension for Visual Studio Code.</span></span>

1. <span data-ttu-id="8a918-105">När tilläggets flik har laddats på Visual Studio Code klickar du på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="8a918-105">When the extension tab loads in Visual Studio Code, click **Install**.</span></span>

1. <span data-ttu-id="8a918-106">När installationen är klar klickar du på **Läs in på nytt**.</span><span class="sxs-lookup"><span data-stu-id="8a918-106">After installation is complete, click **Reload**.</span></span>

    <span data-ttu-id="8a918-107">Visual Studio Code visar</span><span class="sxs-lookup"><span data-stu-id="8a918-107">Visual Studio Code displays the</span></span> ![Azure-ikonen](../media/2-setup/visual-studio-code-explorer-icon.png) <span data-ttu-id="8a918-109">Azure-ikonen till vänster på skärmen när tillägget har installerats och lästs in på nytt.</span><span class="sxs-lookup"><span data-stu-id="8a918-109">Azure icon on the left side of the screen after the extension is installed and reloaded.</span></span>

## <a name="create-an-azure-cosmos-db-account-in-visual-studio-code"></a><span data-ttu-id="8a918-110">Skapa ett Azure Cosmos DB-konto i Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8a918-110">Create an Azure Cosmos DB account in Visual Studio Code</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

1. <span data-ttu-id="8a918-111">Logga in på Azure i Visual Studio Code genom att klicka på **Visa** > **Kommandopalett** och ange **Azure: Logga in**.</span><span class="sxs-lookup"><span data-stu-id="8a918-111">In Visual Studio Code, sign in to Azure by clicking **View** > **Command Palette** and typing **Azure: Sign In**.</span></span> <span data-ttu-id="8a918-112">[Azure-kontotillägget](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) måste vara installerat i Visual Studio Code för att kunna använda Azure: inloggning.</span><span class="sxs-lookup"><span data-stu-id="8a918-112">You must have the [Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extension installed to use Azure: Sign In.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8a918-113">Logga in på Azure med samma konto som användes för att skapa sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="8a918-113">Login to Azure using the same account used to create the sandbox.</span></span> <span data-ttu-id="8a918-114">Sandbox-miljön ger åtkomst till en Concierge-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8a918-114">The sandbox provides access to a Concierge Subscription.</span></span>

    <span data-ttu-id="8a918-115">Följ anvisningarna för att kopiera och klistra in koden i webbläsaren. Därmed autentiseras din Visual Studio Code-session.</span><span class="sxs-lookup"><span data-stu-id="8a918-115">Follow the prompts to copy and paste the code provided in the web browser, which authenticates your Visual Studio Code session.</span></span>

1. <span data-ttu-id="8a918-116">Klicka på ![Azure ikon](../media/2-setup/visual-studio-code-explorer-icon.png) **Azure**-ikonen i den vänstra menyn. Högerklicka sedan på **Concierge-prenumeration** och klicka på **Skapa konto**.</span><span class="sxs-lookup"><span data-stu-id="8a918-116">Click the ![Azure icon](../media/2-setup/visual-studio-code-explorer-icon.png) **Azure** icon on the left menu, and then right-click **Concierge Subscription**, and click **Create Account**.</span></span>

    <span data-ttu-id="8a918-117">Om du inte ser Concierge-prenumerationen i listan bör du kontrollera att du är inloggad på Azure i Visual Studio Code med samma konto som du använde för att skapa sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="8a918-117">If you do not see the Concierge Subscription listed, ensure you logged into Azure in Visual Studio Code using the same account used to create the sandbox.</span></span> <span data-ttu-id="8a918-118">Om du har filtrerat dina Azure-prenumerationer i Azure-kontotillägget bör du kontrollera att Concierge-prenumeration har markerats i kommandot `> Azure: Select Subscriptions`.</span><span class="sxs-lookup"><span data-stu-id="8a918-118">Additionally, if you have filtered your Azure subscriptions in the Azure Account extension, verify the Concierge Subscription is checked in the `> Azure: Select Subscriptions` command.</span></span>

1. <span data-ttu-id="8a918-119">Klicka på knappen __+__ för att börja skapa ett Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="8a918-119">Click the __+__ button to start creating a Cosmos DB account.</span></span> <span data-ttu-id="8a918-120">Du blir ombedd att välja prenumerationen om du har fler än en.</span><span class="sxs-lookup"><span data-stu-id="8a918-120">You will be asked to select the subscription if you have more than one.</span></span>

1. <span data-ttu-id="8a918-121">Ange ett unikt namn för ditt Azure Cosmos DB-konto i textrutan längst upp på skärmen och tryck sedan på Retur.</span><span class="sxs-lookup"><span data-stu-id="8a918-121">In the text box at the top of the screen, enter a unique name for your Azure Cosmos DB account, and then press enter.</span></span> <span data-ttu-id="8a918-122">Kontonamnet får bara innehålla gemener, siffror och bindestreck och måste vara mellan 3 och 31 tecken.</span><span class="sxs-lookup"><span data-stu-id="8a918-122">The account name can contain only lowercase letters, numbers and the '-' character, and must be between 3 and 31 characters.</span></span>

1. <span data-ttu-id="8a918-123">Välj sedan **SQL (DocumentDB)** > **<rgn>[resursgruppnamn för sandbox]</rgn>** och välj sedan en plats.</span><span class="sxs-lookup"><span data-stu-id="8a918-123">Next, select **SQL (DocumentDB)** > **<rgn>[sandbox resource group name]</rgn>**, and then select a location.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    <span data-ttu-id="8a918-124">Fliken utdata i Visual Studio Code visar förloppet för att skapa kontot. Det tar några minuter att slutföra det.</span><span class="sxs-lookup"><span data-stu-id="8a918-124">The output tab in Visual Studio Code displays the progress of the account creation, it takes a few minutes to complete.</span></span>

1. <span data-ttu-id="8a918-125">När kontot har skapats kan du expandera din Azure-prenumeration i fönstret **Azure: Cosmos DB**. Tillägget visas det nya Azure Cosmos DB-kontot.</span><span class="sxs-lookup"><span data-stu-id="8a918-125">After the account is created, expand your Azure subscription in the **Azure: Cosmos DB** pane and the extension displays the new Azure Cosmos DB account.</span></span> <span data-ttu-id="8a918-126">I följande bild heter det nya kontot **inlärningsmoduler**.</span><span class="sxs-lookup"><span data-stu-id="8a918-126">In the following image, the new account is named **learning-modules**.</span></span>

    ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/azure-cosmos-db-vs-code-extension.png)

## <a name="create-an-azure-cosmos-db-database-and-collection-in-visual-studio-code"></a><span data-ttu-id="8a918-128">Skapa en Azure Cosmos DB-databas och en samling i Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8a918-128">Create an Azure Cosmos DB database and collection in Visual Studio Code</span></span>

<span data-ttu-id="8a918-129">Nu ska vi skapa en ny databas och samling för dina kunder.</span><span class="sxs-lookup"><span data-stu-id="8a918-129">Now let's create a new database and collection for your customers.</span></span>

1. <span data-ttu-id="8a918-130">Högerklicka på ditt nya konto i Azure Cosmos DB och klicka sedan på **Skapa databas**.</span><span class="sxs-lookup"><span data-stu-id="8a918-130">In the Azure: Cosmos DB pane, right-click your new account, and then click **Create Database**.</span></span>
1. <span data-ttu-id="8a918-131">I indatapaletten överst på skärmen skriver du `Users` för databasens namn. Tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="8a918-131">In the input palette at the top of the screen, type `Users` for the database name and press Enter.</span></span>
1. <span data-ttu-id="8a918-132">Ange `WebCustomers` för samlingens namn och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="8a918-132">Enter `WebCustomers` for the collection name and press Enter.</span></span>
1. <span data-ttu-id="8a918-133">Ange `userId` för partitionsnyckeln och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="8a918-133">Enter `userId` for the partition key and press Enter.</span></span>
1. <span data-ttu-id="8a918-134">Bekräfta `1000` för den inledande dataflödeskapaciteten och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="8a918-134">Finally, confirm `1000` for the initial throughput capacity and press Enter.</span></span>
1. <span data-ttu-id="8a918-135">Expandera kontot i fönstret **Azure: Cosmos DB**. Den nya **Användardatabasen** och samlingen **WebCustomers** visas.</span><span class="sxs-lookup"><span data-stu-id="8a918-135">Expand the account in the **Azure: Cosmos DB** pane, and the new **Users** database and **WebCustomers** collection are displayed.</span></span>

    ![Animering av ovanstående anvisningar som körs via Azure Cosmos DB-tillägget i Visual Studio Code.](../media/2-setup/vs-code-azure-cosmos-db-extension.gif)

<span data-ttu-id="8a918-137">Nu när du har ditt Azure Cosmos DB-konto kan du sätta igång i Visual Studio Code!</span><span class="sxs-lookup"><span data-stu-id="8a918-137">Now that you have your Azure Cosmos DB account, lets get to work in Visual Studio Code!</span></span>
