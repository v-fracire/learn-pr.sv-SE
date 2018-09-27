<span data-ttu-id="630fa-101">Det program du skapar är en plattformsoberoende mobilapp som kommunicerar med en Azure-funktion och delar din plats.</span><span class="sxs-lookup"><span data-stu-id="630fa-101">The application you're building is a cross-platform mobile app that talks to an Azure function to share your location.</span></span> <span data-ttu-id="630fa-102">I den här utbildningsenheten skapar du en tom mobilapp med Visual Studio och installera ett NuGet-paket med ett API för att hämta användarens plats.</span><span class="sxs-lookup"><span data-stu-id="630fa-102">In this unit, you create the blank mobile app using Visual Studio and install a NuGet package that has an API for getting the user's location.</span></span>

## <a name="create-the-xamarinforms-project"></a><span data-ttu-id="630fa-103">Skapa Xamarin.Forms-projektet</span><span class="sxs-lookup"><span data-stu-id="630fa-103">Create the Xamarin.Forms project</span></span>

1. <span data-ttu-id="630fa-104">Välj *Arkiv -> Nytt -> Projekt* i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="630fa-104">From Visual Studio, select *File->New->Project...*.</span></span>

1. <span data-ttu-id="630fa-105">Gå till det vänstra trädet och välj *Visual C# -> Plattformsoberoende*. Välj sedan *Mobilapp (Xamarin.Forms)* från panelen i mitten.</span><span class="sxs-lookup"><span data-stu-id="630fa-105">From the tree on the left-hand side, select *Visual C#->Cross-Platform* and then select *Mobile App (Xamarin.Forms)* from the panel in the center.</span></span>

1. <span data-ttu-id="630fa-106">Ge lösningen namnet ”ImHere”.</span><span class="sxs-lookup"><span data-stu-id="630fa-106">Name the solution "ImHere".</span></span>

1. <span data-ttu-id="630fa-107">Välj en lämplig plats för lösningen.</span><span class="sxs-lookup"><span data-stu-id="630fa-107">Choose an appropriate location for the solution.</span></span>

1. <span data-ttu-id="630fa-108">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="630fa-108">Click **OK**.</span></span>

    ![Dialogrutan Ny lösning](../media/2-new-solution-dialog.png)

1. <span data-ttu-id="630fa-110">I dialogrutan **Ny plattformsoberoende app** väljer du mallen *Tom app*.</span><span class="sxs-lookup"><span data-stu-id="630fa-110">From the **New Cross Platform App** dialog, select the *Blank App* template.</span></span>

1. <span data-ttu-id="630fa-111">I den här modulen skapar du en UWP-app, så avmarkera iOS och Android och lämna UWP markerat.</span><span class="sxs-lookup"><span data-stu-id="630fa-111">For this module you will build a UWP app, so uncheck iOS and Android and leave UWP checked.</span></span>

1. <span data-ttu-id="630fa-112">Som *Strategi för delning av kod* väljer du **.NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="630fa-112">For the *Code Sharing Strategy*, select **.NET Standard**.</span></span>

1. <span data-ttu-id="630fa-113">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="630fa-113">Click **OK**.</span></span>

    ![Dialogrutan Konfigurera ny lösning](../media/2-configure-solution-dialog.png)

<span data-ttu-id="630fa-115">I Visual Studio skapas två projekt åt dig – en UWP-app med namnet `ImHere.UWP` och .NET-standardbiblioteket `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="630fa-115">Visual Studio will create two projects for you - a UWP app called `ImHere.UWP` and a .NET Standard library, `ImHere`.</span></span> <span data-ttu-id="630fa-116">Xamarin.Forms-appar består av två delar – ett eller flera plattformsspecifika approjekt och ett (eller flera) .NET-standardbibliotek.</span><span class="sxs-lookup"><span data-stu-id="630fa-116">Xamarin.Forms apps are made up of two parts - one or more platform-specific app projects and one (or more) .NET Standard libraries.</span></span> <span data-ttu-id="630fa-117">De plattformsspecifika approjekten innehåller plattformsspecifik kod som behövs för att köra en app på motsvarande plattform.</span><span class="sxs-lookup"><span data-stu-id="630fa-117">The platform-specific app projects contain the platform-specific code needed to run an app on the relevant platform.</span></span> <span data-ttu-id="630fa-118">De här projekten startar sedan en Xamarin.Forms-app som definierats i ett plattformsoberoende .NET-standardbibliotek.</span><span class="sxs-lookup"><span data-stu-id="630fa-118">These projects then launch a Xamarin.Forms app that is defined in a cross-platform .NET Standard library.</span></span> <span data-ttu-id="630fa-119">Du skapar appen i plattformsoberoende kod, och när den körs översätts de användargränssnitt du skapar till relevanta plattformsspecifika UI-komponenter.</span><span class="sxs-lookup"><span data-stu-id="630fa-119">You build your app in cross-platform code and, at runtime, any user interfaces you create are translated into the relevant platform-specific UI components.</span></span>

## <a name="adding-xamarinessentials"></a><span data-ttu-id="630fa-120">Lägga till Xamarin.Essentials</span><span class="sxs-lookup"><span data-stu-id="630fa-120">Adding Xamarin.Essentials</span></span>

<span data-ttu-id="630fa-121">Plattformarna UWP, Android och iOS har ett stort antal liknande funktioner där operativsystem och maskinvara används.</span><span class="sxs-lookup"><span data-stu-id="630fa-121">The UWP, Android, and iOS platforms provide numerous similar capabilities that take advantage of the operating system and hardware.</span></span> <span data-ttu-id="630fa-122">Trots dessa likheter är API:erna mycket olika.</span><span class="sxs-lookup"><span data-stu-id="630fa-122">Despite these similarities, the APIs are very different.</span></span> <span data-ttu-id="630fa-123">Om du vill använda de här API:erna från plattformsoberoende kod måste du skriva plattformsspecifik kod i dina approjekt som du exponerar för dina .NET-standardbibliotek.</span><span class="sxs-lookup"><span data-stu-id="630fa-123">Using these APIs from cross-platform code requires writing platform-specific code in your app projects that you expose to your .NET Standard libraries.</span></span> <span data-ttu-id="630fa-124">[Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/?azure-portal=true) är ett NuGet-paket som innehåller en plattformsoberoende abstraktion över ett antal av dessa API:er så att du inte behöver skriva plattformsspecifik kod.</span><span class="sxs-lookup"><span data-stu-id="630fa-124">[Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/?azure-portal=true) is a NuGet package that provides a cross-platform abstraction over a number of these APIs so that you don't need to write platform-specific code.</span></span> <span data-ttu-id="630fa-125">Detta inkluderar geografisk plats-API:er som du använder i din app för att hämta användarens plats.</span><span class="sxs-lookup"><span data-stu-id="630fa-125">This includes the geolocation APIs that you will use in your app to get the user's location.</span></span>

1. <span data-ttu-id="630fa-126">Högerklicka på lösningen `ImHere` (den översta nivålösningen, inte `ImHere` .NET- standardprojektet) i lösningsutforskaren i Visual Studio och välj *Hantera NuGet-paket för Lösningen...*.</span><span class="sxs-lookup"><span data-stu-id="630fa-126">Right-click on the `ImHere` solution (the top level solution, not the `ImHere` .NET Standard project) in the Visual Studio Solution Explorer and select *Manage NuGet Packages for Solution...*.</span></span>

1. <span data-ttu-id="630fa-127">Välj fliken **Bläddra** och sök efter ”Xamarin.Essentials”.</span><span class="sxs-lookup"><span data-stu-id="630fa-127">Select the **Browse** tab and search for "Xamarin.Essentials".</span></span> <span data-ttu-id="630fa-128">Det här paketet är för närvarande tillgängligt som förhandsversion av NuGet-paketet, så se till att kryssrutan *Inkludera förhandsversioner* är markerad.</span><span class="sxs-lookup"><span data-stu-id="630fa-128">This package is currently available as a prerelease NuGet package, so check the *include prelease* box.</span></span>

    > [!TIP]
    > <span data-ttu-id="630fa-129">Kontrollera att *“inklude prelease”* är markerat om du inte ser Xamarin.Essentials NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="630fa-129">If you do not see the Xamarin.Essentials NuGet package, double check that *include prelease* is checked.</span></span> 

1. <span data-ttu-id="630fa-130">Välj NuGet-paketet **Xamarin.Essentials**.</span><span class="sxs-lookup"><span data-stu-id="630fa-130">Select the **Xamarin.Essentials** NuGet package.</span></span>

1. <span data-ttu-id="630fa-131">Du kan se alla dina projekt i projektlistan till höger.</span><span class="sxs-lookup"><span data-stu-id="630fa-131">Check all your projects in the project list on the right-hand side.</span></span>

1. <span data-ttu-id="630fa-132">Klicka på knappen **Installera** för att installera NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="630fa-132">Click the **Install** button to install the NuGet package.</span></span> <span data-ttu-id="630fa-133">Du måste godkänna licensavtalet för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="630fa-133">You'll need to accept the license to continue.</span></span>

    ![Lägga till NuGet-paketet Xamarin.Essentials i alla projekt i lösningen](../media/2-add-essentials-nuget.png)

## <a name="building-and-running-the-app"></a><span data-ttu-id="630fa-135">Skapa och köra appen</span><span class="sxs-lookup"><span data-stu-id="630fa-135">Building and running the app</span></span>

1. <span data-ttu-id="630fa-136">Högerklicka på projektet `ImHere.UWP` i Lösningsutforskaren och välj *Ange som startprojekt*.</span><span class="sxs-lookup"><span data-stu-id="630fa-136">Right-click on the `ImHere.UWP` project in Solution Explorer and select *Set as StartUp project*.</span></span>

1. <span data-ttu-id="630fa-137">Ange versionskonfigurationen som **Felsökning**, plattformen som **x86** och att enheten ska köras **Lokalt**.</span><span class="sxs-lookup"><span data-stu-id="630fa-137">Set the build configuration to **Debug**, the platform to **x86**, and the device to run on to **Local Machine**.</span></span>

    ![Ställ in x86-felsökningskonfigurationen att köras lokalt](../media/2-debug-configuration.png)

1. <span data-ttu-id="630fa-139">Börja felsöka appen.</span><span class="sxs-lookup"><span data-stu-id="630fa-139">Start debugging the app.</span></span>

    ![Appen körs](../media/2-debuging-app.png)

## <a name="summary"></a><span data-ttu-id="630fa-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="630fa-141">Summary</span></span>

<span data-ttu-id="630fa-142">I den här utbildningsenheten har du skapat en ny plattformsoberoende Xamarin.Forms-mobilapp och lagt till NuGet-paketet Xamarin.Essentials.</span><span class="sxs-lookup"><span data-stu-id="630fa-142">In this unit, you created a new Xamarin.Forms cross-platform mobile app and added the Xamarin.Essentials NuGet package.</span></span> <span data-ttu-id="630fa-143">Härnäst får du lära dig att skapa mobilappens gränssnitt och logik.</span><span class="sxs-lookup"><span data-stu-id="630fa-143">Next, you learn how to build up the mobile app UI and logic.</span></span>