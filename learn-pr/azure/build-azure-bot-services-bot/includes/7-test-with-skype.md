När chattroboten har distribuerats kan du ansluta den till kanaler som Skype, Slack, Microsoft Teams och Facebook Messenger, där du kan interagera med dem på samma sätt som med en person. I den här enheten lägger du till chattroboten bland dina Skype-kontakter och för en konversation med den i Skype.

1. Om du inte har installerat Skype i datorn ska du göra det nu. Du kan ladda ned Skype för Windows, macOS eller Linux från https://www.skype.com/en/download-skype/skype-for-computer/.

1. Gå tillbaka till din web app bot i Azure-portalen och klicka på **Kanaler** i menyn till vänster. Klicka på ikonen för **Skype**. Klicka sedan på **Avbryt** längst ned på bladet.

    ![Redigera Skype-kanalen](../media-draft/7-portal-edit-skype.png)

1. Klicka på **Skype**. Klicka sedan på **Lägg till bland kontakter** för att lägga till chattroboten som en Skype-kontakt och starta Skype.

    ![Ansluta till Skype](../media-draft/7-portal-click-skype.png)

1. Starta en konversation genom att skriva ”hi” i Skype-fönstret. Kommunicera sedan med roboten genom att ställa frågor och se hur den svarar. Du kan se exempel på frågor att ställa i filen **Factbot.tsv** som du använde till att fylla i kunskapsbasen i [Övning 2](#Exercise2).
 
    ![Chatta med chattroboten i Skype](../media-draft/7-skype-responses.png)

Nu har du en fungerande chattrobot som skapats med Azure Bot Service, fyllts med intelligens via Microsoft QnA Maker och som vem som helst i världen kan interagera med. Anslut gärna chattroboten till andra kanaler och prova den i olika situationer. Och om du vill göra chattroboten smartare kan du utöka kunskapsbasen med fler frågor och svar. Du kan till exempel använda [vanliga frågor och svar](https://docs.microsoft.com/azure/bot-service/bot-service-resources-bot-framework-faq?view=azure-bot-service-3.0) för Bot Framework om du vill träna chattroboten att besvara frågor om själva ramverket.