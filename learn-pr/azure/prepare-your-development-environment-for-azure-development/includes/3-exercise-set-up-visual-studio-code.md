<span data-ttu-id="e294b-101">I den här övningen kommer du installera Visual Studio Code och Azure App Service-tillägget. Det kommer göra dig redo att utveckla för Microsoft Azure och distribuera en webbapp.</span><span class="sxs-lookup"><span data-stu-id="e294b-101">In this exercise you will install Visual Studio Code and the Azure App Service extension, which will get you ready to develop for Microsoft Azure and deploying a web app.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="e294b-102">Övningssteg</span><span class="sxs-lookup"><span data-stu-id="e294b-102">Exercise steps</span></span>

<span data-ttu-id="e294b-103">Börja med att identifiera vilket operativsystem du använder. Följ sedan stegen i aktuellt avsnitt nedan för att installera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e294b-103">First identify which operating system you are using and follow the steps in the appropriate section below to install Visual Studio Code.</span></span>

### <a name="windows"></a><span data-ttu-id="e294b-104">Windows</span><span class="sxs-lookup"><span data-stu-id="e294b-104">Windows</span></span>

1. <span data-ttu-id="e294b-105">Ladda ned Visual Studio Code-installationsprogrammet för Windows.</span><span class="sxs-lookup"><span data-stu-id="e294b-105">Download the Visual Studio Code installer for Windows.</span></span>
2. <span data-ttu-id="e294b-106">Kör installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e294b-106">Run the installer.</span></span> <span data-ttu-id="e294b-107">Det går fort.</span><span class="sxs-lookup"><span data-stu-id="e294b-107">This won't take long.</span></span>
3. <span data-ttu-id="e294b-108">Öppna VS Code genom att antingen navigera till installationsmappen (som standard C:\Program Files\Microsoft VS-kod för en 64-bitars dator).</span><span class="sxs-lookup"><span data-stu-id="e294b-108">Open VS Code by either navigating to the installation folder (default is C:\Program Files\Microsoft VS Code for a 64-bit machine).</span></span>

### <a name="macos"></a><span data-ttu-id="e294b-109">macOS</span><span class="sxs-lookup"><span data-stu-id="e294b-109">macOS</span></span>

1. <span data-ttu-id="e294b-110">Ladda ned Visual Studio Code för macOS.</span><span class="sxs-lookup"><span data-stu-id="e294b-110">Download Visual Studio Code for macOS.</span></span>
2. <span data-ttu-id="e294b-111">Dubbelklicka på det nedladdade arkivet för att visa innehållet.</span><span class="sxs-lookup"><span data-stu-id="e294b-111">Double-click on the downloaded archive to expand the contents.</span></span>
3. <span data-ttu-id="e294b-112">Dra Visual Studio-Code.app till programmappen för att göra den tillgänglig i Launchpad.</span><span class="sxs-lookup"><span data-stu-id="e294b-112">Drag Visual Studio Code.app to the Applications folder, making it available in the Launchpad.</span></span>
4. <span data-ttu-id="e294b-113">Lägg till VS Code till Dock genom att högerklicka på ikonen och välja Options (Alternativ) -> Keep in Dock (Fäst i Dock).</span><span class="sxs-lookup"><span data-stu-id="e294b-113">Add VS Code to your Dock by right-clicking on the icon and choosing Options -> Keep in Dock.</span></span>

### <a name="linux--debian-and-ubuntu"></a><span data-ttu-id="e294b-114">Linux – Debian och Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e294b-114">Linux – Debian and Ubuntu</span></span>

1. <span data-ttu-id="e294b-115">Ladda ned och installera [.deb paketet (64-bitars)](https://go.microsoft.com/fwlink/?LinkID=760868), antingen via det grafiska programvarucentret, eller via kommandoraden (om tillgängligt) (ersätt `<file>` med det .deb-filnamn som du laddade ned):</span><span class="sxs-lookup"><span data-stu-id="e294b-115">Download and install the [.deb package (64-bit)](https://go.microsoft.com/fwlink/?LinkID=760868) either through the graphical software center, if it's available, or through the command line (replacing `<file>` with the .deb filename you downloaded):</span></span>

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

### <a name="linux--rhel-fedora-and-centos"></a><span data-ttu-id="e294b-116">Linux – RHEL, Fedora och CentOS</span><span class="sxs-lookup"><span data-stu-id="e294b-116">Linux – RHEL, Fedora, and CentOS</span></span>

1. <span data-ttu-id="e294b-117">Använd följande skript för att installera nyckeln och databasen:</span><span class="sxs-lookup"><span data-stu-id="e294b-117">Use the following script to install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    ```

1. <span data-ttu-id="e294b-118">Uppdatera paketcachen och installera paketet med dnf (Fedora 22 och senare):</span><span class="sxs-lookup"><span data-stu-id="e294b-118">Update the package cache and install the package using dnf (Fedora 22 and above):</span></span>

    ```bash
    dnf check-update
    sudo dnf install code
    ```

### <a name="linux--opensuse-and-sle"></a><span data-ttu-id="e294b-119">Linux – openSUSE och SLE</span><span class="sxs-lookup"><span data-stu-id="e294b-119">Linux – openSUSE and SLE</span></span>

1. <span data-ttu-id="e294b-120">Yum-databasen fungerar även för openSUSE- och SLE-baserade system. Följande skript installerar nyckeln och databasen:</span><span class="sxs-lookup"><span data-stu-id="e294b-120">The yum repository also works for openSUSE and SLE based systems, the following script will install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
    ```

1. <span data-ttu-id="e294b-121">Uppdatera paketcachen och installera paketet med:</span><span class="sxs-lookup"><span data-stu-id="e294b-121">Update the package cache and install the package using:</span></span>

    ```bash
    sudo zypper refresh
    sudo zypper install code
    ```

> [!NOTE]
> <span data-ttu-id="e294b-122">Mer information om hur du installerar eller uppdaterar VS Code på olika Linux-distributioner finns under avsnittet om hur du [kör VS Code i Linux-dokumentation](https://code.visualstudio.com/docs/setup/linux).</span><span class="sxs-lookup"><span data-stu-id="e294b-122">For further details about installing or updating VS Code on various Linux distributions, please see the [Running VS Code on Linux documentation](https://code.visualstudio.com/docs/setup/linux).</span></span>

## <a name="install-azure-app-service-extension"></a><span data-ttu-id="e294b-123">Installera tillägget för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e294b-123">Install Azure App Service extension</span></span>

<span data-ttu-id="e294b-124">Öppna VS Code när det har installerats.</span><span class="sxs-lookup"><span data-stu-id="e294b-124">Once you have installed VS Code, open it.</span></span>

1. <span data-ttu-id="e294b-125">Gå till fliken Tillägg.</span><span class="sxs-lookup"><span data-stu-id="e294b-125">Go to the Extensions tab.</span></span>
2. <span data-ttu-id="e294b-126">Leta upp Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e294b-126">Search for Azure App Service.</span></span>
3. <span data-ttu-id="e294b-127">Klicka på Installera.</span><span class="sxs-lookup"><span data-stu-id="e294b-127">Click Install.</span></span>

![Hitta Azure App Service-tillägget från Marketplace i Visual Studio Code](../media-draft/3-install-azure-extension.png)

<span data-ttu-id="e294b-129">Nu installeras tillägget och du kan ansluta till din Azure-prenumeration och utveckla och distribuera dina webb-, mobil- eller API-appar till en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e294b-129">This will install the extension and you will be ready to connect to your Azure subscription and develop for, and deploy your web, mobile or API app to an Azure App Service.</span></span>
