<span data-ttu-id="a68c1-101">Nu när du har kört igång appen på din lokala dator är det dags att publicera den till Azure.</span><span class="sxs-lookup"><span data-stu-id="a68c1-101">Now that you've got your app up and running on your local machine, it's time to get it published to Azure.</span></span> 

<span data-ttu-id="a68c1-102">I den här delen skapar, bygger och kör du ett nytt ASP.NET-webbprogram på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="a68c1-102">In this unit, you will create, build, and run a new ASP.NET web application on your local machine.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="a68c1-103">Skapa ett nytt projekt</span><span class="sxs-lookup"><span data-stu-id="a68c1-103">Create a new project</span></span>

### <a name="visual-studio-for-windows"></a><span data-ttu-id="a68c1-104">Visual Studio för Windows</span><span class="sxs-lookup"><span data-stu-id="a68c1-104">Visual Studio for Windows</span></span>

<span data-ttu-id="a68c1-105">Det första steget är att starta Visual Studio och skapa ett lokalt ASP.NET Core-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a68c1-105">The first step is to start Visual Studio and create a local ASP.NET Core web application.</span></span>

1. <span data-ttu-id="a68c1-106">På startsidan för Visual Studio väljer du **Arkiv** och klickar sedan på **Nytt** följt av **Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="a68c1-106">On the Visual Studio start page, select **File**, then click **New**, and then click **Project..**.</span></span>

1. <span data-ttu-id="a68c1-107">I dialogrutan **Nytt projekt** går du till fönsterrutan till vänster och väljer **Webb**.</span><span class="sxs-lookup"><span data-stu-id="a68c1-107">In the **New Project** dialog box, on the left-hand pane, select **Web**.</span></span>

1. <span data-ttu-id="a68c1-108">I den mittersta fönsterrutan klickar du på **ASP.NET Core-webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="a68c1-108">In the center pane, click **ASP.NET Core Web Application**.</span></span>

1. <span data-ttu-id="a68c1-109">Längst ned i dialogrutan går du till fältet **Namn** och anger **Alpine Ski House**.</span><span class="sxs-lookup"><span data-stu-id="a68c1-109">At the bottom of the dialog box, in the **Name** field, enter **Alpine Ski House**.</span></span>

1. <span data-ttu-id="a68c1-110">Välj en **plats** för den nya lösningen.</span><span class="sxs-lookup"><span data-stu-id="a68c1-110">Select a **Location** for your new solution.</span></span>

1. <span data-ttu-id="a68c1-111">Klicka på knappen **OK** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="a68c1-111">Click the **OK** button to create your project.</span></span>

1. <span data-ttu-id="a68c1-112">I dialogrutan **Nytt ASP.NET Core-webbprogram** visas ett urval av startmallar.</span><span class="sxs-lookup"><span data-stu-id="a68c1-112">In the **New ASP.NET Core Web Application** dialog box, you will see a selection of starting templates.</span></span> <span data-ttu-id="a68c1-113">För den här övningen väljer du **Webbprogram** och klickar sedan på **OK** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="a68c1-113">For this exercise, select **Web Application**, and then click **OK** to create your project.</span></span>

    ![Dialogrutan Nytt projekt](../media-draft/3-aspnet-templates.png)

    > [!NOTE]
    > <span data-ttu-id="a68c1-115">Du kan även välja olika startmallar i den här dialogrutan beroende på dina behov för utveckling av webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a68c1-115">You can also select different starting templates in this dialog box depending on your web development requirements.</span></span> <span data-ttu-id="a68c1-116">Längst upp i dialogrutan kan du även välja version av ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a68c1-116">At the top of the dialog box, you are also able to select the version of ASP.NET Core.</span></span> <span data-ttu-id="a68c1-117">Du bör välja ASP.NET Core 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a68c1-117">You should select ASP.NET Core 2.0 or later.</span></span>

1. <span data-ttu-id="a68c1-118">Du bör nu ha din nya lösning för ASP.NET Core-program.</span><span class="sxs-lookup"><span data-stu-id="a68c1-118">You should now have your new ASP.NET Core web application solution.</span></span>

    ![Dialogrutan Nytt projekt](../media-draft/3-new-solution.png)

### <a name="visual-studio-mac"></a><span data-ttu-id="a68c1-120">Visual Studio Mac</span><span class="sxs-lookup"><span data-stu-id="a68c1-120">Visual Studio Mac</span></span>

1. <span data-ttu-id="a68c1-121">På startsidan för Visual Studio väljer du **Arkiv** och klickar sedan på **Nytt** följt av **Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="a68c1-121">On the Visual Studio start page, select **File**, then click **New**, and then click **Project..**.</span></span>

1. <span data-ttu-id="a68c1-122">Under. **NET Core** väljer du en **ASP.NET Core-webbapp** och klickar sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a68c1-122">Under .**NET Core**, select an **ASP.NET Core Web App**, and then click **Next**.</span></span>

1. <span data-ttu-id="a68c1-123">Som **projektnamn** skriver du **AlpineSkiHouse**.</span><span class="sxs-lookup"><span data-stu-id="a68c1-123">For the **Project Name**, type **AlpineSkiHouse**.</span></span> <span data-ttu-id="a68c1-124">Det bör även fylla i lösningens namn automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a68c1-124">This should also autopopulate the solution name.</span></span>

1. <span data-ttu-id="a68c1-125">Välj en **plats** på den lokala datorn för projektet.</span><span class="sxs-lookup"><span data-stu-id="a68c1-125">Select a **location** on your local machine for the project.</span></span>

1. <span data-ttu-id="a68c1-126">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a68c1-126">Click **Create**.</span></span>

## <a name="build-and-test-on-your-local-machine"></a><span data-ttu-id="a68c1-127">Bygga och testa på din lokala dator</span><span class="sxs-lookup"><span data-stu-id="a68c1-127">Build and test on your local machine</span></span>

<span data-ttu-id="a68c1-128">Nu ska vi bygga och testa det nya projektet på din lokala dator för att se till att det byggs och distribueras lokalt innan du distribuerar till Azure.</span><span class="sxs-lookup"><span data-stu-id="a68c1-128">Now, let's build and test your new project on your local machine to ensure it builds and deploys locally before deploying to Azure.</span></span>

1. <span data-ttu-id="a68c1-129">Tryck på **F5** för att köra appen i felsökningsläge eller på **Ctrl-F5** för att köra utan att koppla felsökaren.</span><span class="sxs-lookup"><span data-stu-id="a68c1-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span>

    ![Dialogrutan Nytt projekt](../media-draft/3-webapp-launch.png)

<span data-ttu-id="a68c1-131">Visual Studio startar IIS Express och kör appen.</span><span class="sxs-lookup"><span data-stu-id="a68c1-131">Visual Studio starts IIS Express and runs the app.</span></span> <span data-ttu-id="a68c1-132">När Visual Studio skapar ett webbprojekt används en slumpmässig port för webbservern.</span><span class="sxs-lookup"><span data-stu-id="a68c1-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a68c1-133">I den föregående bilden är portnumret 44381.</span><span class="sxs-lookup"><span data-stu-id="a68c1-133">In the preceding image, the port number is 44381.</span></span> <span data-ttu-id="a68c1-134">När du kör appen visas sannolikt ett annat portnummer.</span><span class="sxs-lookup"><span data-stu-id="a68c1-134">When you run the app, you'll likely see a different port number.</span></span>

> [!TIP]
> <span data-ttu-id="a68c1-135">Om du startar appen med **Ctrl+F5** (icke-felsökningsläge) kan du göra kodändringar, spara filen, uppdatera webbläsaren och se kodändringarna.</span><span class="sxs-lookup"><span data-stu-id="a68c1-135">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a68c1-136">Många utvecklare föredrar att använda icke-felsökningsläge för att snabbt starta appen och visa ändringar.</span><span class="sxs-lookup"><span data-stu-id="a68c1-136">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a68c1-137">Du lägger kanske märker till avsnittet längst upp på webbplatsen som är till för din policy om sekretess och användning av cookies.</span><span class="sxs-lookup"><span data-stu-id="a68c1-137">You might notice the section at the top of the web page that provides a place for your privacy and cookie use policy.</span></span> <span data-ttu-id="a68c1-138">Välj **Godkänn** för att ge samtycke till spårning.</span><span class="sxs-lookup"><span data-stu-id="a68c1-138">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="a68c1-139">Den här appen spårar inte personlig information.</span><span class="sxs-lookup"><span data-stu-id="a68c1-139">This app doesn't track personal information.</span></span> <span data-ttu-id="a68c1-140">Koden som genereras av mallen innehåller resurser som hjälper till att uppfylla dataskyddsförordningen (GDPR).</span><span class="sxs-lookup"><span data-stu-id="a68c1-140">The template-generated code includes assets to help meet General Data Protection Regulation (GDPR).</span></span>

## <a name="summary"></a><span data-ttu-id="a68c1-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a68c1-141">Summary</span></span>

<span data-ttu-id="a68c1-142">Det första steget för att köra igång ASP.NET-webbplats är att skapa och köra den lokalt.</span><span class="sxs-lookup"><span data-stu-id="a68c1-142">The first step to getting your ASP.NET site up and running is to create it and run it locally.</span></span> <span data-ttu-id="a68c1-143">Nu när webbplatsen har skapats är du redo att distribuera den till Azure.</span><span class="sxs-lookup"><span data-stu-id="a68c1-143">Now that your site is created, you are ready to deploy it to Azure.</span></span>
