<span data-ttu-id="8895d-101">I den här utbildningsenheten skapar du ett Azure-containerregister med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8895d-101">In this unit, you will create an Azure container registry using the Azure CLI.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]
 
## <a name="create-an-azure-container-registry"></a><span data-ttu-id="8895d-102">Skapa ett Azure-containerregister</span><span class="sxs-lookup"><span data-stu-id="8895d-102">Create an Azure container registry</span></span>

<span data-ttu-id="8895d-103">Vi kommer att arbeta i en kostnadsfri sandbox-miljö, så du behöver inte skapa en egen resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8895d-103">We'll be working in a free sandbox, so there's no need to create your own Resource Group.</span></span> <span data-ttu-id="8895d-104">Skapa ett Azure-containerregister med kommandot `az acr create`.</span><span class="sxs-lookup"><span data-stu-id="8895d-104">Create an Azure container registry with the `az acr create` command.</span></span> <span data-ttu-id="8895d-105">Containerregistrets namn måste vara unikt i Azure och innehålla mellan 5 och 50 alfanumeriska tecken.</span><span class="sxs-lookup"><span data-stu-id="8895d-105">The container registry name must be unique within Azure and contain between 5 and 50 alphanumeric characters.</span></span> <span data-ttu-id="8895d-106">Ersätt `<acrName>` med ett unikt namn för ditt register.</span><span class="sxs-lookup"><span data-stu-id="8895d-106">Replace `<acrName>` with a unique name for your registry.</span></span>

<span data-ttu-id="8895d-107">I det här exemplet distribueras en premiumregister-SKU.</span><span class="sxs-lookup"><span data-stu-id="8895d-107">In this example, a premium registry SKU is deployed.</span></span> <span data-ttu-id="8895d-108">Premium-SKU:n krävs för georeplikering.</span><span class="sxs-lookup"><span data-stu-id="8895d-108">The premium SKU is required for geo-replication.</span></span> <span data-ttu-id="8895d-109">Ange följande kommando i Cloud Shell-redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8895d-109">Enter the following command into the cloud shell editor.</span></span>

```azurecli
az acr create --resource-group <rgn>[sandbox resource group name]</rgn> --name <acrName> --sku Premium
```

<span data-ttu-id="8895d-110">Här följer exempel på utdata för ett nytt Azure-containerregister:</span><span class="sxs-lookup"><span data-stu-id="8895d-110">Here's an example output for a new Azure container registry:</span></span>

```output
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

<span data-ttu-id="8895d-111">I resten av modulen använder vi `<acrName>` som platshållare för det containerregisternamn du väljer i det här steget.</span><span class="sxs-lookup"><span data-stu-id="8895d-111">The rest of this module refers to `<acrName>` as a placeholder for the container registry name that you chose in this step.</span></span>

<span data-ttu-id="8895d-112">I den här enheten skapade du ett Azure-containerregister med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8895d-112">In this unit, you created an Azure container registry using the Azure CLI.</span></span>