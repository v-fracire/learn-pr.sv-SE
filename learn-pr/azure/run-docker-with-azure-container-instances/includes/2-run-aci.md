<span data-ttu-id="a3ca1-101">Här skapar du en container i Azure och gör den tillgänglig på Internet via ett fullständigt kvalificerat domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="a3ca1-101">Here, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="why-use-azure-container-instances"></a><span data-ttu-id="a3ca1-102">Varför är det bra att använda Azure Container Instances?</span><span class="sxs-lookup"><span data-stu-id="a3ca1-102">Why use Azure Container Instances?</span></span>

<span data-ttu-id="a3ca1-103">Azure Container Instances är användbart för scenarier som kan fungera i isolerade containrar, däribland enkla program, automatisering av uppgifter och byggjobb.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-103">Azure Container Instances is useful for scenarios that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="a3ca1-104">Azure Container Instances har följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a3ca1-104">Azure Container Instances offers the following benefits:</span></span>

- <span data-ttu-id="a3ca1-105">**Snabb start**: starta containrar på bara några sekunder.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-105">**Fast startup**: Launch containers in seconds.</span></span>
- <span data-ttu-id="a3ca1-106">**Fakturering per sekund**: kostnader debiteras bara när containern körs.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-106">**Per second billing**: Incur costs only while the container is running.</span></span>
- <span data-ttu-id="a3ca1-107">**Säkerhet på hypervisornivå**: isolera programmet lika fullständigt som det skulle vara i en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-107">**Hypervisor-level security**: Isolate your application as completely as it would be in a VM.</span></span>
- <span data-ttu-id="a3ca1-108">**Anpassade storlekar**: ange exakta värden för processorkärnor och minne.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-108">**Custom sizes**: Specify exact values for CPU cores and memory.</span></span>
- <span data-ttu-id="a3ca1-109">**Beständig lagring**: montera Azure Files-resurser direkt till en container för att hämta och bevara tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-109">**Persistent storage**: Mount Azure Files shares directly to a container to retrieve and persist state.</span></span>
- <span data-ttu-id="a3ca1-110">**Linux och Windows**: schemalägg både Windows- och Linux-containrar med hjälp av samma API.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-110">**Linux and Windows**: Schedule both Windows and Linux containers using the same API.</span></span>

<span data-ttu-id="a3ca1-111">För scenarier där du behöver fullständig containerorkestrering, som tjänstidentifiering för flera containrar, autoskalning och koordinerade programuppgraderingar rekommenderar vi Azure Kubernetes Service (AKS).</span><span class="sxs-lookup"><span data-stu-id="a3ca1-111">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend Azure Kubernetes Service (AKS).</span></span>

## <a name="create-a-container"></a><span data-ttu-id="a3ca1-112">Skapa en container</span><span class="sxs-lookup"><span data-stu-id="a3ca1-112">Create a container</span></span>

<span data-ttu-id="a3ca1-113">Du skapar en container genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp i kommandot **az container create**.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-113">You create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command.</span></span> <span data-ttu-id="a3ca1-114">Du kan också göra containern tillgänglig på Internet genom att ange en DNS-namnetikett.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-114">You can optionally expose the container to the Internet by specifying a DNS name label.</span></span> <span data-ttu-id="a3ca1-115">I det här exemplet distribuerar du en container som är värd för en liten webbapp.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-115">In this example, you deploy a container that hosts a small web app.</span></span> <span data-ttu-id="a3ca1-116">Du kan också välja plats för avbildningen – standardvärdet nedan är ”Östra USA”, men du kan ändra den till en plats nära dig i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-116">You can also select the location to place the image - we're defaulting to "East US" below, but you can change it to a location close to you from the following list.</span></span>

<span data-ttu-id="a3ca1-117"><!-- TODO: fix region list so it's not hardcoded here --> Med den kostnadsfria sandbox-miljön kan du skapa resurser i en delmängd av Azures globala regioner.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-117"><!-- TODO: fix region list so it's not hardcoded here --> The free sandbox allows you to create resources in a subset of Azure's global regions.</span></span> <span data-ttu-id="a3ca1-118">Välj en region från följande lista när du skapar resurser:</span><span class="sxs-lookup"><span data-stu-id="a3ca1-118">Select a region from the following list when creating any resources:</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="a3ca1-119">- Västra USA 2 - Centrala USA - Östra USA - Västeuropa - Sydostasien</span><span class="sxs-lookup"><span data-stu-id="a3ca1-119">- westus2 - centralus - eastus - westeurope - southeastasia</span></span> :::column-end:::
:::row-end:::

<span data-ttu-id="a3ca1-120">Kör följande kommando i Cloud Shell för att starta en containerinstans.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-120">Execute the following command in the Cloud Shell to start a container instance.</span></span> <span data-ttu-id="a3ca1-121">Värdet `--dns-name-label` måste vara unikt i den Azure-region där du skapar instansen, så du kan behöva ändra `[dns-name]` till något unikt.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-121">The `--dns-name-label` value must be unique within the Azure region you create the instance, so you will need to replace `[dns-name]` with something unique.</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label [dns-name] \
    --location eastus
```

<span data-ttu-id="a3ca1-122">Om några sekunder bör du få ett svar på din begäran.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-122">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="a3ca1-123">Först har containern statusen **Creating** (skapas) men den bör starta inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-123">Initially, the container is in the **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="a3ca1-124">Du kan kontrollera statusen med kommandot `az container show`:</span><span class="sxs-lookup"><span data-stu-id="a3ca1-124">You can check the status using the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

<span data-ttu-id="a3ca1-125">När du kör kommandot visas containerns fullständiga domännamn (FQDN) och dess etableringstillstånd:</span><span class="sxs-lookup"><span data-stu-id="a3ca1-125">When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:</span></span>

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

<span data-ttu-id="a3ca1-126">När containern övergår till statusen **Succeeded** (Lyckades) navigerar du till domännamnet i en webbläsare för att verifiera att det lyckades.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-126">Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser to verify success.</span></span>

<span data-ttu-id="a3ca1-127">Här skapade du en Azure-containerinstans för att köra en webbserver och ett program.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-127">Here, you created an Azure container instance to run a web server and application.</span></span> <span data-ttu-id="a3ca1-128">Du använde även appen via FQDN-värdet för containerinstansen.</span><span class="sxs-lookup"><span data-stu-id="a3ca1-128">You also accessed this application using the FQDN of the container instance.</span></span>