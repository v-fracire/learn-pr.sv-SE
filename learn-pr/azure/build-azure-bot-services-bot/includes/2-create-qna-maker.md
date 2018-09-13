[QnA Maker](https://www.qnamaker.ai/) ingår i [Azure Cognitive Services](https://www.microsoft.com/cognitive-services/), en svit med tjänster och API:er som används för att bygga intelligenta appar med artificiell intelligens (AI) och maskininlärning. Istället för att koda en robot och förutse alla frågor en användare kan ställa med tillhörande svar, kan du ansluta den till en kunskapsbas med frågor och svar som skapats med QnA Maker. Ett vanligt scenario är att man skapar en kunskapsbas utifrån en FAQ-fil (Vanliga frågor och svar) så att roboten kan svara på domänspecifika frågor som ”hur hittar jag min Windows-produktnyckel” eller ”varifrån kan jag ladda ned Visual Studio Code?”

I den här övningen ska du använda QnA Maker för att skapa en kunskapsbas med frågor som ”vilka NFL-lag har vunnit Super Bowl flest gånger” och ”vilken är den största staden i hela världen?” Därefter ska du distribuera kunskapsbasen i en Azure-webbapp så att den kan nås via en HTTPS-slutpunkt.

1. Öppna [QnA Maker-portalen](https://www.qnamaker.ai/) i webbläsaren och logga in med ditt Microsoft-konto om du inte redan är inloggad. Klicka sedan på **Create a knowledge base** (Skapa en kunskapsbas) i menyn högst upp på sidan.
 
    ![Skapa en kunskapsbas](../images/qna-new-kb.png)

    _Skapa en kunskapsbas_

1. Klicka på **Create a QnA service** (Skapa en QnA Maker-tjänst).

    ![Skapa en QnA-tjänst](../images/create-kb-1.png)

    _Skapa en QnA-tjänst_

1. Ange ett namn, till exempel ”qa-factbot-kb”, i fältet **Namn**. Det här namnet måste vara unikt för Azure, så se noga till så att en grön bock visas bredvid det *samt* i fältet **App name** (Appnamn) längre ned på bladet. Välj **Use existing** (Använd befintlig) under **Resource Group** (Resursgrupp) och välj sedan resursgruppen ”factbot-rg” som du skapade när du har distribuerade Azure Web App-roboten i [övning 1](#Exercise1). Välj prisnivåerna **F0** och **F**. (Båda är kostnadsfria nivåer som lämpar sig väl för robotexperiment.) Välj dem plats som ligger närmast dig i de bägge listrutorna för plats och klicka sedan på knappen **Skapa** längst ned på bladet.

    ![Skapa en QnA-tjänst](../images/new-qna-maker-service.png)

    _Skapa en QnA-tjänst_

1. Klicka på **Resursgrupper** i menyfliksområdet som visas till vänster i portalen. Öppna resursgruppen ”factbot-rg”. Vänta tills statusen ”Distribuerar” ändras till ”Lyckades” högst upp på bladet – detta är en indikation som visar att distributionen av QnA och resurserna har slutförts. Med jämna mellanrum klickar du på **Uppdatera** överst på bladet för att uppdatera distributionens status.

    ![Distributionen har slutförts](../images/resource-group-master-2.png)

    _Distributionen har slutförts_

1. Gå tillbaka till sidan [Create a knowledge base](https://www.qnamaker.ai/Create) (Skapa en kunskapsbas) i QnA Maker-portalen. Under **Azure QnA service** väljer du det QnA-tjänstnamn du angav i steg 3. Ge sedan kunskapsbasen ett namn, till exempel ”Factbot Knowledge Base”.

    ![Ge kunskapsbasen ett namn](../images/create-kb-2-3.png)

    _Ge kunskapsbasen ett namn_

1. Du kan mata in frågor och svar i kunskapsbasen i QnA Maker manuellt, eller importera dem från FAQ-filer (Vanliga frågor och svar) online eller lokala filer. Format som stöds är tabbavgränsade textfiler, Microsoft Word-dokument, Excel-kalkylblad och PDF-filer.

    Exempel: [Klicka här](https://topcs.blob.core.windows.net/public/bots-resources.zip) att ladda ned en ZIP-fil som innehåller en textfil med namnet **Factbot.tsv** och kopiera sedan filen till datorn. Rulla nedåt i QnA Maker-portalen, klicka på **+ Lägg till fil** och välj **Factbot.tsv**. Den här filen innehåller 20 frågor och svar i tabbavgränsat format.

    ![Fylla på kunskapsbasen](../images/create-kb-4.png)

    _Fylla på kunskapsbasen_

1. Klicka på **Create your KB** (Skapa din kunskapsbas) längst ned på sidan och vänta tills kunskapsbasen har skapats. Det ska ta mindre än en minut.

    ![Skapa kunskapsbasen](../images/create-kb-5.png)

    _Skapa kunskapsbasen_

1. Kontrollera att frågor och svar som importerats från **Factbot.tsv** visas i kunskapsbasen. Klicka på **Save and train** (Spara och träna) och vänta tills träningen har slutförts.

    ![Träna kunskapsbasen](../images/save-and-train.png)

    _Träna kunskapsbasen_

1. Klicka på knappen **Test** till höger om knappen **Save and train** (Spara och träna). Skriv ”Hi” i meddelanderutan och tryck på **Retur**. Kontrollera att svaret är ”Welcome to the QnA Factbot”, som här nedan.

    ![Testa kunskapsbasen](../images/test-kb.png)

    _Testa kunskapsbasen_

1. Skriv ”What book has sold the most copies?” i meddelanderutan och tryck på **Retur**. Vad är svaret?

1. Klicka på knappen **Test** igen för att minimera testpanelen. Klicka sedan på **Publish** (Publicera) i menyn överst på sidan och klicka på **Publish** (Publicera) längst ned på sidan för att publicera kunskapsbasen. Efter *publicering* blir kunskapsbasen tillgänglig på en HTTPS-slutpunkt.

    ![Publicera kunskapsbasen](../images/publish-kb.png)

    _Publicera kunskapsbasen_ 

Vänta tills publiceringen har slutförts och kontrollera att du får ett meddelande om att QnA-tjänsten har distribuerats. Kunskapsbasen finns nu i en egen Azure-webbapp. Nästa steg är att distribuera en robot som kan använda sig av den.