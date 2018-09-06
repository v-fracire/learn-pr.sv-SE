<span data-ttu-id="a7cde-101">Du kan hämta containeravbildningar från Azure Container Registry från flera plattformar för containerhantering, som Azure Container Instances, Azure Kubernetes Registry och Docker för Windows eller Mac.</span><span class="sxs-lookup"><span data-stu-id="a7cde-101">Container images can be pulled from Azure Container Registry from many container management platforms, such as Azure Container Instances, Azure Kubernetes Registry, and Docker for Windows or Mac.</span></span> <span data-ttu-id="a7cde-102">När du kör containeravbildningar från Azure Container Registry kan du behöva autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a7cde-102">When running container images from Azure Container Registry, authentication credentials may be needed.</span></span> <span data-ttu-id="a7cde-103">Du bör använda ett huvudnamn för Azure-tjänsten till autentisering i Container Registry.</span><span class="sxs-lookup"><span data-stu-id="a7cde-103">It is recommended to use an Azure service principal for authentication with Container Registry.</span></span> <span data-ttu-id="a7cde-104">Dessutom bör du skydda autentiseringsuppgifterna för Azure-tjänstens huvudnamn i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a7cde-104">Furthermore, it is also recommended to secure the Azure service principal credentials in Azure Key Vault.</span></span>

<span data-ttu-id="a7cde-105">I den här utbildningsenheten får du skapa ett huvudnamn för ditt Azure-containerregister, lagra det i Key Vault och sedan distribuera containern till Azure Container Instances med hjälp av autentiseringsuppgifterna för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a7cde-105">In this unit, you will create a service principal for your Azure container registry, store it in Azure Key Vault, and then deploy the container to Azure Container Instances using the service principal's credentials.</span></span>

## <a name="configure-registry-authentication"></a><span data-ttu-id="a7cde-106">Konfigurera registerautentisering</span><span class="sxs-lookup"><span data-stu-id="a7cde-106">Configure registry authentication</span></span>

<span data-ttu-id="a7cde-107">Du bör använda huvudnamn för åtkomst till Azure-containerregister i alla produktionsscenarier.</span><span class="sxs-lookup"><span data-stu-id="a7cde-107">All production scenarios should use service principals to access an Azure container registry.</span></span> <span data-ttu-id="a7cde-108">Med ett huvudnamn för tjänsten får du rollbaserad åtkomstkontroll (RBAC) för containeravbildningarna.</span><span class="sxs-lookup"><span data-stu-id="a7cde-108">Service principals allow you to provide role-based access control (RBAC) to your container images.</span></span> <span data-ttu-id="a7cde-109">Du kan till exempel konfigurera ett huvudnamn för tjänsten som enbart har hämtningsåtkomst till ett register.</span><span class="sxs-lookup"><span data-stu-id="a7cde-109">For example, you can configure a service principal with pull-only access to a registry.</span></span>

<span data-ttu-id="a7cde-110">Om du inte redan har ett valv i Azure Key Vault skapar du ett med följande kommandon i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a7cde-110">If you don't already have a vault in Azure Key Vault, create one with the Azure CLI using the following commands.</span></span>

<span data-ttu-id="a7cde-111">Skapa först en variabel med namnet på ditt containerregister.</span><span class="sxs-lookup"><span data-stu-id="a7cde-111">First, create a variable with the name of your container registry.</span></span> <span data-ttu-id="a7cde-112">Den här variabeln används i hela utbildningsenheten.</span><span class="sxs-lookup"><span data-stu-id="a7cde-112">This variable is used throughout this unit.</span></span>

```azurecli
ACR_NAME=<acrName>
```

<span data-ttu-id="a7cde-113">Skapa ett Azure-nyckelvalv med kommandot `az keyvault create`.</span><span class="sxs-lookup"><span data-stu-id="a7cde-113">Create an Azure key vault with the `az keyvault create` command.</span></span>

```azurecli
az keyvault create --resource-group myResourceGroup --name $ACR_NAME-keyvault
```

<span data-ttu-id="a7cde-114">Nu måste du skapa ett huvudnamn för tjänsten och lagra autentiseringsuppgifterna för det i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a7cde-114">Now, you need to create a service principal and store its credentials in your key vault.</span></span>

<span data-ttu-id="a7cde-115">Använd cmdleten `az ad sp create-for-rbac` till att skapa tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a7cde-115">Use the `az ad sp create-for-rbac` command to create the service principal.</span></span> <span data-ttu-id="a7cde-116">Argumentet `--role` konfigurerar huvudnamnet för tjänsten med rollen *läsare*, vilket endast ger hämtningsåtkomst till registret.</span><span class="sxs-lookup"><span data-stu-id="a7cde-116">The `--role` argument configures the service principal with the *reader* role, which grants it pull-only access to the registry.</span></span> <span data-ttu-id="a7cde-117">Om du vill bevilja både push- och hämtningsåtkomst ändrar du argumentet `--role` till *deltagare*.</span><span class="sxs-lookup"><span data-stu-id="a7cde-117">To grant both push and pull access, change the `--role` argument to *contributor*.</span></span>

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

<span data-ttu-id="a7cde-118">Så här ser utdata ut när du har skapat tjänsten huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a7cde-118">This is what the output of the service principal creation will look like.</span></span> <span data-ttu-id="a7cde-119">Anteckna värdena för `appId` och `password`.</span><span class="sxs-lookup"><span data-stu-id="a7cde-119">Take note of the `appId` and the `password` values.</span></span> <span data-ttu-id="a7cde-120">De här värdena lagras i Azure-nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a7cde-120">These will be stored in the Azure key vault.</span></span>

```bash
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

<span data-ttu-id="a7cde-121">Använd sedan kommandot `az keyvault secret set` till att lagra *appId*-värdet för tjänstens huvudnamn i valvet.</span><span class="sxs-lookup"><span data-stu-id="a7cde-121">Next, use the `az keyvault secret set` command to store the service principal's *appId* in the vault.</span></span> <span data-ttu-id="a7cde-122">Ersätt `<appId>` med `appId`-värdet för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a7cde-122">Replace `<appId>` with the `appId` of the service principal.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <appId>
```

<span data-ttu-id="a7cde-123">Använd nu kommandot `az keyvault secret set` till att lagra *password*-värdet för tjänstens huvudnamn i valvet.</span><span class="sxs-lookup"><span data-stu-id="a7cde-123">Now, use the `az keyvault secret set` command to store the service principal's *password* in the vault.</span></span> <span data-ttu-id="a7cde-124">Ersätt `<password>` med `password`-värdet för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a7cde-124">Replace `<password>` with the `password` of the service principal.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
```

<span data-ttu-id="a7cde-125">Du har skapat ett Azure-nyckelvalv och lagrat två hemligheter i det:</span><span class="sxs-lookup"><span data-stu-id="a7cde-125">You've created an Azure key vault and stored two secrets in it:</span></span>

* <span data-ttu-id="a7cde-126">`$ACR_NAME-pull-usr`: ID:t för tjänstens huvudnamn som ska användas som **användarnamn** för containerregistret.</span><span class="sxs-lookup"><span data-stu-id="a7cde-126">`$ACR_NAME-pull-usr`: The service principal ID, for use as the container registry **username**.</span></span>
* <span data-ttu-id="a7cde-127">`$ACR_NAME-pull-pwd`: lösenordet för tjänstens huvudnamn som ska användas som **lösenord** för containerregistret.</span><span class="sxs-lookup"><span data-stu-id="a7cde-127">`$ACR_NAME-pull-pwd`: The service principal password, for use as the container registry **password**.</span></span>

<span data-ttu-id="a7cde-128">Nu kan du referera till de här hemligheterna med namn när du eller dina program och tjänster hämtar avbildningar från registret.</span><span class="sxs-lookup"><span data-stu-id="a7cde-128">You can now reference these secrets by name when you or your applications and services pull images from the registry.</span></span>

### <a name="deploy-a-container-with-azure-cli"></a><span data-ttu-id="a7cde-129">Distribuera en container med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a7cde-129">Deploy a container with Azure CLI</span></span>

<span data-ttu-id="a7cde-130">Nu när autentiseringsuppgifterna för tjänstens huvudnamn lagras i Azure Key Vault kan dina program och tjänster använda dem för att få åtkomst till ditt privata register.</span><span class="sxs-lookup"><span data-stu-id="a7cde-130">Now that the service principal credentials are stored in Azure Key Vault, your applications and services can use them to access your private registry.</span></span>

<span data-ttu-id="a7cde-131">Kör kommandot `az container create` för att distribuera en containerinstans.</span><span class="sxs-lookup"><span data-stu-id="a7cde-131">Execute the following `az container create` command to deploy a container instance.</span></span> <span data-ttu-id="a7cde-132">I kommandot används de autentiseringsuppgifter för tjänstens huvudnamn som lagras i Azure Key Vault för autentisering i containerregistret.</span><span class="sxs-lookup"><span data-stu-id="a7cde-132">The command uses the service principal's credentials stored in Azure Key Vault to authenticate to your container registry.</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

<span data-ttu-id="a7cde-133">Hämta Azure-containerinstansens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a7cde-133">Get the IP address of the Azure container instance.</span></span>

```azurecli
az container show --resource-group myResourceGroup --name acr-build --query ipAddress.ip --output table
```

<span data-ttu-id="a7cde-134">Öppna en webbläsare och navigera till containerns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a7cde-134">Open up a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="a7cde-135">Om allt är konfigurerat på rätt sätt bör du se följande resultat:</span><span class="sxs-lookup"><span data-stu-id="a7cde-135">If everything has been configured correctly, you should see the following results:</span></span>

![Webbappsexempel med texten Hello World](../media/hello.png)

## <a name="summary"></a><span data-ttu-id="a7cde-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a7cde-137">Summary</span></span>

<span data-ttu-id="a7cde-138">I den här utbildningsenheten har du lagrat autentiseringsuppgifter för Azure Container Registry i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a7cde-138">In this unit, the Azure Container Registry credentials were stored in Azure Key Vault.</span></span> <span data-ttu-id="a7cde-139">Sedan distribuerade du en containeravbildning till Azure Container Instances med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a7cde-139">A container image was then deployed to Azure Container Instances using the Azure CLI.</span></span> <span data-ttu-id="a7cde-140">I nästa utbildningsenhet får du använda georeplikering i Azure Container Registry för att kopiera containeravbildningen till flera Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="a7cde-140">In the next unit, you will use Azure Container Registry geo-replication to copy the container image to multiple Azure datacenters.</span></span>
