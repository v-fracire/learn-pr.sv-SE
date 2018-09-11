I den här övningen laddar du upp ditt ASP.NET Core-program till Azure.

## <a name="create-a-staging-deployment-slot"></a>Skapa ett distributionsfack för mellanlagring

1. Leta upp och klicka på menyalternativet **Distributionsfack** i navigeringsfönstret till vänster.

1. Azure öppnar bladet **Distributionsfack** enligt vad som visas nedan.

    ![Distributionsfack](../media-draft/7-deployment-slot-blade.png)

1. Leta upp och klicka på knappen **+ Lägg till plats** i det övre navigeringsfältet på bladet med distributionsfack.

1. Azure-portalen öppnar bladet **Lägg till ett fack**, se nedan.

    ![Lägga till ett fack](../media-draft/7-new-deployment-slot-blade.png)

    Namnge ditt distributionsfack. I det här fallet använder du **Staging**.

    I valet av **konfigurationskälla** har du två alternativ.

    1. Välj om du vill klona konfigurationselementen från ett annat fack eller en webbapp som skapats i Azure.

    1. Du kan välja att inte klona några konfigurationselement. I så fall väljer du alternativet **Klona inte konfigurationen från ett befintligt fack**.

    Välj det andra alternativet, **Klona inte konfigurationen från ett befintligt fack**.

1. Leta upp och klicka på knappen **OK** längst ned på bladet.

1. När distributionsfacket har skapats navigerar Azure-portalen tillbaka till bladet **Distributionsfack** för din webbapp.

    Du kan nu se det nya distributionsfacket du just skapade.

    ![Distributionsfacket har skapats](../media-draft/7-deployment-slot-created.png)

1. Leta upp och klicka på det nya distributionsfacket.

1. Azure Portal navigerar till sidan **Översikt** för distributionsfacket du skapade.

    ![Distributionsfack för mellanlagring](../media-draft/7-deployment-slot-staging.png)

    Observera **webbadressen** till distributionsfacket för mellanlagring. Det är samma webbadress som du såg tidigare i början av utbildningsenheten.

    Ett distributionsfack behandlas som en webbapp i Azure. Det är dock en särskild typ som är underordnad den ursprungliga webbappen och kan växlas med den ursprungliga webbappen.

    Om du klickar på **webbadressen** visas samma standardsida som Azure skapade för webbappen första gången vi skapade den i Azure Portal.

Nu när distributionsfacket för mellanlagring har skapats måste du konfigurera **autentiseringsuppgifter för distributionen**.

## <a name="create-deployment-credentials"></a>Skapa autentiseringsuppgifter för distribution

I Azure måste du ställa in autentiseringsuppgifter för distribution innan du kan börja den faktiska distributionsprocessen. Därför får du lära dig hur du skapar egna autentiseringsuppgifter för distribution.

1. Leta upp och klicka på menyalternativet **Autentiseringsuppgifter för distribution** i navigeringsfönstret till vänster.

1. Azure Portal navigerar till bladet **Autentiseringsuppgifter för distribution**, se nedan.

    ![Autentiseringsuppgifter för distribution](../media-draft/7-deployment-credentials.png)

    Ange ett **användarnamn** och ett **lösenord** och bekräfta sedan lösenordet.

    > Se till att skriva ned användarnamnet och lösenordet så att du inte glömmer dem. Du behöver dem senare när vi börjar ladda upp och distribuera koden till Azure.

    När du har valt användarnamn och lösenord letar du upp och klickar på knappen **Spara** längst upp på bladet **Autentiseringsuppgifter för distribution**.

Nu när autentiseringsuppgifterna för distribution har skapats ska du konfigurera **distributionsalternativen**.

## <a name="use-a-local-git-repository-as-your-deployment-option"></a>Använda en lokal Git-lagringsplats som distributionsalternativ

Nu är det dags att skapa en lokal Git-lagringsplats i Azure så att du kan komma igång med att ladda upp kod.

1. Leta upp och klicka på menyalternativet **Distributionsalternativ** i navigeringsfönstret till vänster.

1. Azure Portal navigerar till bladet **Distributionsalternativ**, se nedan.

    ![Distributionsalternativ](../media-draft/7-deployment-options.png)

1. Klicka på **Choose source / Configure required settings** (Välj källa/konfigurera nödvändiga inställningar).

    ![Autentiseringsuppgifter för distribution](../media-draft/7-deployment-sources.png)

1. Azure Portal visar de alternativ du kan konfigurera och använda. I vårt fall väljer du alternativet **Lokal Git-lagringsplats**.

    ![Lokal Git-lagringsplats](../media-draft/7-local-git-repo.png)

1. Leta upp och klicka på knappen **OK** längst ned på bladet.

1. Leta upp och klicka på menykommandot **Översikt** i det vänstra navigeringsfönstret.

    ![Distributionsfack med Git konfigurerat](../media-draft/7-staging-after-setting-git.png)

    Lägg märke till två viktiga uppgifter:
    - **Användarnamn för Git/distribution**: det här är de autentiseringsuppgifter som du använder senare för att ansluta till den lokala Git-lagringsplatsen i Azure.

    - **URL för Git-klon**: det här är webbadressen till den lokala Git-lagringsplatsen som du kommer använda för **fjärranslutning** till webbappens lokala lagringsplats.

Det är dags att börja ladda upp kod till ett distributionsfack för mellanlagring.

## <a name="install-git-on-your-machine"></a>Installera Git på datorn

Jag kommer att installera Git på min Ubuntu 18.04-dator.

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

1. Det är alltid en bra idé att konfigurera Git-inställningar genom att ange namn och e-postadress. För det behöver du ange följande kommandon och ersätta platshållarna för namn och e-post utan klamrar (`{}`):

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. Verifiera att informationen har registrerats av Git med följande kommando:

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

1. Navigera till innehållsrotmappen för på webbappen. Du kan skriva så här:

    ```console
    cd ~/Documents/BestBikeApp/
    ```

    ![Innehållsrotmapp för webbapp](../media-draft/7-web-app-content-root-folder.png)

1. Initiera en ny Git-lagringsplats med följande kommando:

    ```console
    git init
    ```

    Om kommandot lyckas visas ett meddelande som ser ut så här:

    ```console
    Initialized empty Git repository in /home/your-user/Documents/BestBikeApp/.git/
    ```

1. Mellanlagra alla webbappsfiler i Git.

   Nästa steg är att informera Git om dina webbappsfiler. Det gör du genom att lägga till alla filer i arbetskatalogen så att de **mellanlagras** av Git. Ange följande kommando:

    ```console
    git add .
    ```

    När du anger ”.” i kommandot ovan läggs alla filer till för mellanlagring i Git.

1. Nu behöver du checka in ändringarna i Git.

   När du mellanlagrar filerna med Git behöver du checka in filerna i den lokala **Git-incheckningshistoriken**. Det gör du med följande kommando:

   `git commit -m "Initial create"`

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

    ```
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

1. Logga in på Azure Portal.

1. Leta upp och klicka på menyalternativet **Alla resurser** i navigeringsfönstret till vänster.

1. Azure Portal navigerar till listan med alla resurser som skapats i Azure hittills.

1. Leta upp och klicka på mellanlagringsplatsen du skapade ovan. Kom ihåg att distributionsfack ses som webbappar och därför visas som webbappsresurser under **Alla resurser**.

1. När du kommer till bladet med distributionsfacket för mellanlagring går du till **Distributionsalternativ**.

    ![Distributionsfack för mellanlagring efter uppladdning av webbappen](../media-draft/7-staging-deployment-slot-after-uploading-files.png)

    Du kommer att se att din första lokala incheckning nu har laddats upp till Azure Portal!

    Genom att skicka koden lokalt till den fjärranslutna Git-lagringsplatsen som hanteras i Azure har Azure registrerat den här åtgärden.

    Varje gång du skickar kod till Azure så visas en ny post tillsammans med meddelandet du angav när du checkade in ändringarna lokalt.

1. Vi går till webbadressen för **mellanlagringsfacket**. Vi nämnde webbadressen ovan, men om du glömmer den kan du alltid se adressen på sidan **Översikt** i distributionsfacket för mellanlagring.

1. Ange följande webbadress i webbläsarens adressfält: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).

    ![Mellanlagringsfack som hanteras online](../media-draft/7-staging-slot-hosted-online.png)

Du har nu laddat upp dina lokala webbappsfiler till distributionsfacket för mellanlagring i Azure.

## <a name="swapping-the-staging-and-production-deployment-slots"></a>Växla mellan distributionsfack för mellanlagring och produktion

Nu när programmet är igång och körs i distributionsfacket för mellanlagring som hanteras i Azure är det dags att växla till produktionsfacket. Det gör du så här:

1. Leta upp och navigera till det ursprungliga webbappsbladet som du skapade i utbildningsenhet 2.

1. Leta upp och klicka på menyalternativet **Distributionsfack** i navigeringsfönstret till vänster.

    ![Bladet för distributionsfack för webbapp](../media-draft/7-web-app-slots.png)

1. Leta upp och klicka på knappen **Växla** längst upp på bladet.

1. Azure Portal tar dig till bladet **Växla**.

    ![Bladet Växla](../media-draft/7-swap-blade.png)

1. I fältet **Växla** väljer du **Växla**.

1. I fältet **Källa** väljer du **Mellanlagring**.

1. I fältet **Mål** väljer du **Produktion**.

1. Leta upp och klicka på knappen **OK** längst ned på bladet.

1. Azure startar växlingsprocessen. Den här åtgärden tar vanligtvis några sekunder beroende på hur stor webbappen som växlas är.

1. När åtgärden är färdig går du till webbappens URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).

    ![Sida för webbapp](../media-draft/7-web-app-page.png)

    Växlingsåtgärden är färdig! Du kan nu se att koden du överförde till distributionsfacket för mellanlagring även finns i produktionsfacket!

1. Gå nu till webbadressen för mellanlagringsfacket: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).

    ![Mellanlagring för webbapp](../media-draft/7-staging-after-swapping.png)

    Distributionsfacket för mellanlagring innehåller nu webbappens ursprungliga standardfiler som tidigare hanterades i produktionsfacket.

Grattis! Du har laddat upp din webbapp till Azure!
