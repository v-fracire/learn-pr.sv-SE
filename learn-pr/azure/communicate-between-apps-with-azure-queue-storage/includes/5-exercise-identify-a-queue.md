Nu ska vi skapa ett klientprogram som ska arbeta med en kö. Sedan ska vi lägga till vår anslutningssträng i koden.

## <a name="create-the-application"></a>Skapa programmet

Vi skapar nu ett .NET Core-program som du kan köra i Linux, macOS eller Windows. Vi ger det namnet **QueueApp**. Vi gör det enkelt för oss genom att använda en enda app för att både skicka och ta emot meddelanden till och från kön.

1. Använd kommandot `dotnet new` till att skapa en ny konsolapp med namnet **QueueApp**. Du kan skriva kommandon i Cloud Shell till höger. Om du arbetar lokalt kan du skriva dem i ett terminalfönster. Nu har du skapat en enkel app med en enda källfil: `Program.cs`.

    ```azurecli
    dotnet new console -n QueueApp
    ```

1. Växla till den nya mappen `QueueApp` och kompilera appen så att du ser att allting fungerar.

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a>Hämta anslutningssträngen

Du kan se din anslutningssträng i avsnittet **Inställningar -> Åtkomstnycklar** för lagringskontot i Azure Portal.

Du kan också hämta den via verktygen för Azure CLI eller PowerShell. Nu ska vi använda Azure CLI-kommandot. Kom ihåg att byta ut `<name>` mot namnet på lagringskontot du skapade. Du kan använda `az storage account list` om du behöver en påminnelse.

```azurecli
az storage account show-connection-string --name <name> --resource-group ExerciseResources
```

Det här kommandot returnerar ett JSON-block med anslutningssträngen. Du ser namnet på lagringskontot och åtkomstnyckeln:

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a>Lägg till anslutningssträngen i programmet

Slutligen ska vi lägga till anslutningssträngen i vår app så att den kan komma åt lagringskontot.

> [!WARNING]
> Vi gör det enkelt för oss och placerar anslutningssträngen i filen **Program.cs**. I en produktionsmiljö skulle du förvara den på en säker plats. För användning på serversidan rekommenderar vi att du använder Azure Key Vault.

1. Skriv `code .` i terminalen för att öppna onlinekodredigeraren. Om du arbetar lokalt kan du använda valfri IDE. Vi rekommenderar Visual Studio Code, som är en utmärkt plattformsoberoende IDE.

1. Öppna källfilen `Program.cs` i projektet.

1. Gå till klassen `Program` och lägg till ett värde av typen `const string` för anslutningssträngen. Du behöver bara värdet (det börjar med texten **DefaultEndpointsProtocol**).

1. Spara filen. Du kan klicka på ellipsen ”...” i molnredigerarens högra hörn, då ser du även kortkommandot.

Koden bör se ut ungefär så här (strängvärdet är unikt för ditt konto).

```csharp
...
namespace QueueApp
{
    class Program
    {
        private const string ConnectionString = "DefaultEndpointsProtocol=https; ...";
        
        ...
    }
}
```

Nu när det här enkla projektet är konfigurerat ska vi titta på hur du arbetar med köer i koden. Det handlar om _meddelanden_.