<span data-ttu-id="ec44c-101">Anta att du har valt Azure PowerShell som automatiseringslösning.</span><span class="sxs-lookup"><span data-stu-id="ec44c-101">Suppose you have chosen Azure PowerShell as your automation solution.</span></span> <span data-ttu-id="ec44c-102">Dina Administratörer föredrar att köra sina skript lokalt i stället för i Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="ec44c-102">Your administrators prefer to run their scripts locally rather than in the Azure Cloud Shell.</span></span> <span data-ttu-id="ec44c-103">Teamet använder datorer som kör Linux, macOS och Windows.</span><span class="sxs-lookup"><span data-stu-id="ec44c-103">The team uses machines that run Linux, macOS, and Windows.</span></span> <span data-ttu-id="ec44c-104">Du måste få igång Azure PowerShell på alla deras enheter.</span><span class="sxs-lookup"><span data-stu-id="ec44c-104">You need to get Azure PowerShell working on all their devices.</span></span> 

## <a name="what-must-be-installed"></a><span data-ttu-id="ec44c-105">Vad måste finnas installerat?</span><span class="sxs-lookup"><span data-stu-id="ec44c-105">What must be installed?</span></span>
<span data-ttu-id="ec44c-106">Vi kommer att gå igenom de faktiska installationsinstruktionerna i nästa utbildningsenhet, men nu ska vi titta på de båda komponenter som utgör Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ec44c-106">We'll go through the actual installation instructions in the next unit, but let's look at the two components which make up Azure PowerShell.</span></span>

- <span data-ttu-id="ec44c-107">**PowerShell-basprodukten** Den här produkten finns i två varianter: PowerShell för Windows och PowerShell Core för macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="ec44c-107">**The base PowerShell product** This comes in two variants: PowerShell on Windows, and PowerShell Core on macOS and Linux.</span></span>
- <span data-ttu-id="ec44c-108">**Azure PowerShell-modulen** Den här extramodulen måste installeras för att du ska få tillgång till Azure-specifika kommandon i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ec44c-108">**The Azure PowerShell module** This extra module must be installed to add the Azure-specific commands to PowerShell.</span></span>

> [!TIP]
> <span data-ttu-id="ec44c-109">PowerShell ingår i Windows (men du kan behöva uppdatera det).</span><span class="sxs-lookup"><span data-stu-id="ec44c-109">PowerShell is included with Windows (but might have an update available).</span></span> <span data-ttu-id="ec44c-110">Däremot måste du installera PowerShell Core i Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="ec44c-110">You will need to install PowerShell Core on Linux and macOS.</span></span>

<span data-ttu-id="ec44c-111">När basprodukten är installerad lägger du till Azure PowerShell-modulen i installationen.</span><span class="sxs-lookup"><span data-stu-id="ec44c-111">Once the base product is installed, you then add the Azure PowerShell module to your installation.</span></span>

## <a name="how-to-install-powershell-core"></a><span data-ttu-id="ec44c-112">Så installerar du PowerShell Core</span><span class="sxs-lookup"><span data-stu-id="ec44c-112">How to install PowerShell Core</span></span>
<span data-ttu-id="ec44c-113">Du använder en pakethanterare till att installera PowerShell Core i både Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="ec44c-113">On both Linux and macOS, you use a package manager to install PowerShell Core.</span></span> <span data-ttu-id="ec44c-114">Vilken pakethanterare som rekommenderas beror på operativsystemet och distributionen.</span><span class="sxs-lookup"><span data-stu-id="ec44c-114">The recommended package manager differs by OS and distribution.</span></span>

> [!NOTE]
> <span data-ttu-id="ec44c-115">PowerShell Core är tillgängligt från Microsofts datalager, så du måste först lägga till det här datalagret i pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="ec44c-115">PowerShell Core is available in the Microsoft repository, so you'll first need to add that repository to your package manager.</span></span>

### <a name="linux"></a><span data-ttu-id="ec44c-116">Linux</span><span class="sxs-lookup"><span data-stu-id="ec44c-116">Linux</span></span>
<span data-ttu-id="ec44c-117">I Linux använder du olika pakethanterare beroende på vilken Linux-distribution du väljer.</span><span class="sxs-lookup"><span data-stu-id="ec44c-117">On Linux, the package manager will change based on the Linux distribution you choose.</span></span>

| <span data-ttu-id="ec44c-118">Distribution</span><span class="sxs-lookup"><span data-stu-id="ec44c-118">Distribution(s)</span></span>  | <span data-ttu-id="ec44c-119">Pakethanterare</span><span class="sxs-lookup"><span data-stu-id="ec44c-119">Package manager</span></span> |
|------------------|-----------------|
| <span data-ttu-id="ec44c-120">Ubuntu, Debian</span><span class="sxs-lookup"><span data-stu-id="ec44c-120">Ubuntu, Debian</span></span>   | `apt-get`       |
| <span data-ttu-id="ec44c-121">Red Hat, CentOS</span><span class="sxs-lookup"><span data-stu-id="ec44c-121">Red Hat, CentOS</span></span>  | `yum`           |
| <span data-ttu-id="ec44c-122">OpenSUSE</span><span class="sxs-lookup"><span data-stu-id="ec44c-122">OpenSUSE</span></span>         | `zypper`        |
| <span data-ttu-id="ec44c-123">Fedora</span><span class="sxs-lookup"><span data-stu-id="ec44c-123">Fedora</span></span>           | `dnf`           |

### <a name="mac"></a><span data-ttu-id="ec44c-124">Mac</span><span class="sxs-lookup"><span data-stu-id="ec44c-124">Mac</span></span>
<span data-ttu-id="ec44c-125">I macOS använder du `Homebrew` för att installera PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="ec44c-125">On macOS, you will use `Homebrew` to install PowerShell Core.</span></span>

<span data-ttu-id="ec44c-126">I nästa avsnitt går vi igenom installationsstegen för några vanliga plattformar i detalj.</span><span class="sxs-lookup"><span data-stu-id="ec44c-126">In the next section, you will go through the detailed installation steps for some common platforms.</span></span>