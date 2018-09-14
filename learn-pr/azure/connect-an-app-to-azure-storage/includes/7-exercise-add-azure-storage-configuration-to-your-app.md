I den här enheten ska du hämta åtkomstnyckeln och lägga till den i appkonfigurationen. Eftersom vi har skapat ett .NET Core-konsolprogram måste vi också lägga till stöd för läsning från konfigurationsfiler.

## <a name="create-a-json-configuration-file"></a>Skapa en JSON-konfigurationsfil

1. I Visual Studio-projektet, högerklicka på projektet i den **Solution Explorer** och klicka på **Lägg till** > **nytt objekt...** .

1. I den **Lägg till nytt objekt** dialogrutan den **Web** > **JavaScript JSON-konfigurationsfil** mall.
1. Ge den nya filen en **namn** av `appsettings.json` och klicka på **Lägg till**.

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
1. Högerklicka på den **appsettings.json** fil i den **Solution Explorer** och välj **egenskaper**.

1. Ändra **Copy to Output Directory** (Kopiera till utdatakatalog) till **Copy if newer** (Kopiera om nyare).

  ![Skärmbild av Visual Studio som visar dialogrutan appsettings.json egenskapssidor med avsnittet Kopiera till utdatakatalog markerat och ange att kopiera om nyare.](..\media-draft\7-build-action.png)

  > Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.

## <a name="add-support-to-read-a-json-configuration-file"></a>Lägga till stöd för läsning från en JSON-konfigurationsfil

Ett .NET Core-konsolprogram kräver att vissa bibliotek läggs till för att det ska kunna läsa från en JSON-konfigurationsfil utan problem.

1. Högerklicka på projektet och välj **hantera NuGet-paket...** .

1. Klicka på den **Bläddra** och ange **Microsoft.Extensions.Configuration.Json** i sökfältet.

1. Välj den **Microsoft.Extensions.Configuration.Json** paketet från sökresultaten och, i den högra rutan, väljer **installera**.

1. Dialogrutan **Förhandsgranska ändringar** visas. Klicka på **OK**.

1. En **godkännande av licensen** dialogrutan visas. Klicka på **jag accepterar** när du har granskat licenser.

## <a name="read-from-the-configuration-file"></a>Läsa från konfigurationsfilen

Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.

1. Leta upp filen **Program.cs** i projektet och öppna den i Visual Studio.

1. Högst upp i filen, lägger du till följande ytterligare `using` instruktioner:

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

## <a name="add-your-connection-string-to-the-configuration-file"></a>Lägga till anslutningssträngen i konfigurationsfilen

Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.

1. Gå till Azure-portalen.

1. Under den **alla resurser** avsnittet, leta reda på nyligen skapade lagringskontot.

1. Navigera till den **inställningar** > **Kontonycklar** avsnittet i bladet storage-konto.

1. Kopiera den **anslutningssträngen** värdet från den **key1** fält.

1. Klistra in innehållet i åtkomstnyckeln som du kopierade från portalen som värde för **StorageAccountConnectionString**. Konfigurationen bör nu se ut ungefär så här med din anslutning sträng ersätter den `<your-access-key-connection-string-goes-here>` platshållaren (utan vinkelparenteser):

    ```json
    {
      "StorageAccountConnectionString": "<your-access-key-connection-string-goes-here>"
    }
    ```
