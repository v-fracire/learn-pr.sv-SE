<span data-ttu-id="796ac-101">Ett bra tips är att placera ett containerregister i varje region där avbildningar körs, så att åtgärder utförs närmare nätverket och du får snabba och tillförlitliga överföringar på avbildningsnivå.</span><span class="sxs-lookup"><span data-stu-id="796ac-101">As a best practice, placing a container registry in each region where images are run allows network-close operations, enabling fast, reliable image layer transfers.</span></span> <span data-ttu-id="796ac-102">Med georeplikering kan ett Azure-containerregister fungera som ett enda register för flera regioner, med regionala register som har flera huvudregister.</span><span class="sxs-lookup"><span data-stu-id="796ac-102">Geo-replication enables an Azure container registry to function as a single registry, serving multiple regions with multi-master regional registries.</span></span>

<span data-ttu-id="796ac-103">Ett georeplikerat register ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="796ac-103">A geo-replicated registry provides the following benefits:</span></span>

* <span data-ttu-id="796ac-104">Namn på register/avbildningar/taggar kan användas i flera regioner</span><span class="sxs-lookup"><span data-stu-id="796ac-104">Single registry/image/tag names can be used across multiple regions</span></span>
* <span data-ttu-id="796ac-105">Nätverksnära registeråtkomst från regionala distributioner</span><span class="sxs-lookup"><span data-stu-id="796ac-105">Network-close registry access from regional deployments</span></span>
* <span data-ttu-id="796ac-106">Inga ytterligare avgifter för utgående trafik eftersom avbildningarna hämtas från ett lokalt, replikerat register i samma region som containervärden</span><span class="sxs-lookup"><span data-stu-id="796ac-106">No additional egress fees because images are pulled from a local, replicated registry in the same region as your container host</span></span>
* <span data-ttu-id="796ac-107">Enkel hantering av ett register i flera regioner</span><span class="sxs-lookup"><span data-stu-id="796ac-107">Single management of a registry across multiple regions</span></span>

## <a name="replicate-image-to-multiple-locations"></a><span data-ttu-id="796ac-108">Replikera avbildningar till flera platser</span><span class="sxs-lookup"><span data-stu-id="796ac-108">Replicate image to multiple locations</span></span>

<span data-ttu-id="796ac-109">Använd kommandot `az acr replication create` till att replikera dina containeravbildningar till en annan region.</span><span class="sxs-lookup"><span data-stu-id="796ac-109">Use the `az acr replication create` command to replicate your container images to another region.</span></span> <span data-ttu-id="796ac-110">I det här exemplet skapas en replikering för regionen `japaneast`.</span><span class="sxs-lookup"><span data-stu-id="796ac-110">In this example, a replication is created for the `japaneast` region.</span></span> <span data-ttu-id="796ac-111">Ersätt `<acrName>` med namnet på ditt containerregister.</span><span class="sxs-lookup"><span data-stu-id="796ac-111">Update `<acrName>` with the name of your container registry.</span></span>

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

<span data-ttu-id="796ac-112">Utdata bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="796ac-112">The output should look similar to the following:</span></span>

```console
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

<span data-ttu-id="796ac-113">Om du vill se en lista med alla containeravbildningsrepliker använder du kommandot `az acr replication list`.</span><span class="sxs-lookup"><span data-stu-id="796ac-113">To see a list of all container image replicas, use the `az acr replication list` command.</span></span> <span data-ttu-id="796ac-114">Ersätt `<acrName>` med namnet på ditt containerregister.</span><span class="sxs-lookup"><span data-stu-id="796ac-114">Update `<acrName>` with the name of your container registry.</span></span>

```azurecli
az acr replication list --registry <acrName> --output table
```

<span data-ttu-id="796ac-115">Utdata bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="796ac-115">The output should look similar to the following:</span></span>

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

<span data-ttu-id="796ac-116">Även om du inte använde Azure-portalen i den här övningen är det bra att se hur det ser ut där.</span><span class="sxs-lookup"><span data-stu-id="796ac-116">While the Azure portal was not used in this exercise, it is neat to see the portal experience.</span></span>

<span data-ttu-id="796ac-117">Om du väljer `Replications` för ett Azure-containerregister i Azure-portalen ser du en karta över aktuella replikeringar.</span><span class="sxs-lookup"><span data-stu-id="796ac-117">From within the Azure portal, selecting `Replications` for an Azure container registry displays a map that details current replications.</span></span> <span data-ttu-id="796ac-118">Du kan replikera containeravbildningar till ytterligare regioner genom att välja regionerna på kartan.</span><span class="sxs-lookup"><span data-stu-id="796ac-118">Container images can be replicated to additional regions by selecting the regions on the map.</span></span>

![Karta över containerreplikering i Azure-portalen](../media/replication-map.png)

## <a name="clean-up"></a><span data-ttu-id="796ac-120">Rensa</span><span class="sxs-lookup"><span data-stu-id="796ac-120">Clean up</span></span>

<span data-ttu-id="796ac-121">Det här är den sista utbildningsenheten i modulen Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="796ac-121">This is the last unit of the Azure Container Registry learning module.</span></span> <span data-ttu-id="796ac-122">Nu kan du rensa alla resurser du har skapat genom att ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="796ac-122">At this point, you can clean up the created resources by deleting the resource group.</span></span> <span data-ttu-id="796ac-123">Det gör du med kommandot `az group delete`.</span><span class="sxs-lookup"><span data-stu-id="796ac-123">To do so, use the `az group delete` command.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a><span data-ttu-id="796ac-124">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="796ac-124">Summary</span></span>

<span data-ttu-id="796ac-125">I den här modulen har du replikerat en containeravbildning till flera Azure-regioner med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="796ac-125">In this module, you replicated a container image to multiple Azure regions using the Azure CLI.</span></span>