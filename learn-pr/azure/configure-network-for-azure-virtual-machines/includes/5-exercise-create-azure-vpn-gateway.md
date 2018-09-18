<span data-ttu-id="c3ecf-101">Du vill vara säker på att du kan ansluta klienter eller platser inom din miljö i Azure med hjälp av krypterade tunnlar över det offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-101">You want to ensure that you can connect clients or sites within your environment into Azure using encrypted tunnels across the public internet.</span></span> <span data-ttu-id="c3ecf-102">I den här övningen skapar du en punkt till plats-VPN-gateway och ansluter sedan till den gatewayen från en klientdator.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-102">In this unit, you'll create a point-to-site VPN gateway, and then connect to that gateway from a client computer.</span></span> <span data-ttu-id="c3ecf-103">Du använder ursprungliga Azure-certifikatautentiseringsanslutningar för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-103">You'll use native Azure certificate authentication connections for security.</span></span>

<span data-ttu-id="c3ecf-104">Du kommer att utföra följande processer:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-104">You will carry out the following process:</span></span>

1. <span data-ttu-id="c3ecf-105">Skapa en routningsbaserad VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-105">Create a RouteBased VPN gateway.</span></span>

1. <span data-ttu-id="c3ecf-106">Överföra den offentliga nyckeln för ett rotcertifikat i autentiseringssyfte.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-106">Upload the public key for a root certificate for authentication purposes.</span></span>

1. <span data-ttu-id="c3ecf-107">Generera ett klientcertifikat från rotcertifikatet och sedan installera klientcertifikatet på varje klientdator som ska ansluta till det virtuella nätverket i autentiseringssyfte.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-107">Generate a client certificate from the root certificate, and then install the client certificate on each client computer that will connect to the virtual network for authentication purposes.</span></span>

1. <span data-ttu-id="c3ecf-108">Skapa VPN-klientkonfigurationsfiler som innehåller all nödvändig information för att klienten ska kunna ansluta till det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-108">Create VPN client configuration files, which contain the necessary information for the client to connect to the virtual network.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c3ecf-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c3ecf-109">Before you begin</span></span>
<!---TODO: These should be prerequisites in the first unit and on the index.yml--->

<span data-ttu-id="c3ecf-110">För att kunna slutföra den här modulen måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-110">To complete this module, you must have:</span></span>

- <span data-ttu-id="c3ecf-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c3ecf-111">Azure PowerShell</span></span>

- <span data-ttu-id="c3ecf-112">En mapp med namnet C:\cert</span><span class="sxs-lookup"><span data-stu-id="c3ecf-112">A folder called C:\cert</span></span>

<span data-ttu-id="c3ecf-113">Så här installerar du Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-113">To install Azure PowerShell:</span></span>

1. <span data-ttu-id="c3ecf-114">Högerklicka på Windows-knappen och klicka på **PowerShell (administratör)**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-114">Right-click the Windows button and click **PowerShell (Admin)**.</span></span>

1. <span data-ttu-id="c3ecf-115">I meddelanderutan **User Account Control** klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-115">In the **User Account Control** message box, click **Yes**.</span></span>

1. <span data-ttu-id="c3ecf-116">I PowerShell-fönstret anger du följande kommando och trycker på Retur:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-116">In the PowerShell window, type the following command and press Enter:</span></span>

    ```PowerShell
    Import-Module AzureRM
    ```

1. <span data-ttu-id="c3ecf-117">I säkerhetsrutan skriver du A och trycker på Retur.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-117">At the security prompt, type A and press Enter.</span></span>

## <a name="sign-in-and-set-variables"></a><span data-ttu-id="c3ecf-118">Logga in och ange variabler</span><span class="sxs-lookup"><span data-stu-id="c3ecf-118">Sign in and set variables</span></span>

<span data-ttu-id="c3ecf-119">Logga in och ange variabler genom att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-119">To sign in and set variables, carry out the following steps:</span></span>

1. <span data-ttu-id="c3ecf-120">Anslut till Azure genom att ange följande kommando och trycka på Retur:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-120">Connect to Azure by entering the following command and pressing Enter:</span></span>

    ```PowerShell
    Connect-AzureRmAccount
    ```

1. <span data-ttu-id="c3ecf-121">Hämta en lista över dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-121">Get a list of your Azure subscriptions.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
1. <span data-ttu-id="c3ecf-122">Ange den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-122">Specify the subscription that you want to use.</span></span>

    ```PowerShell
    Select-AzureRmSubscription -SubscriptionName "{Name of your subscription}"
    ```

    <span data-ttu-id="c3ecf-123">Ersätt ”namn på prenumeration” med ditt prenumerationsnamn.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-123">Replace "Name of subscription" with your subscription name.</span></span>

1. <span data-ttu-id="c3ecf-124">Ange följande variabler och tryck på Retur efter varje.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-124">Enter the following variables and press Enter after each one.</span></span>

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

## <a name="configure-a-virtual-network"></a><span data-ttu-id="c3ecf-125">Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="c3ecf-125">Configure a virtual network</span></span>

1. <span data-ttu-id="c3ecf-126">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-126">Create a resource group.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name $RG -Location $Location
    ```

1. <span data-ttu-id="c3ecf-127">Skapa undernätskonfigurationer för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-127">Create subnet configurations for the virtual network.</span></span> <span data-ttu-id="c3ecf-128">Dessa måste ha namnen **FrontEnd, BackEnd** och **GatewaySubnet**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-128">These have the name **FrontEnd, BackEnd**, and **GatewaySubnet**.</span></span> <span data-ttu-id="c3ecf-129">Alla dessa undernät finns i prefixet för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-129">All of these subnets exist within the virtual network prefix.</span></span>

    ```PowerShell
    $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. <span data-ttu-id="c3ecf-130">Skapa det virtuella nätverket med undernätsvärdena och en statisk DNS-server.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-130">Create the virtual network using the subnet values and a static DNS server.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c3ecf-131">Ignorera varningsmeddelandet och vänta sedan tills kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-131">Ignore the warning message, and then wait for the command to finish.</span></span>

    ```PowerShell
    New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. <span data-ttu-id="c3ecf-132">Ange nu variablerna för det här nätverket som du just har skapat.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-132">Now specify the variables for this network that you have just created.</span></span>

    ```PowerShell
    $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. <span data-ttu-id="c3ecf-133">Begär en dynamiskt tilldelad offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-133">Request a dynamically assigned public IP address.</span></span>

    ```PowerShell
    $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## <a name="create-the-vpn-gateway"></a><span data-ttu-id="c3ecf-134">Skapa VPN-gatewayen</span><span class="sxs-lookup"><span data-stu-id="c3ecf-134">Create the VPN gateway</span></span>

<span data-ttu-id="c3ecf-135">När du skapar denna VPN-gateway:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-135">When creating this VPN gateway:</span></span>

- <span data-ttu-id="c3ecf-136">GatewayType måste vara Vpn</span><span class="sxs-lookup"><span data-stu-id="c3ecf-136">GatewayType must be Vpn</span></span>
- <span data-ttu-id="c3ecf-137">VpnType måste vara RouteBased</span><span class="sxs-lookup"><span data-stu-id="c3ecf-137">VpnType must be RouteBased</span></span>

> [!NOTE]
> <span data-ttu-id="c3ecf-138">Observera att den här delen av övningen kan ta upp till 45 minuter att slutföra beroende på värdet för GatewaySku.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-138">Note that this part of the exercise can take up to 45 minutes to complete, depending on the value of GatewaySku.</span></span>

1. <span data-ttu-id="c3ecf-139">Skapa VPN-gatewayen genom att köra följande kommando och trycka på Retur.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-139">To create the VPN gateway, run the following command and press Enter.</span></span>

    ```PowerShell
    New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
    -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
    -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. <span data-ttu-id="c3ecf-140">Vänta tills kommandoutdatan visas.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-140">Wait for the command output to appear.</span></span>

## <a name="add-the-vpn-client-address-pool"></a><span data-ttu-id="c3ecf-141">Lägg till VPN-klientadresspoolen</span><span class="sxs-lookup"><span data-stu-id="c3ecf-141">Add the VPN client address pool</span></span>

1. <span data-ttu-id="c3ecf-142">Ange följande kommando och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-142">Run the following command and press Enter.</span></span>

    ```PowerShell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. <span data-ttu-id="c3ecf-143">Vänta tills kommandoutdatan visas.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-143">Wait for the command output to appear.</span></span>

## <a name="generate-a-client-certificate"></a><span data-ttu-id="c3ecf-144">Generera ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="c3ecf-144">Generate a client certificate</span></span>

1. <span data-ttu-id="c3ecf-145">Skapa det självsignerade rotcertifikatet.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-145">Create the self-signed root certificate.</span></span>

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. <span data-ttu-id="c3ecf-146">Generera ett klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-146">Generate a client certificate.</span></span>

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

1. <span data-ttu-id="c3ecf-147">Exportera den offentliga nyckeln för rotcertifikatet.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-147">Export the root certificate public key.</span></span> <span data-ttu-id="c3ecf-148">På klientdatorn anger du följande kommando och trycker på Retur.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-148">On your client computer, type the following command and press Enter.</span></span>

    ```PowerShell
    certmgr
    ```

1. <span data-ttu-id="c3ecf-149">Navigera till Personliga/Certifikat.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-149">Navigate to Personal/Certificates.</span></span> <span data-ttu-id="c3ecf-150">Högerklicka på P2SRootCert, klicka på **Alla uppgifter**och välj sedan **Exportera**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-150">Right-click P2SRootCert, click **All tasks**, and then select **Export**.</span></span>

1. <span data-ttu-id="c3ecf-151">I guiden Exportera certifikat klickar du på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-151">In the Certificate Export Wizard, click **Next**.</span></span>

1. <span data-ttu-id="c3ecf-152">Kontrollera att **Nej, exportera inte den privata nyckeln** har markerats och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-152">Ensure that **No, do not export the private key** is selected, and then click **Next**.</span></span>

1. <span data-ttu-id="c3ecf-153">På sidan **Filformat för export** väljer du **Base 64-kodad X.509 (. CER).** och klickar sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-153">On the **Export File Format** page, ensure that **Base-64 encoded X.509 (.CER)** is selected, and then click **Next**.</span></span>

1. <span data-ttu-id="c3ecf-154">På sidan **File to export** (Fil att exportera) under **Filnamn** anger du **C:\cert\P2SRootCert.cer** och klickar sedan på Nästa.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-154">In the **File to Export** page, under **File name**, enter **C:\cert\P2SRootCert.cer**, and then click Next.</span></span>

1. <span data-ttu-id="c3ecf-155">På sidan **Slutför guiden Exportera certifikat** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-155">On the **Completing the Certificate Export Wizard** page, click **Finish**.</span></span>

1. <span data-ttu-id="c3ecf-156">I meddelanderutan **Exportera certifikat** klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-156">On the **Certificate Export Wizard** message box, click **OK**.</span></span>

## <a name="upload-the-root-certificate-public-key-information"></a><span data-ttu-id="c3ecf-157">Ladda upp informationen om den offentliga nyckeln för rotcertifikatet</span><span class="sxs-lookup"><span data-stu-id="c3ecf-157">Upload the root certificate public key information</span></span>

1. <span data-ttu-id="c3ecf-158">I följande PowerShell-fönster kör du följande kommando för att deklarera en variabel för certifikatets namn:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-158">In the PowerShell window, execute the following command to declare a variable for the certificate name:</span></span>

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. <span data-ttu-id="c3ecf-159">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c3ecf-159">Execute the following command:</span></span>

    ```PowerShell
    $filePathForCert = "C:\cert\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. <span data-ttu-id="c3ecf-160">Ladda nu upp certifikatet till Azure.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-160">Now upload the certificate to Azure.</span></span> <span data-ttu-id="c3ecf-161">Azure identifierar det som ett betrott rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-161">Azure then recognizes it as a trusted root certificate.</span></span>

    ```PowerShell
    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNetDataGW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
    ```

## <a name="configure-the-native-vpn-client"></a><span data-ttu-id="c3ecf-162">Konfigurera den inbyggda VPN-klienten</span><span class="sxs-lookup"><span data-stu-id="c3ecf-162">Configure the native VPN client</span></span>

1. <span data-ttu-id="c3ecf-163">Kör följande kommando för att skapa VPN-klientkonfigurationsfiler i .ZIP-format.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-163">Execute the following command to create VPN client configuration files in .ZIP format.</span></span>

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. <span data-ttu-id="c3ecf-164">Kopiera URL:en som returnerades i utdatan från detta kommando och klistra in den i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-164">Copy the URL returned in the output from this command and paste it into your browser.</span></span> <span data-ttu-id="c3ecf-165">Webbläsaren bör nu börja ladda ned en .ZIP-fil.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-165">Your browser should start downloading a .ZIP file.</span></span> <span data-ttu-id="c3ecf-166">Packa upp den och placera den på en lämplig plats.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-166">Unzip it and put it in a suitable location.</span></span>

1. <span data-ttu-id="c3ecf-167">I den extraherade mappen navigerar du till antingen WindowsAmd64-mappen (för 64-bitars Windows-datorer) eller till mappen WindowsZX86 (för 32-bitars datorer).</span><span class="sxs-lookup"><span data-stu-id="c3ecf-167">In the extracted folder, navigate to either the WindowsAmd64 folder (for 64-bit Windows computers) or the WindowsZX86 folder (for 32-bit computers).</span></span>

1. <span data-ttu-id="c3ecf-168">Dubbelklicka på filen VpnClientSetupxxxxx.exe, beroende på din arkitektur.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-168">Double-click on the VpnClientSetupxxxxx.exe file, depending on your architecture.</span></span>

1. <span data-ttu-id="c3ecf-169">På skärmen **Windows skyddade datorn** klickar du på **Mer info** sedan på **Kör ändå**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-169">In the **Windows protected your PC** screen, click **More info**, and then click **Run anyway**.</span></span>

1. <span data-ttu-id="c3ecf-170">I meddelanderutan **User Account Control** klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-170">In the **User Account Control** dialog box, click **Yes**.</span></span>

1. <span data-ttu-id="c3ecf-171">I dialogrutan **VNetData** klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-171">In the **VNetData** dialog box, click **Yes**.</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="c3ecf-172">Anslut till Azure</span><span class="sxs-lookup"><span data-stu-id="c3ecf-172">Connect to Azure</span></span>

1. <span data-ttu-id="c3ecf-173">Tryck på Windows-tangenten, skriv **Inställningar** och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-173">Press the Windows key, type **Settings** and press Enter.</span></span>

1. <span data-ttu-id="c3ecf-174">I fönstret **Inställningar** klickar du på **Network and Internet** (Nätverk och internet).</span><span class="sxs-lookup"><span data-stu-id="c3ecf-174">In the **Settings** window, click **Network and Internet**.</span></span>

1. <span data-ttu-id="c3ecf-175">Klicka på **VPN** i det vänstra fönstret.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-175">In the left-hand pane, click **VPN**.</span></span>

1. <span data-ttu-id="c3ecf-176">I det högra fönstret klickar du på **VNetData** och klickar sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-176">In the right-hand pane, click **VNetData**, and then click **Connect**.</span></span>

1. <span data-ttu-id="c3ecf-177">I fönstret VNetData klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-177">In the VNetData window, click **Connect**.</span></span>

1. <span data-ttu-id="c3ecf-178">I nästa VNetData-fönster klickar du på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-178">In the next VNetData window, click **Continue**.</span></span>

1. <span data-ttu-id="c3ecf-179">I meddelanderutan **User Account Control** klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-179">In the **User Account Control** message box, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="c3ecf-180">Om de här stegen inte fungerar kan du behöva starta om datorn.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-180">If these steps do not work, you may need to restart your computer.</span></span>

## <a name="verify-your-connection"></a><span data-ttu-id="c3ecf-181">Verifiera din anslutning</span><span class="sxs-lookup"><span data-stu-id="c3ecf-181">Verify your connection</span></span>

1. <span data-ttu-id="c3ecf-182">Tryck på Windows-tangenten, skriv **cmd** och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-182">Press the Windows key, type **cmd** and press Enter.</span></span> <span data-ttu-id="c3ecf-183">Fönstret för **kommandotolken** öppnas.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-183">The **Command Prompt** window appears.</span></span>

1. <span data-ttu-id="c3ecf-184">Skriv `IPCONFIG /ALL` och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-184">Type `IPCONFIG /ALL` and press Enter.</span></span>

1. <span data-ttu-id="c3ecf-185">Kopiera IP-adressen under PPP-anslutningen för VNetData eller skriv ned den.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-185">Copy the IP address under PPP adapter VNetData, or write it down.</span></span>

1. <span data-ttu-id="c3ecf-186">Bekräfta att IP-adressen finns i **VPNClientAddressPool-intervallet 172.16.201.0/24**.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-186">Confirm that IP address is in the **VPNClientAddressPool range of 172.16.201.0/24**.</span></span>

1. <span data-ttu-id="c3ecf-187">Du har har upprättat en anslutning till Azure VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-187">You have successfully made a connection to the Azure VPN gateway.</span></span>

## <a name="summary"></a><span data-ttu-id="c3ecf-188">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c3ecf-188">Summary</span></span>

<span data-ttu-id="c3ecf-189">Du har precis konfigurerat en VPN-gateway så att du kan skapa en krypterad klientanslutning till ett virtuellt nätverk i Azure.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-189">You just set up a VPN gateway, allowing you to make an encrypted client connection to a virtual network in Azure.</span></span> <span data-ttu-id="c3ecf-190">Den här metoden är mycket bra med klientdatorer och mindre plats-till-plats-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="c3ecf-190">This approach is great with client computers and smaller site-to-site connections.</span></span>