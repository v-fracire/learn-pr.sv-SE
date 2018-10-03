> [!NOTE]
> När du startar den virtuella datorn, finns användarnamnet och lösenordet som du måste logga in med på fliken **resurser** bredvid instruktionerna.

När du ska skapa en robot är det första steget att hitta en lämplig värdplats för roboten i Azure. Web Apps-funktionen i Azure App Service passar perfekt som värd för robotprogram och Azure Bot Service är specialutformat för att tillhandahålla dem. I den här enheten ska du använda Azure-portalen för att tillhandahålla en Azure Web App-robot.

1. Logga in på Azure-portalen genom att öppna https://portal.azure.com i VM-webbläsaren.

1. Välj **+ Skapa en resurs** följt av **AI + Machine Learning** och sedan **Robot för webbappar**.

    ![Skärmbild av Azure-portalen som visar bladet Skapa resurs med resurstypen för roboten för webbappar markerad.](../media/2-new-bot-service.png)

1. Ange ett namn, till exempel ”qa-factbot”, i rutan **Appnamn**. *Det här namnet måste vara unikt för Azure, så se noga till så att en grön bock visas bredvid det.*

1. Välj rätt **prenumeration** och **resursgrupp** och välj de befintliga resurserna.

1. Välj den plats som ligger närmast dig och välj prisnivån **S1**.

1. Välj **robotmallen**. Välj **SDK v3** som version, **Node.js** som SDK-språk och **Question and Answer** (Frågor och svar) som malltyp. Välj sedan **Välj** längst ned på bladet.

    ![Skärmbild av Azure-portalen som visar bladet Robotmall i skapandeprocessen för roboten, med Node.js som SDK-språk och mallalternativet Frågor och svar markerat.](../media/2-portal-select-template.png)

1. Välj nu **App Service-plan/plats** följt av **Skapa ny** och skapa sedan en App Service-plan med namnet ”qa-factbot-service-plan” eller liknande i samma region som du valde i föregående steg. När det är klart väljer du **Skapa** längst ned på bladet ”Robot för webbapp” för att starta distributionen.

    ![Skärmbild av Azure-portalen som visar en exempelkonfiguration av ett blad för en ny robot för webbappar.](../media/2-portal-start-bot-creation.png)

    > [!NOTE]
    > Distributionen tar vanligtvis högst två minuter.

1. När distributionen är klar klickar du på **Resursgrupper** i menyfliksområdet som visas till vänster i portalen.
1. Välj den färdiga resursgruppen för den här gruppen för att öppna resursgruppen där vi distribuerade Azure Web App-roboten.

Du bör nu se flera resurser som skapats för din Azure web app-robot. Det inträffade mycket bakom kulisserna när Azures robot för webbappar distribuerades. En robot skapades och registrerades, ett [Azure-webbprogram](https://azure.microsoft.com/services/app-service/web/) skapades för att agera värd för roboten och roboten konfigurerades för att fungera tillsammans med [Microsoft QnA Maker](https://www.qnamaker.ai/). Nästa steg är att använda QnA Maker för att skapa en kunskapsbas med frågor och svar som ökar robotens intelligens.
