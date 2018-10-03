I den här övningen ska vi skapa en Azure-funktion som visar namnet och storleken för en blob när den har skapats eller uppdaterats.

## <a name="create-a-blob-trigger"></a>Skapa en blobutlösare

Vi ska fortsätta att använda vårt befintliga Azure Functions-program och lägger till en blobutlösare.

1. Kontrollera att du är inloggad på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Navigera till skärmen **Alla resurser** och välj din funktionsapp.

1. Peka på **Funktioner** och välj plustecknet (+).

1. Välj **Blob-utlösare**.

1. Lämna standardvärdet för **Namn**.

1. Lämna standardvärdet för **Sökväg**.

1. Välj den _nya_ länken bredvid listrutan **lagringskontoanslutning**. I bladet som öppnas klickar du på **Skapa ny**. Ange ett unikt namn för det nya lagringskontot och välj **OK** att skapa lagringskontot och stänga fönstret.

1. När du har kommit tillbaka till skärmen Ny funktion väljer du **Skapa** för att skapa funktionen.

## <a name="create-a-blob-container"></a>Skapa en blobcontainer

Nu när vi har skapat en blobutlösare ska vi använda Storage Explorer för att skapa en blob och utlösa funktionen.

1. Öppna det lagringskonto som du valde att använda (eller skapade) på en ny flik.

    > [!TIP]
    > Du kan duplicera en flik i de flesta webbläsare genom att högerklicka på fliken i fråga och välja **Duplicera** från menyn som visas. Vi vill använda en ny flik så att vi kan växla mellan de två tjänsterna som vi arbetar med.

1. Välj **Lagringskonton** i sidopanelen eller välj **Alla resurser** i sidopanelen och filtrera sedan efter namn.

1. Klicka på sektionen **Storage Explorer (förhandsversion)** för att öppna ett nytt fönster där du kan arbeta med blobbar och filer.

Kom ihåg att vår blobutlösare endast övervakar den plats som beskrivs i fältet **Sökväg**. Som standard bör sökvägen vara:

> samples-workitems/{namn}

Vi behöver skapa en container med namnet **samples-workitems**.

1. Högerklicka på **BLOBCONTAINRAR** och välj **Skapa blobcontainer**.

1. Ange **samples-workitems** som namn och lämna standardinställningen **Privat** för åtkomstnivån och välj **OK**.

## <a name="turn-on-your-blob-trigger"></a>Aktivera blobutlösaren

Nu när vi har skapat containern som ska övervakas kör vi vår funktion så att vi ser utdata när en blob skapas.

1. Gå tillbaka till webbläsarfliken med din Azure-funktion (eller öppna den igen).

1. Välj blobutlösaren för att öppna kodskärmen.

1. Öppna **Loggpanelen** längst ned på skärmen.

## <a name="create-a-blob"></a>Skapa en blob

Nu är blobutlösaren aktiverad och lyssnar efter aktivitet. Nu ska vi skapa en blob och se om vi får ett loggmeddelande.

1. Gå tillbaka till webbläsarfliken med Storage Explorer.

1. I Storage Explorer väljer du containern **samples-workitems** från listan **BLOBBCONTAINER**.

1. Välj **Överför** från verktygsfältet.

1. Välj en fil från datorn.

1. Under **Autentiseringstyp**, välj **SSH**.

1. Välj **Överför**.

1. Gå tillbaka till fliken Azure-funktion och kontrollera utdataloggarna för ett meddelande som visar vilken fil som laddades upp.