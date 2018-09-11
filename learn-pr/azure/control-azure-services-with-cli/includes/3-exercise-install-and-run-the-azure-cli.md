<span data-ttu-id="17f41-101">Nu ska vi installera Azure CLI på din lokala dator och därefter köra ett enkelt kommando för att verifiera installationen.</span><span class="sxs-lookup"><span data-stu-id="17f41-101">Let's install the Azure CLI on your local machine, and then run a simple command to verify your installation.</span></span> <span data-ttu-id="17f41-102">Vilken metod du använder för att installera Azure CLI beror på datorns operativsystem.</span><span class="sxs-lookup"><span data-stu-id="17f41-102">The method you use for installing the Azure CLI depends on the operating system of your computer.</span></span> <span data-ttu-id="17f41-103">Välj anvisningarna för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="17f41-103">Please choose the steps for your operating system.</span></span>

<span data-ttu-id="17f41-104">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="17f41-104">::: zone pivot="linux"</span></span>

### <a name="linux"></a><span data-ttu-id="17f41-105">Linux</span><span class="sxs-lookup"><span data-stu-id="17f41-105">Linux</span></span>

<span data-ttu-id="17f41-106">Här ska du installera Azure CLI på **Ubuntu Linux** med (**APT**) och Bash-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="17f41-106">Here you will install the Azure CLI on **Ubuntu Linux** using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span>

> [!WARNING]
> <span data-ttu-id="17f41-107">Kommandona nedan är för Ubuntu version 18.04.</span><span class="sxs-lookup"><span data-stu-id="17f41-107">The commands listed below are for Ubuntu version 18.04.</span></span> <span data-ttu-id="17f41-108">Om du använder en annan version av Ubuntu måste du lägga till en annan lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="17f41-108">If you are using a different version of Ubuntu, you must add a different repository.</span></span> <span data-ttu-id="17f41-109">Mer information finns i [Installera Azure CLI 2.0 med apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).</span><span class="sxs-lookup"><span data-stu-id="17f41-109">For details see [Install the Azure CLI 2.0 with apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).</span></span>

1. <span data-ttu-id="17f41-110">Ändra din källista så att Microsoft-lagringsplatsen registreras, och så att pakethanteraren kan hitta Azure CLI-paketet.</span><span class="sxs-lookup"><span data-stu-id="17f41-110">Modify your sources list so that the Microsoft repository is registered, and the package manager can locate the Azure CLI package.</span></span>

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. <span data-ttu-id="17f41-111">Importera krypteringsnyckeln för Microsoft Ubuntu-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="17f41-111">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="17f41-112">Då kan pakethanteraren verifiera att Azure CLI-paketet som du installerar kommer från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="17f41-112">This will allow the package manager to verify that the Azure CLI package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="17f41-113">Installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="17f41-113">Install the Azure CLI.</span></span>

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

<span data-ttu-id="17f41-114">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="17f41-114">::: zone-end</span></span>

<span data-ttu-id="17f41-115">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="17f41-115">::: zone pivot="macos"</span></span>

### <a name="macos"></a><span data-ttu-id="17f41-116">macOS</span><span class="sxs-lookup"><span data-stu-id="17f41-116">macOS</span></span>

<span data-ttu-id="17f41-117">Här installerar du Azure CLI på macOS med Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="17f41-117">Here you will install the Azure CLI on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="17f41-118">Om kommandot **brew** är otillgängligt kanske du måste installera Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="17f41-118">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="17f41-119">Mer information finns på [Homebrews webbplats](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="17f41-119">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="17f41-120">Uppdatera brew-lagringsplatsen för att se till att du får det senaste Azure CLI-paketet.</span><span class="sxs-lookup"><span data-stu-id="17f41-120">Update your brew repository to make sure you get the latest Azure CLI package.</span></span>

    ```bash
    brew update
    ```

1. <span data-ttu-id="17f41-121">Installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="17f41-121">Install the Azure CLI.</span></span>

    ```bash
    brew install azure-cli
    ```

<span data-ttu-id="17f41-122">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="17f41-122">::: zone-end</span></span>

<span data-ttu-id="17f41-123">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="17f41-123">::: zone pivot="windows"</span></span>

### <a name="windows"></a><span data-ttu-id="17f41-124">Windows</span><span class="sxs-lookup"><span data-stu-id="17f41-124">Windows</span></span>

<span data-ttu-id="17f41-125">Här installerar du Azure CLI på Windows med hjälp av MSI-installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="17f41-125">Here you will install the Azure CLI on Windows using the MSI installer.</span></span>

1. <span data-ttu-id="17f41-126">Gå till [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), och i webbläsarens dialogruta för säkerhet klickar du på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="17f41-126">Go to [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), and in the browser security dialog box, click **Run**.</span></span>
1. <span data-ttu-id="17f41-127">I installationsprogrammet godkänner du licensvillkoren och klickar sedan på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="17f41-127">In the installer, accept the license terms, and then click **Install**.</span></span>
1. <span data-ttu-id="17f41-128">I dialogrutan **User Account Control** väljer du **Ja**.</span><span class="sxs-lookup"><span data-stu-id="17f41-128">In the **User Account Control** dialog, select **Yes**.</span></span>

<span data-ttu-id="17f41-129">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="17f41-129">::: zone-end</span></span>

## <a name="running-the-azure-cli"></a><span data-ttu-id="17f41-130">Köra Azure CLI</span><span class="sxs-lookup"><span data-stu-id="17f41-130">Running the Azure CLI</span></span>

<span data-ttu-id="17f41-131">Du kör Azure CLI genom att öppna ett bash-gränssnitt (Linux och macOS), eller från kommandotolken eller PowerShell (Windows).</span><span class="sxs-lookup"><span data-stu-id="17f41-131">You run the Azure CLI by opening a bash shell (Linux and macOS), or from the command prompt or PowerShell (Windows).</span></span>

1. <span data-ttu-id="17f41-132">Starta Azure CLI och verifiera installationen genom att köra versionskontrollen.</span><span class="sxs-lookup"><span data-stu-id="17f41-132">Start the Azure CLI and verify your installation by running the version check.</span></span>

    ```bash
    az --version
    ```

<span data-ttu-id="17f41-133">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="17f41-133">::: zone pivot="windows"</span></span>

> [!TIP]
> <span data-ttu-id="17f41-134">Det finns några fördelar med att köra Azure CLI från PowerShell jämfört med köra Azure CLI från Kommandotolken för Windows.</span><span class="sxs-lookup"><span data-stu-id="17f41-134">Running the Azure CLI from PowerShell has some advantages over running the Azure CLI from the Windows command prompt.</span></span> <span data-ttu-id="17f41-135">PowerShell ger ytterligare funktioner än de som är tillgängliga från Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="17f41-135">PowerShell provides additional tab completion features over those available from the command prompt.</span></span> 

<span data-ttu-id="17f41-136">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="17f41-136">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="17f41-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="17f41-137">Summary</span></span>

<span data-ttu-id="17f41-138">Du konfigurerar dina lokala datorer för att administrera Azure-resurser med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="17f41-138">You set up your local machines to administer Azure resources with the Azure CLI.</span></span> <span data-ttu-id="17f41-139">Du kan nu använda Azure CLI lokalt för att ange kommandon eller köra skript.</span><span class="sxs-lookup"><span data-stu-id="17f41-139">You can now use the Azure CLI locally to enter commands or execute scripts.</span></span> <span data-ttu-id="17f41-140">Azure CLI vidarebefordrar dina kommandon till Azure-datacenter där de körs i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="17f41-140">The Azure CLI will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>