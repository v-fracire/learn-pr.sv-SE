<span data-ttu-id="2f16a-101">Genom att ställa in miljövariabler i dina containerinstanser kan du skapa en dynamisk konfiguration av programmet eller skriptet som körs av containern.</span><span class="sxs-lookup"><span data-stu-id="2f16a-101">Setting environment variables in your container instances allows you to provide dynamic configuration of the application or script run by the container.</span></span> <span data-ttu-id="2f16a-102">Miljövariablerna anges med hjälp av Azure CLI, PowerShell eller Azure-portalen när containern skapas.</span><span class="sxs-lookup"><span data-stu-id="2f16a-102">Environment variables are set using the Azure CLI, PowerShell, or the Azure portal at container creation time.</span></span> <span data-ttu-id="2f16a-103">Säkra miljövariabler används till att förhindra att känslig information visas i utdata från containeråtgärderna.</span><span class="sxs-lookup"><span data-stu-id="2f16a-103">Secured environment variables are used to prevent sensitive information from being displayed in container operations output.</span></span>

<span data-ttu-id="2f16a-104">I den här utbildningsenheten skapar du en Cosmos DB-instans och sedan en Azure Container Instance-körning med anslutningsinformationen för Azure Cosmos DB-instansen som är lagrad som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="2f16a-104">In this unit, an Azure Cosmos DB instance is created, and then an Azure container instance runs with the connection information of the Azure Cosmos DB instance stored as environment variables.</span></span> <span data-ttu-id="2f16a-105">En app i containern använder variablerna till att skriva och läsa data från Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2f16a-105">An application in the container uses the variables to write and read data from Azure Cosmos DB.</span></span> <span data-ttu-id="2f16a-106">I den här utbildningsenheten skapar du både en miljövariabel och en säker miljövariabel.</span><span class="sxs-lookup"><span data-stu-id="2f16a-106">In this unit, you will create both an environment variable and a secured environment variable.</span></span>

## <a name="deploy-azure-cosmos-db"></a><span data-ttu-id="2f16a-107">Distribuera Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2f16a-107">Deploy Azure Cosmos DB</span></span>

<span data-ttu-id="2f16a-108">Skapa en Azure Cosmos DB-instans med kommandot `az Azure Cosmos DB create`.</span><span class="sxs-lookup"><span data-stu-id="2f16a-108">Create the Azure Cosmos DB instance with the `az Azure Cosmos DB create` command.</span></span> <span data-ttu-id="2f16a-109">I det här exemplet placeras dessutom adressen till Azure Cosmos DB-slutpunkten i en variabel med namnet *COSMOS_DB_ENDPOINT*.</span><span class="sxs-lookup"><span data-stu-id="2f16a-109">This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.</span></span>

<span data-ttu-id="2f16a-110">Kommandot tar några minuter att slutföra:</span><span class="sxs-lookup"><span data-stu-id="2f16a-110">This command can take a few minutes to complete:</span></span>

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group myResourceGroup --name aci-cosmos --query documentEndpoint -o tsv)
```

<span data-ttu-id="2f16a-111">Hämta sedan anslutningsnyckeln för Azure Cosmos DB med kommandot `az cosmosdb list-keys` och lagra den i en variabel med namnet *COSMOS_DB_MASTERKEY*:</span><span class="sxs-lookup"><span data-stu-id="2f16a-111">Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*:</span></span>

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group myResourceGroup --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a><span data-ttu-id="2f16a-112">Distribuera en containerinstans</span><span class="sxs-lookup"><span data-stu-id="2f16a-112">Deploy a container instance</span></span>

<span data-ttu-id="2f16a-113">Skapa en Azure-containerinstans med kommandot `az container create`.</span><span class="sxs-lookup"><span data-stu-id="2f16a-113">Create an Azure container instance using the `az container create` command.</span></span> <span data-ttu-id="2f16a-114">Observera att två miljövariabler skapas, `COSMOS_DB_ENDPOINT` och `COSMOS_DB_ENDPOINT`.</span><span class="sxs-lookup"><span data-stu-id="2f16a-114">Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_ENDPOINT`.</span></span> <span data-ttu-id="2f16a-115">De här variablerna innehåller de värden som behövs för att ansluta till Azure Cosmos DB-instansen:</span><span class="sxs-lookup"><span data-stu-id="2f16a-115">These variables hold the values needed to connect to the Azure Cosmos DB instance:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

<span data-ttu-id="2f16a-116">När du har skapat containern kan du hämta IP-adressen med kommandot `az container show`:</span><span class="sxs-lookup"><span data-stu-id="2f16a-116">Once the container has been created, get the IP address with the `az container show` command:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query ipAddress.ip --output tsv
```

<span data-ttu-id="2f16a-117">Öppna en webbläsare och navigera till containerns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2f16a-117">Open up a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="2f16a-118">Du bör se nedanstående app.</span><span class="sxs-lookup"><span data-stu-id="2f16a-118">You should see the following application.</span></span> <span data-ttu-id="2f16a-119">När du röstar lagras rösten i Azure Cosmos DB-instansen.</span><span class="sxs-lookup"><span data-stu-id="2f16a-119">When casing a vote, the vote is stored in the Azure Cosmos DB instance.</span></span>

![Azure-röstningsprogram med två alternativ, katter eller hundar.](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a><span data-ttu-id="2f16a-121">Säkra miljövariabler</span><span class="sxs-lookup"><span data-stu-id="2f16a-121">Secured environment variables</span></span>

<span data-ttu-id="2f16a-122">I föregående övning skapade vi en container med anslutningsinformation för Azure Cosmos DB lagrad i två miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="2f16a-122">In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables.</span></span> <span data-ttu-id="2f16a-123">Som standard visas miljövariabler i klartext i Azure Portal och i kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="2f16a-123">By default, environment variables are displayed in the Azure portal and command-line tools in plain text.</span></span>

<span data-ttu-id="2f16a-124">Om du till exempel hämtar information om containern som skapades i föregående övning med kommandot `az container show` visas miljövariablerna i klartext:</span><span class="sxs-lookup"><span data-stu-id="2f16a-124">For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query containers[0].environmentVariables
```

<span data-ttu-id="2f16a-125">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="2f16a-125">Example output:</span></span>

```bash
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": "https://aci-cosmos.documents.azure.com:443/"
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": "Xm5BwdLlCllBvrR26V00000000S2uOusuglhzwkE7dOPMBQ3oA30n3rKd8PKA13700000000095ynys863Ghgw=="
  }
]
```

Med säkra miljövariabler förhindrar du att utdata visas i klartext. <span data-ttu-id="2f16a-127">Om du vill använda säkra miljövariabler ersätter du argumentet `--environment-variables` med argumentet `--secure-environment-variables`.</span><span class="sxs-lookup"><span data-stu-id="2f16a-127">To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.</span></span>

<span data-ttu-id="2f16a-128">Kör följande exempel för att skapa en container med namnet *aci-demo-secure* som använder säkra miljövariabler:</span><span class="sxs-lookup"><span data-stu-id="2f16a-128">Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

<span data-ttu-id="2f16a-129">När containern nu returneras med kommandot `az container show` visas inte miljövariablerna:</span><span class="sxs-lookup"><span data-stu-id="2f16a-129">Now, when the container is returned with the `az container show` command, the environment variables are not displayed:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-secure --query containers[0].environmentVariables
```

<span data-ttu-id="2f16a-130">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="2f16a-130">Example output:</span></span>

```bash
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": null
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": null
  }
]
```

## <a name="summary"></a><span data-ttu-id="2f16a-131">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2f16a-131">Summary</span></span>

<span data-ttu-id="2f16a-132">I den här utbildningsenheten skapade du en Azure Cosmos DB-instans och en Azure-containerinstans.</span><span class="sxs-lookup"><span data-stu-id="2f16a-132">In this unit, you created an Azure Cosmos DB instance and an Azure container instance.</span></span> <span data-ttu-id="2f16a-133">Du ställde in miljövariabler i containerinstansen så att appen som kördes i containern kunde ansluta till Azure Cosmos DB-instansen.</span><span class="sxs-lookup"><span data-stu-id="2f16a-133">Environment variables were set in the container instance such that the application running inside of the container was able to connect to the Azure Cosmos DB instance.</span></span>

<span data-ttu-id="2f16a-134">I nästa utbildningsenhet monterar du datavolymer vid en Azure-containerinstans för bevarande av data.</span><span class="sxs-lookup"><span data-stu-id="2f16a-134">In the next unit, you will mount data volumes to an Azure container instance for data persistence.</span></span>
