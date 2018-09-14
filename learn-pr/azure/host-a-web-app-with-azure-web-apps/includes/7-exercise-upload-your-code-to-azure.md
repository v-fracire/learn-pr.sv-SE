I den här övningen överför du ditt ASP.NET Core-program till en Azure-webbapp.

## <a name="create-a-staging-deployment-slot"></a>Skapa ett distributionsfack för mellanlagring

1. Öppna den webbapp-resurs som du skapade tidigare. Om du har stängt resursbladet, kan du hitta det igen genom att söka efter appen i **Alla resurser** eller den innehållande resursgruppen i **Resursgrupper**.

1. Klicka på menykommandot **Distributionsfack** i det vänstra navigeringsfönstret.

1. I bladet **Distributionsfack**, klickar du på knappen**Lägg till fack** i det övre navigationsfältet på bladet.

1. Azure-portalen öppnar bladet **Lägg till ett fack** som det visas nedan.

    1. Namnge ditt distributionsfack. I det här fallet använder du `staging`.

    2. När du väljer **Konfigurationskälla** har du två alternativ.

        * Du kan välja att klona konfigurationselementen från ett befintligt distributionsfack eller webbapp som skapats i Azure.
        * Eller så kan du välja att inte klona några konfigurationselement. Välj alternativet **Klona inte konfigurationen från ett befintligt fack**.

        För det här distributionsfacket, väljer du det andra alternativet **Klona inte konfigurationen från ett befintligt fack**. Du kommer att konfigurera det direkt.

    ![Skärmbild av Azure-portalen som visar konfigurationen för ett nytt distributionsfack för mellanlagring.](../media/7-new-deployment-slot-blade.png)

1. Klicka på **OK** längst ned på bladet för att skapa ditt nya distributionsfack.

1. När distributionsfacket har skapats navigerar Azure-portalen tillbaka till bladet **Distributionsfack** för din webbapp.

    Du kan nu se det nya distributionsfacket som du just skapade.

    ![Skärmbild av Azure-portalen som visar bladet Distributionsfack med det nya facket som har skapats.](../media/7-deployment-slot-created.png)

1. Välj det nya distributionsfacket.

1. Azure-portalen navigerar till **Översikt**-sidan för det nyligen skapade distributionsfacket.

    ![Distributionsfack för mellanlagring](../media/7-deployment-slot-staging.png)

    Observera **URL** för distributionsfacket för mellanlagring. Det är en annan URL från den du såg tidigare, med facknamnet sist.

    Ett distributionsfack behandlas som en fullvärdig webbapp i Azure. Det är dock en särskild typ som är underordnad den ursprungliga webbappen och kan växlas med den ursprungliga webbappen.

    Om du klickar på **webbadressen** visas samma standardsida som Azure skapade för webbappen första gången vi skapade den i Azure Portal.

Nu när distributionsfacket för mellanlagring har skapats måste du konfigurera **autentiseringsuppgifter för distributionen**.

## <a name="create-deployment-credentials"></a>Skapa autentiseringsuppgifter för distribution

I Azure måste du ställa in autentiseringsuppgifter för distribution innan du kan börja den faktiska distributionsprocessen. Därför kommer du att lära dig hur du skapar dina egna autentiseringsuppgifter för distribution.

1. Klicka på menykommandot **Autentiseringsuppgifter för distribution** i det vänstra navigeringsfönstret.

1. Azure-portalen navigerar till bladet **Autentiseringsuppgifter för distribution** som det visas nedan.

    Ange ett **användarnamn** och ett **lösenord** och bekräfta sedan lösenordet igen.

    > [!NOTE]
    > Se till att skriva ned användarnamnet och lösenordet så att du inte glömmer dem! Du behöver dem senare när vi börjar ladda upp och distribuera koden till Azure.

    ![Skärmbild av Azure-portalen som visar bladet Autentiseringsuppgifter för distribution på mellanlagringsfacket med exempel på autentiseringsuppgifter i de obligatoriska fälten.](../media/7-deployment-credentials.png)

1. Klicka på **Spara** längst upp på bladet **Autentiseringsuppgifter för distribution**.

Nu när autentiseringsuppgifterna för distribution har skapats behöver du konfigurera övriga distributionsalternativ.

## <a name="use-a-local-git-repository-as-your-deployment-option"></a>Använda en lokal Git-lagringsplats som distributionsalternativ

Därefter skapar vi en lokal Git-lagringsplats i Azure så att du kan börja ladda upp din kod.

1. Inom webbappen distributionsfack för **mellanlagring**, klickar du på menyalternativet **Distributionsalternativ** i det vänstra navigeringsfönstret.

1. Azure-portalen navigerar till bladet **Distributionsalternativ**.

1. Klicka på **Välj källa** för att konfigurera de nödvändiga inställningarna.

1. Azure-portalen visar de tillgängliga alternativen som du kan konfigurera och använda. I vårt fall väljer vi alternativet **Lokal Git-lagringsplats**.

1. Du kommer tillbaka till bladet **Lägg till distributionslösning**. Klicka på **OK** längst ned på bladet för att konfigurera distributionskällan.

1. Navigera nu till avsnittet **Distributionscenter (förhandsversion)** till vänster för att visa den nya distributionsinformationen.

    ![Skärmbild av Azure-portalen som visar distributionsfackets Distributionscenter-blad med URI för Git Clone för det markerade facket.](../media/7-staging-after-setting-git.png)

    Den viktiga informationen att notera här är **URI för Git Clone** som är den lokala Git-lagringsplatsens URL som du kommer att använda som en **fjärranslutning** för din lokala webbapplagringsplats.

Det är dags att börja ladda upp din kod till ett distributionsfack för mellanlagring.

## <a name="install-git-on-your-machine"></a>Installera Git på datorn

Om du inte redan har gjort det, installerar du Git på din Linux-dator.

> [!NOTE]
> De här instruktionerna är för Ubuntu 18.04 så stegen för att installera Git kan skilja sig för din distribution och version. Ta en titt på [Linux Git Installationsinstruktioner](https://git-scm.com/download/linux) för de steg som krävs.

1. Öppna ett nytt **terminalfönster**

1. Ange följande kommando. Du uppmanas att ange ditt Ubuntu-användarlösenord.

    ```console
    sudo apt-get update
    ```

1. När uppdateringen är färdig anger du följande kommando för att installera Git lokalt. Du uppmanas att godkänna installationen av Git på din dator.

    ```console
    sudo apt-get install git-core
    ```

1. För att kontrollera att Git är installerat anger du följande kommando:

    ```console
    git --version
    ```

   Om installationen lyckas visas följande utdata:

    ```console
    git version 2.17.1
    ```

1. Det är alltid en bra idé att konfigurera Git-inställningar genom att ange ditt namn och din e-post. För att göra det så behöver du utfärda följande kommandon, där du ersätter platshållarna `{your name}` och `{your email}` med ditt eget namn och e-postadress (utan `{}` klammerparanteserna):

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. För att verifiera att din information har registrerats av Git anger du följande kommando:

    ```console
    cat ~/.gitconfig
    ```

   Du bör se följande utdata med ditt namn och din e-postadress:

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a>Initiera en lokal Git-lagringsplats för webbappen

När du ska börja använda Git behöver du initiera en lokal Git-lagringsplats för webbappen.

1. Öppna ett nytt **terminalfönster**.

1. Navigera till innehållsrotmappen för den webbapp som du skapade tidigare. Du kan skriva följande:

    ```console
    cd ~/Documents/BestBikeApp/
    ```

1. Initiera en ny Git-lagringsplats genom att ange följande kommando:

    ```console
    git init
    ```

    Om kommandot lyckas visas ett meddelande som ser ut så här:

    ```console
    Initialized empty Git repository in /home/{your-user}/Documents/BestBikeApp/.git/
    ```

1. Mellanlagra alla webbappsfiler i Git.

   Nästa steg är att informera Git om dina webbappsfiler. Det gör du genom att lägga till alla filer i arbetskatalogen så att de **mellanlagras** av Git. Ange följande kommando:

    ```console
    git add .
    ```

    När du anger ”.” i kommandot ovan läggs alla filer till för mellanlagring i Git.

1. Nu behöver du checka in ändringarna i Git.

   När du mellanlagrar filerna med Git behöver du checka in filerna i den lokala **Git-incheckningshistoriken**. Det gör du med följande kommando:

    ```console
   git commit -m "Initial create"
    ```

   Kommandot `commit` kan ta argumentet `-m` med ett meddelande om incheckningen du skapar. När du senare överför koden till Azure kan du se samma meddelande för just den här incheckningen.

## <a name="add-a-remote-for-the-local-git-repository"></a>Lägga till en fjärranslutning till den lokala Git-lagringsplatsen

Hittills har du initierat en ny lokal Git-lagringsplats. Du har dessutom checkat in alla dina webbappsfiler till Git. Det som återstår är att lägga till en **fjärranslutning** som ansluter din lokala Git-lagringsplats till den som hanteras i Azure.

För att göra det behöver du:

1. Kopiera den **Git-klon-URL** som du såg ovan.

1. När den har kopierats går du tillbaka till **terminalfönstret** och anger följande Git-kommando:

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    Git-kommandot ovan kopplar din lokala Git-lagringsplats till den som hanteras i Azure. Nu kan du börja skicka och hämta mellan den lokala och den fjärranslutna Git-lagringsplatsen!

1. Verifiera ovanstående kommando genom att ange följande Git-kommando:

    ```console
    git remote -v
    ```

    Ovanstående kommando genererar följande utdata:

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a>Skicka koden till Azure

Nu när du har kopplat din lokala Git-lagringsplats till den fjärranslutna Git-lagringsplatsen i Azure kommer du att skriva och kompilera appen och sedan skicka den till Azure.

1. Skriv följande Git-kommando för att skicka din **huvudgren** till den fjärranslutna Git-lagringsplatsen i Azure:

    ```console
    git push origin master
    ```

1. Du får ange lösenordet du konfigurerade i avsnittet **Autentiseringsuppgifter för distribution** ovan. Ange lösenordet och tryck på Retur. Git börjar överföra dina incheckade filer till den fjärranslutna Git-lagringsplatsen i Azure som konfigurerats under distributionsfacket för mellanlagring.

## <a name="verify-the-web-app-is-uploaded-to-azure"></a>Verifiera att webbappen har överförts till Azure

1. Logga in på Azure-portalen.

1. Klicka på menykommandot **Alla resurser** i det vänstra navigeringsfönstret.

1. Azure-portalen navigerar till listan över alla resurser som skapats på Azure hittills.

1. Klicka på det mellanlagringsfack som skapades ovan. Kom ihåg att ett distributionsfack betraktas som en webbapp och därför visas som en webbappresurs under **Alla resurser**.

1. När du kommer till bladet distributionsfack för mellanlagring går du till **Distributionsalternativ**.

    Du kommer att se att din första incheckning som du har lokalt på datorn nu har laddats upp till Azure-portalen.

    Genom att skicka koden lokalt till den fjärranslutna Git-lagringsplatsen som hanteras i Azure har Azure registrerat den här åtgärden.

    Varje gång du skickar koden till Azure så visas en ny post tillsammans med det meddelande som du anger när du checkar in ändringarna lokalt på din dator.

    ![Skärmbild av Azure-portalen som visar en nyligen genomförd Git-lagringsplatsdistribution på bladet Utvecklingsalternativ.](../media/7-staging-deployment-slot-after-uploading-files.png)

1. Vi går till URL:en för **mellanlagringsfacket**. URL:en nämndes ovan, men om du glömmer den så kan du alltid gå till sidan **Översikt** i distributionsfacket för mellanlagring och hämta URL:en.

1. Ange följande URL i webbläsarens adressfält: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).

    ![Skärmbild som visar en webbläsarvy för webbplatsen för distributionsfack för mellanlagring.](../media/7-staging-slot-hosted-online.png)

Du har överfört dina lokala webbappfiler till distributionsfacket för mellanlagring på Azure.

## <a name="swapping-the-staging-and-production-deployment-slots"></a>Växla mellan distributionsfack för mellanlagring och produktion

Nu när programmet är igång och körs i distributionsfacket för mellanlagring som hanteras i Azure är det dags att växla till produktionsfacket. För att göra det följer du dessa steg:

1. Gå till det ursprungliga webbappbladet som skapades tidigare. Du hittar den ursprungliga Webbappen från bladet **Alla resurser**.

1. Klicka på menykommandot **Distributionsfack** i det vänstra navigeringsfönstret.

1. Klicka på knappen **Växla** längst upp på bladet.

1. Azure-portalen tar dig till bladet **Växla**.

1. I fältet **Växla** väljer du **Växla**.

1. I fältet **Källa** väljer du **Mellanlagring**.

1. I fältet **Mål** väljer du **Produktion**.

    ![Skärmbild av Azure-portalen som visar bladet distributionsfackväxling.](../media/7-swap-blade.png)

1. Klicka på knappen **OK** längst ned på bladet.

1. Azure startar växlingsprocessen. Den här åtgärden tar vanligtvis några sekunder beroende på hur stor webbappen som växlas är.

1. När åtgärden är klar går du till webbappens URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).

    ![Skärmbild som visar en webbläsarvy över det tidigare distributionsfacket för mellanlagring som nu finns som primär webbapp.](../media/7-web-app-page.png)

    Växlingsåtgärden är nu klar! Du kan nu se att den kod som du överförde till distributionsfacket för mellanlagring även finns på produktionsfacket.

1. Besök nu URL:en för mellanlagringsfacket: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).

    ![Skärmbild som visar en webbläsarvy över det tidigare primära distributionsfacket som nu finns som en webbapp med distributionsfack för mellanlagring.](../media/7-staging-after-swapping.png)

    Distributionsfacket för mellanlagring innehåller nu de ursprungliga standardwebbappfiler som tidigare hanterades under produktionsfacket.

Grattis! Du har överfört din webbapp till Azure och växlat distributionsfack.
