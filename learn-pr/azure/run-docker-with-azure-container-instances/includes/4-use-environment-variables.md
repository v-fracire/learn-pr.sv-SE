<span data-ttu-id="51208-101">Miljövariabler i dina containerinstanser gör att du kan skapa en dynamisk konfiguration av programmet eller skriptet som körs av containern.</span><span class="sxs-lookup"><span data-stu-id="51208-101">Environment variables in your container instances allow you to provide dynamic configuration of the application or script run by the container.</span></span> <span data-ttu-id="51208-102">Du kan använda Azure CLI, PowerShell eller Azure-portalen för att ställa in variabler när du skapar containern.</span><span class="sxs-lookup"><span data-stu-id="51208-102">You can use the Azure CLI, PowerShell, or the Azure portal to set variables when you create the container.</span></span> <span data-ttu-id="51208-103">Säkra miljövariabler används till att förhindra att känslig information visas i utdata från containern.</span><span class="sxs-lookup"><span data-stu-id="51208-103">Secured environment variables are used to prevent sensitive information from being displayed in container output.</span></span>

<span data-ttu-id="51208-104">Här skapar du en Azure Cosmos DB-instans och använder miljövariabler för att skicka anslutningsinformationen till en Azure containerinstans.</span><span class="sxs-lookup"><span data-stu-id="51208-104">Here, you will create an Azure Cosmos DB instance and use environment variables to pass the connection information to an Azure container instance.</span></span> <span data-ttu-id="51208-105">En app i containern använder variablerna till att skriva och läsa data från Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="51208-105">An application in the container uses the variables to write and read data from Cosmos DB.</span></span> <span data-ttu-id="51208-106">Du skapar både en miljövariabel och en säker miljövariabel.</span><span class="sxs-lookup"><span data-stu-id="51208-106">You will create both an environment variable and a secured environment variable.</span></span>

## <a name="deploy-azure-cosmos-db"></a><span data-ttu-id="51208-107">Distribuera Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="51208-107">Deploy Azure Cosmos DB</span></span>

<span data-ttu-id="51208-108">Skapa en Azure Cosmos DB-instans med kommandot `az cosmosdb create`.</span><span class="sxs-lookup"><span data-stu-id="51208-108">Create the Azure Cosmos DB instance with the `az cosmosdb create` command.</span></span> <span data-ttu-id="51208-109">I det här exemplet placeras dessutom adressen till Azure Cosmos DB-slutpunkten i en variabel med namnet *COSMOS_DB_ENDPOINT*.</span><span class="sxs-lookup"><span data-stu-id="51208-109">This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.</span></span> <span data-ttu-id="51208-110">Ange ett unikt namn för `[cosmos-db-name]`.</span><span class="sxs-lookup"><span data-stu-id="51208-110">You'll need to supply a unique name for `[cosmos-db-name]`.</span></span>

<span data-ttu-id="51208-111">Kommandot tar några minuter att slutföra:</span><span class="sxs-lookup"><span data-stu-id="51208-111">This command can take a few minutes to complete:</span></span>

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[sandbox resource group name]</rgn> --name [cosmos-db-name] --query documentEndpoint --output tsv)
```

<span data-ttu-id="51208-112">Hämta sedan anslutningsnyckeln för Azure Cosmos DB med kommandot `az cosmosdb list-keys` och lagra den i en variabel med namnet *COSMOS_DB_MASTERKEY*.</span><span class="sxs-lookup"><span data-stu-id="51208-112">Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*.</span></span> <span data-ttu-id="51208-113">Glöm inte att ersätta `[cosmos-db-name]` igen:</span><span class="sxs-lookup"><span data-stu-id="51208-113">Don't forget to replace `[cosmos-db-name]` again:</span></span>

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[sandbox resource group name]</rgn> --name [cosmos-db-name] --query primaryMasterKey --output tsv)
```

## <a name="deploy-a-container-instance"></a><span data-ttu-id="51208-114">Distribuera en containerinstans</span><span class="sxs-lookup"><span data-stu-id="51208-114">Deploy a container instance</span></span>

<span data-ttu-id="51208-115">Skapa en Azure-containerinstans med kommandot `az container create`.</span><span class="sxs-lookup"><span data-stu-id="51208-115">Create an Azure container instance using the `az container create` command.</span></span> <span data-ttu-id="51208-116">Observera att två miljövariabler skapas, `COSMOS_DB_ENDPOINT` och `COSMOS_DB_MASTERKEY`.</span><span class="sxs-lookup"><span data-stu-id="51208-116">Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_MASTERKEY`.</span></span> <span data-ttu-id="51208-117">De här variablerna innehåller de värden som behövs för att ansluta till Azure Cosmos DB-instansen:</span><span class="sxs-lookup"><span data-stu-id="51208-117">These variables hold the values needed to connect to the Azure Cosmos DB instance:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --location eastus \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

<span data-ttu-id="51208-118">När du har skapat containern kan du hämta IP-adressen med kommandot `az container show`:</span><span class="sxs-lookup"><span data-stu-id="51208-118">Once the container has been created, get the IP address with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo \
    --query ipAddress.ip \
    --output tsv
```

<span data-ttu-id="51208-119">Öppna en webbläsare och navigera till containerns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="51208-119">Open a browser and navigate to the IP address of the container.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="51208-120">Det kan ibland ta en minut eller två att starta containrar helt och kunna ta emot anslutningar.</span><span class="sxs-lookup"><span data-stu-id="51208-120">Sometimes containers take a minute or two to fully start up and be able to receive connections.</span></span> <span data-ttu-id="51208-121">Om det inte finns något svar när du navigerar till IP-adressen i webbläsaren kan du vänta en stund och uppdatera sidan.</span><span class="sxs-lookup"><span data-stu-id="51208-121">If there's no response when you navigate to the IP address in your browser,  wait a few moments and refresh the page.</span></span>

 <span data-ttu-id="51208-122">När appen är tillgänglig bör du se följande program.</span><span class="sxs-lookup"><span data-stu-id="51208-122">Once the app is available, you should see the following application.</span></span> <span data-ttu-id="51208-123">När någon röstar lagras rösten i Azure Cosmos DB-instansen.</span><span class="sxs-lookup"><span data-stu-id="51208-123">When casting a vote, the vote is stored in the Azure Cosmos DB instance.</span></span>

![Azure-röstningsprogram med två alternativ, katter eller hundar.](../media/4-azure-vote.png)

## <a name="secured-environment-variables"></a><span data-ttu-id="51208-125">Säkra miljövariabler</span><span class="sxs-lookup"><span data-stu-id="51208-125">Secured environment variables</span></span>

<span data-ttu-id="51208-126">I föregående övning skapade vi en container med anslutningsinformation för Azure Cosmos DB lagrad i två miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="51208-126">In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables.</span></span> <span data-ttu-id="51208-127">Som standard visas miljövariabler i klartext i Azure Portal och i kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="51208-127">By default, environment variables are displayed in the Azure portal and command-line tools in plain text.</span></span>

<span data-ttu-id="51208-128">Om du till exempel hämtar information om containern som skapades i föregående övning med kommandot `az container show` visas miljövariablerna i klartext:</span><span class="sxs-lookup"><span data-stu-id="51208-128">For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:</span></span>

```azurecli
az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name aci-demo --query containers[0].environmentVariables
```

<span data-ttu-id="51208-129">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="51208-129">Example output:</span></span>

```json
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

Med säkra miljövariabler förhindrar du att utdata visas i klartext. <span data-ttu-id="51208-131">Om du vill använda säkra miljövariabler ersätter du argumentet `--environment-variables` med argumentet `--secure-environment-variables`.</span><span class="sxs-lookup"><span data-stu-id="51208-131">To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.</span></span>

<span data-ttu-id="51208-132">Kör följande exempel för att skapa en container med namnet *aci-demo-secure* som använder säkra miljövariabler:</span><span class="sxs-lookup"><span data-stu-id="51208-132">Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --location eastus \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

<span data-ttu-id="51208-133">När containern nu returneras med kommandot `az container show` visas inte miljövariablerna:</span><span class="sxs-lookup"><span data-stu-id="51208-133">Now, when the container is returned with the `az container show` command, the environment variables are not displayed:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --query containers[0].environmentVariables
```

<span data-ttu-id="51208-134">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="51208-134">Example output:</span></span>

```json
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