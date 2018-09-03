Ni ska titta på en annan typ av situation. Din organisation kanske är en skola som använder virtuella Windows-datorer till att starta testmiljöer där studenterna kan installera webbappar, konfigurera domäner och utforska tjänster och funktioner i Windows utan att skolans datorer påverkas. Lärarna kan sedan ansluta till de här virtuella datorerna via RDP när de ska kontrollera hur det går för studenterna. Vi ska utforska hur du kan skapa något som liknar detta med Azure.

## <a name="create-a-windows-vm"></a>Skapa en virtuell Windows-dator

Så här skapar du en virtuell Windows-dator:

1. Logga in på Azure via [Azure Portal](https://portal.azure.com?azure-portal=true).

1. Klicka på **Skapa en resurs** uppe till vänster i Azure Portal.

1. I **sökfältet** anger du **Windows Server 2016 Datacenter** och klickar på länken med samma namn.

### <a name="configure-the-vm-settings"></a>Konfigurera inställningar för den virtuella datorn

1. Under **Grundläggande inställningar**, i fältet **Namn**, anger du ett namn på den virtuella datorn, till exempel ”StudentVM”.

1. I fältet **Typ av virtuell datordisk** klickar du på listrutan så att du ser alternativen. Se till att **SSD** är valt.

1. I fältet **Användarnamn** anger du ett lämpligt användarnamn för inloggning på den virtuella datorn.

1. I fältet **Lösenord** anger du ett lösenord som är minst 12 tecken långt. Det måste dessutom innehålla versaler och gemener, siffror och specialtecken.

1. Under **Resursgrupp** klickar du på **Skapa ny**. Ge resursgruppen ett namn, till exempel ”myTestRG”.

1. Välj en lämplig plats för den virtuella datorn och klicka på **OK**.

### <a name="select-the-vm-image-size-and-options"></a>Välj storlek och alternativ för den virtuella datorn

1. På sidan **Välj en storlek** klickar du på avbildningen **B1s** och sedan på **Välj**.

   > [!Note] 
   > B1-avbildningen har endast 1 GB RAM och genererar minnesfel när du först loggar in. Du kommer dock ändra storlek på avbildningen i en senare övning.

1. På sidan **Inställningar**, under **Välj offentliga inkommande portar**, väljer du **Inga offentliga inkommande portar**. Vi konfigurerar RDP-åtkomsten senare.

### <a name="finish-configuring-the-vm-and-create-the-image"></a>Slutför konfigurationen av den virtuella datorn och skapa avbildningen

1. Bläddra längst ned på sidan och klicka på **OK**.

1. Kontrollera dina inställningar under **Skapa**. Klicka på **Skapa** längst ned. På instrumentpanelen i Azure ser du den virtuella dator som distribueras. Det här kan ta flera minuter.

## <a name="summary"></a>Sammanfattning

Nu har du skapat en virtuell Windows Server-dator som passar för studenternas behov och som kan nås från skolans nätverk. I nästa övning får du lära dig att ansluta till och hantera den virtuella datorn via RDP.
