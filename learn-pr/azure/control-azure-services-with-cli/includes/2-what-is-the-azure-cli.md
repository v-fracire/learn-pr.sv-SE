<span data-ttu-id="03bf4-101">Azure CLI är ett kommandoradsprogram som används för att ansluta till Azure och köra administrativa kommandon på Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="03bf4-101">The Azure CLI is a command-line program to connect to Azure and execute administrative commands on Azure resources.</span></span> <span data-ttu-id="03bf4-102">Det körs i Linux, macOS och Windows och gör att administratörer och utvecklare kan köra sina kommandon via en terminal eller från kommandoraden (eller ett skript) i stället för i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="03bf4-102">It runs on Linux, macOS, and Windows and allows administrators and developers to execute their commands through a terminal or command-line prompt (or script!) instead of a web browser.</span></span> <span data-ttu-id="03bf4-103">Om du till exempel vill starta om en virtuell dator (VM) använder du ett kommando liknande följande:</span><span class="sxs-lookup"><span data-stu-id="03bf4-103">For example, to restart a virtual machine (VM), you would use a command like the following:</span></span>

 ```azurecli
 az vm restart -g MyResourceGroup -n MyVm
 ```

<span data-ttu-id="03bf4-104">Azure CLI tillhandahåller plattformsoberoende kommandoradsverktyg för hantering av Azure-resurser och kan installeras lokalt på Linux-, Mac- eller Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="03bf4-104">The Azure CLI provides cross-platform command-line tools for managing Azure resources, and can be installed locally on Linux, Mac, or Windows computers.</span></span> <span data-ttu-id="03bf4-105">Azure CLI kan också användas från en webbläsare via Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="03bf4-105">The Azure CLI can also be used from a browser through the Azure Cloud Shell.</span></span> <span data-ttu-id="03bf4-106">I båda fallen kan det användas interaktivt eller skriptas.</span><span class="sxs-lookup"><span data-stu-id="03bf4-106">In both cases, it can be used interactively or scripted.</span></span> <span data-ttu-id="03bf4-107">För interaktiv användning startar du först ett gränssnitt som cmd.exe i Windows eller Bash i Linux eller macOS och anger sedan kommandot i gränssnittets kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="03bf4-107">For interactive use, you first launch a shell such as cmd.exe on Windows or Bash on Linux or macOS and then issue the command at the shell prompt.</span></span> <span data-ttu-id="03bf4-108">Om du vill automatisera repetitiva uppgifter sätter du samman CLI-kommandona till ett kommandoskript med hjälp av skriptsyntax för det valda gränssnittet och kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="03bf4-108">To automate repetitive tasks, you assemble the CLI commands into a shell script using the script syntax of your chosen shell and then execute the script.</span></span>

## <a name="how-to-install-azure-cli"></a><span data-ttu-id="03bf4-109">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="03bf4-109">How to install Azure CLI</span></span>

<span data-ttu-id="03bf4-110">Du använder en pakethanterare för att installera PowerShell Core i både Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="03bf4-110">On both Linux and macOS, you use a package manager to install PowerShell Core.</span></span> <span data-ttu-id="03bf4-111">Den rekommenderade pakethanteraren skiljer sig beroende på operativsystem och distribution:</span><span class="sxs-lookup"><span data-stu-id="03bf4-111">The recommended package manager differs by OS and distribution:</span></span>

- <span data-ttu-id="03bf4-112">Linux: **apt-get** i Ubuntu, **yum** i Red Hat och **zypper** i OpenSUSE</span><span class="sxs-lookup"><span data-stu-id="03bf4-112">Linux: **apt-get** on Ubuntu, **yum** on Red Hat, and **zypper** on OpenSUSE</span></span>
- <span data-ttu-id="03bf4-113">Mac: **Homebrew**</span><span class="sxs-lookup"><span data-stu-id="03bf4-113">Mac: **Homebrew**</span></span>

<span data-ttu-id="03bf4-114">Azure CLI är tillgängligt i Microsofts databas, så du måste först lägga till databasen till pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="03bf4-114">The Azure CLI is available in the Microsoft repository, so you'll first need to add that repository to your package manager.</span></span>

<span data-ttu-id="03bf4-115">I Windows installerar du Azure CLI genom att hämta och köra en MSI-fil.</span><span class="sxs-lookup"><span data-stu-id="03bf4-115">On Windows, you install the Azure CLI by downloading and running an MSI file.</span></span>

## <a name="using-the-azure-cli-in-scripts"></a><span data-ttu-id="03bf4-116">Använda Azure CLI i skript</span><span class="sxs-lookup"><span data-stu-id="03bf4-116">Using the Azure CLI in scripts</span></span>

<span data-ttu-id="03bf4-117">Om du vill använda Azure CLI-kommandona i skript måste du vara medveten om eventuella problem relaterade till ”servergränssnittet” eller miljön som ska användas för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="03bf4-117">If you want to use the Azure CLI commands in scripts, you need to be aware of any issues around the "shell" or environment used for running the script.</span></span> <span data-ttu-id="03bf4-118">I ett bash-gränssnitt används exempelvis följande syntax för att ställa in variabler:</span><span class="sxs-lookup"><span data-stu-id="03bf4-118">For example, in a bash shell, the following syntax is used when setting variables:</span></span>

```azurecli
variable="value"
variable=integer
```

<span data-ttu-id="03bf4-119">Om du använder en PowerShell-miljö för att köra Azure CLI-skript måste du använda den här syntaxen för variabler:</span><span class="sxs-lookup"><span data-stu-id="03bf4-119">If you use a PowerShell environment for running Azure CLI scripts, you'll need to use this syntax for variables:</span></span>

```powershell
$variable="value"
$variable=integer
```

<span data-ttu-id="03bf4-120">Azure CLI måste installeras innan det kan användas för att hantera Azure-resurser från en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="03bf4-120">The Azure CLI must be installed before it can be used to manage Azure resources from a local computer.</span></span> <span data-ttu-id="03bf4-121">Installationsstegen varierar för Windows, Linux och macOS, men när det har installerats är kommandona samma för alla plattformar.</span><span class="sxs-lookup"><span data-stu-id="03bf4-121">The installation steps vary for Windows, Linux, and macOS, but once installed, the commands are common across platforms.</span></span>
