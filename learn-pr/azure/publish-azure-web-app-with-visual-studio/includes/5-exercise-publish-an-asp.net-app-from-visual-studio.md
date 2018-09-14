Nu har du ett ASP.NET Core-webbprogram som körs lokalt. I den här enheten publicerar du programmet till Azure App Service.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="visual-studio-for-windows"></a>Visual Studio för Windows

1. Öppna Solution Explorer, högerklicka på projektet och välj **Publicera**.

1. I dialogrutan som visas väljer du **App Service** som publiceringsmål.  Till höger, Välj **Skapa ny** att skapa en App Service-app.

1. Klicka på den **publicera** för att skapa din app.

> [!NOTE]
> När du distribuerar appar, kan du skapa en ny App Service-plan eller fortsätta att lägga till appar i en befintlig plan. I den här övningen skapar du en app i **Azure App Service**.

### <a name="visual-studio-mac"></a>Visual Studio Mac

1. Öppna Solution Explorer och högerklicka på ditt projekt.

1. Välj **Publicera**.

1. Klicka på **Publicera till Azure**. Då ansluts ditt Azure-konto. Det kan ta några minuter att ansluta och visa dina aktuella tjänster.

1. Klicka på **New** att skapa en app.

## <a name="configure-your-new-azure-app-service"></a>Konfigurera din nya Azure App Service

1. I dialogrutan **Skapa App Service** klickar du på **Lägg till ett konto** och loggar in på din Azure-prenumeration. Välj kontot som innehåller den önskade prenumerationen i listrutan om du redan är inloggad.

1. Ange nödvändig information om din App Service-plan.

    ![Skapa App Service](../media-draft/5-CreateAppService.png)

    I dialogrutan **Skapa App Service** anger du följande information:

    - **Appnamn**: det här är namnet på ditt program.  Detta avgör URL:en för det publicerade programmet, som blir https://_AppName_.azurewebsites.net.  Det här måste vara ett unikt värde. Därför kan du behöva prova några olika namn för att hitta ett som inte har reserverats.

    - **Prenumeration**: Azure-prenumeration som du vill distribuera appen till.

    - **Resursgrupp:** väljer du den befintliga <rgn>[Sandbox resursgruppens namn]</rgn> resursgrupp.

    - **Värdplan**: värdplanen anger plats, storlek och funktioner för den webbservergrupp som är värd för din app. Du kan välja en befintlig värdplan eller skapa en ny. I den här övningen ska du skapa en.

        Klicka på knappen **Ny...**  intill värdplanen.

        ![Ny värdplan](../media-draft/5-NewHostingPlan.png)

        Namnge värdplanen och ange sedan plats och storlek.  
        
        > [!NOTE]
        > Om du anger olika platser och storlekar påverkar det kostnaden för tjänsten. För den här övningen rekommenderar vi att du anger storleken **Kostnadsfri**, vilket säkerställer att det inte medför några kostnader.

    - **Application Insights:** anger om du vill använda Application Insights för programmet. För den här övningen rekommenderar vi att du väljer **Ingen.**

1. Klicka på den **skapa** knappen för att börja etablera din app. Förloppet visas allt eftersom den distribueras:

    ![Distribuering pågår](../media-draft/5-DeployProgress.png)

1. När appen har etablerats ska Visual Studio skapa och distribuera din programkod.  I utdata från Visual Studio-kompileringen kan du se resultatet av distributionen.

    ![Publiceringsresultat](../media-draft/5-PublishResult.png)

1. Grattis! Ditt ASP.NET Core-webbprogram har publiceras och är nu live. Du kommer att kunna se den slutliga URL:en för webbplatsen i byggets utdata och även på publiceringssidan i Visual Studio.

    ![Publiceringsresultat](../media-draft/5-PublishPage.png)

1. Du kan testa webbplatsen genom att gå till den angivna webbadressen.

    ![Live-webbplats](../media-draft/5-WebPageLive.png)

## <a name="summary"></a>Sammanfattning

Den här övningen visas hur du etablerar en app i Azure App Service och distribuera en ASP.NET Core-webbapp. Nu har du en webbapp som är live.
