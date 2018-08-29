
<span data-ttu-id="c6a2d-101">I den här övningen installerar du Azure CLI på din lokala dator och kör därefter ett enkelt kommando för att verifiera installationen.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-101">In this exercise, you will install the Azure CLI on your local machine, and then run a simple command to verify your installation.</span></span> 

## <a name="installing-the-azure-cli"></a><span data-ttu-id="c6a2d-102">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c6a2d-102">Installing the Azure CLI</span></span>
<span data-ttu-id="c6a2d-103">Vilken metod du använder för att installera Azure CLI beroende på datorns operativsystem.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-103">The method you use for installing the Azure CLI depends on the operating system of your computer.</span></span> <span data-ttu-id="c6a2d-104">Välj anvisningarna för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-104">Please choose the steps for your operating system.</span></span>

### <a name="linux"></a><span data-ttu-id="c6a2d-105">Linux</span><span class="sxs-lookup"><span data-stu-id="c6a2d-105">Linux</span></span>
<span data-ttu-id="c6a2d-106">Här ska du installera Azure CLI på Ubuntu Linux med (**APT**) och Bash-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-106">Here you will install the Azure CLI on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a2d-107">Kommandona nedan är för Ubuntu version 18.04.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-107">The commands listed below are for Ubuntu version 18.04.</span></span> <span data-ttu-id="c6a2d-108">Om du använder en annan version av Ubuntu måste du lägga till en annan lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-108">If you are using a different version of Ubuntu, you must add a different repository.</span></span> <span data-ttu-id="c6a2d-109">Mer information finns i [Installera Azure CLI 2.0 med apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).</span><span class="sxs-lookup"><span data-stu-id="c6a2d-109">For details see [Install the Azure CLI 2.0 with apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).</span></span>

1. <span data-ttu-id="c6a2d-110">Ändra din källista så att Microsoft-lagringsplatsen registreras, och så att pakethanteraren kan hitta Azure CLI-paketet.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-110">Modify your sources list so that the Microsoft repository is registered, and the package manager can locate the Azure CLI package.</span></span>

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```
1. <span data-ttu-id="c6a2d-111">Importera krypteringsnyckeln för Microsoft Ubuntu-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-111">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="c6a2d-112">Då kan pakethanteraren verifiera att Azure CLI-paketet som du installerar kommer från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-112">This will allow the package manager to verify that the Azure CLI package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```
1. <span data-ttu-id="c6a2d-113">Installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-113">Install the Azure CLI.</span></span>

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

### <a name="macos"></a><span data-ttu-id="c6a2d-114">macOS</span><span class="sxs-lookup"><span data-stu-id="c6a2d-114">macOS</span></span>
<span data-ttu-id="c6a2d-115">Här installerar du Azure CLI på macOS med Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-115">Here you will install the Azure CLI on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6a2d-116">Om kommandot **brew** är otillgängligt kanske du måste installera Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-116">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="c6a2d-117">Mer information finns på [Homebrews webbplats](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="c6a2d-117">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="c6a2d-118">Uppdatera brew-lagringsplatsen för att se till att du får det senaste Azure CLI-paketet.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-118">Update your brew repository to make sure you get the latest Azure CLI package.</span></span>

    ```bash
    brew update
    ```
1. <span data-ttu-id="c6a2d-119">Installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-119">Install the Azure CLI.</span></span>

    ```bash
    brew install azure-cli
    ```

### <a name="windows"></a><span data-ttu-id="c6a2d-120">Windows</span><span class="sxs-lookup"><span data-stu-id="c6a2d-120">Windows</span></span>
<span data-ttu-id="c6a2d-121">Här installerar du Azure CLI på Windows med hjälp av MSI-installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-121">Here you will install the Azure CLI on Windows using the MSI installer.</span></span>

1. <span data-ttu-id="c6a2d-122">Gå till [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), och i webbläsarens dialogruta för säkerhet klickar du på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-122">Go to [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), and in the browser security dialog box, click **Run**.</span></span>
1. <span data-ttu-id="c6a2d-123">I installationsprogrammet godkänner du licensvillkoren och klickar sedan på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-123">In the installer, accept the license terms, and then click **Install**.</span></span>
1. <span data-ttu-id="c6a2d-124">I dialogrutan **User Account Control** väljer du **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-124">In the **User Account Control** dialog, select **Yes**.</span></span>

## <a name="running-the-azure-cli"></a><span data-ttu-id="c6a2d-125">Köra Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c6a2d-125">Running the Azure CLI</span></span>
<span data-ttu-id="c6a2d-126">Du kör Azure CLI genom att öppna ett bash-gränssnitt (Linux och macOS), eller från kommandotolken eller PowerShell (Windows).</span><span class="sxs-lookup"><span data-stu-id="c6a2d-126">You run the Azure CLI by opening a bash shell (Linux and macOS), or from the command prompt or PowerShell (Windows).</span></span>

1. <span data-ttu-id="c6a2d-127">Starta Azure CLI och verifiera installationen genom att köra versionskontrollen.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-127">Start the Azure CLI and verify your installation by running the version check.</span></span>

    ```bash
    az --version
    ```

> [!NOTE]
> <span data-ttu-id="c6a2d-128">I Windows finns det några fördelar med att köra Azure CLI från PowerShell jämfört med att köra Azure CLI från kommandotolken. Exempelvis tillhandahåller PowerShell extra funktioner för tabbifyllning utöver dem som finns i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-128">In Windows, running the Azure CLI from PowerShell has some advantages over running the Azure CLI from the command prompt; for example, PowerShell provides additional tab completion features over those available from the command prompt.</span></span> 

## <a name="summary"></a><span data-ttu-id="c6a2d-129">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c6a2d-129">Summary</span></span>
<span data-ttu-id="c6a2d-130">Du konfigurerar dina lokala datorer för att administrera Azure-resurser med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-130">You set up your local machines to administer Azure resources with the Azure CLI.</span></span> <span data-ttu-id="c6a2d-131">Du kan nu använda Azure CLI lokalt för att ange kommandon eller köra skript.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-131">You can now use the Azure CLI locally to enter commands or execute scripts.</span></span> <span data-ttu-id="c6a2d-132">Azure CLI vidarebefordrar dina kommandon till Azure-datacenter där de körs i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c6a2d-132">The Azure CLI will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>
