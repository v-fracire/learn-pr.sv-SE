<span data-ttu-id="140dc-101">I den här delen installerar du **Azure PowerShell** på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="140dc-101">In this unit, you will install **Azure PowerShell** on your local machine.</span></span> <span data-ttu-id="140dc-102">Välj rätt avsnitt för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="140dc-102">Choose the appropriate section for your operating system.</span></span>

## <a name="linux-and-mac"></a><span data-ttu-id="140dc-103">Linux och Mac</span><span class="sxs-lookup"><span data-stu-id="140dc-103">Linux and Mac</span></span>
<span data-ttu-id="140dc-104">På Linux och macOS är det första steget att installera **PowerShell Core**.</span><span class="sxs-lookup"><span data-stu-id="140dc-104">On Linux and macOS, the first step is to install **PowerShell Core**.</span></span>

### <a name="linux"></a><span data-ttu-id="140dc-105">Linux</span><span class="sxs-lookup"><span data-stu-id="140dc-105">Linux</span></span>
<span data-ttu-id="140dc-106">Som vi nämnde i den sista delen inbegriper installation av PowerShell för Linux användning av en pakethanterare.</span><span class="sxs-lookup"><span data-stu-id="140dc-106">As mentioned in the last unit, installing PowerShell for Linux will involve using a package manager.</span></span> <span data-ttu-id="140dc-107">Vi kommer att använda **Ubuntu 18.04** i vårt exempel här, men vi har [detaljerade instruktioner för andra versioner och distributioner i vår dokumentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span><span class="sxs-lookup"><span data-stu-id="140dc-107">We will use **Ubuntu 18.04** for our example here, but we have [detailed instructions for other versions and distributions in our documentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span></span>

<span data-ttu-id="140dc-108">Du ska installera PowerShell Core på Ubuntu Linux med (**APT**) och Bash-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="140dc-108">You will install PowerShell Core on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span> 

1. <span data-ttu-id="140dc-109">Importera krypteringsnyckeln för Microsoft Ubuntu-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="140dc-109">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="140dc-110">Då kan pakethanteraren verifiera att PowerShell Core-paketet som du installerar kommer från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="140dc-110">This will allow the package manager to verify that the PowerShell Core package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="140dc-111">Registrera **Microsoft Ubuntu-lagringsplatsen** så att pakethanteraren kan hitta PowerShell Core-paketet.</span><span class="sxs-lookup"><span data-stu-id="140dc-111">Register the **Microsoft Ubuntu repository** so the package manager can locate the PowerShell Core package.</span></span>

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. <span data-ttu-id="140dc-112">Uppdatera listan över paket.</span><span class="sxs-lookup"><span data-stu-id="140dc-112">Update the list of packages.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="140dc-113">Installera PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="140dc-113">Install PowerShell Core.</span></span>

    ```bash
    sudo apt-get install -y powershell
    ```

1. <span data-ttu-id="140dc-114">Starta PowerShell för att verifiera att det har installerats ordentligt.</span><span class="sxs-lookup"><span data-stu-id="140dc-114">Start PowerShell to verify that it installed successfully.</span></span>

    ```bash
    pwsh
    ```

### <a name="macos"></a><span data-ttu-id="140dc-115">macOS</span><span class="sxs-lookup"><span data-stu-id="140dc-115">macOS</span></span>
<span data-ttu-id="140dc-116">Installera därefter **PowerShell Core** på macOS med Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="140dc-116">Next, install **PowerShell Core** on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="140dc-117">Om kommandot **brew** är otillgängligt kanske du måste installera Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="140dc-117">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="140dc-118">Mer information finns på [Homebrews webbplats](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="140dc-118">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="140dc-119">Installera Homebrew-Cask för att hämta fler paket som PowerShell Core-paketet:</span><span class="sxs-lookup"><span data-stu-id="140dc-119">Install Homebrew-Cask to obtain more packages, including the PowerShell Core package:</span></span>

    ```bash
    brew tap caskroom/cask
    ```

1. <span data-ttu-id="140dc-120">Installera PowerShell Core:</span><span class="sxs-lookup"><span data-stu-id="140dc-120">Install PowerShell Core:</span></span>

    ```bash
    brew cask installs powershell
    ```

1. <span data-ttu-id="140dc-121">Starta PowerShell Core för att verifiera att det har installerats ordentligt:</span><span class="sxs-lookup"><span data-stu-id="140dc-121">Start PowerShell Core to verify that it installed successfully:</span></span>

    ```bash
    pwsh
    ```

## <a name="install-azure-powershell"></a><span data-ttu-id="140dc-122">Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="140dc-122">Install Azure PowerShell</span></span>
<span data-ttu-id="140dc-123">När du har installerat den grundläggande **PowerShell**-produkten installerar du **Azure PowerShell** för att lägga till de Azure-specifika kommandona.</span><span class="sxs-lookup"><span data-stu-id="140dc-123">After installing the base **PowerShell** product, install **Azure PowerShell** to add the Azure-specific commands.</span></span>

### <a name="windows"></a><span data-ttu-id="140dc-124">Windows</span><span class="sxs-lookup"><span data-stu-id="140dc-124">Windows</span></span>
<span data-ttu-id="140dc-125">Installera Azure PowerShell på Windows med `Install-Module` PowerShell-kommandot.</span><span class="sxs-lookup"><span data-stu-id="140dc-125">Install Azure PowerShell on Windows using the `Install-Module` PowerShell command.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="140dc-126">Du måste ha PowerShell-version 5.0 eller senare för att installera Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="140dc-126">You must have PowerShell version 5.0 or higher to install Azure PowerShell.</span></span> <span data-ttu-id="140dc-127">Kontrollera din version av PowerShell med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="140dc-127">To check your version of PowerShell, use the following command:</span></span> 
>
> `$PSVersionTable.PSVersion` 
>
><span data-ttu-id="140dc-128">Om versionsnumret är lägre än 5.0 följer du instruktionerna för att [uppgradera din befintliga Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="140dc-128">If the version number is lower than 5.0, follow the instructions for [upgrading existing Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span></span>

1. <span data-ttu-id="140dc-129">Öppna **startmenyn** och skriv **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="140dc-129">Open the **Start** menu and type **Windows PowerShell**.</span></span>

1. <span data-ttu-id="140dc-130">Högerklicka på ikonen för **Windows PowerShell** och välj **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="140dc-130">Right-click the **Windows PowerShell** icon and select **Run as administrator**.</span></span>

1. <span data-ttu-id="140dc-131">I dialogrutan **User Account Control** väljer du **Ja**.</span><span class="sxs-lookup"><span data-stu-id="140dc-131">In the **User Account Control** dialog, select **Yes**.</span></span>

1. <span data-ttu-id="140dc-132">Ange följande kommando och tryck på Enter:</span><span class="sxs-lookup"><span data-stu-id="140dc-132">Type the following command, and then press Enter:</span></span>

    ```powershell
    Install-Module -Name AzureRM
    ```

1. <span data-ttu-id="140dc-133">Om du blir tillfrågad om du litar på moduler från PSGallery svarar du **Ja** eller **Ja till alla**.</span><span class="sxs-lookup"><span data-stu-id="140dc-133">If you are asked whether you trust modules from PSGallery, answer **Yes** or **Yes to All**.</span></span>

> [!TIP]
> <span data-ttu-id="140dc-134">Om du får ett felmeddelande som anger att en version av Azure PowerShell-modulen redan är installerad kan du uppdatera till den _senaste_ versionen genom att köra kommandot:</span><span class="sxs-lookup"><span data-stu-id="140dc-134">If you get an error message indicating that a version of the Azure PowerShell module is already installed, you can update to the _latest_ version by issuing the command:</span></span>
> 
> `Update-Module -Name AzureRM`
> 
> <span data-ttu-id="140dc-135">Precis som för kommandot `Install-Module` ska du svara **Ja** eller **Ja till alla** när du blir uppmanad att lita på modulen.</span><span class="sxs-lookup"><span data-stu-id="140dc-135">As with the `Install-Module` command, answer **Yes** or **Yes to All** when prompted to trust the module.</span></span>

### <a name="linux-or-macos"></a><span data-ttu-id="140dc-136">Linux eller macOS</span><span class="sxs-lookup"><span data-stu-id="140dc-136">Linux or macOS</span></span>
<span data-ttu-id="140dc-137">Vi använder samma grundläggande process för att installera Azure PowerShell på antingen Linux eller macOS.</span><span class="sxs-lookup"><span data-stu-id="140dc-137">We use the same basic process to install the Azure PowerShell on either Linux or macOS.</span></span> <span data-ttu-id="140dc-138">Proceduren är densamma för båda operativsystemen.</span><span class="sxs-lookup"><span data-stu-id="140dc-138">The procedure is the same for both operating systems.</span></span> <span data-ttu-id="140dc-139">Skillnaden gäller hämtning av en utökad PowerShell Core-session.</span><span class="sxs-lookup"><span data-stu-id="140dc-139">The difference is in getting an elevated PowerShell Core session.</span></span>

1. <span data-ttu-id="140dc-140">I en terminal skriver du följande kommando för att starta PowerShell Core med utökad behörighet.</span><span class="sxs-lookup"><span data-stu-id="140dc-140">In a terminal, type the following command to launch PowerShell Core with elevated privileges.</span></span>

    ```bash
    sudo pwsh
    ```

1. <span data-ttu-id="140dc-141">Kör följande kommando i Azure PowerShell-kommandotolken för att installera Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="140dc-141">Run the following command at the PowerShell Core prompt to install Azure PowerShell.</span></span>

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. <span data-ttu-id="140dc-142">Om du blir tillfrågad om du litar på moduler från **PSGallery** svarar du **Ja** eller **Ja till alla**.</span><span class="sxs-lookup"><span data-stu-id="140dc-142">If you are asked whether you trust modules from **PSGallery**, answer **Yes** or **Yes to All**.</span></span>

## <a name="summary"></a><span data-ttu-id="140dc-143">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="140dc-143">Summary</span></span>
<span data-ttu-id="140dc-144">Du har konfigurerat dina lokala datorer för att administrera Azure-resurser med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="140dc-144">You have setup your local machine(s) to administer Azure resources with Azure PowerShell.</span></span> <span data-ttu-id="140dc-145">Du kan nu använda Azure PowerShell lokalt för att ange kommandon eller köra skript.</span><span class="sxs-lookup"><span data-stu-id="140dc-145">You can now use Azure PowerShell locally to enter commands or execute scripts.</span></span> <span data-ttu-id="140dc-146">Azure PowerShell vidarebefordrar dina kommandon till Azure-datacenter där de körs i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="140dc-146">Azure PowerShell will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>