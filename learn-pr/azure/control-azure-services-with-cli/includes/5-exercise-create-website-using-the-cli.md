<span data-ttu-id="d3275-101">Nu ska vi använda Azure CLI för att skapa en resursgrupp och sedan distribuera en webbapp i den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d3275-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span>

<span data-ttu-id="d3275-102">Du kan använda Azure CLI som du har installerat med din egen Azure-prenumeration, men det finns en kostnadsfri sandbox-miljö som är tillgänglig för den här övningen med Azure Cloud Shell där Azure CLI redan är installerat.</span><span class="sxs-lookup"><span data-stu-id="d3275-102">You can use the Azure CLI you installed with your own Azure subscription, but there is a free sandbox environment available for this exercise with Azure Cloud Shell where the Azure CLI is already installed.</span></span> <span data-ttu-id="d3275-103">Följande instruktioner gäller om du använder den kostnadsfria sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="d3275-103">The following instructions assume you will be using the free sandbox.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="d3275-104">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d3275-104">Create a resource group</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

> [!NOTE]
> <span data-ttu-id="d3275-105">Normalt sett skulle du skapa en resursgrupp för alla dina relaterade Azure-resurser med ett `az group create ...`-kommando, men för de här övningarna har en resursgrupp redan skapats åt dig.</span><span class="sxs-lookup"><span data-stu-id="d3275-105">Normally, you would create a resource group for all your related Azure resources with an `az group create ...` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="d3275-106">Den här resursgruppens namn kommer att användas för kommandona längre fram i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="d3275-106">This resource group name will be injected into the later commands in this exercise.</span></span>

<!-- TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well. -->

<!-- 1. Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows. -->

<!-- 1. Start the Azure CLI and run the login command.

    ```azurecli
    az login
    ```
    If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin). -->

<!-- 1. Create a resource group.

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ``` -->

1. <span data-ttu-id="d3275-107">Kontrollera att resursgruppen har skapats genom att visa alla resursgrupper i en tabell.</span><span class="sxs-lookup"><span data-stu-id="d3275-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```azurecli
    az group list --output table
    ```

    <span data-ttu-id="d3275-108">Du kanske bara kan se den `<rgn>[Sandbox resource group name]</rgn>` resursgrupp som skapades för den kostnadsfria sandbox-användningen.</span><span class="sxs-lookup"><span data-stu-id="d3275-108">You may only see the `<rgn>[Sandbox resource group name]</rgn>` resource group that was generated for your free sandbox usage.</span></span>

<!-- > [!TIP]
> You can also confirm the resource was created in the Azure portal. Open a web browser, sign in to the portal and navigate to the **Resource Groups** section. The new resource group should be displayed in the list. -->

1. <span data-ttu-id="d3275-109">I takt med att du utvecklar mer i Azure kan du ha många resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="d3275-109">As you do more Azure development, you can end up with several resource groups.</span></span> <span data-ttu-id="d3275-110">Om det finns flera objekt i grupplistan kan du filtrera returvärdena genom att lägga till ett `--query`-alternativ.</span><span class="sxs-lookup"><span data-stu-id="d3275-110">If you have several items in the group list, you can filter the return values by adding a `--query` option.</span></span> <span data-ttu-id="d3275-111">Testa följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d3275-111">Try this command:</span></span>

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    <span data-ttu-id="d3275-112">Frågan är formaterad med **JMESPath**, som är ett standardfrågespråk för JSON-begäranden.</span><span class="sxs-lookup"><span data-stu-id="d3275-112">The query is formated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="d3275-113">Du kan läsa mer om det här kraftfulla filterspråket på <http://jmespath.org/>.</span><span class="sxs-lookup"><span data-stu-id="d3275-113">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="d3275-114">Vi tar även upp frågor mer ingående i modulen **Hantera virtuella datorer med Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="d3275-114">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="d3275-115">Steg för att skapa en serviceplan</span><span class="sxs-lookup"><span data-stu-id="d3275-115">Steps to create a service plan</span></span>

<span data-ttu-id="d3275-116">När du kör webbappar med Azure App Service betalar du för de Azure-beräkningsresurser som används av apparna, och beloppet beror på den App Service-plan som är associerad med webbapparna.</span><span class="sxs-lookup"><span data-stu-id="d3275-116">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="d3275-117">Serviceplaner avgör vilken region som används för appens datacenter, hur många virtuella datorer som används och prisnivån.</span><span class="sxs-lookup"><span data-stu-id="d3275-117">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="d3275-118">Skapa en App Service-plan för att köra din app.</span><span class="sxs-lookup"><span data-stu-id="d3275-118">Create an App Service plan to run your app.</span></span> <span data-ttu-id="d3275-119">Följande kommando anger inte någon prisnivå eller information om VM-instansen, så som standard får du en plan på **Basic**-nivå med en **Liten** VM-instans.</span><span class="sxs-lookup"><span data-stu-id="d3275-119">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="d3275-120">Namnet på appen och planen måste vara _unika_. Lägg därför till ett suffix till namnet och ersätt texten `<unique>` i kommandot nedan med några siffror, dina initialer eller en textsträng så att namnet blir unikt i hela Azure.</span><span class="sxs-lookup"><span data-stu-id="d3275-120">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span>

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --location eastus
    ```

    <span data-ttu-id="d3275-121">Det här kommandot kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="d3275-121">This command can take several minutes to complete.</span></span>

1. <span data-ttu-id="d3275-122">Kontrollera att serviceplanen har skapats genom att visa alla dina planer i en tabell.</span><span class="sxs-lookup"><span data-stu-id="d3275-122">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="d3275-123">Steg för att skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="d3275-123">Steps to create a web app</span></span>

<span data-ttu-id="d3275-124">Nu ska vi skapa webbappen i din serviceplan.</span><span class="sxs-lookup"><span data-stu-id="d3275-124">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="d3275-125">Du kan distribuera koden på samma gång, men i vårt exempel gör vi det i separata steg.</span><span class="sxs-lookup"><span data-stu-id="d3275-125">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="d3275-126">Skapa webbappen och ange namnet på planen som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="d3275-126">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="d3275-127">**Precis som planen måste appnamnet vara unikt. Ersätt därför markören `<unique>` med en textsträng så att namnet blir unikt globalt.**</span><span class="sxs-lookup"><span data-stu-id="d3275-127">**Just like the plan, the app name must be unique, replace the `<unique>` marker with some text to make the name globally unique.**</span></span>
    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group popupResGroup --plan popupappplan-<unique>
    ```

1. <span data-ttu-id="d3275-128">Kontrollera att appen har skapats genom att visa alla dina appar i en tabell.</span><span class="sxs-lookup"><span data-stu-id="d3275-128">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```azurecli
    az webapp list --output table
    ```

1. <span data-ttu-id="d3275-129">Anteckna **DefaultHostName**, du behöver det här värdet senare.</span><span class="sxs-lookup"><span data-stu-id="d3275-129">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="d3275-130">Steg för att distribuera kod från GitHub</span><span class="sxs-lookup"><span data-stu-id="d3275-130">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="d3275-131">Det sista steget är att distribuera kod från en GitHub-lagringsplats till webbappen.</span><span class="sxs-lookup"><span data-stu-id="d3275-131">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="d3275-132">Vi ska använda en enkel PHP-sida som är tillgänglig på Github-lagringsplatsen för Azure-exempel och som visar ”HelloWorld!”</span><span class="sxs-lookup"><span data-stu-id="d3275-132">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="d3275-133">när den körs.</span><span class="sxs-lookup"><span data-stu-id="d3275-133">when it executes.</span></span> <span data-ttu-id="d3275-134">Var noga med att använda namnet på webbappen som du skapat.</span><span class="sxs-lookup"><span data-stu-id="d3275-134">Make sure to use the web app name you created.</span></span>

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="d3275-135">När den har distribuerats blir din webbplats tillgänglig via det unika appnamnet i domänen `azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d3275-135">Once it's deployed, Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="d3275-136">Om appnamnet till exempel är ”popupapp mslearn123” blir webbadressen: <http://popupapp-mslearn123.azurewebsites.net>.</span><span class="sxs-lookup"><span data-stu-id="d3275-136">For example, if my app name was "popupapp-mslearn123", then my website address would be: <http://popupapp-mslearn123.azurewebsites.net>.</span></span> <span data-ttu-id="d3275-137">Du måste ange rätt URL för att komma till din specifika instans.</span><span class="sxs-lookup"><span data-stu-id="d3275-137">You will need to type in the correct URL to hit your specific instance.</span></span>

1. <span data-ttu-id="d3275-138">”Hello World!” visas på sidan.</span><span class="sxs-lookup"><span data-stu-id="d3275-138">The page displays "Hello World!"</span></span>

1. <span data-ttu-id="d3275-139">Stäng webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="d3275-139">Close the browser window.</span></span>

<span data-ttu-id="d3275-140">I den här övningen fick du se ett vanligt mönster för en interaktiv Azure CLI-session.</span><span class="sxs-lookup"><span data-stu-id="d3275-140">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="d3275-141">Först använde du ett standardkommando för att skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d3275-141">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="d3275-142">Sedan använde du en uppsättning kommandon för att distribuera en resurs (i det här exemplet en webbapp) till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d3275-142">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="d3275-143">Du kan enkelt samla de här kommandona i ett kommandoskript och köra det varje gång du behöver skapa samma resurs.</span><span class="sxs-lookup"><span data-stu-id="d3275-143">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
