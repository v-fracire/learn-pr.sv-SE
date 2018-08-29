I den här modulen kommer du att skapa en enkel konsolapp med hjälp av den integrerade terminalen, installera NuGet-paket och använda Azure Cosmos DB-tillägget för att se databaser och samlingar som skapats i den föregående modulen. Du hämtar Azure Cosmos DB-anslutningssträngen från tillägget och börjar sedan att utveckla mot Azure Cosmos DB. 

## <a name="create-a-console-app"></a>Skapa en konsolapp

1. Öppna Visual Studio Code och välj **Arkiv** > **Öppna mapp**.

2. Skapa en ny mapp med namnet **learning-module** där det nya C#-projektet ska sparas och klicka på **Välj mapp**.

2. Öppna den integrerade terminalen från Visual Studio Code genom att välja **Visa** > **Integrerad terminal** på huvudmenyn.

3. Ange **dotnet new console** i terminalfönstret.

    Med det här kommandot skapas en **Program.cs**-fil i mappen med ett enkelt ”Hello World”-program som redan har skrivits, samt en C#-projektfil med namnet **learning module.csproj**.

4. Ange följande kommando i terminalfönstret för att köra ”Hello World”-programmet. 

    ```
    dotnet run
    ```

    ”Hello world!” visas i terminalfönstret som resultat.

## <a name="connect-the-app-to-azure-cosmos-db"></a>Ansluta appen till Azure Cosmos DB

1. Logga in på Azure genom att klicka på **Visa** > **Kommandopalett** och ange **Azure: Logga in**.

    Följ anvisningarna för att kopiera och klistra in koden i webbläsaren. Därmed autentiseras din Visual Studio Code-session.

2. Klicka på ikonen ![Utforskaren](../media/2-setup/visual-studio-code-explorer-icon.png) **Utforskar**-ikonen på menyn till vänster och expandera sedan **Azure Cosmos DB**.

3. Expandera Azure-prenumerationen > Azure Cosmos DB-kontot. Om du har skapat databasen **Produkter** och samlingen **Kläder** i de föregående modulerna visas de i tillägget.

   ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/azure-cosmos-db-vs-code-extension.png) 

4. Nu skapar du en ny databas och samling för dina kunder.

    Högerklicka på ditt konto i Utforskaren och klicka sedan på **Skapa databas**. 
    
    I textrutan högst upp på skärmen anger du **Användare** som databasnamn > **retur** > **Webbkunder** som namn på samlingen > **retur** > **userId** som partitionsnyckel > **retur** > **1000** som inledande genomflödeskapacitet > **retur**.

    ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing-->

    Den nya databasen Användare och samlingen Webbkunder visas i Utforskaren.

5. Kör följande kommandon i en ny prompt i den integrerade terminalen för att installera de nödvändiga NuGet-paketen.

    ```
    dotnet add package System.Net.Http
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet add package Bogus
    dotnet restore
    ```

6. Klicka på **Program.cs** högst upp i Utforskaren för att öppna filen.

7. Lägg till följande using-uttryck efter `using System;`.

    ```csharp
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

8. Lägg till följande två konstanter och din *klientvariabel* i programklassen:

    ```csharp
    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;
    ```

    <!--TODO: Use more secure method-->

9. Kopiera anslutningssträngen från Azure Cosmos DB-tillägget genom att högerklicka på utbildningsmodulkontot och klicka på **Copy Connection String** (Kopiera anslutningssträng).

    ![Visual Studio Code-tillägg för Azure Cosmos DB](../media/2-setup/vs-code-copy-connection-string.gif) 

10. Klistra in anslutningssträngen i en textfil och kopiera sedan **AccountEndpoint**-delen från textfilen till **EndpointUrl** i Program.cs.

    AccountEndpoint ska se ut som följande kod:

    ```csharp
    private const string EndpointUrl = "https://<account name>.documents.azure.com:443/;
    ```

12. Kopiera sedan **AccountKey**-värdet från textvärdet till **PrimaryKey**-värdet i Program.cs.

12. Ange följande kommando för att köra programmet i den integrerade terminalen och kontrollera att det körs.

    ```csharp
    dotnet run
    ```

13. Lägg till en ny asynkron uppgift för att skapa en ny klient och kontrollera om databasen Användare finns genom att lägga till följande metod efter huvudmetoden.
    
    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });
    }
    ```

14. Återigen, ange följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.

    ```csharp
    dotnet run
    ```

15. Kopiera och klistra in följande kod i **huvudmetoden** så att den befintliga raden `Console.WriteLine("Hello World!");` överskrivs.

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

16. Återigen, ange följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.

    ```csharp
    dotnet run
    ```

    Konsolen visar följande resultat.
    
    ```
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

## <a name="summary"></a>Sammanfattning

I den här modulen har du skapat grunden till ditt Azure Cosmos DB-program. Du har konfigurerat din utvecklingsmiljö i Visual Studio Code, skapat ett enkelt HelloWorld-projekt, anslutit projektet till Azure Cosmos DB-slutpunkten och kontrollerat att din databas och samling finns.