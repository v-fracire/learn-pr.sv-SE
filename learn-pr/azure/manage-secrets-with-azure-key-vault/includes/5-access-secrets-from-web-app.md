Nu när du vet hur du ska aktivera att MSI skapar en identitet som vår app ska använda vid autentisering, skapar vi en app som använder den identiteten för att få åtkomst till hemligheter i valvet.

## <a name="reading-secrets-in-an-aspnet-core-app"></a>Läsa hemligheter i en ASP.NET Core-app

API:n för Azure Key Vault är en REST API som sköter all hantering och användning av nycklar och valv. Varje hemlighet i ett valv har en unik URL och hemligheterna hämtas med HTTP GET-begäranden.

Det officiella Key Vault-klientbiblioteket för .NET Core är `KeyVaultClient`-klassen i Microsoft.Azure.KeyVault NuGet-paketet. Du behöver inte använda det direkt, men &mdash; med ASP.NET Cores `AddAzureKeyVault`-metod kan du läsa in alla hemligheter från ett valv till konfigurations-API:n vid start. Med den här tekniken får du åtkomst till alla dina hemligheter med samma `IConfiguration`-gränssnitt som du använder i resten av din konfiguration. Appar som använder `AddAzureKeyVault` kräver både behörigheten **Hämta** och **Lista** för valvet.

> [!TIP]
> Oavsett vilket ramverk och språk du använder när du skapar din app, ska du cachelagra hemliga värden lokalt eller läsa in dem i minnet vid appstarten – om du inte har en särskild anledning att låta bli. Det är onödigt långsamt och dyrt att läsa dem direkt från valvet varje gång du behöver dem.

`AddAzureKeyVault` kräver endast valvnamnet som indata, vilket vi får från våra lokala appkonfiguration. Den hanterar också automatiskt MSI-autentisering &mdash; när den används i en app som distribueras till Azure App Service med MSI aktiverat, identifierar den MSI-tokentjänsten och använder den vid autentiseringen. Det är ett bra alternativ för de flesta scenarier så vi kommer att använda det i övningen för denna enhet.

## <a name="handling-secrets-in-an-app"></a>Hantera hemligheter i en app

När en hemlighet har lästs in i din app är det upp till din app att hantera den på ett säkert sätt. I appen som vi skapar i den här modulen skriver vi vår hemlighet till klientsvaret och visar den i en webbläsare för att kunna kontrollera att den har lästs in korrekt. **Att returnera en hemlighet till klienten är *inte* något som du normalt sett gör!** Vanligtvis använder du hemligheter till att exempelvis initiera klientbibliotek för databaser eller fjärr-API:er.

> [!IMPORTANT]
> Granska alltid koden noga för att säkerställa att din app aldrig skriver hemligheterna till någon typ av utdata, inklusive loggar, lagring och svar.

## <a name="exercise"></a>Övning

Vi ska skapa en ny webb-API i ASP.NET Core och använda `AddAzureKeyVault` till att läsa in hemligheten från vårt valv.

### <a name="create-the-app"></a>Skapa appen

I Azure Cloud Shell-terminalen kör du följande för att skapa en ny webb-API i ASP.NET Core och öppna den i redigeringsprogrammet.

```console
dotnet new webapi -o KeyVaultDemoApp
cd KeyVaultDemoApp
code .
```

När redigeringsprogrammet har lästs in kör du följande kommandon i gränssnittet för att lägga till NuGet-paketet med `AddAzureKeyVault` och återställa alla beroenden i appen.

```console
dotnet add package Microsoft.Extensions.Configuration.AzureKeyVault
dotnet restore
```

### <a name="add-code-to-load-and-use-secrets"></a>Lägga till kod för att läsa in och använda hemligheter

För att kunna visa en bra användning av Key Vault, ändrar vi vår app så att den läser in hemligheter från valvet vid start. Vi lägger också till en ny kontrollant med en slutpunkt som hämtar vår **SecretPassword**-hemlighet från valvet.

Först startas appen: Öppna `Program.cs`, ta bort innehållet och ersätt det med följande kod:

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;

namespace KeyVaultDemoApp
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((context, config) =>
                {
                    // Build the current set of configuration to load values from
                    // JSON files and environment variables, including VaultName.
                    var builtConfig = config.Build();

                    // Use VaultName from the configuration to create the full vault URL.
                    var vaultUrl = $"https://{builtConfig["VaultName"]}.vault.azure.net/";

                    // Load all secrets from the vault into configuration. This will automatically
                    // authenticate to the vault using MSI. If MSI is not available, it will
                    // check if Visual Studio and/or the Azure CLI are installed locally and
                    // see if they are configured with credentials that can access the vault.
                    config.AddAzureKeyVault(vaultUrl);
                })
                .UseStartup<Startup>();
    }
}
```

> [!TIP]
> Spara filerna med `Ctrl+S` när du är klar med redigeringen.

Den enda ändringen i startkoden är att vi lägger till `ConfigureAppConfiguration`. Nu läser vi in valvnamnet från konfigurationen och anropar `AddAzureKeyVault` med den.

Därefter kommer vi till kontrollanten: Skapa en ny fil i mappen `Controllers` med namnet `SecretTestController.cs` och klistra in följande kod i den.

> [!TIP]
> Du kan skapa en ny fil med kommandot `touch` i gränssnittet. I det här fallet använder du `touch Controllers/SecretTestController.cs`. Du måste klicka på knappen Uppdatera i rutan Filer i redigeringsprogrammet för att se det.

```csharp
using System;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;

namespace KeyVaultDemoApp.Controllers
{
    [Route("api/[controller]")]
    public class SecretTestController : ControllerBase
    {
        private readonly IConfiguration _configuration;

        public SecretTestController(IConfiguration configuration)
        {
            _configuration = configuration;
        }

        [HttpGet]
        public IActionResult Get()
        {
            // Get the secret value from configuration. This can be done anywhere
            // we have access to IConfiguration. This does not call the Key Vault
            // API, because the secrets were loaded at startup.
            var secretName = "SecretPassword";
            var secretValue = _configuration[secretName];

            if (secretValue == null)
            {
                return StatusCode(
                    StatusCodes.Status500InternalServerError,
                    $"Error: No secret named {secretName} was found...");
            }
            else {
                return Content($"Secret value: {secretValue}" +
                    Environment.NewLine + Environment.NewLine +
                    "This is for testing only! Never output a secret " +
                    "to a response or anywhere else in a real app!");
            }
        }
    }
}
```

Kör `dotnet build` i gränssnittet för kontrollera att allt kompileras. Appen är redo att köras &mdash; nu ska vi få in den i Azure!