I den här övningen ska du ta bort resursgruppen som innehåller roboten och alla resurser som är kopplade till den. Om du tar bort resursgruppen raderas även allt innehåll i den. Därmed kan inga fler kostnader uppkomma. Det går inte att återställa borttagna resursgrupper, så se noga till så att du är helt klar med den innan du tar bort den.

1. Gå tillbaka till bladet för resursgruppen ”factbot-rg”. Klicka sedan på **Ta bort resursgrupp** överst på bladet.

    ![Ta bort resursgruppen](../images/delete-resource-group.png)

    _Ta bort resursgruppen_

1. För säkerhets skull måste du ange resursgruppens namn. (När du väl tar tagit bort en resursgrupp går det inte att återställa den.) Skriv namnet på resursgruppen. Klicka på knappen **Ta bort** för att ta bort alla spår av den här labbuppgiften från din Azure-prenumeration.

Efter några minuter tas resursgruppen och de resurser som finns i den bort. Debiteringen upphör när du klickar på **Ta bort**. Du kommer alltså inte att debiteras för den tid det tar att radera resurserna. På samma sätt inleds inte debiteringen förrän resurserna är fullständigt distribuerade och klara att köras.

### <a name="summary"></a>Sammanfattning

Det finns mycket mer du kan göra för att dra nytta av den inneboende kapaciteten hos Azure Bot Service. Du kan till exempel införliva [Dialogs](http://aihelpwebsite.com/Blog/EntryId/9/Introduction-To-Using-Dialogs-With-The-Microsoft-Bot-Framework), [FormFlow](https://blogs.msdn.microsoft.com/uk_faculty_connection/2016/07/14/building-a-microsoft-bot-using-microsoft-bot-framework-using-formflow/) och [Microsoft Language Understanding and Intelligence Services (LUIS)](https://docs.botframework.com/node/builder/guides/understanding-natural-language/). Med dessa, och andra funktioner, kan du skapa sofistikerade robotar som svarar på användarnas frågor och kommandon samt interagerar på ett smidigt, talspråkligt och icke-linjärt sätt. Mer information och tips för att komma igång finns i [översikten Vad är Microsoft Bot Framework](https://blogs.msdn.microsoft.com/uk_faculty_connection/2016/04/05/what-is-microsoft-bot-framework-overview/).