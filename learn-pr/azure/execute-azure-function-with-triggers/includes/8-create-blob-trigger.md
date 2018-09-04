I den här övningen ska vi skapa en Azure-funktion som visar namnet och storleken för en blob när den har skapats eller uppdaterats. 

> [!NOTE]
> För att kunna slutföra den här övningen måste du vara inloggad på [Azure Portal](https://portal.azure.com?azure-portal=true) med ett giltigt konto.

## <a name="create-a-blob-trigger"></a>Skapa en blobutlösare

Vi ska fortsätta att använda vårt befintliga Azure Functions-program och lägger till en blobutlösare.

1. Peka på **Funktioner** och välj plustecknet (+).

    ![Peka på Funktioner och välj plustecknet](../media-drafts/4-hover-function.png)

2. Välj **Anpassad funktion** och sedan **Blob-utlösare**.

3. Välj **C#** som språk. 

4. Lämna standardvärdet för **Namn**.

5. Lämna standardvärdet för **Sökväg**.

6. Välj ett befintligt Azure Storage-konto eller välj **Skapa** om du vill att Azure ska skapa ett nytt konto åt dig.

## <a name="create-a-blob-container"></a>Skapa en blobcontainer

Nu när vi har skapat en blobutlösare ska vi använda Storage Explorer för att skapa en blob och utlösa funktionen.

1. Öppna det lagringskonto som du valde att använda (eller skapade) på en ny flik. Ett enkelt sätt att göra detta är att öppna en ny flik på Azure Portal och klicka på **Lagringskonton** i sidopanelen eller att använda **Alla tjänster** i sidopanelen och sedan filtrera efter namn. Vi vill använda en ny flik så att vi kan växla mellan de två tjänsterna som vi arbetar med.

2. Klicka på **Storage Explorer (förhandsversion)**. Nu öppnas ett nytt blad där du kan arbeta med blobar och filer.

Kom ihåg att vår blobutlösare endast övervakar den plats som beskrivs i fältet **Sökväg**. Som standard bör sökvägen vara:

> samples-workitems/{namn}

Vi behöver skapa en container med namnet **samples-workitems**.

3. Högerklicka på **BLOBCONTAINRAR** och välj **Skapa blobcontainer**.

4. Ange **samples-workitems** som namn och lämna standardinställningen **Privat** för åtkomstnivån.

## <a name="turn-on-your-blob-trigger"></a>Aktivera blobutlösaren

Nu när vi har skapat containern som ska övervakas kör vi vår funktion så att vi ser utdata när en blob skapas.

1. Växla tillbaka till fliken Azure-funktion (eller öppna den igen).

2. Välj blobutlösaren för att öppna kodskärmen.

3. Välj **Kör**. Utdatafönstret öppnas.

## <a name="create-a-blob"></a>Skapa en blob

Nu är blobutlösaren aktiverad och lyssnar efter aktivitet. Nu ska vi skapa en blob och se om vi får ett loggmeddelande.

1. Välj containern **samples-workitems** i Storage Explorer.

2. Välj **Överför** från verktygsfältet.

3. Välj en fil från datorn.

4. Välj **Överför**.

5. Gå tillbaka till fliken Azure-funktion och kontrollera utdataloggarna för ett meddelande som visar vilken fil som laddades upp.

## <a name="pause-the-function"></a>Pausa funktionen

Klicka på **Pausa** ovanför loggfönstret så att du inte debiteras för ytterligare begäranden.

![Pausa funktionen](../media-drafts/4-pause-timer.png)


