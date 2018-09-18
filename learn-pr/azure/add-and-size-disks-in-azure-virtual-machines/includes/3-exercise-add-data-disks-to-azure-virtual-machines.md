I den här övningen antar vi att ditt företag kör en SMTP-e-postserver (Simple Mail Transfer Protocol). Du vill migrera den här servern till Azure. Du vill att SMTP-servern ska lagra inkommande meddelanden för din egen domän i en mapp med namnet ”Drop” på en dedikerad virtuell hårddisk.

Målet med den här övningen är att skapa en virtuell Windows-dator (VM) och ansluta en ny virtuell hårddisk (VHD) med namnet ”Incoming” (Inkommande) för att lagra katalogen ”Drop”.

## <a name="sign-in-to-azure"></a>Logga in på Azure
<!---TODO: Need update for sanbox?--->

1. Logga in på [Azure-portalen](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-windows-vm-in-the-azure-portal"></a>Skapa en virtuell Windows-dator i Azure-portalen

Skapa en virtuell dator som värd för SMTP-servern med dess dataenheter genom att följa dessa steg:

1. Välj **Skapa en resurs** längst upp till vänster i Azure-portalen.

1. I sökrutan ovanför listan över resurser i Azure Marketplace söker du efter och väljer **Windows Server 2016 Datacenter** och därefter **Skapa**.

1. I fönsterrutan **Basics** (Grundläggande) som öppnas till höger anger du följande egenskapsvärden. 


|Egenskap  |Värde  |Anteckningar  |
|---------|---------|---------|
|Namn     |   **MailSenderVM**      |         |
|Typ av virtuell datordisk     |  **Standard HDD**       |   Välj det här värdet i listrutan.      |
|Användarnamn     |  **mailmaster**       |         |
|Lösenord     |  Lösenordet måste vara minst 12 tecken långt och uppfylla [de definierade kraven på komplexitet](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).       | Se till att komma ihåg det här användarnamnet och lösenordet eftersom vi kommer använda dem i hela modulen.         |
|Prenumeration     |  Välj din prenumeration.       |  Välj det här värdet i listrutan.       |
|Resursgrupp     |  Välj **Skapa nytt** och skriv sedan **MailInfrastructure**.       |  Vi samlar in alla resurser som används i den här modulen till en resursgrupp.       |
|Plats     |   En plats nära dig.      | Välj det här värdet i listrutan.        |

4. Välj **OK** längst ned på sidan **Basics** (Grundläggande) för att fortsätta till konfigurationsrutan **Storlek**.

1. I konfigurationsrutan **Storlek** söker du efter och väljer **B1ms**, och sedan klickar du på **Välj**.

1. I fönsterrutan **Inställningar** går du till **Använd hanterade diskar** och klickar på **Nej**. Vi går igenom hanterade diskar senare i den här modulen.

1. I listrutan **Välj offentliga inkommande portar**  väljer du **RDP (3389)**. Vi använder den här porten för att fjärransluta till den virtuella datorn när den har skapats.

1. Lämna alla de andra inställningarna på deras standardvärden och klicka sedan på **OK**.

1. I fönsterrutan **Skapa** granskar du konfigurationen.

1. När du har granskat konfigurationen väljer du **Skapa**. Azure skapar och startar den nya virtuella datorn.

> [!TIP]
> Det kan ta några minuter att skapa den virtuella datorn och distribuera den i Azure. Du kan övervaka förloppet i hubben **Meddelanden**. Azure visar en dialogruta med ett meddelande när det är klart.

## <a name="add-an-empty-data-disk-to-our-vm"></a>Lägga till en tom datadisk till den virtuella datorn

Vi kommer att ge den disk som lagrar katalogen ”Drop” för SMTP-servern namnet ”Incoming” (Inkommande). Vi lägger till en ny tom disk till servern med följande steg:

1. I navigeringsfönstret till vänster går du till **FAVORITER** och väljer **Virtuella datorer**.

1. I listan över virtuella datorer väljer du **MailSenderVM**.

1. Under **INSTÄLLNINGAR** i konfigurationsmenyn **MailSenderVM** till vänster väljer du **Diskar**.

1. Under **Datadiskar** väljer du **Lägg till datadisk**.

1. I fönsterrutan **Attach unmanaged disks** (Anslut ohanterade diskar) anger du följande egenskaper.


|Egenskap  |Värde  |Anteckningar  |
|---------|---------|---------|
|Namn     |   **MailSenderVMIncoming**      |         |
|Källtyp     |  **Ny (tom disk)**       |   Välj det här värdet i listrutan.       |
|Kontotyp     |  **Standard HDD**       |  Välj det här värdet i listrutan.        |


6. Till vänster om fältet **Lagringscontainer** väljer du **Bläddra**.

1. I listan över lagringskonton söker du efter det lagringskonto vars namn börjar med **mailinfrastructure** och väljer det.

1. I listan över containrar klickar du på **vhds** och sedan **Välj**.

1. När du är tillbaka på skärmen **Attach unmanaged disk** (Anslut ohanterad disk) väljer du **OK**.

1. När du är tillbaka på skärmen **MailSenderVM - Disks** (Diskar) väljer du **Spara**.

Vi har nu definierat en disk med namnet **MainSenderVMIncoming**. För att använda disken behöver vi först partitionera och formatera den när vi loggar in på den virtuella datorn. 

## <a name="partition-and-format-a-data-disk"></a>Partitionera och formatera en datadisk

På samma sätt som med fysiska diskar initierar och formaterar du en partition på en virtuell hårddisk innan den kan användas.

### <a name="log-into-our-windows-vm-using-rdp"></a>Logga in på vår virtuella Windows-dator med hjälp av RDP

1. På huvudskärmen för den virtuella datorn **MailSenderVM** väljer du **Översikt**.

1. Välj **Anslut** längst upp till vänster på översiktsskärmen.

1. I dialogrutan **Ansluta till den virtuella datorn** som öppnas till höger väljer du **Download to RDP File** (Ladda ned till RDP-fil).

   ![Skärmbild av dialogrutan ”Ansluta till den virtuella datorn” med knappen ”Download RDP file” (Ladda ned RDP-fil) markerad.](../media-draft/download-rdp.png)

4. En fil som heter **MailSenderVM.rdp** laddas ned till din lokala `Downloads`-mapp. Den här filen är konfigurationsfilen för fjärrskrivbord för den virtuella datorn MailSenderVM. Öppna filen för att starta anslutningsprocessen.

1. I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **Anslut**.

1. I dialogrutan **Windows-säkerhet** klickar du på **Använd ett annat konto**.

1. I textrutan **Användarnamn** skriver du **mailmaster**.

1. I textrutan **Lösenord** skriver du det lösenord som du angav för det här användarnamnet i den här övningen. 

1. I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **Ja**.

En Fjärrinloggningssession till den virtuella datorn startas nu. Det kan ta en stund att logga in för första gången. När inloggningen är klar visas verktyget **Serverhanteraren** på skärmen.

### <a name="partition-and-format-our-data-disk-using-server-manager"></a>Partitionera och formatera datadisken med hjälp av Serverhanteraren

1. När **Serverhanteraren** visas väljer du **File and Storage Services** (Fil- och lagringstjänster) i navigeringsfönstret till vänster.

1. Under **Volymer** väljer du **Diskar**.

1. I listan över diskar är disk **0** operativsystemdisken och disk **1** är den temporära disken. Väljer disk **2**, som är den nya virtuella hårddisken som du just lade till.

1. Överst på fönsterrutan **VOLYMER** väljer du **UPPGIFTER** följt av **New Volume...** (Ny volym...). Menyn finns längst upp på skärmen enligt följande.

   ![Skärmbild på menyn ”UPPGIFTER” expanderad för att visa tre menykommandon. De är ”New Volume...” (Ny volym...), ”Rescan Storage” (Skanna om lagring) och ”Refresh” (Uppdatera).](../media-draft/tasks-menu.png)


1. I guiden **Ny volym** klickar du på **Nästa**.

1. På sidan **Select server and disk** (Välj server och disk) väljer du **MailSenderVM** och **Disk 2** och klickar sedan på **Nästa**.

1. I dialogrutan **Offline or Uninitiated Disk** (Offlinedisk eller oinitierad disk) klickar du på **OK**.

1. På sidan **Specify the size of the volume** (Ange storlek på volymen) klickar du på **Nästa**.

1. På sidan **Assign a drive letter** (Tilldela en enhetsbeteckning) väljer du **F:** följt av **Nästa**.

1. På sidan **Select file system settings** (Välj filsysteminställningar) går du till textrutan **Volume label** (Volymetikett) och skriver **Inkommande** och väljer sedan **Nästa**.

1. På sidan **Confirm selections** (Bekräfta val) väljer du **Skapa**. Windows initierar disken och formaterar den nya partitionen.

1. På sidan **Completion** (Slutförande) väljer du **Stäng**.

Nu tittar vi på vår nya diskvolym i Utforskaren. 
1. Öppna **Utforskaren**.

1. I navigeringsfönstret till vänster klickar du på **Den här datorn** och dubbelklickar sedan på **Inkommande (F:)**.

1. Välj **Start** och sedan **Ny mapp**.

1. Typ **Drop** och tryck sedan på Retur.

Nu har vi en ny volym som skapats på vår virtuella hårddisk som kallas **Incoming** (Inkommande), och vi har skapat en mapp med namnet **Drop** på volymen.  

1. Stäng Utforskaren.

1. På **Aktivitetsfältet** klickar du på **Start-knappen** följt av **Strömknappen** och sedan på **Disconnect** (Koppla från).

Grattis! Du har skapat en virtuell Windows-dator, anslutit en ny virtuell hårddisk, skapat en volym på den virtuella hårddisken och lagt till en mapp i den volymen. Som du kanske kommer ihåg är den disktyp som vi använde för den nya virtuella hårddisken **Standard HDD**. I nästa enhet går vi igenom skillnaderna mellan Standard- och Premium-lagring. 
