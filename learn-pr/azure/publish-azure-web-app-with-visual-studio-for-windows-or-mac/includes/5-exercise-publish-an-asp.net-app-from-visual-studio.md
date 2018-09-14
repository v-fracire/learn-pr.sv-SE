Nu har du ett ASP.NET Core-webbprogram som körs lokalt. I den här övningen publicerar du programmet i Azure App Service.

### <a name="visual-studio-for-windows"></a>Visual Studio för Windows

1. Öppna Solution Explorer, högerklicka på projektet och välj **Publicera**.

1. I dialogrutan som visas väljer du **App Service** som publiceringsmål.  Till höger väljer du **Skapa ny** för att skapa en ny App Service.

1. Klicka på knappen **Publicera** för att skapa din nya App Service.

> [!NOTE]
> När du distribuerar apparna kan du skapa en ny App Service-plan, eller så kan du fortsätta att lägga till appar i en befintlig plan. I den här övningen skapar du en ny **Azure App Service**.

### <a name="visual-studio-mac"></a>Visual Studio Mac

1. Öppna Solution Explorer och högerklicka på ditt projekt.

1. Välj **Publicera**.

1. Klicka på **Publicera till Azure**. Då ansluts ditt Azure-konto. Det kan ta några minuter att ansluta och visa dina aktuella tjänster.

1. Klicka på **Ny** för att skapa en ny App Service.

## <a name="configure-your-new-azure-app-service"></a>Konfigurera din nya Azure App Service

1. I dialogrutan **Skapa App Service** klickar du på **Lägg till ett konto** och loggar in på din Azure-prenumeration. Välj kontot som innehåller den önskade prenumerationen i listrutan om du redan är inloggad.

1. Ange nödvändig information om din App Service-plan.

    ![Skapa App Service](../media-draft/5-CreateAppService.png)

    I dialogrutan **Skapa App Service** anger du följande information:

    - **Appnamn**: det här är namnet på ditt program.  Detta avgör URL:en för det publicerade programmet, som blir https://_AppName_.azurewebsites.net.  Det här måste vara ett unikt värde. Därför kan du behöva prova några olika namn för att hitta ett som inte har reserverats.

    - **Prenumeration**: den Azure-prenumeration du vill distribuera din App Service till.

    - **Resursgrupp**: klicka på knappen **Ny...** bredvid resursgruppen och ange ett unikt namn för resursgruppen.

        ![Ny resursgrupp](../media-draft/5-NewResourceGroup.png)

    - **Värdplan**: värdplanen anger plats, storlek och funktioner för den webbservergrupp som är värd för din app. Du kan välja en befintlig värdplan eller skapa en ny. För den här övningen skapar du en ny.

        Klicka på knappen **Ny...**  intill värdplanen.

        ![Ny värdplan](../media-draft/5-NewHostingPlan.png)

        Namnge värdplanen och ange sedan plats och storlek.  
        
        > [!NOTE]
        > Om du anger olika platser och storlekar påverkar det kostnaden för tjänsten. För den här övningen rekommenderar vi att du anger storleken **Kostnadsfri**, vilket säkerställer att det inte medför några kostnader.

    - **Application Insights:** anger om du vill använda Application Insights för programmet. För den här övningen rekommenderar vi att du väljer **Ingen.**

1. Klicka på knappen **Skapa** för att börja etablera din App Service. Förloppet visas allt eftersom den distribueras:

    ![Distribuering pågår](../media-draft/5-DeployProgress.png)

1. När din App Service har etablerats skapar och distribuerar Visual Studio webbappen.  I utdata från Visual Studio-kompileringen kan du se resultatet av distributionen.

    ![Publiceringsresultat](../media-draft/5-PublishResult.png)

1. Grattis! Ditt ASP.NET Core-webbprogram har publiceras och är nu live. Du kan se URL:en för webbplatsen i utdata från kompileringen och även på publiceringssidan i Visual Studio.

    ![Publiceringsresultat](../media-draft/5-PublishPage.png)

1. Du kan testa webbplatsen genom att gå till den angivna webbadressen.

    ![Live-webbplats](../media-draft/5-WebPageLive.png)

## <a name="summary"></a>Sammanfattning

Den här övningen visade hur du etablerar en Azure App Service och distribuerar ett ASP.NET Core-webbprogram. Nu har du en webbapp som är live.
