Tillägget Azure Cosmos DB för Visual Studio Code gör det enklare att skapa konton, databaser och samlingar genom att använda kommandofönstret för att skapa resurser.

I den här enheten ska du installera Azure Cosmos DB-tillägget för Visual Studio och sedan använda det för att skapa ett konto, databas och samling.

## <a name="install-the-azure-cosmos-db-extension-for-visual-studio"></a>Installera Azure Cosmos DB-tillägget för Visual Studio

1. Gå till [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) och installera tillägget **Azure Cosmos DB** för Visual Studio Code.

1. När tilläggets flik har laddats på Visual Studio Code klickar du på **Installera**.

1. När installationen är klar klickar du på **Läs in på nytt**.

    Visual Studio Code visar ![Azure-ikonen](../media/2-setup/visual-studio-code-explorer-icon.png) Azure-ikonen till vänster på skärmen när tillägget har installerats och lästs in på nytt.

## <a name="create-an-azure-cosmos-db-account-in-visual-studio-code"></a>Skapa ett Azure Cosmos DB-konto i Visual Studio Code

[!include[](../../../includes/azure-sandbox-activate.md)]

1. Logga in på Azure i Visual Studio Code genom att klicka på **Visa** > **Kommandopalett** och ange **Azure: Logga in**. [Azure-kontotillägget](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) måste vara installerat i Visual Studio Code för att kunna använda Azure: inloggning.

    > [!IMPORTANT]
    > Logga in på Azure med samma konto som användes för att skapa sandbox-miljön. Sandbox-miljön ger åtkomst till en Concierge-prenumeration.

    Följ anvisningarna för att kopiera och klistra in koden i webbläsaren. Därmed autentiseras din Visual Studio Code-session.

1. Klicka på ![Azure ikon](../media/2-setup/visual-studio-code-explorer-icon.png) **Azure**-ikonen i den vänstra menyn. Högerklicka sedan på **Concierge-prenumeration** och klicka på **Skapa konto**.

    Om du inte ser Concierge-prenumerationen i listan bör du kontrollera att du är inloggad på Azure i Visual Studio Code med samma konto som du använde för att skapa sandbox-miljön. Om du har filtrerat dina Azure-prenumerationer i Azure-kontotillägget bör du kontrollera att Concierge-prenumeration har markerats i kommandot `> Azure: Select Subscriptions`.

1. Klicka på knappen __+__ för att börja skapa ett Cosmos DB-konto. Du blir ombedd att välja prenumerationen om du har fler än en.

1. Ange ett unikt namn för ditt Azure Cosmos DB-konto i textrutan längst upp på skärmen och tryck sedan på Retur. Kontonamnet får bara innehålla gemener, siffror och bindestreck och måste vara mellan 3 och 31 tecken.

1. Välj sedan **SQL (DocumentDB)** > **<rgn>[Sandbox-resursgruppens namn]</rgn>** och välj sedan en plats.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    Fliken utdata i Visual Studio Code visar förloppet för att skapa konto. Det tar några minuter att slutföra det.

1. När kontot har skapats kan du expandera din Azure-prenumeration i fönstret **Azure: Cosmos DB**. Tillägget visas det nya Azure Cosmos DB-kontot. I följande bild heter det nya kontot **inlärningsmoduler**.

    ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/azure-cosmos-db-vs-code-extension.png)

## <a name="create-an-azure-cosmos-db-database-and-collection-in-visual-studio-code"></a>Skapa en Azure Cosmos DB-databas och en samling i Visual Studio Code

Nu ska vi skapa en ny databas och samling för dina kunder.

1. Högerklicka på ditt nya konto i Azure Cosmos DB och klicka sedan på **Skapa databas**.
1. I indatapaletten överst på skärmen skriver du `Users` för databasens namn. Tryck på Retur.
1. Ange `WebCustomers` för samlingens namn och tryck på Retur.
1. Ange `userId` för partitionsnyckeln och tryck på Retur.
1. Bekräfta `1000` för den inledande dataflödeskapaciteten och tryck på Retur.
1. Expandera kontot i fönstret **Azure: Cosmos DB**. Den nya **Användardatabasen** och samlingen **WebCustomers** visas.

    ![Animering av ovanstående anvisningar som körs via Azure Cosmos DB-tillägget i Visual Studio Code.](../media/2-setup/vs-code-azure-cosmos-db-extension.gif)

Nu när du har ditt Azure Cosmos DB-konto kan du sätta igång i Visual Studio Code!
