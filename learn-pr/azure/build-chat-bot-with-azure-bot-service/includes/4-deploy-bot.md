[!INCLUDE [0-vm-note](0-vm-note.md)]

När du skapade en Azure Web App-robot distribuerades även en Azure Web App som fungerar som värd. Roboten kräver dock lite en del kodning och den måste även distribueras till Azures webbapp. Som tur är kan har koden redan genererats åt dig av Azure Bot Service. I den här kursdelen ska du använda Visual Studio Code för att placera koden på en lokal Git-lagringsplats och publicera roboten till Azure genom att skicka ändringarna från den lokala lagringsplatsen till en fjärrlagringsplats som är ansluten till den Azure Web App som är värd för roboten – en process som kallas [kontinuerlig integrering](https://wikipedia.org/wiki/Continuous_integration).

1. Skapa en mapp med namnet ”Factbot” på valfri plats på hårddisken för att rymma robotens källkod.

1. Gå tillbaka till Azure-portalen i VM-webbläsaren och öppna den redan skapade övningsresursgruppen. Klicka sedan på roboten för webbappen som du skapade i den föregående övningen.

1. Välj **Build** på menyn till vänster och välj sedan alternativet för att **ladda ned robotkällkod** för att förbereda en zip-fil som innehåller robotens källkod. När ZIP-filen är klar klickar du på knappen för att **ladda ned robotkällkod** för att ladda ned den. När hämtningen är slutförd kan du extrahera innehållet i zip-filen till mappen ”Factbot” som du skapade tidigare.

1. Tillbaka på Web App Bot-bladet Build i Azure-portalen väljer du **Konfigurera kontinuerlig distribution**.

1. Välj **Konfiguration** högst upp på bladet **Distributioner** och därefter **Välj källa**.

1. Välj sedan **Lokal Git-lagringsplats** som distributionskälla.

1. Välj sedan **Skapa anslutning** och ange ett användarnamn och ett lösenord. Du kommer förmodligen att behöva ange ett annat användarnamn än ”FactbotAdministrator” eftersom namnet måste vara unikt i Azure. Välj **OK** för att återgå till bladet **Distributionsalternativ** och välj sedan **OK** igen för att återgå till bladet **Distributioner**.

    ![Skärmbild av Azure-portalen med den nya robotens App Service-blad som visar skärmen Autentiseringsuppgifter för distribution, med menyalternativet Autentiseringsuppgifter för distribution och knappen Spara markerat.](../media/4-portal-enter-ci-creds.png)

1. Medan distributionssystem etableras stänger du bladet **Distributioner** och väljer **Alla App Service-inställningar** på menyn till vänster.

1. Starta **Visual Studio Code** och använd kommandot **Arkiv** > **Öppna mapp...** för att öppna mappen ”Factbot” som du kopierade robotens källkod till.

1. Klicka på knappen **Källkodskontroll** i aktivitetsfältet till vänster i Visual Studio Code.

1. Klicka på ikonen **Skapa lagringsplats** längst upp.

1. Klicka på knappen **Skapa lagringsplats** i dialogrutan.

1. Skriv ”First commit.” i meddelanderutan.

1. Klicka på bockmarkeringen för att genomföra ändringarna och mellanlagra alla filer när du tillfrågas.

    > [!NOTE]
    > Om ett Git-fel inträffar om att du inte ställt in din identitet i Git startar du en kommandotolk och kör följande kommandon. Ersätt platshållarvärdena för e-post och namn om du vill. Klicka sedan på knappen Genomför igen.
    >
    > ```bash
    > git config --global user.email "Lab User"
    > git config --global user.name "LabUser#######@learn"
    > ```

1. Välj **Terminal** på menyn **Visa** i Visual Studio Code för att öppna den integrerade terminalen.

1. Kör följande kommando i den integrerade terminalen och ersätt BOT_NAME på följande två platser med robotnamnet som du angav i övning 1.

    > [!NOTE]
    > En fullständig fjärransluten Git-URL finns även i App Service-resursen i avsnittet **Översikt** under **URL för Git-klon**.

    ```bash
    git remote add qna-factbot https://BOT_NAME.scm.azurewebsites.net:443/BOT_NAME.git
    ```

1. Återgå till panelen **Källkontroll** och klicka på ellipsen (de tre punkterna) längst upp på panelen KÄLLKONTROLL och välj **Publish Branch** (Publicera gren) på menyn för att skicka robotkoden från den lokala lagringsplatsen till Azure. Om du ombeds uppge autentiseringsuppgifter ska du uppge det användarnamn och det lösenord som du angav i steg 9 i den här övningen.

Din robot har nu publicerats till Azure. Men innan du testar den ska vi köra den lokalt. Du ska också få lära dig att felsöka roboten i Visual Studio Code.
