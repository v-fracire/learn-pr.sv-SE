<span data-ttu-id="e24a9-101">I Azure CLI kan du skriva kommandon och köra dem direkt.</span><span class="sxs-lookup"><span data-stu-id="e24a9-101">The Azure CLI lets you write commands and execute them immediately.</span></span> <span data-ttu-id="e24a9-102">Kom ihåg att målet i det här utvecklingsexemplet är att distribuera nya versioner av en webbapp så att de kan testas.</span><span class="sxs-lookup"><span data-stu-id="e24a9-102">Recall that the overall goal in the software development example is to deploy new builds of a web app for testing.</span></span> <span data-ttu-id="e24a9-103">Det första steget är att skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e24a9-103">The first step is to create a resource group.</span></span> <span data-ttu-id="e24a9-104">Kom ihåg att målet här är att skapa resurserna via en lokal installation av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e24a9-104">Remember that the goal here is to create these resources using a local installation of the Azure CLI.</span></span> 

<span data-ttu-id="e24a9-105">I den här utbildningsenheten får du lära dig att använda Azure CLI till att logga in på din Azure-prenumeration och skapa en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="e24a9-105">This unit shows you how to use the Azure CLI to sign in to your Azure subscription and create a new resource.</span></span>

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a><span data-ttu-id="e24a9-106">Vilka Azure-resurser kan jag hantera med Azure CLI?</span><span class="sxs-lookup"><span data-stu-id="e24a9-106">What Azure resources can be managed using the Azure CLI?</span></span>
<span data-ttu-id="e24a9-107">Med Azure CLI kan du styra nästan alla aspekter av varje Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="e24a9-107">The Azure CLI lets you control nearly every aspect of every Azure resource.</span></span> <span data-ttu-id="e24a9-108">Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory (Azure AD), containers, maskininlärning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="e24a9-108">You can work with resource groups, storage, virtual machines, Azure Active Directory (Azure AD), containers, machine learning, and so on.</span></span>

<span data-ttu-id="e24a9-109">Kommandona i CLI är ordnade i grupper och undergrupper.</span><span class="sxs-lookup"><span data-stu-id="e24a9-109">Commands in the CLI are structured in groups and subgroups.</span></span> <span data-ttu-id="e24a9-110">Varje grupp representerar en tjänst som tillhandahålls av Azure och undergrupperna delar upp kommandon för de här tjänsterna i logiska grupper.</span><span class="sxs-lookup"><span data-stu-id="e24a9-110">Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings.</span></span> <span data-ttu-id="e24a9-111">Till exempel innehåller gruppen **storage** undergrupper som **account**, **blob**, **storage** och **queue**.</span><span class="sxs-lookup"><span data-stu-id="e24a9-111">For example, the **storage** group contains subgroups including **account**, **blob**, **storage**, and **queue**.</span></span>

<span data-ttu-id="e24a9-112">Hur hittar jag då de kommandon jag behöver?</span><span class="sxs-lookup"><span data-stu-id="e24a9-112">So, how do you find the particular commands you need?</span></span> <span data-ttu-id="e24a9-113">Ett sätt är att använda **az find**.</span><span class="sxs-lookup"><span data-stu-id="e24a9-113">One way is to use **az find**.</span></span> <span data-ttu-id="e24a9-114">Om du till exempel vill hitta kommandon som hjälper dig att hantera en **lagringsblob** kan du använda det här find-kommandot:</span><span class="sxs-lookup"><span data-stu-id="e24a9-114">For example, if you want to find commands that might help you manage a storage **blob**, you'd use the following find command:</span></span>

```bash
az find -q blob
```

<span data-ttu-id="e24a9-115">Om du redan känner till namnet på det kommando du vill använda så kan argumentet **--help** för kommandot vara användbart.</span><span class="sxs-lookup"><span data-stu-id="e24a9-115">If you already know the name of the command you want, the **--help** argument for that command may be more useful.</span></span> <span data-ttu-id="e24a9-116">Du ser då detaljerad information om kommandot, och för en kommandogrupp visas en lista med tillgängliga underkommandon.</span><span class="sxs-lookup"><span data-stu-id="e24a9-116">You get detailed information on the command, and for a command group, a list of the available subcommands.</span></span> <span data-ttu-id="e24a9-117">I vårt lagringsexempel kan du visa en lista med undergrupper och kommandon för hantering av bloblagring så här:</span><span class="sxs-lookup"><span data-stu-id="e24a9-117">So, with our storage example, here's how you can get a list of the subgroups and commands for managing blob storage:</span></span>

```bash
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a><span data-ttu-id="e24a9-118">Skapa en Azure-resurs</span><span class="sxs-lookup"><span data-stu-id="e24a9-118">How to create an Azure resource</span></span>
<span data-ttu-id="e24a9-119">Det ingår normalt tre steg när du ska skapa en ny resurs i Azure: anslut till Azure-prenumerationen, skapa resursen och kontrollera att den skapades.</span><span class="sxs-lookup"><span data-stu-id="e24a9-119">When creating a new Azure resource, there are typically three steps: connect to your Azure subscription, create the resource, and verify that creation was successful (see below).</span></span>

![Steg för att skapa en resurs med Azure CLI](../media-drafts/4-create-resources-overview.png)

<span data-ttu-id="e24a9-121">Varje steg motsvarar ett eget Azure CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="e24a9-121">Each step corresponds to a different Azure CLI command.</span></span>

### <a name="connect"></a><span data-ttu-id="e24a9-122">Ansluta</span><span class="sxs-lookup"><span data-stu-id="e24a9-122">Connect</span></span>
<span data-ttu-id="e24a9-123">Eftersom du arbetar med en lokal installation av Azure CLI måste du autentisera dig innan du kan köra Azure-kommandon. Det gör du med Azure CLI-kommandot **login**.</span><span class="sxs-lookup"><span data-stu-id="e24a9-123">Since you're working with a local install of the Azure CLI, you'll need to authenticate before you can execute Azure commands, by using the Azure CLI **login** command.</span></span> 

```bash
az login
```

<span data-ttu-id="e24a9-124">Azure CLI öppnar normalt din standardwebbläsare och visar inloggningssidan för Azure.</span><span class="sxs-lookup"><span data-stu-id="e24a9-124">The Azure CLI will typically launch your default browser to open the Azure sign-in page.</span></span> <span data-ttu-id="e24a9-125">Om det inte fungerar följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span><span class="sxs-lookup"><span data-stu-id="e24a9-125">If this doesn't work, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

<span data-ttu-id="e24a9-126">När inloggningen är färdig ansluts du till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e24a9-126">After a successful sign in, you'll be connected to your Azure subscription.</span></span> 

### <a name="create"></a><span data-ttu-id="e24a9-127">Skapa</span><span class="sxs-lookup"><span data-stu-id="e24a9-127">Create</span></span>
<span data-ttu-id="e24a9-128">Du behöver ofta skapa en ny resursgrupp innan du skapar en ny Azure-tjänst, så vi använder resursgrupper som exempel för att visa hur du kan skapa Azure-resurser via CLI.</span><span class="sxs-lookup"><span data-stu-id="e24a9-128">You'll often need to create a new resource group before you create a new Azure service, so we'll use resource groups as an example to show how to create Azure resources from the CLI.</span></span>

<span data-ttu-id="e24a9-129">Du skapar en resursgrupp med Azure CLI-kommandot **group create**.</span><span class="sxs-lookup"><span data-stu-id="e24a9-129">The Azure CLI **group create** command creates a resource group.</span></span> <span data-ttu-id="e24a9-130">Du måste ange ett namn och en plats.</span><span class="sxs-lookup"><span data-stu-id="e24a9-130">You must specify a name and location.</span></span> <span data-ttu-id="e24a9-131">Namnet måste vara unikt inom prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="e24a9-131">The name must be unique within your subscription.</span></span> <span data-ttu-id="e24a9-132">Platsen avgör var metadata för resursgruppen lagras.</span><span class="sxs-lookup"><span data-stu-id="e24a9-132">The location determines where the metadata for your resource group will be stored.</span></span> <span data-ttu-id="e24a9-133">Du använder strängar som ”USA, västra”, ”Europa, norra” eller ”västra Indien” för att ange platsen. Du kan också använda konkatenerade ord som usavästra, europanorra eller indienvästra.</span><span class="sxs-lookup"><span data-stu-id="e24a9-133">You use strings like "West US", "North Europe", or "West India" to specify the location; alternatively, you can use single word equivalents, such as westus, northeurope, or westindia.</span></span> <span data-ttu-id="e24a9-134">Den grundläggande syntaxen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="e24a9-134">The core syntax is:</span></span>

```bash
az group create --name <name> --location <location>
```

### <a name="verify"></a><span data-ttu-id="e24a9-135">Verifiera</span><span class="sxs-lookup"><span data-stu-id="e24a9-135">Verify</span></span>
<span data-ttu-id="e24a9-136">För många Azure-resurser har Azure CLI ett **list**-underkommando som visar information om resursen.</span><span class="sxs-lookup"><span data-stu-id="e24a9-136">For many Azure resources, the Azure CLI provides a **list** subcommand to view resource details.</span></span> <span data-ttu-id="e24a9-137">Azure CLI-kommandot **group list** visar till exempel en lista med dina Azure-resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="e24a9-137">For example, the Azure CLI **group list** command lists your Azure resource groups.</span></span> <span data-ttu-id="e24a9-138">Det här är användbart när du ska kontrollera att resursgruppen har skapats:</span><span class="sxs-lookup"><span data-stu-id="e24a9-138">This is useful here to verify whether creation of the resource group was successful:</span></span>

```bash
az group list
```

<span data-ttu-id="e24a9-139">Du kan göra vyn tydligare genom att formatera utdata som en tabell:</span><span class="sxs-lookup"><span data-stu-id="e24a9-139">To get a more concise view, you can format the output as a simple table:</span></span>

```bash
az group list --output table
```
