### <a name="delete-the-data-science-vm"></a>Ta bort en Data Science VM

I den här enheten ska du ta bort resursgruppen som skapades i övning 1 när du skapade en Data Science VM. Om du tar bort resursgruppen raderas även allt innehåll i den. Därmed kan inga fler kostnader uppkomma. Det går inte att återställa borttagna resursgrupper, så se noga till så att du är helt klar med den innan du tar bort den. **Tänk på att du inte distribuera den här resursgruppen längre än nödvändigt** eftersom en Data Science VM är ganska dyr i drift.

1. Gå tillbaka till bladet för resursgruppen som du skapade i övning 1. Klicka sedan på knappen **Ta bort resursgrupp** överst på bladet.

    ![Ta bort resursgruppen](../media-draft/6-delete-resource-group.png)

1. För säkerhets skull måste du ange resursgruppens namn. (När du väl tar tagit bort en resursgrupp går det inte att återställa den.) Skriv namnet på resursgruppen. Klicka på knappen **Ta bort** för att ta bort alla spår av den här modulen från din Azure-prenumeration.

Efter några minuter tas resursgruppen och de resurser som finns i den bort. Debiteringen upphör när du klickar på **Ta bort**. Du kommer alltså inte att debiteras för den tid det tar att radera resurserna. På samma sätt inleds inte debiteringen förrän resurserna är fullständigt distribuerade och klara att köras.

### <a name="summary"></a>Sammanfattning

Stegen i den här modulen är generella och kan även användas i samband med andra typer av bildklassificeringar. Du kan till exempel träna samma TensorFlow-modell till att känna igen kattbilder eller identifiera defekter på produkter på ett löpande band. Bildklassificering är ett av de vanligaste användningsområdena för maskininlärning idag och användbarheten kommer troligen att öka med tiden. Nu när du har tillägnat dig grundkunskaperna kan du prova att skapa egna modeller för bildklassificering. Kanske är du något nytt på spåren!