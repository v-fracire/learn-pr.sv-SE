<span data-ttu-id="ebf99-101">Appen och Azure-funktionen är nu färdiga och körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="ebf99-101">The app and Azure Function are now complete and running locally.</span></span> <span data-ttu-id="ebf99-102">Här publicerar du funktionen i Azure för att köra den i molnet.</span><span class="sxs-lookup"><span data-stu-id="ebf99-102">In this unit, you publish the function to Azure to run in the cloud.</span></span>

> [!Note]
> <span data-ttu-id="ebf99-103">Du publicerar din funktion från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ebf99-103">You will publish your function from Visual Studio.</span></span> <span data-ttu-id="ebf99-104">Det här är ett bra sätt att komma igång med konceptbeskrivningar, prototyper och utbildningar, men du bör **inte** använda den här metoden för produktionsappar.</span><span class="sxs-lookup"><span data-stu-id="ebf99-104">This is a great way to get started for proof-of-concepts, prototypes, and learning, but for a production-quality app you should **not** use this method.</span></span> <span data-ttu-id="ebf99-105">Du bör använda någon form av CI-baserad distributionen.</span><span class="sxs-lookup"><span data-stu-id="ebf99-105">You should use some form of CI-based deployment.</span></span> <span data-ttu-id="ebf99-106">Du kan läsa mer om hur du gör det i [dokumentationen om Azure Functions-distribution](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="ebf99-106">You can read more about doing this in the [Azure Functions Deployment docs](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true).</span></span>

## <a name="publishing-your-app-to-azure"></a><span data-ttu-id="ebf99-107">Publicera din app i Azure</span><span class="sxs-lookup"><span data-stu-id="ebf99-107">Publishing your app to Azure</span></span>

<span data-ttu-id="ebf99-108">Du kan publicera Azure-funktioner i Azure från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ebf99-108">Azure functions can be published to Azure from inside Visual Studio.</span></span>

1. <span data-ttu-id="ebf99-109">Stoppa den lokala körningen av Azure Functions om det fortfarande körs sedan den föregående enheten.</span><span class="sxs-lookup"><span data-stu-id="ebf99-109">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

1. <span data-ttu-id="ebf99-110">Högerklicka på appen `ImHere.Functions` i lösningsutforskaren och välj *Publicera*.</span><span class="sxs-lookup"><span data-stu-id="ebf99-110">Right-click on the `ImHere.Functions` app in the solution explorer and select *Publish...*.</span></span>

    ![Högerklicka på Publicera i Functions-appen](../media/8-right-click-publish.png)

1. <span data-ttu-id="ebf99-112">I dialogrutan **Välj ett publiceringsmål** väljer du *Azure-funktionsapp*, och för **Azure App Service** väljer du *Skapa ny*.</span><span class="sxs-lookup"><span data-stu-id="ebf99-112">From the **Pick a publish target** dialog, select *Azure Function App*, and for **Azure App Service**, select *Create New*.</span></span> <span data-ttu-id="ebf99-113">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="ebf99-113">Click **Publish**.</span></span>

    ![Skapa en ny Azure App Service att publicera i](../media/8-pick-publish-target.png)

1. <span data-ttu-id="ebf99-115">Logga in på Azure med användarnamnet och lösenordet på fliken **Resurser** i dessa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="ebf99-115">Sign in to Azure using the username and password in the **Resources** tab of these instructions.</span></span> <span data-ttu-id="ebf99-116">Om du skapar den här appen lokalt i stället för med hjälp av den virtuella datorn ska du logga in med ditt Azure-konto. Du kan skapa ett nytt om det behövs med hjälp av länkarna i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ebf99-116">If you are building this app locally instead of using the VM, log in with your Azure account, creating a new one if necessary using the links on the dialog.</span></span>

1. <span data-ttu-id="ebf99-117">Lämna alla värden enligt standard, eftersom detta skapar all nödvändig infrastruktur för att köra funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="ebf99-117">Leave all the values as the defaults, as this will create all the necessary infrastructure to run your Functions app.</span></span>

1. <span data-ttu-id="ebf99-118">Klicka på **Skapa** så etableras alla resurser i Azure och din Azure Functions-app publiceras.</span><span class="sxs-lookup"><span data-stu-id="ebf99-118">Click **Create** to provision all the resources on Azure and publish your Azure Functions app.</span></span>

    ![Skapa App Service-tjänsten](../media/8-create-app-service.png)

1. <span data-ttu-id="ebf99-120">Du kan bli tillfrågad om du vill uppdatera funktionsversionen i Azure.</span><span class="sxs-lookup"><span data-stu-id="ebf99-120">You may be asked if you want to update the functions version on Azure.</span></span> <span data-ttu-id="ebf99-121">Om den här dialogrutan visas väljer du **Ja** för att se till att funktionsappen publiceras med den senaste Azure Functions-körningsversionen.</span><span class="sxs-lookup"><span data-stu-id="ebf99-121">If this dialog appears, select **Yes** to ensure your function app is published with the latest Azure Functions runtime version.</span></span>
    <span data-ttu-id="ebf99-122">![Dialogrutan för uppdatering av Azure Functions](../media/8-update-functions-on-azure.png)</span><span class="sxs-lookup"><span data-stu-id="ebf99-122">![The update Azure Functions dialog](../media/8-update-functions-on-azure.png)</span></span>

<span data-ttu-id="ebf99-123">Etableringen kan ta några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="ebf99-123">Provisioning will take a couple of minutes to complete.</span></span> <span data-ttu-id="ebf99-124">Följande resurser etableras:</span><span class="sxs-lookup"><span data-stu-id="ebf99-124">The following resources will be provisioned:</span></span>

- <span data-ttu-id="ebf99-125">ett lagringskonto för lagring av filerna som behövs i Azure Functions-appen</span><span class="sxs-lookup"><span data-stu-id="ebf99-125">A storage account to store the files needed for the Azure Functions app</span></span>
- <span data-ttu-id="ebf99-126">en App Service-plan för hantering av beräkningsresurserna som behövs i Azure Functions-appen</span><span class="sxs-lookup"><span data-stu-id="ebf99-126">An App Service plan to manage the compute resources needed by the Azure Functions app</span></span>
- <span data-ttu-id="ebf99-127">den App Service-tjänst som kör Azure-funktionen</span><span class="sxs-lookup"><span data-stu-id="ebf99-127">The App Service that runs the Azure function</span></span>

<span data-ttu-id="ebf99-128">Funktionen publiceras nu och är tillgänglig för anrop på **https://\<ditt-appnamn\>.azurewebsites.net/api/SendLocation**.</span><span class="sxs-lookup"><span data-stu-id="ebf99-128">The function will now be published and available to call at **https://\<your-app-name\>.azurewebsites.net/api/SendLocation**.</span></span>

## <a name="configuring-your-app"></a><span data-ttu-id="ebf99-129">Konfigurera din app</span><span class="sxs-lookup"><span data-stu-id="ebf99-129">Configuring your app</span></span>

<span data-ttu-id="ebf99-130">När Azure-funktionen kördes lokalt användes Twilio-autentiseringsuppgifter som lagrades i en `local.settings.json`-fil.</span><span class="sxs-lookup"><span data-stu-id="ebf99-130">When the Azure function was running locally, it was using Twilio credentials that were stored in a `local.settings.json` file.</span></span> <span data-ttu-id="ebf99-131">Precis som namnet antyder är den här filen till för lokala inställningar, inte för Azure-inställningar.</span><span class="sxs-lookup"><span data-stu-id="ebf99-131">As the name suggests, this file is for local settings, not Azure settings.</span></span> <span data-ttu-id="ebf99-132">Innan du kan anropa Azure-funktionen i Azure måste du konfigurera inställningarna `TwilioAccountSid` och `TwilioAuthToken`.</span><span class="sxs-lookup"><span data-stu-id="ebf99-132">Before the Azure function can be called inside Azure, the `TwilioAccountSid` and `TwilioAuthToken` settings need to be configured.</span></span>

1. <span data-ttu-id="ebf99-133">Klicka på alternativet **Hantera programinställningar** på fliken Publicera.</span><span class="sxs-lookup"><span data-stu-id="ebf99-133">From the Publish tab, click the **Manage Application Settings** option.</span></span>

    ![Alternativet Hantera programinställningar](../media/8-application-settings-option.png)

1. <span data-ttu-id="ebf99-135">I dialogrutan **Programinställningar** visas programinställningar med ett värde för både lokalt och fjärranslutet – det lokala kommer från din `local.settings.json`-fil och fjärrvärdet är det din funktion använder när den ligger i Azure.</span><span class="sxs-lookup"><span data-stu-id="ebf99-135">The **Application Settings** dialog will show application settings with both a local and remote value - the local coming from your `local.settings.json` file, and the remote value is the one your function will use when it is hosted in Azure.</span></span> <span data-ttu-id="ebf99-136">Kopiera värdena från rutorna *Lokal* till *Fjärr* för värdena **TwilioAccountSid** och **TwilioAuthToken**.</span><span class="sxs-lookup"><span data-stu-id="ebf99-136">Copy the values from the *Local* to the *Remote* boxes for the **TwilioAccountSid** and **TwilioAuthToken** values.</span></span>

    ![Ställa in Twilio-autentiseringsuppgifter i programinställningarna](../media/8-set-creds-in-app-settings.png)

1. <span data-ttu-id="ebf99-138">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ebf99-138">Click **OK**.</span></span> <span data-ttu-id="ebf99-139">I och med detta publiceras värdena i Azure-funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="ebf99-139">This will publish the values to the Azure functions app.</span></span>

## <a name="pointing-the-mobile-app-to-azure"></a><span data-ttu-id="ebf99-140">Se till att mobilappen pekar på Azure</span><span class="sxs-lookup"><span data-stu-id="ebf99-140">Pointing the mobile app to Azure</span></span>

1. <span data-ttu-id="ebf99-141">Kopiera **Webbplats-URL** från fliken Publicera med knappen **Kopiera till Urklipp** bredvid värdet.</span><span class="sxs-lookup"><span data-stu-id="ebf99-141">From the Publish tab, copy the **Site URL** using the **Copy to clipboard** button next to the value.</span></span>

    ![Kopiera webbplatsadressen från fliken Publicera](../media/8-copy-site-url.png)

1. <span data-ttu-id="ebf99-143">Öppna `MainViewModel` från projektet `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="ebf99-143">Open the `MainViewModel` from the `ImHere` project.</span></span>

1. <span data-ttu-id="ebf99-144">Uppdatera värdet för fältet `baseUrl` med webbplatsadressen du kopierade från fliken Publicera.</span><span class="sxs-lookup"><span data-stu-id="ebf99-144">Update the value of the `baseUrl` field to be the site URL copied from the Publish tab.</span></span>

## <a name="test-it-out"></a><span data-ttu-id="ebf99-145">Testa den</span><span class="sxs-lookup"><span data-stu-id="ebf99-145">Test it out</span></span>

1. <span data-ttu-id="ebf99-146">Ange appen `ImHere.UWP` som startapp och kör den.</span><span class="sxs-lookup"><span data-stu-id="ebf99-146">Set the `ImHere.UWP` app as the startup app and run it.</span></span>

1. <span data-ttu-id="ebf99-147">Ange ett telefonnummer och klicka på knappen **Send Location**.</span><span class="sxs-lookup"><span data-stu-id="ebf99-147">Enter a phone number and click the **Send Location** button.</span></span>

1. <span data-ttu-id="ebf99-148">Du bör få platsen som ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ebf99-148">You should receive the location as an SMS message.</span></span>

1. <span data-ttu-id="ebf99-149">Om du får felet *Tjänsten är inte tillgänglig* ska du kontrollera vilken version av ”Microsoft.Azure.WebJobs.Extensions.Twilio” NuGet-paketet som funktionsappen använder, som ska vara 3.0.0-rc1, INTE 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="ebf99-149">If you get back a *Service Unavailable* error, check what version of the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package your functions app is using, it should be 3.0.0-rc1, NOT 3.0.0.</span></span>
1. <span data-ttu-id="ebf99-150">Om du får felet *Tjänsten är inte tillgänglig* ska du kontrollera vilken version av ”Microsoft.Azure.WebJobs.Extensions.Twilio” NuGet-paketet som funktionsappen använder, som ska vara 3.0.0-rc1, **INTE** 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="ebf99-150">If you get back a *Service Unavailable* error, check what version of the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package your functions app is using, it should be 3.0.0-rc1, **NOT** 3.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="ebf99-151">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ebf99-151">Summary</span></span>

<span data-ttu-id="ebf99-152">Nu har du lärt dig hur du publicerar ett Azure Functions-projekt i Azure från Visual Studio och konfigurerar programinställningar.</span><span class="sxs-lookup"><span data-stu-id="ebf99-152">In this unit, you learned how to publish an Azure Functions project to Azure from inside Visual Studio and configure application settings.</span></span>