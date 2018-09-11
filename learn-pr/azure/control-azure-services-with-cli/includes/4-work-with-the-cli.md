<span data-ttu-id="ec1c1-101">Med Azure CLI kan du skriva kommandon och köra dem direkt från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-101">The Azure CLI lets you type commands and execute them immediately from the command line.</span></span> <span data-ttu-id="ec1c1-102">Kom ihåg att målet i det här utvecklingsexemplet är att distribuera nya versioner av en webbapp så att de kan testas.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-102">Recall that the overall goal in the software development example is to deploy new builds of a web app for testing.</span></span> <span data-ttu-id="ec1c1-103">Nu ska vi titta på vilka slags uppgifter du kan utföra med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-103">Let's talk about the sorts of tasks you can do with the Azure CLI.</span></span>

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a><span data-ttu-id="ec1c1-104">Vilka Azure-resurser kan jag hantera med Azure CLI?</span><span class="sxs-lookup"><span data-stu-id="ec1c1-104">What Azure resources can be managed using the Azure CLI?</span></span>

<span data-ttu-id="ec1c1-105">Med Azure CLI kan du styra nästan alla aspekter av varje Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-105">The Azure CLI lets you control nearly every aspect of every Azure resource.</span></span> <span data-ttu-id="ec1c1-106">Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory (Azure AD), containrar, maskininlärning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-106">You can work with resource groups, storage, virtual machines, Azure Active Directory (Azure AD), containers, machine learning, and so on.</span></span>

<span data-ttu-id="ec1c1-107">Kommandona i CLI är strukturerade i _grupper_ och _undergrupper_.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-107">Commands in the CLI are structured in _groups_ and _subgroups_.</span></span> <span data-ttu-id="ec1c1-108">Varje grupp representerar en tjänst som tillhandahålls av Azure och undergrupperna delar in kommandona för de här tjänsterna i logiska grupper.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-108">Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings.</span></span> <span data-ttu-id="ec1c1-109">Till exempel innehåller gruppen `storage` undergrupper som **account**, **blob**, **storage** och **queue**.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-109">For example, the `storage` group contains subgroups including **account**, **blob**, **storage**, and **queue**.</span></span>

<span data-ttu-id="ec1c1-110">Hur hittar jag de kommandon jag behöver?</span><span class="sxs-lookup"><span data-stu-id="ec1c1-110">So, how do you find the particular commands you need?</span></span> <span data-ttu-id="ec1c1-111">Ett sätt är att använda `az find`.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-111">One way is to use `az find`.</span></span> <span data-ttu-id="ec1c1-112">Om du till exempel vill hitta kommandon som hjälper dig att hantera en lagringsblob kan du använda det här find-kommandot:</span><span class="sxs-lookup"><span data-stu-id="ec1c1-112">For example, if you want to find commands that might help you manage a storage blob, you can use the following find command:</span></span>

```bash
az find -q blob
```

<span data-ttu-id="ec1c1-113">Om du redan vet namnet på det kommando som du vill köra kan du använda argumentet `--help` för att visa mer detaljerad information om kommandot, eller visa en lista över de tillgängliga underkommandona för en kommandogrupp.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-113">If you already know the name of the command you want, the `--help` argument for that command will get you more detailed information on the command, and for a command group, a list of the available subcommands.</span></span> <span data-ttu-id="ec1c1-114">I vårt lagringsexempel kan du visa en lista med undergrupper och kommandon för hantering av bloblagring så här:</span><span class="sxs-lookup"><span data-stu-id="ec1c1-114">So, with our storage example, here's how you can get a list of the subgroups and commands for managing blob storage:</span></span>

```bash
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a><span data-ttu-id="ec1c1-115">Skapa en Azure-resurs</span><span class="sxs-lookup"><span data-stu-id="ec1c1-115">How to create an Azure resource</span></span>

<span data-ttu-id="ec1c1-116">När du ska skapa en ny resurs i Azure gör du det vanligen i tre steg: ansluter till Azure-prenumerationen, skapar resursen och kontrollerar att den skapades.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-116">When creating a new Azure resource, there are typically three steps: connect to your Azure subscription, create the resource, and verify that creation was successful.</span></span> <span data-ttu-id="ec1c1-117">Följande illustration visar en översikt över den här processen.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-117">The following illustration shows a high-level overview of the process.</span></span>

![En bild som visar hur du skapar en Azure-resurs med hjälp av kommandoradsgränssnittet.](../media-drafts/4-create-resources-overview.png)

<span data-ttu-id="ec1c1-119">Varje steg motsvarar ett eget Azure CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-119">Each step corresponds to a different Azure CLI command.</span></span>

### <a name="connect"></a><span data-ttu-id="ec1c1-120">Ansluta</span><span class="sxs-lookup"><span data-stu-id="ec1c1-120">Connect</span></span>

<span data-ttu-id="ec1c1-121">Eftersom du arbetar med en lokal installation av Azure CLI måste du autentisera dig innan du kan köra Azure-kommandon. Det gör du med Azure CLI-kommandot **login**.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-121">Since you're working with a local install of the Azure CLI, you'll need to authenticate before you can execute Azure commands, by using the Azure CLI **login** command.</span></span> 

```bash
az login
```

<span data-ttu-id="ec1c1-122">Azure CLI öppnar normalt din standardwebbläsare och visar inloggningssidan för Azure.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-122">The Azure CLI will typically launch your default browser to open the Azure sign-in page.</span></span> <span data-ttu-id="ec1c1-123">Om det inte fungerar följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span><span class="sxs-lookup"><span data-stu-id="ec1c1-123">If this doesn't work, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

<span data-ttu-id="ec1c1-124">När inloggningen är färdig ansluts du till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-124">After a successful sign in, you'll be connected to your Azure subscription.</span></span> 

### <a name="create"></a><span data-ttu-id="ec1c1-125">Skapa</span><span class="sxs-lookup"><span data-stu-id="ec1c1-125">Create</span></span>

<span data-ttu-id="ec1c1-126">Du behöver ofta skapa en ny resursgrupp innan du skapar en ny Azure-tjänst, så vi använder resursgrupper som exempel för att visa hur du kan skapa Azure-resurser via CLI.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-126">You'll often need to create a new resource group before you create a new Azure service, so we'll use resource groups as an example to show how to create Azure resources from the CLI.</span></span>

<span data-ttu-id="ec1c1-127">Du skapar en resursgrupp med Azure CLI-kommandot **group create**.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-127">The Azure CLI **group create** command creates a resource group.</span></span> <span data-ttu-id="ec1c1-128">Du måste ange ett namn och en plats.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-128">You must specify a name and location.</span></span> <span data-ttu-id="ec1c1-129">Namnet måste vara unikt inom prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-129">The name must be unique within your subscription.</span></span> <span data-ttu-id="ec1c1-130">Platsen avgör var metadata för resursgruppen lagras.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-130">The location determines where the metadata for your resource group will be stored.</span></span> <span data-ttu-id="ec1c1-131">Du använder strängar som ”USA, västra”, ”Europa, norra” eller ”västra Indien” för att ange platsen. Du kan också använda konkatenerade ord som usavästra, europanorra eller indienvästra.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-131">You use strings like "West US", "North Europe", or "West India" to specify the location; alternatively, you can use single word equivalents, such as westus, northeurope, or westindia.</span></span> <span data-ttu-id="ec1c1-132">Den grundläggande syntaxen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="ec1c1-132">The core syntax is:</span></span>

```bash
az group create --name <name> --location <location>
```

### <a name="verify"></a><span data-ttu-id="ec1c1-133">Verifiera</span><span class="sxs-lookup"><span data-stu-id="ec1c1-133">Verify</span></span>

<span data-ttu-id="ec1c1-134">För många Azure-resurser har Azure CLI ett **list**-underkommando som visar information om resursen.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-134">For many Azure resources, the Azure CLI provides a **list** subcommand to view resource details.</span></span> <span data-ttu-id="ec1c1-135">Azure CLI-kommandot **group list** visar till exempel en lista med dina Azure-resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="ec1c1-135">For example, the Azure CLI **group list** command lists your Azure resource groups.</span></span> <span data-ttu-id="ec1c1-136">Det här är användbart när du ska kontrollera att resursgruppen har skapats:</span><span class="sxs-lookup"><span data-stu-id="ec1c1-136">This is useful here to verify whether creation of the resource group was successful:</span></span>

```bash
az group list
```

<span data-ttu-id="ec1c1-137">Du kan göra vyn tydligare genom att formatera utdata som en tabell:</span><span class="sxs-lookup"><span data-stu-id="ec1c1-137">To get a more concise view, you can format the output as a simple table:</span></span>

```bash
az group list --output table
```
