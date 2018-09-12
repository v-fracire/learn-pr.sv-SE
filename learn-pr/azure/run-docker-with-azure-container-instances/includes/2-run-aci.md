<span data-ttu-id="e76cf-101">Azure Container Instances gör det enkelt att skapa och hantera Docker-containers i Azure, utan att behöva etablera virtuella datorer eller gå upp till en högre tjänstnivå.</span><span class="sxs-lookup"><span data-stu-id="e76cf-101">Azure Container Instances makes it easy to create and manage Docker containers in Azure, without having to provision virtual machines or adopt a higher-level service.</span></span> <span data-ttu-id="e76cf-102">I den här utbildningsenheten skapar du en container i Azure och gör den tillgänglig på internet via ett fullständigt kvalificerat domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="e76cf-102">In this unit, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="e76cf-103">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e76cf-103">Create a resource group</span></span>

<span data-ttu-id="e76cf-104">Azure Container Instances måste precis som alla Azure-resurser placeras i en resursgrupp, som är en logisk samling där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="e76cf-104">Azure Container Instances, like all Azure resources, must be placed in a resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="e76cf-105">Skapa en resursgrupp med kommandot `az group create`.</span><span class="sxs-lookup"><span data-stu-id="e76cf-105">Create a resource group with the `az group create` command.</span></span>

<span data-ttu-id="e76cf-106">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *eastus*:</span><span class="sxs-lookup"><span data-stu-id="e76cf-106">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="e76cf-107">Skapa en container</span><span class="sxs-lookup"><span data-stu-id="e76cf-107">Create a container</span></span>

<span data-ttu-id="e76cf-108">Du kan skapa en container genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp i kommandot **az container create**.</span><span class="sxs-lookup"><span data-stu-id="e76cf-108">You can create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command.</span></span> <span data-ttu-id="e76cf-109">Du kan också göra containern tillgänglig på Internet genom att ange en DNS-namnetikett.</span><span class="sxs-lookup"><span data-stu-id="e76cf-109">You can optionally expose the container to the internet by specifying a DNS name label.</span></span> <span data-ttu-id="e76cf-110">I det här exemplet distribuerar du en container som är värd för en liten webbapp.</span><span class="sxs-lookup"><span data-stu-id="e76cf-110">In this example, you deploy a container that hosts a small web app.</span></span>

<span data-ttu-id="e76cf-111">Kör följande kommando för att starta en containerinstans.</span><span class="sxs-lookup"><span data-stu-id="e76cf-111">Execute the following command to start a container instance.</span></span> <span data-ttu-id="e76cf-112">Värdet *--dns-name-label* måste vara unikt i den Azure-region där du skapar instansen, så du kan behöva ändra värdet för att det ska vara unikt:</span><span class="sxs-lookup"><span data-stu-id="e76cf-112">The *--dns-name-label* value must be unique within the Azure region you create the instance, so you might need to modify this value to ensure uniqueness:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

<span data-ttu-id="e76cf-113">Om några sekunder bör du få ett svar på din begäran.</span><span class="sxs-lookup"><span data-stu-id="e76cf-113">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="e76cf-114">Först har containern statusen **Creating** (skapas) men den bör starta inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="e76cf-114">Initially, the container is in the **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="e76cf-115">Du kan kontrollera statusen med kommandot `az container show`:</span><span class="sxs-lookup"><span data-stu-id="e76cf-115">You can check the status using the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

<span data-ttu-id="e76cf-116">När du kör kommandot visas containerns fullständiga domännamn (FQDN) och dess etableringsstatus:</span><span class="sxs-lookup"><span data-stu-id="e76cf-116">When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:</span></span>

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

<span data-ttu-id="e76cf-117">När containern övergår till statusen **Lyckades** går du till dess FQDN i din webbläsare:</span><span class="sxs-lookup"><span data-stu-id="e76cf-117">Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser:</span></span>

![Skärmbild av webbläsare med en app som körs i en Azure-containerinstans](../media-draft/aci-app-browser.png)

## <a name="summary"></a><span data-ttu-id="e76cf-119">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e76cf-119">Summary</span></span>

<span data-ttu-id="e76cf-120">I den här utbildningsenheten har du skapat en Azure-containerinstans som kör en webbserver och en app.</span><span class="sxs-lookup"><span data-stu-id="e76cf-120">In this unit, you created an Azure container instance that is running a web server and application.</span></span> <span data-ttu-id="e76cf-121">Du använde också appen via FQDN-värdet för containerinstansen.</span><span class="sxs-lookup"><span data-stu-id="e76cf-121">You also accessed this application using the FQDN of the container instance.</span></span>

<span data-ttu-id="e76cf-122">I nästa utbildningsenhet konfigurerar du containerinstansens omstartsprincip.</span><span class="sxs-lookup"><span data-stu-id="e76cf-122">In the next unit, you will configure container instance restart policies.</span></span>