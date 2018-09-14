<span data-ttu-id="b2fbf-101">Nu ska vi installera Azure CLI på din lokala dator och därefter köra ett enkelt kommando för att verifiera installationen.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-101">Let's install the Azure CLI on your local machine, and then run a simple command to verify your installation.</span></span> <span data-ttu-id="b2fbf-102">Vilken metod du använder för att installera Azure CLI beror på datorns operativsystem.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-102">The method you use for installing the Azure CLI depends on the operating system of your computer.</span></span> <span data-ttu-id="b2fbf-103">Välj anvisningarna för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-103">Please choose the steps for your operating system.</span></span>

> [!NOTE]
> <span data-ttu-id="b2fbf-104">I den här övningen får du hjälp att installera verktyget Azure CLI lokalt.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-104">This exercise guides you through installing the Azure CLI tool locally.</span></span> <span data-ttu-id="b2fbf-105">Resten av modulen använder Azure Cloud Shell så att du kan utnyttja den kostnadsfria prenumerationssupporten i Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-105">The remainder of the module will use the Azure Cloud Shell so you can leverage the free subscription support in Microsoft Learn.</span></span> <span data-ttu-id="b2fbf-106">Du kan se den här övningen som en valfri aktivitet och bara läsa anvisningarna om du vill.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-106">You can consider this exercise as an optional activity and just review the instructions if you prefer.</span></span>

<span data-ttu-id="b2fbf-107">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="b2fbf-107">::: zone pivot="linux"</span></span>

## <a name="linux"></a><span data-ttu-id="b2fbf-108">Linux</span><span class="sxs-lookup"><span data-stu-id="b2fbf-108">Linux</span></span>

<span data-ttu-id="b2fbf-109">Här ska du installera Azure CLI på **Ubuntu Linux** med (**APT**) och Bash-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-109">Here you will install the Azure CLI on **Ubuntu Linux** using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span>

> [!TIP]
> <span data-ttu-id="b2fbf-110">Kommandona nedan är för Ubuntu version 18.04.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-110">The commands listed below are for Ubuntu version 18.04.</span></span> <span data-ttu-id="b2fbf-111">Andra versioner och distributioner av Linux har andra anvisningar.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-111">Other versions and distributions of Linux have different instructions.</span></span> <span data-ttu-id="b2fbf-112">Läs i den [officiella dokumentationen](https://docs.microsoft.com/cli/azure/install-azure-cli) om du använder en annan version av Linux.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-112">Check the [official documentation](https://docs.microsoft.com/cli/azure/install-azure-cli) if you are using a different Linux version.</span></span>

1. <span data-ttu-id="b2fbf-113">Ändra din källista så att Microsoft-lagringsplatsen registreras, och så att pakethanteraren kan hitta Azure CLI-paketet.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-113">Modify your sources list so that the Microsoft repository is registered, and the package manager can locate the Azure CLI package.</span></span>

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. <span data-ttu-id="b2fbf-114">Importera krypteringsnyckeln för Microsoft Ubuntu-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-114">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="b2fbf-115">Då kan pakethanteraren verifiera att Azure CLI-paketet som du installerar kommer från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-115">This will allow the package manager to verify that the Azure CLI package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="b2fbf-116">Installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-116">Install the Azure CLI.</span></span>

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

<span data-ttu-id="b2fbf-117">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="b2fbf-117">::: zone-end</span></span>

<span data-ttu-id="b2fbf-118">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="b2fbf-118">::: zone pivot="macos"</span></span>

## <a name="macos"></a><span data-ttu-id="b2fbf-119">macOS</span><span class="sxs-lookup"><span data-stu-id="b2fbf-119">macOS</span></span>

<span data-ttu-id="b2fbf-120">Här installerar du Azure CLI på macOS med Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-120">Here you will install the Azure CLI on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b2fbf-121">Om kommandot **brew** är otillgängligt kanske du måste installera Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-121">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="b2fbf-122">Mer information finns på [Homebrews webbplats](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="b2fbf-122">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="b2fbf-123">Uppdatera brew-lagringsplatsen för att se till att du får det senaste Azure CLI-paketet.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-123">Update your brew repository to make sure you get the latest Azure CLI package.</span></span>

    ```bash
    brew update
    ```

1. <span data-ttu-id="b2fbf-124">Installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-124">Install the Azure CLI.</span></span>

    ```bash
    brew install azure-cli
    ```

<span data-ttu-id="b2fbf-125">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="b2fbf-125">::: zone-end</span></span>

<span data-ttu-id="b2fbf-126">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="b2fbf-126">::: zone pivot="windows"</span></span>

## <a name="windows"></a><span data-ttu-id="b2fbf-127">Windows</span><span class="sxs-lookup"><span data-stu-id="b2fbf-127">Windows</span></span>

<span data-ttu-id="b2fbf-128">Här installerar du Azure CLI på Windows med hjälp av MSI-installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-128">Here you will install the Azure CLI on Windows using the MSI installer.</span></span>

1. <span data-ttu-id="b2fbf-129">Gå till [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), och i webbläsarens dialogruta för säkerhet klickar du på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-129">Go to [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), and in the browser security dialog box, click **Run**.</span></span>
1. <span data-ttu-id="b2fbf-130">I installationsprogrammet godkänner du licensvillkoren och klickar sedan på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-130">In the installer, accept the license terms, and then click **Install**.</span></span>
1. <span data-ttu-id="b2fbf-131">I dialogrutan **User Account Control** väljer du **Ja**.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-131">In the **User Account Control** dialog, select **Yes**.</span></span>

<span data-ttu-id="b2fbf-132">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="b2fbf-132">::: zone-end</span></span>

## <a name="running-the-azure-cli"></a><span data-ttu-id="b2fbf-133">Köra Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b2fbf-133">Running the Azure CLI</span></span>

<span data-ttu-id="b2fbf-134">Du kör Azure CLI genom att öppna ett bash-gränssnitt (Linux och macOS), eller från kommandotolken eller PowerShell (Windows).</span><span class="sxs-lookup"><span data-stu-id="b2fbf-134">You run the Azure CLI by opening a bash shell (Linux and macOS), or from the command prompt or PowerShell (Windows).</span></span>

1. <span data-ttu-id="b2fbf-135">Starta Azure CLI och verifiera installationen genom att köra versionskontrollen.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-135">Start the Azure CLI and verify your installation by running the version check.</span></span>

    ```azurecli
    az --version
    ```

<span data-ttu-id="b2fbf-136">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="b2fbf-136">::: zone pivot="windows"</span></span>

> [!TIP]
> <span data-ttu-id="b2fbf-137">Det finns några fördelar med att köra Azure CLI från PowerShell jämfört med köra Azure CLI från Kommandotolken för Windows.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-137">Running the Azure CLI from PowerShell has some advantages over running the Azure CLI from the Windows command prompt.</span></span> <span data-ttu-id="b2fbf-138">PowerShell ger ytterligare funktioner än de som är tillgängliga från Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-138">PowerShell provides additional tab completion features over those available from the command prompt.</span></span> 

<span data-ttu-id="b2fbf-139">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="b2fbf-139">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="b2fbf-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b2fbf-140">Summary</span></span>

<span data-ttu-id="b2fbf-141">Du konfigurerar dina lokala datorer för att administrera Azure-resurser med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-141">You set up your local machines to administer Azure resources with the Azure CLI.</span></span> <span data-ttu-id="b2fbf-142">Du kan nu använda Azure CLI lokalt för att ange kommandon eller köra skript.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-142">You can now use the Azure CLI locally to enter commands or execute scripts.</span></span> <span data-ttu-id="b2fbf-143">Azure CLI vidarebefordrar dina kommandon till Azure-datacenter där de körs i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b2fbf-143">The Azure CLI will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>