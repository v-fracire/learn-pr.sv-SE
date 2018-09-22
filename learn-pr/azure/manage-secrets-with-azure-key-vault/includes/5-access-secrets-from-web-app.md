<span data-ttu-id="c47d6-101">Nu när du vet hur du kan aktivera hanterade identiteter för Azure-resurser för att skapa en identitet som vår app kan använda för autentisering, ska vi skapa en app som använder identiteten för att komma åt hemligheter i valvet.</span><span class="sxs-lookup"><span data-stu-id="c47d6-101">Now that you know how enabling managed identities for Azure resources creates an identity for our app to use for authentication, we'll create an app that uses that identity to access secrets in the vault.</span></span>

## <a name="reading-secrets-in-an-aspnet-core-app"></a><span data-ttu-id="c47d6-102">Läsa hemligheter i en ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="c47d6-102">Reading secrets in an ASP.NET Core app</span></span>

<span data-ttu-id="c47d6-103">API:n för Azure Key Vault är en REST API som sköter all hantering och användning av nycklar och valv.</span><span class="sxs-lookup"><span data-stu-id="c47d6-103">The Azure Key Vault API is a REST API that handles all management and usage of keys and vaults.</span></span> <span data-ttu-id="c47d6-104">Varje hemlighet i ett valv har en unik URL och hemligheterna hämtas med HTTP GET-begäranden.</span><span class="sxs-lookup"><span data-stu-id="c47d6-104">Each secret in a vault has a unique URL, and secret values are retrieved with HTTP GET requests.</span></span>

<span data-ttu-id="c47d6-105">Det officiella Key Vault-klientbiblioteket för .NET Core är `KeyVaultClient`-klassen i Microsoft.Azure.KeyVault NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="c47d6-105">The official Key Vault client library for .NET Core is the `KeyVaultClient` class in the Microsoft.Azure.KeyVault NuGet package.</span></span> <span data-ttu-id="c47d6-106">Du behöver inte använda det direkt, men &mdash; med ASP.NET Cores `AddAzureKeyVault`-metod kan du läsa in alla hemligheter från ett valv till konfigurations-API:n vid start.</span><span class="sxs-lookup"><span data-stu-id="c47d6-106">You don't need to use it directly, though &mdash; with ASP.NET Core's `AddAzureKeyVault` method, you can load all the secrets from a vault into the Configuration API at startup.</span></span> <span data-ttu-id="c47d6-107">Med den här tekniken får du åtkomst till alla dina hemligheter med samma `IConfiguration`-gränssnitt som du använder i resten av din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c47d6-107">This technique enables you to access all of your secrets by name using the same `IConfiguration` interface you use for the rest of your configuration.</span></span> <span data-ttu-id="c47d6-108">Appar som använder `AddAzureKeyVault` kräver både behörigheten **Hämta** och **Lista** för valvet.</span><span class="sxs-lookup"><span data-stu-id="c47d6-108">Apps that use `AddAzureKeyVault` require both **Get** and **List** permissions to the vault.</span></span>

> [!TIP]
> <span data-ttu-id="c47d6-109">Oavsett vilket ramverk och språk du använder när du skapar din app, ska du designa det att cachelagra hemliga värden lokalt eller läsa in dem i minnet vid start – om du inte har en särskild anledning att låta bli.</span><span class="sxs-lookup"><span data-stu-id="c47d6-109">Regardless of the framework or language you use to build your app, you should design it to cache secret values locally or load them into memory at startup unless you have a specific reason not to.</span></span> <span data-ttu-id="c47d6-110">Det är onödigt långsamt och dyrt att läsa dem direkt från valvet varje gång du behöver dem.</span><span class="sxs-lookup"><span data-stu-id="c47d6-110">Reading them directly from the vault every time you need them is unnecessarily slow and expensive.</span></span>

<span data-ttu-id="c47d6-111">`AddAzureKeyVault` kräver endast valvnamnet som indata, vilket vi får från våra lokala appkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="c47d6-111">`AddAzureKeyVault` only requires the vault name as an input, which we'll get from our local app configuration.</span></span> <span data-ttu-id="c47d6-112">Den hanterar även autentisering med hanterade identiteter automatiskt. När den används i en app som distribuerats till Azure App Service med hanterade identiteter för Azure-resurser aktiverat, identifierar den tokentjänsten för hanterade identiteter och använder den för att autentisera.</span><span class="sxs-lookup"><span data-stu-id="c47d6-112">It also automatically handles managed identity authentication &mdash; when used in an app deployed to Azure App Service with managed identities for Azure resources enabled, it will detect the managed identities token service and use it to authenticate.</span></span> <span data-ttu-id="c47d6-113">Det är ett bra alternativ för de flesta scenarier så vi kommer att använda det i övningen för denna enhet.</span><span class="sxs-lookup"><span data-stu-id="c47d6-113">It's a good fit for most scenarios and implements all best practices, and we'll use it in this unit's exercise.</span></span>

## <a name="handling-secrets-in-an-app"></a><span data-ttu-id="c47d6-114">Hantera hemligheter i en app</span><span class="sxs-lookup"><span data-stu-id="c47d6-114">Handling secrets in an app</span></span>

<span data-ttu-id="c47d6-115">När en hemlighet har lästs in i din app är det upp till din app att hantera den på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="c47d6-115">Once a secret is loaded into your app, it's up to your app to handle it securely.</span></span> <span data-ttu-id="c47d6-116">I appen som vi skapar i den här modulen skriver vi vår hemlighet till klientsvaret och visar den i en webbläsare för att kunna kontrollera att den har lästs in korrekt.</span><span class="sxs-lookup"><span data-stu-id="c47d6-116">In the app we build in this module, we write our secret value out to the client response and view it in a web browser to demonstrate that it has been loaded successfully.</span></span> <span data-ttu-id="c47d6-117">**Att returnera en hemlighet till klienten är *inte* något som du normalt sett gör!**</span><span class="sxs-lookup"><span data-stu-id="c47d6-117">**Returning a secret value to the client is *not* something you'd normally do!**</span></span> <span data-ttu-id="c47d6-118">Vanligtvis använder du hemligheter till att exempelvis initiera klientbibliotek för databaser eller fjärr-API:er.</span><span class="sxs-lookup"><span data-stu-id="c47d6-118">Usually, you'll use secrets to do things like initialize client libraries for databases or remote APIs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c47d6-119">Granska alltid koden noga för att säkerställa att din app aldrig skriver hemligheterna till någon typ av utdata, inklusive loggar, lagring och svar.</span><span class="sxs-lookup"><span data-stu-id="c47d6-119">Always carefully review your code to ensure that your app never writes secrets to any kind of output, including logs, storage, and responses.</span></span>

## <a name="exercise"></a><span data-ttu-id="c47d6-120">Övning</span><span class="sxs-lookup"><span data-stu-id="c47d6-120">Exercise</span></span>

<span data-ttu-id="c47d6-121">Vi ska skapa en ny webb-API i ASP.NET Core och använda `AddAzureKeyVault` till att läsa in hemligheten från vårt valv.</span><span class="sxs-lookup"><span data-stu-id="c47d6-121">We'll create a new ASP.NET Core web API and use `AddAzureKeyVault` to load the secret from our vault.</span></span>

### <a name="create-the-app"></a><span data-ttu-id="c47d6-122">Skapa appen</span><span class="sxs-lookup"><span data-stu-id="c47d6-122">Create the app</span></span>

<span data-ttu-id="c47d6-123">I Azure Cloud Shell-terminalen kör du följande för att skapa en ny webb-API i ASP.NET Core och öppna den i redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c47d6-123">In the Azure Cloud Shell terminal, run the following to create a new ASP.NET Core web API application and open it in the editor.</span></span>

```console
dotnet new webapi -o KeyVaultDemoApp
cd KeyVaultDemoApp
code .
```

<span data-ttu-id="c47d6-124">När redigeringsprogrammet har lästs in kör du följande kommandon i gränssnittet för att lägga till NuGet-paketet med `AddAzureKeyVault` och återställa alla beroenden i appen.</span><span class="sxs-lookup"><span data-stu-id="c47d6-124">After the editor loads, run the following commands in the shell to add the NuGet package containing `AddAzureKeyVault` and restore all of the app's dependencies.</span></span>

```console
dotnet add package Microsoft.Extensions.Configuration.AzureKeyVault
dotnet restore
```

### <a name="add-code-to-load-and-use-secrets"></a><span data-ttu-id="c47d6-125">Lägga till kod för att läsa in och använda hemligheter</span><span class="sxs-lookup"><span data-stu-id="c47d6-125">Add code to load and use secrets</span></span>

<span data-ttu-id="c47d6-126">För att kunna visa en bra användning av Key Vault, ändrar vi vår app så att den läser in hemligheter från valvet vid start.</span><span class="sxs-lookup"><span data-stu-id="c47d6-126">To demonstrate good usage of Key Vault, we will modify our app to load secrets from the vault at startup.</span></span> <span data-ttu-id="c47d6-127">Vi lägger också till en ny kontrollant med en slutpunkt som hämtar vår **SecretPassword**-hemlighet från valvet.</span><span class="sxs-lookup"><span data-stu-id="c47d6-127">We'll also add a new controller with an endpoint that gets our **SecretPassword** secret from the vault.</span></span>

<span data-ttu-id="c47d6-128">Först startas appen: Öppna `Program.cs`, ta bort innehållet och ersätt det med följande kod:</span><span class="sxs-lookup"><span data-stu-id="c47d6-128">First, the app startup: Open `Program.cs`, delete the contents and replace them with the following code:</span></span>

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
                    // authenticate to the vault using a managed identity. If a managed identity
                    // is not available, it will check if Visual Studio and/or the Azure CLI are
                    // installed locally and see if they are configured with credentials that can
                    // access the vault.
                    config.AddAzureKeyVault(vaultUrl);
                })
                .UseStartup<Startup>();
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="c47d6-129">Spara filerna när du är klar med redigeringen.</span><span class="sxs-lookup"><span data-stu-id="c47d6-129">Make sure to save files when you're done editing them.</span></span> <span data-ttu-id="c47d6-130">Du kan antingen göra det med menyn ... eller snabbtangenten (<kbd>Ctrl+S</kbd> i Windows och Linux, <kbd>Cmd+S</kbd> i macOS).</span><span class="sxs-lookup"><span data-stu-id="c47d6-130">You can do this either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

<span data-ttu-id="c47d6-131">Den enda ändringen i startkoden är att vi lägger till `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="c47d6-131">The only change from the starter code is the addition of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="c47d6-132">Nu läser vi in valvnamnet från konfigurationen och anropar `AddAzureKeyVault` med den.</span><span class="sxs-lookup"><span data-stu-id="c47d6-132">This is where we load the vault name from configuration and call `AddAzureKeyVault` with it.</span></span>

<span data-ttu-id="c47d6-133">Därefter kommer vi till kontrollanten: Skapa en ny fil i mappen `Controllers` med namnet `SecretTestController.cs` och klistra in följande kod i den.</span><span class="sxs-lookup"><span data-stu-id="c47d6-133">Next, the controller: Create a new file in the `Controllers` folder called `SecretTestController.cs` and paste the following code into it.</span></span>

> [!TIP]
> <span data-ttu-id="c47d6-134">Du kan skapa en ny fil med kommandot `touch` i gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c47d6-134">To create a new file, use the `touch` command in the shell.</span></span> <span data-ttu-id="c47d6-135">I det här fallet använder du `touch Controllers/SecretTestController.cs`.</span><span class="sxs-lookup"><span data-stu-id="c47d6-135">In this case, use `touch Controllers/SecretTestController.cs`.</span></span> <span data-ttu-id="c47d6-136">Du måste klicka på knappen Uppdatera i rutan Filer i redigeringsprogrammet för att se det.</span><span class="sxs-lookup"><span data-stu-id="c47d6-136">You'll need to click the refresh button in the Files pane of the editor to see it there.</span></span>

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

<span data-ttu-id="c47d6-137">Kör `dotnet build` i gränssnittet för kontrollera att allt kompileras.</span><span class="sxs-lookup"><span data-stu-id="c47d6-137">Run `dotnet build` in the shell to make sure everything compiles.</span></span> <span data-ttu-id="c47d6-138">Appen är redo att köras &mdash; nu ska vi få in den i Azure!</span><span class="sxs-lookup"><span data-stu-id="c47d6-138">The app is ready to run &mdash; now let's get it into Azure!</span></span>