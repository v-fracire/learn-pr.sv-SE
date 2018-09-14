::: zone pivot="csharp" Let's add support to our .NET core application to retrieve a connection string from a configuration file. Vi börjar genom att lägga till nödvändiga installationerna om du vill hantera konfigurationen i en JSON-fil.

## <a name="create-a-json-configuration-file"></a>Skapa en JSON-konfigurationsfil

1. Kontrollera att du är i rätt arbetskatalogen för ditt projekt.

1. Använd den `touch` verktyg på kommandoraden för att skapa en fil med namnet **appsettings.json**.

    ```bash
    touch appsettings.json
    ```

1. Öppna projektet med interaktiva redigeraren. Om du arbetar lokalt kan använda redigeringsprogram val. Vi rekommenderar att **Visual Studio Code**, vilket är en utökningsbar plattformsoberoende IDE. Följande kommandon är avsedda för Cloud Shell-redigeraren, men är mycket lika VS Code.

    ```bash
    code .
    ```

1. Välj den **appsettings.json** i redigeraren och Lägg till följande text. Spara filen. I redigeraren online finns en meny i det övre högra hörnet som har gemensamma filåtgärder.

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. Välj sedan projektfilen (**PhotoSharingApp.csproj**) att öppna den i redigeraren.

1. Lägg till följande konfiguration block för att inkludera den nya filen i projektet och kopiera den till utdatamappen. Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.

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

1. Spara filen. (Kontrollera att du gör detta eller förlorar du ändringen när du lägger till paketet nedan!)

## <a name="add-support-to-read-a-json-configuration-file"></a>Lägga till stöd för läsning från en JSON-konfigurationsfil

Ett .NET Core-program kräver ytterligare NuGet-paket för att läsa en JSON-konfigurationsfil.

1. I avsnittet kommandotolk i fönstret, lägger du till en referens till den **Microsoft.Extensions.Configuration.Json** NuGet-paketet.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Lägg till kod för att läsa konfigurationsfilen

Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.

1. Välj **Program.cs** i redigeraren.

1. Överst i filen visas raden **using System;**. Lägg till följande rader med kod under den raden:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Ersätt innehållet i den **Main** metoden med följande kod. Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.

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

Vi lägger till stöd i vår Node.js-program för att hämta en anslutningssträng från en konfigurationsfil. Vi börjar genom att lägga till nödvändiga grovjobbet för att hantera konfigurationen från vårt JavaScript-fil.

## <a name="create-a-env-configuration-file"></a>Skapa en .env-konfigurationsfil

1. Kontrollera att du är i rätt arbetskatalogen för ditt projekt.

1. Använd den `touch` verktyg på kommandoraden för att skapa en fil med namnet **.env**.

    ```bash
    touch .env
    ```

1. Öppna projektet med interaktiva redigeraren, om du arbetar lokalt, använda redigeringsprogram föredrar - rekommenderar vi **Visual Studio Code** som är en utökningsbar plattformsoberoende IDE. Följande kommandon är för Cloud Shell-redigeraren, men är mycket lika VS Code.
    
    ```bash
    code .
    ```

1. Välj den **.env** i redigeraren och Lägg till följande text. Sparar du filen - redigeraren online finns det en meny i det övre högra hörnet som har gemensamma filåtgärder.

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > Den **AZURE_STORAGE_CONNECTION_STRING** värdet är en hårdkodad miljövariabel som används för Storage-API: er för att leta upp åtkomstnycklar. Du kan använda ditt eget namn om du föredrar, men du måste ange namnet på när du skapar den `BlobService` objekt.

1. Spara filen.

## <a name="add-support-to-read-an-environment-configuration-file"></a>Lägg till stöd för att läsa en konfigurationsfil för miljö

Node.js-appar kan innehålla stöd för att läsa från den **.env** filen genom att lägga till den **dotenv** paketet.

1. I avsnittet kommandotolk i fönstret lägger du till ett beroende till den *dotenv** paketet med hjälp av `npm`.

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Lägg till kod för att läsa konfigurationsfilen

Nu när vi har lagt till de bibliotek som krävs för att aktivera läsning konfiguration, måste vi aktivera funktionen i vårt program.

1. Välj *index.js** i redigeraren.

1. Överst i filen, en **#! / usr/bin/env noden** raden finns. Under den raden lägger du till en `require` -uttrycket för att läsa in den **dotenv** paketet. Detta gör miljövariabler som definieras i vår **.env** tillgänglig till programmet.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

## <a name="add-the-connection-string-to-the-configuration-file"></a>Lägg till anslutningssträngen i konfigurationsfilen

Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. Navigera till ditt lagringskonto. Du kan använda den **alla resurser** avsnittet för att hitta lagringskontot eller söka efter namn från den _sökrutan_ längst ned i portalfönstret.

1. Välj den **åtkomstnycklar** bladet för storage-konto i portalen.

1. Kopiera den **key1** anslutningssträngen.

1. Klistra in innehållet i åtkomstnyckeln som du kopierade från portalen som värde för anslutning för konfigurationen strängvariabel.

Konfigurationen bör nu se ut ungefär så här:

::: zone pivot="csharp"
```json
{
    "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
}
```
::: zone-end

::: zone pivot="javascript"
```
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
```
::: zone-end

Nu när vi har att alla kabelanslutna, kan vi börja att lägga till kod för att använda våra storage-konto.