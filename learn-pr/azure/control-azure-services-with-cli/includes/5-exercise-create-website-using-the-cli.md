<span data-ttu-id="cd44e-101">Nu ska vi använda Azure CLI för att skapa en resursgrupp och sedan distribuera en webbapp i den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="cd44e-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="using-a-resource-group"></a><span data-ttu-id="cd44e-102">Använda en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="cd44e-102">Using a resource group</span></span>

<span data-ttu-id="cd44e-103">När du arbetar med din egen dator och Azure-prenumeration behöver du först logga in på Azure med kommandot `az login`.</span><span class="sxs-lookup"><span data-stu-id="cd44e-103">When you are working with your own machine and Azure subscription you will need to first login to Azure using the `az login` command.</span></span> <span data-ttu-id="cd44e-104">Det här behövs inte med Cloud Shell-miljön.</span><span class="sxs-lookup"><span data-stu-id="cd44e-104">This is unnecessary with the Cloud Shell environment.</span></span>

<span data-ttu-id="cd44e-105">Därefter skulle du normalt skapa en resursgrupp för alla dina relaterade Azure-resurser med ett `az group create`-kommando, men för de här övningarna har en resursgrupp redan skapats åt dig.</span><span class="sxs-lookup"><span data-stu-id="cd44e-105">Next, you would normally create a resource group for all your related Azure resources with an `az group create` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="cd44e-106">Använd **<rgn>[resursgruppsnamn för sandbox-miljö]</rgn>** som resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="cd44e-106">Use **<rgn>[sandbox resource group name]</rgn>** for your resource group.</span></span>

1. <span data-ttu-id="cd44e-107">Du kan be Azure CLI att lista alla dina resursgrupper i en tabell.</span><span class="sxs-lookup"><span data-stu-id="cd44e-107">You can ask the Azure CLI to list all your resource groups in a table.</span></span> <span data-ttu-id="cd44e-108">Det bör bara finnas en när du arbetar med den kostnadsfria sandbox-miljön i Azure.</span><span class="sxs-lookup"><span data-stu-id="cd44e-108">There should just be one while you are in the free Azure sandbox.</span></span>

    ```azurecli
    az group list --output table
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="cd44e-109">I takt med att du utvecklar mer i Azure kan du ha många resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="cd44e-109">As you do more Azure development, you can end up with several resource groups.</span></span> <span data-ttu-id="cd44e-110">Om det finns flera objekt i grupplistan kan du filtrera returvärdena genom att lägga till ett `--query`-alternativ.</span><span class="sxs-lookup"><span data-stu-id="cd44e-110">If you have several items in the group list, you can filter the return values by adding a `--query` option.</span></span> <span data-ttu-id="cd44e-111">Testa följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cd44e-111">Try this command:</span></span>

    ```azurecli
    az group list --query "[?name == '<rgn>[sandbox resource group name]</rgn>']"
    ```

    <span data-ttu-id="cd44e-112">Frågan är formaterad med **JMESPath**, som är ett standardfrågespråk för JSON-begäranden.</span><span class="sxs-lookup"><span data-stu-id="cd44e-112">The query is formated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="cd44e-113">Du kan läsa mer om det här kraftfulla filterspråket på <http://jmespath.org/>.</span><span class="sxs-lookup"><span data-stu-id="cd44e-113">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="cd44e-114">Vi tar även upp frågor mer ingående i modulen **Hantera virtuella datorer med Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="cd44e-114">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="cd44e-115">Steg för att skapa en serviceplan</span><span class="sxs-lookup"><span data-stu-id="cd44e-115">Steps to create a service plan</span></span>

<span data-ttu-id="cd44e-116">När du kör webbappar med Azure App Service betalar du för de Azure-beräkningsresurser som används av apparna, och beloppet beror på den App Service-plan som är associerad med webbapparna.</span><span class="sxs-lookup"><span data-stu-id="cd44e-116">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="cd44e-117">Serviceplaner avgör vilken region som används för appens datacenter, hur många virtuella datorer som används och prisnivån.</span><span class="sxs-lookup"><span data-stu-id="cd44e-117">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="cd44e-118">Skapa en App Service-plan för att köra din app.</span><span class="sxs-lookup"><span data-stu-id="cd44e-118">Create an App Service plan to run your app.</span></span> <span data-ttu-id="cd44e-119">Följande kommando anger inte någon prisnivå eller information om VM-instansen, så som standard får du en plan på **Basic**-nivå med en **Liten** VM-instans.</span><span class="sxs-lookup"><span data-stu-id="cd44e-119">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="cd44e-120">Namnet på appen och planen måste vara _unika_. Lägg därför till ett suffix till namnet och ersätt texten `<unique>` i kommandot nedan med några siffror, dina initialer eller en textsträng så att namnet blir unikt i hela Azure.</span><span class="sxs-lookup"><span data-stu-id="cd44e-120">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span>

    <span data-ttu-id="cd44e-121">För parametern `--location` använder du ett av platsvärdena nedan.</span><span class="sxs-lookup"><span data-stu-id="cd44e-121">For the `--location` parameter, use one of the below location values.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --location <location>
    ```

    <span data-ttu-id="cd44e-122">Det här kommandot kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="cd44e-122">This command can take several minutes to complete.</span></span>

1. <span data-ttu-id="cd44e-123">Kontrollera att serviceplanen har skapats genom att visa alla dina planer i en tabell.</span><span class="sxs-lookup"><span data-stu-id="cd44e-123">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="cd44e-124">Steg för att skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="cd44e-124">Steps to create a web app</span></span>

<span data-ttu-id="cd44e-125">Nu ska vi skapa webbappen i din serviceplan.</span><span class="sxs-lookup"><span data-stu-id="cd44e-125">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="cd44e-126">Du kan distribuera koden på samma gång, men i vårt exempel gör vi det i separata steg.</span><span class="sxs-lookup"><span data-stu-id="cd44e-126">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="cd44e-127">Skapa webbappen och ange namnet på den plan som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="cd44e-127">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="cd44e-128">**Precis som planen måste appnamnet vara unikt**. Ersätt därför markören `<unique>` med en textsträng så att namnet blir unikt globalt.</span><span class="sxs-lookup"><span data-stu-id="cd44e-128">**Just like the plan, the app name must be unique**, replace the `<unique>` marker with some text to make the name globally unique.</span></span>

    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --plan popupappplan-<unique>
    ```

1. <span data-ttu-id="cd44e-129">Kontrollera att appen har skapats genom att visa alla dina appar i en tabell.</span><span class="sxs-lookup"><span data-stu-id="cd44e-129">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```azurecli
    az webapp list --output table
    ```

1. <span data-ttu-id="cd44e-130">Anteckna det **DefaultHostName** som visas i tabellen; det här är den nåbara webbadressen för den nya webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="cd44e-130">Make a note of the **DefaultHostName** listed in the table; this is the reachable web address for the new website.</span></span> <span data-ttu-id="cd44e-131">Azure gör att din webbplats blir tillgänglig via det unika appnamnet i domänen `azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="cd44e-131">Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="cd44e-132">Om appnamnet till exempel är ”popupwebapp-mslearn123” blir URL:en: `http://popupwebapp-mslearn123.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="cd44e-132">For example, if my app name was "popupwebapp-mslearn123", then my website URL would be: `http://popupwebapp-mslearn123.azurewebsites.net`.</span></span>

1. <span data-ttu-id="cd44e-133">Webbplatsen har en ”QuickStart”-sida (Snabbstart) som skapas av Azure och som du kan se antingen i en webbläsare eller med CURL. Använd bara **DefaultHostName**:</span><span class="sxs-lookup"><span data-stu-id="cd44e-133">Your site has a "QuickStart" page created by Azure that you can see either in a browser, or with CURL, just use the **DefaultHostName**:</span></span>

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="cd44e-134">Steg för att distribuera kod från GitHub</span><span class="sxs-lookup"><span data-stu-id="cd44e-134">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="cd44e-135">Det sista steget är att distribuera kod från en GitHub-lagringsplats till webbappen.</span><span class="sxs-lookup"><span data-stu-id="cd44e-135">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="cd44e-136">Vi ska använda en enkel PHP-sida som är tillgänglig på Github-lagringsplatsen för Azure-exempel och som visar ”HelloWorld!”</span><span class="sxs-lookup"><span data-stu-id="cd44e-136">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="cd44e-137">när den körs.</span><span class="sxs-lookup"><span data-stu-id="cd44e-137">when it executes.</span></span> <span data-ttu-id="cd44e-138">Var noga med att använda namnet på den webbapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="cd44e-138">Make sure to use the web app name you created.</span></span>

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="cd44e-139">Besök din webbplats igen med en webbläsare eller CURL när den har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="cd44e-139">Once it's deployed, hit your site again with a browser or CURL.</span></span>

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
    <span data-ttu-id="cd44e-140">”Hello World!” visas på sidan.</span><span class="sxs-lookup"><span data-stu-id="cd44e-140">The page displays "Hello World!"</span></span>

    ```output
    Hello World!
    ```

<span data-ttu-id="cd44e-141">I den här övningen fick du se ett vanligt mönster för en interaktiv Azure CLI-session.</span><span class="sxs-lookup"><span data-stu-id="cd44e-141">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="cd44e-142">Först använde du ett standardkommando för att skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="cd44e-142">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="cd44e-143">Sedan använde du en uppsättning kommandon för att distribuera en resurs (i det här exemplet en webbapp) till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="cd44e-143">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="cd44e-144">Du kan enkelt samla de här kommandona i ett kommandoskript och köra det varje gång du behöver skapa samma resurs.</span><span class="sxs-lookup"><span data-stu-id="cd44e-144">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>