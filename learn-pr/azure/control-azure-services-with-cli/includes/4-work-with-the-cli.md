<span data-ttu-id="8a066-101">Med Azure CLI kan du skriva kommandon och köra dem direkt från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="8a066-101">The Azure CLI lets you type commands and execute them immediately from the command line.</span></span> <span data-ttu-id="8a066-102">Kom ihåg att målet i det här utvecklingsexemplet är att distribuera nya versioner av en webbapp så att de kan testas.</span><span class="sxs-lookup"><span data-stu-id="8a066-102">Recall that the overall goal in the software development example is to deploy new builds of a web app for testing.</span></span> <span data-ttu-id="8a066-103">Nu ska vi titta på vilka slags uppgifter du kan utföra med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8a066-103">Let's talk about the sorts of tasks you can do with the Azure CLI.</span></span>

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a><span data-ttu-id="8a066-104">Vilka Azure-resurser kan jag hantera med Azure CLI?</span><span class="sxs-lookup"><span data-stu-id="8a066-104">What Azure resources can be managed using the Azure CLI?</span></span>
<span data-ttu-id="8a066-105">Med Azure CLI kan du styra nästan alla aspekter av varje Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="8a066-105">The Azure CLI lets you control nearly every aspect of every Azure resource.</span></span> <span data-ttu-id="8a066-106">Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory (Azure AD), containrar, maskininlärning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="8a066-106">You can work with resource groups, storage, virtual machines, Azure Active Directory (Azure AD), containers, machine learning, and so on.</span></span>

<span data-ttu-id="8a066-107">Kommandona i CLI är strukturerade i _grupper_ och _undergrupper_.</span><span class="sxs-lookup"><span data-stu-id="8a066-107">Commands in the CLI are structured in _groups_ and _subgroups_.</span></span> <span data-ttu-id="8a066-108">Varje grupp representerar en tjänst som tillhandahålls av Azure och undergrupperna delar in kommandona för de här tjänsterna i logiska grupper.</span><span class="sxs-lookup"><span data-stu-id="8a066-108">Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings.</span></span> <span data-ttu-id="8a066-109">Till exempel innehåller gruppen `storage` undergrupper som **account**, **blob**, **storage** och **queue**.</span><span class="sxs-lookup"><span data-stu-id="8a066-109">For example, the `storage` group contains subgroups including **account**, **blob**, **storage**, and **queue**.</span></span>

<span data-ttu-id="8a066-110">Hur hittar jag de kommandon jag behöver?</span><span class="sxs-lookup"><span data-stu-id="8a066-110">So, how do you find the particular commands you need?</span></span> <span data-ttu-id="8a066-111">Ett sätt är att använda `az find`.</span><span class="sxs-lookup"><span data-stu-id="8a066-111">One way is to use `az find`.</span></span> <span data-ttu-id="8a066-112">Om du till exempel vill hitta kommandon som hjälper dig att hantera en lagringsblob kan du använda det här find-kommandot:</span><span class="sxs-lookup"><span data-stu-id="8a066-112">For example, if you want to find commands that might help you manage a storage blob, you can use the following find command:</span></span>

```bash
az find -q blob
```

<span data-ttu-id="8a066-113">Om du redan vet namnet på det kommando som du vill köra kan du använda argumentet `--help` för att visa mer detaljerad information om kommandot, eller visa en lista över de tillgängliga underkommandona för en kommandogrupp.</span><span class="sxs-lookup"><span data-stu-id="8a066-113">If you already know the name of the command you want, the `--help` argument for that command will get you more detailed information on the command, and for a command group, a list of the available subcommands.</span></span> <span data-ttu-id="8a066-114">I vårt lagringsexempel kan du visa en lista med undergrupper och kommandon för hantering av bloblagring så här:</span><span class="sxs-lookup"><span data-stu-id="8a066-114">So, with our storage example, here's how you can get a list of the subgroups and commands for managing blob storage:</span></span>

```bash
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a><span data-ttu-id="8a066-115">Skapa en Azure-resurs</span><span class="sxs-lookup"><span data-stu-id="8a066-115">How to create an Azure resource</span></span>
<span data-ttu-id="8a066-116">Du utför vanligtvis tre steg när du skapar en ny resurs i Azure: anslut till Azure-prenumerationen, skapa resursen och kontrollera att den har skapats.</span><span class="sxs-lookup"><span data-stu-id="8a066-116">When creating a new Azure resource, there are typically three steps: connect to your Azure subscription, create the resource, and verify that creation was successful (see below).</span></span>

![Steg för att skapa en resurs med Azure CLI](../media-drafts/4-create-resources-overview.png)

<span data-ttu-id="8a066-118">Varje steg motsvarar ett separat Azure CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="8a066-118">Each step corresponds to a different Azure CLI command.</span></span>

### <a name="connect"></a><span data-ttu-id="8a066-119">Ansluta</span><span class="sxs-lookup"><span data-stu-id="8a066-119">Connect</span></span>
<span data-ttu-id="8a066-120">Eftersom du arbetar med en lokal installation av Azure CLI måste du autentisera dig innan du kan köra Azure-kommandon. Det gör du med Azure CLI-kommandot **login**.</span><span class="sxs-lookup"><span data-stu-id="8a066-120">Since you're working with a local install of the Azure CLI, you'll need to authenticate before you can execute Azure commands, by using the Azure CLI **login** command.</span></span> 

```bash
az login
```

<span data-ttu-id="8a066-121">Azure CLI öppnar normalt din standardwebbläsare och visar inloggningssidan för Azure.</span><span class="sxs-lookup"><span data-stu-id="8a066-121">The Azure CLI will typically launch your default browser to open the Azure sign-in page.</span></span> <span data-ttu-id="8a066-122">Om det inte fungerar följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span><span class="sxs-lookup"><span data-stu-id="8a066-122">If this doesn't work, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

<span data-ttu-id="8a066-123">När inloggningen är färdig ansluts du till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8a066-123">After a successful sign in, you'll be connected to your Azure subscription.</span></span> 

### <a name="create"></a><span data-ttu-id="8a066-124">Skapa</span><span class="sxs-lookup"><span data-stu-id="8a066-124">Create</span></span>
<span data-ttu-id="8a066-125">Du behöver ofta skapa en ny resursgrupp innan du skapar en ny Azure-tjänst, så vi använder resursgrupper som exempel för att visa hur du kan skapa Azure-resurser via CLI.</span><span class="sxs-lookup"><span data-stu-id="8a066-125">You'll often need to create a new resource group before you create a new Azure service, so we'll use resource groups as an example to show how to create Azure resources from the CLI.</span></span>

<span data-ttu-id="8a066-126">Du skapar en resursgrupp med Azure CLI-kommandot **group create**.</span><span class="sxs-lookup"><span data-stu-id="8a066-126">The Azure CLI **group create** command creates a resource group.</span></span> <span data-ttu-id="8a066-127">Du måste ange ett namn och en plats.</span><span class="sxs-lookup"><span data-stu-id="8a066-127">You must specify a name and location.</span></span> <span data-ttu-id="8a066-128">Namnet måste vara unikt inom prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="8a066-128">The name must be unique within your subscription.</span></span> <span data-ttu-id="8a066-129">Platsen avgör var metadata för resursgruppen lagras.</span><span class="sxs-lookup"><span data-stu-id="8a066-129">The location determines where the metadata for your resource group will be stored.</span></span> <span data-ttu-id="8a066-130">Du använder strängar som ”USA, västra”, ”Europa, norra” eller ”Indien, västra” för att ange platsen. Du kan också använda konkatenerade ord som usavästra, europanorra eller indienvästra.</span><span class="sxs-lookup"><span data-stu-id="8a066-130">You use strings like "West US", "North Europe", or "West India" to specify the location; alternatively, you can use single word equivalents, such as westus, northeurope, or westindia.</span></span> <span data-ttu-id="8a066-131">Den grundläggande syntaxen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="8a066-131">The core syntax is:</span></span>

```bash
az group create --name <name> --location <location>
```

### <a name="verify"></a><span data-ttu-id="8a066-132">Verifiera</span><span class="sxs-lookup"><span data-stu-id="8a066-132">Verify</span></span>
<span data-ttu-id="8a066-133">För många Azure-resurser har Azure CLI ett **list**-underkommando som visar information om resursen.</span><span class="sxs-lookup"><span data-stu-id="8a066-133">For many Azure resources, the Azure CLI provides a **list** subcommand to view resource details.</span></span> <span data-ttu-id="8a066-134">Azure CLI-kommandot **group list** visar till exempel en lista med dina Azure-resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="8a066-134">For example, the Azure CLI **group list** command lists your Azure resource groups.</span></span> <span data-ttu-id="8a066-135">Det här är användbart när du ska kontrollera att resursgruppen har skapats:</span><span class="sxs-lookup"><span data-stu-id="8a066-135">This is useful here to verify whether creation of the resource group was successful:</span></span>

```bash
az group list
```

<span data-ttu-id="8a066-136">Du kan göra vyn tydligare genom att formatera utdata som en tabell:</span><span class="sxs-lookup"><span data-stu-id="8a066-136">To get a more concise view, you can format the output as a simple table:</span></span>

```bash
az group list --output table
```
