<span data-ttu-id="f3c5b-101">Anta att ditt företag har compute-arbetsbelastningar som distribuerats till flera regioner för att kunna betjäna din distribuerade kundbas med lokal närvaro.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-101">Suppose your company has compute workloads deployed to several regions to make sure you have a local presence to serve your distributed customer base.</span></span> 

<span data-ttu-id="f3c5b-102">Ditt mål är att placera ett containerregister i varje region där avbildningar körs.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-102">Your aim is to place a container registry in each region where images are run.</span></span> <span data-ttu-id="f3c5b-103">Den här strategin tillåter nätverksnära åtgärder, vilket tillåter snabba, tillförlitliga överföringar av bildlager.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-103">This strategy will allow for network-close operations, enabling fast, reliable image layer transfers.</span></span> 

<span data-ttu-id="f3c5b-104">Med georeplikering kan ett Azure-containerregister fungera som ett enda register för flera regioner, med regionala register som har flera huvudregister.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-104">Geo-replication enables an Azure container registry to function as a single registry, serving several regions with multi-master regional registries.</span></span>

<span data-ttu-id="f3c5b-105">Ett georeplikerat register ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f3c5b-105">A geo-replicated registry provides the following benefits:</span></span>

- <span data-ttu-id="f3c5b-106">Namn på register/avbildningar/taggar kan användas i flera regioner</span><span class="sxs-lookup"><span data-stu-id="f3c5b-106">Single registry/image/tag names can be used across multiple regions</span></span>
- <span data-ttu-id="f3c5b-107">Nätverksnära registeråtkomst från regionala distributioner</span><span class="sxs-lookup"><span data-stu-id="f3c5b-107">Network-close registry access from regional deployments</span></span>
- <span data-ttu-id="f3c5b-108">Inga ytterligare avgifter för utgående trafik eftersom avbildningarna hämtas från ett lokalt, replikerat register i samma region som containervärden</span><span class="sxs-lookup"><span data-stu-id="f3c5b-108">No additional egress fees, as images are pulled from a local, replicated registry in the same region as your container host</span></span>
- <span data-ttu-id="f3c5b-109">Enkel hantering av ett register i flera regioner</span><span class="sxs-lookup"><span data-stu-id="f3c5b-109">Single management of a registry across multiple regions</span></span>

## <a name="replicate-an-image-to-multiple-locations"></a><span data-ttu-id="f3c5b-110">Replikera en avbildning till flera platser</span><span class="sxs-lookup"><span data-stu-id="f3c5b-110">Replicate an image to multiple locations</span></span>

<span data-ttu-id="f3c5b-111">Du kommer att använda Azure CLI-kommandot `az acr replication create` för att replikera dina containeravbildningar från en region till en annan.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-111">You'll use the `az acr replication create` Azure CLI command to replicate your container images from one region to another.</span></span> <span data-ttu-id="f3c5b-112">I det här exemplet skapar du en replikering för regionen `japaneast`.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-112">In this example, you'll create a replication for the `japaneast` region.</span></span> <span data-ttu-id="f3c5b-113">Ersätt `<acrName>` med namnet på ditt Container Registry.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-113">Update `<acrName>` with the name of your Container Registry.</span></span>

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

<span data-ttu-id="f3c5b-114">Dina utdata bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="f3c5b-114">The output should look similar to the following:</span></span>

```output
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myACR0007/replications/japaneast",
  "location": "japaneast",
  "name": "japaneast",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": {
    "displayStatus": "Syncing",
    "message": null,
    "timestamp": "2018-08-15T20:22:09.275792+00:00"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries/replications"
}
```

<span data-ttu-id="f3c5b-115">Som ett sista steg kan du hämta alla containeravbildningsrepliker som skapats.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-115">As a final step, you're able to retrieve all container image replicas created.</span></span> <span data-ttu-id="f3c5b-116">Hämta den här listan med kommandot `az acr replication list`.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-116">You'll use the `az acr replication list` command to retrieve this list.</span></span> <span data-ttu-id="f3c5b-117">Ersätt `<acrName>` med namnet på ditt Container Registry.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-117">Update `<acrName>` with the name of your Container Registry.</span></span>

```azurecli
az acr replication list --registry <acrName> --output table
```

<span data-ttu-id="f3c5b-118">Utdata bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="f3c5b-118">The output should look similar to the following:</span></span>

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

<span data-ttu-id="f3c5b-119">Tänk på att du inte är begränsad till Azure CLI för att lista dina avbildningsrepliker.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-119">Keep in mind that you are not limited to the Azure CLI to list your image replicas.</span></span> <span data-ttu-id="f3c5b-120">Om du väljer `Replications` inom Azure-portalen för ett Azure Container Registry så visas en karta över aktuella replikeringar.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-120">From within the Azure portal, selecting `Replications` for an Azure Container Registry displays a map that details current replications.</span></span> <span data-ttu-id="f3c5b-121">Du kan replikera containeravbildningar till ytterligare regioner genom att välja regionerna på kartan.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-121">Container images can be replicated to additional regions by selecting the regions on the map.</span></span>

![Karta över containerreplikering i Azure-portalen](../media/replication-map.png)

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]
 

## <a name="summary"></a><span data-ttu-id="f3c5b-123">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f3c5b-123">Summary</span></span>

<span data-ttu-id="f3c5b-124">I den här modulen har du replikerat en containeravbildning till flera Azure-datacenter med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f3c5b-124">You've now successfully replicated a container image to multiple Azure datacenters using the Azure CLI.</span></span> 