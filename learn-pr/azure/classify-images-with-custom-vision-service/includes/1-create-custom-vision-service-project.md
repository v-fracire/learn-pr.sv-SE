När du ska skapa en modell för bildklassificering med Custom Vision Service börjar du med att skapa ett projekt. I den här enheten ska du skapa ett Custom Vision Service-projekt med hjälp av Custom Vision Service-portalen.

1. Öppna [Custom Vision Service-portalen](https://www.customvision.ai/) i webbläsaren. Klicka på **Logga in**.

    ![Logga in på Custom Vision Service-portalen](../media/1-portal-sign-in.png)

1. Om du ombeds logga in ska du göra det med autentiseringsuppgifterna för ditt Microsoft-konto. Om du ombeds ge appen åtkomst till din information ska du klicka på **Ja**. Godkänn också tjänstvillkoren om du ombeds göra det.

1. Klicka på **Nytt projekt** för att skapa ett nytt projekt.

    ![Skapa ett Custom Vision Service-projekt](../media/1-portal-click-new-project.png)

1. Ge projektet namnet ”Artwork” i dialogrutan New project (Nytt projekt) och se till så att alternativet **General** (Allmänt) är valt som domän. Klicka på **Create project** (Skapa projekt).

    > Med hjälp av domäner kan du optimera modellen för en viss typ av bilder. Om du till exempel vill klassificera matbilder efter vilka typer av maträtter eller vilket kök som visas kan det vara bra att välja domänen Food (Mat). Domänen General (Allmänt) väljer du i de fall där det inte finns någon passande domän eller om du känner dig osäker på vad du ska välja.

   ![Skapa ett Custom Vision Service-projekt](../media/1-portal-create-project.png)

Nästa steg är att ladda upp bilder till projektet och klassificera bilder genom att tilldela taggar.