Du vill se till att du kan ansluta klienter eller platser inom din miljö i Azure med hjälp av krypterade tunnlar över det offentliga internet. I den här övningen skapar du en punkt till plats-VPN-anslutning och sedan ansluter du den gatewayen från en klientdator. Du använder ursprungliga Azure-certifikatautentiseringsanslutningar för säkerhet.

Du kommer att utföra följande processer:

1. Skapa en routningsbaserad VPN-gateway

1. Överför den offentliga nyckeln för ett rotcertifikat i autentiseringssyfte

1. Generera ett klientcertifikat från rotcertifikatet och installera sedan klientcertifikatet på varje klientdator som ska ansluta till det virtuella nätverket i autentiseringssyfte

1. Skapa VPN-klientkonfigurationsfiler, som innehåller all nödvändig information som krävs för att klienten ska kunna ansluta till det virtuella nätverket.

## <a name="before-you-begin"></a>Innan du börjar

För att slutföra den här labbuppgiften måste du ha följande:

- Azure PowerShell

- En mapp med namnet C:\cert

Så här installerar du Azure PowerShell:

1. Högerklicka på Windows-knappen och klicka på **PowerShell (administratör)**

1. I meddelanderutan **User Account Control** klickar du på **Ja**

1. I PowerShell-fönstret anger du följande kommando och trycker på Retur:

    ```PowerShell
    Import-Module AzureRM
    ```

1. I säkerhetsrutan skriver du A och trycker på Retur.

## <a name="sign-in-and-set-variables"></a>Logga in och ange variabler

Logga in och ange variabler genom att utföra följande steg:

1. Anslut till Azure genom att ange följande kommando och trycka på Retur:

    ```PowerShell
    Connect-AzureRmAccount
    ```

1. Hämta en lista över dina Azure-prenumerationer.

    ```PowerShell
    Get-AzureRmSubscription
    ```
1. Ange den prenumeration som du vill använda.

    ```PowerShell
    Select-AzureRmSubscription -SubscriptionName "{Name of your subscription}"
    ```

    Ersätt ”namn på prenumeration” med ditt prenumerationsnamn.

1. Ange följande variabler och tryck på Retur efter varje.

    ```PowerShell
    $VNetName  = "VNetData"
    $FESubName = "FrontEnd"
    $BESubName = "Backend"
    $GWSubName = "GatewaySubnet"
    $VNetPrefix1 = "192.168.0.0/16"
    $VNetPrefix2 = "10.254.0.0/16"
    $FESubPrefix = "192.168.1.0/24"
    $BESubPrefix = "10.254.1.0/24"
    $GWSubPrefix = "192.168.200.0/26"
    $VPNClientAddressPool = "172.16.201.0/24"
    $RG = "TestRG"
    $Location = "East US"
    $GWName = "VNetDataGW"
    $GWIPName = "VNetDataGWPIP"
    $GWIPconfName = "gwipconf"
    ```

## <a name="configure-a-vnet"></a>Konfigurera ett virtuellt nätverk

1. Skapa en resursgrupp.

    ```PowerShell
    New-AzureRmResourceGroup -Name $RG -Location $Location
    ```

1. Skapa undernätskonfigurationer för det virtuella nätverket. Dessa måste ha namnen **FrontEnd, BackEnd** och **GatewaySubnet**. Alla dessa undernät finns i prefixet för det virtuella nätverket.

    ```PowerShell
    $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. Skapa det virtuella nätverket med undernätsvärdena och en statisk DNS-server. 
    > [!IMPORTANT]
    > Ignorera varningsmeddelandet och vänta sedan tills kommandot har slutförts.

    ```PowerShell
    New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. Ange nu variablerna för det här nätverket som du just har skapat.

    ```PowerShell
    $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. Begär en dynamiskt tilldelad offentlig IP-adress.

    ```PowerShell
    $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## <a name="create-the-vpn-gateway"></a>Skapa VPN-gatewayen

När du skapar denna VPN-gateway:

- GatewayType måste vara Vpn
- VpnType måste vara RouteBased

> [!NOTE]
> Observera att den här delen av övningen kan ta upp till 45 minuter att slutföra beroende på värdet för gateway-sku

1. Skapa VPN-gatewayen genom att köra följande kommando och trycka på Retur.

    ```PowerShell
    New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
    -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
    -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. Vänta tills kommandoutdatan visas.

## <a name="add-the-vpn-client-address-pool"></a>Lägg till VPN-klientadresspoolen

1. Ange följande kommando och tryck på Retur.

    ```PowerShell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. Vänta tills kommandoutdatan visas.

## <a name="generate-a-client-certificate"></a>Generera ett klientcertifikat

1. Skapa det självsignerade rotcertifikatet.

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. Generera ett klientcertifikat.

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

1. Exportera den offentliga nyckeln för rotcertifikatet. På klientdatorn anger du följande kommando och trycker på Retur.

    ```PowerShell
    certmgr
    ```

1. Navigera till Personliga/Certifikat. Högerklicka på P2SRootCert, klicka på **Alla uppgifter**och välj sedan **Exportera**.

1. I guiden Exportera certifikat klickar du på **Nästa**.

1. Kontrollera att **Nej, exportera inte den privata nyckeln** har markerats och klicka sedan på **Nästa**.

1. På sidan **Filformat för export** väljer du **Base 64-kodad X.509 (. CER).** och klickar sedan på **Nästa**.

1. På sidan **File to export** (Fil att exportera) under **Filnamn** anger du **C:\cert\P2SRootCert.cer** och klickar sedan på Nästa.

1. På sidan **Slutför guiden Exportera certifikat** klickar du på **Slutför**.

1. I meddelanderutan **Exportera certifikat** klickar du på **OK**.

## <a name="upload-the-root-certificate-public-key-information"></a>Ladda upp informationen om den offentliga nyckeln för rotcertifikatet

1. I följande PowerShell-fönster kör du följande kommando för att deklarera en variabel för certifikatets namn:

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. Kör följande kommando:

    ```PowerShell
    $filePathForCert = "C:\cert\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. Ladda nu upp certifikatet till Azure. Azure identifierar det som ett betrott rotcertifikat.

    ```PowerShell
    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNetDataGW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
    ```

## <a name="configure-the-native-vpn-client"></a>Konfigurera den inbyggda VPN-klienten

1. Kör följande kommando för att skapa VPN-klientkonfigurationsfiler i .ZIP-format.

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. Kopiera URL:en som returnerades i utdatan från detta kommando och klistra in den i din webbläsare. Webbläsaren bör nu börja ladda ned en .ZIP-fil. Packa upp den och placera den på en lämplig plats.

1. I den extraherade mappen navigerar du till antingen WindowsAmd64-mappen (för 64-bitars Windows-datorer) eller till WindowsZX86 (för 32-bitars datorer).

1. Dubbelklicka på filen VpnClientSetupxxxxx.exe, beroende på din arkitektur.

1. På skärmen **Windows skyddade datorn** klickar du på **Mer info** sedan på **Kör ändå**.

1. I dialogrutan **User Account Control** klickar du på **Ja**.

1. I dialogrutan **VNetData** klickar du på **Ja**.

## <a name="connect-to-azure"></a>Anslut till Azure

1. Tryck på Windows-tangenten, skriv **Inställningar** och tryck på Retur.

1. I fönstret **Inställningar** klickar du på **Network and Internet** (Nätverk och internet).

1. Klicka på **VPN** i det vänstra fönstret.

1. I det högra fönstret klickar du på **VNetData** sedan på **Anslut**.

1. I fönstret VNetData klickar du på **Anslut**.

1. I nästa VNetData-fönster klickar du på **Fortsätt**.

1. I meddelanderutan **User Account Control** klickar du på **Ja**.

> [!NOTE]
> Om de här stegen inte fungerar kan du behöva starta om datorn

## <a name="verify-your-connection"></a>Verifiera din anslutning

1. Tryck på Windows-tangenten, skriv **cmd** och tryck på Retur. Fönstret för **kommandotolken** öppnas.

1. Skriv `IPCONFIG /ALL` och tryck på Retur.

1. Kopiera IP-adressen under PPP-anslutningen för VNetData eller skriv ned den.

1. Bekräfta att IP-adressen finns i **VPNClientAddressPool-intervallet 172.16.201.0/24**.

1. Du har upprättat en anslutning till Azure VPN-gatewayen.

## <a name="summary"></a>Sammanfattning

Du har precis konfigurerat en VPN-gateway så att du kan skapa en krypterad klientanslutning till ett virtuellt nätverk i Azure. Den här metoden är mycket bra med klientdatorer och mindre plats till plats-anslutningar.