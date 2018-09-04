<span data-ttu-id="d9cd1-101">I den här övningen ska du installera Visual Studio på antingen en Windows- eller Mac OS-dator.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-101">In this exercise you will install Visual Studio on either you Windows or macOS computer.</span></span> <span data-ttu-id="d9cd1-102">På Windows måste arbetsbelastningen Azure Development installeras.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-102">On Windows, the Azure development workload will need to be installed.</span></span> <span data-ttu-id="d9cd1-103">Och i Visual Studio för Mac, gör det inbyggda arbetsflödet Connected Services att du kan skapa appar för Azure App Services.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-103">And on Visual Studio for Mac, the built in Connected Services workflow will enable you to build apps for Azure App Services.</span></span> <span data-ttu-id="d9cd1-104">I slutet kommer du vara redo att börja skapa program och publicera dem på Azure.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-104">At the end, you will be ready to start creating applications and publishing them to Azure.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="d9cd1-105">Övningssteg</span><span class="sxs-lookup"><span data-stu-id="d9cd1-105">Exercise Steps</span></span>

<span data-ttu-id="d9cd1-106">Det finns vissa skillnader i att installera Visual Studio mellan Windows och macOS, som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-106">There are slight differences in installing Visual Studio between Windows and macOS, which is outlined below.</span></span>

### <a name="windows"></a><span data-ttu-id="d9cd1-107">Windows</span><span class="sxs-lookup"><span data-stu-id="d9cd1-107">Windows</span></span>

1. <span data-ttu-id="d9cd1-108">Ladda ned installationsprogrammet för Visual Studio från https://visualstudio.microsoft.com/downloads/</span><span class="sxs-lookup"><span data-stu-id="d9cd1-108">Download the Visual Studio installer from https://visualstudio.microsoft.com/downloads/</span></span>
2. <span data-ttu-id="d9cd1-109">Kör installationsprogrammet så öppnas fönstret för arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-109">Run the installer and it will open the Workloads window.</span></span>
3. <span data-ttu-id="d9cd1-110">Välj arbetsbelastningen Azure Development.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-110">Choose the Azure development workload.</span></span>

    ![Välja arbetsbelastningen Azure Development i Visual Studio 2017-installationsprogrammet](../media-draft/5-select-azure-workload.png)

4. <span data-ttu-id="d9cd1-112">(Valfritt) Installera utvecklingsarbetsbelastningar för ASP.NET och webbutveckling för att kunna skapa webbprogram för Azure.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-112">(Optional) Install the ASP.NET and web development workload to be ready to create web applications for Azure.</span></span>
5. <span data-ttu-id="d9cd1-113">Klicka på **Installera** och vänta tills Visual Studio är installerat.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-113">Click **Install** and wait for Visual Studio to install.</span></span>
6. <span data-ttu-id="d9cd1-114">Öppna Visual Studio när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-114">When the installation is complete, open Visual Studio.</span></span>
7. <span data-ttu-id="d9cd1-115">Gå till menyn Visa i Visual Studio och kontrollera att du har alternativet Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-115">Go to the View menu in Visual Studio and make sure you have the Cloud Explorer option.</span></span>

    ![Verifiera att alternativet Cloud Explorer är tillgängligt i menyn Visa i Visual Studio 2017](../media-draft/5-verify-cloud-explorer.png)

## <a name="macos"></a><span data-ttu-id="d9cd1-117">macOS</span><span class="sxs-lookup"><span data-stu-id="d9cd1-117">macOS</span></span>

1. <span data-ttu-id="d9cd1-118">Gå till https://visualstudio.microsoft.com/ och hämta installationsprogrammet för att installera Visual Studio för Mac.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-118">Go to https://visualstudio.microsoft.com/ and download the Visual Studio for Mac installer.</span></span>
2. <span data-ttu-id="d9cd1-119">Klicka på VisualStudioInstaller.dmg-filen för att montera installationsprogrammet och kör det sedan genom att dubbelklicka på logotypen.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-119">Click the VisualStudioInstaller.dmg file to mount the installer and then run it by double-clicking the logo.</span></span>
3. <span data-ttu-id="d9cd1-120">Bekräfta sekretess- och licensvillkoren när de visas.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-120">Acknowledge the Privacy and License terms when presented.</span></span>
4. <span data-ttu-id="d9cd1-121">Installationsprogrammet kommer fråga vilka komponenter du vill installera.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-121">The installer will ask which components you wish to install.</span></span> <span data-ttu-id="d9cd1-122">Azure-komponenter ingår redan i Visual Studio för Mac, men du rekommenderas att installera .NET Core-plattformen för att utveckla webbfunktioner för Azure.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-122">Azure components are already part of Visual Studio for Mac, but it is recommended to install the .NET Core platform to develop web experiences for Azure.</span></span>

    ![Verifiera att .NET Core-alternativet väljs i Visual Studio för Mac](../media-draft/5-vsmac-install-net-core.png)

5. <span data-ttu-id="d9cd1-124">Klicka på **Installera och uppdatera** när du är nöjd med dina val och vänta tills installationsprogrammet är klart.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-124">Click **Install and Update** once you are happy with the selections and wait for the installer to complete.</span></span>
6. <span data-ttu-id="d9cd1-125">Om du uppmanas höja behörighetsnivån kan du använda dina administratörsautentiseringsuppgifter för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-125">If you are prompted to elevate the permissions needed, use your administrator credentials to do so.</span></span>
7. <span data-ttu-id="d9cd1-126">När installationsprogrammet är klart kan du starta Visual Studio för Mac.</span><span class="sxs-lookup"><span data-stu-id="d9cd1-126">Once the installer is complete, start Visual Studio for Mac.</span></span>
