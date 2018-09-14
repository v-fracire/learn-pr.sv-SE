Du har redan skapat resurserna som är viktiga för din online bank-arkitektur. I den här övningen ska slutför du installationen genom att konfigurera säkerhetsinställningar, en backend-adresspool, hälsoregler för avsökning och belastningsutjämningsregler. När installationen är klar ska du testa belastningsutjämnaren genom att ta bort en virtuell dator från poolen och verifiera att klientbegäranden inte längre skickas till den här virtuella datorn.

## <a name="inbound-rules"></a>Regler för inkommande trafik

Skapa en inkommande Säkerhetsregel för inkommande HTTP-anslutningar som använder port 80.

1. I den [Azure-portalen](https://portal.azure.com/?azure-portal=true), i den vänstra menyn klickar du på **alla resurser**. Klicka sedan i resurslistan på **woodgrove NSG**.

1. På den **woodgrove NSG** bladet under **inställningar**, klickar du på **ingående säkerhetsregler**, och klicka sedan på **Lägg till**.

1. På den **Lägg till ingående säkerhetsregel** bladet anger du följande information:
    - Källa: **servicetagg**
    - Källtjänsttagg: **Internet**
    - Källportsintervall: **\***
    - Mål: **alla**
    - Målportsintervall: **80**
    - Protokoll: **TCP**
    - Åtgärd: **Tillåt**
    - Prioritet: **100**
    - Namn: **woodgrove-HTTP-rule**

1. Klicka på **Lägg till**.

Skapa en inkommande Säkerhetsregel för inkommande RDP-anslutningar som använder port 3389.

1. På den **woodgrove-NSG - reglerna för inkommande** bladet klickar du på **Lägg till**.

1. På den **Lägg till ingående säkerhetsregel** bladet anger du följande information:
    - Källa: **servicetagg**
    - Källtjänsttagg: **Internet**
    - Källportsintervall: **\***
    - Mål: **alla**
    - Målportsintervall: **3389**
    - Protokoll: **TCP**
    - Åtgärd: **Tillåt**
    - Prioritet: **200**
    - Namn: **woodgrove-RDP-rule**

1. Klicka på **Lägg till**.

## <a name="prepare-a-script-to-install-and-configure-iis-on-the-vms"></a>Förbereda ett skript för att installera och konfigurera IIS på de virtuella datorerna

1. Kopiera följande PowerShell-kommandon i en textredigerare:

    ```powershell
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # remove default htm file
    remove-item  C:\inetpub\wwwroot\iisstart.htm

    # Add a new htm file that displays server name
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Web page from <b>" + $env:computername + "</b>")
    ```

1. Spara filen lokalt som **ConfigureIIS.ps1**.

### <a name="install-and-configure-iis-on-vms"></a>Installera och konfigurera IIS på virtuella datorer

1. I Azure-portalen, i den vänstra menyn klickar du på **alla resurser**. Klicka sedan i resurslistan på **woodgrove SVR01**.

1. På sidan Översikt **Connect**.

1. I den **Anslut till den virtuella datorn** bladet klickar du på **ladda ned RDP-filen**. Klicka i popup-fönstret **öppna**.

1. Klicka på **Anslut** i fönstret **Anslutning till fjärrskrivbord**.

1. I den **Windows Security** dialogrutan klickar du på **fler alternativ**, och klicka sedan på **Använd ett annat konto**.

1. I den **Windows Security** dialogrutan Ange de autentiseringsuppgifter som du använde när du har etablerat de virtuella datorerna och klicka sedan på **OK**.

1. Om du får en **anslutning till fjärrskrivbord** varning och klicka på **Ja**.

1. Öppna mappen som innehåller skriptet på din lokala dator.

1. Kopiera **ConfigureIIS.ps1** till Urklipp.

1. Växla till RDP-session. Öppna Windows Explorer och klistra in **ConfigureIIS.ps1** till **C:\\**.

1. I RDP-session klickar du på **starta**, och öppna sedan **Windows PowerShell**.

1. I PowerShell-Kommandotolken skriver du följande kommando och tryck sedan på **RETUR:**

    ```powershell
    cd \
    ```

1. I PowerShell-Kommandotolken skriver du följande kommando och tryck sedan på **RETUR:**

    ```powershell
    .\ConfigureIIS.ps1
    ```

1. Stäng RDP-anslutningen.

1. Upprepa steg 1-13 för **woodgrove SVR02** och **woodgrove SVR03**.

## <a name="create-a-back-end-address-pool"></a>Skapa en backend-adresspool

Använd portalen för att skapa en backend-adresspool för offentlig belastningsutjämnare.

1. I Azure-portalen, i den vänstra menyn klickar du på **alla resurser**. Klicka sedan i resurslistan på **woodgrove-LB**.

1. På den **woodgrove-LB** bladet under **inställningar**, klickar du på **serverdelspooler**, och klicka sedan på **Lägg till**.

1. På den **Lägg till serverdelspool** bladet Lägg till följande information:
    - Name: **woodgrove-BEP**
    - Som är kopplad till: Välj **tillgänglighetsuppsättning**
    - Tillgänglighetsuppsättning: **woodgrove-AS**

1. Klicka på **lägga till en mål-IP-konfiguration**. I den **virtuella måldatorn** väljer **woodgrove SVR01**. I den **nätverks-IP-konfiguration** väljer du den Virtuella datorns IP-adresspool.

1. Klicka på **lägga till en mål-IP-konfiguration**. I den **virtuella måldatorn** väljer **woodgrove SVR02**, och i den **nätverks-IP-konfiguration** väljer du den Virtuella datorns IP-adresspool.

1. Klicka på **lägga till en mål-IP-konfiguration**. I den **virtuella måldatorn** väljer **woodgrove SVR03**, och i den **nätverks-IP-konfiguration** väljer du den Virtuella datorns IP-adresspool.

1. På den **Lägg till serverdelspool** bladet klickar du på **OK**.

1. Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.

## <a name="create-a-health-probe-for-the-load-balancer"></a>Skapa en hälsoavsökning för belastningsutjämnaren

Lägg sedan till en hälsoavsökning för HTTP via port 80.

1. På den **woodgrovelb** bladet under **inställningar**, klickar du på **hälsoavsökningar**, och klicka sedan på **Lägg till**.

1. På den **Lägg till hälsoavsökning** bladet Lägg till följande information:
    - Name: **woodgrove-HP**
    - Protokoll: **HTTP**
    - Port: **80**
    - Sökväg: **/**
    - Intervall: **15**
    - Tröskelvärde för ej felfri: **2**

1. På den **Lägg till hälsoavsökning** bladet klickar du på **OK**.

1. Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.

## <a name="create-a-load-balancer-rule"></a>Skapa en lastbalanseringsregel

Skapa slutligen en belastningsutjämning regel för HTTP via port 80, som associerar backend poolen med hälsoavsökningen.

1. På den **woodgrove-LB** bladet under **inställningar**, klickar du på **belastningsutjämningsregler**, och klicka sedan på **Lägg till**.

1. På den **Lägg till belastningsutjämningsregel** bladet i den **namn** skriver **woodgrove-HTTP-LBRule**. Kontrollera att följande har angetts automatiskt:
    - IP Version: **IPv4**
    - Frontend-IP-adress: **LoadBalancerFrontEnd**
    - Protokoll: **TCP**
    - Port: **80**
    - Backend-port: **80**
    - Serverdelspool: **woodgrove BEP**
    - Hälsoavsökning: **woodgrove HP**
    - Persistence för sessioner: **None**
    - Timeout vid inaktivitet: **4 minuter**
    - Flytande IP: **inaktiverad**

1. På den **Lägg till belastningsutjämningsregel** bladet klickar du på **OK**.

1. Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.

## <a name="test-the-load-balancer"></a>Testa lastbalanseraren

1. I Azure-portalen, i den vänstra menyn klickar du på **alla resurser**, och klicka sedan i resurslistan på **woodgrove-LB-IP-**.

1. På den **översikt** bladet och välja den **IP-adress**, och klicka sedan på den **kopia** knappen.

1. I en ny webbläsarflik klistrar du in IP-adressen i adressfältet i webbläsaren. Anteckna namnet på servern och fortsätta den här fliken.

1. I Azure-portalen, i den vänstra menyn klickar du på **alla resurser**. Klicka på den server som du antecknade ovan i resurslistan.

1. På den **översikt** bladet klickar du på **stoppa**, och klicka sedan på **Ja**.

1. Vänta tills den virtuella datorn har stoppats och sedan växla till fliken du granskat i steg 3. Uppdatera sidan.

1. Belastningsutjämnaren skickar nu HTTP-begäran till en av dina andra virtuella datorer.

I den här övningen ska slutfört du distributionen av dina backend-virtuella datorer och belastningsutjämnaren.
