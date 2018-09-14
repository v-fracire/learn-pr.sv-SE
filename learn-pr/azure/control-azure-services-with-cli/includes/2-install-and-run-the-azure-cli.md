<span data-ttu-id="5a2c5-101">Anta att ditt företag har valt Azure CLI som kommandoradslösning för hantering av Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-101">Suppose your company has chosen the Azure CLI as your command-line solution for managing Azure resources.</span></span> <span data-ttu-id="5a2c5-102">Dina administratörer och utvecklare föredrar att köra sina kommandon och skript lokalt i stället för via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-102">Your administrators and developers prefer to run their commands and scripts locally rather than through a browser.</span></span> <span data-ttu-id="5a2c5-103">Teamet använder datorer som kör Linux, macOS och Windows.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-103">The team uses machines that run Linux, macOS, and Windows.</span></span> <span data-ttu-id="5a2c5-104">Du måste få igång Azure CLI på alla deras enheter.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-104">You need to get the Azure CLI working on all their devices.</span></span>

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="5a2c5-105">Vad är Azure CLI?</span><span class="sxs-lookup"><span data-stu-id="5a2c5-105">What is the Azure CLI?</span></span>
<span data-ttu-id="5a2c5-106">Azure CLI är ett kommandoradsprogram som används för att ansluta till Azure och köra administrativa kommandon på Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-106">The Azure CLI is a command-line program to connect to Azure and execute administrative commands on Azure resources.</span></span> <span data-ttu-id="5a2c5-107">Om du till exempel vill starta om en virtuell dator (VM) använder du ett kommando liknande följande:</span><span class="sxs-lookup"><span data-stu-id="5a2c5-107">For example, to restart a Virtual Machine (VM), you would use a command like the following:</span></span>

 ```bash
 az vm restart -g MyResourceGroup -n MyVm
 ```

<span data-ttu-id="5a2c5-108">Azure CLI tillhandahåller plattformsoberoende kommandoradsverktyg för hantering av Azure-resurser och kan installeras lokalt på Linux-, Mac- eller Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-108">The Azure CLI provides cross-platform command-line tools for managing Azure resources, and can be installed locally on Linux, Mac, or Windows computers.</span></span> <span data-ttu-id="5a2c5-109">Azure CLI kan också användas från en webbläsare via Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-109">The Azure CLI can also be used from a browser through the Azure Cloud Shell.</span></span> <span data-ttu-id="5a2c5-110">I båda fallen kan det användas interaktivt eller skriptas.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-110">In both cases, it can be used interactively or scripted.</span></span> <span data-ttu-id="5a2c5-111">För interaktiv användning startar du först ett gränssnitt som cmd.exe i Windows eller Bash i Linux eller macOS och anger sedan kommandot i gränssnittets kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-111">For interactive use, you first launch a shell such as cmd.exe on Windows or Bash on Linux or macOS and then issue the command at the shell prompt.</span></span> <span data-ttu-id="5a2c5-112">Om du vill automatisera repetitiva uppgifter sätter du samman CLI-kommandona till ett kommandoskript med hjälp av skriptsyntax för det valda gränssnittet och kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-112">To automate repetitive tasks, you assemble the CLI commands into a shell script using the script syntax of your chosen shell and then execute the script.</span></span>

## <a name="how-to-install-azure-cli"></a><span data-ttu-id="5a2c5-113">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5a2c5-113">How to install Azure CLI</span></span>
<span data-ttu-id="5a2c5-114">Du använder en pakethanterare för att installera PowerShell Core i både Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-114">On both Linux and macOS, you use a package manager to install PowerShell Core.</span></span> <span data-ttu-id="5a2c5-115">Den rekommenderade pakethanteraren skiljer sig beroende på operativsystem och distribution:</span><span class="sxs-lookup"><span data-stu-id="5a2c5-115">The recommended package manager differs by OS and distribution:</span></span>
- <span data-ttu-id="5a2c5-116">Linux: **apt-get** i Ubuntu, **yum** i Red Hat och **zypper** i OpenSUSE</span><span class="sxs-lookup"><span data-stu-id="5a2c5-116">Linux: **apt-get** on Ubuntu, **yum** on Red Hat, and **zypper** on OpenSUSE</span></span>
- <span data-ttu-id="5a2c5-117">Mac: **Homebrew**</span><span class="sxs-lookup"><span data-stu-id="5a2c5-117">Mac: **Homebrew**</span></span>

<span data-ttu-id="5a2c5-118">Azure CLI är tillgängligt i Microsofts databas, så du måste först lägga till databasen till pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-118">The Azure CLI is available in the Microsoft repository, so you'll first need to add that repository to your package manager.</span></span>

<span data-ttu-id="5a2c5-119">I Windows installerar du Azure CLI genom att hämta och köra en MSI-fil.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-119">On Windows, you install the Azure CLI by downloading and running an MSI file.</span></span>

## <a name="using-the-azure-cli-in-scripts"></a><span data-ttu-id="5a2c5-120">Använda Azure CLI i skript</span><span class="sxs-lookup"><span data-stu-id="5a2c5-120">Using the Azure CLI in scripts</span></span>
<span data-ttu-id="5a2c5-121">Om du vill använda Azure CLI-kommandona i skript måste du vara medveten om eventuella problem relaterade till ”servergränssnittet” eller miljön som ska användas för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-121">If you want to use the Azure CLI commands in scripts, you need to be aware of any issues around the "shell" or environment used for running the script.</span></span> <span data-ttu-id="5a2c5-122">I ett bash-gränssnitt används exempelvis följande syntax för att ställa in variabler:</span><span class="sxs-lookup"><span data-stu-id="5a2c5-122">For example, in a bash shell, the following syntax is used when setting variables:</span></span>

 ```bash
 variable="string"
 variable=integer
 ```

<span data-ttu-id="5a2c5-123">Om du använder en PowerShell-miljö för att köra Azure CLI-skript måste du använda den här syntaxen för variabler:</span><span class="sxs-lookup"><span data-stu-id="5a2c5-123">If you use a PowerShell environment for running Azure CLI scripts, you'll need to use this syntax for variables:</span></span>

 ```powershell
 $variable="string"
 $variable=integer
 ```

## <a name="summary"></a><span data-ttu-id="5a2c5-124">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5a2c5-124">Summary</span></span>
<span data-ttu-id="5a2c5-125">Azure CLI måste installeras innan det kan användas för att hantera Azure-resurser från en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-125">The Azure CLI must be installed before it can be used to manage Azure resources from a local computer.</span></span> <span data-ttu-id="5a2c5-126">Installationsstegen varierar för Windows, Linux och macOS, men när det har installerats är kommandona samma för alla plattformar.</span><span class="sxs-lookup"><span data-stu-id="5a2c5-126">The installation steps vary for Windows, Linux, and macOS, but once installed, the commands are common across platforms.</span></span> 