I den här övningen ska du hämta åtkomstnyckeln och lägga till den i appkonfigurationen. Eftersom vi har skapat ett .NET Core-konsolprogram måste vi också lägga till stöd för läsning från konfigurationsfiler.

## <a name="create-a-json-configuration-file"></a>Skapa en JSON-konfigurationsfil

1. Högerklicka på Visual Studio-projektet som vi skapade i den föregående övningen, välj **Lägg till**och välj sedan **Nytt objekt** (eller håll ned Ctrl+Skift+ A).
1. En modal dialogruta visas. Välj **JavaScript JSON Configuration File** och skriv **appsettings.json** i textfältet *Namn* och klicka på **Lägg till** 
   ![appsettings](..\media-draft\9-appsettings.png)
1. En fil läggs till i projektet med ett innehåll som liknar följande:
    ```json
    {
      "exclude": [
        "**/bin",
        "**/bower_components",
        "**/jspm_packages",
        "**/node_modules",
        "**/obj",
        "**/platforms"
      ]
    }
    ```
1. Ta bort posten **exclude** (undanta) och det associerade innehållet. Ersätt detta med en enda post, **StorageAccountConnectionString**, med ett tomt strängvärde. Det bör se ut ungefär så här:
    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. Välj filen **appsettings.json**, högerklicka på den och välj **Egenskaper** (du kan också trycka på Alt+Retur eller F4).
1. Ändra **Copy to Output Directory** (Kopiera till utdatakatalog) till **Copy if newer**
  ![build action](..\media-draft\10-build-action.png) (Kopiera om nyare).

  > Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.

## <a name="add-support-to-read-a-json-configuration-file"></a>Lägga till stöd för läsning från en JSON-konfigurationsfil

Ett .NET Core-konsolprogram kräver att vissa bibliotek läggs till för att det ska kunna läsa från en JSON-konfigurationsfil utan problem.

1. Högerklicka på projektet och välj **Hantera Nuget-paket …**
  ![Hantera NuGet-paket](..\media-draft\11-manage-nuget-packages.png).
1. Då visas en sida med de Nuget-paket som är installerade för tillfället. Klicka på **Bläddra** och skriv **Microsoft.Extensions.Configuration.Json** i sökfältet. Paketet **Microsoft.Extensions.Configuration.Json** visas i sökresultaten.
1. Välj paketet **Microsoft.Extensions.Configuration.Json** och välj **Installera** i den högra rutan.
1. Dialogrutan **Förhandsgranska ändringar** visas. Klicka på **OK**
1. En dialogruta för godkännande av licens visas. Klicka på **Jag accepterar**.

## <a name="read-from-the-configuration-file"></a>Läsa från konfigurationsfilen

Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.

1. Leta upp filen **Program.cs** i projektet och öppna den i Visual Studio.
1. Överst i filen står det **using System;**. Lägg till följande rader med kod under den raden:
    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```
1. Ersätt innehållet i **huvudmetoden** med följande kod:
    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```
1. Filen **Program.cs** bör se ut så här nu:
    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;

    namespace DemoConsoleApp
    {
        class Program
        {
            static void Main(string[] args)
            {
                var builder = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile("appsettings.json");

                var configuration = builder.Build();
            }
        }
    }
    ```

> Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.

## <a name="add-your-storage-connection-string-to-the-configuration-file"></a>Lägga till anslutningssträngen för lagringskontot i konfigurationsfilen

Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.

1. Gå till Azure-portalen för den prenumeration som innehåller ditt lagringskonto.
1. Navigera till ditt lagringskonto.
1. Navigera till bladet Kontonycklar för lagringskontot i portalen.
1. Kopiera anslutningssträngen **key1**.
1. Klistra in innehållet i åtkomstnyckeln som du kopierade från portalen som värde för **StorageAccountConnectionString**. Konfigurationen bör nu se ut ungefär så här:
    ```json
    {
      "StorageAccountConnectionString": "your-access-key-connection-string-goes-here"
    }
    ```


