<span data-ttu-id="426a1-101">Som standard är Azure-containerinstanser tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="426a1-101">By default, Azure Container Instances are stateless.</span></span> <span data-ttu-id="426a1-102">Om containern kraschar eller stoppas förloras hela tillståndet.</span><span class="sxs-lookup"><span data-stu-id="426a1-102">If the container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="426a1-103">Om du vill bevara tillståndet längre än containern måste du montera en volym från en extern lagring.</span><span class="sxs-lookup"><span data-stu-id="426a1-103">To persist state beyond the lifetime of the container, you must mount a volume from an external store.</span></span>

<span data-ttu-id="426a1-104">I den här utbildningsenheten monterar du en Azure-filresurs vid en Azure-containerinstans för lagring och hämtning av data.</span><span class="sxs-lookup"><span data-stu-id="426a1-104">In this unit, you will mount an Azure file share to an Azure Container Instance for data storage and retrieval.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="426a1-105">Skapa en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="426a1-105">Create an Azure file share</span></span>

<span data-ttu-id="426a1-106">Innan du kan använda en Azure-filresurs med Azure Container Instances måste du skapa den.</span><span class="sxs-lookup"><span data-stu-id="426a1-106">Before using an Azure file share with Azure Container Instances, you must create it.</span></span> <span data-ttu-id="426a1-107">Kör följande skript för att skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="426a1-107">Run the following script to create a storage account.</span></span> <span data-ttu-id="426a1-108">Namnet på lagringskontot måste vara globalt unikt, så skriptet lägger till ett slumpmässigt värde i bassträngen.</span><span class="sxs-lookup"><span data-stu-id="426a1-108">The storage account name must be globally unique, so the script adds a random value to the base string.</span></span>

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --location eastus --sku Standard_LRS
```

<span data-ttu-id="426a1-109">Kör följande kommando för att placera lagringskontots anslutningssträng i miljövariabeln *AZURE_STORAGE_CONNECTION_STRING*.</span><span class="sxs-lookup"><span data-stu-id="426a1-109">Run the following command to place the storage account connection string into an environment variable named *AZURE_STORAGE_CONNECTION_STRING*.</span></span> <span data-ttu-id="426a1-110">Miljövariabeln kan tolkas i Azure CLI och kan användas i lagringsrelaterade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="426a1-110">This environment variable is understood by the Azure CLI and can be used in storage-related operations.</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`
```

<span data-ttu-id="426a1-111">Skapa filresursen med kommandot `az storage share create`.</span><span class="sxs-lookup"><span data-stu-id="426a1-111">Create the files share the `az storage share create` command.</span></span> <span data-ttu-id="426a1-112">I följande exempel skapas en resurs med namnet *aci-share-demo*.</span><span class="sxs-lookup"><span data-stu-id="426a1-112">The following example creates a share with the name *aci-share-demo*.</span></span>

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a><span data-ttu-id="426a1-113">Hämta autentiseringsuppgifter för lagringen</span><span class="sxs-lookup"><span data-stu-id="426a1-113">Get storage credentials</span></span>

<span data-ttu-id="426a1-114">När du ska montera en Azure-filresurs som en volym i Azure Container Instances behöver du tre värden: namnet på lagringskontot, resursnamnet och åtkomstnyckeln för lagringen.</span><span class="sxs-lookup"><span data-stu-id="426a1-114">To mount an Azure file share as a volume in Azure Container Instances, you need three values: the storage account name, the share name, and the storage access key.</span></span>

<span data-ttu-id="426a1-115">Om du använde skriptet ovan skapades namnet på lagringskontonamnet med ett slumpmässigt värde i slutet.</span><span class="sxs-lookup"><span data-stu-id="426a1-115">If you used the script above, the storage account name was created with a random value at the end.</span></span> <span data-ttu-id="426a1-116">Du kan hämta den faktiska strängen (inklusive den slumpmässiga delen) med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="426a1-116">To query the final string (including the random portion), use the following commands:</span></span>

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="426a1-117">Resursnamnet är redan känt (aci-share-demo), så allt som återstår är nyckeln för lagringskontot, som du kan hämta med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="426a1-117">The share name is already known (aci-share-demo), so all that remains is the storage account key, which can be found using the following command:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a><span data-ttu-id="426a1-118">Distribuera containern och montera volymen</span><span class="sxs-lookup"><span data-stu-id="426a1-118">Deploy container and mount volume</span></span>

<span data-ttu-id="426a1-119">När du ska montera en Azure-filresurs som en volym i en container anger du resursens och volymens monteringspunkt när du skapar containern.</span><span class="sxs-lookup"><span data-stu-id="426a1-119">To mount an Azure file share as a volume in a container, specify the share and volume mount point when you create the container.</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

<span data-ttu-id="426a1-120">När du har skapat containern hämtar du den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="426a1-120">Once the container has been created, get the public IP address.</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-files --query ipAddress.ip -o tsv
```

<span data-ttu-id="426a1-121">Öppna en webbläsare och navigera till containerns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="426a1-121">Open a browser and navigate to the containers IP address.</span></span> <span data-ttu-id="426a1-122">Du kommer att se ett enkelt formulär.</span><span class="sxs-lookup"><span data-stu-id="426a1-122">You will be presented with a simple form.</span></span> <span data-ttu-id="426a1-123">Ange lite text och klicka på **Submit** (Skicka).</span><span class="sxs-lookup"><span data-stu-id="426a1-123">Enter some text and click **Submit**.</span></span> <span data-ttu-id="426a1-124">Den här åtgärden skapar en fil i Azure-filresursen med den angivna texten som filinnehåll.</span><span class="sxs-lookup"><span data-stu-id="426a1-124">This action will create a file in the Azure File share with the text entered here as the file body.</span></span>

![Demo av filresurs i Azure Container Instances.](../media-draft/files-ui.png)

<span data-ttu-id="426a1-126">Du kan kontrollera processen genom att öppna filresursen i Azure-portalen och ladda ned filen.</span><span class="sxs-lookup"><span data-stu-id="426a1-126">To validate, you can navigate to the file share in the Azure portal and download the file.</span></span>

![Demo av exempelfil med innehåll.](../media-draft/sample-text.png)

<span data-ttu-id="426a1-128">Om de filer och data som lagras i Azure-filresursen var värdefulla skulle resursen återmonteras vid en ny containerinstans för att tillhandahålla tillståndskänsliga data.</span><span class="sxs-lookup"><span data-stu-id="426a1-128">If the files and data stored in the Azure File share were of any value, this share could be remounted on a new container instance to provide stateful data.</span></span>


## <a name="summary"></a><span data-ttu-id="426a1-129">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="426a1-129">Summary</span></span>

<span data-ttu-id="426a1-130">I den här utbildningsenheten har du skapat en Azure-filresurs, en container och monterat filresursen vid containern.</span><span class="sxs-lookup"><span data-stu-id="426a1-130">In this unit, you have created an Azure File Share, a container, and have mounted the file share to that container.</span></span> <span data-ttu-id="426a1-131">Resursen användes sedan till att lagra appdata.</span><span class="sxs-lookup"><span data-stu-id="426a1-131">This share was then used to store application data.</span></span>

<span data-ttu-id="426a1-132">I nästa utbildningsenhet går du igenom felsökning av några vanliga problem med containerinstanser.</span><span class="sxs-lookup"><span data-stu-id="426a1-132">In the next unit, you will work through some common Container Instances troubleshooting operations.</span></span>

