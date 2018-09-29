<span data-ttu-id="c3e46-101">Som standard är Azure-containerinstanser tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="c3e46-101">By default, Azure Container Instances is stateless.</span></span> <span data-ttu-id="c3e46-102">Om containern kraschar eller stoppas förloras hela tillståndet.</span><span class="sxs-lookup"><span data-stu-id="c3e46-102">If the container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="c3e46-103">Om du vill bevara tillståndet längre än containerns livslängd måste du montera en volym från en extern lagring.</span><span class="sxs-lookup"><span data-stu-id="c3e46-103">To persist state beyond the lifetime of the container, you must mount a volume from an external store.</span></span>

<span data-ttu-id="c3e46-104">Här monterar du en Azure-filresurs till en Azure-containerinstans för lagring och hämtning av data.</span><span class="sxs-lookup"><span data-stu-id="c3e46-104">Here, you will mount an Azure file share to an Azure container instance for data storage and retrieval.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="c3e46-105">Skapa en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="c3e46-105">Create an Azure file share</span></span>

<span data-ttu-id="c3e46-106">Kör följande skript för att skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c3e46-106">Run the following script to create a storage account.</span></span> <span data-ttu-id="c3e46-107">Namnet på lagringskontot måste vara globalt unikt, så skriptet lägger till ett slumpmässigt värde i bassträngen:</span><span class="sxs-lookup"><span data-stu-id="c3e46-107">The storage account name must be globally unique, so the script adds a random value to the base string:</span></span>

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --sku Standard_LRS \
    --location eastus
```

<span data-ttu-id="c3e46-108">Kör följande kommando för att placera lagringskontots anslutningssträng i miljövariabeln *AZURE_STORAGE_CONNECTION_STRING*.</span><span class="sxs-lookup"><span data-stu-id="c3e46-108">Run the following command to place the storage account connection string into an environment variable named *AZURE_STORAGE_CONNECTION_STRING*.</span></span> <span data-ttu-id="c3e46-109">Miljövariabeln kan tolkas i Azure CLI och kan användas i lagringsrelaterade åtgärder:</span><span class="sxs-lookup"><span data-stu-id="c3e46-109">This environment variable is understood by the Azure CLI and can be used in storage-related operations:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv)
```

<span data-ttu-id="c3e46-110">Skapa en filresurs i lagringskontot genom att köra kommandot `az storage share create`.</span><span class="sxs-lookup"><span data-stu-id="c3e46-110">Create a file share in the storage account by running the `az storage share create` command.</span></span> <span data-ttu-id="c3e46-111">I följande exempel skapas en resurs med namnet *aci-share-demo*:</span><span class="sxs-lookup"><span data-stu-id="c3e46-111">The following example creates a share with the name *aci-share-demo*:</span></span>

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a><span data-ttu-id="c3e46-112">Hämta autentiseringsuppgifter för lagring</span><span class="sxs-lookup"><span data-stu-id="c3e46-112">Get storage credentials</span></span>

<span data-ttu-id="c3e46-113">När du ska montera en Azure-filresurs som en volym i Azure Container Instances behöver du tre värden: namnet på lagringskontot, resursnamnet och åtkomstnyckeln för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="c3e46-113">To mount an Azure file share as a volume in Azure Container Instances, you need three values: the storage account name, the share name, and the storage account access key.</span></span>

<span data-ttu-id="c3e46-114">Om du använde skriptet ovan skapades namnet på lagringskontot med ett slumpmässigt värde i slutet.</span><span class="sxs-lookup"><span data-stu-id="c3e46-114">If you used the script above, the storage account name was created with a random value at the end.</span></span> <span data-ttu-id="c3e46-115">Du kan hämta den faktiska strängen (inklusive den slumpmässiga delen) med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c3e46-115">To query the final string (including the random portion), use the following commands:</span></span>

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group <rgn>[sandbox resource group name]</rgn> --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="c3e46-116">Resursnamnet är redan känt (aci-share-demo), så allt som återstår är nyckeln för lagringskontot, som du kan hämta med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c3e46-116">The share name is already known (aci-share-demo), so all that remains is the storage account key, which can be found using the following command:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group <rgn>[sandbox resource group name]</rgn> --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a><span data-ttu-id="c3e46-117">Distribuera containern och montera volymen</span><span class="sxs-lookup"><span data-stu-id="c3e46-117">Deploy container and mount volume</span></span>

<span data-ttu-id="c3e46-118">När du ska montera en Azure-filresurs som en volym i en container anger du resursens och volymens monteringspunkt när du skapar containern:</span><span class="sxs-lookup"><span data-stu-id="c3e46-118">To mount an Azure file share as a volume in a container, specify the share and volume mount point when you create the container:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --location eastus \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

<span data-ttu-id="c3e46-119">När du har skapat containern hämtar du den offentliga IP-adressen:</span><span class="sxs-lookup"><span data-stu-id="c3e46-119">Once the container has been created, get the public IP address:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --query ipAddress.ip \
    --output tsv
```

<span data-ttu-id="c3e46-120">Öppna en webbläsare och navigera till containerns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c3e46-120">Open a browser and navigate to the container's IP address.</span></span> <span data-ttu-id="c3e46-121">Du kommer att se ett enkelt formulär.</span><span class="sxs-lookup"><span data-stu-id="c3e46-121">You will be presented with a simple form.</span></span> <span data-ttu-id="c3e46-122">Ange lite text och klicka på **Submit** (Skicka).</span><span class="sxs-lookup"><span data-stu-id="c3e46-122">Enter some text and click **Submit**.</span></span> <span data-ttu-id="c3e46-123">Den här åtgärden skapar en fil i Azure Files-resursen som innehåller den angivna texten.</span><span class="sxs-lookup"><span data-stu-id="c3e46-123">This action will create a file in the Azure Files share containing the entered text.</span></span>

![Demo av filresurs i Azure Container Instances](../media/5-files-ui.png)

<span data-ttu-id="c3e46-125">Du kan kontrollera processen genom att öppna filresursen i Azure-portalen och ladda ned filen.</span><span class="sxs-lookup"><span data-stu-id="c3e46-125">To validate, you can navigate to the file share in the Azure portal and download the file.</span></span>

1. <span data-ttu-id="c3e46-126">Hämta filnamnet</span><span class="sxs-lookup"><span data-stu-id="c3e46-126">Get the filename</span></span>

    ```azurecli
    az storage file list -s aci-share-demo -o table
    ```

1. <span data-ttu-id="c3e46-127">Ladda ned det, se till att ersätta `<filename>` nedan.</span><span class="sxs-lookup"><span data-stu-id="c3e46-127">Download it, make sure to replace the `<filename>` below.</span></span>

    ```azurecli
    az storage file download -s aci-share-demo -n <filename>
    ```
    
![Demo av exempelfil med innehåll](../media/5-sample-text.png)

<span data-ttu-id="c3e46-129">Om de filer och data som lagrades i Azure Files-resursen var värdefulla skulle resursen återmonteras på en ny containerinstans för att tillhandahålla tillståndskänsliga data.</span><span class="sxs-lookup"><span data-stu-id="c3e46-129">If the files and data stored in the Azure Files share were of any value, this share could be remounted on a new container instance to provide stateful data.</span></span>