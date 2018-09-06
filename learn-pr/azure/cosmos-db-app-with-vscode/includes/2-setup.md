<span data-ttu-id="a6219-101">I den här modulen kommer du att skapa en enkel konsolapp med hjälp av den integrerade terminalen, installera NuGet-paket och använda Azure Cosmos DB-tillägget för att se databaser och samlingar som skapats i den föregående modulen.</span><span class="sxs-lookup"><span data-stu-id="a6219-101">In this module you will create a simple console app using the integrated terminal, install NuGet packages, and use the Azure Cosmos DB extension to see databases and collections created in the previous module.</span></span> <span data-ttu-id="a6219-102">Du hämtar Azure Cosmos DB-anslutningssträngen från tillägget och konfigurerar sedan anslutningen till Azure Cosmos DB i syfte att skapa en egen användardatabas.</span><span class="sxs-lookup"><span data-stu-id="a6219-102">You'll retrieve your Azure Cosmos DB connection string from the extension, and then start configure the connection to Azure Cosmos DB to create your User database.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="a6219-103">Skapa en konsolapp</span><span class="sxs-lookup"><span data-stu-id="a6219-103">Create a console app</span></span>

1. <span data-ttu-id="a6219-104">Öppna Visual Studio Code och välj **Arkiv** > **Öppna mapp**.</span><span class="sxs-lookup"><span data-stu-id="a6219-104">Open Visual Studio Code, and then select **File** > **Open Folder**.</span></span>

2. <span data-ttu-id="a6219-105">Skapa en ny mapp med namnet **learning-module** där det nya C#-projektet ska sparas och klicka på **Välj mapp**.</span><span class="sxs-lookup"><span data-stu-id="a6219-105">Create a new folder named **learning-module** where you want your new C# project to be, and then click **Select Folder**.</span></span>

2. <span data-ttu-id="a6219-106">Öppna den integrerade terminalen från Visual Studio Code genom att välja **Visa** > **Integrerad terminal** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="a6219-106">Open the integrated terminal from Visual Studio Code by selecting **View** > **Integrated Terminal** from the main menu.</span></span>

3. <span data-ttu-id="a6219-107">Ange **dotnet new console** i terminalfönstret.</span><span class="sxs-lookup"><span data-stu-id="a6219-107">In the terminal window, type **dotnet new console**.</span></span>

    <span data-ttu-id="a6219-108">Med det här kommandot skapas en **Program.cs**-fil i mappen med ett enkelt ”Hello World”-program som redan har skrivits, samt en C#-projektfil med namnet **learning module.csproj**.</span><span class="sxs-lookup"><span data-stu-id="a6219-108">This command creates a **Program.cs** file in your folder with a simple "Hello World" program already written, along with a C# project file named **learning-module.csproj**.</span></span>

4. <span data-ttu-id="a6219-109">Ange följande kommando i terminalfönstret för att köra ”Hello World”-programmet.</span><span class="sxs-lookup"><span data-stu-id="a6219-109">In the terminal window, type the following command to run the "Hello World" program.</span></span> 

    ```
    dotnet run
    ```

    <span data-ttu-id="a6219-110">”Hello world!” visas i terminalfönstret</span><span class="sxs-lookup"><span data-stu-id="a6219-110">The terminal window displays "Hello world!"</span></span> <span data-ttu-id="a6219-111">som resultat.</span><span class="sxs-lookup"><span data-stu-id="a6219-111">as output.</span></span>

## <a name="connect-the-app-to-azure-cosmos-db"></a><span data-ttu-id="a6219-112">Ansluta appen till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a6219-112">Connect the app to Azure Cosmos DB</span></span>

1. <span data-ttu-id="a6219-113">Logga in på Azure genom att klicka på **Visa** > **Kommandopalett** och ange **Azure: Logga in**.</span><span class="sxs-lookup"><span data-stu-id="a6219-113">Sign in to Azure by clicking **View** > **Command Palette** and typing **Azure: Sign In**.</span></span>

    <span data-ttu-id="a6219-114">Följ anvisningarna för att kopiera och klistra in koden i webbläsaren. Därmed autentiseras din Visual Studio Code-session.</span><span class="sxs-lookup"><span data-stu-id="a6219-114">Follow the prompts to copy and paste the code provided in the web browser, which authenticates your Visual Studio Code session.</span></span>

2. <span data-ttu-id="a6219-115">Klicka på ikonen ![Utforskaren](../media/2-setup/visual-studio-code-explorer-icon.png) **Utforskar**-ikonen på menyn till vänster och expandera sedan **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="a6219-115">Click the ![Explorer icon](../media/2-setup/visual-studio-code-explorer-icon.png) **Explorer** icon on the left menu, and then expand **Azure Cosmos DB**.</span></span>

3. <span data-ttu-id="a6219-116">Expandera Azure-prenumerationen > Azure Cosmos DB-kontot.</span><span class="sxs-lookup"><span data-stu-id="a6219-116">Expand your Azure subscription > Azure Cosmos DB account.</span></span> <span data-ttu-id="a6219-117">Om du har skapat databasen **Produkter** och samlingen **Kläder** i de föregående modulerna visas de i tillägget.</span><span class="sxs-lookup"><span data-stu-id="a6219-117">If you created the **Products** database and **Clothing** collection in the previous modules, the extension displays them.</span></span>

   ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/azure-cosmos-db-vs-code-extension.png) 

4. <span data-ttu-id="a6219-119">Nu skapar du en ny databas och samling för dina kunder.</span><span class="sxs-lookup"><span data-stu-id="a6219-119">Now let's create a new database and collection for your customers.</span></span>

    <span data-ttu-id="a6219-120">Högerklicka på ditt konto i Utforskaren och klicka sedan på **Skapa databas**.</span><span class="sxs-lookup"><span data-stu-id="a6219-120">In the Explorer window, right-click your account, and then click **Create Database**.</span></span> 
    
    <span data-ttu-id="a6219-121">I textrutan högst upp på skärmen anger du **Användare** som databasnamn > **retur** > **Webbkunder** som namn på samlingen > **retur** > **userId** som partitionsnyckel > **retur** > **1000** som inledande genomflödeskapacitet > **retur**.</span><span class="sxs-lookup"><span data-stu-id="a6219-121">In the text box at the top of the screen, type **Users** for the database name > **Enter** > **WebCustomers** for the collection name > **Enter** > **userId** for the partition key > **Enter** > **1000** for the initial throughput capacity > **Enter**.</span></span>

    <span data-ttu-id="a6219-122">![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing--></span><span class="sxs-lookup"><span data-stu-id="a6219-122">![Azure Cosmos DB Visual Studio Code extension](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing--></span></span>

    <span data-ttu-id="a6219-123">Den nya databasen Användare och samlingen Webbkunder visas i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="a6219-123">The new Users database and WebCustomers collection are displayed in the Explorer window.</span></span>

5. <span data-ttu-id="a6219-124">Kör följande kommandon i en ny prompt i den integrerade terminalen för att installera de nödvändiga NuGet-paketen.</span><span class="sxs-lookup"><span data-stu-id="a6219-124">In the integrated terminal, run each of the following commands at a new prompt to install the required NuGet packages.</span></span>

    ```
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet add package Bogus
    dotnet restore
    ```

6. <span data-ttu-id="a6219-125">Klicka på **Program.cs** högst upp i Utforskaren för att öppna filen.</span><span class="sxs-lookup"><span data-stu-id="a6219-125">At the top of the Explorer pane, click **Program.cs** to open the file.</span></span>

7. <span data-ttu-id="a6219-126">Lägg till följande using-uttryck efter `using System;`.</span><span class="sxs-lookup"><span data-stu-id="a6219-126">Add the following using statements after `using System;`.</span></span>

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

8. <span data-ttu-id="a6219-127">Skapa en ny fil med namnet App.config i mappen learning-module och lägg till följande kod.</span><span class="sxs-lookup"><span data-stu-id="a6219-127">Create a new file named App.config in the learning-module folder, and add the following code.</span></span>
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

9. <span data-ttu-id="a6219-128">Kopiera anslutningssträngen från Azure Cosmos DB-tillägget genom att högerklicka på learning-module-kontot och klicka på **Copy Connection String** (Kopiera anslutningssträng).</span><span class="sxs-lookup"><span data-stu-id="a6219-128">Copy your connection string from the Azure Cosmos DB extension by right-clicking the learning-module account, and clicking **Copy Connection String**.</span></span>

    ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/vs-code-copy-connection-string.gif) 

10. <span data-ttu-id="a6219-130">Klistra in anslutningssträngen i en textfil, kopiera sedan **AccountEndpoint**-delen från textfilen och klistra in den i **accountEndpoint** i App.config.</span><span class="sxs-lookup"><span data-stu-id="a6219-130">Paste the connection string into a text file, and then copy the **AccountEndpoint** portion from the text file into the **accountEndpoint** in App.config.</span></span>

    <span data-ttu-id="a6219-131">accountEndpoint ska se ut som följande kod:</span><span class="sxs-lookup"><span data-stu-id="a6219-131">The accountEndpoint should look like the following code:</span></span>

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

12. <span data-ttu-id="a6219-132">Kopiera sedan **AccountKey**-värdet från textvärdet och klistra in det i **accountKey**-värdet i App.config.</span><span class="sxs-lookup"><span data-stu-id="a6219-132">Now copy the **AccountKey** value from the text value into the **accountKey** value in App.config.</span></span>

12. <span data-ttu-id="a6219-133">Ange följande kommando för att köra programmet i den integrerade terminalen och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="a6219-133">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

13. <span data-ttu-id="a6219-134">Lägg till en ny asynkron uppgift för att skapa en ny klient och kontrollera om databasen Användare finns genom att lägga till följande metod efter huvudmetoden.</span><span class="sxs-lookup"><span data-stu-id="a6219-134">Add a new asynchronous task to create a new client, and check whether the Users database exists by adding the following method after the main method.</span></span>
    
    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpointUrl"]), ConfigurationManager.AppSettings["primaryKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });
    }
    ```

14. <span data-ttu-id="a6219-135">Återigen, ange följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="a6219-135">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

15. <span data-ttu-id="a6219-136">Kopiera och klistra in följande kod i **huvudmetoden** så att den befintliga raden `Console.WriteLine("Hello World!");` överskrivs.</span><span class="sxs-lookup"><span data-stu-id="a6219-136">Copy and paste the following code into the **Main** method, overwriting the current `Console.WriteLine("Hello World!");` line.</span></span>

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

16. <span data-ttu-id="a6219-137">Återigen, ange följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="a6219-137">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="a6219-138">Konsolen visar följande resultat.</span><span class="sxs-lookup"><span data-stu-id="a6219-138">The console displays the following output.</span></span>
    
    ```
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

## <a name="summary"></a><span data-ttu-id="a6219-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a6219-139">Summary</span></span>

<span data-ttu-id="a6219-140">I den här modulen har du skapat grunden till ditt Azure Cosmos DB-program.</span><span class="sxs-lookup"><span data-stu-id="a6219-140">In this module, you set up the groundwork for your Azure Cosmos DB application.</span></span> <span data-ttu-id="a6219-141">Du har konfigurerat din utvecklingsmiljö i Visual Studio Code, skapat ett enkelt HelloWorld-projekt, anslutit projektet till Azure Cosmos DB-slutpunkten och kontrollerat att din databas och samling finns.</span><span class="sxs-lookup"><span data-stu-id="a6219-141">You set up your development environment in Visual Studio Code, created a simple HelloWorld project, connected the project to the Azure Cosmos DB endpoint, and ensured your database and collection exist.</span></span>