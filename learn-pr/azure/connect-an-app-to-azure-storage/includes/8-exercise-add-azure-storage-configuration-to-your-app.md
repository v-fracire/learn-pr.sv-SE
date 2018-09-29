::: zone pivot="csharp" Nu ska vi lägga till stöd i vår .NET Core-app för att hämta en anslutningssträng från en konfigurationsfil. Vi börjar med att lägga till de delar som behövs för att hantera konfigurationen i en JSON-fil.

## <a name="create-a-json-configuration-file"></a>Skapa en JSON-konfigurationsfil

1. `cd` till katalogen PhotoSharingApp om du inte redan är där.

1. Använd verktyget `touch` på kommandoraden till att skapa en fil med namnet **appsettings.json**.

    ```bash
    touch appsettings.json
    ```

1. Öppna projektet i en redigerare. Om du arbetar lokalt kan du använda valfri redigerare. Vi rekommenderar **Visual Studio Code**, som är en utökningsbar plattformsoberoende IDE. Om du arbetar i Cloud Shell rekommenderar vi Cloud Shell-redigeraren. Följande kommando fungerar för båda.

    ```bash
    code .
    ```

1. Välj filen **appsettings.json** i redigeraren och lägg till följande text. Spara filen. I Cloud Shell-redigeraren finns det en meny i det övre högra hörnet med vanliga filåtgärder.

    ```json
    {
      "StorageAccountConnectionString": "<value>"
    }
    ```

1. Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app. Kör följande kommando i Cloud Shell.

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. Kopiera anslutningssträngen som returneras från det kommandot och ersätt `"<value>"` i filen **appsettings.json** med den här anslutningssträngen. Spara filen.

1. Öppna sedan projektfilen (**PhotoSharingApp.csproj**) i redigeraren.

1. Lägg till följande konfigurationsblock för att inkludera den nya filen i projektet och kopiera den till utdatamappen. Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. Spara filen. (Var noga med att göra det här, annars förlorar du dina ändringar när du lägger till paketet nedan!)

## <a name="add-support-to-read-a-json-configuration-file"></a>Lägga till stöd för läsning från en JSON-konfigurationsfil

I .NET Core-program behövs fler NuGet-paket för att kunna läsa en JSON-konfigurationsfil.

1. Lägg till en referens till NuGet-paketet **Microsoft.Extensions.Configuration.Json** i kommandotolken i fönstret.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Lägga till kod för att läsa konfigurationsfilen

Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.

1. Välj **Program.cs** i redigeraren.

1. Längst upp i filen ser du raden **using System;**. Lägg till följande kodrader under den här raden:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Ersätt innehållet i metoden **Main** med följande kod. Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

Filen **Program.cs** bör se ut så här nu:

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace PhotoSharingApp
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

::: zone-end

::: zone pivot="javascript"

Nu ska vi lägga till stöd i vår Node.js-app för att hämta en anslutningssträng från en konfigurationsfil. Vi börjar med att lägga till de delar som behövs för att hantera konfigurationen från vår JavaScript-fil.

## <a name="create-a-env-configuration-file"></a>Skapa en .env-konfigurationsfil

1. Se till att du befinner dig i rätt arbetskatalog för projektet.

1. Använd verktyget `touch` på kommandoraden till att skapa en fil med namnet **.env**.

    ```bash
    touch .env
    ```

1. Öppna projektet i Cloud Shell-redigeraren.

    ```bash
    code .
    ```

1. Välj filen **.env** i redigeraren och lägg till följande text. Du kan behöva klicka på knappen Uppdatera i koden för att se de nya filerna. Spara filen. I onlineredigeraren finns det en meny i det övre högra hörnet med vanliga filåtgärder.

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > Värdet **AZURE_STORAGE_CONNECTION_STRING** är en hårdkodad miljövariabel som API:erna i Storage använder till att slå upp åtkomstnycklar. Du kan använda ett eget namn om du vill, men du måste ange namnet när du skapar `BlobService`-objektet i Node.js-appen.

1. Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app. Kör följande kommando i Cloud Shell.

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. Kopiera anslutningssträngen som returneras från kommandot, minus citattecknen, och ersätt `<value>` i filen **.env** med den här anslutningssträngen.

1. Spara filen.

## <a name="add-support-to-read-an-environment-configuration-file"></a>Lägga till stöd för läsning från en miljökonfigurationsfil

Node.js-appar kan innehålla stöd för att läsa från filen **.env** om du lägger till paketet **dotenv**.

1. Lägg till ett beroende för paketet *dotenv** i kommandotolken i fönstret med hjälp av `npm`.

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Lägga till kod för att läsa konfigurationsfilen

Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt program.

1. Välj *index.js** i redigeringsprogrammet.

1. Längst upp i filen ser du raden **#!/usr/bin/env node**. Under den här raden lägger du till ett `require`-uttryck som läser in paketet **dotenv**. Det här gör miljövariablerna som definieras i vår **.env**-fil tillgängliga i programmet.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

Nu när vi har ordnat med allt det här kan vi börja lägga till kod för att använda vårt lagringskonto.