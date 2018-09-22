<span data-ttu-id="7d7df-101">Nu är det dags att köra vår app i Azure.</span><span class="sxs-lookup"><span data-stu-id="7d7df-101">Now it's time to run our app in Azure.</span></span> <span data-ttu-id="7d7df-102">Vi behöver skapa en Azure App Service-app, konfigurera den med en hanterad identitet och vår valvkonfiguration samt distribuera vår kod.</span><span class="sxs-lookup"><span data-stu-id="7d7df-102">We need to create an Azure App Service app, set it up with a managed identity and our vault configuration, and deploy our code.</span></span>

## <a name="create-the-app-service-plan-and-app"></a><span data-ttu-id="7d7df-103">Skapa App Service-planen och appen</span><span class="sxs-lookup"><span data-stu-id="7d7df-103">Create the App Service plan and app</span></span>

<span data-ttu-id="7d7df-104">Att skapa en App Service-app är en tvåstegsprocess: Först skapas *planen*, sedan *appen*.</span><span class="sxs-lookup"><span data-stu-id="7d7df-104">Creating an App Service app is a two-step process: First create the *plan*, then the *app*.</span></span>

<span data-ttu-id="7d7df-105">Namnet på *planen* behöver bara vara unikt inom din prenumeration, så du kan använda samma namn som vi har använt: **keyvault-exercise-plan**.</span><span class="sxs-lookup"><span data-stu-id="7d7df-105">The *plan* name only needs to be unique within your subscription, so you can use the same name we've used: **keyvault-exercise-plan**.</span></span> <span data-ttu-id="7d7df-106">Appnamnet måste däremot vara globalt unikt, så du måste välja ett eget.</span><span class="sxs-lookup"><span data-stu-id="7d7df-106">The app name needs to be globally unique, though, so you'll need to pick your own.</span></span> <span data-ttu-id="7d7df-107">Använd samma plats som du använde när du skapade valvet.</span><span class="sxs-lookup"><span data-stu-id="7d7df-107">Use the same location you used when creating the vault.</span></span>

<span data-ttu-id="7d7df-108">Kör följande i Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="7d7df-108">In Azure Cloud Shell, run the following:</span></span>

```azurecli
az appservice plan create \
    --name keyvault-exercise-plan \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
    --location eastus

az webapp create \
    --name <your-unique-app-name> \
    --plan keyvault-exercise-plan \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

## <a name="add-configuration-to-the-app"></a><span data-ttu-id="7d7df-109">Lägga till konfigurationen i appen</span><span class="sxs-lookup"><span data-stu-id="7d7df-109">Add configuration to the app</span></span>

<span data-ttu-id="7d7df-110">När vi distribuerar till Azure följer vi rekommenderade metoder för App Service och placerar VaultName-konfigurationen i programinställningarna istället för i en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="7d7df-110">For deploying to Azure, we'll follow the App Service best practice of putting the VaultName configuration in an application setting instead of a configuration file.</span></span> <span data-ttu-id="7d7df-111">Kör det här kommandot för att skapa programinställningen:</span><span class="sxs-lookup"><span data-stu-id="7d7df-111">Run this command to create the application setting:</span></span>

```azurecli
az webapp config appsettings set \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --settings VaultName=<your-unique-vault-name>
```

## <a name="enable-managed-identity"></a><span data-ttu-id="7d7df-112">Aktivera hanterad identitet</span><span class="sxs-lookup"><span data-stu-id="7d7df-112">Enable managed identity</span></span>

<span data-ttu-id="7d7df-113">Att aktivera hanterade identiteter på en app tar en kodrad &mdash; kör det här om du vill aktivera det på din app:</span><span class="sxs-lookup"><span data-stu-id="7d7df-113">Enabling managed identity on an app is a one-liner &mdash; run this to enable it on your app:</span></span>

```azurecli
az webapp identity assign \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="7d7df-114">Kopiera värdet **principalId** från de JSON-utdata du får.</span><span class="sxs-lookup"><span data-stu-id="7d7df-114">From the JSON output that results, copy the **principalId** value.</span></span> <span data-ttu-id="7d7df-115">PrincipalId är det unika ID:t för appens nya identitet i Azure Active Directory och vi ska använda det i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="7d7df-115">PrincipalId is the unique ID of the app's new identity in Azure Active Directory, and we're going to use it in the next step.</span></span>

## <a name="grant-access-to-the-vault"></a><span data-ttu-id="7d7df-116">Bevilja åtkomst till valvet</span><span class="sxs-lookup"><span data-stu-id="7d7df-116">Grant access to the vault</span></span>

<span data-ttu-id="7d7df-117">Det sista steget innan du distribuerar är att tilldela Key Vault-behörigheter till din apps hanterade identitet.</span><span class="sxs-lookup"><span data-stu-id="7d7df-117">The last step before deploying is to assign Key Vault permissions to your app's managed identity.</span></span> <span data-ttu-id="7d7df-118">Använd värdet **principalId** som du kopierade i föregående steg som värde för **object-id** i kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="7d7df-118">Use the **principalId** value you copied from the previous step as the value for **object-id** in the command below.</span></span> <span data-ttu-id="7d7df-119">Om du kör det här kommandot så beviljas **hämta**- och **lista**-åtkomst:</span><span class="sxs-lookup"><span data-stu-id="7d7df-119">Running this command will grant **Get** and **List** access:</span></span>

```azurecli
az keyvault set-policy \
    --name <your-unique-vault-name> \
    --object-id <your-managed-identity-principleid> \
    --secret-permissions get list
```

## <a name="deploy-the-app-and-try-it-out"></a><span data-ttu-id="7d7df-120">Distribuera appen och testa den</span><span class="sxs-lookup"><span data-stu-id="7d7df-120">Deploy the app and try it out</span></span>

<span data-ttu-id="7d7df-121">Din konfiguration är klar och du är redo att distribuera!</span><span class="sxs-lookup"><span data-stu-id="7d7df-121">All your configuration is set and you're ready to deploy!</span></span> <span data-ttu-id="7d7df-122">Nedanstående kommandon publicerar webbplatsen till mappen `pub`, zippar upp den till `site.zip` och distribuerar zip-filen till App Service.</span><span class="sxs-lookup"><span data-stu-id="7d7df-122">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="7d7df-123">Du måste `cd` tillbaka till katalogen KeyVaultDemoApp om du inte redan har gjort det.</span><span class="sxs-lookup"><span data-stu-id="7d7df-123">You'll need to `cd` back to the KeyVaultDemoApp directory if you haven't already.</span></span>

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*

az webapp deployment source config-zip \
    --src site.zip \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="7d7df-124">När du har fått ett resultat som visar att webbplatsen har distribuerats, öppnar du `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="7d7df-124">Once you get a result that indicates the site has deployed, open `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in a browser.</span></span> <span data-ttu-id="7d7df-125">Du bör nu se det hemliga värdet **reindeer_flotilla**.</span><span class="sxs-lookup"><span data-stu-id="7d7df-125">You should see the secret value, **reindeer_flotilla**.</span></span>

<span data-ttu-id="7d7df-126">Din app är klar och distribuerad!</span><span class="sxs-lookup"><span data-stu-id="7d7df-126">Your app is finished and deployed!</span></span>