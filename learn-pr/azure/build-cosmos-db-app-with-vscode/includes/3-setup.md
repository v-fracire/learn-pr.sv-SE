<span data-ttu-id="0c33e-101">I Visual Studio Code kan du skapa ett konsolprogram med hjälp av den integrerade terminalen och några korta kommandon.</span><span class="sxs-lookup"><span data-stu-id="0c33e-101">Visual Studio Code enables you to create a console application by using the integrated terminal and a few short commands.</span></span>

<span data-ttu-id="0c33e-102">I den här enheten kommer du skapa en enkel konsolapp med hjälp av den integrerade terminalen, hämta Azure Cosmos DB-anslutningssträngen från tillägget och konfigurera anslutningen från ditt program till Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0c33e-102">In this unit, you will create a simple console app using the integrated terminal, retrieve your Azure Cosmos DB connection string from the extension, and then configure the connection from your application to Azure Cosmos DB.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="0c33e-103">Skapa en konsolapp</span><span class="sxs-lookup"><span data-stu-id="0c33e-103">Create a console app</span></span>

1. <span data-ttu-id="0c33e-104">I Visual Studio Code väljer du **Arkiv** > **Öppna fil**.</span><span class="sxs-lookup"><span data-stu-id="0c33e-104">In Visual Studio Code, select **File** > **Open Folder**.</span></span>

1. <span data-ttu-id="0c33e-105">Skapa en ny mapp med namnet `learning-module` på önskad plats och klicka sedan på **Välj mapp**.</span><span class="sxs-lookup"><span data-stu-id="0c33e-105">Create a new folder named `learning-module` in the location of your choice, and then click **Select Folder**.</span></span>

1. <span data-ttu-id="0c33e-106">Se till att filer sparas automatiskt genom att klicka på Arkiv-menyn och markera **Spara automatiskt** om det här alternativet är tomt.</span><span class="sxs-lookup"><span data-stu-id="0c33e-106">Ensure that file auto-save is enabled by clicking on the File menu and checking **Auto Save** if it is blank.</span></span> <span data-ttu-id="0c33e-107">Du kommer att kopiera in flera kodblock så att du alltid använder de senaste versionerna av dina filer.</span><span class="sxs-lookup"><span data-stu-id="0c33e-107">You will be copying in several blocks of code, and this will ensure you are always operating against the latest edits of your files.</span></span>

1. <span data-ttu-id="0c33e-108">Öppna den integrerade terminalen från Visual Studio Code genom att välja **Visa** > **Terminal** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="0c33e-108">Open the integrated terminal from Visual Studio Code by selecting **View** > **Terminal** from the main menu.</span></span>

1. <span data-ttu-id="0c33e-109">Kopiera och klistra in följande kommando i terminalfönstret.</span><span class="sxs-lookup"><span data-stu-id="0c33e-109">In the terminal window, copy and paste the following command.</span></span>

    ```bash
    dotnet new console
    ```

    <span data-ttu-id="0c33e-110">Med det här kommandot skapar en **Program.cs**-fil i mappen med ett enkelt ”Hello World”-program som redan har skrivits, samt en C#-projektfil med namnet **learning-module.csproj**.</span><span class="sxs-lookup"><span data-stu-id="0c33e-110">This command creates a **Program.cs** file in your folder with a simple "Hello World" program already written, along with a C# project file named **learning-module.csproj**.</span></span>

1. <span data-ttu-id="0c33e-111">Kopiera och klistra in följande kommando i terminalfönstret för att köra ”Hello World”-programmet.</span><span class="sxs-lookup"><span data-stu-id="0c33e-111">In the terminal window, copy and paste the following command to run the "Hello World" program.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="0c33e-112">”Hello world!” visas i terminalfönstret</span><span class="sxs-lookup"><span data-stu-id="0c33e-112">The terminal window displays "Hello world!"</span></span> <span data-ttu-id="0c33e-113">som resultat.</span><span class="sxs-lookup"><span data-stu-id="0c33e-113">as output.</span></span>

## <a name="connect-the-app-to-azure-cosmos-db"></a><span data-ttu-id="0c33e-114">Ansluta appen till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0c33e-114">Connect the app to Azure Cosmos DB</span></span>

1. <span data-ttu-id="0c33e-115">När terminalen uppmanar dig ska du kopiera och klistra in följande kommandoblock för att installera de nödvändiga NuGet-paketen.</span><span class="sxs-lookup"><span data-stu-id="0c33e-115">At the terminal prompt, copy and paste the following command block to install the required NuGet packages.</span></span>

    ```bash
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package System.Configuration.ConfigurationManager
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet restore
    ```

1. <span data-ttu-id="0c33e-116">Klicka på **Program.cs** högst upp i Utforskaren för att öppna filen.</span><span class="sxs-lookup"><span data-stu-id="0c33e-116">At the top of the Explorer pane, click **Program.cs** to open the file.</span></span>

1. <span data-ttu-id="0c33e-117">Lägg till följande using-uttryck efter `using System;`.</span><span class="sxs-lookup"><span data-stu-id="0c33e-117">Add the following using statements after `using System;`.</span></span>

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

    <span data-ttu-id="0c33e-118">Om du får ett meddelande om att lägga till resurser som krävs saknas klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="0c33e-118">If you get a message about adding required missing assets, click **Yes**.</span></span>

1. <span data-ttu-id="0c33e-119">Skapa en ny fil med namnet App.config i mappen learning-module och lägg till följande kod.</span><span class="sxs-lookup"><span data-stu-id="0c33e-119">Create a new file named App.config in the learning-module folder, and add the following code.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. <span data-ttu-id="0c33e-120">Kopiera anslutningssträngen genom att klicka på ![Azure-ikon](../media/2-setup/visual-studio-code-explorer-icon.png) Azure-ikonen till vänster, expandera Concierge-prenumerationen, högerklicka på ditt nya Azure Cosmos DB-konto och klicka sedan på **kopiera anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="0c33e-120">Copy your connection string by clicking the ![Azure icon](../media/2-setup/visual-studio-code-explorer-icon.png) Azure icon on the left, expanding your Concierge Subscription, right-clicking your new Azure Cosmos DB account, and then clicking **Copy Connection String**.</span></span>

1. <span data-ttu-id="0c33e-121">Klistra in anslutningssträngen i slutet på filen App-config, kopiera sedan **AccountEndpoint**-delen från anslutningssträngen och klistra in den i **accountEndpoint** i App.config.</span><span class="sxs-lookup"><span data-stu-id="0c33e-121">Paste the connection string into the end of the App.config file, and then copy the **AccountEndpoint** portion from the connection string into the **accountEndpoint** value in App.config.</span></span>

    <span data-ttu-id="0c33e-122">accountEndpoint ska se ut som följande kod:</span><span class="sxs-lookup"><span data-stu-id="0c33e-122">The accountEndpoint should look like the following code:</span></span>

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. <span data-ttu-id="0c33e-123">Kopiera värdet **AccountKey** från anslutningssträngen i värdet **accountKey** och ta sedan bort den ursprungliga anslutningssträngen som du klistrade in.</span><span class="sxs-lookup"><span data-stu-id="0c33e-123">Now copy the **AccountKey** value from the connection string into the **accountKey** value, and then delete the original connection string you copied in.</span></span>

1. <span data-ttu-id="0c33e-124">Kopiera och klistra in följande kommando i terminalfönstret för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="0c33e-124">At the terminal prompt, copy and paste the following command to run the program.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="0c33e-125">Programmet ”Hello World!” visas</span><span class="sxs-lookup"><span data-stu-id="0c33e-125">The program displays Hello World!</span></span> <span data-ttu-id="0c33e-126">i terminalen.</span><span class="sxs-lookup"><span data-stu-id="0c33e-126">in the terminal.</span></span>

## <a name="create-the-documentclient"></a><span data-ttu-id="0c33e-127">Skapa DocumentClient</span><span class="sxs-lookup"><span data-stu-id="0c33e-127">Create the DocumentClient</span></span>

<span data-ttu-id="0c33e-128">Nu är det dags att skapa en instans av DocumentClient, vilket är en klientdelrepresentation av Azure Cosmos DB-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0c33e-128">Now it's time to create an instance of the DocumentClient, which is the client-side representation of the Azure Cosmos DB service.</span></span> <span data-ttu-id="0c33e-129">Den här klienten används för att konfigurera och köra förfrågningar till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0c33e-129">This client is used to configure and execute requests against the service.</span></span>

1. <span data-ttu-id="0c33e-130">I Program.cs, lägger du till följande i början av klassen Program.</span><span class="sxs-lookup"><span data-stu-id="0c33e-130">In Program.cs, add the following to the beginning of the Program class.</span></span>

    ```csharp
    private DocumentClient client;
    ```

1. <span data-ttu-id="0c33e-131">Lägg till en ny asynkron uppgift för att skapa en ny klient och kontrollera om användardatabasen finns genom att lägga till följande metod efter metoden `Main`.</span><span class="sxs-lookup"><span data-stu-id="0c33e-131">Add a new asynchronous task to create a new client, and check whether the Users database exists by adding the following method after the `Main` method.</span></span>

    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["accountEndpoint"]), ConfigurationManager.AppSettings["accountKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });

        Console.WriteLine("Database and collection validation complete");
    }
    ```

1. <span data-ttu-id="0c33e-132">Återigen, kopiera och klistra in följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="0c33e-132">In the integrated terminal, again, copy and paste the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="0c33e-133">Kopiera och klistra in följande kod i **huvudmetoden** så att den befintliga raden `Console.WriteLine("Hello World!");` överskrivs.</span><span class="sxs-lookup"><span data-stu-id="0c33e-133">Copy and paste the following code into the **Main** method, overwriting the current `Console.WriteLine("Hello World!");` line.</span></span>

    ```csharp
    try
    {
        Program p = new Program();
        p.BasicOperations().Wait();
    }
    catch (DocumentClientException de)
    {
        Exception baseException = de.GetBaseException();
        Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
    }
    catch (Exception e)
    {
        Exception baseException = e.GetBaseException();
        Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
    }
    finally
    {
        Console.WriteLine("End of demo, press any key to exit.");
        Console.ReadKey();
    }
    ```

1. <span data-ttu-id="0c33e-134">Återigen, ange följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="0c33e-134">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="0c33e-135">Konsolen visar följande resultat.</span><span class="sxs-lookup"><span data-stu-id="0c33e-135">The console displays the following output.</span></span>

    ```output
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

<span data-ttu-id="0c33e-136">I den här kursdelen har du skapat grunden till ditt Azure Cosmos DB-program.</span><span class="sxs-lookup"><span data-stu-id="0c33e-136">In this unit, you set up the groundwork for your Azure Cosmos DB application.</span></span> <span data-ttu-id="0c33e-137">Du har konfigurerat din utvecklingsmiljö i Visual Studio Code, skapat ett enkelt HelloWorld-projekt, anslutit projektet till Azure Cosmos DB-slutpunkten och kontrollerat att din databas och samling finns.</span><span class="sxs-lookup"><span data-stu-id="0c33e-137">You set up your development environment in Visual Studio Code, created a simple HelloWorld project, connected the project to the Azure Cosmos DB endpoint, and ensured your database and collection exist.</span></span>
