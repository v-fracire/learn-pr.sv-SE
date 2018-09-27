<span data-ttu-id="9eda7-101">Du kan hämta containeravbildningar från Azure Container Registry med hjälp av flera containerhanteringsplattformar, t.ex. Azure Container Instances, Azure Kubernetes Registry och Docker för Windows eller Mac.</span><span class="sxs-lookup"><span data-stu-id="9eda7-101">Container images can be pulled from Azure Container Registry using many container management platforms, such as Azure Container Instances, Azure Kubernetes Registry, and Docker for Windows or Mac.</span></span> <span data-ttu-id="9eda7-102">När du kör containeravbildningar från Azure Container Registry kan du behöva autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9eda7-102">When running container images from Azure Container Registry, authentication credentials may be needed.</span></span> 

<span data-ttu-id="9eda7-103">Du bör använda ett huvudnamn för Azure-tjänsten till autentisering i Container Registry.</span><span class="sxs-lookup"><span data-stu-id="9eda7-103">It is recommended to use an Azure service principal for authentication with Container Registry.</span></span> <span data-ttu-id="9eda7-104">Du bör dessutom skydda autentiseringsuppgifterna för Azure-tjänstens huvudnamn i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="9eda7-104">It is also recommended to secure the Azure service principal credentials in Azure Key Vault.</span></span> <span data-ttu-id="9eda7-105">I den här enheten följer vi den rekommenderade metoden.</span><span class="sxs-lookup"><span data-stu-id="9eda7-105">In this unit we'll follow the recommended approach.</span></span>

<span data-ttu-id="9eda7-106">I vår övning använder vi dock det inbyggda administratörskonto som kan aktiveras i alla Azure Container-register.</span><span class="sxs-lookup"><span data-stu-id="9eda7-106">However, for our live practice we will use the built-in admin account that can be enabled in all Azure Container Registries.</span></span> <span data-ttu-id="9eda7-107">Administratörskontot fungerar med de kostnadsfria sandbox-resurserna.</span><span class="sxs-lookup"><span data-stu-id="9eda7-107">The admin account works with the free sandbox resources.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="9eda7-108">Skapa en variabel med containerregistrets namn med små bokstäver (t.ex. ”mincontainer” istället för ”MinContainer”).</span><span class="sxs-lookup"><span data-stu-id="9eda7-108">Create a variable with the name of your container registry in lowercase (for example, instead of "MyContainer" make the value "mycontainer").</span></span> <span data-ttu-id="9eda7-109">Den här variabeln används i hela utbildningsenheten.</span><span class="sxs-lookup"><span data-stu-id="9eda7-109">This variable is used throughout this unit.</span></span>

```azurecli
ACR_NAME=<acrName>
```

### <a name="service-principal"></a><span data-ttu-id="9eda7-110">Tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="9eda7-110">Service Principal</span></span>

<span data-ttu-id="9eda7-111">Vi ska skapa ett tjänstens huvudnamn för produktionsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9eda7-111">For a production application, we would want to create a service principal here.</span></span> <span data-ttu-id="9eda7-112">Kom ihåg att **detta inte fungerar i vår sandbox-miljö**, men det är bästa praxis att följa i dina egna system.</span><span class="sxs-lookup"><span data-stu-id="9eda7-112">Remember **this won't work in our sandbox environment**, but it's a best practice to follow in your own systems.</span></span> <span data-ttu-id="9eda7-113">I vår övning kommer du att använda anvisningarna för administratörskontot nedan.</span><span class="sxs-lookup"><span data-stu-id="9eda7-113">For our live practice, you'll use the Admin Account instructions below.</span></span>

<span data-ttu-id="9eda7-114">Skapa tjänstens huvudnamn med kommandot `az ad sp create-for-rbac`.</span><span class="sxs-lookup"><span data-stu-id="9eda7-114">The `az ad sp create-for-rbac` command can be used to create the service principal.</span></span> <span data-ttu-id="9eda7-115">Argumentet `--role` konfigurerar huvudnamnet för tjänsten med rollen *läsare*, vilket endast ger hämtningsåtkomst till registret.</span><span class="sxs-lookup"><span data-stu-id="9eda7-115">The `--role` argument configures the service principal with the *reader* role, which grants it pull-only access to the registry.</span></span> <span data-ttu-id="9eda7-116">Om du vill bevilja både push- och hämtningsåtkomst ändrar du argumentet `--role` till *deltagare*.</span><span class="sxs-lookup"><span data-stu-id="9eda7-116">To grant both push and pull access, you would change the `--role` argument to *contributor*.</span></span>

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

<span data-ttu-id="9eda7-117">Så här ser utdata ut när du har skapat tjänsten huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="9eda7-117">This is what the output of the service principal creation would look like.</span></span> <span data-ttu-id="9eda7-118">Anteckna värdena för `appId` och `password`.</span><span class="sxs-lookup"><span data-stu-id="9eda7-118">Take note of the `appId` and the `password` values.</span></span> <span data-ttu-id="9eda7-119">De här värdena lagras i Azure-nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="9eda7-119">These should be stored in the Azure key vault.</span></span>

```output
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="admin-account"></a><span data-ttu-id="9eda7-120">Administratörskonto</span><span class="sxs-lookup"><span data-stu-id="9eda7-120">Admin Account</span></span>

<span data-ttu-id="9eda7-121">Azure Container-registrer har ett inbyggt administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="9eda7-121">Azure Container Registries come with a built-in Admin account.</span></span> <span data-ttu-id="9eda7-122">Det här inte är kopplat till Azure AD eller rollbaserade åtkomstkontroller och **bör därför endast användas i testsyfte**.</span><span class="sxs-lookup"><span data-stu-id="9eda7-122">This isn't tied to Azure AD or role based access controls and therefore **should only be used for testing**.</span></span> 

1. <span data-ttu-id="9eda7-123">Aktivera administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="9eda7-123">Enable the Admin Account.</span></span>
    ```azurecli
      az acr update -n $ACR_NAME --admin-enabled true
    ```

2. <span data-ttu-id="9eda7-124">Skapa en fråga för att hämta ett automatiskt genererat användarnamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="9eda7-124">Query to get the autogenerated username and password</span></span>

    ```azurecli
      az acr credential show --name $ACR_NAME
    ```

<span data-ttu-id="9eda7-125">Dina utdata ser ut ungefär som nedan.</span><span class="sxs-lookup"><span data-stu-id="9eda7-125">The output is similar to below.</span></span> <span data-ttu-id="9eda7-126">Notera `username` och `value` tillsammans med `name`-lösenordet.</span><span class="sxs-lookup"><span data-stu-id="9eda7-126">Take note of the `username` and the `value` paired with the `name` "password".</span></span> <span data-ttu-id="9eda7-127">Du sparar dem i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="9eda7-127">You'll save these into key vault.</span></span>

```output
{  "passwords": [
    {
      "name": "password",
      "value": "aaaaa"
    },
    {
      "name": "password2",
      "value": "bbbbb"
    }
  ],
  "username": "ccccc"
}
```

### <a name="save-the-username-and-password-to-the-key-vault"></a><span data-ttu-id="9eda7-128">Spara användarnamnet och lösenordet i nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="9eda7-128">Save the username and password to the key vault</span></span>

1. <span data-ttu-id="9eda7-129">Skapa ett Azure Key Vault med kommandot `az keyvault create`.</span><span class="sxs-lookup"><span data-stu-id="9eda7-129">Create an Azure Key Vault with the `az keyvault create` command.</span></span>

    ```azurecli
    az keyvault create --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACR_NAME-keyvault
    ```

1. <span data-ttu-id="9eda7-130">Spara ACR-användarnamnet i valvet genom att använda kommandot `az keyvault secret set`.</span><span class="sxs-lookup"><span data-stu-id="9eda7-130">Use the `az keyvault secret set` command to store the username for the ACR in the vault.</span></span> <span data-ttu-id="9eda7-131">Om du använder tjänstens huvudnamn använder du app-ID:t för det här värdet.</span><span class="sxs-lookup"><span data-stu-id="9eda7-131">If you were using service principals you'd use the appId for this value.</span></span> <span data-ttu-id="9eda7-132">Eftersom vi använder administratörskontot kan vi spara användarnamnet från frågan ovan.</span><span class="sxs-lookup"><span data-stu-id="9eda7-132">Since we're using the admin account this time we'll save the username from our query above.</span></span> <span data-ttu-id="9eda7-133">Ange kommandot nedan, och glöm inte att ersätta `<username>`.</span><span class="sxs-lookup"><span data-stu-id="9eda7-133">Enter the command below, don't forget to replace `<username>`.</span></span>

    ```azurecli
    az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <username>
    ```

1. <span data-ttu-id="9eda7-134">Använd kommandot `az keyvault secret set` till att lagra *lösenordet* i valvet.</span><span class="sxs-lookup"><span data-stu-id="9eda7-134">Use the `az keyvault secret set` command to store the *password* in the vault.</span></span> <span data-ttu-id="9eda7-135">Ersätt `<password>` med `password` från frågan ovan.</span><span class="sxs-lookup"><span data-stu-id="9eda7-135">Replace `<password>` with the `password` from the query above.</span></span>

    ```azurecli
    az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
    ```

<span data-ttu-id="9eda7-136">Du har nu skapat ett Azure-nyckelvalv och lagrat två hemligheter i det:</span><span class="sxs-lookup"><span data-stu-id="9eda7-136">You've now created an Azure key vault and stored two secrets in it:</span></span>

* <span data-ttu-id="9eda7-137">`$ACR_NAME-pull-usr`: **användarnamn** för containerregister.</span><span class="sxs-lookup"><span data-stu-id="9eda7-137">`$ACR_NAME-pull-usr`: the container registry **username**.</span></span>
* <span data-ttu-id="9eda7-138">`$ACR_NAME-pull-pwd`: **lösenord** för containerregister.</span><span class="sxs-lookup"><span data-stu-id="9eda7-138">`$ACR_NAME-pull-pwd`: the container registry **password**.</span></span>

<span data-ttu-id="9eda7-139">Nu kan du referera till de här hemligheterna med namn när du eller dina program och tjänster hämtar avbildningar från registret.</span><span class="sxs-lookup"><span data-stu-id="9eda7-139">You can now reference these secrets by name when you or your applications and services pull images from the registry.</span></span>

### <a name="deploy-a-container-with-azure-cli"></a><span data-ttu-id="9eda7-140">Distribuera en container med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9eda7-140">Deploy a container with Azure CLI</span></span>

<span data-ttu-id="9eda7-141">Nu när autentiseringsuppgifterna för tjänstens huvudnamn lagras i Azure Key Vault kan dina program och tjänster använda dem för att få åtkomst till ditt privata register.</span><span class="sxs-lookup"><span data-stu-id="9eda7-141">Now that the service principal credentials are stored in Azure Key Vault, your applications and services can use them to access your private registry.</span></span>

<span data-ttu-id="9eda7-142">Kör kommandot `az container create` för att distribuera en containerinstans.</span><span class="sxs-lookup"><span data-stu-id="9eda7-142">Execute the following `az container create` command to deploy a container instance.</span></span> <span data-ttu-id="9eda7-143">I kommandot används de autentiseringsuppgifter för tjänstens huvudnamn som lagras i Azure Key Vault för autentisering i containerregistret.</span><span class="sxs-lookup"><span data-stu-id="9eda7-143">The command uses the service principal's credentials stored in Azure Key Vault to authenticate to your container registry.</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name acr-tasks \
    --image $ACR_NAME.azurecr.io/helloacrtasks:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --location eastus \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

<span data-ttu-id="9eda7-144">Hämta Azure-containerinstansens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9eda7-144">Get the IP address of the Azure container instance.</span></span>

```azurecli
az container show --resource-group  <rgn>[sandbox resource group name]</rgn> --name acr-tasks --query ipAddress.ip --output table
```

<span data-ttu-id="9eda7-145">Öppna en webbläsare och navigera till containerns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9eda7-145">Open a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="9eda7-146">Om allt är konfigurerat på rätt sätt bör du se följande resultat:</span><span class="sxs-lookup"><span data-stu-id="9eda7-146">If everything has been configured correctly, you should see the following results:</span></span>

![Exempel på ett webbprogram med texten Hello World](../media/hello.png)

