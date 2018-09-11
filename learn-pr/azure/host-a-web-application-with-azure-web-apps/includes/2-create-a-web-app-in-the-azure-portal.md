<span data-ttu-id="04f21-101">Här får du lära dig hur du skapar en Azure-webbapp med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="04f21-101">Here, you'll learn how create an Azure web app using the Azure portal.</span></span>

## <a name="why-use-the-azure-portal"></a><span data-ttu-id="04f21-102">Varför bör jag använda Azure Portal?</span><span class="sxs-lookup"><span data-stu-id="04f21-102">Why use the Azure portal?</span></span>

<span data-ttu-id="04f21-103">Det första steget i att hantera webbprogrammet är att skapa en **webbapp** i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="04f21-103">The first step in hosting your web application is to create a **web app** inside your Azure subscription.</span></span>

<span data-ttu-id="04f21-104">Det finns flera sätt att skapa en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="04f21-104">There are several ways you can create an Azure web app.</span></span> <span data-ttu-id="04f21-105">Du kan använda Azure Portal, Azure CLI, ett skript eller en IDE.</span><span class="sxs-lookup"><span data-stu-id="04f21-105">You can use the Azure portal, the Azure CLI, a script, or an IDE.</span></span>

<span data-ttu-id="04f21-106">Här använder vi portalen eftersom det är en grafisk upplevelse, vilket gör det till ett bra inlärningsverktyg.</span><span class="sxs-lookup"><span data-stu-id="04f21-106">Here, we are going to use the portal because it's a graphical experience, which makes it a great learning tool.</span></span> <span data-ttu-id="04f21-107">Portalen hjälper dig att upptäcka tillgängliga funktioner, lägga till ytterligare resurser och anpassa befintliga resurser.</span><span class="sxs-lookup"><span data-stu-id="04f21-107">The portal helps you discover available features, add additional resources, and customize existing resources.</span></span>

## <a name="what-is-web-apps-in-azure"></a><span data-ttu-id="04f21-108">Vad är Web Apps i Azure?</span><span class="sxs-lookup"><span data-stu-id="04f21-108">What is Web Apps in Azure?</span></span>

<span data-ttu-id="04f21-109">Web Apps är en fullständigt hanterad plattform i Azure-miljön som är optimerad för hantering av webbappar, REST-API:er och mobila serverdelar.</span><span class="sxs-lookup"><span data-stu-id="04f21-109">Web Apps is a fully managed computing platform within the Azure environment that is optimized for hosting web apps, REST APIs, and mobile back ends.</span></span>

<span data-ttu-id="04f21-110">Den här plattformen som en tjänst (PaaS), som tillhandahålls av Microsoft Azure, gör att du kan fokusera på att bygga medan Azure tar hand om infrastrukturen för att köra och skala dina program.</span><span class="sxs-lookup"><span data-stu-id="04f21-110">This platform as a service (PaaS) offered by Microsoft Azure allows you to focus on the build side of things while Azure takes care of the infrastructure to run and scale your applications.</span></span>

## <a name="how-to-create-an-azure-web-app"></a><span data-ttu-id="04f21-111">Så skapar du en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="04f21-111">How to create an Azure web app</span></span>

<span data-ttu-id="04f21-112">När det är dags att hantera din egen app går du till Azure Portal och skapar en ny **resurs** av typen **Webbapp**.</span><span class="sxs-lookup"><span data-stu-id="04f21-112">When it's time to host your own app, you visit the Azure portal and create a new **resource** of the type **Web App**.</span></span> <span data-ttu-id="04f21-113">Genom att skapa en sådan resurs på Azure Portal skapar du i själva verket en **container** eller en ritning som du kan använda som värd för alla webbaserade program som stöds av Azure, oavsett om det är ASP.NET Core, Node.js, PHP osv. Bilden nedan visar hur enkelt det är att konfigurera det ramverk/språk som används av appen.</span><span class="sxs-lookup"><span data-stu-id="04f21-113">By creating such a resource in the Azure portal, you are actually creating a **container** or a blueprint that you can use to host any web-based application that is supported by Azure, whether it be ASP.NET Core, Node.js, PHP, etc. The figure below shows how easy it is to configure the framework/language used by the app.</span></span>

![Inställningar för webbappen](../media/2-web-app-settings.png)

<span data-ttu-id="04f21-115">Azure Portal innehåller en mall för att skapa en ny webbapp.</span><span class="sxs-lookup"><span data-stu-id="04f21-115">The Azure portal provides a template to create a new web app.</span></span> <span data-ttu-id="04f21-116">Den här mallen kräver följande fält:</span><span class="sxs-lookup"><span data-stu-id="04f21-116">This template requires the following fields:</span></span>

- <span data-ttu-id="04f21-117">**Appnamn**: namnet på webbappen.</span><span class="sxs-lookup"><span data-stu-id="04f21-117">**App name**: The name of the web app.</span></span>
- <span data-ttu-id="04f21-118">**Prenumeration**: en giltig och aktiv prenumeration.</span><span class="sxs-lookup"><span data-stu-id="04f21-118">**Subscription**: A valid and active subscription.</span></span>
- <span data-ttu-id="04f21-119">**Resursgrupp**: en giltig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="04f21-119">**Resource group**: A valid resource group.</span></span> <span data-ttu-id="04f21-120">Avsnitten nedan förklarar i detalj vad en resursgrupp är.</span><span class="sxs-lookup"><span data-stu-id="04f21-120">The sections below explain in detail what a resource group is.</span></span>
- <span data-ttu-id="04f21-121">**OS**: operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="04f21-121">**OS**: The operating system.</span></span> <span data-ttu-id="04f21-122">Alternativen är: Windows, Linux och Docker-containrar.</span><span class="sxs-lookup"><span data-stu-id="04f21-122">The options are: Windows, Linux, and Docker containers.</span></span> <span data-ttu-id="04f21-123">I Windows kan du hantera valfri typ av program från en mängd olika tekniker.</span><span class="sxs-lookup"><span data-stu-id="04f21-123">On Windows, you can host any type of application from a variety of technologies.</span></span> <span data-ttu-id="04f21-124">Detsamma gäller för Linux-värd, förutom det faktum att du bara kan skapa ASP.NET Core-webbappar som använder NET Core-ramverket via Linux.</span><span class="sxs-lookup"><span data-stu-id="04f21-124">The same applies for Linux hosting, except the fact that you can only create ASP.NET Core web apps that use the .NET Core framework over Linux.</span></span> <span data-ttu-id="04f21-125">För andra ASP.NET-appar som använder fullständigt .NET Framework måste du vara värd via Windows OS.</span><span class="sxs-lookup"><span data-stu-id="04f21-125">For other ASP.NET apps using the full .NET Framework, you will have to host over Windows OS.</span></span> <span data-ttu-id="04f21-126">Det sista alternativet är Docker-containrar, där du kan distribuera dina egna lokala Docker-containrar direkt via containrar som hanteras och underhålls av Azure.</span><span class="sxs-lookup"><span data-stu-id="04f21-126">The final option is Docker containers, where you can deploy your own local Docker containers directly over containers hosted and maintained by Azure.</span></span> <span data-ttu-id="04f21-127">I princip kan alltså alla webbappar som använder en teknik med öppen källkod (PHP, ASP.NET Core osv.) hanteras via ett Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="04f21-127">So basically, any web app that makes use of an open-source technology (PHP, ASP.NET Core, etc.) can be hosted on a Linux OS.</span></span>
- <span data-ttu-id="04f21-128">**App Service-plan/plats**: en giltig App Azure Service-plan.</span><span class="sxs-lookup"><span data-stu-id="04f21-128">**App Service plan/location**: A valid Azure App Service plan.</span></span> <span data-ttu-id="04f21-129">Avsnitten nedan förklarar i detalj vad en App Service-plan är.</span><span class="sxs-lookup"><span data-stu-id="04f21-129">The sections below explain in detail what an App Service plan is.</span></span>
- <span data-ttu-id="04f21-130">**Application Insights**: du kan aktivera alternativet Azure Application Insights och dra nytta av de verktyg för övervakning och mått som Azure Portal tillhandahåller för att hjälpa dig att hålla koll på prestandan för dina appar.</span><span class="sxs-lookup"><span data-stu-id="04f21-130">**Applications Insights**: You can turn on the Azure Application Insights option and benefit from the monitoring and metric tools that the Azure portal offers to help you keep an eye on the performance of your apps.</span></span>

<span data-ttu-id="04f21-131">Med Azure Portal får du många kraftfulla verktyg för att hantera, övervaka och styra din webbapp.</span><span class="sxs-lookup"><span data-stu-id="04f21-131">The Azure portal gives you the upper hand in managing, monitoring, and controlling your web app through the many available tools.</span></span>

### <a name="deployment-slots"></a><span data-ttu-id="04f21-132">Distributionsfack</span><span class="sxs-lookup"><span data-stu-id="04f21-132">Deployment slots</span></span>

<span data-ttu-id="04f21-133">Med hjälp av Azure Portal kan du enkelt lägga till **distributionsfack** till en webbapp.</span><span class="sxs-lookup"><span data-stu-id="04f21-133">Using the Azure portal, you can easily add **deployment slots** to a web app.</span></span> <span data-ttu-id="04f21-134">Du kan till exempel skapa ett distributionsfack för **mellanlagring** som du kan skicka koden till för att testa på Azure.</span><span class="sxs-lookup"><span data-stu-id="04f21-134">For instance, you can create a **staging** deployment slot where you can push your code to test on Azure.</span></span> <span data-ttu-id="04f21-135">När du är nöjd med koden kan du enkelt **växla** distributionsfacket för mellanlagring med produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="04f21-135">Once you are happy with your code, you can easily **swap** the staging deployment slot with the production slot.</span></span> <span data-ttu-id="04f21-136">Du gör allt detta med några enkla musklick i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="04f21-136">You do all this with a few simple mouse clicks in the Azure portal.</span></span>

![Distributionsfack](../media/2-deployment-slots.png)

### <a name="continuous-integrationdeployment-support"></a><span data-ttu-id="04f21-138">Stöd för kontinuerlig integrering/distribution</span><span class="sxs-lookup"><span data-stu-id="04f21-138">Continuous integration/deployment support</span></span>

<span data-ttu-id="04f21-139">Azure Portal tillhandahåller färdig kontinuerlig integrering och distribution med Visual Studio Team Services, GitHub, Bitbucket, Dropbox, OneDrive eller en lokal Git-lagringsplats som hanteras fullständigt av Azure.</span><span class="sxs-lookup"><span data-stu-id="04f21-139">The Azure portal provides out-of-the-box continuous integration and deployment with Visual Studio Team Services, GitHub, Bitbucket, Dropbox, OneDrive, or a local Git repository that Azure fully manages.</span></span> <span data-ttu-id="04f21-140">Du ansluter din webbapp med någon av ovanstående källor så gör Azure resten genom att automatiskt synkronisera koden och eventuella framtida ändringar av koden till webbappen.</span><span class="sxs-lookup"><span data-stu-id="04f21-140">You connect your web app with any of the above sources and Azure will do the rest for you by auto-syncing code and any future changes on the code into the web app.</span></span> <span data-ttu-id="04f21-141">Med Visual Studio Team Services definierar du dessutom din egen bygg- och lanseringsprocess som avslutas med att din källkod kompileras, testerna körs, en version skapas och slutligen att versionen skickas till en webbapp varje gång du checkar in koden.</span><span class="sxs-lookup"><span data-stu-id="04f21-141">Furthermore, with Visual Studio Team Services, you can define your own build and release process that ends up compiling your source code, running the tests, building a release, and finally pushing the release into a web app every time your commit the code.</span></span> <span data-ttu-id="04f21-142">Allt detta händer implicit utan manuella åtgärder.</span><span class="sxs-lookup"><span data-stu-id="04f21-142">All that happens implicitly without any need to intervene.</span></span>

![Konfigurera kontinuerlig integrering](../media/2-continuous-integration.PNG)

### <a name="integrated-visual-studio-publishing-and-ftp-publishing"></a><span data-ttu-id="04f21-144">Integrerad Visual Studio-publicering och FTP-publicering</span><span class="sxs-lookup"><span data-stu-id="04f21-144">Integrated Visual Studio publishing and FTP publishing</span></span>

<span data-ttu-id="04f21-145">Utöver att kunna konfigurera kontinuerlig integrering/distribution för webbappen kan du alltid dra nytta av den nära integreringen med Visual Studio för att publicera webbappen till Azure via WebDeploy-teknik.</span><span class="sxs-lookup"><span data-stu-id="04f21-145">In addition to being able to set up continuous integration/deployment for your web app, you can always benefit from the tight integration with Visual Studio to publish your web app to Azure via Web Deploy technology.</span></span> <span data-ttu-id="04f21-146">Dessutom har Azure stöd för FTP, men FTP är inte den optimala lösningen för publicering eftersom det saknar vissa funktioner i WebDeploy för att välja endast de filer som har ändrats eller lagts i stället för att bara publicera alltihop i Azure!</span><span class="sxs-lookup"><span data-stu-id="04f21-146">Also, Azure supports FTP, although you are better off not using FTP for publishing because it lacks some capability in Web Deploy to pick and choose only those files that were changed or added, and not just publish everything to Azure!</span></span>

### <a name="built-in-auto-scale-support-automatically-scale-updown-based-on-real-world-load"></a><span data-ttu-id="04f21-147">Inbyggt stöd för automatisk skalning (skala automatiskt upp/ned baserat på verklig belastning)</span><span class="sxs-lookup"><span data-stu-id="04f21-147">Built-in auto scale support (automatically scale up/down based on real-world load)</span></span>

<span data-ttu-id="04f21-148">I webbappen ingår möjligheten att skala upp/ned eller skala ut. Beroende på användningen av webbappen kan du skala upp/ned appen genom att öka/minska resurserna för den underliggande datorn som är värd för webbappen.</span><span class="sxs-lookup"><span data-stu-id="04f21-148">Baked into the web app is the ability to scale up/down or scale out. Depending on the usage of the web app, you can scale your app up/down by increasing/decreasing the resources of the underlying machine that is hosting your web app.</span></span> <span data-ttu-id="04f21-149">Resurser kan vara antal kärnor eller mängden tillgängligt RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="04f21-149">Resources can be number of cores or the amount of RAM available.</span></span>

<span data-ttu-id="04f21-150">Att skala ut är å andra sidan möjligheten att öka antalet datorinstanser som kör webbappen.</span><span class="sxs-lookup"><span data-stu-id="04f21-150">Scaling out, on the other hand, is the ability to increase the number of machine instances that are running your web app.</span></span>

## <a name="what-is-a-resource-group"></a><span data-ttu-id="04f21-151">Vad är en resursgrupp?</span><span class="sxs-lookup"><span data-stu-id="04f21-151">What is a resource group?</span></span>

<span data-ttu-id="04f21-152">En resursgrupp är en metod för att gruppera resurser och tjänster som är beroende av varandra, till exempel virtuella datorer, webbappar, databaser och mer för ett visst program och en miljö.</span><span class="sxs-lookup"><span data-stu-id="04f21-152">A resource group is a method of grouping interdependent resources and services such as virtual machines, web apps, databases, and more for a given application and environment.</span></span> <span data-ttu-id="04f21-153">Se den som en **mapp**, en plats för att gruppera element i appen.</span><span class="sxs-lookup"><span data-stu-id="04f21-153">Think of it as a **folder**, a place to group elements of your app.</span></span>

<span data-ttu-id="04f21-154">Med resursgrupper kan du enkelt hantera och ta bort resurser.</span><span class="sxs-lookup"><span data-stu-id="04f21-154">Resource groups allow you to easily manage and delete resources.</span></span> <span data-ttu-id="04f21-155">De ger också ett sätt att övervaka, kontrollera, komma åt, etablera och hantera fakturering för samlingar av resurser som krävs för att köra ett program eller används av en klient.</span><span class="sxs-lookup"><span data-stu-id="04f21-155">They also provide a way to monitor, control access, provision, and manage billing for collections of resources that are required to run an application or are used by a client.</span></span>

## <a name="what-is-an-app-service-plan"></a><span data-ttu-id="04f21-156">Vad är en App Service-plan?</span><span class="sxs-lookup"><span data-stu-id="04f21-156">What is an App Service plan?</span></span>

<span data-ttu-id="04f21-157">En App Service-plan är en uppsättning fysiska resurser och tillgänglig kapacitet som du kan distribuera dina App Service-appar till.</span><span class="sxs-lookup"><span data-stu-id="04f21-157">An App Service plan is a set of physical resources and capacity available to deploy your App Service apps into.</span></span>

<span data-ttu-id="04f21-158">Azure-portalen innehåller en mall för att skapa en ny App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="04f21-158">The Azure portal provides a template to create a new App Service plan.</span></span> <span data-ttu-id="04f21-159">Den här mallen kräver följande grundläggande information:</span><span class="sxs-lookup"><span data-stu-id="04f21-159">This template requires the following basic information:</span></span>

- <span data-ttu-id="04f21-160">Region (USA, västra, USA, centrala, Europa, norra osv.)</span><span class="sxs-lookup"><span data-stu-id="04f21-160">Region (West US, Central US, North Europe, etc.)</span></span>
- <span data-ttu-id="04f21-161">Skalningsantal (en, två, tre instanser osv.)</span><span class="sxs-lookup"><span data-stu-id="04f21-161">Scale count (one, two, three instances, etc.)</span></span>
- <span data-ttu-id="04f21-162">Instansstorlek (liten, medel eller stor)</span><span class="sxs-lookup"><span data-stu-id="04f21-162">Instance size (Small, Medium, or Large)</span></span>
- <span data-ttu-id="04f21-163">Lagerhållningsenhet – SKU (Kostnadsfri, Delad, Basic, Standard, Premium och nyligen Premium v2)</span><span class="sxs-lookup"><span data-stu-id="04f21-163">Stock Keeping Unit - SKU (Free, Shared, Basic, Standard, Premium, and more recently, Premium v2)</span></span>

<span data-ttu-id="04f21-164">Funktionerna Web Apps, Mobile Apps och API Apps i Azure App Service och Azure Functions i en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="04f21-164">The Web Apps, Mobile Apps, and API Apps features of Azure App Service, and Azure Functions all run in an App Service plan.</span></span> <span data-ttu-id="04f21-165">Du kan distribuera ett obegränsat antal program till en App Service-plan, men hur många du använder beror starkt på vilka typer av program som distribueras och vilka resurser som krävs för CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="04f21-165">While you can deploy an unlimited number of applications into an App Service plan, the number you use greatly depends on the types of applications deployed and their required resources in CPU utilization.</span></span>

<span data-ttu-id="04f21-166">Du kan alltid använda din App Service-plan i Azure Portal för att visualisera din CPU- och minnesanvändning så att du kan bestämma ditt behov av att skala eller flytta program till en annan App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="04f21-166">You can always use your App Service plan in the Azure portal to visualize your CPU and memory utilization to help determine your needs for scaling or moving applications into another App Service plan.</span></span>