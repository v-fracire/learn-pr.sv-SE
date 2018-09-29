<span data-ttu-id="058c0-101">Anta att du utvecklar ett ekonomiprogram för nystartade företag.</span><span class="sxs-lookup"><span data-stu-id="058c0-101">Suppose you are developing a financial management application for new business startups.</span></span> <span data-ttu-id="058c0-102">Du vill skydda kundernas data och har därför bestämt dig för att implementera Azure Disk Encryption (ADE) på alla operativsystem och datadiskar på de servrar som ska vara värdar för programmet.</span><span class="sxs-lookup"><span data-stu-id="058c0-102">You want to ensure that all your customers' data is secured, so you have decided to implement Azure Disk Encryption (ADE) across all OS and data disks on the servers that will host this application.</span></span> <span data-ttu-id="058c0-103">Enligt dina efterlevnadskrav måste du också ansvara för din egen krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="058c0-103">As part of your compliance requirements, you also need to be responsible for your own encryption key management.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="058c0-104">I den här modulen får du kryptera diskar på en befintlig virtuell dator och hantera krypteringsnycklar med ditt eget Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="058c0-104">In this unit, you'll encrypt disks on an existing virtual machine, and manage the encryption keys using your own Azure Key Vault.</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="058c0-105">Förbereda miljön</span><span class="sxs-lookup"><span data-stu-id="058c0-105">Prepare the environment</span></span>

<span data-ttu-id="058c0-106">Vi börjar med att distribuera en ny virtuell Windows-dator i en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="058c0-106">We'll start by deploying a new Windows VM in an Azure Virtual Machine.</span></span>

### <a name="deploy-windows-vm"></a><span data-ttu-id="058c0-107">Distribuera en virtuell Windows-dator</span><span class="sxs-lookup"><span data-stu-id="058c0-107">Deploy Windows VM</span></span>

<span data-ttu-id="058c0-108">Använd Azure PowerShell till att skapa och distribuera en ny virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="058c0-108">Use the Azure PowerShell to create and deploy a new Windows virtual machine.</span></span>

1. <span data-ttu-id="058c0-109">Börja med att bestämma var du vill placera de nya resurserna.</span><span class="sxs-lookup"><span data-stu-id="058c0-109">Start by deciding where to place the new resources.</span></span> <span data-ttu-id="058c0-110">Välj en plats nära dig i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="058c0-110">Select a location near you from the following list.</span></span>

    <span data-ttu-id="058c0-111"><!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]</span><span class="sxs-lookup"><span data-stu-id="058c0-111"><!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]</span></span>
    

1. <span data-ttu-id="058c0-112">Definiera en PowerShell-variabel som innehåller den valda platsen.</span><span class="sxs-lookup"><span data-stu-id="058c0-112">Define a PowerShell variable to hold the selected location.</span></span> <span data-ttu-id="058c0-113">Här definieras den som ”USA, östra”, ändra det till den plats du vill använda.</span><span class="sxs-lookup"><span data-stu-id="058c0-113">It's defined as "East US" here, change it to your preferred location.</span></span>

    ```powershell
    $location = "eastus"
    ```
    
    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="058c0-114">Definiera sedan några mer praktiska variabler för att fånga _namnet_ på den virtuella datorn och _resursgruppen_.</span><span class="sxs-lookup"><span data-stu-id="058c0-114">Next, define a few more convenient variables to capture the _name_ of the VM and the _resource group_.</span></span> <span data-ttu-id="058c0-115">Observera att vi använder den i förväg skapade resursgruppen här. Normalt skulle du skapa en _ny_ resursgrupp i din prenumeration med hjälp av `New-AzureRmResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="058c0-115">Note that we are using the pre-created resource group here, normally you would create a _new_ resource group in your subscription using `New-AzureRmResourceGroup`.</span></span>

    ```powershell
    $vmName = "fmdata-vm01"
    $rgName = "<rgn>[sandbox Resource Group]</rgn>"
    ```
    
1. <span data-ttu-id="058c0-116">Använd `New-AzureRmVm` för att skapa en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="058c0-116">Use `New-AzureRmVm` to create a new virtual machine.</span></span>
    
    ```powershell
    New-AzureRmVm `
        -ResourceGroupName $rgName `
        -Name $vmName `
        -Location $location `
        -OpenPorts 3389
    ```
    
    <span data-ttu-id="058c0-117">Ange ett användarnamn och lösenord för den virtuella datorn när du uppmanas till det av Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="058c0-117">Enter a username and password for the VM when you are prompted by the Cloud Shell.</span></span> <span data-ttu-id="058c0-118">Detta används som det första skapade kontot för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="058c0-118">This will be used as the initial account created for the VM.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="058c0-119">Det här kommandot använder vissa standardinställningar eftersom vi inte har angett så många alternativ.</span><span class="sxs-lookup"><span data-stu-id="058c0-119">This command will use some defaults since we didn't supply a bunch of options.</span></span> <span data-ttu-id="058c0-120">Mer specifikt skapas en _Windows 2016 Server_-avbildning med storleken _Standard_DS1_v2_.</span><span class="sxs-lookup"><span data-stu-id="058c0-120">Specifically, this will create a _Windows 2016 Server_ image with the size to _Standard_DS1_v2_.</span></span> <span data-ttu-id="058c0-121">Kom ihåg att virtuella datorer på Basic-nivån inte stöder ADE om du vill ange storleken på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="058c0-121">Remember that the Basic tier VMs do not support ADE if you decide to specify the VM size.</span></span>

1. <span data-ttu-id="058c0-122">När den virtuella datorn är klar med distributionen registrerar du informationen om den virtuella datorn i en variabel.</span><span class="sxs-lookup"><span data-stu-id="058c0-122">Once the VM finishes deploying, capture the VM details in a variable.</span></span> <span data-ttu-id="058c0-123">Du kan använda den här variabeln till att utforska det som har skapats.</span><span class="sxs-lookup"><span data-stu-id="058c0-123">You can use this variable to explore what was created.</span></span>

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```
    
1. <span data-ttu-id="058c0-124">Du kan se de OS-diskar som är anslutna till den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="058c0-124">You can see the OS disk attached to the VM:</span></span>

    ```powershell
    $vm.StorageProfile.OSDisk
    ```

    ```output
    OsType                  : Windows
    EncryptionSettings      :
    Name                    : fmdata-vm01_OsDisk_1_6bcf8dcd49794aa785bad45221ec4433
    Vhd                     :
    Image                   :
    Caching                 : ReadWrite
    WriteAcceleratorEnabled :
    CreateOption            : FromImage
    DiskSizeGB              : 127
    ManagedDisk             : Microsoft.Azure.Management.Compute.Models.ManagedDiskP
                              arameters
    ```
        
1. <span data-ttu-id="058c0-125">Kontrollera den aktuella statusen för kryptering på OS-disken (och eventuella datadiskar).</span><span class="sxs-lookup"><span data-stu-id="058c0-125">Check the current status of encryption on the OS disk (and any data disks).</span></span>

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  `
        -ResourceGroupName $rgName `
        -VMName $vmName
    ```

    <span data-ttu-id="058c0-126">Som du ser är diskarna just nu _okrypterade_.</span><span class="sxs-lookup"><span data-stu-id="058c0-126">As you can see the disks are current _unencrypted_.</span></span> 

    ```output
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : NotEncrypted
    OsVolumeEncryptionSettings :
    ProgressMessage            : No Encryption extension or metadata found on the VM
    ```
    
<span data-ttu-id="058c0-127">Vi ändrar på det.</span><span class="sxs-lookup"><span data-stu-id="058c0-127">Let's change that.</span></span>
    
## <a name="encrypt-the-vm-disks-with-azure-disk-encryption"></a><span data-ttu-id="058c0-128">Kryptera VM-diskarna med Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="058c0-128">Encrypt the VM disks with Azure Disk Encryption</span></span>

<span data-ttu-id="058c0-129">Vi behöver skydda dessa data, så vi krypterar diskarna.</span><span class="sxs-lookup"><span data-stu-id="058c0-129">We need to protect this data, so let's encrypt the disks.</span></span> <span data-ttu-id="058c0-130">Kom ihåg att det finns flera åtgärder som vi behöver utföra:</span><span class="sxs-lookup"><span data-stu-id="058c0-130">Recall that there are several steps we need to perform:</span></span>

1. <span data-ttu-id="058c0-131">Skapa ett nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="058c0-131">Create a key vault.</span></span>
1. <span data-ttu-id="058c0-132">Ange nyckelvalvet så att det stöder diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="058c0-132">Set the key vault up to support disk encryption.</span></span>
1. <span data-ttu-id="058c0-133">Berätta för Azure att kryptera VM-diskarna med hjälp av nyckeln som lagras i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="058c0-133">Tell Azure to encrypt the VM disks using the key stored in the Key Vault.</span></span>

> [!TIP]
> <span data-ttu-id="058c0-134">Vi går igenom stegen separat, men när du gör detta i din egen prenumeration kan du använda ett praktiskt PowerShell-skript som är länkat i sammanfattningen av den här modulen.</span><span class="sxs-lookup"><span data-stu-id="058c0-134">We're going to walk through the steps individually, but when you're doing this task in your own subscription, you can use a handy PowerShell script which is linked in the Summary of this module.</span></span>

### <a name="create-a-key-vault"></a><span data-ttu-id="058c0-135">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="058c0-135">Create a key vault</span></span>

<span data-ttu-id="058c0-136">För att skapa ett nyckelvalv i Azure Key Vault måste vi aktivera tjänsten i vår prenumeration.</span><span class="sxs-lookup"><span data-stu-id="058c0-136">To create an Azure Key Vault, we need to enable the service in our subscription.</span></span> <span data-ttu-id="058c0-137">Det här är ett engångskrav.</span><span class="sxs-lookup"><span data-stu-id="058c0-137">This is a one-time requirement.</span></span>

> [!TIP]
> <span data-ttu-id="058c0-138">Beroende på din prenumeration kan du behöva aktivera **Microsoft.KeyVault**-providern med cmdleten `Register-AzureRmResourceProvider`.</span><span class="sxs-lookup"><span data-stu-id="058c0-138">Depending on your subscription, you might need to enable the **Microsoft.KeyVault** provider with the `Register-AzureRmResourceProvider` cmdlet.</span></span> <span data-ttu-id="058c0-139">Detta är inte nödvändigt i Azure-sandboxprenumerationen.</span><span class="sxs-lookup"><span data-stu-id="058c0-139">This is not necessary in the Azure sandbox subscription.</span></span>

1. <span data-ttu-id="058c0-140">Bestäm ett namn på det nya nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="058c0-140">Decide on a name for your new key vault.</span></span> <span data-ttu-id="058c0-141">Det måste vara unikt och kan bestå av mellan 3 och 24 tecken, innehålla siffror, bokstäver, och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="058c0-141">It must be unique and can be between 3 and 24 characters, composed of numbers, letters, and and dashes.</span></span> <span data-ttu-id="058c0-142">Prova att lägga till några slumptal i slutet, där du ersätter den ”1234” nedan.</span><span class="sxs-lookup"><span data-stu-id="058c0-142">Try adding some random numbers to the end, replacing the "1234" below.</span></span>

    ```powershell
    $keyVaultName = "mvmdsk-kv-1234"
    ```
        
1. <span data-ttu-id="058c0-143">Skapa ett nyckelvalv i Azure Key Vault med `New-AzureRmKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="058c0-143">Create an Azure Key Vault with `New-AzureRmKeyVault`.</span></span>
    - <span data-ttu-id="058c0-144">Se till att det placeras i samma resursgrupp _och_ på samma plats som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="058c0-144">Make sure it's placed in the same resource group _and_ location as your VM.</span></span>
    - <span data-ttu-id="058c0-145">Aktivera nyckelvalvet för användning med diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="058c0-145">Enable the Key Vault for use with disk encryption.</span></span> 
    - <span data-ttu-id="058c0-146">Ange ett unikt namn på nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="058c0-146">Specify a unique Key Vault name.</span></span>

    ```powershell
    New-AzureRmKeyVault -VaultName $keyVaultName `
        -Location $location `
        -ResourceGroupName $rgName `
        -EnabledForDiskEncryption
    ```

    <span data-ttu-id="058c0-147">Du får en varning från det här kommandot om att inga användare har åtkomst.</span><span class="sxs-lookup"><span data-stu-id="058c0-147">You will get a warning from this command about no users having access.</span></span>

    ```output
    WARNING: Access policy is not set. No user or application have access permission to use this vault. This can happen if the vault was created by a service principal. Please use Set-AzureRmKeyVaultAccessPolicy to set access policies.
    ```

    <span data-ttu-id="058c0-148">Detta är okej eftersom vi bara använder valvet för att lagra krypteringsnycklar för den virtuella datorn och användarna inte behöver åtkomst till dessa data.</span><span class="sxs-lookup"><span data-stu-id="058c0-148">This is ok since we are just using the vault to store the encryption keys for the VM and users won't need to access this data.</span></span>

### <a name="encrypt-the-disk"></a><span data-ttu-id="058c0-149">Kryptera disken</span><span class="sxs-lookup"><span data-stu-id="058c0-149">Encrypt the disk</span></span>

<span data-ttu-id="058c0-150">Vi är nästan redo att kryptera diskarna.</span><span class="sxs-lookup"><span data-stu-id="058c0-150">We are almost ready to encrypt the disks.</span></span> <span data-ttu-id="058c0-151">Innan vi gör det vill vi utfärda en varning om att skapa säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="058c0-151">Before we do, a warning about creating backups.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="058c0-152">Om detta vore ett produktionssystem skulle vi behöva utföra en säkerhetskopiering av de hanterade diskarna – antingen med Azure Backup eller genom att skapa en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="058c0-152">If this were a production system, we would need to perform a backup of the managed disks - either using Azure Backup, or by creating a snapshot.</span></span> <span data-ttu-id="058c0-153">Du kan skapa ögonblicksbilder i Azure-portalen eller via kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="058c0-153">You can create snapshots in the Azure portal, or through the command line.</span></span> <span data-ttu-id="058c0-154">I PowerShell använder du cmdleten `New-AzureRmSnapshot`.</span><span class="sxs-lookup"><span data-stu-id="058c0-154">In PowerShell, use the `New-AzureRmSnapshot` cmdlet.</span></span> <span data-ttu-id="058c0-155">Eftersom det här är en enkel övning och vi kommer att slänga dessa data när du är klar hoppar vi över det här steget.</span><span class="sxs-lookup"><span data-stu-id="058c0-155">Since this is a simple exercise and we're going to throw this data away when you're done, we're going to skip this step.</span></span> 

1. <span data-ttu-id="058c0-156">Börja med att definiera en variabel som innehåller informationen om nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="058c0-156">Start by defining a variable to hold the Key Vault information.</span></span>

    ```powershell
    $keyVault = Get-AzureRmKeyVault `
        -VaultName $keyVaultName `
        -ResourceGroupName $rgName
    ```

1. <span data-ttu-id="058c0-157">Använd sedan cmdleten `Set-AzureRmVmDiskEncryptionExtension` till att kryptera VM-diskarna.</span><span class="sxs-lookup"><span data-stu-id="058c0-157">Then, use the `Set-AzureRmVmDiskEncryptionExtension` cmdlet to encrypt the VM disks.</span></span>
    - <span data-ttu-id="058c0-158">Med parametern `VolumeType` kan du ange vilka diskar som ska krypteras: [_Alla_ | _OS_ | _Data_].</span><span class="sxs-lookup"><span data-stu-id="058c0-158">The `VolumeType` parameter allows you to specify which disks to encrypt: [_All_ | _OS_ | _Data_].</span></span> <span data-ttu-id="058c0-159">Värdet _Alla_ ställs in som standard.</span><span class="sxs-lookup"><span data-stu-id="058c0-159">It will default to _All_.</span></span> <span data-ttu-id="058c0-160">Du kan endast kryptera datadiskar för vissa distributioner av Linux.</span><span class="sxs-lookup"><span data-stu-id="058c0-160">You can only encrypt data disks for some distributions of Linux.</span></span>
    - <span data-ttu-id="058c0-161">Du måste ange flaggan `SkipVmBackup` för hanterade diskar, annars misslyckas kommandot eftersom det inte finns någon ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="058c0-161">You have to supply the `SkipVmBackup` flag for managed disks or the command will fail because there is no snapshot.</span></span>

    ```powershell
    Set-AzureRmVmDiskEncryptionExtension `
        -ResourceGroupName $rgName `
        -VMName $vmName `
        -VolumeType All `
        -DiskEncryptionKeyVaultId $keyVault.ResourceId `
        -DiskEncryptionKeyVaultUrl $keyVault.VaultUri `
        -SkipVmBackup
    ```

1. <span data-ttu-id="058c0-162">Denna cmdlet varnar dig om att den virtuella datorn måste kopplas från och att uppgiften kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="058c0-162">The cmdlet will warn you that the VM must be taken offline, and that the task may take several minutes to complete.</span></span> <span data-ttu-id="058c0-163">Gå vidare och låt den fortsätta.</span><span class="sxs-lookup"><span data-stu-id="058c0-163">Go ahead and let it continue.</span></span>

    ```output
    Enable AzureDiskEncryption on the VM
    This cmdlet prepares the VM and enables encryption which may reboot the machine and takes 10-15 minutes to
    finish. Please save your work on the VM before confirming. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    ```
    
1. <span data-ttu-id="058c0-164">När den är klar kan du kontrollera krypteringsstatusen igen.</span><span class="sxs-lookup"><span data-stu-id="058c0-164">Once it's complete, check the encryption status again.</span></span>

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
    ```

    <span data-ttu-id="058c0-165">Nu bör diskarna vara krypterade.</span><span class="sxs-lookup"><span data-stu-id="058c0-165">Now the OS disk should be encrypted.</span></span> <span data-ttu-id="058c0-166">Eventuella anslutna datadiskar som är synliga för Windows krypteras också.</span><span class="sxs-lookup"><span data-stu-id="058c0-166">Any attached data disks that are visible to Windows will also be encrypted.</span></span>

    ```output
    OsVolumeEncrypted          : Encrypted
    DataVolumesEncrypted       : NoDiskFound
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : Provisioning succeeded
    ```

> [!NOTE]        
> <span data-ttu-id="058c0-167">Nya diskar som har lagts till efter krypteringen kommer _inte_ att krypteras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="058c0-167">New disks added after encryption will _not_ be automatically encrypted.</span></span> <span data-ttu-id="058c0-168">Du kan köra cmdleten `Set-AzureRmVMDiskEncryptionExtension` igen för att kryptera nya diskar.</span><span class="sxs-lookup"><span data-stu-id="058c0-168">You can re-run the `Set-AzureRmVMDiskEncryptionExtension` cmdlet to encrypt new disks.</span></span> <span data-ttu-id="058c0-169">Se till att ange ett nytt sekvensnummer om du lägger till diskar till en virtuell dator som har diskar som redan krypterats.</span><span class="sxs-lookup"><span data-stu-id="058c0-169">Make sure to provide a new sequence number if you add disks to a VM that has already had disks encrypted.</span></span> <span data-ttu-id="058c0-170">Dessutom krypteras inte diskar som inte är synliga för operativsystemet – disken måste vara korrekt partitionerad, formaterad och monterad för att kunna upptäckas av Bitlocker-tillägget.</span><span class="sxs-lookup"><span data-stu-id="058c0-170">In addition, disks that are not visible to the operating system will not be encrypted - the disk must be properly partitioned, formatted, and mounted to be seen by the Bitlocker extension.</span></span>