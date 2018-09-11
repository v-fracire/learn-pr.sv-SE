I den här övningen ska du skapa ett nytt lagringskonto i din Azure-prenumeration. Du kommer sedan att använda Azure Cloud Shell till att skapa en ny kö, lägga till ett meddelande i den, läsa meddelandet och sedan ta bort det från kön.

Det här är samma åtgärder som vidtas av komponenterna i ett distribuerat program. En mobilapp kan till exempel lägga till ett meddelande i en kö, där det väntar på att en webbtjänst ska hämta och bearbeta det.

## <a name="create-a-storage-account"></a>Skapa ett lagringskonto

Eftersom Azure Storage-köer ingår i Azure-lagringskonton för generell användning, måste du börja med att skapa ett lagringskonto:

1. I en webbläsare går du till [Azure Portal](https://portal.azure.com?azure-portal=true) och loggar in med dina vanliga autentiseringsuppgifter.

1. Klicka på **Alla tjänster** i det övre vänstra hörnet.

1. Rulla ned till avsnittet **Lagring** och klicka sedan på **Lagringskonton**.

1. Klicka på **Lägg till** längst upp till vänster på bladet **Lagringskonton**.

  ![Skärmbild av bladet Lagringskonton med Lägg till markerat](../media-draft/4-create-a-storage-account-1.png)

1. Ange följande information i dialogrutan som visas. Vart och ett av dessa alternativ har en `(i)`- ikon i portalen som du kan använda för att få mer information om alternativet.

    - Ange ett unikt namn för lagringskontot i textrutan **Namn**.
    - Kontrollera att **Resource Manager** är valt under **Distributionsmodell**.
    - Välj **Lagring (generell användning v2)** i listrutan **Typ av konto**.
    - Välj en region nära dig i listrutan **Plats**.
    - Välj **Lokalt redundant lagring (LRS)** i listrutan **Replikering**.
    - Välj **Standard** under **Prestanda**.
    - Under **Åtkomstnivå** väljer du **Lågfrekvent**.
    - Under **Säker överföring krävs** väljer du **Inaktiverat**.
    - Välj din prenumeration under **Prenumeration**.
    - Välj **Skapa ny** under **Resursgrupp**. Skriv **MusicSharingResourceGroup** i textrutan.
    - Under **Virtuella nätverk** väljer du **Inaktiverat**. 

    ![Skärmbild av dialogrutan Skapa lagringskonto](../media-draft/4-create-a-storage-account-2.png)

1. Klicka på **Skapa** – Azure skapar en ny resursgrupp och ett nytt lagringskonto som är associerade med den.

    ![Skärmbild av dialogrutan Skapa lagringskonto med Skapa markerat](../media-draft/4-create-a-storage-account-3.png)

## <a name="create-a-queue"></a>Skapa en kö

Nu när lagringskontot har skapats kan du lägga till en ny kö till det. Du måste skapa kön med hjälp av PowerShell-kommandon:

1. Klicka på länken **Cloud Shell** uppe till höger i portalen `(>_)`.

    ![Skärmbild av Azure Portal med ikonen Cloud Shell markerad](../media-draft/4-create-a-storage-queue-1.png)

1. På skärmen **Välkommen till Azure Cloud Shell** klickar du på **PowerShell (Linux)**.

1. Om skärmen **You have no storage mounted** (Du har ingen lagring monterad) visas klickar du på **Skapa lagring**.

1. Skapa lagringskontot genom att skriva följande kommando när kommandotolken `PS Azure` visas. Ersätt `<storageaccountname>` med det unika namnet för ditt lagringskonto som du skapade ovan och tryck sedan på **Retur**. Vi tilldelar det resulterande objektet till en variabel med namnet `$storageaccount`.

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. Därefter måste vi hämta lagringskontots _sammanhang_ – vilket är en egenskap för det returnerade objektet. Nu ska vi tilldela det till en annan variabel med namnet `$context`.

    ```powershell
    $context = $storageaccount.Context
    ```

1. Nu är vi redo att skapa kön. Använd kommandot `New-AzureStorageQueue` och tilldela den till en `$messageQueue`-variabel.
    - Skicka en `-Name`-parameter med värdet `musicsharingmessages`
    - Skicka en `-Context`-parameter med värdet som du hämtade i föregående steg.

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a>Lägga till ett meddelande i kön

Nu när du har skapat en kö på lagringskontot kan du lägga till ett meddelande i den.

1. Du skapar ett nytt meddelande med metoden `New-Object` där du skapar en .NET `CloudQueueMessage` med ett strängbaserat argument:

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. Om du vill lägga till det nya meddelandet i den nya kön, skickar du den skapade `CloudQueueMessage` till `AddMessageAsync`-metoden i din `$messageQueue`-kö.

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

## <a name="verify-the-message-was-queued"></a>Kontrollera att meddelandet placerades i kön

Vi kan använda **Storage Explorer** när vi arbetar med vår kö. Det finns två tillgängliga varianter:

- En plattformsoberoende skrivbordsapp för Linux, macOS och Windows som du kan hämta.
- En webbförhandsversion i Azure Portal. Det är den som vi använder här, men du kan installera skrivbordsversionen om du föredrar det – instruktionerna är mycket lika.

1. Klicka på **Alla resurser** i menyn på vänster sida i Azure Portal.

1. Klicka på lagringskontot du skapade tidigare i listan med resurser.

1. Klicka på **Storage Explorer (förhandsversion)** på bladet för lagringskontot.

1. Klicka på **musicsharingmessages** i Storage Explorer under **KÖER**. Storage Explorer bör nu visa meddelandet som du nyss lade till.

## <a name="retrieve-and-remove-the-message"></a>Hämta och ta bort meddelandet

En målkomponent för ett meddelande i en lagringskö måste hämta meddelandet som ligger först i kön. Målkomponenten måste sedan bearbeta meddelandet och ta bort det från kön så att andra komponenter inte hämtar det.

1. Vi kan hämta det första meddelande som är tillgängligt i PowerShell med hjälp av `GetMessageAsync`-metoden i vår kö. Det här är en asynkron .NET-metod, eftersom vi vill vänta tills vi bara kan använda `Result`-egenskapen för att hämta det returnerade värdet. Det här returnerar ett objekt som motsvarar det meddelande som vi kan tilldela till en parameter.

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. Vi kan få en textversion av meddelandet genom att anropa `AsString` – detta kommer att visa värdet i konsolen.

    ```powershell
    $retrievedMessage.AsString
    ```

1. Vi kan också visa alla egenskaper för meddelandet genom att bara skriva variabelnamnet och trycka på **Retur**.

    ```powershell
    $retrievedMessage
    ```

1. `GetMessageAsync` tar *inte* bort meddelandet – det bara returnerar det, vilket innebär att vi kan bearbeta det igen. Om du vill ta bort meddelandet från kön kan vi använda `DeleteMessageAsync`-metoden på kön – detta kräver att vi skickar in det meddelande som vi ta bort.

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. Om du vill kontrollera att meddelandet är borta, uppdaterar du kön som visas i Azure Portal genom att gå till bladet Lagringskonto och välja **Översikt > Storage Explorer**. Klicka på **musicsharingmessages** under **KÖER**. Storage Explorer bör nu visa att kön är tom eftersom du har tagit bort det enda meddelandet.


## <a name="summary"></a>Sammanfattning
Köer för lagringskonton är en bra lösning när du vill skicka _meddelanden_ mellan komponenterna i ett distribuerat program. Det här är ett bra alternativ om du vill garantera leveransen eller säkerställa att meddelandena levereras i samma ordning som du skickade dem. Köer innebär dock att avsändaren och mottagaren måste förstå formatet för de data som skickas – det finns ett underförstått datakontrakt mellan dem som lägger till en slags ”koppling” mellan de två kommunikationstjänsterna.

Det är inte alla arkitekturer som behöver skicka formaterade datablock, vissa skickar egentligen bara enkla meddelanden som vi kan läsa och glömma utan att behöva ha någon kunskap om vad som hanterar meddelandet. I dessa scenarion är användningen av en kö inte något bra alternativ. Låt oss titta på en annan strategi för meddelanden som är lämpligare än den här typen av kommunikation.