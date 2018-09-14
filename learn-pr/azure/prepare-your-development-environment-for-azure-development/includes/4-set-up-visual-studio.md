<span data-ttu-id="6e228-101">Visual Studio är en komplett och omfattande IDE-miljö (Integrated Development Environment) för alla slags programvaruexperter.</span><span class="sxs-lookup"><span data-stu-id="6e228-101">Visual Studio is a fully featured and rich integrated development environment (IDE), aimed at almost any kind of software professional.</span></span> <span data-ttu-id="6e228-102">Visual Studio har en fullständig uppsättning verktyg och funktioner som är specifikt utformade för apputveckling med Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6e228-102">Visual Studio has a full set of tools and features specifically aimed at developing applications with Microsoft Azure.</span></span> <span data-ttu-id="6e228-103">Integreringen i Visual Studio garanterar att distributions-, felsöknings- och utvecklingsverktygen för Azure är av högsta klass.</span><span class="sxs-lookup"><span data-stu-id="6e228-103">The tight integration in Visual Studio means that Azure deployment, debugging, and development tools are first class.</span></span> <span data-ttu-id="6e228-104">Och detsamma gäller Visual Studio för Mac.</span><span class="sxs-lookup"><span data-stu-id="6e228-104">Visual Studio for Mac is no different.</span></span> <span data-ttu-id="6e228-105">I den här delen tittar vi närmare på båda versionerna och hur de hjälper dig att utveckla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="6e228-105">This unit will introduce you to both, and their strengths when it comes to Azure development.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="6e228-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e228-106">Visual Studio</span></span>

<span data-ttu-id="6e228-107">Visual Studio är en komplett IDE-miljö (Integrated Development Environment) som hjälper dig att utveckla program för Windows, Android, iOS, webben och Azure.</span><span class="sxs-lookup"><span data-stu-id="6e228-107">Visual Studio is a fully featured IDE used to develop applications for Windows, Android, iOS, the web, and Azure.</span></span>

<span data-ttu-id="6e228-108">När du installerar Visual Studio kan du välja mellan flera arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="6e228-108">When installing Visual Studio, you'll see that several workloads are available.</span></span> <span data-ttu-id="6e228-109">Arbetsbelastningar är samlingar med bibliotek och komponenter som definierar ett funktionsområde som kan installeras.</span><span class="sxs-lookup"><span data-stu-id="6e228-109">Workloads are collections of libraries and components that define an area of functionality that can be installed.</span></span> <span data-ttu-id="6e228-110">I stället för att installera enskilda komponenter separat, vilket kräver att du känner till och kommer ihåg alla beroenden mellan komponenterna, kan du använda arbetsbelastningar för att utföra ”områdesspecifika” installationer.</span><span class="sxs-lookup"><span data-stu-id="6e228-110">Instead of installing a single individual component, where you must know and remember the dependencies between each, you can use workloads to do "themed" installations.</span></span> <span data-ttu-id="6e228-111">Detta säkerställer att alla nödvändiga komponenter ingår.</span><span class="sxs-lookup"><span data-stu-id="6e228-111">This ensures that all necessary components are included.</span></span>

<span data-ttu-id="6e228-112">Basinstallationen av Visual Studio innehåller inga verktyg eller bibliotek för Azure-utveckling.</span><span class="sxs-lookup"><span data-stu-id="6e228-112">The base installation of Visual Studio comes with no tools or libraries for Azure development.</span></span> <span data-ttu-id="6e228-113">För detta måste du lägga till arbetsbelastningen för Azure-utveckling.</span><span class="sxs-lookup"><span data-stu-id="6e228-113">For that, you need to include the Azure development workload.</span></span> <span data-ttu-id="6e228-114">Den innehåller Azure SDK:er, verktyg och mallprojekt som hjälper dig att komma igång med att skapa program och upplevelser i Azure.</span><span class="sxs-lookup"><span data-stu-id="6e228-114">This includes the Azure SDKs, tooling, and template projects for getting started creating applications and experiences on Azure.</span></span>

<span data-ttu-id="6e228-115">Hämta installationsprogrammet för att installera Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e228-115">To install Visual Studio, download the installer.</span></span> <span data-ttu-id="6e228-116">Installationsprogrammet uppmanar dig att ange vilka arbetsbelastningar som ska installeras. Välj arbetsbelastningen för Azure-utveckling.</span><span class="sxs-lookup"><span data-stu-id="6e228-116">This installer will ask which workloads to install, which is where you specify the Azure development workload.</span></span> <span data-ttu-id="6e228-117">Om ytterligare funktioner krävs är dessa troligen tillgängliga via NuGet eller ett Visual Studio-tillägg.</span><span class="sxs-lookup"><span data-stu-id="6e228-117">If there is additional functionality needed, this is most likely available through NuGet or a Visual Studio extension.</span></span>

## <a name="visual-studio-for-mac"></a><span data-ttu-id="6e228-118">Visual Studio för Mac</span><span class="sxs-lookup"><span data-stu-id="6e228-118">Visual Studio for Mac</span></span>

<span data-ttu-id="6e228-119">Visual Studio för Mac är en internt utformad och utvecklad IDE för macOS.</span><span class="sxs-lookup"><span data-stu-id="6e228-119">Visual Studio for Mac is a natively designed and developed IDE for macOS.</span></span> <span data-ttu-id="6e228-120">Det ger en förstklassig upplevelse där utvecklare kan skapa program för mobilappar för Android och iOS, webben och .NET Core-lösningar.</span><span class="sxs-lookup"><span data-stu-id="6e228-120">It provides a first class developer experience for creating applications for mobile apps on Android and iOS, the web, and .NET Core solutions.</span></span> <span data-ttu-id="6e228-121">Det är även perfekt för att skapa program i Azure.</span><span class="sxs-lookup"><span data-stu-id="6e228-121">It is also perfectly suited for creating applications on Azure.</span></span>

<span data-ttu-id="6e228-122">Basinstallationen av Visual Studio för Mac innehåller sammanhangsbaserad integration av Azure-verktyg.</span><span class="sxs-lookup"><span data-stu-id="6e228-122">The base installation of Visual Studio for Mac comes with contextual integration of Azure tooling.</span></span> <span data-ttu-id="6e228-123">Om du till exempel skapar en Xamarin-app för Android, så tillhandahåller arbetsbelastningen Connected Services en länk för att skapa en serverdel för mobilappar med Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6e228-123">For example, if you are building a Xamarin app for Android, then the Connected Services workload will provide a link to create a mobile back end with Azure App Service.</span></span> <span data-ttu-id="6e228-124">Om du vill skapa en Azure-funktion, så är det en projektmall under kategorin Moln.</span><span class="sxs-lookup"><span data-stu-id="6e228-124">If you want to create an Azure function, then that is a project template under the Cloud category.</span></span>

<span data-ttu-id="6e228-125">Om du behöver verktyg för Azure-funktioner som inte ingår i basinstallationen använder du NuGet.</span><span class="sxs-lookup"><span data-stu-id="6e228-125">If you require tools for Azure features and functions that aren't in the base installation, NuGet is the way to go.</span></span> <span data-ttu-id="6e228-126">NuGet-pakethanteraren har en mängd Azure-paket som utökar funktionerna och verktygen i Visual Studio för Mac.</span><span class="sxs-lookup"><span data-stu-id="6e228-126">The NuGet package manager has a ton of Azure packages that extend the functionality and tooling of Visual Studio for Mac.</span></span>

<span data-ttu-id="6e228-127">Hämta installationsprogrammet för att installera Visual Studio för Mac.</span><span class="sxs-lookup"><span data-stu-id="6e228-127">To install Visual Studio for Mac, download the installer.</span></span> <span data-ttu-id="6e228-128">Installationsprogrammet inspekterar ditt system för att avgöra vilka komponenter som behövs eller som måste uppdateras.</span><span class="sxs-lookup"><span data-stu-id="6e228-128">The installer will inspect your system to determine what components are needed or need to be updated.</span></span> <span data-ttu-id="6e228-129">Du kan anpassa komponenterna som ska installeras från de som saknas på datorn.</span><span class="sxs-lookup"><span data-stu-id="6e228-129">You can customize the components to install from those that are found to be missing from your system.</span></span> <span data-ttu-id="6e228-130">Basinstallationen innehåller Azure-verktyg.</span><span class="sxs-lookup"><span data-stu-id="6e228-130">The base installation will include Azure tooling.</span></span> <span data-ttu-id="6e228-131">Om ytterligare funktioner krävs är de troligen tillgängliga via NuGet eller ett Visual Studio för Mac-tillägg.</span><span class="sxs-lookup"><span data-stu-id="6e228-131">If additional functionality is needed, it will likely be available through NuGet or a Visual Studio for Mac extension.</span></span>

> [!NOTE]
> <span data-ttu-id="6e228-132">Du kan uppmanas att ange administratörsautentiseringsuppgifter på datorn för att installera vissa komponenter.</span><span class="sxs-lookup"><span data-stu-id="6e228-132">You may be prompted for administrator credentials on your machine to install certain components.</span></span>

## <a name="summary"></a><span data-ttu-id="6e228-133">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6e228-133">Summary</span></span>

<span data-ttu-id="6e228-134">I den här delen har du installerat Visual Studio för Windows eller på macOS.</span><span class="sxs-lookup"><span data-stu-id="6e228-134">In this unit, you have installed Visual Studio either on Windows or on macOS.</span></span>

<span data-ttu-id="6e228-135">För Windows valde du arbetsbelastningen för Azure-utveckling i installationsprogrammet, som installerade alla nödvändiga verktyg som krävs för att skapa Azure-program och generera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="6e228-135">For Windows you chose the Azure development workload in the installer experience, which installed all the necessary tooling for building Azure applications and generating Azure resources.</span></span> <span data-ttu-id="6e228-136">Du kan sedan komma åt alla Azure-resurser för din prenumeration via utforskarverktyg eller via resursreferenser.</span><span class="sxs-lookup"><span data-stu-id="6e228-136">You can then access all the Azure resources for your subscription through Explorer tooling, or via resource referencing.</span></span>

<span data-ttu-id="6e228-137">I Visual Studio för Mac är vissa Azure-verktyg redan inbyggda i basinstallationen, och många fler funktioner är tillgängliga via NuGet.</span><span class="sxs-lookup"><span data-stu-id="6e228-137">Visual Studio for Mac comes with some Azure tooling built into the base installation, and many more features available through NuGet.</span></span> <span data-ttu-id="6e228-138">Detta ger även åtkomst till resurser och tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6e228-138">This will give access to resources and services on your Azure subscription as well.</span></span>