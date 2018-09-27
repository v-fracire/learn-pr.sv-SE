<span data-ttu-id="df1ca-101">Om du vill använda Visual Studio Code för Azure-utveckling måste du installera Visual Studio Code lokalt och ett eller flera Azure-tillägg.</span><span class="sxs-lookup"><span data-stu-id="df1ca-101">To use Visual Studio Code for Azure development, you'll need to install Visual Studio Code locally and one or more Azure extensions.</span></span> <span data-ttu-id="df1ca-102">I den här övningen lägger vi till tillägget **Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="df1ca-102">In this exercise, we'll add the **Azure App Service** extension.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="df1ca-103">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="df1ca-103">Install Visual Studio Code</span></span>

<span data-ttu-id="df1ca-104">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="df1ca-104">::: zone pivot="windows"</span></span>

### <a name="windows"></a><span data-ttu-id="df1ca-105">Windows</span><span class="sxs-lookup"><span data-stu-id="df1ca-105">Windows</span></span>

1. <span data-ttu-id="df1ca-106">[Ladda ned Visual Studio Code-installationsprogrammet för Windows](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="df1ca-106">[Download the Visual Studio Code installer for Windows](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="df1ca-107">Kör installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="df1ca-107">Run the installer.</span></span>

1. <span data-ttu-id="df1ca-108">Öppna Visual Studio Code genom att trycka på Windows-tangenten eller genom att klicka på Windows-ikonen i aktivitetsfältet. Skriv ”Visual Studio Code” och klicka på resultatet **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="df1ca-108">Open Visual Studio Code by pressing the Windows key or clicking the Windows icon on the task bar, typing "Visual Studio Code" and clicking on the **Visual Studio Code** result.</span></span>

<span data-ttu-id="df1ca-109">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="df1ca-109">::: zone-end</span></span>

<span data-ttu-id="df1ca-110">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="df1ca-110">::: zone pivot="macos"</span></span>

### <a name="macos"></a><span data-ttu-id="df1ca-111">macOS</span><span class="sxs-lookup"><span data-stu-id="df1ca-111">macOS</span></span>

1. <span data-ttu-id="df1ca-112">[Ladda ned Visual Studio Code för macOS](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="df1ca-112">[Download Visual Studio Code for macOS](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="df1ca-113">Dubbelklicka på det nedladdade arkivet för att visa innehållet.</span><span class="sxs-lookup"><span data-stu-id="df1ca-113">Double-click on the downloaded archive to expand the contents.</span></span>

1. <span data-ttu-id="df1ca-114">Dra Visual Studio-Code.app till mappen Program.</span><span class="sxs-lookup"><span data-stu-id="df1ca-114">Drag Visual Studio Code.app to the Applications folder.</span></span>

1. <span data-ttu-id="df1ca-115">Öppna Visual Studio Code genom att klicka på ikonen för App-avsnittet eller genom att söka efter Visual Studio Code i Spotlight.</span><span class="sxs-lookup"><span data-stu-id="df1ca-115">Open Visual Studio Code by clicking on the icon the Apps section or by searching for Visual Studio Code in Spotlight.</span></span>

<span data-ttu-id="df1ca-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="df1ca-116">::: zone-end</span></span>

<span data-ttu-id="df1ca-117">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="df1ca-117">::: zone pivot="linux"</span></span>

### <a name="linux"></a><span data-ttu-id="df1ca-118">Linux</span><span class="sxs-lookup"><span data-stu-id="df1ca-118">Linux</span></span> 

#### <a name="debian-and-ubuntu"></a><span data-ttu-id="df1ca-119">Debian och Ubuntu</span><span class="sxs-lookup"><span data-stu-id="df1ca-119">Debian and Ubuntu</span></span>

1. <span data-ttu-id="df1ca-120">Ladda ned och installera [.deb paketet (64-bitars)](https://go.microsoft.com/fwlink/?LinkID=760868) via det grafiska programvarucentret, eller via kommandoraden (om tillgängligt) (ersätt `<file>` med det .deb-filnamn som du laddade ned):</span><span class="sxs-lookup"><span data-stu-id="df1ca-120">Download and install the [.deb package (64-bit)](https://go.microsoft.com/fwlink/?LinkID=760868) through the graphical software center, if it's available, or through the command line (replacing `<file>` with the .deb filename you downloaded):</span></span>

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

#### <a name="rhel-fedora-and-centos"></a><span data-ttu-id="df1ca-121">RHEL, Fedora och CentOS</span><span class="sxs-lookup"><span data-stu-id="df1ca-121">RHEL, Fedora, and CentOS</span></span>

1. <span data-ttu-id="df1ca-122">Använd följande skript för att installera nyckeln och databasen:</span><span class="sxs-lookup"><span data-stu-id="df1ca-122">Use the following script to install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    ```

1. <span data-ttu-id="df1ca-123">Uppdatera paketcachen och installera paketet med dnf (Fedora 22 och senare):</span><span class="sxs-lookup"><span data-stu-id="df1ca-123">Update the package cache, and install the package by using dnf (Fedora 22 and above):</span></span>

    ```bash
    dnf check-update
    sudo dnf install code
    ```

#### <a name="opensuse-and-sle"></a><span data-ttu-id="df1ca-124">openSUSE och SLE</span><span class="sxs-lookup"><span data-stu-id="df1ca-124">openSUSE and SLE</span></span>

1. <span data-ttu-id="df1ca-125">Yum-lagringsplatsen fungerar även för openSUSE- och SLE-baserade system.</span><span class="sxs-lookup"><span data-stu-id="df1ca-125">The yum repository also works for openSUSE and SLE based systems.</span></span> <span data-ttu-id="df1ca-126">Följande skript installerar nyckeln och databasen:</span><span class="sxs-lookup"><span data-stu-id="df1ca-126">The following script will install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
    ```

1. <span data-ttu-id="df1ca-127">Uppdatera paketcachen och installera paketet med:</span><span class="sxs-lookup"><span data-stu-id="df1ca-127">Update the package cache and install the package by using:</span></span>

    ```bash
    sudo zypper refresh
    sudo zypper install code
    ```

> [!NOTE]
> <span data-ttu-id="df1ca-128">Mer information om hur man installerar eller uppdaterar Visual Studio Code på olika Linux-distributioner finns under avsnittet om hur man [kör Visual Studio Code i Linux-dokumentation](https://code.visualstudio.com/docs/setup/linux).</span><span class="sxs-lookup"><span data-stu-id="df1ca-128">For further details about installing or updating Visual Studio Code on various Linux distributions, please see the [Running Visual Studio Code on Linux documentation](https://code.visualstudio.com/docs/setup/linux).</span></span>

<span data-ttu-id="df1ca-129">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="df1ca-129">::: zone-end</span></span>

## <a name="install-azure-app-service-extension"></a><span data-ttu-id="df1ca-130">Installera tillägget för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="df1ca-130">Install Azure App Service extension</span></span>

1. <span data-ttu-id="df1ca-131">Öppna Visual Studio Code om du inte redan har gjort det.</span><span class="sxs-lookup"><span data-stu-id="df1ca-131">If you haven't already, open Visual Studio Code.</span></span>

1. <span data-ttu-id="df1ca-132">Öppna webbläsartillägget som går att hitta via menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="df1ca-132">Open the Extensions browser; it's accessed via the menu on the left.</span></span>

1. <span data-ttu-id="df1ca-133">Sök efter **Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="df1ca-133">Search for **Azure App Service**.</span></span>

1. <span data-ttu-id="df1ca-134">Välj resultatet **Azure App Service** och klicka på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="df1ca-134">Select the **Azure App Service** result and click **Install**.</span></span>

    <span data-ttu-id="df1ca-135">Följande skärmbild visar Azure App Service-tillägget som valts i sökresultaten för Visual Studio Code-tillägget.</span><span class="sxs-lookup"><span data-stu-id="df1ca-135">The following screenshot shows the Azure App Service extension selected from the Visual Studio Code extension search results.</span></span>

    ![Skärmbild av Visual Studio Code som visar fliken Tillägg med Azure App Service-tillägget markerat i sökresultaten.](../media/3-install-azure-extension.png)

<span data-ttu-id="df1ca-137">Detta installerar tillägget.</span><span class="sxs-lookup"><span data-stu-id="df1ca-137">This will install the extension.</span></span> <span data-ttu-id="df1ca-138">Nu är du redo att ansluta till din Azure-prenumeration och distribuera en webb-, mobil- eller API-app till en Azure App Service-tjänst.</span><span class="sxs-lookup"><span data-stu-id="df1ca-138">You're now ready to connect to your Azure subscription and deploy a web, mobile, or API app to an Azure App Service.</span></span>
