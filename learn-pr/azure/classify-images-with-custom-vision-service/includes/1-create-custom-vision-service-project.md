När du ska skapa en modell för bildklassificering med Custom Vision Service börjar du med att skapa ett projekt. I den här enheten ska du skapa ett Custom Vision Service-projekt med hjälp av Custom Vision Service-portalen.

1. Öppna [Custom Vision Service-portalen](https://www.customvision.ai/?azure-portal=true) i webbläsaren. Välj sedan **Sign In** (Logga in).

1. Om du ombeds logga in ska du göra det med autentiseringsuppgifterna för ditt Microsoft-konto. Om du ombeds ge appen åtkomst till din information ska du klicka på **Ja**. Godkänn också tjänstvillkoren om du ombeds göra det.

1. Klicka på **Nytt projekt** för att skapa ett nytt projekt.

    ![Skapa ett Custom Vision Service-projekt](../media/1-portal-click-new-project.png)

1. I dialogrutan **Skapa nytt projekt** ger du projektet namnet *Artworks* och ser till att **Almänt** är markerat i listan **Domäner**. Du kan behålla standardinställningarna för **Projekttyper** och **Klassificeringstyper**. Välj **Skapa projekt** för att skapa projektet.

    > Med hjälp av domäner kan du optimera modellen för en viss typ av bilder. Om du till exempel vill klassificera matbilder efter vilka typer av maträtter eller vilket kök som visas kan det vara bra att välja domänen Food (Mat). Domänen General (Allmänt) väljer du i de fall där det inte finns någon passande domän eller om du känner dig osäker på vad du ska välja.

   ![Skapa ett Custom Vision Service-projekt](../media/1-portal-create-project.png)

Nästa steg är att ladda upp bilder till projektet och klassificera bilder genom att tilldela taggar.