I den här övningen anta att ditt företag körs en Simple Mail Transfer Protocol (SMTP) e-postservern. Du vill migrera den här servern till Azure. Du vill lagra inkommande meddelanden för din egen domän i en mapp med namnet ”släppa” på en dedikerad virtuell Hårddisk med SMTP-servern.

Målet med den här övningen är att skapa en Windows-dator (VM) och koppla en ny virtuell hårddisk (VHD) kallas ”inkommande” för att lagra ”målkatalog”.

## <a name="sign-in-to-azure"></a>Logga in på Azure

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-windows-vm-in-the-azure-portal"></a>Skapa en virtuell Windows-dator i Azure portal

Följ dessa steg om du vill skapa en virtuell dator som värd för SMTP-servern med dess enheter:

1. Välj **skapa en resurs** i det övre vänstra hörnet i Azure Portal.

1. I sökrutan ovanför listan över resurser i Azure Marketplace, Sök efter och välj **Windows Server 2016 Datacenter**, och välj sedan **skapa**.

1. I den **grunderna** fönstret som öppnas till höger, ange följande egenskapsvärden. 


|Egenskap  |Värde  |Anteckningar  |
|---------|---------|---------|
|Namn     |   **MailSenderVM**      |         |
|Typ av virtuell datordisk     |  **Standard HDD**       |   Välj det här värdet i listrutan.      |
|Användarnamn     |  **mailmaster**       |         |
|Lösenord     |  Lösenordet måste vara minst 12 tecken långt och uppfylla [de definierade kraven på komplexitet](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).       | Se till att komma ihåg det här användarnamnet och lösenordet eftersom vi ska använda dem i modulen.         |
|Prenumeration     |  Välj din prenumeration.       |  Välj det här värdet i listrutan.       |
|Resursgrupp     |  Välj **Använd befintlig** och välj <rgn>[Sandbox resursgruppens namn]</rgn>.       |  Vi kan samla in alla resurser som används i den här modulen i en resursgrupp.       |
|Plats     |   En plats nära dig.      | Välj det här värdet i listrutan.        |

4. Välj **OK** längst ned på den **grunderna** sidan för att fortsätta till den **storlek** konfigurationsruta.

1. I den **storlek** konfigurationsruta, Sök efter och välj **B1ms**, och klicka sedan på **Välj**.

1. I den **inställningar** fönstret lämnar de flesta värden oförändrade. Under **Använd hanterade diskar**, klickar du på **nr**. Vi kommer att diskutera hanterade diskar senare i den här modulen.

1. I den **Välj offentliga inkommande portar** listrutan **RDP (port 3389)**. Vi använder den här porten fjärråtkomst till våra virtuella datorn när den har skapats.

1. Lämna de andra inställningarna på standardnivå och klicka sedan på **OK**.

1. I den **skapa** fönstret Granska konfigurationen.

1. När du har granskat konfigurationen, välja **skapa**. Azure skapar och startar den nya virtuella datorn.

> [!TIP]
> Skapa din virtuella dator och distribuera den i Azure kan ta några minuter. Du kan se förloppet i den **meddelanden** hub. Azure visar dialogruta med ett meddelande när den är klar.

## <a name="add-an-empty-data-disk-to-our-vm"></a>Lägga till en tom datadisk till våra VM

Vi kommer att namnge disken lagrar ”släpp”-katalogen för din SMTP-server ”inkommande”. Nu ska vi lägga till en ny tom disk till servern med följande steg:

1. I navigeringsfönstret till vänster under **Favorits**väljer **virtuella datorer**.

1. Välj i listan över virtuella datorer, **MailSenderVM**.

1. Under **inställningar** av den **MailSenderVM** configuration menyn till vänster, Välj **diskar**.

1. Under **datadiskar**väljer **Lägg till datadisk**.

1. I den **koppla ohanterade diskar** fönstret, ange följande egenskaper.

|Egenskap  |Värde  |Anteckningar  |
|---------|---------|---------|
|Namn     |   **MailSenderVMIncoming**      |         |
|Typ av datakälla     |  **Ny (tom disk)**       |   Välj det här värdet i listrutan.       |
|Kontotyp     |  **Standard HDD**       |  Välj det här värdet i listrutan.        |

6. Till vänster om den **lagringsbehållare** väljer **Bläddra**.

1. Sök efter lagringskontot vars namn börjar med i listan över lagringskonton, **mailinfrastructure** och markera den.

1. I listan över behållare, klickar du på **virtuella hårddiskar** och välj sedan **Välj**.

1. Gå tillbaka till den **Anslut ohanterad disk** väljer **OK**.

1. Gå tillbaka till den **MailSenderVM - diskar** väljer **spara**.

Vi har nu definierat en disk med namnet **MainSenderVMIncoming**. Om du vill använda disken måste behöver vi först partitionera och formatera den när vi loggar in på den virtuella datorn.

## <a name="partition-and-format-a-data-disk"></a>Partitionera och formatera en datadisk

Precis som med fysiska diskar, initiera och formatera en partition på en virtuell Hårddisk innan den kan användas.

### <a name="log-into-our-windows-vm-using-rdp"></a>Logga in på vår Windows virtuell dator med RDP

1. I den **MailSenderVM** VM-huvudskärmen väljer **översikt**.

1. Välj **Connect** från upp till vänster på skärmen.

1. I den **Anslut till den virtuella datorn** dialogrutan som öppnas till höger, Välj **ladda ned till RDP-filen**.

   ![Skärmbild av dialogrutan ”Anslut till virtuell dator” med knappen ”Hämta RDP-filen” är markerad.](../media-draft/download-rdp.png)

4. En fil som heter **MailSenderVM.rdp** hämtas till din lokala `Downloads` mapp. Den här filen är konfiguration av fjärrskrivbord-fil för den virtuella datorn MailSenderVM. Öppna filen för att starta anslutningen.

1. I den **anslutning till fjärrskrivbord** dialogrutan klickar du på **Connect**.

1. Klicka på **Använd ett annat konto** i dialogrutan **Windows-säkerhet**.

1. I den **användarnamn** textrutan typ **mailmaster**.

1. I den **lösenord** textrutan skriver du lösenordet som du angav för det här användarnamnet i den här övningen. 

1. I den **anslutning till fjärrskrivbord** dialogrutan klickar du på **Ja**.

En fjärrskrivbordssession till den virtuella datorn startas nu. Det kan ta en stund att logga in för första gången. När inloggningen är klar visas den **Serverhanteraren** verktyget kommer att visas på skärmen.

### <a name="partition-and-format-our-data-disk-using-server-manager"></a>Partitionera och formatera vår datadisk med hjälp av Serverhanteraren

1. När **Serverhanteraren** visas, väljer **fil- och lagringstjänster** i navigeringsfönstret till vänster.

1. Under **volymer**väljer **diskar**.

1. I listan över diskar disk **0** är operativsystemet disk och disk **1** är den temporära disken. Väljer disk **2**, vilket är den nya virtuella Hårddisken som du just lade till.

1. Överst på den **volymer** väljer **uppgifter** följt av **ny volym...** . På menyn är uppe till höger på skärmen på följande sätt.

   ![Skärmbild av menyn ”aktiviteter” expanderas för att visa tre menyalternativ. De är ”ny volym...”, ”Sök igenom lagring” och ”uppdatera”.](../media-draft/tasks-menu.png)


1. I den **ny volym** guiden klickar du på **nästa**.

1. I den **Välj server och disk** väljer **MailSenderVM** och **Disk 2**, och klicka sedan på **nästa**.

1. I den **Offline eller Uninitiated Disk** dialogrutan klickar du på **OK**.

1. I den **ange storleken på volymen** klickar du på **nästa**.

1. I den **tilldela en enhetsbeteckning** väljer **F:** följt av **nästa**.

1. I den **Välj filsysteminställningar** sidan den **volymetikett** textrutan typ **inkommande** och välj sedan **nästa**.

1. I den **bekräfta val** väljer **skapa**. Windows initierar disken och formaterar den nya partitionen.

1. I den **slutförande** väljer **Stäng**.

Låt oss ta en titt på våra nya diskvolymen i Utforskaren.

1. Öppna **Utforskaren**.

1. I navigeringsfönstret till vänster, klickar du på **den här datorn** och dubbelklicka sedan på **inkommande (F:)**.

1. Välj **Start**, och sedan **ny mapp**.

1. Typ **släppa** och tryck sedan på RETUR.

Nu har vi en ny volym som skapats på våra virtuella Hårddisken som kallas **inkommande**, och vi har skapat en mapp med namnet **släppa** på volymen.  

1. Stäng Windows Explorer.

1. På den **Aktivitetsfältet**, klickar du på den **starta** knapp, klickar du på den **Power** knappen och klicka sedan på **Disconnect**.

Grattis! Du har har skapat en virtuell Windows-dator, kopplade en ny virtuell Hårddisk, skapat en volym på den virtuella Hårddisken och lade till en mapp till volymen. Om du kommer ihåg disktyp som vi använde för den nya virtuella Hårddisken har en **Standard HDD**. I nästa enhet Lär vi skillnaderna mellan standard och premium storage. 
