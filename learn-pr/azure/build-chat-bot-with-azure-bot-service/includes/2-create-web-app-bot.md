När du ska skapa en robot är det första steget att hitta en lämplig värdplats för roboten i Azure. [Web Apps](https://azure.microsoft.com/services/app-service/web/)-funktionen i Azure App Service passar perfekt som värd för robotprogram och Azure Bot Service är specialutformat för att tillhandahålla dem. I den här enheten ska du använda Azure-portalen för att tillhandahålla en Azure Web App-robot.

<!---TODO: Update for sandbox?--->
1. Öppna [Azure-portalen](https://portal.azure.com/?azure-portal=true) i webbläsaren. Om du ombeds logga in ska du göra det med autentiseringsuppgifterna för ditt Microsoft-konto.

1. Klicka på **+ Skapa en resurs**, följt av **AI + maskininlärning** och sedan **Web App-robot**.

    ![Skärmbild av Azure-portalen som visar skapa ett Resursblad med Web App-robot resurstypen markerat.](../media/2-new-bot-service.png)

1. Ange ett namn, till exempel ”qa-factbot” i rutan **Appnamn**. *Det här namnet måste vara unikt inom Azure så se noga till så att en grön bock visas bredvid det.* Välj **Skapa ny** under **Resursgrupp** och ge resursgruppen namnet ”factbot-rg”. Välj den plats som ligger närmast dig och välj den kostnadsfria prisnivån **F0**. Klicka sedan på **Robotmall**.

1. Välj **Node.js** som SDK-språk och **Fråga och svar** som malltyp. Klicka sedan på **Välj** längst ned på bladet.

    ![Skärmbild av Azure-portalen som visar bladet Bot mallen i skapandeprocessen bot med Node.js SDK språk och frågor och svar mallalternativ markerat.](../media/2-portal-select-template.png)

1. Klicka nu på **App Service-plan/-plats** följt av **Skapa ny** och skapa sedan en App Service-plan med namnet ”qa-factbot-service-plan” eller liknande i samma region som du valde i steg 3. När det är klart klickar du på **Skapa** längst ned på bladet Web App-robot för att starta distributionen.

    ![Skärmbild av Azure-portalen som visar ett exempel configuration blad för en ny Web App-robot.](../media/2-portal-start-bot-creation.png)

1. Klicka på **Resursgrupper** i menyfliksområdet som visas till vänster i portalen. Klicka sedan på **factbot-rg** för att öppna resursgruppen som skapades för Azure Web App-roboten. Vänta tills statusen Distribuerar ändras till Lyckades högst upp på bladet. Det är en indikation som visar att distributionen av Azure Web App-roboten har slutförts. Distributionen tar generellt sätt två minuter eller mindre. Klicka med jämna mellanrum på **Uppdatera** överst på bladet för att uppdatera distributionens status.

Mycket inträffade bakom kulisserna medan Azure Web App-roboten distribuerades. En robot skapades och registrerades, en [Azure Web App](https://azure.microsoft.com/services/app-service/web/) skapades för att agera värd för roboten och roboten konfigurerades för att fungera tillsammans med [Microsoft QnA Maker](https://www.qnamaker.ai/). Nästa steg är att använda QnA Maker för att skapa en kunskapsbas med frågor och svar som ökar robotens intelligens.
