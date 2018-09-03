I den här övningen ska du skapa ett nytt lagringskonto i din Azure-prenumeration. Du kommer sedan att använda Azure Cloud Shell för att skapa en ny kö, lägga till ett meddelande till den och sedan läsa meddelandet och ta bort det från kön.

Det här är samma åtgärder som vidtas av komponenterna i ett distribuerat program. En mobilapp kan till exempel lägga till ett meddelande till en kö där det väntar på att en webbtjänst ska hämta och bearbeta det.

## <a name="create-a-storage-account"></a>Skapa ett lagringskonto

Eftersom Azure Storage-köer är en del av Azure-lagringskonton av typen generell användning måste du börja med att skapa ett lagringskonto:

1. Öppna en webbläsare och gå till [Azure-portalen](http://portal.azure.com) och logga in med dina vanliga autentiseringsuppgifter.
1. Klicka på **Alla tjänster** i det övre vänstra hörnet.
1. Rulla ned till avsnittet **Lagring** och klicka på **Lagringskonton**.
1. Klicka på **Lägg till** längst upp till vänster på bladet **Lagringskonton**.

    ![Skärmbild bladet Lagringskonton med Lägg till markerat](../images/5-create-a-storage-account-1.png)

1. Ange ett unikt namn för lagringskontot i textrutan **Namn**.
1. Kontrollera att **Resource Manager** är valt under **Distributionsmodell**.
1. Välj **Lagring (general-purpose v2)** i listrutan **Typ av konto**.
1. Välj en region nära dig i listrutan **Plats**.
1. Välj **Lokalt redundant lagring (LRS)** i listrutan **Replikering**.
1. Välj **Standard** under **Prestanda**.
1. Välj **Lågfrekvent** under **Åtkomstnivå**.
1. Välj **Inaktiverat** under **Säker överföring krävs**.
1. Välj din prenumeration under **Prenumeration**.

    ![Skärmbild av dialogrutan Skapa lagringskonto](../images/5-create-a-storage-account-2.png)

1. Välj **Skapa ny** under **Resursgrupp**. Skriv **MusicSharingResourceGroup** i textrutan.
1. Välj **Inaktiverat** under **Virtuella nätverk** och klicka sedan på **Skapa**.

    ![Skärmbild av dialogrutan Skapa lagringskonto med Skapa markerat](../images/5-create-a-storage-account-3.png)

Azure skapar det nya lagringskontot och den nya resursgruppen.

## <a name="create-a-queue"></a>Skapa en kö

Nu när lagringskontot har skapats kan du lägga till en ny kö till det. Du måste skapa kön med hjälp av PowerShell-kommandon:

1. Klicka på länken **Cloud Shell** uppe till höger i portalen.

    ![Skärmbild av Azure-portalen med ikonen Cloud Shell markerat](../images/5-create-a-storage-queue-1.png)

1. På skärmen **Välkommen till Azure Cloud Shell** klickar du på **PowerShell (Linux)**.
1. Om skärmen **Du har ingen lagring monterad** visas klickar du på **Skapa lagring**.
1. Skapa lagringskontot genom att skriva följande kommando när kommandotolken `PS Azure` visas. Ersätt `<storageaccountname>` med det unika namnet för ditt lagringskonto och tryck sedan på retur:

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. Ange följande kommando och tryck sedan på retur för att få kontexten för lagringskontot:

    ```powershell
    $context = $storageaccount.Context
    ```

1. Ange följande kommando och tryck sedan på retur för att skapa en ny kö:

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a>Lägga till ett meddelande till kön

Nu när du har skapat en kö i lagringskontot kan du lägga till ett meddelande till det.

1. Ange följande kommando och tryck sedan på retur för att skapa ett nytt meddelande:

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. Ange följande kommando och tryck sedan på retur för att lägga till det nya meddelandet till den nya kön:

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. Klicka på **Alla resurser** i menyn på vänster sida i Azure-portalen.
1. Klicka på lagringskontot du skapade tidigare i listan över resurser.
1. Klicka på **Storage Explorer (Förhandsversion)** på bladet för lagringskontot.
1. Klicka på **musicsharingmessages** i Storage Explorer under **KÖER**. Storage Explorer visar meddelandet du just lade till.

## <a name="retrieve-and-remove-the-message"></a>Hämta och ta bort meddelandet

En målkomponent för ett meddelande i en lagringskö måste hämta meddelandet längst fram i kön. Målkomponenten måste sedan bearbeta meddelandet och ta bort det från kön så att andra komponenter inte hämtar det.

1. För att hämta meddelandet som är först i kön skriver du följande kommando och trycker sedan på retur i Cloud Shell:

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. Ange följande kommando och tryck sedan på retur för att visa meddelandet:

    ```powershell
    $retrievedMessage.AsString
    ```

1. Ange följande kommando och tryck sedan på retur för att visa alla egenskaper för meddelandet:

    ```powershell
    $retrievedMessage
    ```

1. Ange följande kommando och tryck sedan på retur för att ta bort meddelandet från kön:

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. Du kan uppdatera kövisningen i Azure-portalen genom att gå till bladet Lagringskonto. Klicka på **Översikt** och sedan på **Storage Explorer**.
1. Klicka på **musicsharingmessages** under **KÖER**. Storage Explorer visar att kön är tom eftersom du har tagit bort det enda meddelandet.

## <a name="cleanup"></a>Rensa

Om du vill ta bort alla resurser som skapades under den här övningen anger du följande kommando i Cloud Shell: 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a>Sammanfattning

Här skapade du ett lagringskonto i din Azure-prenumeration och en ny kö i det. Du kan även använda PowerShell för att simulera åtgärderna för distribuerade programkomponenter genom att lägga till ett meddelande till kön och sedan hämta och ta bort det.

Köer för lagringskonton är en bra lösning när du vill skicka meddelanden mellan komponenterna i ett distribuerat program. Välj inte lagringsköer om du vill publicera händelser.