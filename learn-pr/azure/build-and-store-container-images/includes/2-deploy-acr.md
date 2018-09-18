<span data-ttu-id="b12a3-101">I den här utbildningsenheten skapar du ett Azure-containerregister med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b12a3-101">In this unit, you will create an Azure container registry using the Azure CLI.</span></span>

## <a name="create-an-azure-container-registry"></a><span data-ttu-id="b12a3-102">Skapa ett Azure-containerregister</span><span class="sxs-lookup"><span data-stu-id="b12a3-102">Create an Azure container registry</span></span>

<span data-ttu-id="b12a3-103">Innan du skapar Azure-containerregistret måste du ha en *resursgrupp* att distribuera den till.</span><span class="sxs-lookup"><span data-stu-id="b12a3-103">Before you create your Azure container registry, you need a *resource group* to deploy it to.</span></span> <span data-ttu-id="b12a3-104">En resursgrupp är en logisk samling där alla Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="b12a3-104">A resource group is a logical collection into which all Azure resources are deployed and managed.</span></span>

<span data-ttu-id="b12a3-105">Skapa en resursgrupp med kommandot `az group create`.</span><span class="sxs-lookup"><span data-stu-id="b12a3-105">Create a resource group with the `az group create` command.</span></span> <span data-ttu-id="b12a3-106">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i regionen *eastus*:</span><span class="sxs-lookup"><span data-stu-id="b12a3-106">In the following example, a resource group named *myResourceGroup* is created in the *eastus* region:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="b12a3-107">När du har skapat resursgruppen skapar du ett Azure-containerregister med kommandot `az acr create`.</span><span class="sxs-lookup"><span data-stu-id="b12a3-107">Once you've created the resource group, create an Azure container registry with the `az acr create` command.</span></span> <span data-ttu-id="b12a3-108">Namnet på containerregistret måste vara unikt i Azure och innehålla mellan 5 och 50 alfanumeriska tecken.</span><span class="sxs-lookup"><span data-stu-id="b12a3-108">The container registry name must be unique within Azure, and contain between 5 and 50 alphanumeric characters.</span></span> <span data-ttu-id="b12a3-109">Ersätt `<acrName>` med ett unikt namn för ditt register.</span><span class="sxs-lookup"><span data-stu-id="b12a3-109">Replace `<acrName>` with a unique name for your registry.</span></span>

<span data-ttu-id="b12a3-110">I det här exemplet distribueras en premium-SKU för registret.</span><span class="sxs-lookup"><span data-stu-id="b12a3-110">For this example, a premium registry SKU is deployed.</span></span> <span data-ttu-id="b12a3-111">Premium-SKU:n krävs för georeplikering.</span><span class="sxs-lookup"><span data-stu-id="b12a3-111">The premium SKU is required for geo-replication.</span></span> <span data-ttu-id="b12a3-112">Mer information om SKU:er för Container Registry finns i [SKU:er i Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-skus)</span><span class="sxs-lookup"><span data-stu-id="b12a3-112">For more information on Container Registry SKUs, see, [Azure Container Registry SKUs](https://docs.microsoft.com/azure/container-registry/container-registry-skus)</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Premium
```

<span data-ttu-id="b12a3-113">Här är exempel på utdata för ett nytt Azure-containerregister:</span><span class="sxs-lookup"><span data-stu-id="b12a3-113">Here's example output for a new Azure container registry:</span></span>

```console
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-15T19:19:07.042178+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myACR0007",
  "location": "eastus",
  "loginServer": "myacr.azurecr.io",
  "name": "myACR",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

<span data-ttu-id="b12a3-114">I resten av modulen använder vi `<acrName>` som platshållare för det containerregisternamn du väljer i det här steget.</span><span class="sxs-lookup"><span data-stu-id="b12a3-114">The rest of this module refers to `<acrName>` as a placeholder for the container registry name that you chose in this step.</span></span>

## <a name="summary"></a><span data-ttu-id="b12a3-115">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b12a3-115">Summary</span></span>

<span data-ttu-id="b12a3-116">I den här enheten skapade du ett Azure-containerregister med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b12a3-116">In this unit, you created an Azure container registry using the Azure CLI.</span></span>