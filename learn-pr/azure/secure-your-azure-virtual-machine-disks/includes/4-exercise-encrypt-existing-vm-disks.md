<span data-ttu-id="07312-101">Anta att du utvecklar ett ekonomiprogram för nystartade företag.</span><span class="sxs-lookup"><span data-stu-id="07312-101">Suppose you are developing a financial management application for new business startups.</span></span> <span data-ttu-id="07312-102">Du vill skydda kundernas data och har därför bestämt dig för att implementera Azure Disk Encryption (ADE) på alla operativsystem och datadiskar på de servrar som ska vara värdar för programmet.</span><span class="sxs-lookup"><span data-stu-id="07312-102">You want to ensure that all your customers' data is secured, so you have decided to implement Azure Disk Encryption (ADE) across all OS and data disks on the servers that will host this application.</span></span> <span data-ttu-id="07312-103">Enligt dina efterlevnadskrav måste du också ansvara för din egen krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="07312-103">As part of your compliance requirements, you also need to be responsible for your own encryption key management.</span></span>

<span data-ttu-id="07312-104">I den här modulen får du kryptera diskar på befintliga virtuella Windows-datorer och hantera krypteringsnycklar med ditt eget Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="07312-104">In this unit, you'll encrypt disks on existing Windows VMs, and manage the encryption keys using your own Azure Key Vault.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="07312-105">Den här övningen förutsätter att Azure PowerShell finns installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="07312-105">This exercise assumes that Azure PowerShell is installed on your computer.</span></span> <span data-ttu-id="07312-106">Gå till [Installera Azure PowerShell på Windows med PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) för information om hur du installerar Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="07312-106">Go to [Install Azure PowerShell on Windows with PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) for information on how to install Azure PowerShell.</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="07312-107">Förbereda miljön</span><span class="sxs-lookup"><span data-stu-id="07312-107">Prepare the environment</span></span>

<span data-ttu-id="07312-108">Vi börjar med att distribuera en virtuell Windows-dator på en ny resursgrupp och sedan lägga till en datadisk till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="07312-108">We'll start by deploying a Windows VM to a new resource group, and then add a data disk to the VM.</span></span>

### <a name="deploy-windows-vm-using-the-azure-portal"></a><span data-ttu-id="07312-109">Distribuera en virtuell Windows-dator med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="07312-109">Deploy Windows VM using the Azure portal</span></span>

<span data-ttu-id="07312-110">Här använder du Microsoft Azure-portalen för att skapa och distribuera en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="07312-110">Here you'll use the Azure portal to create and deploy a Windows VM.</span></span> <span data-ttu-id="07312-111">Börja med att definiera grundläggande information för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="07312-111">Start by defining the basic VM information:</span></span>

1. <span data-ttu-id="07312-112">Öppna en webbläsare och gå till [Azure-portalen](http://portal.azure.com) och logga in med dina vanliga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="07312-112">In a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your normal credentials.</span></span>

1. <span data-ttu-id="07312-113">I sidopanelen klickar du på **Virtuella datorer** och sedan på **Skapa virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="07312-113">In the sidebar, click **Virtual machines**, and then click **Create virtual machine**.</span></span>

1. <span data-ttu-id="07312-114">På bladet Beräkning i området **Rekommenderas** klickar du på **Windows Server**.</span><span class="sxs-lookup"><span data-stu-id="07312-114">On the Compute blade, in the **Recommended** section, click **Windows Server**.</span></span>

1. <span data-ttu-id="07312-115">Klicka på **Windows Server 2016 Datacenter** i bladet **Windows Server**.</span><span class="sxs-lookup"><span data-stu-id="07312-115">In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="07312-116">Klicka på **Skapa** i bladet **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="07312-116">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="07312-117">I bladet **Basic** i rutan **Namn** skriver du **moneyappsvr01.**</span><span class="sxs-lookup"><span data-stu-id="07312-117">In the **Basics** blade, in the **Name** box, type **moneyappsvr01.**</span></span>

1. <span data-ttu-id="07312-118">I rutorna **Användarnamn** och **Lösenord** skriver du ett namn och lösenord för ett administratörskonto på den här servern.</span><span class="sxs-lookup"><span data-stu-id="07312-118">In the **Username** and **Password boxes**, type a name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="07312-119">Välj din Azure-prenumeration i fältet **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="07312-119">In the **Subscription** box, select your Azure subscription.</span></span>

1. <span data-ttu-id="07312-120">Välj **Skapa ny** under **Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="07312-120">Under **Resource Group**, select **Create new**.</span></span> <span data-ttu-id="07312-121">Skriv **moneyapprg** i rutan.</span><span class="sxs-lookup"><span data-stu-id="07312-121">In the box, type **moneyapprg**.</span></span>

1. <span data-ttu-id="07312-122">Välj en region nära dig i listrutan **Plats**.</span><span class="sxs-lookup"><span data-stu-id="07312-122">In the **Location** drop-down list, select a region near you.</span></span>

1. <span data-ttu-id="07312-123">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="07312-123">Click **OK**.</span></span>

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a><span data-ttu-id="07312-124">Välj en storlek för den virtuella datorn och starta distributionen.</span><span class="sxs-lookup"><span data-stu-id="07312-124">Choose a size for the VM, and start the deployment</span></span>

> [!IMPORTANT]
> <span data-ttu-id="07312-125">Kom ihåg att virtuella datorer på Basic-nivå inte stöder ADE.</span><span class="sxs-lookup"><span data-stu-id="07312-125">Remember that basic tier VMs do not support ADE.</span></span>

1. <span data-ttu-id="07312-126">På bladet **Välj storlek** väljer du **Standard** SKU, till exempel **B1s**.</span><span class="sxs-lookup"><span data-stu-id="07312-126">On the **Choose a size** blade, select a **Standard** SKU, such as **B1s**.</span></span> <span data-ttu-id="07312-127">Klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="07312-127">Then click **Select**.</span></span>

1. <span data-ttu-id="07312-128">På bladet **Inställningar** i **Välj offentliga inkommande portar** klickar du på **RDP**.</span><span class="sxs-lookup"><span data-stu-id="07312-128">On the **Settings** blade, in the **Select public inbound ports** list, click **RDP**.</span></span> <span data-ttu-id="07312-129">Bläddra sedan nedåt och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="07312-129">Then scroll down and click **OK**.</span></span>

1. <span data-ttu-id="07312-130">På bladet **Skapa** klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="07312-130">On the **Create** blade, click **Create**.</span></span>

1. <span data-ttu-id="07312-131">Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="07312-131">Wait until the VM has deployed before continuing with the exercise.</span></span>

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="07312-132">Lägg till en datadisk i en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="07312-132">Add a data disk to the VM</span></span>

1. <span data-ttu-id="07312-133">I den vänstra menyn klickar du på **Alla resurser** och klickar sedan på **moneyappsvr01**.</span><span class="sxs-lookup"><span data-stu-id="07312-133">In the left menu, click **All resources**, and then click **moneyappsvr01**.</span></span>

1. <span data-ttu-id="07312-134">På bladet **Virtuella datorer**, under **INSTÄLLNINGAR**, klickar du på **Diskar**.</span><span class="sxs-lookup"><span data-stu-id="07312-134">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="07312-135">Observera, på bladet **Diskar**, att operativsystemdiskens krypteringsstatus för närvarande är **Ej aktiverat** och klicka sedan på **Lägg till datadisk**.</span><span class="sxs-lookup"><span data-stu-id="07312-135">On the **Disks** blade, note that the OS disk encryption status is currently **Not enabled**, and then click **Add data disk**.</span></span>

1. <span data-ttu-id="07312-136">Klicka på listan **Namn** och klicka sedan på **Skapa disk**.</span><span class="sxs-lookup"><span data-stu-id="07312-136">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="07312-137">I bladet **Skapa hanterad disk** i **Namn** skriver du **moneyappsvr01_data**.</span><span class="sxs-lookup"><span data-stu-id="07312-137">In the **Create managed disk** blade, in the **Name** box, type **moneyappsvr01_data**.</span></span>

1. <span data-ttu-id="07312-138">Under **Resursgrupp** väljer du **Använd befintlig** och välj sedan **moneyapprg**.</span><span class="sxs-lookup"><span data-stu-id="07312-138">Under **Resource Group**, select **Use existing**, and in the list, select **moneyapprg**.</span></span>

1. <span data-ttu-id="07312-139">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="07312-139">Click **Create**.</span></span>

1. <span data-ttu-id="07312-140">Vänta tills disken har skapats innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="07312-140">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="07312-141">På bladet **Diskar** klickar du på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="07312-141">On the **Disks** blade, click **Save**.</span></span> <span data-ttu-id="07312-142">Observera att datadiskens krypteringsstatus för närvarande är **Ej aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="07312-142">Note that the data disk encryption status is currently **Not enabled**.</span></span>

## <a name="configure-disk-encryption-prerequisites"></a><span data-ttu-id="07312-143">Konfigurera förhandskrav för diskkryptering</span><span class="sxs-lookup"><span data-stu-id="07312-143">Configure disk encryption prerequisites</span></span>

<span data-ttu-id="07312-144">Du kommer nu använda förhandskonfigurationsskriptet för Azure Disk Encryption för att konfigurerar alla förhandskrav för diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="07312-144">You'll now use the Azure Disk Encryption prerequisites configuration script to configure all the disk encryption prerequisites.</span></span> <span data-ttu-id="07312-145">Det här skriptet skapar och förbereder ett nyckelvalv i samma region som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="07312-145">This script will create and prepare a key vault in the same region as your VM.</span></span>

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="07312-146">Förbereda förhandskonfigurationsskriptet för Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="07312-146">Prepare the Azure Disk Encryption prerequisite setup script</span></span>

1. <span data-ttu-id="07312-147">Gå till GitHub-sidan som innehåller [förhandskonfigurationsskriptet för Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span><span class="sxs-lookup"><span data-stu-id="07312-147">Go to the [Azure Disk Encryption prerequisite setup script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1) GitHub page.</span></span>

1. <span data-ttu-id="07312-148">Klicka på **Raw** på GitHub-sidan.</span><span class="sxs-lookup"><span data-stu-id="07312-148">On the GibHub page, click **Raw**.</span></span>

1. <span data-ttu-id="07312-149">Markera all text på sidan med CTRL-A. Kopiera sedan all text till Urklipp med CTRL-C.</span><span class="sxs-lookup"><span data-stu-id="07312-149">Use Ctrl-A to select all the text on the page, and then use Ctrl-C to copy all the text on the page to the clipboard.</span></span>

1. <span data-ttu-id="07312-150">Klicka på **Start** på din dator och bläddra till **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="07312-150">On your computer, click **Start**, and then browse to **Windows PowerShell ISE**.</span></span>

1. <span data-ttu-id="07312-151">Högerklicka på **Windows PowerShell ISE** och klicka på **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="07312-151">Right-click **Windows PowerShell ISE**, and click **Run as administrator**.</span></span>

1. <span data-ttu-id="07312-152">I fönstret Administratör: Windows PowerShell ISE klickar du på **Visa** och sedan på **Visa skriptfönster**.</span><span class="sxs-lookup"><span data-stu-id="07312-152">In the Administrator: Windows PowerShell ISE window, click **View**, and then click **Show Script Pane**.</span></span>

1. <span data-ttu-id="07312-153">Klistra in den kopierade texten i skriptfönstret.</span><span class="sxs-lookup"><span data-stu-id="07312-153">Paste the copied text into the script pane.</span></span>

1. <span data-ttu-id="07312-154">I skriptfönstret letar du reda på följande kodblock:</span><span class="sxs-lookup"><span data-stu-id="07312-154">In the script pane, locate the following block of code:</span></span>

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. <span data-ttu-id="07312-155">I kodblocket, ändra `$false` till `$true`.</span><span class="sxs-lookup"><span data-stu-id="07312-155">In the code block, change `$false` to `$true`.</span></span>

1. <span data-ttu-id="07312-156">Klicka på **Arkiv**, klicka sedan på **Spara som** och navigera till mappen som du vill använda för att spara skriptet.</span><span class="sxs-lookup"><span data-stu-id="07312-156">Click **File**, then click **Save As**, and navigate to the folder you'd like to use to save the script.</span></span>

1. <span data-ttu-id="07312-157">I fältet **Filnamn** anger du **ADEPrereqScript.ps1** och klickar sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="07312-157">In the **File name** box, type **ADEPrereqScript.ps1**, and click **Save**.</span></span>

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="07312-158">Köra förhandskonfigurationsskriptet för Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="07312-158">Run the Azure Disk Encryption prerequisite setup script</span></span>

1. <span data-ttu-id="07312-159">I PowerShell ISE-fönstret anger du följande kommando och trycker sedan på **RETUR**:</span><span class="sxs-lookup"><span data-stu-id="07312-159">In the  PowerShell ISE console pane, type the following command, and press **Enter**:</span></span>

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. <span data-ttu-id="07312-160">I PowerShell ISE-fönstret anger du följande kommando och trycker sedan på **RETUR**:</span><span class="sxs-lookup"><span data-stu-id="07312-160">In the PowerShell ISE console pane, type the following command, and press **Enter**:</span></span>

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   <span data-ttu-id="07312-161">Om dialogrutan **Ändring av körningsprincipen** visas klickar du antingen på **Ja till alla** eller **Ja** (om alternativet _Ja till alla_ inte visas).</span><span class="sxs-lookup"><span data-stu-id="07312-161">If you get an **Execution Policy Change** dialog box, click either **Yes to all** or **Yes** (if you do not get a _Yes to all_ option).</span></span>

1. <span data-ttu-id="07312-162">I PowerShell ISE-fönstret anger du följande kommando och trycker sedan på **RETUR**:</span><span class="sxs-lookup"><span data-stu-id="07312-162">In the  PowerShell ISE console pane, type the following command, and press **Enter**:</span></span>

   ```powershell
   Login-AzureRmAccount
   ```

1. <span data-ttu-id="07312-163">Ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="07312-163">Enter your Azure credentials.</span></span>

1. <span data-ttu-id="07312-164">Välj din **SubscriptionId**-sträng och kopiera den till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="07312-164">Select your **SubscriptionId** string, and copy it to the clipboard.</span></span>

1. <span data-ttu-id="07312-165">Välj **Arkiv** i PowerShell ISE och välj sedan **Kör**.</span><span class="sxs-lookup"><span data-stu-id="07312-165">In the PowerShell ISE, click **File**, and then click **Run**.</span></span>

1. <span data-ttu-id="07312-166">I konsolfönstret vid **resourceGroupName:** anger du **moneyapprg**.</span><span class="sxs-lookup"><span data-stu-id="07312-166">In the console pane, at the **resourceGroupName:** prompt, type  **moneyapprg**.</span></span> <span data-ttu-id="07312-167">Tryck sedan på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="07312-167">Then press **Enter**.</span></span>

1. <span data-ttu-id="07312-168">I konsolfönstret vid **keyVaultName:** anger du **moneyapprg**.</span><span class="sxs-lookup"><span data-stu-id="07312-168">In the console pane, at the **keyVaultName:** prompt, type **moneyappkv**.</span></span> <span data-ttu-id="07312-169">Tryck sedan på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="07312-169">Then press **Enter**.</span></span>

1. <span data-ttu-id="07312-170">I konsolfönstret vid **plats:** anger du den plats som du använde när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="07312-170">In the console pane, at the **location:** prompt, type the location you used when creating your VM.</span></span>

1. <span data-ttu-id="07312-171">I konsolfönstret i **subscriptionId:** klistrar du in ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="07312-171">In the console pane, at the **subscriptionId:** prompt, paste your subscription ID.</span></span>

1. <span data-ttu-id="07312-172">Nyckelvalvet **moneyappkv** kommer nu att skapas.</span><span class="sxs-lookup"><span data-stu-id="07312-172">The **moneyappkv** key vault will now be created.</span></span> <span data-ttu-id="07312-173">När du har gjort allt det markerar du översiktstexten (visas i grönt) och kopierar den till Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="07312-173">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="07312-174">Tryck på **RETUR** för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="07312-174">Press **Enter** to continue.</span></span>

1. <span data-ttu-id="07312-175">I konsolfönstret vid **aadAppName:** anger du **moneyapp**.</span><span class="sxs-lookup"><span data-stu-id="07312-175">In the console pane, at the **aadAppName:**  prompt, type **moneyapp**.</span></span> <span data-ttu-id="07312-176">Tryck sedan på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="07312-176">Then press **Enter**.</span></span>

1. <span data-ttu-id="07312-177">Azure AD-programmet **moneyapp** skapas nu.</span><span class="sxs-lookup"><span data-stu-id="07312-177">The **moneyapp** Azure AD application will now be created.</span></span> <span data-ttu-id="07312-178">När du har gjort allt det markerar du översiktstexten (visas i grönt) och kopierar den till Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="07312-178">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="07312-179">Tryck på **RETUR** för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="07312-179">Press **Enter** to continue.</span></span>

### <a name="encrypt-your-vm-disks-with-powershell"></a><span data-ttu-id="07312-180">Kryptera dina virtuella datordiskar med PowerShell</span><span class="sxs-lookup"><span data-stu-id="07312-180">Encrypt your VM disks with PowerShell</span></span>

<span data-ttu-id="07312-181">Kontrollera krypteringsstatusen för operativsystemet och datadiskarna:</span><span class="sxs-lookup"><span data-stu-id="07312-181">Verify the encryption status of the OS and data disks:</span></span>

1. <span data-ttu-id="07312-182">I PowerShell ISE-fönstret anger du följande kommando och trycker sedan på **RETUR**:</span><span class="sxs-lookup"><span data-stu-id="07312-182">In the PowerShell ISE console pane, type the following command, and press **Enter**:</span></span>

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > <span data-ttu-id="07312-183">Namnet på den virtuella datorn måste stå inom enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="07312-183">The VM name must be enclosed in single quotes.</span></span>

1. <span data-ttu-id="07312-184">I PowerShell-skriptfönstret anger du följande kommando och trycker sedan på **RETUR**:</span><span class="sxs-lookup"><span data-stu-id="07312-184">In the PowerShell ISE script pane, enter the following command, and press **Enter**:</span></span>

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. <span data-ttu-id="07312-185">I dialogrutan **Aktivera AzureDiskEncryption på den virtuella datorn** klickar du på **Ja** och noterar meddelandet om att krypteringen kan ta 10–15 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="07312-185">In the **Enable AzureDiskEncryption on the VM** dialog box, click **Yes**, and note the message that encryption may take 10-15 minutes to complete.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="07312-186">Vänta tills kommandot har slutförts innan du fortsätter med övningen.</span><span class="sxs-lookup"><span data-stu-id="07312-186">Wait until the command has completed before continuing with this exercise.</span></span>

### <a name="verify-the-encryption-status-of-your-vm-disks"></a><span data-ttu-id="07312-187">Kontrollera krypteringsstatusen för de virtuella datordiskarna</span><span class="sxs-lookup"><span data-stu-id="07312-187">Verify the encryption status of your VM disks</span></span>

<span data-ttu-id="07312-188">Växla till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="07312-188">Switch to the Azure portal.</span></span> <span data-ttu-id="07312-189">Observera att statusen för diskkryptering för operativsystemet och datadiskarna nu är **Aktiverad** på bladet **Diskar** för **moneyappsvr01**.</span><span class="sxs-lookup"><span data-stu-id="07312-189">On the **Disks** blade for **moneyappsvr01**, note that the disk encryption status for the OS and Data disks is now **Enabled**.</span></span>
