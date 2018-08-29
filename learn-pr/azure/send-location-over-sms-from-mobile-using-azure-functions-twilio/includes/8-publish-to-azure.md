<span data-ttu-id="33ff2-101">Appen och Azure-funktionen är nu färdiga och körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="33ff2-101">The app and Azure function are now complete and running locally.</span></span> <span data-ttu-id="33ff2-102">Här får du publicera Azure-funktionen i Azure för att köra den i molnet.</span><span class="sxs-lookup"><span data-stu-id="33ff2-102">In this unit, you publish the Azure function to Azure to run in the cloud.</span></span>

> <span data-ttu-id="33ff2-103">Här kommer du att publicera din funktion från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33ff2-103">In this unit, you will publish your function from Visual Studio.</span></span> <span data-ttu-id="33ff2-104">Det här är ett bra sätt att komma igång med konceptbeskrivningar, prototyper och utbildningar, men du bör **inte** använda den här metoden för produktionsappar.</span><span class="sxs-lookup"><span data-stu-id="33ff2-104">This is a great way to get started for proof-of-concepts, prototypes, and learning, but for a production-quality app you should **not** use this method.</span></span> <span data-ttu-id="33ff2-105">Du bör använda någon form av CI-baserad distributionen.</span><span class="sxs-lookup"><span data-stu-id="33ff2-105">You should use some form of CI-based deployment.</span></span> <span data-ttu-id="33ff2-106">Du kan läsa mer om hur du gör det i [dokumentationen om Azure Functions-distribution](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment).</span><span class="sxs-lookup"><span data-stu-id="33ff2-106">You can read more about doing this in the [Azure Functions Deployment docs](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment).</span></span>
>
> <span data-ttu-id="33ff2-107">[![Riktiga vänner låter inte sina vänner publicera med högerklick](../media/8-friends-dont-let-friends-publish.png)](https://damianbrady.com.au/2018/02/01/friends-dont-let-friends-right-click-publish/)</span><span class="sxs-lookup"><span data-stu-id="33ff2-107">[![Friends don't let friends right-click publish](../media/8-friends-dont-let-friends-publish.png)](https://damianbrady.com.au/2018/02/01/friends-dont-let-friends-right-click-publish/)</span></span>

## <a name="publishing-your-app-to-azure"></a><span data-ttu-id="33ff2-108">Publicera din app i Azure</span><span class="sxs-lookup"><span data-stu-id="33ff2-108">Publishing your app to Azure</span></span>

<span data-ttu-id="33ff2-109">Du kan publicera Azure-funktioner i Azure från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33ff2-109">Azure functions can be published to Azure from inside Visual Studio.</span></span>

1. <span data-ttu-id="33ff2-110">Stoppa den lokala körningen av Azure Functions om det fortfarande körs sedan den föregående enheten.</span><span class="sxs-lookup"><span data-stu-id="33ff2-110">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

2. <span data-ttu-id="33ff2-111">Högerklicka på appen `ImHere.Functions` i lösningsutforskaren och välj *Publicera*.</span><span class="sxs-lookup"><span data-stu-id="33ff2-111">Right-click on the `ImHere.Functions` app in the solution explorer and select *Publish...*.</span></span>

    ![Högerklicka på Publicera i Functions-appen](../media/8-right-click-publish.png)

3. <span data-ttu-id="33ff2-113">I dialogrutan **Välj ett publiceringsmål** väljer du *Azure-funktionsapp*, och för **Azure App Service** väljer du *Skapa ny*.</span><span class="sxs-lookup"><span data-stu-id="33ff2-113">From the **Pick a publish target** dialog, select *Azure Function App*, and for **Azure App Service**, select *Create New*.</span></span> <span data-ttu-id="33ff2-114">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="33ff2-114">Click **Publish**.</span></span>

    ![Skapa en ny Azure App Service att publicera i](../media/8-pick-publish-target.png)

4. <span data-ttu-id="33ff2-116">Välj ditt Azure-konto i listrutan uppe till höger om du har fler än ett Azure-konto och rätt inte redan är valt.</span><span class="sxs-lookup"><span data-stu-id="33ff2-116">Select your Azure account from the drop-down in the top-right corner if you have more than one Azure accounts and the right one isn't selected.</span></span>

5. <span data-ttu-id="33ff2-117">Ge din Functions-app ett namn.</span><span class="sxs-lookup"><span data-stu-id="33ff2-117">Give your Functions app a name.</span></span> <span data-ttu-id="33ff2-118">Det här namnet måste vara globalt unikt bland alla Functions-appar i hela Azure, så använd någonting i stil med ”ImHere -\<DittNamn\>”.</span><span class="sxs-lookup"><span data-stu-id="33ff2-118">This name needs to be globally unique across all the Functions apps in the whole of Azure, so use something like "ImHere-\<YourName\>".</span></span>

6. <span data-ttu-id="33ff2-119">Välj den prenumeration som Functions-appen ska skapas under.</span><span class="sxs-lookup"><span data-stu-id="33ff2-119">Select the subscription you want to create this Functions app under.</span></span>

7. <span data-ttu-id="33ff2-120">Skapa en ny resursgrupp för den här Functions-appen genom att klicka på knappen **Ny...**  bredvid listrutan **Resursgrupp** och ge den ett namn som ”ImHere”.</span><span class="sxs-lookup"><span data-stu-id="33ff2-120">Create a new resource group for this Functions app by clicking the **New...** button next to the **Resource Group** drop-down and giving it a name such as "ImHere".</span></span> <span data-ttu-id="33ff2-121">Resursgruppnamn måste vara unika för din prenumeration, inte globalt i hela Azure.</span><span class="sxs-lookup"><span data-stu-id="33ff2-121">Resource group names need to be unique to your subscription, not globally unique across Azure.</span></span> <span data-ttu-id="33ff2-122">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="33ff2-122">Then, click **OK**.</span></span>

    ![Skapa en ny resursgrupp](../media/8-create-new-resource-group.png)

   <span data-ttu-id="33ff2-124">När du skapar en ny resursgrupp blir det lättare att rensa resurserna senare.</span><span class="sxs-lookup"><span data-stu-id="33ff2-124">Creating a new resource group makes it easier to clean up later.</span></span> <span data-ttu-id="33ff2-125">När du tar bort resursgruppen vet du att allt du skapat för den här Functions-appen tas bort samtidigt.</span><span class="sxs-lookup"><span data-stu-id="33ff2-125">You can delete the resource group and know that everything you've created for this Functions app will all be deleted at the same time.</span></span>

8. <span data-ttu-id="33ff2-126">Skapa en ny värdplan genom att klicka på knappen **Ny...**  bredvid listrutan **Värdplan**.</span><span class="sxs-lookup"><span data-stu-id="33ff2-126">Create a new hosting plan by clicking the **New...** button next to the **Hosting Plan** drop-down.</span></span> <span data-ttu-id="33ff2-127">Namnet på App Service-planen är som standard appnamnet med ”Plan” i slutet.</span><span class="sxs-lookup"><span data-stu-id="33ff2-127">The App Service plan name will default to your app name with "Plan" on the end.</span></span> <span data-ttu-id="33ff2-128">För **Plats** väljer du den plats som är närmast dig. Se till att **Storlek** är inställt på förbrukning.</span><span class="sxs-lookup"><span data-stu-id="33ff2-128">Set the **Location** to the closest location to you and make sure **Size** is set to consumption.</span></span> <span data-ttu-id="33ff2-129">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="33ff2-129">Then, click **OK**.</span></span>

    ![Konfigurera värdplanen](../media/8-configure-hosting-plan.png)

9. <span data-ttu-id="33ff2-131">Skapa ett nytt lagringskonto genom att klicka på knappen **Nytt...**  bredvid listrutan **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="33ff2-131">Create a new storage account by clicking the **New...** button next to the **Storage Account** drop-down.</span></span> <span data-ttu-id="33ff2-132">Ett standardnamn anges, så behåll alla standardvärden och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="33ff2-132">A default name will be provided, so keep all the default values and click **OK**.</span></span>

    ![skapar ett lagringskonto](../media/8-create-storage-account.png)

10. <span data-ttu-id="33ff2-134">Klicka på **Skapa** så etableras alla resurser i Azure och din Azure Functions-app publiceras.</span><span class="sxs-lookup"><span data-stu-id="33ff2-134">Click **Create** to provision all the resources on Azure and publish your Azure Functions app.</span></span>

    ![Skapa App Service-tjänsten](../media/8-create-app-service.png)

<span data-ttu-id="33ff2-136">Etableringen kan ta några minuter att köra.</span><span class="sxs-lookup"><span data-stu-id="33ff2-136">Provisioning will take a couple of minutes or so to run.</span></span> <span data-ttu-id="33ff2-137">Följande resurser etableras:</span><span class="sxs-lookup"><span data-stu-id="33ff2-137">The following resources will be provisioned:</span></span>

* <span data-ttu-id="33ff2-138">ett lagringskonto för lagring av filerna som behövs i Azure Functions-appen</span><span class="sxs-lookup"><span data-stu-id="33ff2-138">A storage account to store the files needed for the Azure Functions app</span></span>
* <span data-ttu-id="33ff2-139">en App Service-plan för hantering av beräkningsresurserna som behövs i Azure Functions-appen</span><span class="sxs-lookup"><span data-stu-id="33ff2-139">An App Service plan to manage the compute resources needed by the Azure Functions app</span></span>
* <span data-ttu-id="33ff2-140">den App Service-tjänst som kör Azure-funktionen.</span><span class="sxs-lookup"><span data-stu-id="33ff2-140">The App Service that runs the Azure function</span></span>

<span data-ttu-id="33ff2-141">Funktionen publiceras nu och är tillgänglig för anrop på https://<ditt-appnamn>.azurewebsites.net/api/SendLocation.</span><span class="sxs-lookup"><span data-stu-id="33ff2-141">The function will now be published and available to call at https://<your-app-name>.azurewebsites.net/api/SendLocation.</span></span>

## <a name="configuring-your-app"></a><span data-ttu-id="33ff2-142">Konfigurera din app</span><span class="sxs-lookup"><span data-stu-id="33ff2-142">Configuring your app</span></span>

<span data-ttu-id="33ff2-143">När Azure-funktionen kördes lokalt användes Twilio-autentiseringsuppgifter som lagrades i en `local.settings.json`-fil.</span><span class="sxs-lookup"><span data-stu-id="33ff2-143">When the Azure function was running locally, it was using Twilio credentials that were stored in a `local.settings.json` file.</span></span> <span data-ttu-id="33ff2-144">Precis som namnet antyder är den här filen till för lokala inställningar, inte för Azure-inställningar.</span><span class="sxs-lookup"><span data-stu-id="33ff2-144">As the name suggests, this file is for local settings, not Azure settings.</span></span> <span data-ttu-id="33ff2-145">Innan du kan anropa Azure-funktionen i Azure måste du konfigurera inställningarna `TwilioAccountSid` och `TwilioAuthToken`.</span><span class="sxs-lookup"><span data-stu-id="33ff2-145">Before the Azure function can be called inside Azure, the `TwilioAccountSid` and `TwilioAuthToken` settings need to be configured.</span></span>

1. <span data-ttu-id="33ff2-146">Klicka på alternativet **Hantera programinställningar** på fliken Publicera.</span><span class="sxs-lookup"><span data-stu-id="33ff2-146">From the Publish tab, click the **Manage Application Settings** option.</span></span>

    ![Alternativet Hantera programinställningar](../media/8-application-settings-option.png)

2. <span data-ttu-id="33ff2-148">Klicka på knappen **Lägg till** för att lägga till en ny inställning.</span><span class="sxs-lookup"><span data-stu-id="33ff2-148">Click the **Add** button to add a new setting.</span></span> <span data-ttu-id="33ff2-149">Ge den namnet ”TwilioAccountSid” och ange SID:et för ditt Twilio-konto som värde.</span><span class="sxs-lookup"><span data-stu-id="33ff2-149">Name it "TwilioAccountSid" and set the value to your Twilio account SID.</span></span> <span data-ttu-id="33ff2-150">Upprepa det här steget för din autentiseringstoken och använd namnet ”TwilioAuthToken”.</span><span class="sxs-lookup"><span data-stu-id="33ff2-150">Repeat this step for your Auth Token using the name "TwilioAuthToken".</span></span>

    ![Ställa in Twilio-autentiseringsuppgifter i programinställningarna](../media/8-set-creds-in-app-settings.png)

3. <span data-ttu-id="33ff2-152">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="33ff2-152">Click **OK**.</span></span>

4. <span data-ttu-id="33ff2-153">Klicka på **Publicera** så att du publicerar om Azure Functions-appen med de nya programinställningarna.</span><span class="sxs-lookup"><span data-stu-id="33ff2-153">Click **Publish** to republish the Azure Functions app with the new application settings.</span></span>

    ![Knappen Publicera](../media/8-publish-application-button.png)

## <a name="pointing-the-mobile-app-to-azure"></a><span data-ttu-id="33ff2-155">Se till att mobilappen pekar på Azure</span><span class="sxs-lookup"><span data-stu-id="33ff2-155">Pointing the mobile app to Azure</span></span>

1. <span data-ttu-id="33ff2-156">Kopiera **Webbplats-URL** från fliken Publicera med kopieringsknappen bredvid värdet.</span><span class="sxs-lookup"><span data-stu-id="33ff2-156">From the Publish tab, copy the **Site URL** using the copy button next to the value.</span></span>

    ![Kopiera webbplatsadressen från fliken Publicera](../media/8-copy-site-url.png)

2. <span data-ttu-id="33ff2-158">Öppna `MainViewModel` från projektet `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="33ff2-158">Open the `MainViewModel` from the `ImHere` project.</span></span>

3. <span data-ttu-id="33ff2-159">Uppdatera värdet för fältet `baseUrl` med webbplatsadressen du kopierade från fliken Publicera.</span><span class="sxs-lookup"><span data-stu-id="33ff2-159">Update the value of the `baseUrl` field to be the site URL copied from the Publish tab.</span></span>

4. <span data-ttu-id="33ff2-160">Ändra protokollet för det här värdet från `http` till `https`.</span><span class="sxs-lookup"><span data-stu-id="33ff2-160">Change the protocol for this value from `http` to `https`.</span></span> <span data-ttu-id="33ff2-161">Webbplatsadressen anges alltid med HTTP, men du måste använda HTTPS när du ska anropa en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="33ff2-161">The site URL is always given using HTTP, but you have to use HTTPS to call an Azure function.</span></span>

## <a name="test-it-out"></a><span data-ttu-id="33ff2-162">Testa den</span><span class="sxs-lookup"><span data-stu-id="33ff2-162">Test it out</span></span>

1. <span data-ttu-id="33ff2-163">Ange appen `ImHere.UWP` som startapp och kör den.</span><span class="sxs-lookup"><span data-stu-id="33ff2-163">Set the `ImHere.UWP` app as the startup app and run it.</span></span>

2. <span data-ttu-id="33ff2-164">Ange ett telefonnummer och klicka på knappen **Send Location**.</span><span class="sxs-lookup"><span data-stu-id="33ff2-164">Enter a phone number and click the **Send Location** button.</span></span>

3. <span data-ttu-id="33ff2-165">Du bör få platsen som ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="33ff2-165">You should receive the location as an SMS message.</span></span>

## <a name="summary"></a><span data-ttu-id="33ff2-166">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="33ff2-166">Summary</span></span>

<span data-ttu-id="33ff2-167">Nu har du lärt dig hur du publicerar ett Azure Functions-projekt i Azure från Visual Studio och konfigurerar programinställningar.</span><span class="sxs-lookup"><span data-stu-id="33ff2-167">In this unit, you learned how to publish an Azure Functions project to Azure from inside Visual Studio and configure application settings.</span></span>