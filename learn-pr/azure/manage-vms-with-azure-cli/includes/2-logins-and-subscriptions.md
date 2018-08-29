<span data-ttu-id="6d236-101">Innan vi börjar ska vi ta en titt på syntaxen för verktyget Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6d236-101">Before we start, let's review the syntax for the Azure CLI tool.</span></span> <span data-ttu-id="6d236-102">Om du har gjort modulen **Control Azure services with the Azure CLI** (Kontrollera Azure-tjänster med Azure CLI) vet du att Azure CLI-kommandon antar formen:</span><span class="sxs-lookup"><span data-stu-id="6d236-102">If you've taken the **Control Azure services with the Azure CLI** module, then you know that Azure CLI commands take the form of:</span></span>

```azurecli
az [command] [subcommand] [--parameter --parameter]
```

<span data-ttu-id="6d236-103">`[command]` identifierar det specifika område av Azure du vill kontrollera.</span><span class="sxs-lookup"><span data-stu-id="6d236-103">The `[command]` identifies the specific area of Azure you want to control.</span></span> <span data-ttu-id="6d236-104">Du kan exempelvis hantera prenumerationsinformation med kommandot `account`, eller SQL-databaser med kommandot `sql`.</span><span class="sxs-lookup"><span data-stu-id="6d236-104">For example, you can manage subscription information with the `account` command, or SQL databases with the `sql` command.</span></span> <span data-ttu-id="6d236-105">`[subcommand]` och `[--parameters]` är sedan beroende av de kommandon du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="6d236-105">The `[subcommand]` and `[--parameters]` are then dependent upon the command you're working with.</span></span> 

<span data-ttu-id="6d236-106">Du kan visa en lista över kommandon, underkommandon och parametrar genom att skriva ett partiellt kommando.</span><span class="sxs-lookup"><span data-stu-id="6d236-106">You can view a list of commands, subcommands, and parameters by typing in a partial command.</span></span> <span data-ttu-id="6d236-107">Om du till exempel skriver `az` på kommandoraden ser du den översta hjälpskärmen, och om du skriver `az vm` får du alla underkommandon för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6d236-107">For example, typing `az` at the command line will give you the top-level help screen, and typing `az vm` will give you all the subcommands for virtual machines.</span></span> <span data-ttu-id="6d236-108">Den här metoden kan vara ett bra sätt att utforska Azure CLI-verktyget.</span><span class="sxs-lookup"><span data-stu-id="6d236-108">This approach can be a great way to explore the Azure CLI tool.</span></span>

> [!NOTE]
> <span data-ttu-id="6d236-109">Vi använder webbläsarbaserade Azure Cloud Shell för att arbeta med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6d236-109">We will be using browser-hosted Azure Cloud Shell to work with the Azure CLI.</span></span> <span data-ttu-id="6d236-110">Om du föredrar att arbeta på din lokala dator kan du även köra alla kommandon vi täcker från kommandoraden om du [installerar Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="6d236-110">If you prefer to work from your local machine, all of the commands we cover can also be executed from the command line by [installing the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="6d236-111">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="6d236-111">Log in to Azure</span></span>

<span data-ttu-id="6d236-112">Det första du ska göra när du arbetar med Azure CLI är att logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6d236-112">The first thing you'll do when working with the Azure CLI is to log in to your Azure account.</span></span> <span data-ttu-id="6d236-113">Det gör du med kommandot `login`.</span><span class="sxs-lookup"><span data-stu-id="6d236-113">This is done with the `login` command.</span></span> <span data-ttu-id="6d236-114">Om du använder Cloud Shell finns det en knapp för att logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="6d236-114">If you're using Cloud Shell, there will be a button to sign in to Azure.</span></span>

```azurecli
az login
```

<span data-ttu-id="6d236-115">Kommandot öppnar ett webbläsarfönster och gör att du kan välja det Microsoft-konto du vill använda.</span><span class="sxs-lookup"><span data-stu-id="6d236-115">This command will launch a browser window and allow you to select the Microsoft account you want to use.</span></span>

## <a name="working-with-subscriptions"></a><span data-ttu-id="6d236-116">Arbeta med prenumerationer</span><span class="sxs-lookup"><span data-stu-id="6d236-116">Working with subscriptions</span></span>

<span data-ttu-id="6d236-117">I den här modulen arbetar vi i en tillfällig prenumeration som har skapats som en spelplan, men du använder vanligtvis en prenumeration från ditt eget konto.</span><span class="sxs-lookup"><span data-stu-id="6d236-117">In this module, we will work in a temporary subscription created as a playground, but you will normally use a subscription from your own account.</span></span> <span data-ttu-id="6d236-118">Om du har mer än en prenumeration kan du få en tydligt formaterad lista över prenumerationer med instruktionen `az account list --output table`.</span><span class="sxs-lookup"><span data-stu-id="6d236-118">If you have more than one subscription, you can get a clearly formatted list of subscriptions using the `az account list --output table` statement.</span></span>

```
Name                                  CloudName    SubscriptionId                        State    IsDefault
------------------------------------  -----------  ------------------------------------  -------  -----------
Contoso Legacy Resources              AzureCloud   abc13b0c-d2c4-64b2-9ac5-2f4cb819b752  Enabled  True
Visual Studio Enterprise              AzureCloud   233aebce-23c2-4572-c056-c029449e93ed  Enabled  False
```

<span data-ttu-id="6d236-119">Tänk på att kommandona även identifierar _standardprenumerationen_ där alla dina kommandon gäller.</span><span class="sxs-lookup"><span data-stu-id="6d236-119">Notice that the command also identifies the _default_ subscription where all your commands will apply.</span></span> <span data-ttu-id="6d236-120">Om du föredrar att arbeta i en annan prenumeration kan du använda kommandot `az account set --subscription "[name]"`.</span><span class="sxs-lookup"><span data-stu-id="6d236-120">If you would prefer to work in a different subscription, you can use the `az account set --subscription "[name]"` command.</span></span> <span data-ttu-id="6d236-121">Vi skulle exempelvis kunna ställa in vår aktuella prenumeration på `Visual Studio Enterprise` från listan ovan via följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6d236-121">For example, we could set our current subscription to be `Visual Studio Enterprise` from the above list through the following command:</span></span>

```azurecli
az account set --subscription "Visual Studio Enterprise"
```