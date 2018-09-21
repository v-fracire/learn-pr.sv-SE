I Visual Studio Code kan du skapa ett konsolprogram med hjälp av den integrerade terminalen och några korta kommandon.

I den här enheten kommer du skapa en enkel konsolapp med hjälp av den integrerade terminalen, hämta Azure Cosmos DB-anslutningssträngen från tillägget och konfigurera anslutningen från ditt program till Azure Cosmos DB.

## <a name="create-a-console-app"></a>Skapa en konsolapp

1. I Visual Studio Code väljer du **Arkiv** > **Öppna fil**.

1. Skapa en ny mapp med namnet `learning-module` på önskad plats och klicka sedan på **Välj mapp**.

1. Se till att filer sparas automatiskt genom att klicka på Arkiv-menyn och markera **Spara automatiskt** om det här alternativet är tomt. Du kommer att kopiera in flera kodblock så att du alltid använder de senaste versionerna av dina filer.

1. Öppna den integrerade terminalen från Visual Studio Code genom att välja **Visa** > **Terminal** på huvudmenyn.

1. Kopiera och klistra in följande kommando i terminalfönstret.

    ```bash
    dotnet new console
    ```

    Med det här kommandot skapar en **Program.cs**-fil i mappen med ett enkelt ”Hello World”-program som redan har skrivits, samt en C#-projektfil med namnet **learning-module.csproj**.

1. Kopiera och klistra in följande kommando i terminalfönstret för att köra ”Hello World”-programmet.

    ```bash
    dotnet run
    ```

    ”Hello world!” visas i terminalfönstret som resultat.

## <a name="connect-the-app-to-azure-cosmos-db"></a>Ansluta appen till Azure Cosmos DB

1. När terminalen uppmanar dig ska du kopiera och klistra in följande kommandoblock för att installera de nödvändiga NuGet-paketen.

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

1. Klicka på **Program.cs** högst upp i Utforskaren för att öppna filen.

1. Lägg till följande using-uttryck efter `using System;`.

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

    Om du får ett meddelande om att lägga till resurser som krävs saknas klickar du på **Ja**.

1. Skapa en ny fil med namnet App.config i mappen learning-module och lägg till följande kod.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. Kopiera anslutningssträngen genom att klicka på ![Azure-ikon](../media/2-setup/visual-studio-code-explorer-icon.png) Azure-ikonen till vänster, expandera Concierge-prenumerationen, högerklicka på ditt nya Azure Cosmos DB-konto och klicka sedan på **kopiera anslutningssträngen**.

1. Klistra in anslutningssträngen i slutet på filen App-config, kopiera sedan **AccountEndpoint**-delen från anslutningssträngen och klistra in den i **accountEndpoint** i App.config.

    accountEndpoint ska se ut som följande kod:

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. Kopiera värdet **AccountKey** från anslutningssträngen i värdet **accountKey** och ta sedan bort den ursprungliga anslutningssträngen som du klistrade in.

1. Kopiera och klistra in följande kommando i terminalfönstret för att köra programmet.

    ```csharp
    dotnet run
    ```

    Programmet ”Hello World!” visas i terminalen.

## <a name="create-the-documentclient"></a>Skapa DocumentClient

Nu är det dags att skapa en instans av DocumentClient, vilket är en klientdelrepresentation av Azure Cosmos DB-tjänsten. Den här klienten används för att konfigurera och köra förfrågningar till tjänsten.

1. I Program.cs, lägger du till följande i början av klassen Program.

    ```csharp
    private DocumentClient client;
    ```

1. Lägg till en ny asynkron uppgift för att skapa en ny klient och kontrollera om användardatabasen finns genom att lägga till följande metod efter metoden `Main`.

    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["accountEndpoint"]), ConfigurationManager.AppSettings["accountKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });

        Console.WriteLine("Database and collection validation complete");
    }
    ```

1. Återigen, kopiera och klistra in följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.

    ```csharp
    dotnet run
    ```

1. Kopiera och klistra in följande kod i **huvudmetoden** så att den befintliga raden `Console.WriteLine("Hello World!");` överskrivs.

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

1. Återigen, ange följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.

    ```csharp
    dotnet run
    ```

    Konsolen visar följande resultat.

    ```output
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

I den här kursdelen har du skapat grunden till ditt Azure Cosmos DB-program. Du har konfigurerat din utvecklingsmiljö i Visual Studio Code, skapat ett enkelt HelloWorld-projekt, anslutit projektet till Azure Cosmos DB-slutpunkten och kontrollerat att din databas och samling finns.
