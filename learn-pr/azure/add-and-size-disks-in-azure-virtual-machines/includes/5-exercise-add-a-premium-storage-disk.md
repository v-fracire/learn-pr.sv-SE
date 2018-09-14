Om din virtuella dator (VM) är värd för ett disk-intensiva program, bör du använda premium storage för de virtuella hårddiskarna (VHD).

Till exempel att du vill lägga till en virtuell Hårddisk för att lagra utgående e-post på SMTP-servern VM. För att optimera prestanda för diskar som du bestämmer dig för premium storage för utgående e-post.

Vi lägger till en premium SSD till den virtuella datorn.

## <a name="sign-in-to-azure"></a>Logga in på Azure

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-premium-storage-account"></a>Skapa ett premium storage-konto

Premiumlagring för alla virtuella hårddiskar måste vara lagrad i ett premium storage-konto. Följ dessa steg om du vill skapa ett premium storage-konto.

1. Välj **lagringskonton** under **Favoriter** på den vänstra menyn i portalen.

1. Välj **+ Lägg till** på upp till vänster på den **lagringskonton** skärmen.

1. I den **skapa lagringskonto** fönstret som öppnas och ange följande egenskaper.

|Egenskap  |Värde  |Anteckningar  |
|---------|---------|---------|
|Namn     |    *Ett unikt namn (se anmärkning)*     |   Det här namnet måste vara unikt inom alla befintliga lagringskontonamn i Azure. Det måste vara 3 och 24 tecken långt och får bara innehålla gemena bokstäver och siffror.      |
|Typ av konto     |  **Lagring (general-purpose v1)**       |         |
|Plats     |  *Välj samma plats som den virtuella datorn som du skapade tidigare*       |         |
|Replikering     |   **Lokalt redundant lagring (LRS)**      |  Välj det här värdet i listrutan. Om du vet vi skapar ett premium storage-konto och premium storage stöder LRS-replikering.       |
|Prestanda     |  **Premium**       | Premium storage-konton backas upp av SSD-enheter och ger prestanda med låg fördröjning.        |
|Resursgrupp     |  *Välj **Använd befintlig** och sedan <rgn>[Sandbox resursgruppens namn]</rgn>*      |  Vi vill hålla alla resurser tillsammans under samma resursgrupp.       |

När du har fyllt i den här dialogrutan, bör det se ut som på följande skärmbild. 

![”Skapa storage-konto” dialogrutan visar alla egenskaperna enligt anvisningarna.](../media-draft/create-premium-sa.png)

1. Välj **skapa** att starta processen för storage-konto. Den här processen kan ta en stund att slutföra. 

1. När du får ett meddelande om att distributionen av det nya lagringskontot har slutförts kan du välja **uppdatera** i storage kontolista för att visa premium storage-konto som vi skapade. Anteckna namnet på det här kontot, som kommer att användas i nästa steg.

## <a name="create-vhd-in-the-premium-storage-account"></a>Skapa virtuell Hårddisk i premium storage-konto

Nu kan du lägga till en ny virtuell Hårddisk till den virtuella datorn och ange premium storage-konto som dess plats. Följ de här stegen:

1. I navigeringsfönstret till vänster under **Favoriter**väljer **virtuella datorer**.

1. Välj i listan över virtuella datorer, **MailSenderVM**.

1. Under **inställningar** av den **MailSenderVM** configuration menyn till vänster, Välj **diskar**.

1. Under **datadiskar**väljer **Lägg till datadisk**.

1. I den **koppla ohanterade diskar** fönstret, ange följande egenskaper.


|Egenskap  |Värde  |Anteckningar  |
|---------|---------|---------|
|Namn     |   **MailSenderVMOutgoing**      |         |
|Källtyp     |  **Ny (tom disk)**       |   Välj det här värdet i listrutan.       |
|Kontotyp     |  **Premium SSD**       |  Välj det här värdet i listrutan.        |

1. Till vänster om den **lagringsbehållare** väljer **Bläddra**.

1. I listan över lagringskonton, hitta och väljer premium storage-konto som du skapade tidigare i den här enheten. Typ av posten visas som **Premium-LRS**.

1. Välj i listan över behållare, __+ behållare__.

1. I den **ny behållare** fönstret i den **namn** textrutan typ **virtuella hårddiskar** och välj sedan **OK**.

1. Välj i listan över behållare, **virtuella hårddiskar** och välj sedan **Välj**.

1. Gå tillbaka till den **Anslut ohanterad disk** väljer **OK**.

1. Gå tillbaka till den **MailSenderVM - diskar** väljer **spara**. När du lägger Azure till den nya premium storage disken till den virtuella datorn.

Våra virtuella datorn har en operativsystemdisk och en standarddisk en premium SSD-baserad disk.

> [!NOTE]
> Den nya disken måste initieras, partitionerade och formaterade innan den kan lagra data. De här stegen har uteslutits från den här övningen för att undvika upprepning. Om du vill utföra dessa uppgifter kan du slutföra stegen i den [partitionera och formatera en datadisk](../3-exercise-add-data-disks-to-azure-virtual-machines.yml##partition-and-format-a-data-disk) avsnitt i den föregående övningen.
