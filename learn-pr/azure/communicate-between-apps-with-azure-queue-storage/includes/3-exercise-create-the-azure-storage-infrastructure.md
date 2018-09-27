<span data-ttu-id="9ffdb-101">Du har upptäckt att trafiktoppar kan överbelasta mellannivån.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-101">You've discovered that spikes in traffic can overwhelm our middle-tier.</span></span> <span data-ttu-id="9ffdb-102">För att hantera detta har du bestämt dig för att lägga till en kö mellan klientdelen och mellannivån i ditt program för artikeluppladdning.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-102">To deal with this, you've decided to add a queue between the front-end and the middle tier in your article-upload application.</span></span>

<span data-ttu-id="9ffdb-103">Det första steget när du skapar en kö är att skapa ett Azure Storage-konto som ska lagra våra data.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-103">The first step in creating a queue is to create the Azure Storage Account that will store our data.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-storage-account-with-the-azure-cli"></a><span data-ttu-id="9ffdb-104">Skapa ett lagringskonto med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9ffdb-104">Create a Storage Account with the Azure CLI</span></span>

> [!TIP] 
> <span data-ttu-id="9ffdb-105">Vanligtvis startar du ett nytt projekt genom att skapa en _resursgrupp_ som ska innehålla alla associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-105">Normally, you'd start a new project by creating a _resource group_ to hold all the associated resources.</span></span> <span data-ttu-id="9ffdb-106">I det här fallet ska vi använda Azure Sandbox som tillhandahåller en resursgrupp med namnet <rgn>[resursgruppnamn för sandbox]</rgn>.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-106">In this case, we'll be using the Azure sandbox which provides a resource group named <rgn>[sandbox resource group name]</rgn>.</span></span>

<span data-ttu-id="9ffdb-107">Kör kommandot `az storage account create` för att skapa lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-107">Use the `az storage account create` command to create the storage account.</span></span> <span data-ttu-id="9ffdb-108">Du kan skriva kommandot i Cloud Shell-fönstret till höger.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-108">You can type the command into the Cloud Shell window on the right.</span></span>

<span data-ttu-id="9ffdb-109">Kommandot kräver flera parametrar:</span><span class="sxs-lookup"><span data-stu-id="9ffdb-109">The command needs several parameters:</span></span>

| <span data-ttu-id="9ffdb-110">Parameter</span><span class="sxs-lookup"><span data-stu-id="9ffdb-110">Parameter</span></span> | <span data-ttu-id="9ffdb-111">Värde</span><span class="sxs-lookup"><span data-stu-id="9ffdb-111">Value</span></span> |
|-----------|-------|
| `--name`  | <span data-ttu-id="9ffdb-112">Ställer in namnet.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-112">Sets the name.</span></span> <span data-ttu-id="9ffdb-113">Kom ihåg att lagringskonton använder namnet till att generera en offentlig URL, så det måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-113">Remember that storage accounts use the name to generate a public URL - so it has to be unique.</span></span> <span data-ttu-id="9ffdb-114">Namnet på lagringskontot måste dessutom vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-114">In addition, the account name must be between 3 and 24 characters, and be composed of numbers and lowercase letters only.</span></span> <span data-ttu-id="9ffdb-115">Vi rekommenderar att du använder prefixet **articles** med ett slumptal som suffix, men du kan använda vad du vill.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-115">We recommend you use the prefix **articles** with a random number suffix but you can use whatever you like.</span></span> |
| `-g`        | <span data-ttu-id="9ffdb-116">Tillhandahåller **resursgruppen** och använder _<rgn>[resursgruppnamn för sandbox]</rgn>_ som värde.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-116">Supplies the **Resource Group**, use _<rgn>[sandbox resource group name]</rgn>_ as the value.</span></span> |
| `--kind`    | <span data-ttu-id="9ffdb-117">Anger **typ av lagringskonto** – använd _StorageV2_ för att skapa ett allmänt V2-konto.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-117">Sets the **Storage Account type** - use _StorageV2_ to create a general-purpose V2 account.</span></span> |
| `--sku`     | <span data-ttu-id="9ffdb-118">Ställer in **replikerings- och lagringstyp**, vilket har standardinställningen _Standard_RAGRS_.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-118">Sets the **Replication and Storage type**, it defaults to _Standard_RAGRS_.</span></span> <span data-ttu-id="9ffdb-119">Vi använder _Standard_LRS_, vilket innebär att kontot bara är lokalt redundant i datacentret.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-119">Let's use _Standard_LRS_ which means it's only locally redundant within the data center.</span></span> |
| `-l`        | <span data-ttu-id="9ffdb-120">Anger **platsen** oberoende av resursgruppens ägare.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-120">Sets the **Location** independent of the resource group owner.</span></span> <span data-ttu-id="9ffdb-121">Det här är valfritt, men du kan använda det om du vill placera kön i en annan region än resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-121">It's optional, but you can use it to place the queue in a different region than the resource group.</span></span> <span data-ttu-id="9ffdb-122">Välj en plats nära dig från nedanstående lista över tillgängliga regioner i sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-122">Place it close to you, choosing from the below list of available regions in the sandbox.</span></span> |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="9ffdb-123">Här är ett kommandoradsexempel med ovanstående parametrar.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-123">Here's an example command line that uses the above parameters.</span></span> <span data-ttu-id="9ffdb-124">Se till att ändra parametern `--name`.</span><span class="sxs-lookup"><span data-stu-id="9ffdb-124">Make sure to change the `--name` parameter.</span></span>

```azurecli
az storage account create --name [unique-name] -g <rgn>[sandbox resource group name]</rgn> --kind StorageV2 --sku Standard_LRS
```

<!-- Paste tip-->
[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
