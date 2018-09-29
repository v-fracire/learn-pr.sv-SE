<span data-ttu-id="bbf38-101">Tänk dig att ditt företag distribuerar flera servrar som en del av övergången till molnet.</span><span class="sxs-lookup"><span data-stu-id="bbf38-101">Suppose your company is deploying several servers as part of their cloud transition.</span></span> <span data-ttu-id="bbf38-102">Virtuella datordiskar måste krypteras under distributionen så att de inte är sårbara under något skede.</span><span class="sxs-lookup"><span data-stu-id="bbf38-102">VM disks must be encrypted during the deployment, so there's no time when the disks are vulnerable.</span></span> <span data-ttu-id="bbf38-103">Du vill automatisera den här processen och måste ändra Azure Resource Management-mallarna så att kryptering aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="bbf38-103">You want to automate this process, and have to modify the Azure Resource Manager templates to automatically enable encryption.</span></span>

<span data-ttu-id="bbf38-104">Här tar vi en titt på hur du använder en Azure Resource Manager-mall för att automatiskt aktivera kryptering för nya virtuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="bbf38-104">Here, we'll look at how to use an Azure Resource Manager template to automatically enable encryption for new Windows VMs.</span></span>

## <a name="what-are-azure-resource-manager-templates"></a><span data-ttu-id="bbf38-105">Vad är Azure Resource Manager-mallar?</span><span class="sxs-lookup"><span data-stu-id="bbf38-105">What are Azure Resource Manager templates?</span></span>

<span data-ttu-id="bbf38-106">Resource Manager-mallar är JSON-filer som används till att definiera en uppsättning resurser som ska distribueras till Azure.</span><span class="sxs-lookup"><span data-stu-id="bbf38-106">Resource Manager templates are JSON files used to define a set of resources to deploy to Azure.</span></span> <span data-ttu-id="bbf38-107">Du kan skriva dem från början, och för vissa Azure-resurser som virtuella datorer kan du generera dem genom att använda Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="bbf38-107">You can write them from scratch, and for some Azure resources, including VMs, you can use the Azure portal to generate them.</span></span> <span data-ttu-id="bbf38-108">Du måste fylla i all nödvändig information för en manuell distribution av virtuella datorer, men i stället för att distribuera den virtuella datorn till Azure sparar du mallen.</span><span class="sxs-lookup"><span data-stu-id="bbf38-108">You'll need to complete the required information for a manual VM deployment, but instead of deploying the VM to Azure, you save the template.</span></span> <span data-ttu-id="bbf38-109">Du kan sedan _återanvända_ mallen för att konfigurera den specifika VM-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="bbf38-109">You can then _reuse_ the template to create that specific VM configuration.</span></span>

<span data-ttu-id="bbf38-110">Det finns [exempelmallar i docs](https://azure.microsoft.com/resources/templates) för att automatisera alla typer av administrativa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="bbf38-110">There are [example templates available in docs](https://azure.microsoft.com/resources/templates) to automate all sorts of administrative tasks.</span></span> <span data-ttu-id="bbf38-111">Vi kunde faktiskt ha använt en av dessa mallar till att kryptera vår virtuella dator som vi precis gjorde manuellt!</span><span class="sxs-lookup"><span data-stu-id="bbf38-111">In fact, we could have used one of these templates to encrypt our VM that we just did manually!</span></span>

![Skärmbild som visar Azure-mallarna](../media/5-browse-templates.png)

## <a name="using-github-templates"></a><span data-ttu-id="bbf38-113">Använda GitHub-mallar</span><span class="sxs-lookup"><span data-stu-id="bbf38-113">Using GitHub templates</span></span>

<span data-ttu-id="bbf38-114">Den faktiska mallkällan lagras i GitHub.</span><span class="sxs-lookup"><span data-stu-id="bbf38-114">The actual template source is stored in GitHub.</span></span> <span data-ttu-id="bbf38-115">Du kan bläddra till en mall i GitHub och distribuera direkt till Azure från sidan.</span><span class="sxs-lookup"><span data-stu-id="bbf38-115">You can browse to a template in GitHub and deploy right to Azure from the page.</span></span>

![Skärmbild som visar en GitHub-mall med knappen för att distribuera till Azure markerad](../media/5-deploy-from-github.png)

<span data-ttu-id="bbf38-117">När mallen distribueras visas en lista med obligatoriska inmatningsfält.</span><span class="sxs-lookup"><span data-stu-id="bbf38-117">When the template is deployed, Azure will display a list of required input fields.</span></span>

![Skärmbild som visar mallen i Azure-portalen](../media/5-fill-in-template.png)

<span data-ttu-id="bbf38-119">Du kan sedan köra mallen för att skapa, ändra eller ta bort resurser.</span><span class="sxs-lookup"><span data-stu-id="bbf38-119">You can then execute the template to create, modify, or remove resources.</span></span>

### <a name="running-templates-in-the-azure-portal"></a><span data-ttu-id="bbf38-120">Köra mallar i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bbf38-120">Running templates in the Azure portal</span></span>

<span data-ttu-id="bbf38-121">Om du redan vet vilken mall du vill använda, eller om du har sparat mallar i ditt Azure-konto, kan du använda resursen **Skapa en resurs** > **Malldistribution** för att hitta och köra definierade mallar i portalen.</span><span class="sxs-lookup"><span data-stu-id="bbf38-121">If you already know the template you want to use, or you have saved templates in your Azure account, you can use the **Create a resource** > **Template Deployment** resource to locate and run defined templates in the portal.</span></span> <span data-ttu-id="bbf38-122">Du kan söka igenom mallar efter namn, redigera en mall för att ändra parametrar eller beteende och köra mallen direkt från det grafiska användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="bbf38-122">You can search through templates by name, edit a template to change the parameters or behavior, and execute the template right from the GUI.</span></span>

### <a name="running-templates-from-the-command-line"></a><span data-ttu-id="bbf38-123">Köra mallar från kommandoraden</span><span class="sxs-lookup"><span data-stu-id="bbf38-123">Running templates from the command line</span></span>

<span data-ttu-id="bbf38-124">Om du får en webbadress till en mall kan du köra den med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bbf38-124">Given a URL to a template, you can execute it with Azure PowerShell.</span></span> <span data-ttu-id="bbf38-125">Vi kan till exempel köra diskkrypteringsmallen med följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="bbf38-125">For example, we could run the disk encryption template with the following PowerShell command:</span></span>

```powershell
New-AzureRmResourceGroupDeployment `
    -Name encrypt-disk `
    -ResourceGroupName <resource-group-name> `
    -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

<span data-ttu-id="bbf38-126">Om du hellre vill använda Azure CLI kan du göra det med kommandot `group deployment create`.</span><span class="sxs-lookup"><span data-stu-id="bbf38-126">Or, if you prefer the Azure CLI, with the `group deployment create` command.</span></span>

```azurecli
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> \ 
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

