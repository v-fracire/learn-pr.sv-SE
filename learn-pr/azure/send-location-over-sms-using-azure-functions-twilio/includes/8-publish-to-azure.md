Appen och Azure-funktionen är nu färdiga och körs lokalt. Här publicerar du funktionen i Azure för att köra den i molnet.

> [!Note]
> Du publicerar din funktion från Visual Studio. Det här är ett bra sätt att komma igång med konceptbeskrivningar, prototyper och utbildningar, men du bör **inte** använda den här metoden för produktionsappar. Du bör använda någon form av CI-baserad distributionen. Du kan läsa mer om hur du gör det i [dokumentationen om Azure Functions-distribution](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true).

## <a name="publishing-your-app-to-azure"></a>Publicera din app i Azure

Du kan publicera Azure-funktioner i Azure från Visual Studio.

1. Stoppa den lokala körningen av Azure Functions om det fortfarande körs sedan den föregående enheten.

1. Högerklicka på appen `ImHere.Functions` i lösningsutforskaren och välj *Publicera*.

    ![Högerklicka på Publicera i Functions-appen](../media/8-right-click-publish.png)

1. I dialogrutan **Välj ett publiceringsmål** väljer du *Azure-funktionsapp*, och för **Azure App Service** väljer du *Skapa ny*. Klicka på **Publicera**.

    ![Skapa en ny Azure App Service att publicera i](../media/8-pick-publish-target.png)

1. Logga in på Azure med användarnamnet och lösenordet på fliken **Resurser** i dessa instruktioner. Om du skapar den här appen lokalt i stället för med hjälp av den virtuella datorn ska du logga in med ditt Azure-konto. Du kan skapa ett nytt om det behövs med hjälp av länkarna i dialogrutan.

1. Lämna alla värden enligt standard, eftersom detta skapar all nödvändig infrastruktur för att köra funktionsappen.

1. Klicka på **Skapa** så etableras alla resurser i Azure och din Azure Functions-app publiceras.

    ![Skapa App Service-tjänsten](../media/8-create-app-service.png)

1. Du kan bli tillfrågad om du vill uppdatera funktionsversionen i Azure. Om den här dialogrutan visas väljer du **Ja** för att se till att funktionsappen publiceras med den senaste Azure Functions-körningsversionen.
    ![Dialogrutan för uppdatering av Azure Functions](../media/8-update-functions-on-azure.png)

Etableringen kan ta några minuter att slutföra. Följande resurser etableras:

- ett lagringskonto för lagring av filerna som behövs i Azure Functions-appen
- en App Service-plan för hantering av beräkningsresurserna som behövs i Azure Functions-appen
- den App Service-tjänst som kör Azure-funktionen

Funktionen publiceras nu och är tillgänglig för anrop på **https://\<ditt-appnamn\>.azurewebsites.net/api/SendLocation**.

## <a name="configuring-your-app"></a>Konfigurera din app

När Azure-funktionen kördes lokalt användes Twilio-autentiseringsuppgifter som lagrades i en `local.settings.json`-fil. Precis som namnet antyder är den här filen till för lokala inställningar, inte för Azure-inställningar. Innan du kan anropa Azure-funktionen i Azure måste du konfigurera inställningarna `TwilioAccountSid` och `TwilioAuthToken`.

1. Klicka på alternativet **Hantera programinställningar** på fliken Publicera.

    ![Alternativet Hantera programinställningar](../media/8-application-settings-option.png)

1. I dialogrutan **Programinställningar** visas programinställningar med ett värde för både lokalt och fjärranslutet – det lokala kommer från din `local.settings.json`-fil och fjärrvärdet är det din funktion använder när den ligger i Azure. Kopiera värdena från rutorna *Lokal* till *Fjärr* för värdena **TwilioAccountSid** och **TwilioAuthToken**.

    ![Ställa in Twilio-autentiseringsuppgifter i programinställningarna](../media/8-set-creds-in-app-settings.png)

1. Klicka på **OK**. I och med detta publiceras värdena i Azure-funktionsappen.

## <a name="pointing-the-mobile-app-to-azure"></a>Se till att mobilappen pekar på Azure

1. Kopiera **Webbplats-URL** från fliken Publicera med knappen **Kopiera till Urklipp** bredvid värdet.

    ![Kopiera webbplatsadressen från fliken Publicera](../media/8-copy-site-url.png)

1. Öppna `MainViewModel` från projektet `ImHere`.

1. Uppdatera värdet för fältet `baseUrl` med webbplatsadressen du kopierade från fliken Publicera.

## <a name="test-it-out"></a>Testa den

1. Ange appen `ImHere.UWP` som startapp och kör den.

1. Ange ett telefonnummer och klicka på knappen **Send Location**.

1. Du bör få platsen som ett SMS-meddelande.

1. Om du får felet *Tjänsten är inte tillgänglig* ska du kontrollera vilken version av ”Microsoft.Azure.WebJobs.Extensions.Twilio” NuGet-paketet som funktionsappen använder, som ska vara 3.0.0-rc1, INTE 3.0.0.
1. Om du får felet *Tjänsten är inte tillgänglig* ska du kontrollera vilken version av ”Microsoft.Azure.WebJobs.Extensions.Twilio” NuGet-paketet som funktionsappen använder, som ska vara 3.0.0-rc1, **INTE** 3.0.0.

## <a name="summary"></a>Sammanfattning

Nu har du lärt dig hur du publicerar ett Azure Functions-projekt i Azure från Visual Studio och konfigurerar programinställningar.