<span data-ttu-id="b2f90-101">Med Azure App Service-autentisering kan du använda nyckelfärdig autentisering i en Azure Functions-app.</span><span class="sxs-lookup"><span data-stu-id="b2f90-101">Azure App Service Authentication enables turn-key authentication support in an Azure Function app.</span></span> <span data-ttu-id="b2f90-102">Autentiseringen integreras sömlöst med Facebook, Twitter, Microsoft Account, Google och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b2f90-102">It integrates seamlessly with Facebook, Twitter, Microsoft Account, Google, and Azure Active Directory.</span></span> <span data-ttu-id="b2f90-103">Du lägger till App Service-autentisering för att skydda serverdels-API:erna för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="b2f90-103">You'll add App Service Authentication to protect the back-end APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="b2f90-104">Aktivera App Service-autentisering</span><span class="sxs-lookup"><span data-stu-id="b2f90-104">Enable App Service Authentication</span></span>

1. <span data-ttu-id="b2f90-105">Öppna funktionsappen på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b2f90-105">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="b2f90-106">Välj **Autentisering/auktorisering** under **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="b2f90-106">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Välj Autentisering/auktorisering](../images/6-authorization.jpg)

1. <span data-ttu-id="b2f90-108">Välj följande värden:</span><span class="sxs-lookup"><span data-stu-id="b2f90-108">Select the following values:</span></span>
    
    | <span data-ttu-id="b2f90-109">Inställning</span><span class="sxs-lookup"><span data-stu-id="b2f90-109">Setting</span></span>      |  <span data-ttu-id="b2f90-110">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="b2f90-110">Suggested value</span></span>   | <span data-ttu-id="b2f90-111">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b2f90-111">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="b2f90-112">**App Service-autentisering**</span><span class="sxs-lookup"><span data-stu-id="b2f90-112">**App Service Authentication**</span></span> | <span data-ttu-id="b2f90-113">På</span><span class="sxs-lookup"><span data-stu-id="b2f90-113">On</span></span> | <span data-ttu-id="b2f90-114">Aktivera autentisering.</span><span class="sxs-lookup"><span data-stu-id="b2f90-114">Enable authentication.</span></span> |
    | <span data-ttu-id="b2f90-115">**Åtgärd när begäran inte har autentiserats**</span><span class="sxs-lookup"><span data-stu-id="b2f90-115">**Action when request is not authenticated**</span></span> | <span data-ttu-id="b2f90-116">Logga in med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b2f90-116">Log in with Azure Active Directory</span></span> | <span data-ttu-id="b2f90-117">Välj en konfigurerad autentiseringsmetod (nedan).</span><span class="sxs-lookup"><span data-stu-id="b2f90-117">Select a configured authentication method (below).</span></span> |
    | <span data-ttu-id="b2f90-118">**Autentiseringsprovidrar**</span><span class="sxs-lookup"><span data-stu-id="b2f90-118">**Authentication Providers**</span></span> | <span data-ttu-id="b2f90-119">Se nedan</span><span class="sxs-lookup"><span data-stu-id="b2f90-119">See below</span></span> | <span data-ttu-id="b2f90-120">Se nedan</span><span class="sxs-lookup"><span data-stu-id="b2f90-120">See below</span></span> |
    | <span data-ttu-id="b2f90-121">**Tokenarkiv**</span><span class="sxs-lookup"><span data-stu-id="b2f90-121">**Token store**</span></span> | <span data-ttu-id="b2f90-122">På</span><span class="sxs-lookup"><span data-stu-id="b2f90-122">On</span></span> | <span data-ttu-id="b2f90-123">Tillåt att App Service lagrar och hanterar token.</span><span class="sxs-lookup"><span data-stu-id="b2f90-123">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="b2f90-124">**Tillåtna externa omdirigeringswebbadresser**</span><span class="sxs-lookup"><span data-stu-id="b2f90-124">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="b2f90-125">URL:en för ditt program, till exempel: https://firstserverlessweb.z4.web.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="b2f90-125">The URL of your application, for example: https://firstserverlessweb.z4.web.core.windows.net/</span></span> | <span data-ttu-id="b2f90-126">URL:er som App Service kan omdirigera till när en användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="b2f90-126">URL(s) that App Service is allowed to redirect to after a user is authenticated.</span></span> |

1. <span data-ttu-id="b2f90-127">Visa **Azure Active Directory-inställningar** genom att välja **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b2f90-127">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="b2f90-128">Välj **Express** som **Hanteringsläge** och fyll i informationen nedan.</span><span class="sxs-lookup"><span data-stu-id="b2f90-128">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="b2f90-129">Inställning</span><span class="sxs-lookup"><span data-stu-id="b2f90-129">Setting</span></span>      |  <span data-ttu-id="b2f90-130">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="b2f90-130">Suggested value</span></span>   | <span data-ttu-id="b2f90-131">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b2f90-131">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="b2f90-132">**Hanteringsläge**</span><span class="sxs-lookup"><span data-stu-id="b2f90-132">**Management mode**</span></span> | <span data-ttu-id="b2f90-133">Express, Skapa ny AD-app</span><span class="sxs-lookup"><span data-stu-id="b2f90-133">Express, Create new AD app</span></span> | <span data-ttu-id="b2f90-134">Konfigurera automatiskt ett huvudnamn för tjänsten och Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b2f90-134">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="b2f90-135">**Skapa app**</span><span class="sxs-lookup"><span data-stu-id="b2f90-135">**Create app**</span></span> | <span data-ttu-id="b2f90-136">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="b2f90-136">my-serverless-webapp</span></span> | <span data-ttu-id="b2f90-137">Ange ett unikt programnamn.</span><span class="sxs-lookup"><span data-stu-id="b2f90-137">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="b2f90-138">Spara Azure Active Directory-inställningarna genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b2f90-138">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Autentisering/auktorisering och Azure Active Directory-inställningar](../images/6-create-aad.png)

1. <span data-ttu-id="b2f90-140">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b2f90-140">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="b2f90-141">Ändra webbappen för att aktivera autentisering</span><span class="sxs-lookup"><span data-stu-id="b2f90-141">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="b2f90-142">Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella mappen.</span><span class="sxs-lookup"><span data-stu-id="b2f90-142">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="b2f90-143">Du aktiverar autentisering i funktionsappen genom att lägga till följande kodrad i **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="b2f90-143">To enable authentication in your function app, append the following line of code to **settings.js**.</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="b2f90-144">Gör ändringen och visa resultatet genom att köra följande kommandon eller genom att använda en kommandoradsredigerare som VIM.</span><span class="sxs-lookup"><span data-stu-id="b2f90-144">Make the change and view the result by using the following commands or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="b2f90-145">Bekräfta att ändringen har gjorts i filen.</span><span class="sxs-lookup"><span data-stu-id="b2f90-145">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="b2f90-146">Ladda upp filen till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="b2f90-146">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="b2f90-147">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="b2f90-147">Test the application</span></span>

1. <span data-ttu-id="b2f90-148">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b2f90-148">Open the application in a browser.</span></span> <span data-ttu-id="b2f90-149">Klicka på **Logga in** och logga in.</span><span class="sxs-lookup"><span data-stu-id="b2f90-149">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="b2f90-150">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="b2f90-150">Select an image file and upload it.</span></span>

    ![Inloggningssida](../images/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="b2f90-152">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b2f90-152">Summary</span></span>

<span data-ttu-id="b2f90-153">I den här kursdelen har du lärt dig hur du lägger till autentisering i programmet med hjälp av Azure App Service-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b2f90-153">In this unit, you learned how to add authentication to the applcation using Azure App Service Authentication.</span></span>