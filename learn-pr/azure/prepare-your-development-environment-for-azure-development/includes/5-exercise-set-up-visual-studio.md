<span data-ttu-id="1150a-101">I den här övningen ska du installera Visual Studio på antingen en Windows- eller Mac OS-dator.</span><span class="sxs-lookup"><span data-stu-id="1150a-101">In this exercise, you will install Visual Studio on either your Windows or your macOS computer.</span></span> <span data-ttu-id="1150a-102">På Windows måste arbetsbelastningen Azure Development installeras.</span><span class="sxs-lookup"><span data-stu-id="1150a-102">On Windows, the Azure development workload will need to be installed.</span></span> <span data-ttu-id="1150a-103">Och i Visual Studio för Mac gör det inbyggda arbetsflödet Connected Services att du kan skapa appar för Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="1150a-103">And on Visual Studio for Mac, the built-in Connected Services workflow will enable you to build apps for Azure App Service.</span></span> <span data-ttu-id="1150a-104">I slutändan kommer du att vara redo att börja skapa program och publicera dem på Azure.</span><span class="sxs-lookup"><span data-stu-id="1150a-104">At the end, you will be ready to start creating applications and publishing them to Azure.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="1150a-105">Övningssteg</span><span class="sxs-lookup"><span data-stu-id="1150a-105">Exercise steps</span></span>

<span data-ttu-id="1150a-106">Det finns några skillnader mellan att installera Visual Studio på Windows och macOS.</span><span class="sxs-lookup"><span data-stu-id="1150a-106">There are slight differences in installing Visual Studio between Windows and macOS.</span></span> <span data-ttu-id="1150a-107">Dessa skillnader beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1150a-107">The following sections outline these differences.</span></span>

### <a name="windows"></a><span data-ttu-id="1150a-108">Windows</span><span class="sxs-lookup"><span data-stu-id="1150a-108">Windows</span></span>

1. <span data-ttu-id="1150a-109">Hämta installationsprogrammet för Visual Studio från https://visualstudio.microsoft.com/downloads/.</span><span class="sxs-lookup"><span data-stu-id="1150a-109">Download the Visual Studio installer from https://visualstudio.microsoft.com/downloads/.</span></span>

1. <span data-ttu-id="1150a-110">Kör installationsprogrammet så öppnas fönstret för arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="1150a-110">Run the installer and it will open the Workloads window.</span></span>

1. <span data-ttu-id="1150a-111">Välj arbetsbelastningen **Azure Development**.</span><span class="sxs-lookup"><span data-stu-id="1150a-111">Choose the **Azure development** workload.</span></span>

    <span data-ttu-id="1150a-112">Följande skärmbild visar den arbetsbelastning för installationsprogrammet för Visual Studio som har valts för Azure-utveckling i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1150a-112">The following screenshot shows the Visual Studio Installer workload selected to allow Azure development within Visual Studio.</span></span>

    ![Skärmbild av installationsprogrammet för Visual Studio med arbetsbelastningen Azure Development markerad.](../media/5-select-azure-workload.png)

1. <span data-ttu-id="1150a-114">(Valfritt) Installera utvecklingsarbetsbelastningar för ASP.NET och webbutveckling för att kunna skapa webbprogram för Azure.</span><span class="sxs-lookup"><span data-stu-id="1150a-114">(Optional) Install the ASP.NET and web development workload to be ready to create web applications for Azure.</span></span>

1. <span data-ttu-id="1150a-115">Klicka på **Installera** och vänta tills Visual Studio är installerat.</span><span class="sxs-lookup"><span data-stu-id="1150a-115">Click **Install**, and wait for Visual Studio to install.</span></span>

1. <span data-ttu-id="1150a-116">Öppna Visual Studio när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="1150a-116">When the installation is complete, open Visual Studio.</span></span>

1. <span data-ttu-id="1150a-117">Gå till menyn Visa i Visual Studio och kontrollera att du har alternativet **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1150a-117">Go to the View menu in Visual Studio and make sure you have the **Cloud Explorer** option.</span></span>

    <span data-ttu-id="1150a-118">Följande skärmbild visar menyalternativet Cloud Explorer som är tillgängligt om du har installerat arbetsbelastningen Azure Development.</span><span class="sxs-lookup"><span data-stu-id="1150a-118">The following screenshot shows the Cloud Explorer menu option that will be present if you have the Azure development workload installed.</span></span>

    ![Skärmbild av menyn Visa i Visual Studio med menyalternativet Cloud Explorer markerat.](../media/5-verify-cloud-explorer.png)

### <a name="macos"></a><span data-ttu-id="1150a-120">macOS</span><span class="sxs-lookup"><span data-stu-id="1150a-120">macOS</span></span>

1. <span data-ttu-id="1150a-121">Gå till https://visualstudio.microsoft.com/ och hämta installationsprogrammet för att installera Visual Studio för Mac.</span><span class="sxs-lookup"><span data-stu-id="1150a-121">Go to https://visualstudio.microsoft.com/ and download the Visual Studio for Mac installer.</span></span>

1. <span data-ttu-id="1150a-122">Klicka på VisualStudioInstaller.dmg-filen för att montera installationsprogrammet och kör det sedan genom att dubbelklicka på logotypen.</span><span class="sxs-lookup"><span data-stu-id="1150a-122">Click the VisualStudioInstaller.dmg file to mount the installer and then run it by double-clicking the logo.</span></span>

1. <span data-ttu-id="1150a-123">Bekräfta sekretess- och licensvillkoren när de visas.</span><span class="sxs-lookup"><span data-stu-id="1150a-123">Acknowledge the Privacy and License terms when presented.</span></span>

1. <span data-ttu-id="1150a-124">Installationsprogrammet kommer fråga vilka komponenter du vill installera.</span><span class="sxs-lookup"><span data-stu-id="1150a-124">The installer will ask which components you wish to install.</span></span> <span data-ttu-id="1150a-125">Azure-komponenter ingår redan i Visual Studio för Mac, men du rekommenderas att installera **.NET Core**-plattformen för att utveckla webbfunktioner för Azure.</span><span class="sxs-lookup"><span data-stu-id="1150a-125">Azure components are already part of Visual Studio for Mac, but it is recommended to install the **.NET Core** platform to develop web experiences for Azure.</span></span>

    <span data-ttu-id="1150a-126">Följande skärmbild visar den .NET Core-plattform som krävs för att lägga till funktioner för Azure-utveckling i Visual Studio för Mac.</span><span class="sxs-lookup"><span data-stu-id="1150a-126">The following screenshot shows the .NET Core platform required to add Azure development capabilities to Visual Studio for Mac.</span></span>

    ![Skärmbild av installationsprogrammet för Visual Studio för Mac med det valda plattformsalternativet .NET Core markerat.](../media/5-vsmac-install-net-core.png)

1. <span data-ttu-id="1150a-128">Klicka på **Installera och uppdatera** när du är nöjd med dina val och vänta tills installationsprogrammet är klart.</span><span class="sxs-lookup"><span data-stu-id="1150a-128">Click **Install and Update** once you are happy with the selections, and wait for the installer to complete.</span></span>

1. <span data-ttu-id="1150a-129">Om du uppmanas höja behörighetsnivån kan du använda dina administratörsautentiseringsuppgifter för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="1150a-129">If you are prompted to elevate the permissions needed, use your administrator credentials to do so.</span></span>

1. <span data-ttu-id="1150a-130">När installationsprogrammet är klart kan du starta Visual Studio för Mac.</span><span class="sxs-lookup"><span data-stu-id="1150a-130">Once the installer is complete, start Visual Studio for Mac.</span></span>
