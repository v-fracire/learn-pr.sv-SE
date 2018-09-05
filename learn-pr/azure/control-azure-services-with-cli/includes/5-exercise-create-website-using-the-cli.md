<span data-ttu-id="7cbac-101">Nu ska vi använda Azure CLI för att skapa en resursgrupp och sedan distribuera en webbapp i den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7cbac-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="7cbac-102">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7cbac-102">Create a resource group</span></span>

1. <span data-ttu-id="7cbac-103">Öppna ett bash-gränssnitt i Linux eller macOS, eller öppna kommandotolken eller PowerShell om du arbetar i Windows.</span><span class="sxs-lookup"><span data-stu-id="7cbac-103">Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows.</span></span>

1. <span data-ttu-id="7cbac-104">Starta Azure CLI och kör inloggningskommandot.</span><span class="sxs-lookup"><span data-stu-id="7cbac-104">Start the Azure CLI and run the login command.</span></span>

    ```bash
    az login
    ```
    <span data-ttu-id="7cbac-105">Om du inte ser en inloggningssida för Azure i webbläsaren följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span><span class="sxs-lookup"><span data-stu-id="7cbac-105">If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

1. <span data-ttu-id="7cbac-106">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7cbac-106">Create a resource group.</span></span>

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. <span data-ttu-id="7cbac-107">Kontrollera att resursgruppen har skapats genom att visa alla resursgrupper i en tabell.</span><span class="sxs-lookup"><span data-stu-id="7cbac-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```bash
    az group list --output table
    ```

> [!TIP]
> <span data-ttu-id="7cbac-108">Du kan också kontrollera att resursen har skapats på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7cbac-108">You can also confirm the resource was created in the Azure portal.</span></span> <span data-ttu-id="7cbac-109">Öppna en webbläsare, logga in på portalen och gå till avsnittet **Resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="7cbac-109">Open a web browser, sign in to the portal and navigate to the **Resource Groups** section.</span></span> <span data-ttu-id="7cbac-110">Den nya resursgruppen bör visas i listan.</span><span class="sxs-lookup"><span data-stu-id="7cbac-110">The new resource group should be displayed in the list.</span></span>
> 
> ![Använda portalen för att visa resursgrupper](../media-drafts/5-listing-resource-groups.png)

1. <span data-ttu-id="7cbac-112">Om det finns många objekt i gruppen kan du filtrera returvärdena genom att lägga till ett `--query`-alternativ. Prova det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="7cbac-112">If you have a lot of items in the group, you can filter the return values by adding a `--query` option, try this command:</span></span>

    ```bash
    az group list --query '[?name == popupResGroup]'
    ```

    <span data-ttu-id="7cbac-113">Frågan är formaterad med **JMESPath**, som är ett standardfrågespråk för JSON-begäranden.</span><span class="sxs-lookup"><span data-stu-id="7cbac-113">The query is formmated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="7cbac-114">Du kan läsa mer om det här kraftfulla filterspråket på <http://jmespath.org/>.</span><span class="sxs-lookup"><span data-stu-id="7cbac-114">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="7cbac-115">Vi tar även upp frågor mer ingående i modulen **Hantera virtuella datorer med Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="7cbac-115">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="7cbac-116">Steg för att skapa en serviceplan</span><span class="sxs-lookup"><span data-stu-id="7cbac-116">Steps to create a service plan</span></span>

<span data-ttu-id="7cbac-117">När du kör webbappar med Azure App Service betalar du för de Azure-beräkningsresurser som används av apparna, och beloppet beror på den App Service-plan som är associerad med webbapparna.</span><span class="sxs-lookup"><span data-stu-id="7cbac-117">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="7cbac-118">Serviceplaner avgör vilken region som används för appens datacenter, hur många virtuella datorer som används och prisnivån.</span><span class="sxs-lookup"><span data-stu-id="7cbac-118">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="7cbac-119">Skapa en App Service-plan för att köra din app.</span><span class="sxs-lookup"><span data-stu-id="7cbac-119">Create an App Service plan to run your app.</span></span> <span data-ttu-id="7cbac-120">Följande kommando anger inte någon prisnivå eller information om VM-instansen, så som standard får du en plan på **Basic**-nivå med en **Liten** VM-instans.</span><span class="sxs-lookup"><span data-stu-id="7cbac-120">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="7cbac-121">Namnet på appen och planen måste vara _unika_. Lägg därför till ett suffix till namnet och ersätt texten `<unique>` i kommandot nedan med några siffror, dina initialer eller en textsträng så att namnet blir unikt i hela Azure.</span><span class="sxs-lookup"><span data-stu-id="7cbac-121">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span> 

    ```bash
    az appservice plan create --name popupapp-<unique> --resource-group popupResGroup --location westeurope
    ```

1. <span data-ttu-id="7cbac-122">Kontrollera att serviceplanen har skapats genom att visa alla dina planer i en tabell.</span><span class="sxs-lookup"><span data-stu-id="7cbac-122">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```bash
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="7cbac-123">Steg för att skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="7cbac-123">Steps to create a web app</span></span>

<span data-ttu-id="7cbac-124">Nu ska vi skapa webbappen i din serviceplan.</span><span class="sxs-lookup"><span data-stu-id="7cbac-124">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="7cbac-125">Du kan distribuera koden på samma gång, men i vårt exempel gör vi det i separata steg.</span><span class="sxs-lookup"><span data-stu-id="7cbac-125">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="7cbac-126">Skapa webbappen och ange namnet på planen som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="7cbac-126">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="7cbac-127">**Precis som planen måste appnamnet vara unikt. Ersätt därför markören `<unique>` med en textsträng så att namnet blir unikt globalt.**</span><span class="sxs-lookup"><span data-stu-id="7cbac-127">**Just like the plan, the app name must be unique, replace the `<unique>` marker with some text to make the name globally unique.**</span></span>
    ```bash
    az webapp create --name popupapp-<unique> --resource-group popupResGroup --plan popupapp-<unique>
    ```

1. <span data-ttu-id="7cbac-128">Kontrollera att appen har skapats genom att visa alla dina appar i en tabell.</span><span class="sxs-lookup"><span data-stu-id="7cbac-128">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```bash
    az webapp list --output table
    ```

1. <span data-ttu-id="7cbac-129">Anteckna **DefaultHostName**, du behöver det här värdet senare.</span><span class="sxs-lookup"><span data-stu-id="7cbac-129">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="7cbac-130">Steg för att distribuera kod från GitHub</span><span class="sxs-lookup"><span data-stu-id="7cbac-130">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="7cbac-131">Det sista steget är att distribuera kod från en GitHub-lagringsplats till webbappen.</span><span class="sxs-lookup"><span data-stu-id="7cbac-131">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="7cbac-132">Vi ska använda en enkel PHP-sida som är tillgänglig på Github-lagringsplatsen för Azure-exempel och som visar ”HelloWorld!”</span><span class="sxs-lookup"><span data-stu-id="7cbac-132">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="7cbac-133">när den körs.</span><span class="sxs-lookup"><span data-stu-id="7cbac-133">when it executes.</span></span> <span data-ttu-id="7cbac-134">Var noga med att använda namnet på webbappen som du skapat.</span><span class="sxs-lookup"><span data-stu-id="7cbac-134">Make sure to use the web app name you created.</span></span>

    ```bash
    az webapp deployment source config --name popupapp-<unique> --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="7cbac-135">När den har distribuerats blir din webbplats tillgänglig via det unika appnamnet i domänen `azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="7cbac-135">Once it's deployed, Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="7cbac-136">Om appnamnet till exempel är ”popupapp mslearn123” blir webbadressen: <http://popupapp-mslearn123.azurewebsites.net>.</span><span class="sxs-lookup"><span data-stu-id="7cbac-136">For example, if my app name was "popupapp-mslearn123", then my website address would be: <http://popupapp-mslearn123.azurewebsites.net>.</span></span> <span data-ttu-id="7cbac-137">Du måste ange rätt URL för att komma till din specifika instans.</span><span class="sxs-lookup"><span data-stu-id="7cbac-137">You will need to type in the correct URL to hit your specific instance.</span></span>

1. <span data-ttu-id="7cbac-138">”HelloWorld!” visas på sidan.</span><span class="sxs-lookup"><span data-stu-id="7cbac-138">The page displays "HelloWorld!"</span></span>

1. <span data-ttu-id="7cbac-139">Stäng webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="7cbac-139">Close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="7cbac-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7cbac-140">Summary</span></span>

<span data-ttu-id="7cbac-141">I den här övningen fick du se ett vanligt mönster för en interaktiv Azure CLI-session.</span><span class="sxs-lookup"><span data-stu-id="7cbac-141">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="7cbac-142">Först använde du ett standardkommando för att skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7cbac-142">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="7cbac-143">Sedan använde du en uppsättning kommandon för att distribuera en resurs (i det här exemplet en webbapp) till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7cbac-143">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="7cbac-144">Du kan enkelt samla de här kommandona i ett kommandoskript och köra det varje gång du behöver skapa samma resurs.</span><span class="sxs-lookup"><span data-stu-id="7cbac-144">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
