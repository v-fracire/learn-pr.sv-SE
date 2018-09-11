<span data-ttu-id="80f8a-101">Nu är det dags att köra vår app i Azure.</span><span class="sxs-lookup"><span data-stu-id="80f8a-101">Now it's time to run our app in Azure.</span></span> <span data-ttu-id="80f8a-102">Vi måste skapa en Azure App Service-app, konfigurera den med MSI och vår valvkonfiguration, samt distribuera vår kod.</span><span class="sxs-lookup"><span data-stu-id="80f8a-102">We need to create an Azure App Service app, set it up with MSI and our vault configuration, and deploy our code.</span></span>

## <a name="exercise"></a><span data-ttu-id="80f8a-103">Övning</span><span class="sxs-lookup"><span data-stu-id="80f8a-103">Exercise</span></span>

### <a name="create-the-app-service-plan-and-app"></a><span data-ttu-id="80f8a-104">Skapa App Service-planen och appen</span><span class="sxs-lookup"><span data-stu-id="80f8a-104">Create the App Service plan and app</span></span>

<span data-ttu-id="80f8a-105">Att skapa en App Service-app är en tvåstegsprocess: Först skapas *planen*, sedan *appen*.</span><span class="sxs-lookup"><span data-stu-id="80f8a-105">Creating an App Service app is a two-step process: First create the *plan*, then the *app*.</span></span>

<span data-ttu-id="80f8a-106">Namnet på *planen* behöver bara vara unikt inom din prenumeration, så du kan använda samma namn som vi har använt: **keyvault-exercise-plan**.</span><span class="sxs-lookup"><span data-stu-id="80f8a-106">The *plan* name only needs to be unique within your subscription, so you can use the same name we've used: **keyvault-exercise-plan**.</span></span> <span data-ttu-id="80f8a-107">Appnamnet måste däremot vara globalt unikt, så du måste välja ett eget.</span><span class="sxs-lookup"><span data-stu-id="80f8a-107">The app name needs to be globally unique, though, so you'll need to pick your own.</span></span>

<span data-ttu-id="80f8a-108">Kör följande i Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="80f8a-108">In Azure Cloud Shell, run the following:</span></span>

```azurecli
az appservice plan create --name keyvault-exercise-plan --resource-group keyvault-exercise-group
az webapp create --name <your-unique-app-name> --plan keyvault-exercise-plan --resource-group keyvault-exercise-group
```

### <a name="add-configuration-to-the-app"></a><span data-ttu-id="80f8a-109">Lägga till konfigurationen i appen</span><span class="sxs-lookup"><span data-stu-id="80f8a-109">Add configuration to the app</span></span>

<span data-ttu-id="80f8a-110">När vi distribuerar till Azure följer vi rekommenderade metoder för App Service och placerar konfigurationen i programinställningarna i stället för i en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="80f8a-110">For deploying to Azure, we'll follow the App Service best practice of putting configuration in Application Settings instead of a configuration file.</span></span>

```azurecli
az webapp config appsettings set --name <your-unique-app-name> --resource-group keyvault-exercise-group --settings VaultName=<your-unique-vault-name>
```

### <a name="enable-msi"></a><span data-ttu-id="80f8a-111">Aktivera MSI</span><span class="sxs-lookup"><span data-stu-id="80f8a-111">Enable MSI</span></span>

<span data-ttu-id="80f8a-112">Aktivering av MSI i en app görs på en rad:</span><span class="sxs-lookup"><span data-stu-id="80f8a-112">Enabling MSI on an app is a one-liner:</span></span>

```azurecli
az webapp identity assign --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="80f8a-113">Kopiera värdet **principalId** från de JSON-utdata du får.</span><span class="sxs-lookup"><span data-stu-id="80f8a-113">From the JSON output that results, copy the **principalId** value.</span></span> <span data-ttu-id="80f8a-114">PrincipalId är det unika ID:t för appens nya identitet i Azure Active Directory och vi ska använda det i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="80f8a-114">PrincipalId is the unique ID of the app's new identity in Azure Active Directory, and we're going to use it in the next step.</span></span>

### <a name="grant-access-to-the-vault"></a><span data-ttu-id="80f8a-115">Bevilja åtkomst till valvet</span><span class="sxs-lookup"><span data-stu-id="80f8a-115">Grant access to the vault</span></span>

<span data-ttu-id="80f8a-116">Nu måste du ge din appidentitet behörighet att hämta och lista hemligheter från valvet för produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="80f8a-116">Now you need to give your app identity permissions to get and list secrets from your production-environment vault.</span></span> <span data-ttu-id="80f8a-117">Använd värdet **principalId** som du kopierade i föregående steg som värde för **object-id** i kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="80f8a-117">Use the **principalId** value you copied from the previous step as the value for **object-id** in the command below.</span></span>

```azurecli
az keyvault set-policy --name <your-unique-vault-name> --object-id <your-msi-principleid> --secret-permissions get list
```

### <a name="deploy-the-app-and-try-it-out"></a><span data-ttu-id="80f8a-118">Distribuera appen och testa den</span><span class="sxs-lookup"><span data-stu-id="80f8a-118">Deploy the app and try it out</span></span>

<span data-ttu-id="80f8a-119">Din konfiguration är klar och du är redo att distribuera!</span><span class="sxs-lookup"><span data-stu-id="80f8a-119">All your configuration is set and you're ready to deploy!</span></span> <span data-ttu-id="80f8a-120">Nedanstående kommandon publicerar webbplatsen till mappen `pub`, zippar upp den till `site.zip` och distribuerar zip-filen till App Service.</span><span class="sxs-lookup"><span data-stu-id="80f8a-120">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="80f8a-121">Du måste `cd` tillbaka till katalogen KeyVaultDemoApp om du inte redan har gjort det.</span><span class="sxs-lookup"><span data-stu-id="80f8a-121">You'll need to `cd` back to the KeyVaultDemoApp directory if you haven't already.</span></span>

```console
dotnet publish -o pub
zip -j site.zip pub/*
az webapp deployment source config-zip --src site.zip --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="80f8a-122">När du har fått ett resultat som visar att webbplatsen har distribuerats, öppnar du `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="80f8a-122">Once you get a result that indicates the site has deployed, open `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in a browser.</span></span> <span data-ttu-id="80f8a-123">Du bör nu se det hemliga värdet **reindeer_flotilla**.</span><span class="sxs-lookup"><span data-stu-id="80f8a-123">You should see the secret value, **reindeer_flotilla**.</span></span>

<span data-ttu-id="80f8a-124">Din app är klar och distribuerad!</span><span class="sxs-lookup"><span data-stu-id="80f8a-124">Your app is finished and deployed!</span></span>