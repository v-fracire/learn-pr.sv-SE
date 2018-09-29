<span data-ttu-id="2cb56-101">I den här delen installerar du **PowerShell** på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="2cb56-101">In this unit, you will install **PowerShell** on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="2cb56-102">I den här övningen får du hjälp att installera PowerShell-verktygen lokalt.</span><span class="sxs-lookup"><span data-stu-id="2cb56-102">This exercise guides you through installing the PowerShell tools locally.</span></span> <span data-ttu-id="2cb56-103">Resten av modulen använder Azure Cloud Shell så att du kan utnyttja den kostnadsfria prenumerationssupporten i Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="2cb56-103">The remainder of the module will use the Azure Cloud Shell so you can leverage the free subscription support in Microsoft Learn.</span></span> <span data-ttu-id="2cb56-104">Du kan se den här övningen som en valfri aktivitet och bara läsa anvisningarna om du vill.</span><span class="sxs-lookup"><span data-stu-id="2cb56-104">You can consider this exercise as an optional activity and just review the instructions if you prefer.</span></span>

<span data-ttu-id="2cb56-105">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="2cb56-105">::: zone pivot="linux"</span></span>

## <a name="linux"></a><span data-ttu-id="2cb56-106">Linux</span><span class="sxs-lookup"><span data-stu-id="2cb56-106">Linux</span></span>

<span data-ttu-id="2cb56-107">Installation av PowerShell för Linux inbegriper användning av en pakethanterare.</span><span class="sxs-lookup"><span data-stu-id="2cb56-107">Installing PowerShell for Linux will involve using a package manager.</span></span> <span data-ttu-id="2cb56-108">Vi kommer att använda **Ubuntu 18.04** i exemplet här, men det finns [detaljerade instruktioner för andra versioner och distributioner i vår dokumentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span><span class="sxs-lookup"><span data-stu-id="2cb56-108">We will use **Ubuntu 18.04** for our example here, but we have [detailed instructions for other versions and distributions in our documentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span></span>

<span data-ttu-id="2cb56-109">Du ska installera PowerShell Core på Ubuntu Linux med (**APT**) och Bash-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="2cb56-109">You will install PowerShell Core on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span> 

1. <span data-ttu-id="2cb56-110">Importera krypteringsnyckeln för Microsoft Ubuntu-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2cb56-110">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="2cb56-111">Då kan pakethanteraren verifiera att PowerShell Core-paketet som du installerar kommer från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2cb56-111">This will allow the package manager to verify that the PowerShell Core package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="2cb56-112">Registrera **Microsoft Ubuntu-lagringsplatsen** så att pakethanteraren kan hitta PowerShell Core-paketet.</span><span class="sxs-lookup"><span data-stu-id="2cb56-112">Register the **Microsoft Ubuntu repository** so the package manager can locate the PowerShell Core package.</span></span>

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. <span data-ttu-id="2cb56-113">Uppdatera listan över paket.</span><span class="sxs-lookup"><span data-stu-id="2cb56-113">Update the list of packages.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="2cb56-114">Installera PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="2cb56-114">Install PowerShell Core.</span></span>

    ```bash
    sudo apt-get install -y powershell
    ```

1. <span data-ttu-id="2cb56-115">Starta PowerShell för att verifiera att det har installerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="2cb56-115">Start PowerShell to verify that it installed successfully.</span></span>

    ```bash
    pwsh
    ```
<span data-ttu-id="2cb56-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2cb56-116">::: zone-end</span></span>

<span data-ttu-id="2cb56-117">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="2cb56-117">::: zone pivot="macos"</span></span>

## <a name="macos"></a><span data-ttu-id="2cb56-118">MacOS</span><span class="sxs-lookup"><span data-stu-id="2cb56-118">MacOS</span></span>

<span data-ttu-id="2cb56-119">På macOS är det första steget att installera **PowerShell Core**.</span><span class="sxs-lookup"><span data-stu-id="2cb56-119">On macOS, the first step is to install **PowerShell Core**.</span></span> <span data-ttu-id="2cb56-120">Detta görs med hjälp av Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="2cb56-120">This is done using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2cb56-121">Om kommandot **brew** inte är tillgängligt behöver du kanske installera Homebrew-pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="2cb56-121">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="2cb56-122">Mer information finns på [Homebrews webbplats](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="2cb56-122">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="2cb56-123">Installera Homebrew-Cask för att hämta fler paket som PowerShell Core-paketet:</span><span class="sxs-lookup"><span data-stu-id="2cb56-123">Install Homebrew-Cask to obtain more packages, including the PowerShell Core package:</span></span>

    ```bash
    brew tap caskroom/cask
    ```

1. <span data-ttu-id="2cb56-124">Installera PowerShell Core:</span><span class="sxs-lookup"><span data-stu-id="2cb56-124">Install PowerShell Core:</span></span>

    ```bash
    brew cask install powershell
    ```

1. <span data-ttu-id="2cb56-125">Starta PowerShell Core för att verifiera att det har installerats korrekt:</span><span class="sxs-lookup"><span data-stu-id="2cb56-125">Start PowerShell Core to verify that it installed successfully:</span></span>

    ```bash
    pwsh
    ```

<span data-ttu-id="2cb56-126">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2cb56-126">::: zone-end</span></span>

<span data-ttu-id="2cb56-127">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="2cb56-127">::: zone pivot="windows"</span></span>

## <a name="windows"></a><span data-ttu-id="2cb56-128">Windows</span><span class="sxs-lookup"><span data-stu-id="2cb56-128">Windows</span></span>
<span data-ttu-id="2cb56-129">PowerShell ingår i Windows, men det kan finnas en uppdatering tillgänglig för din dator.</span><span class="sxs-lookup"><span data-stu-id="2cb56-129">PowerShell is included with Windows, however there may be an update available for your machine.</span></span> <span data-ttu-id="2cb56-130">Den Azure-support som vi ska använda kräver PowerShell version 5.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2cb56-130">The Azure support we are going to use requires PowerShell version 5.0 or higher.</span></span> <span data-ttu-id="2cb56-131">Du kan kontrollera vilken version du har installerat med följande steg:</span><span class="sxs-lookup"><span data-stu-id="2cb56-131">You can check the version you have installed through the following steps:</span></span>

1. <span data-ttu-id="2cb56-132">Öppna **Start-menyn** och skriv **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="2cb56-132">Open the **Start** menu and type **Windows PowerShell**.</span></span> <span data-ttu-id="2cb56-133">Det kan finnas flera länkgenvägar:</span><span class="sxs-lookup"><span data-stu-id="2cb56-133">There may be multiple shortcut links available:</span></span>
    - <span data-ttu-id="2cb56-134">Windows PowerShell – det här är 64-bitarsversionen och är allmänt det som du bör välja.</span><span class="sxs-lookup"><span data-stu-id="2cb56-134">Windows PowerShell - this is the 64-bit version and generally what you should choose.</span></span>
    - <span data-ttu-id="2cb56-135">Windows PowerShell (x86) – det här är en 32-bitars version som är installerad på 64-bitars Windows.</span><span class="sxs-lookup"><span data-stu-id="2cb56-135">Windows PowerShell (x86) - this is a 32-bit version installed on 64-bit Windows.</span></span>
    - <span data-ttu-id="2cb56-136">Windows PowerShell ISE – Integrated Scripting Environment (ISE) används för att skriva skript i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2cb56-136">Windows PowerShell ISE - The Integrated Scripting Environment (ISE) is used for writing scripts in PowerShell.</span></span> 
    - <span data-ttu-id="2cb56-137">Windows PowerShell ISE (x86) – en 32-bitars version av ISE.</span><span class="sxs-lookup"><span data-stu-id="2cb56-137">Windows PowerShell ISE (x86) - A 32-bit version of the ISE.</span></span>

1. <span data-ttu-id="2cb56-138">Välj Windows PowerShell ISE-ikonen.</span><span class="sxs-lookup"><span data-stu-id="2cb56-138">Select the Windows PowerShell icon.</span></span>

1. <span data-ttu-id="2cb56-139">Skriv följande kommando för att kontrollera vilken version av PowerShell som är installerad.</span><span class="sxs-lookup"><span data-stu-id="2cb56-139">Type the following command to determine the version of PowerShell installed.</span></span>

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
<span data-ttu-id="2cb56-140">Om versionsnumret är lägre än 5.0 följer du dessa instruktioner för att [uppgradera det befintliga Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="2cb56-140">If the version number is lower than 5.0, follow these instructions for [upgrading existing Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span></span>

<span data-ttu-id="2cb56-141">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2cb56-141">::: zone-end</span></span>

<span data-ttu-id="2cb56-142">Du har konfigurerat dina lokala datorer för att stödja PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2cb56-142">You have setup your local machine(s) to support PowerShell.</span></span> <span data-ttu-id="2cb56-143">Därefter går vi igenom ytterligare kommandon som du kan lägga till, inklusive Azure-modulen.</span><span class="sxs-lookup"><span data-stu-id="2cb56-143">Next, we will talk about additional commands you can add including the Azure module.</span></span>