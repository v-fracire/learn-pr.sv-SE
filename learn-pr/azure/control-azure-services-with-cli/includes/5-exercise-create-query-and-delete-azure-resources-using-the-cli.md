
<span data-ttu-id="abe41-101">I den här övningen använder du Azure CLI lokalt till att skapa en resursgrupp och distribuerar sedan en webbapp till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="abe41-101">In this exercise, you will use the Azure CLI on your local machine to create a resource group, and then to deploy a web app into this resource group.</span></span> 

### <a name="steps-to-create-a-resource-group"></a><span data-ttu-id="abe41-102">Steg för att skapar en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="abe41-102">Steps to create a resource group</span></span>
1. <span data-ttu-id="abe41-103">Öppna ett bash-skal i Linux eller macOS, eller öppna Kommandotolken eller PowerShell om du arbetar i Windows.</span><span class="sxs-lookup"><span data-stu-id="abe41-103">Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows.</span></span>

1. <span data-ttu-id="abe41-104">Starta Azure CLI och kör inloggningskommandot.</span><span class="sxs-lookup"><span data-stu-id="abe41-104">Start the Azure CLI and run the login command.</span></span>

    ```bash
    az login
    ```
    <span data-ttu-id="abe41-105">Om du inte ser en inloggningssida för Azure i webbläsaren följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span><span class="sxs-lookup"><span data-stu-id="abe41-105">If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

1. <span data-ttu-id="abe41-106">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="abe41-106">Create a resource group.</span></span>

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. <span data-ttu-id="abe41-107">Kontrollera att resursgruppen har skapats genom att visa alla resursgrupper i en tabell.</span><span class="sxs-lookup"><span data-stu-id="abe41-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```bash
    az group list --output table
    ```
1. <span data-ttu-id="abe41-108">Du kan också kontrollera att resursen har skapats i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="abe41-108">Optionally, confirm the resource was created in the Azure portal.</span></span> <span data-ttu-id="abe41-109">Öppna en webbläsare, logga in i portalen och gå till avsnittet **Resursgrupper** (se nedan).</span><span class="sxs-lookup"><span data-stu-id="abe41-109">Open a web browser, sign in to the portal and navigate to the **Resource Groups** section (see below).</span></span> <span data-ttu-id="abe41-110">Den nya resursgruppen ska visas i listan.</span><span class="sxs-lookup"><span data-stu-id="abe41-110">The new resource group should be displayed in the list.</span></span>

![Använda portalen till att visa resursgrupper](../media-drafts/5-listing-resource-groups.png)

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="abe41-112">Steg för att skapa en serviceplan</span><span class="sxs-lookup"><span data-stu-id="abe41-112">Steps to create a service plan</span></span>
<span data-ttu-id="abe41-113">När du kör webbappar med Azure App Service betalar du för de Azure-beräkningsresurser som används av apparna, och beloppet beror på den App Service-plan som är associerad med webbapparna.</span><span class="sxs-lookup"><span data-stu-id="abe41-113">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="abe41-114">Serviceplaner avgör vilken region som används för appens datacenter, hur många virtuella datorer som används och prisnivån.</span><span class="sxs-lookup"><span data-stu-id="abe41-114">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="abe41-115">Skapa en App Service-plan för att köra din app.</span><span class="sxs-lookup"><span data-stu-id="abe41-115">Create an App Service plan to run your app.</span></span> <span data-ttu-id="abe41-116">Namnen på appen och planen måste vara unika, så ändra strängen ”12345” till ett slumptal.</span><span class="sxs-lookup"><span data-stu-id="abe41-116">The name of the app and plan must be unique, so change the string "12345" to a random number.</span></span> <span data-ttu-id="abe41-117">Följande kommando anger inte någon prisnivå eller information om VM-instansen, så som standard får du en plan på **Basic**-nivå med en **Liten** VM-instans.</span><span class="sxs-lookup"><span data-stu-id="abe41-117">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    ```bash
    az appservice plan create --name popupapp12345 --resource-group popupResGroup --location westeurope
    ```

1. <span data-ttu-id="abe41-118">Kontrollera att serviceplanen har skapats genom att visa alla planer i en tabell.</span><span class="sxs-lookup"><span data-stu-id="abe41-118">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```bash
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="abe41-119">Steg för att skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="abe41-119">Steps to create a web app</span></span>
<span data-ttu-id="abe41-120">Nu ska vi skapa webbappen i din serviceplan.</span><span class="sxs-lookup"><span data-stu-id="abe41-120">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="abe41-121">Du kan distribuera koden på samma gång, men i vårt exempel gör vi det i separata steg.</span><span class="sxs-lookup"><span data-stu-id="abe41-121">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="abe41-122">Skapa webbappen, och kom ihåg att ändra strängen ”12345” till samma slumptal som du använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="abe41-122">Create the web app, remembering to change the string "12345" to the same random number you used earlier.</span></span>
    ```bash
    az webapp create --name popupapp12345 --resource-group popupResGroup --plan popupapp12345
    ```

1. <span data-ttu-id="abe41-123">Kontrollera att appen har skapats genom att visa alla appar i en tabell.</span><span class="sxs-lookup"><span data-stu-id="abe41-123">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```bash
    az webapp list --output table
    ```

1. <span data-ttu-id="abe41-124">Anteckna **DefaultHostName**, du behöver det här värdet senare.</span><span class="sxs-lookup"><span data-stu-id="abe41-124">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="abe41-125">Steg för att distribuera kod från GitHub</span><span class="sxs-lookup"><span data-stu-id="abe41-125">Steps to deploy code from GitHub</span></span>
1. <span data-ttu-id="abe41-126">Det sista steget är att distribuera kod från en GitHub-repo till webbappen. Glöm inte att ändra strängen ”12345” till samma slumptal som du använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="abe41-126">The final step is to deploy code from a GitHub repository to the web app, again remembering to change the string "12345" to the same random number you used earlier.</span></span>
    ```bash
    az webapp deployment source config --name popupapp12345 --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="abe41-127">Kopiera följande webbadress till en webbläsare så att du ser webbappen.</span><span class="sxs-lookup"><span data-stu-id="abe41-127">Copy the following url into a browser to see the web app.</span></span>
<span data-ttu-id="abe41-128">http://popupapp12345.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="abe41-128">http://popupapp12345.azurewebsites.net</span></span>

1. <span data-ttu-id="abe41-129">Du ser ”HelloWorld!” på sidan</span><span class="sxs-lookup"><span data-stu-id="abe41-129">The page displays "HelloWorld!"</span></span>

1. <span data-ttu-id="abe41-130">Stäng webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="abe41-130">Close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="abe41-131">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="abe41-131">Summary</span></span>
<span data-ttu-id="abe41-132">I den här övningen fick du se ett vanligt mönster för en interaktiv Azure CLI-session.</span><span class="sxs-lookup"><span data-stu-id="abe41-132">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="abe41-133">Först använde du ett standardkommando för att skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="abe41-133">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="abe41-134">Sedan använde du en uppsättning kommandon för att distribuera en resurs (i det här exemplet en webbapp) till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="abe41-134">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="abe41-135">Du kan enkelt samla de här kommandona i ett kommandoskript och köra det varje gång du behöver skapa samma resurs.</span><span class="sxs-lookup"><span data-stu-id="abe41-135">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
