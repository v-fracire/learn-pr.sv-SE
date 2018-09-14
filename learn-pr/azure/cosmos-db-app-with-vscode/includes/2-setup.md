<span data-ttu-id="6c463-101">I den här modulen kommer du att skapa en enkel konsolapp med hjälp av den integrerade terminalen, installera NuGet-paket och använda Azure Cosmos DB-tillägget för att se databaser och samlingar som skapats i den föregående modulen.</span><span class="sxs-lookup"><span data-stu-id="6c463-101">In this module you will create a simple console app using the integrated terminal, install NuGet packages, and use the Azure Cosmos DB extension to see databases and collections created in the previous module.</span></span> <span data-ttu-id="6c463-102">Du hämtar Azure Cosmos DB-anslutningssträngen från tillägget och konfigurerar sedan anslutningen till Azure Cosmos DB i syfte att skapa en egen användardatabas.</span><span class="sxs-lookup"><span data-stu-id="6c463-102">You'll retrieve your Azure Cosmos DB connection string from the extension, and then start configure the connection to Azure Cosmos DB to create your User database.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="6c463-103">Skapa en konsolapp</span><span class="sxs-lookup"><span data-stu-id="6c463-103">Create a console app</span></span>

1. <span data-ttu-id="6c463-104">Skapa en mapp där du tänker arbeta.</span><span class="sxs-lookup"><span data-stu-id="6c463-104">Create a folder where you will be working.</span></span>

1. <span data-ttu-id="6c463-105">Öppna en kommandotolk och navigera till mappen.</span><span class="sxs-lookup"><span data-stu-id="6c463-105">Open a command prompt and navigate into the folder.</span></span>

1. <span data-ttu-id="6c463-106">Skapa ett nytt .NET Core-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="6c463-106">Create a new .NET Core console application</span></span>

```bash
dotnet new console 
```

1. <span data-ttu-id="6c463-107">Öppna Visual Studio Code och välj **Arkiv** > **Öppna mapp**.</span><span class="sxs-lookup"><span data-stu-id="6c463-107">Open Visual Studio Code, and then select **File** > **Open Folder**.</span></span>

1. <span data-ttu-id="6c463-108">Skapa en ny mapp där det nya C#-projektet ska sparas. Klicka sedan på **Välj mapp**.</span><span class="sxs-lookup"><span data-stu-id="6c463-108">Create a new folder where you want your new C# project to be, and then click **Select Folder**.</span></span>

1. <span data-ttu-id="6c463-109">Se till att filer sparas automatiskt genom att klicka på Arkiv-menyn och kontrollera att Spara Automatisk är aktiverat och inte är tomt.</span><span class="sxs-lookup"><span data-stu-id="6c463-109">Ensure that file auto save is enabled by clicking on the File menu and checking Auto Save if it is blank.</span></span>

1. <span data-ttu-id="6c463-110">Öppna den integrerade terminalen från Visual Studio Code genom att välja **Visa** > **Integrated Terminal** (Integrerad terminal) på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c463-110">Open the integrated terminal from Visual Studio Code by selecting **View** > **Integrated Terminal** from the main menu.</span></span>

1. <span data-ttu-id="6c463-111">Ange **dotnet new console** i terminalfönstret.</span><span class="sxs-lookup"><span data-stu-id="6c463-111">In the terminal window, type **dotnet new console**.</span></span>

    <span data-ttu-id="6c463-112">Med det här kommandot skapas en **Program.cs**-fil i mappen med ett enkelt ”Hello World”-program som redan har skrivits, samt en C#-projektfil med namnet **learning module.csproj**.</span><span class="sxs-lookup"><span data-stu-id="6c463-112">This command creates a **Program.cs** file in your folder with a simple "Hello World" program already written, along with a C# project file named **learning-module.csproj**.</span></span>

1. <span data-ttu-id="6c463-113">Ange följande kommando i terminalfönstret för att köra ”Hello World”-programmet.</span><span class="sxs-lookup"><span data-stu-id="6c463-113">In the terminal window, type the following command to run the "Hello World" program.</span></span> 

    ```
    dotnet run
    ```

    <span data-ttu-id="6c463-114">”Hello world!” visas i terminalfönstret</span><span class="sxs-lookup"><span data-stu-id="6c463-114">The terminal window displays "Hello world!"</span></span> <span data-ttu-id="6c463-115">som resultat.</span><span class="sxs-lookup"><span data-stu-id="6c463-115">as output.</span></span>

## <a name="connect-the-app-to-azure-cosmos-db"></a><span data-ttu-id="6c463-116">Ansluta appen till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6c463-116">Connect the app to Azure Cosmos DB</span></span>

1. <span data-ttu-id="6c463-117">Logga in på Azure genom att klicka på **Visa** > **Kommandopalett** och ange **Azure: Logga in**.</span><span class="sxs-lookup"><span data-stu-id="6c463-117">Sign in to Azure by clicking **View** > **Command Palette** and typing **Azure: Sign In**.</span></span>

    <span data-ttu-id="6c463-118">Följ anvisningarna för att kopiera och klistra in koden i webbläsaren. Därmed autentiseras din Visual Studio Code-session.</span><span class="sxs-lookup"><span data-stu-id="6c463-118">Follow the prompts to copy and paste the code provided in the web browser, which authenticates your Visual Studio Code session.</span></span>

1. <span data-ttu-id="6c463-119">Klicka på ikonen ![Utforskaren](../media/2-setup/visual-studio-code-explorer-icon.png) **Utforskar**-ikonen på menyn till vänster och expandera sedan **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="6c463-119">Click the ![Explorer icon](../media/2-setup/visual-studio-code-explorer-icon.png) **Explorer** icon on the left menu, and then expand **Azure Cosmos DB**.</span></span>

1. <span data-ttu-id="6c463-120">Expandera Azure-prenumerationen > Azure Cosmos DB-kontot.</span><span class="sxs-lookup"><span data-stu-id="6c463-120">Expand your Azure subscription > Azure Cosmos DB account.</span></span> <span data-ttu-id="6c463-121">Om du har skapat databasen **Produkter** och samlingen **Kläder** i de föregående modulerna visas de i tillägget.</span><span class="sxs-lookup"><span data-stu-id="6c463-121">If you created the **Products** database and **Clothing** collection in the previous modules, the extension displays them.</span></span>

   ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/azure-cosmos-db-vs-code-extension.png) 

1. <span data-ttu-id="6c463-123">Nu skapar du en ny databas och samling för dina kunder.</span><span class="sxs-lookup"><span data-stu-id="6c463-123">Now let's create a new database and collection for your customers.</span></span>

    <span data-ttu-id="6c463-124">Högerklicka på ditt konto i Utforskaren och klicka sedan på **Skapa databas**.</span><span class="sxs-lookup"><span data-stu-id="6c463-124">In the Explorer window, right-click your account, and then click **Create Database**.</span></span> 
    
    <span data-ttu-id="6c463-125">I textrutan högst upp på skärmen anger du **Användare** som databasnamn > **retur** > **Webbkunder** som namn på samlingen > **retur** > **userId** som partitionsnyckel > **retur** > **1000** som inledande genomflödeskapacitet > **retur**.</span><span class="sxs-lookup"><span data-stu-id="6c463-125">In the text box at the top of the screen, type **Users** for the database name > **Enter** > **WebCustomers** for the collection name > **Enter** > **userId** for the partition key > **Enter** > **1000** for the initial throughput capacity > **Enter**.</span></span>

    <span data-ttu-id="6c463-126">![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing--></span><span class="sxs-lookup"><span data-stu-id="6c463-126">![Azure Cosmos DB Visual Studio Code extension](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing--></span></span>

    <span data-ttu-id="6c463-127">Den nya databasen Användare och samlingen Webbkunder visas i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="6c463-127">The new Users database and WebCustomers collection are displayed in the Explorer window.</span></span>

1. <span data-ttu-id="6c463-128">Kör följande kommandon i en ny prompt i den integrerade terminalen för att installera de nödvändiga NuGet-paketen.</span><span class="sxs-lookup"><span data-stu-id="6c463-128">In the integrated terminal, run each of the following commands at a new prompt to install the required NuGet packages.</span></span>

    ```
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package System.Configuration.ConfigurationManager
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet add package Bogus
    dotnet restore
    ```

1. <span data-ttu-id="6c463-129">Klicka på **Program.cs** högst upp i Utforskaren för att öppna filen.</span><span class="sxs-lookup"><span data-stu-id="6c463-129">At the top of the Explorer pane, click **Program.cs** to open the file.</span></span>

1. <span data-ttu-id="6c463-130">Lägg till följande using-uttryck efter `using System;`.</span><span class="sxs-lookup"><span data-stu-id="6c463-130">Add the following using statements after `using System;`.</span></span>

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="6c463-131">Skapa en ny fil med namnet App.config i mappen learning-module och lägg till följande kod.</span><span class="sxs-lookup"><span data-stu-id="6c463-131">Create a new file named App.config in the learning-module folder, and add the following code.</span></span>
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. <span data-ttu-id="6c463-132">Kopiera anslutningssträngen från Azure Cosmos DB-tillägget genom att högerklicka på learning-module-kontot och klicka på **Copy Connection String** (Kopiera anslutningssträng).</span><span class="sxs-lookup"><span data-stu-id="6c463-132">Copy your connection string from the Azure Cosmos DB extension by right-clicking the learning-module account, and clicking **Copy Connection String**.</span></span>

    ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/vs-code-copy-connection-string.gif) 

1. <span data-ttu-id="6c463-134">Klistra in anslutningssträngen i en textfil, kopiera sedan **AccountEndpoint**-delen från textfilen och klistra in den i **accountEndpoint** i App.config.</span><span class="sxs-lookup"><span data-stu-id="6c463-134">Paste the connection string into a text file, and then copy the **AccountEndpoint** portion from the text file into the **accountEndpoint** in App.config.</span></span>

    <span data-ttu-id="6c463-135">accountEndpoint ska se ut som följande kod:</span><span class="sxs-lookup"><span data-stu-id="6c463-135">The accountEndpoint should look like the following code:</span></span>

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. <span data-ttu-id="6c463-136">Kopiera sedan **AccountKey**-värdet från textvärdet och klistra in det i **accountKey**-värdet i App.config.</span><span class="sxs-lookup"><span data-stu-id="6c463-136">Now copy the **AccountKey** value from the text value into the **accountKey** value in App.config.</span></span>

1. <span data-ttu-id="6c463-137">Ange följande kommando för att köra programmet i den integrerade terminalen och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="6c463-137">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="6c463-138">Lägg till en ny asynkron uppgift för att skapa en ny klient och kontrollera om databasen Användare finns genom att lägga till följande metod efter huvudmetoden.</span><span class="sxs-lookup"><span data-stu-id="6c463-138">Add a new asynchronous task to create a new client, and check whether the Users database exists by adding the following method after the main method.</span></span>
    
    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpointUrl"]), ConfigurationManager.AppSettings["primaryKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });
    }
    ```

1. <span data-ttu-id="6c463-139">Återigen, ange följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="6c463-139">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="6c463-140">Kopiera och klistra in följande kod i **huvudmetoden** så att den befintliga raden `Console.WriteLine("Hello World!");` överskrivs.</span><span class="sxs-lookup"><span data-stu-id="6c463-140">Copy and paste the following code into the **Main** method, overwriting the current `Console.WriteLine("Hello World!");` line.</span></span>

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

1. <span data-ttu-id="6c463-141">Återigen, ange följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="6c463-141">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="6c463-142">Konsolen visar följande resultat.</span><span class="sxs-lookup"><span data-stu-id="6c463-142">The console displays the following output.</span></span>
    
    ```
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

## <a name="summary"></a><span data-ttu-id="6c463-143">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6c463-143">Summary</span></span>

<span data-ttu-id="6c463-144">I den här kursdelen har du skapat grunden till ditt Azure Cosmos DB-program.</span><span class="sxs-lookup"><span data-stu-id="6c463-144">In this unit, you set up the groundwork for your Azure Cosmos DB application.</span></span> <span data-ttu-id="6c463-145">Du har konfigurerat din utvecklingsmiljö i Visual Studio Code, skapat ett enkelt HelloWorld-projekt, anslutit projektet till Azure Cosmos DB-slutpunkten och kontrollerat att din databas och samling finns.</span><span class="sxs-lookup"><span data-stu-id="6c463-145">You set up your development environment in Visual Studio Code, created a simple HelloWorld project, connected the project to the Azure Cosmos DB endpoint, and ensured your database and collection exist.</span></span>