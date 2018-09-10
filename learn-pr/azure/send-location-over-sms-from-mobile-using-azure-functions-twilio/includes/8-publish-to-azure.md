Appen och Azure-funktionen är nu färdiga och körs lokalt. Här får du publicera funktionen i Azure för att köra den i molnet.

> Här kommer du att publicera din funktion från Visual Studio. Det här är ett bra sätt att komma igång med konceptbeskrivningar, prototyper och utbildningar, men du bör **inte** använda den här metoden för produktionsappar. Du bör använda någon form av CI-baserad distributionen. Du kan läsa mer om hur du gör det i [dokumentationen om Azure Functions-distribution](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment).
>

## <a name="publishing-your-app-to-azure"></a>Publicera din app i Azure

Du kan publicera Azure-funktioner i Azure från Visual Studio.

1. Stoppa den lokala körningen av Azure Functions om det fortfarande körs sedan den föregående enheten.

1. Högerklicka på appen `ImHere.Functions` i lösningsutforskaren och välj *Publicera*.

    ![Högerklicka på Publicera i Functions-appen](../media-drafts/8-right-click-publish.png)

1. I dialogrutan **Välj ett publiceringsmål** väljer du *Azure-funktionsapp*, och för **Azure App Service** väljer du *Skapa ny*. Klicka på **Publicera**.

    ![Skapa en ny Azure App Service att publicera till](../media-drafts/8-pick-publish-target.png)

1. Välj ditt Azure-konto i listrutan längst uppe till höger om du har fler än ett Azure-konto och det rätta kontot inte redan är valt.

1. Ge din Functions-app ett namn. Det här namnet måste vara globalt unikt bland alla Functions-appar i hela Azure, så använd någonting i stil med ”ImHere -\<DittNamn\>”.

1. Välj den prenumeration som Functions-appen ska skapas under.

1. Skapa en ny resursgrupp för den här Functions-appen genom att klicka på knappen **Ny...**  bredvid listrutan **Resursgrupp** och ge den ett namn som ”ImHere”. Resursgruppnamn måste vara unika för din prenumeration, inte globalt i hela Azure. Klicka sedan på **OK**.

    ![Skapa en ny resursgrupp](../media-drafts/8-create-new-resource-group.png)

   När du skapar en ny resursgrupp blir det lättare att rensa resurserna senare. När du tar bort resursgruppen vet du att allt du skapat för den här Functions-appen tas bort samtidigt.

1. Skapa en ny värdplan genom att klicka på knappen **Ny...**  bredvid listrutan **Värdplan**. Namnet på App Service-planen är som standard appnamnet med ”Plan” i slutet. För **Plats** väljer du den plats som är närmast dig. Se till att **Storlek** är inställt på förbrukning. Klicka sedan på **OK**.

    ![Konfigurera värdplanen](../media-drafts/8-configure-hosting-plan.png)

1. Skapa ett nytt lagringskonto genom att klicka på knappen **Nytt...**  bredvid listrutan **Lagringskonto**. Ett standardnamn anges, så behåll alla standardvärden och klicka på **OK**.

    ![Skapa ett lagringskonto](../media-drafts/8-create-storage-account.png)

1. Klicka på **Skapa** så etableras alla resurser i Azure och din Azure Functions-app publiceras.

    ![Skapa App Service-tjänsten](../media-drafts/8-create-app-service.png)

Etableringen kan ta några minuter att köra. Följande resurser etableras:

- ett lagringskonto för lagring av filerna som behövs i Azure Functions-appen
- en App Service-plan för hantering av beräkningsresurserna som behövs i Azure Functions-appen
- den App Service-tjänst som kör Azure-funktionen.

Funktionen publiceras nu och är tillgänglig för anrop på https://<ditt-appnamn>.azurewebsites.net/api/SendLocation.

## <a name="configuring-your-app"></a>Konfigurera din app

När Azure-funktionen kördes lokalt användes Twilio-autentiseringsuppgifter som lagrades i en `local.settings.json`-fil. Precis som namnet antyder är den här filen till för lokala inställningar, inte för Azure-inställningar. Innan du kan anropa Azure-funktionen i Azure måste du konfigurera inställningarna `TwilioAccountSid` och `TwilioAuthToken`.

1. Klicka på alternativet **Hantera programinställningar** på fliken Publicera.

    ![Alternativet Hantera programinställningar](../media-drafts/8-application-settings-option.png)

1. Klicka på knappen **Lägg till** för att lägga till en ny inställning. Ge den namnet ”TwilioAccountSid” och ange SID:et för ditt Twilio-konto som värde. Upprepa det här steget för din autentiseringstoken och använd namnet ”TwilioAuthToken”.

    ![Ställa in Twilio-autentiseringsuppgifter i programinställningarna](../media-drafts/8-set-creds-in-app-settings.png)

1. Klicka på **OK**.

1. Klicka på **Publicera** så att du publicerar om Azure Functions-appen med de nya programinställningarna.

    ![Knappen Publicera](../media-drafts/8-publish-application-button.png)

## <a name="pointing-the-mobile-app-to-azure"></a>Se till att mobilappen pekar på Azure

1. Kopiera **Webbplats-URL** från fliken Publicera med kopieringsknappen bredvid värdet.

    ![Kopiera webbplatsadressen från fliken Publicera](../media-drafts/8-copy-site-url.png)

1. Öppna `MainViewModel` från projektet `ImHere`.

1. Uppdatera värdet för fältet `baseUrl` med webbplatsadressen du kopierade från fliken Publicera.

1. Ändra protokollet för det här värdet från `http` till `https`. Webbplatsadressen anges alltid med HTTP, men du måste använda HTTPS när du ska anropa en Azure-funktion.

## <a name="test-it-out"></a>Testa den

1. Ange appen `ImHere.UWP` som startapp och kör den.

1. Ange ett telefonnummer och klicka på knappen **Send Location**.

1. Du bör få platsen som ett SMS-meddelande.

## <a name="summary"></a>Sammanfattning

Nu har du lärt dig hur du publicerar ett Azure Functions-projekt i Azure från Visual Studio och konfigurerar programinställningar.