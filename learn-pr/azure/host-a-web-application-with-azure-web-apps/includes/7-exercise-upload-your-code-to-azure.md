I den här övningen laddar du upp ditt ASP.NET Core-program till Azure.

## <a name="create-a-staging-deployment-slot"></a>Skapa ett distributionsfack för mellanlagring

1. Leta upp och klicka på menykommandot **Distributionsfack** i det vänstra navigeringsfönstret.

1. Azure öppnar bladet **Distributionsfack** enligt vad som visas nedan.

    ![Distributionsfack](../media-draft/7-deployment-slot-blade.png)

1. Leta upp och klicka på knappen **+ Lägg till plats** i det övre navigeringsfältet i bladet distributionsfack.

1. Azure-portalen öppnar bladet **Lägg till en plats** enligt vad som visas nedan.

    ![Lägga till en plats](../media-draft/7-new-deployment-slot-blade.png)

    Namnge ditt distributionsfack. I det här fallet använder du **Staging**.

    I valet av **konfigurationskälla** har du två alternativ.

    1. Välj antingen att klona konfigurationselementen från en annan plats eller från den webbapp som skapades på Azure.

    1. Eller så kan du välja att inte klona några konfigurationselement. I så fall väljer du alternativet **Don't clone configuration from an existing slot** (Klona inte konfiguration från en befintlig plats).

    Välj det andra alternativet, **Don't clone configuration from an existing slot** (Klona inte konfiguration från en befintlig plats).

1. Leta upp och klicka på knappen **OK** längst ned på bladet.

1. När distributionsfacket har skapats navigerar Azure-portalen tillbaka till bladet **Distributionsfack** för webbappen.

    Du kan nu se det nya distributionsfacket som du just skapade.

    ![Distributionsfack har skapats](../media-draft/7-deployment-slot-created.png)

1. Leta upp och klicka på det nya distributionsfacket.

1. Azure-portalen navigerar till sidan **Översikt** för det nyligen skapade distributionsfacket.

    ![Distributionsfack för mellanlagring](../media-draft/7-deployment-slot-staging.png)

    Observera **URL:en** för distributionsfacket för mellanlagring; den är exakt samma som den du såg tidigare i början av den här enheten.

    Ett distributionsfack behandlas som en webbapp i Azure. Men det är en särskild typ som är underordnad den ursprungliga webbappen och därför kan växlas med den ursprungliga webbappen.

    Om du klickar på **URL:en** visas samma standardsida som den Azure skapade för webbappen första gången vi skapade den på Azure-portalen.

Nu när distributionsfacket för mellanlagring har skapats behöver du konfigurera **autentiseringsuppgifterna för distribution**.

## <a name="create-deployment-credentials"></a>Skapa autentiseringsuppgifter för distribution

Azure kräver att autentiseringsuppgifterna för distribution konfigureras innan du kan börja faktiska distributionsprocessen. Därför kommer du att lära dig hur du skapar dina egna autentiseringsuppgifter för distribution.

1. Leta upp och klicka på menykommandot **Autentiseringsuppgifter för distribution** i det vänstra navigeringsfönstret.

1. Azure-portalen navigerar till bladet **Autentiseringsuppgifter för distribution** enligt vad som visas nedan.

    ![Autentiseringsuppgifter för distribution](../media-draft/7-deployment-credentials.png)

    Ange ett **användarnamn** och ett **lösenord** och bekräfta sedan lösenordet igen.

    > Se till att skriva ned användarnamnet och lösenordet så att du inte glömmer dem. Du behöver dem senare när vi börjar ladda upp och distribuera koden till Azure.

    När du har valt användarnamn och lösenord letar du upp och klickar på knappen **Spara** längst upp på bladet **Autentiseringsuppgifter för distribution**.

Nu när autentiseringsuppgifterna för distribution har skapats behöver du konfigurera **distributionsalternativen**.

## <a name="use-a-local-git-repository-as-your-deployment-option"></a>Använda en lokal Git-lagringsplats som distributionsalternativ

Nu är det dags att skapa en lokal Git-lagringsplats i Azure så att du startar processen med att ladda upp din kod.

1. Leta upp och klicka på menykommandot **Distributionsalternativ** i det vänstra navigeringsfönstret.

1. Azure-portalen navigerar till bladet **Distributionsalternativ** enligt vad som visas nedan.

    ![Distributionsalternativ](../media-draft/7-deployment-options.png)

1. Klicka på **Choose source / Configure required settings** (Välj källa/konfigurera nödvändiga inställningar).

    ![Autentiseringsuppgifter för distribution](../media-draft/7-deployment-sources.png)

1. Azure-portalen visar de tillgängliga alternativen som du kan konfigurera och använda. I det här fallet väljer du alternativet **Local Git Repository** (Lokal Git-lagringsplats).

    ![Lokal Git-lagringsplats](../media-draft/7-local-git-repo.png)

1. Leta upp och klicka på knappen **OK** längst ned på bladet.

1. Leta upp och klicka på menykommandot **Översikt** i det vänstra navigeringsfönstret.

    ![Distributionsfack med Git konfigurerat](../media-draft/7-staging-after-setting-git.png)

    Lägg märke till två viktiga uppgifter:
    - **Användarnamn för Git/distribution**: det här är de autentiseringsuppgifter som du använder senare för att ansluta till den lokala Git-lagringsplatsen på Azure.

    - **URL för Git-klon**: det här är den lokala Git-lagringsplatsens URL som du ska använda för att ställa in som en **fjärranslutning** för din lokala webbprogramlagringsplats.

Det är dags att börja ladda upp din kod till ett distributionsfack för mellanlagring.

## <a name="install-git-on-your-machine"></a>Installera Git på datorn

Jag kommer att installera Git på min Ubuntu 18.04-dator.

1. Öppna ett nytt **terminalfönster**

1. Ange följande kommando. Du uppmanas att ange ditt Ubuntu-användarlösenord.

    ```console
    sudo apt-get update
    ```

1. När uppdateringen är klar anger du följande kommando för att installera Git lokalt. Du uppmanas att godkänna installationen av Git på datorn.

    ```console
    sudo apt-get install git-core
    ```

1. För att kontrollera att Git nu är installerat anger du följande kommando:

    ```console
    git --version
    ```

   Om installationen lyckas visas följande utdata:

    ```console
    git version 2.17.1
    ```

1. Det är alltid en bra idé att konfigurera Git-inställningar genom att ange ditt namn och din e-post. För det behöver du ange följande kommandon och ersätta platshållarna för namn och e-post utan klamrarna (`{}`):

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. För att verifiera att din information har registrerats av Git anger du följande kommando:

    ```console
    cat ~/.gitconfig
    ```

   Du bör se följande med ditt namn och din e-post som visas:

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a>Initiera en lokal Git-lagringsplats för webbappen

För att börja använda Git behöver du initiera en lokal Git-lagringsplats för webbappen.

1. Öppna ett nytt **terminalfönster**

1. Navigera till innehållsrotmappen för på webbappen; du kan skriva följande:

    ```console
    cd ~/Documents/BestBikeApp/
    ```

    ![Innehållsrotmapp för webbapp](../media-draft/7-web-app-content-root-folder.png)

1. Initiera en ny Git-lagringsplats genom att ange följande kommando

    ```console
    git init
    ```

    Om kommandot lyckas visas ett meddelande som liknar följande:

    ```console
    Initialized empty Git repository in /home/your-user/Documents/BestBikeApp/.git/
    ```

1. Mellanlagra alla webbappfiler till Git

   Nästa steg är att informera Git om dina webbappfiler. Göra det genom att lägga till alla filer i arbetskatalogen så att de **mellanlagras** av Git. Ange följande kommando:

    ```console
    git add .
    ```

    Kommandot ovan lägger till alla filer, vilket representeras av ".", till mellanlagringstillståndet för Git.

1. Nu behöver du checka in ändringarna till Git

   När du mellanlagrar filerna med Git behöver du checka in filerna till **Git-incheckningshistoriken** på den lokala datorn. Det gör du genom att ange följande kommando:

   `git commit -m "Initial create"`

   Kommandot **commit** tar argumentet **-m** för att inkludera ett meddelande med den incheckning som du skapar. Vid ett senare tillfälle när du har överfört koden till Azure kommer du att kunna se samma meddelande lagrat med den här specifika incheckningen.

## <a name="add-a-remote-for-the-local-git-repository"></a>Lägga till en fjärranslutning för den lokala Git-lagringsplatsen

Fram till nu har du initierat en ny lokal Git-lagringsplats. Du har dessutom checkat in alla dina webbappfiler till Git. Det som är kvar att göra är att lägga till en **fjärranslutning** för att ansluta din lokala Git-lagringsplats till den som hanteras i Azure.

För att göra det behöver du:

1. Kopiera den **Git-klon-URL** som du såg ovan.

1. När den har kopierats går du tillbaka till **terminalfönstret** och anger följande Git-kommando:

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    Got-kommandot ovan kopplar din lokala Git-lagringsplats till den som hanteras i Azure. Nu kan du börja skicka och hämta mellan den lokala och den fjärranslutna Git-lagringsplatsen!

1. Kontrollera att kommandot ovan har körts genom att ange följande Git-kommando:

    ```
    git remote -v
    ```

    Ovanstående kommando genererar följande utdata:

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a>Skicka koden till Azure

Nu när du har kopplat din lokala Git-lagringsplats till den fjärranslutna Git-lagringsplatsen på Azure kommer du att utveckla och bygga appen och sedan skicka webbappen till Azure.

1. Skriv följande Git-kommando för att skicka din **huvudgren** till den fjärranslutna Git-lagringsplatsen på Azure:

    ```console
    git push origin master
    ```

1. Du uppmanas att ange det lösenord som du har konfigurerat i avsnittet **Autentiseringsuppgifter för distribution** ovan. Ange lösenordet och tryck på [Retur]. Git börjar ladda upp dina incheckade filer till den fjärranslutna Git-lagringsplatsen på Azure som konfigurerats under distributionsfacket för mellanlagring.

## <a name="verify-the-web-app-is-uploaded-to-azure"></a>Kontrollera att webbappen har laddats upp till Azure

1. Logga in på Azure-portalen

1. Leta upp och klicka på menykommandot **Alla resurser** i det vänstra navigeringsfönstret.

1. Azure-portalen navigerar till listan över alla resurser som skapats på Azure hittills.

1. Leta upp och klicka på den mellanlagringsplats som skapades ovan. Kom ihåg att ett distributionsfack betraktas som en webbapp och därför visas som en webbappresurs under **Alla resurser**.

1. När du kommer till bladet distributionsfack för mellanlagring går du till **Distributionsalternativ**

    ![Distributionsfacket för mellanlagring efter uppladdning av webbappen](../media-draft/7-staging-deployment-slot-after-uploading-files.png)

    Du kommer att se att din första incheckning som du har lokalt på datorn nu har laddats upp till Azure-portalen!

    Genom att skicka koden lokalt till den fjärranslutna Git-lagringsplatsen som hanteras i Azure har Azure registrerat den här åtgärden.

    Varje gång du skickar koden till Azure visas en ny post tillsammans med det meddelande som du anger när du checkar in ändringarna lokalt på datorn.

1. Vi går till URL:en för **mellanlagringsplatsen**. URL:en har nämnts ovan, men om du glömmer den URL:en kan du alltid gå till sidan **Översikt** i distributionsfacket för mellanlagring och hämta URL:en.

1. Ange följande URL i webbläsarens adressfält: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/)

    ![Mellanlagringsplatsen som hanteras online](../media-draft/7-staging-slot-hosted-online.png)

Du har laddat upp dina lokala webbappfiler till en distributionsfacket för mellanlagring på Azure.

## <a name="swapping-the-staging-and-production-deployment-slots"></a>Växla distributionsfack för mellanlagring och produktion

Nu när programmet är igång och körs på det distributionsfack för mellanlagring som hanteras i Azure är det dags att växla den här platsen med platsen för produktion. För att göra det följer du dessa steg:

1. Leta upp och navigera till den ursprungliga webbappsbladet som skapades i enhet 2.

1. Leta upp och klicka på menykommandot **Distributionsfack** i det vänstra navigeringsfönstret.

    ![Bladet för distributionsfack för webbapp](../media-draft/7-web-app-slots.png)

1. Leta upp och klicka på knappen **Växla** längst upp på bladet.

1. Azure-portalen navigerar till bladet **Växla**.

    ![Bladet Växla](../media-draft/7-swap-blade.png)

1. Ställ in fältet **Växla** till **Växla**.

1. Ställ in fältet **Källa** till **Mellanlagring**.

1. Ställ in fältet **Mål** till **Produktion**.

1. Leta upp och klicka på knappen **OK** längst ned på bladet.

1. Azure startar växlingsprocessen. Den här åtgärden behöver vanligtvis några avsnitt beroende på storleken på den webbapp som växlas.

1. När åtgärden är klar går du till webbappens URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/)

    ![Sida för webbapp](../media-draft/7-web-app-page.png)

    Växlingsåtgärden har är klar! Du kan nu se den kod som du laddade upp till en distributionsfacket för mellanlagring som även hanteras på produktionsplatsen!

1. Gå till URL:en för mellanlagringsplatsen: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/)

    ![Mellanlagring för webbapp](../media-draft/7-staging-after-swapping.png)

    Distributionsfacket för mellanlagring innehåller nu de ursprungliga standardwebbprogramfiler som tidigare hanterades under produktionsplatsen.

Grattis! Du har laddat upp din webbapp till Azure!
