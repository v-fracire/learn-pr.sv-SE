<span data-ttu-id="ea832-101">Nu har du ett ASP.NET Core-webbprogram som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="ea832-101">You now have a local running ASP.NET Core web application.</span></span> <span data-ttu-id="ea832-102">I den här övningen publicerar du programmet i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ea832-102">In this exercise, you'll publish your application to Azure App Service.</span></span>

### <a name="visual-studio-for-windows"></a><span data-ttu-id="ea832-103">Visual Studio för Windows</span><span class="sxs-lookup"><span data-stu-id="ea832-103">Visual Studio for Windows</span></span>

1. <span data-ttu-id="ea832-104">Öppna Solution Explorer, högerklicka på projektet och välj **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="ea832-104">In Solution Explorer, right-click your project and select **Publish**.</span></span>

1. <span data-ttu-id="ea832-105">I dialogrutan som visas väljer du **App Service** som publiceringsmål.</span><span class="sxs-lookup"><span data-stu-id="ea832-105">In the dialog box that appears, on the left, choose **App Service** as your publish target.</span></span>  <span data-ttu-id="ea832-106">Till höger väljer du **Skapa ny** för att skapa en ny App Service.</span><span class="sxs-lookup"><span data-stu-id="ea832-106">On the right, select **Create New** to create a new app service.</span></span>

1. <span data-ttu-id="ea832-107">Klicka på knappen **Publicera** för att skapa din nya App Service.</span><span class="sxs-lookup"><span data-stu-id="ea832-107">Click the **Publish** button to create your new app service.</span></span>

> [!NOTE]
> <span data-ttu-id="ea832-108">När du distribuerar apparna kan du skapa en ny App Service-plan, eller så kan du fortsätta att lägga till appar i en befintlig plan.</span><span class="sxs-lookup"><span data-stu-id="ea832-108">When you deploy your apps, you can create a new App Service plan or you can continue to add apps to an existing plan.</span></span> <span data-ttu-id="ea832-109">I den här övningen skapar du en ny **Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="ea832-109">For this exercise, you will create a new **Azure app service**.</span></span>

### <a name="visual-studio-mac"></a><span data-ttu-id="ea832-110">Visual Studio Mac</span><span class="sxs-lookup"><span data-stu-id="ea832-110">Visual Studio Mac</span></span>

1. <span data-ttu-id="ea832-111">Öppna Solution Explorer och högerklicka på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="ea832-111">In Solution Explorer, right-click your project.</span></span>

1. <span data-ttu-id="ea832-112">Välj **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="ea832-112">Select **Publish**.</span></span>

1. <span data-ttu-id="ea832-113">Klicka på **Publicera till Azure**.</span><span class="sxs-lookup"><span data-stu-id="ea832-113">Click **Publish to Azure**.</span></span> <span data-ttu-id="ea832-114">Då ansluts ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ea832-114">This will connect to your Azure account.</span></span> <span data-ttu-id="ea832-115">Det kan ta några minuter att ansluta och visa dina aktuella tjänster.</span><span class="sxs-lookup"><span data-stu-id="ea832-115">It may take a few minutes to connect and list your current services.</span></span>

1. <span data-ttu-id="ea832-116">Klicka på **Ny** för att skapa en ny App Service.</span><span class="sxs-lookup"><span data-stu-id="ea832-116">Click **New** to create a new app service.</span></span>

## <a name="configure-your-new-azure-app-service"></a><span data-ttu-id="ea832-117">Konfigurera din nya Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ea832-117">Configure your new Azure App Service</span></span>

1. <span data-ttu-id="ea832-118">I dialogrutan **Skapa App Service** klickar du på **Lägg till ett konto** och loggar in på din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ea832-118">In the **Create App Service dialog** box, click **Add an account**, and sign in to your Azure subscription.</span></span> <span data-ttu-id="ea832-119">Välj kontot som innehåller den önskade prenumerationen i listrutan om du redan är inloggad.</span><span class="sxs-lookup"><span data-stu-id="ea832-119">If you're already signed in, select the account containing the desired subscription from the drop-down menu.</span></span>

1. <span data-ttu-id="ea832-120">Ange nödvändig information om din App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="ea832-120">Enter the required information about your App Service plan.</span></span>

    ![Skapa App Service](../media-draft/5-CreateAppService.png)

    <span data-ttu-id="ea832-122">I dialogrutan **Skapa App Service** anger du följande information:</span><span class="sxs-lookup"><span data-stu-id="ea832-122">In the **Create App Service** dialog box, you will provide the following information:</span></span>

    - <span data-ttu-id="ea832-123">**Appnamn**: det här är namnet på ditt program.</span><span class="sxs-lookup"><span data-stu-id="ea832-123">**App Name**: This is the name of your application.</span></span>  <span data-ttu-id="ea832-124">Detta avgör URL:en för det publicerade programmet, som blir https://_AppName_.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="ea832-124">This will determine the URL of the published application, which will be https://_AppName_.azurewebsites.net.</span></span>  <span data-ttu-id="ea832-125">Det här måste vara ett unikt värde.</span><span class="sxs-lookup"><span data-stu-id="ea832-125">It has to be a unique value.</span></span> <span data-ttu-id="ea832-126">Därför kan du behöva prova några olika namn för att hitta ett som inte har reserverats.</span><span class="sxs-lookup"><span data-stu-id="ea832-126">Therefore, you will have to try out some different names to find one that is not reserved.</span></span>

    - <span data-ttu-id="ea832-127">**Prenumeration**: den Azure-prenumeration du vill distribuera din App Service till.</span><span class="sxs-lookup"><span data-stu-id="ea832-127">**Subscription**: The Azure subscription you wish to deploy the app service to.</span></span>

    - <span data-ttu-id="ea832-128">**Resursgrupp**: klicka på knappen **Ny...** bredvid resursgruppen och ange ett unikt namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ea832-128">**Resource Group:** Click the **New...** button next to the resource group, and enter a unique name for the resource group.</span></span>

        ![Ny resursgrupp](../media-draft/5-NewResourceGroup.png)

    - <span data-ttu-id="ea832-130">**Värdplan**: värdplanen anger plats, storlek och funktioner för den webbservergrupp som är värd för din app.</span><span class="sxs-lookup"><span data-stu-id="ea832-130">**Hosting Plan:** The hosting plan specifies the location, size, and features of the web server farm that hosts your app.</span></span> <span data-ttu-id="ea832-131">Du kan välja en befintlig värdplan eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="ea832-131">You can select an existing hosting plan or create a new one.</span></span> <span data-ttu-id="ea832-132">För den här övningen skapar du en ny.</span><span class="sxs-lookup"><span data-stu-id="ea832-132">For this exercise, you will create a new one.</span></span>

        <span data-ttu-id="ea832-133">Klicka på knappen **Ny...**  intill värdplanen.</span><span class="sxs-lookup"><span data-stu-id="ea832-133">Click the **New...** button next to the hosting plan.</span></span>

        ![Ny värdplan](../media-draft/5-NewHostingPlan.png)

        <span data-ttu-id="ea832-135">Namnge värdplanen och ange sedan plats och storlek.</span><span class="sxs-lookup"><span data-stu-id="ea832-135">Give your hosting plan a name, and then specify the location and the size.</span></span>  
        
        > [!NOTE]
        > <span data-ttu-id="ea832-136">Om du anger olika platser och storlekar påverkar det kostnaden för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ea832-136">Specifying different locations and sizes will impact the cost of the service.</span></span> <span data-ttu-id="ea832-137">För den här övningen rekommenderar vi att du anger storleken **Kostnadsfri**, vilket säkerställer att det inte medför några kostnader.</span><span class="sxs-lookup"><span data-stu-id="ea832-137">For the exercise, it is recommended that you specify the **Free** size, which will ensure you won't incur any costs.</span></span>

    - <span data-ttu-id="ea832-138">**Application Insights:** anger om du vill använda Application Insights för programmet.</span><span class="sxs-lookup"><span data-stu-id="ea832-138">**Application Insights:** Specifies if you want to use Application Insights for your application.</span></span> <span data-ttu-id="ea832-139">För den här övningen rekommenderar vi att du väljer **Ingen.**</span><span class="sxs-lookup"><span data-stu-id="ea832-139">For this exercise, we recommend that you choose **None.**</span></span>

1. <span data-ttu-id="ea832-140">Klicka på knappen **Skapa** för att börja etablera din App Service.</span><span class="sxs-lookup"><span data-stu-id="ea832-140">Click the **Create** button to start provisioning your app service.</span></span> <span data-ttu-id="ea832-141">Förloppet visas allt eftersom den distribueras:</span><span class="sxs-lookup"><span data-stu-id="ea832-141">You will see progress as it deploys:</span></span>

    ![Distribuering pågår](../media-draft/5-DeployProgress.png)

1. <span data-ttu-id="ea832-143">När din App Service har etablerats skapar och distribuerar Visual Studio webbappen.</span><span class="sxs-lookup"><span data-stu-id="ea832-143">Once the app service is provisioned, Visual Studio will build and deploy your web app.</span></span>  <span data-ttu-id="ea832-144">I utdata från Visual Studio-kompileringen kan du se resultatet av distributionen.</span><span class="sxs-lookup"><span data-stu-id="ea832-144">In your Visual Studio build output, you can see the output of the deployment.</span></span>

    ![Publiceringsresultat](../media-draft/5-PublishResult.png)

1. <span data-ttu-id="ea832-146">Grattis! Ditt ASP.NET Core-webbprogram har publiceras och är nu live.</span><span class="sxs-lookup"><span data-stu-id="ea832-146">Congratulations, your ASP.NET Core web application is now published and live.</span></span> <span data-ttu-id="ea832-147">Du kan se URL:en för webbplatsen i utdata från kompileringen och även på publiceringssidan i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea832-147">You will be able to see the final URL for your site in the build output and also the on publishing page in Visual Studio.</span></span>

    ![Publiceringsresultat](../media-draft/5-PublishPage.png)

1. <span data-ttu-id="ea832-149">Du kan testa webbplatsen genom att gå till den angivna webbadressen.</span><span class="sxs-lookup"><span data-stu-id="ea832-149">To test your website, go to the URL indicated.</span></span>

    ![Live-webbplats](../media-draft/5-WebPageLive.png)

## <a name="summary"></a><span data-ttu-id="ea832-151">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ea832-151">Summary</span></span>

<span data-ttu-id="ea832-152">Den här övningen visade hur du etablerar en Azure App Service och distribuerar ett ASP.NET Core-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ea832-152">This exercise demonstrated the process of provisioning an Azure app service and deploying an ASP.NET Core web application.</span></span> <span data-ttu-id="ea832-153">Nu har du en webbapp som är live.</span><span class="sxs-lookup"><span data-stu-id="ea832-153">You now have a live web app.</span></span>
