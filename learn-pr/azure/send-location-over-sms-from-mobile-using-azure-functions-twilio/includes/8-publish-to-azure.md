Appen och Azure-funktionen är nu färdiga och körs lokalt. Här får du publicera Azure-funktionen i Azure för att köra den i molnet.

> Här kommer du att publicera din funktion från Visual Studio. Det här är ett bra sätt att komma igång med konceptbeskrivningar, prototyper och utbildningar, men du bör **inte** använda den här metoden för produktionsappar. Du bör använda någon form av CI-baserad distributionen. Du kan läsa mer om hur du gör det i [dokumentationen om Azure Functions-distribution](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment).
>
> [![Riktiga vänner låter inte sina vänner publicera med högerklick](../media/8-friends-dont-let-friends-publish.png)](https://damianbrady.com.au/2018/02/01/friends-dont-let-friends-right-click-publish/)

## <a name="publishing-your-app-to-azure"></a>Publicera din app i Azure

Du kan publicera Azure-funktioner i Azure från Visual Studio.

1. Stoppa den lokala körningen av Azure Functions om det fortfarande körs sedan den föregående enheten.

2. Högerklicka på appen `ImHere.Functions` i lösningsutforskaren och välj *Publicera*.

    ![Högerklicka på Publicera i Functions-appen](../media/8-right-click-publish.png)

3. I dialogrutan **Välj ett publiceringsmål** väljer du *Azure-funktionsapp*, och för **Azure App Service** väljer du *Skapa ny*. Klicka på **Publicera**.

    ![Skapa en ny Azure App Service att publicera i](../media/8-pick-publish-target.png)

4. Välj ditt Azure-konto i listrutan uppe till höger om du har fler än ett Azure-konto och rätt inte redan är valt.

5. Ge din Functions-app ett namn. Det här namnet måste vara globalt unikt bland alla Functions-appar i hela Azure, så använd någonting i stil med ”ImHere -\<DittNamn\>”.

6. Välj den prenumeration som Functions-appen ska skapas under.

7. Skapa en ny resursgrupp för den här Functions-appen genom att klicka på knappen **Ny...**  bredvid listrutan **Resursgrupp** och ge den ett namn som ”ImHere”. Resursgruppnamn måste vara unika för din prenumeration, inte globalt i hela Azure. Klicka sedan på **OK**.

    ![Skapa en ny resursgrupp](../media/8-create-new-resource-group.png)

   När du skapar en ny resursgrupp blir det lättare att rensa resurserna senare. När du tar bort resursgruppen vet du att allt du skapat för den här Functions-appen tas bort samtidigt.

8. Skapa en ny värdplan genom att klicka på knappen **Ny...**  bredvid listrutan **Värdplan**. Namnet på App Service-planen är som standard appnamnet med ”Plan” i slutet. För **Plats** väljer du den plats som är närmast dig. Se till att **Storlek** är inställt på förbrukning. Klicka sedan på **OK**.

    ![Konfigurera värdplanen](../media/8-configure-hosting-plan.png)

9. Skapa ett nytt lagringskonto genom att klicka på knappen **Nytt...**  bredvid listrutan **Lagringskonto**. Ett standardnamn anges, så behåll alla standardvärden och klicka på **OK**.

    ![skapar ett lagringskonto](../media/8-create-storage-account.png)

10. Klicka på **Skapa** så etableras alla resurser i Azure och din Azure Functions-app publiceras.

    ![Skapa App Service-tjänsten](../media/8-create-app-service.png)

Etableringen kan ta några minuter att köra. Följande resurser etableras:

* ett lagringskonto för lagring av filerna som behövs i Azure Functions-appen
* en App Service-plan för hantering av beräkningsresurserna som behövs i Azure Functions-appen
* den App Service-tjänst som kör Azure-funktionen.

Funktionen publiceras nu och är tillgänglig för anrop på https://<ditt-appnamn>.azurewebsites.net/api/SendLocation.

## <a name="configuring-your-app"></a>Konfigurera din app

När Azure-funktionen kördes lokalt användes Twilio-autentiseringsuppgifter som lagrades i en `local.settings.json`-fil. Precis som namnet antyder är den här filen till för lokala inställningar, inte för Azure-inställningar. Innan du kan anropa Azure-funktionen i Azure måste du konfigurera inställningarna `TwilioAccountSid` och `TwilioAuthToken`.

1. Klicka på alternativet **Hantera programinställningar** på fliken Publicera.

    ![Alternativet Hantera programinställningar](../media/8-application-settings-option.png)

2. Klicka på knappen **Lägg till** för att lägga till en ny inställning. Ge den namnet ”TwilioAccountSid” och ange SID:et för ditt Twilio-konto som värde. Upprepa det här steget för din autentiseringstoken och använd namnet ”TwilioAuthToken”.

    ![Ställa in Twilio-autentiseringsuppgifter i programinställningarna](../media/8-set-creds-in-app-settings.png)

3. Klicka på **OK**.

4. Klicka på **Publicera** så att du publicerar om Azure Functions-appen med de nya programinställningarna.

    ![Knappen Publicera](../media/8-publish-application-button.png)

## <a name="pointing-the-mobile-app-to-azure"></a>Se till att mobilappen pekar på Azure

1. Kopiera **Webbplats-URL** från fliken Publicera med kopieringsknappen bredvid värdet.

    ![Kopiera webbplatsadressen från fliken Publicera](../media/8-copy-site-url.png)

2. Öppna `MainViewModel` från projektet `ImHere`.

3. Uppdatera värdet för fältet `baseUrl` med webbplatsadressen du kopierade från fliken Publicera.

4. Ändra protokollet för det här värdet från `http` till `https`. Webbplatsadressen anges alltid med HTTP, men du måste använda HTTPS när du ska anropa en Azure-funktion.

## <a name="test-it-out"></a>Testa den

1. Ange appen `ImHere.UWP` som startapp och kör den.

2. Ange ett telefonnummer och klicka på knappen **Send Location**.

3. Du bör få platsen som ett SMS-meddelande.

## <a name="summary"></a>Sammanfattning

Nu har du lärt dig hur du publicerar ett Azure Functions-projekt i Azure från Visual Studio och konfigurerar programinställningar.