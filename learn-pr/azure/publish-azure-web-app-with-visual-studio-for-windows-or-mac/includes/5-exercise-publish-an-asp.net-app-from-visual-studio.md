<span data-ttu-id="ef79f-101">Nu har du ett ASP.NET Core-webbprogram som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="ef79f-101">You now have a local running ASP.NET Core Web Application.</span></span> <span data-ttu-id="ef79f-102">I den här övningen publicerar du programmet till en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ef79f-102">In this exercise, you'll publish your application to an Azure App Service.</span></span>

### <a name="visual-studio-for-windows"></a><span data-ttu-id="ef79f-103">Visual Studio för Windows</span><span class="sxs-lookup"><span data-stu-id="ef79f-103">Visual Studio for Windows</span></span>

1. <span data-ttu-id="ef79f-104">I Solution Explorer **högerklickar** du på ditt projekt och väljer **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="ef79f-104">In the Solution Explorer, **right-click** your project and select **Publish**.</span></span>

1. <span data-ttu-id="ef79f-105">I den dialogruta som visas till vänster väljer du **App Service** som publiceringsmål.</span><span class="sxs-lookup"><span data-stu-id="ef79f-105">In the dialog that appears, on the left choose **App Service** as your publish target.</span></span>  <span data-ttu-id="ef79f-106">Till höger väljer du **Skapa ny** för att skapa en ny App Service.</span><span class="sxs-lookup"><span data-stu-id="ef79f-106">On the right, select **Create New** to create a new App Service.</span></span>

1. <span data-ttu-id="ef79f-107">Klicka på knappen **Publicera** för att skapa din nya App Service</span><span class="sxs-lookup"><span data-stu-id="ef79f-107">Click the **Publish** button to create your new App Service</span></span>

> [!NOTE]
> <span data-ttu-id="ef79f-108">När du distribuerar dina appar kan du skapa en ny App Service-plan, eller så kan du fortsätta att lägga till appar i en befintlig plan.</span><span class="sxs-lookup"><span data-stu-id="ef79f-108">When you deploy your apps you can create a new App Service plan or you can continue to add apps to an existing plan.</span></span> <span data-ttu-id="ef79f-109">För den här övningen skapar du en ny **Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="ef79f-109">For this exercise, you will create a new **Azure App Service**.</span></span>

### <a name="visual-studio-mac"></a><span data-ttu-id="ef79f-110">Visual Studio Mac</span><span class="sxs-lookup"><span data-stu-id="ef79f-110">Visual Studio Mac</span></span>

1. <span data-ttu-id="ef79f-111">I Solution Explorer **högerklickar** du på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="ef79f-111">In the Solution Explorer, **right-click** your project.</span></span>

1. <span data-ttu-id="ef79f-112">Välj **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="ef79f-112">Select **Publish**.</span></span>

1. <span data-ttu-id="ef79f-113">Klicka på **Publicera till Azure** så ansluts det till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ef79f-113">Click **Publish to Azure**, this will connect to your Azure account.</span></span> <span data-ttu-id="ef79f-114">Det kan ta några minuter att ansluta och visa dina aktuella tjänster.</span><span class="sxs-lookup"><span data-stu-id="ef79f-114">It may take a few minutes to connect and list your current services.</span></span>

1. <span data-ttu-id="ef79f-115">Klicka på **Ny** för att skapa en ny App Service.</span><span class="sxs-lookup"><span data-stu-id="ef79f-115">Click **New** to create a new App Service.</span></span>

## <a name="configure-your-new-azure-app-service"></a><span data-ttu-id="ef79f-116">Konfigurera din nya Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef79f-116">Configure your new Azure App Service</span></span>

1. <span data-ttu-id="ef79f-117">I **dialogrutan Skapa App Service** klickar du på **Lägg till ett konto** och loggar in till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ef79f-117">In the **Create App Service dialog**, click **Add an account**, and sign in to your Azure subscription.</span></span> <span data-ttu-id="ef79f-118">Välj det konto som innehåller den önskade prenumerationen i listrutan om du redan är inloggad.</span><span class="sxs-lookup"><span data-stu-id="ef79f-118">If you're already signed in, select the account containing the desired subscription from the dropdown.</span></span>

1. <span data-ttu-id="ef79f-119">Ange nödvändig information om din App Service-plan</span><span class="sxs-lookup"><span data-stu-id="ef79f-119">Enter the required information about your App Service Plan</span></span>

    ![Skapa App Service](../media-draft/5-CreateAppService.png)

    <span data-ttu-id="ef79f-121">I dialogrutan **Skapa App Service** ska du ange följande information:</span><span class="sxs-lookup"><span data-stu-id="ef79f-121">In the **Create App Service** dialog you will provide the following information:</span></span>

    - <span data-ttu-id="ef79f-122">**Appnamn**: det här är namnet på ditt program.</span><span class="sxs-lookup"><span data-stu-id="ef79f-122">**App Name**: This is the name of your application.</span></span>  <span data-ttu-id="ef79f-123">Detta avgör URL:en för det publicerade programmet, vilket blir https://_AppName_.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="ef79f-123">This will determine the URL of the published application, which will be https://_AppName_.azurewebsites.net.</span></span>  <span data-ttu-id="ef79f-124">Det måste vara ett unikt värde. Därför måste du prova några olika namn för att hitta ett som inte har reserverats.</span><span class="sxs-lookup"><span data-stu-id="ef79f-124">It has to be a unique value, therefore you will have to try out some different names to find one that is not reserved.</span></span>

    - <span data-ttu-id="ef79f-125">**Prenumeration**: den Azure-prenumeration som du vill distribuera App Service till.</span><span class="sxs-lookup"><span data-stu-id="ef79f-125">**Subscription**: The Azure Subscription you wish to deploy the App Service to.</span></span>

    - <span data-ttu-id="ef79f-126">**Resursgrupp:** klicka på **Ny...** Knappen bredvid resursgruppen och ange ett unikt namn för resursgruppen</span><span class="sxs-lookup"><span data-stu-id="ef79f-126">**Resource Group:** Click the **New...** Button next to the Resource Group and enter a unique name for the Resource Group'</span></span>

        ![Ny resursgrupp](../media-draft/5-NewResourceGroup.png)

    - <span data-ttu-id="ef79f-128">**Värdplan:** värdplanen anger plats, storlek och funktioner för den webbservergrupp som är värd för din app.</span><span class="sxs-lookup"><span data-stu-id="ef79f-128">**Hosting Plan:** The Hosting Plan specifies the location, size, and features of the web server farm that hosts your app.</span></span> <span data-ttu-id="ef79f-129">Du kan välja en befintlig värdplan eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="ef79f-129">You can select an existing Hosting plan, or create a new one.</span></span>  <span data-ttu-id="ef79f-130">För den här övningen skapar du en ny.</span><span class="sxs-lookup"><span data-stu-id="ef79f-130">For this exercise, you will create a new one.</span></span>

        <span data-ttu-id="ef79f-131">Klicka på knappen **Ny...**  intill värdplanen</span><span class="sxs-lookup"><span data-stu-id="ef79f-131">Click the **New...** button next to the Hosting Plan</span></span>

        ![Ny värdplan](../media-draft/5-NewHostingPlan.png)

        <span data-ttu-id="ef79f-133">Namnge värdplanen och ange plats och storlek.</span><span class="sxs-lookup"><span data-stu-id="ef79f-133">Give your hosting plan a name, specify the location and the size.</span></span>  <span data-ttu-id="ef79f-134">Obs! Om du anger olika plats och storlek påverkar det kostnaden för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef79f-134">Note: Specifying different Locations and Size will impact the cost for the service.</span></span> <span data-ttu-id="ef79f-135">För den här övningen rekommenderar vi att du anger storleken **Kostnadsfri**, vilket säkerställer att det inte medför några kostnader.</span><span class="sxs-lookup"><span data-stu-id="ef79f-135">For the exercise,it is recommended that you specify the size of **Free** which will ensure you won't incur any costs.</span></span>

    - <span data-ttu-id="ef79f-136">**Application Insights:** anger om du vill använda Application Insights för programmet.</span><span class="sxs-lookup"><span data-stu-id="ef79f-136">**Application Insights:** Specifies if you want to use Application Insights for your application.</span></span> <span data-ttu-id="ef79f-137">I den här övningen rekommenderar vi att du väljer Inget</span><span class="sxs-lookup"><span data-stu-id="ef79f-137">For this exercise, is recommended to choose None</span></span>

1. <span data-ttu-id="ef79f-138">Klicka på knappen **Skapa** för att börja etablera din App Service.</span><span class="sxs-lookup"><span data-stu-id="ef79f-138">Click the **Create** Button to start provisioning your App Service.</span></span> <span data-ttu-id="ef79f-139">Förloppet visas allt eftersom den distribueras:</span><span class="sxs-lookup"><span data-stu-id="ef79f-139">You will see progress as it deploys:</span></span>

    ![Distribuering pågår](../media-draft/5-DeployProgress.png)

1. <span data-ttu-id="ef79f-141">När App Service har etablerats skapar och distribuerar Visual Studio webbappen.</span><span class="sxs-lookup"><span data-stu-id="ef79f-141">Once the App Service is provisioned, Visual Studio will build and deploy your Web App.</span></span>  <span data-ttu-id="ef79f-142">I dina Visual Studio-byggeutdata kan du se resultatet av distributionen.</span><span class="sxs-lookup"><span data-stu-id="ef79f-142">In your Visual Studio Build Output you can see the output of the deployment.</span></span>

    ![Publicera resultat](../media-draft/5-PublishResult.png)

1. <span data-ttu-id="ef79f-144">Grattis! Ditt ASP.NET Core-webbprogram har publiceras och är nu live.</span><span class="sxs-lookup"><span data-stu-id="ef79f-144">Congratulations, your ASP.NET Core Web Application is now published and live.</span></span> <span data-ttu-id="ef79f-145">Du kommer att kunna se den slutliga URL:en för webbplatsen i byggeutdata och även publiceringssidan i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef79f-145">You will be able to see the final URL for your site in the build output and also the publishing page in Visual Studio.</span></span>

    ![Publicera resultat](../media-draft/5-PublishPage.png)

1. <span data-ttu-id="ef79f-147">Du kan testa webbplatsen genom att då till den angivna URL:en</span><span class="sxs-lookup"><span data-stu-id="ef79f-147">To test your website, go to the URL indicated</span></span>

    ![Live-webbplats](../media-draft/5-WebPageLive.png)

## <a name="summary"></a><span data-ttu-id="ef79f-149">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ef79f-149">Summary</span></span>

<span data-ttu-id="ef79f-150">Den här övningen visade hur du etablerar en Azure App Service och distribuerar ett ASP.NET Core-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ef79f-150">This exercise demonstrated the process of provisioning an Azure App Service and deploying an ASP.NET Core Web Application.</span></span> <span data-ttu-id="ef79f-151">Nu har du en webbapp som är live.</span><span class="sxs-lookup"><span data-stu-id="ef79f-151">You now have a live web app.</span></span>
