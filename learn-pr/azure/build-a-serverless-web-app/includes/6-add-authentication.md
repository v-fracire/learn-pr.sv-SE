<span data-ttu-id="0886d-101">Med Azure App Service-autentisering kan du använda nyckelfärdig autentisering i en Azure Functions-app.</span><span class="sxs-lookup"><span data-stu-id="0886d-101">Azure App Service authentication enables turn-key authentication support in an Azure Functions app.</span></span> <span data-ttu-id="0886d-102">Autentiseringen integreras sömlöst med Facebook, Twitter, Microsoft-konton, Google och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0886d-102">It integrates seamlessly with Facebook, Twitter, Microsoft accounts, Google, and Azure Active Directory.</span></span> <span data-ttu-id="0886d-103">Du lägger till App Service-autentisering för att skydda serverdels-API:erna för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="0886d-103">You'll add App Service authentication to protect the back-end APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="0886d-104">Aktivera App Service-autentisering</span><span class="sxs-lookup"><span data-stu-id="0886d-104">Enable App Service authentication</span></span>

1. <span data-ttu-id="0886d-105">Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="0886d-105">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="0886d-106">Öppna funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="0886d-106">Open the function app.</span></span>

1. <span data-ttu-id="0886d-107">Välj **Autentisering/auktorisering** under **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="0886d-107">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Välja autentisering och auktorisering](../media/6-authorization.jpg)

1. <span data-ttu-id="0886d-109">Välj följande värden:</span><span class="sxs-lookup"><span data-stu-id="0886d-109">Select the following values:</span></span>

    | <span data-ttu-id="0886d-110">Inställning</span><span class="sxs-lookup"><span data-stu-id="0886d-110">Setting</span></span>      |  <span data-ttu-id="0886d-111">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="0886d-111">Suggested value</span></span>   | <span data-ttu-id="0886d-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0886d-112">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="0886d-113">**App Service-autentisering**</span><span class="sxs-lookup"><span data-stu-id="0886d-113">**App Service Authentication**</span></span> | <span data-ttu-id="0886d-114">På</span><span class="sxs-lookup"><span data-stu-id="0886d-114">On</span></span> | <span data-ttu-id="0886d-115">Aktivera autentisering.</span><span class="sxs-lookup"><span data-stu-id="0886d-115">Enable authentication.</span></span> |
    | <span data-ttu-id="0886d-116">**Åtgärd när begäran inte har autentiserats**</span><span class="sxs-lookup"><span data-stu-id="0886d-116">**Action when request is not authenticated**</span></span> | <span data-ttu-id="0886d-117">Logga in med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0886d-117">Sign in with Azure Active Directory.</span></span> | <span data-ttu-id="0886d-118">Välj en konfigurerad autentiseringsmetod (se nedan).</span><span class="sxs-lookup"><span data-stu-id="0886d-118">Select a configured authentication method (See below).</span></span> |
    | <span data-ttu-id="0886d-119">**Autentiseringsprovidrar**</span><span class="sxs-lookup"><span data-stu-id="0886d-119">**Authentication Providers**</span></span> | <span data-ttu-id="0886d-120">Se nedan.</span><span class="sxs-lookup"><span data-stu-id="0886d-120">See below.</span></span> | <span data-ttu-id="0886d-121">Se nedan.</span><span class="sxs-lookup"><span data-stu-id="0886d-121">See below.</span></span> |
    | <span data-ttu-id="0886d-122">**Tokenarkiv**</span><span class="sxs-lookup"><span data-stu-id="0886d-122">**Token store**</span></span> | <span data-ttu-id="0886d-123">På</span><span class="sxs-lookup"><span data-stu-id="0886d-123">On</span></span> | <span data-ttu-id="0886d-124">Tillåt att App Service lagrar och hanterar token.</span><span class="sxs-lookup"><span data-stu-id="0886d-124">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="0886d-125">**Tillåtna externa omdirigeringswebbadresser**</span><span class="sxs-lookup"><span data-stu-id="0886d-125">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="0886d-126">Webbadressen för ditt program, till exempel https://firstserverlessweb.z4.web.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="0886d-126">The URL of your application, for example https://firstserverlessweb.z4.web.core.windows.net/.</span></span> | <span data-ttu-id="0886d-127">Webbadresser som App Service kan omdirigera till när en användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="0886d-127">URLs that App Service is allowed to redirect to, after a user is authenticated.</span></span> |

1. <span data-ttu-id="0886d-128">Visa **Azure Active Directory-inställningar** genom att välja **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0886d-128">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="0886d-129">Välj **Express** som **Hanteringsläge** och fyll i informationen nedan.</span><span class="sxs-lookup"><span data-stu-id="0886d-129">Select **Express** as the **Management Mode** and fill in the following information.</span></span>

        | <span data-ttu-id="0886d-130">Inställning</span><span class="sxs-lookup"><span data-stu-id="0886d-130">Setting</span></span>      |  <span data-ttu-id="0886d-131">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="0886d-131">Suggested value</span></span>   | <span data-ttu-id="0886d-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0886d-132">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="0886d-133">**Hanteringsläge**</span><span class="sxs-lookup"><span data-stu-id="0886d-133">**Management mode**</span></span> | <span data-ttu-id="0886d-134">Express, Skapa ny AD-app</span><span class="sxs-lookup"><span data-stu-id="0886d-134">Express, Create new AD app</span></span> | <span data-ttu-id="0886d-135">Konfigurera automatiskt ett huvudnamn för tjänsten och Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0886d-135">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="0886d-136">**Skapa app**</span><span class="sxs-lookup"><span data-stu-id="0886d-136">**Create app**</span></span> | <span data-ttu-id="0886d-137">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="0886d-137">my-serverless-webapp</span></span> | <span data-ttu-id="0886d-138">Ange ett unikt programnamn.</span><span class="sxs-lookup"><span data-stu-id="0886d-138">Enter a unique application name.</span></span> |

    1. <span data-ttu-id="0886d-139">Spara Azure Active Directory-inställningarna genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0886d-139">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Inställningar för autentisering, auktorisering och Azure Active Directory](../media/6-create-aad.png)

1. <span data-ttu-id="0886d-141">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0886d-141">Click **Save**.</span></span>

## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="0886d-142">Ändra webbappen för att aktivera autentisering</span><span class="sxs-lookup"><span data-stu-id="0886d-142">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="0886d-143">Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella katalogen.</span><span class="sxs-lookup"><span data-stu-id="0886d-143">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="0886d-144">Du aktiverar autentisering i din funktionsapp genom att ändra **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="0886d-144">You enable authentication in your function app by modifying **settings.js**.</span></span> <span data-ttu-id="0886d-145">Öppna filen i Cloud Shell-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="0886d-145">Open the file in Cloud Shell Editor.</span></span>

    ```azurecli
    code settings.js
    ```

1. <span data-ttu-id="0886d-146">Lägg till följande rad i filen och spara den.</span><span class="sxs-lookup"><span data-stu-id="0886d-146">Append the following line to the file and save it.</span></span>

    ```azurecli
    window.authEnabled = true
    ```

1. <span data-ttu-id="0886d-147">Bekräfta att ändringen har gjorts i filen.</span><span class="sxs-lookup"><span data-stu-id="0886d-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="0886d-148">Ladda upp filen till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="0886d-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload \
        -c \$web \
        --account-name <storage account name> \
        -f settings.js \
        -n settings.js
    ```

## <a name="test-the-application"></a><span data-ttu-id="0886d-149">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="0886d-149">Test the application</span></span>

1. <span data-ttu-id="0886d-150">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0886d-150">Open the application in a browser.</span></span> <span data-ttu-id="0886d-151">Klicka på **Logga in** och logga in.</span><span class="sxs-lookup"><span data-stu-id="0886d-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="0886d-152">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="0886d-152">Select an image file and upload it.</span></span>

    ![Inloggningssida](../media/6-aad-auth.png)

## <a name="summary"></a><span data-ttu-id="0886d-154">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0886d-154">Summary</span></span>

<span data-ttu-id="0886d-155">I den här kursdelen har du lärt dig hur du lägger till autentisering i programmet med hjälp av Azure App Service-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0886d-155">In this unit, you learned how to add authentication to the application using Azure App Service authentication.</span></span>