I den här övningen ska vi skapa en Azure-funktion som visar namn och storlek för en blob när den har skapats eller uppdaterats. 

> [!NOTE]
> För att kunna slutföra den här övningen måste du vara inloggad på [Azure-portalen](https://portal.azure.com/) med ett giltigt konto.

## <a name="create-a-blob-trigger"></a>Skapa en blob-utlösare

Vi fortsätter använda vårt befintliga Azure Functions-program och lägger nu till en blob-utlösare.

1. Peka på **Functions** och välj plustecknet (+).

    ![Peka på Functions och välj plustecknet](../media-drafts/4-hover-function.png)

1. Välj **Blob-utlösare**.

1. Välj **#C** som språk. 

1. Låt **Namn** vara inställt på standardvärdet.

1. Låt **Sökväg** vara inställt på standardvärdet.

1. Välj ett befintligt Azure Storage-konto eller välj **Skapa** om du vill att Azure ska skapa ett nytt konto åt dig.

## <a name="download-storage-explorer"></a>Hämta Lagringsutforskaren

Nu när vi har skapat en blob-utlösare kan vi hämta Lagringsutforskaren, som gör att vi enkelt kan skapa en blob.

- Hämta [Lagringsutforskaren](http://storageexplorer.com).

## <a name="connect-to-your-azure-storage-account"></a>Anslut till ditt Azure Storage-konto

Nu har vi hämtat Lagringsutforskaren. Låt oss logga in med de autentiseringsuppgifter som vi har fått.

1. Välj plustecknet (+) till vänster i Lagringsutforskaren.

1. Välj **Använd ett kontonamn och en nyckel för lagringen**.

1. Välj **Nästa**.

1. Under blob-utlösaren i Azure väljer du **Integrera**.

1. Välj **Dokumentation** för att expandera vyn.

1. Kopiera **Kontonamn** och **Kontonyckel**.

1. Gå tillbaka till Lagringsutforskaren och klistra in **Kontonamn** och **Kontonyckel**.

1. Ange ett **Visningsnamn**. Det här värdet är namnet på anslutningen i Lagringsutforskaren.

1. Välj **Nästa**.

1. Välj **Anslut**. 

## <a name="create-a-blob-container"></a>Skapa en blobcontainer

Vi är inte anslutna till vårt Azure Storage-konto. Kom ihåg att vår blob-utlösare endast övervakar den plats som beskrivs i fältet **Sökväg**. Som standard ska vår sökväg vara:

> samples-workitems/{namn}

Vi måste skapa en behållare med namnet **samples-workitems**.

1. Expandera ditt lagringskonto i Lagringsutforskaren. Namnet ska vara det **Visningsnamn** som du angav vid anslutningen.

1. Högerklicka på **Blobbehållare** och välj **Skapa blobbehållare**.

1. Ange **samples-workitems**.

## <a name="turn-on-your-blob-trigger"></a>Aktivera din blob-utlösare

Nu när vi har skapat vår behållare som ska övervakas, kör vi vår funktion för att kunna se utdata när en blob skapas.

1. Välj den blob-utlösare som öppnar kodskärmen.

1. Välj **Kör**.

## <a name="create-a-blob"></a>Skapa en blob

Vår blob-utlösare är nu igång och lyssnar efter aktivitet. Nu ska vi skapa en blob för att se om vi får ett loggmeddelande.

1. I Lagringsutforskaren väljer du behållaren **samples-workitems**.

1. Välj **Överför**. 

1. Välj **Överför filer**.

1. Välj en fil från datorn.

1. Välj **Överför**.

1. Gå tillbaka till Azure. Titta i dina loggar om det finns något meddelande som visar vilken fil som överfördes.

## <a name="clean-up"></a>Rensa

För att säkerställa att du inte debiteras för den här funktionen, väljer du **Pausa** ovanför loggfönstret.

![Pausa](../media-drafts/4-pause-timer.png)


