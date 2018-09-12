<span data-ttu-id="84dff-101">Anta att du utvecklar ett ekonomiprogram för nystartade företag.</span><span class="sxs-lookup"><span data-stu-id="84dff-101">Suppose you are developing a financial management application for new business startups.</span></span> <span data-ttu-id="84dff-102">Du vill skydda kundernas data så du har bestämt dig för att implementera ADE på alla operativsystem och datadiskar på de servrar som ska vara värdar för programmet.</span><span class="sxs-lookup"><span data-stu-id="84dff-102">You want to ensure that all your customers' data is secured, so you have decided to implement ADE across all OS and data disks on the servers that will host this application.</span></span> <span data-ttu-id="84dff-103">Enligt dina efterlevnadskrav måste du också ansvara för din egen krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="84dff-103">As part of your compliance requirements, you also need to be responsible for your own encryption key management.</span></span>

<span data-ttu-id="84dff-104">I den här delen får du kryptera diskar på befintliga virtuella Windows-datorer och hantera krypteringsnycklar med ditt eget Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="84dff-104">In this unit you'll encrypt disks on existing Windows VMs, and manage the encryption keys using your own Azure Key Vault.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="84dff-105">Den här övningen förutsätter att Azure PowerShell är installerat på datorn. Gå till [Installera Azure PowerShell på Windows med PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) för information om hur du installerar Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84dff-105">This exercise assumes that Azure PowerShell is installed on your computer; go to [Install Azure PowerShell on Windows with PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) for information on how to install Azure PowerShell.</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="84dff-106">Förbereda miljön</span><span class="sxs-lookup"><span data-stu-id="84dff-106">Prepare the environment</span></span>

<span data-ttu-id="84dff-107">Vi börjar med att distribuera en virtuell Windows-dator på en ny resursgrupp och sedan lägga till en datadisk på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="84dff-107">We'll start by deploying a Windows VM to a new resource group and then add a data disk to the VM.</span></span>

### <a name="deploy-windows-vm-using-azure-portal"></a><span data-ttu-id="84dff-108">Distribuera en virtuell Windows-dator med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84dff-108">Deploy Windows VM using Azure portal</span></span>

<span data-ttu-id="84dff-109">Här installerar du Azure-portalen för att skapa och distribuera en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="84dff-109">Here you'll install use the Azure portal to create and deploy a Windows VM.</span></span> <span data-ttu-id="84dff-110">Starta genom att definiera grundläggande information för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="84dff-110">Start by defining the basic VM information:</span></span>

1. <span data-ttu-id="84dff-111">Öppna en webbläsare och gå till [Azure-portalen](http://portal.azure.com) och logga in med dina vanliga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="84dff-111">In a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your normal credentials.</span></span>

1. <span data-ttu-id="84dff-112">I sidopanelen klickar du på **Virtuella datorer** och sedan på **Skapa virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="84dff-112">In the sidebar, click **Virtual machines**, and then click **Create virtual machine**.</span></span>

1. <span data-ttu-id="84dff-113">På bladet Beräkning i området **Rekommenderas** klickar du på **Windows Server**.</span><span class="sxs-lookup"><span data-stu-id="84dff-113">On the Compute blade, in the **Recommended** section, click **Windows Server**.</span></span>

1. <span data-ttu-id="84dff-114">Klicka på **Windows Server 2016 Datacenter** i bladet **Windows Server**.</span><span class="sxs-lookup"><span data-stu-id="84dff-114">In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="84dff-115">Klicka på **Skapa** i bladet **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="84dff-115">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="84dff-116">I bladet **Basic** i rutan **Namn** skriver du **moneyappsvr01.**</span><span class="sxs-lookup"><span data-stu-id="84dff-116">In the **Basics** blade, in the **Name** box, type **moneyappsvr01.**</span></span>

1. <span data-ttu-id="84dff-117">I rutorna **Användarnamn** och **Lösenord** skriver du ett namn och lösenord för ett administratörskonto på den här servern.</span><span class="sxs-lookup"><span data-stu-id="84dff-117">In the **Username** and **Password boxes**, type a name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="84dff-118">Välj din Azure-prenumeration i fältet **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="84dff-118">In the **Subscription** box, select your Azure subscription.</span></span>

1. <span data-ttu-id="84dff-119">Under **Resursgrupp** väljer du **Skapa ny** och skriver in **moneyapprp**.</span><span class="sxs-lookup"><span data-stu-id="84dff-119">Under **Resource Group**, select **Create new**, and in the box, type **moneyapprg**.</span></span>

1. <span data-ttu-id="84dff-120">Välj en region nära dig i listrutan **Plats**.</span><span class="sxs-lookup"><span data-stu-id="84dff-120">In the **Location** drop-down list, select a region near you.</span></span>

1. <span data-ttu-id="84dff-121">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="84dff-121">Click **OK**.</span></span>

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a><span data-ttu-id="84dff-122">Välj storlek för den virtuella datorn och starta distributionen</span><span class="sxs-lookup"><span data-stu-id="84dff-122">Choose a size for the VM, and start the deployment</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84dff-123">Kom ihåg att virtuella datorer på Basic-nivå inte stöder ADE</span><span class="sxs-lookup"><span data-stu-id="84dff-123">Remember that basic tier VMs do not support ADE</span></span>

1. <span data-ttu-id="84dff-124">På bladet **Välj storlek** väljer du **Standard** SKU, till exempel **B1s** och klickar sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="84dff-124">On the **Choose a size** blade, select a **Standard** SKU, such as **B1s**, and then click **Select**.</span></span>

1. <span data-ttu-id="84dff-125">På bladet **Inställningar** i **Välj offentliga inkommande portar** klickar du på **RDP** och bläddrar sedan nedåt och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="84dff-125">On the **Settings** blade, in the **Select public inbound ports** list, click **RDP**, and then scroll down and click **OK**.</span></span>

1. <span data-ttu-id="84dff-126">På bladet **API-app** klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="84dff-126">On the **Create** blade, click **Create**.</span></span>

1. <span data-ttu-id="84dff-127">Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="84dff-127">Wait until the VM has deployed before continuing with the exercise.</span></span>

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="84dff-128">Lägg till en datadisk i en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="84dff-128">Add a data disk to the VM</span></span>

1. <span data-ttu-id="84dff-129">I den vänstra menyn klickar du på **Alla resurser** och klickar sedan på **moneyappsvr01**.</span><span class="sxs-lookup"><span data-stu-id="84dff-129">In the left menu, click **All resources**, and then click **moneyappsvr01**.</span></span>

1. <span data-ttu-id="84dff-130">På bladet **Virtuella datorer**, under **INSTÄLLNINGAR**, klickar du på **Diskar**.</span><span class="sxs-lookup"><span data-stu-id="84dff-130">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="84dff-131">Observera, på bladet **Diskar**, att operativsystemdiskens krypteringsstatus för närvarande är **Ej aktiverat** och klicka sedan på **Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="84dff-131">On the **Disks** blade, note that the OS disk encryption status is currently **Not enabled**, and then click **Add data disk**.</span></span>

1. <span data-ttu-id="84dff-132">Klicka på listan **Namn** och klicka sedan på **Skapa disk**.</span><span class="sxs-lookup"><span data-stu-id="84dff-132">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="84dff-133">I bladet **Skapa hanterad disk** i **Namn** skriver du **moneyappsvr01_data**.</span><span class="sxs-lookup"><span data-stu-id="84dff-133">In the **Create managed disk** blade, in the **Name** box, type **moneyappsvr01_data**.</span></span>

1. <span data-ttu-id="84dff-134">Under **Resursgrupp** väljer du **Använd befintlig** och välj sedan **moneyapprg**.</span><span class="sxs-lookup"><span data-stu-id="84dff-134">Under **Resource Group**, select **Use existing**, and in the list, select **moneyapprg**.</span></span>

1. <span data-ttu-id="84dff-135">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="84dff-135">Click **Create**.</span></span>

1. <span data-ttu-id="84dff-136">Vänta tills disken har skapats innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="84dff-136">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="84dff-137">På bladet **diskar** klickar du på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="84dff-137">On the **Disks** blade, click **Save**.</span></span> <span data-ttu-id="84dff-138">Observera att datadiskens krypteringsstatus för närvarande är **_Ej aktiverat_**.</span><span class="sxs-lookup"><span data-stu-id="84dff-138">Note that the data disk encryption status is currently **_Not enabled_**.</span></span>

## <a name="configure-disk-encryption-pre-requisites"></a><span data-ttu-id="84dff-139">Konfigurera förhandskrav för diskkryptering</span><span class="sxs-lookup"><span data-stu-id="84dff-139">Configure disk encryption pre-requisites</span></span>

<span data-ttu-id="84dff-140">Du kommer nu använda konfigurationsskriptet för att konfigurera förhandskrav för Azure-diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="84dff-140">You'll now use the Azure disk encryption prerequisites configuration script to configure all the disk encryption pre-requisites.</span></span> <span data-ttu-id="84dff-141">Det här skriptet skapar och förbereder ett nyckelvalv i samma region som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="84dff-141">This script will create and prepare a Key Vault in the same region as your VM.</span></span>

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="84dff-142">Förbered konfigurationsskriptet för Azure-diskkryptering</span><span class="sxs-lookup"><span data-stu-id="84dff-142">Prepare the Azure Disk Encryption Prerequisite Setup Script</span></span>

1. <span data-ttu-id="84dff-143">Gå till den GitHub-sidan som innehåller det [nödvändiga konfigurationsskriptet för Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span><span class="sxs-lookup"><span data-stu-id="84dff-143">Go to the [Azure Disk Encryption Prerequisite Setup Script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1)  GitHub page.</span></span>

1. <span data-ttu-id="84dff-144">Klicka på **Raw** på GibHub-sidan.</span><span class="sxs-lookup"><span data-stu-id="84dff-144">On the GibHub page, click **Raw**.</span></span>

1. <span data-ttu-id="84dff-145">Markera all text på sidan genom att använda CTRL-A. Använd sedan CTRL-C och kopiera all text till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="84dff-145">Use CTRL-A to select all the text on the page and then use CTRL-C to copy all the text on the page to the clipboard.</span></span>

1. <span data-ttu-id="84dff-146">Klicka på **Start** på din dator och bläddra till **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="84dff-146">On your computer, click **Start**, then browse to **Windows PowerShell ISE**.</span></span>

1. <span data-ttu-id="84dff-147">Högerklicka på **Windows PowerShell ISE** och klicka på **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="84dff-147">Right click **Windows PowerShell ISE** and click **Run as administrator**.</span></span>

1. <span data-ttu-id="84dff-148">I fönstret Administratör: Windows PowerShell ISE klickar du på **Visa** och sedan på **Visa skriptfönster**.</span><span class="sxs-lookup"><span data-stu-id="84dff-148">In the Administrator: Windows PowerShell ISE window, click **View** and then click **Show Script Pane**.</span></span>

1. <span data-ttu-id="84dff-149">Klistra in den kopierade texten i skriptfönstret.</span><span class="sxs-lookup"><span data-stu-id="84dff-149">Paste the copied text into the script pane.</span></span>

1. <span data-ttu-id="84dff-150">I skriptfönstret letar du reda på följande kodblock:</span><span class="sxs-lookup"><span data-stu-id="84dff-150">In the script pane, locate the following block of code:</span></span>

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. <span data-ttu-id="84dff-151">I kodblocket, ändra `$false` till `$true`.</span><span class="sxs-lookup"><span data-stu-id="84dff-151">In the code block, change `$false` to `$true`.</span></span>

1. <span data-ttu-id="84dff-152">Klicka på **Arkiv**, klicka sedan på **Spara som** och navigera till mappen som du vill använda för att spara skriptet.</span><span class="sxs-lookup"><span data-stu-id="84dff-152">Click **File**, then click **Save As**, and navigate to the folder you'd like to use to save the script.</span></span>

1. <span data-ttu-id="84dff-153">I textrutan för **filnamn** anger du **ADEPrereqScript.ps1** och klickar på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="84dff-153">In the **File name** box, type **ADEPrereqScript.ps1**, and click **Save**.</span></span>

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="84dff-154">Kör konfigurationsskriptet för Azure-diskkryptering</span><span class="sxs-lookup"><span data-stu-id="84dff-154">Run the Azure Disk Encryption Prerequisite Setup Script</span></span>

1. <span data-ttu-id="84dff-155">I PowerShell-fönstret anger du följande kommando och trycker på **RETUR**:</span><span class="sxs-lookup"><span data-stu-id="84dff-155">In the  PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. <span data-ttu-id="84dff-156">I PowerShell-fönstret anger du följande kommando och trycker på **RETUR**:</span><span class="sxs-lookup"><span data-stu-id="84dff-156">In the PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   <span data-ttu-id="84dff-157">Om dialogrutan **Ändring av körningsprincipen** visas klickar du antingen på **Ja till alla** eller **Ja** (om du inte får alternativet _Ja till alla_).</span><span class="sxs-lookup"><span data-stu-id="84dff-157">If you get an **Execution Policy Change** dialog box, click either **Yes to all** or **Yes** (if you do not get a _Yes to all_ option).</span></span>

1. <span data-ttu-id="84dff-158">I PowerShell-fönstret anger du följande kommando och trycker på **RETUR**:</span><span class="sxs-lookup"><span data-stu-id="84dff-158">In the  PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```powershell
   Login-AzureRmAccount
   ```

1. <span data-ttu-id="84dff-159">Ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="84dff-159">Enter your Azure credentials.</span></span>

1. <span data-ttu-id="84dff-160">Välj din **SubscriptionId**-sträng och kopiera den till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="84dff-160">Select your **SubscriptionId** string, and copy it to the clipboard.</span></span>

1. <span data-ttu-id="84dff-161">Klicka på **Arkiv** i PowerShell ISE och klicka sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="84dff-161">In the  PowerShell ISE, click **File**, and then click **Run**.</span></span>

1. <span data-ttu-id="84dff-162">I konsolfönstret i **resourceGroupName:** anger du **moneyapprg** och trycker på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="84dff-162">In the console pane, at the **resourceGroupName:** prompt, type  **moneyapprg**, and press **ENTER**.</span></span>

1. <span data-ttu-id="84dff-163">I konsolfönstret i **keyVaultName:** anger du **moneyapprg** och trycker på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="84dff-163">In the console pane, at the **keyVaultName:** prompt, type **moneyappkv**, and press **ENTER**.</span></span>

1. <span data-ttu-id="84dff-164">I konsolfönstret i **plats:** anger du den plats som du använde när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="84dff-164">In the console pane, at the **location:** prompt, type the location you used when creating your VM.</span></span>

1. <span data-ttu-id="84dff-165">I konsolfönstret i **subscriptionId:** klistrar du in ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="84dff-165">In the console pane, at the **subscriptionId:** prompt, paste your subscription ID.</span></span>

1. <span data-ttu-id="84dff-166">Nyckelvalvet **moneyappkv** kommer nu att skapas.</span><span class="sxs-lookup"><span data-stu-id="84dff-166">The **moneyappkv** key vault will now be created.</span></span> <span data-ttu-id="84dff-167">När detta har slutförts väljer du översiktstext (i grönt och kopierar den till Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="84dff-167">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="84dff-168">Tryck på **RETUR** för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="84dff-168">Press **ENTER** to continue.</span></span>

1. <span data-ttu-id="84dff-169">I konsolfönstret i **aadAppName:** anger du **moneyapp** och trycker på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="84dff-169">In the console pane, at the **aadAppName:**  prompt, type **moneyapp**, and press **ENTER**.</span></span>

1. <span data-ttu-id="84dff-170">Azure AD-programmet **moneyapp** skapas nu.</span><span class="sxs-lookup"><span data-stu-id="84dff-170">The **moneyapp** Azure AD application will now be created.</span></span> <span data-ttu-id="84dff-171">När detta har slutförts väljer du översiktstext (i grönt och kopierar den till Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="84dff-171">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="84dff-172">Tryck på **RETUR** för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="84dff-172">Press **ENTER** to continue.</span></span>

### <a name="encrypt-your-vm-disks-with-powershell"></a><span data-ttu-id="84dff-173">Kryptera dina virtuella datordiskar med PowerShell</span><span class="sxs-lookup"><span data-stu-id="84dff-173">Encrypt your VM disks with PowerShell</span></span>

<span data-ttu-id="84dff-174">Kontrollera krypteringsstatusen för operativsystemet och datadiskarna:</span><span class="sxs-lookup"><span data-stu-id="84dff-174">Verify the encryption status of the OS and data disks:</span></span>

1. <span data-ttu-id="84dff-175">I PowerShell-fönstret anger du följande kommando och trycker på RETUR:</span><span class="sxs-lookup"><span data-stu-id="84dff-175">In the PowerShell ISE console pane, type the following command, and press ENTER:</span></span>

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > <span data-ttu-id="84dff-176">Namnet på den virtuella datorn måste stå inom enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="84dff-176">The VM name must be enclosed in single quotes.</span></span>

1. <span data-ttu-id="84dff-177">I PowerShell-skriptfönstret anger du följande kommando och trycker på RETUR:</span><span class="sxs-lookup"><span data-stu-id="84dff-177">In the PowerShell ISE script pane, enter the following command, and press ENTER:</span></span>

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. <span data-ttu-id="84dff-178">I dialogrutan **Aktivera AzureDiskEncryption på den virtuella datorn** dialogrutan klickar du på **Ja** och noterar meddelandet om att krypteringen kan ta 10 – 15 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="84dff-178">In the **Enable AzureDiskEncryption on the VM** dialog box, click **Yes**, and note the message that encryption may take 10-15 minutes to complete.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="84dff-179">Vänta tills kommandot har slutförts innan du fortsätter med den här övningen</span><span class="sxs-lookup"><span data-stu-id="84dff-179">Wait until the command has completed before continuing with this exercise</span></span>

### <a name="verify-the-encryption-status-of-your-vm-disks"></a><span data-ttu-id="84dff-180">Kontrollera krypteringsstatusen för de virtuella datordiskarna</span><span class="sxs-lookup"><span data-stu-id="84dff-180">Verify the encryption status of your VM disks</span></span>

<span data-ttu-id="84dff-181">Växla till Azure-portalen. Observera på bladet **Diskar** för **moneyappsvr01** att statusen för diskkryptering för operativsystemet och datadiskarna nu är **Aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="84dff-181">Switch to the Azure portal, and on the **Disks** blade for **moneyappsvr01**, note that the disk encryption status for the OS and Data disks is now **Enabled**.</span></span>
