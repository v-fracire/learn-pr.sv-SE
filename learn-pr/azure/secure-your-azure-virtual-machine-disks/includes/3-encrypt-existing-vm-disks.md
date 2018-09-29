<span data-ttu-id="40ef4-101">Anta att ditt företag har beslutat att implementera Azure Disk Encryption (ADE) för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="40ef4-101">Suppose your company has decided to implement Azure Disk Encryption (ADE) across all VMs.</span></span> <span data-ttu-id="40ef4-102">Du behöver utvärdera hur du distribuerar kryptering till befintliga virtuella datorvolymer.</span><span class="sxs-lookup"><span data-stu-id="40ef4-102">You need to evaluate how to roll out encryption to existing VM volumes.</span></span> <span data-ttu-id="40ef4-103">Här ska vi titta på kraven för ADE och de steg som behövs för att kryptera diskar på befintliga virtuella Linux- och Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="40ef4-103">Here, we'll look at the requirements for ADE, and the steps involved in encrypting disks on existing Linux and Windows VMs.</span></span>

## <a name="azure-disk-encryption-prerequisites"></a><span data-ttu-id="40ef4-104">Förhandskrav för Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="40ef4-104">Azure Disk Encryption prerequisites</span></span>

<span data-ttu-id="40ef4-105">Innan du kan kryptera dina virtuella datordiskar behöver du göra följande:</span><span class="sxs-lookup"><span data-stu-id="40ef4-105">Before you can encrypt your VM disks, you need to:</span></span>

1. <span data-ttu-id="40ef4-106">Skapa ett nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="40ef4-106">Create a key vault.</span></span>
1. <span data-ttu-id="40ef4-107">Konfigurera åtkomstprincipen för nyckelvalv så att den stöder diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="40ef4-107">Set the key vault access policy to support disk encryption.</span></span>
1. <span data-ttu-id="40ef4-108">Använda nyckelvalvet för att lagra krypteringsnycklarna för ADE.</span><span class="sxs-lookup"><span data-stu-id="40ef4-108">Use the key vault to store the encryption keys for ADE.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="40ef4-109">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="40ef4-109">Azure Key Vault</span></span>

<span data-ttu-id="40ef4-110">De krypteringsnycklar som används av ADE kan lagras i ett Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="40ef4-110">The encryption keys used by ADE can be stored in Azure Key Vault.</span></span> <span data-ttu-id="40ef4-111">Azure Key Vault är ett verktyg för att lagra och komma åt hemligheter på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="40ef4-111">Azure Key Vault is a tool for securely storing and accessing secrets.</span></span> <span data-ttu-id="40ef4-112">En hemlighet är något som du vill begränsa åtkomst till, till exempel API-nycklar, lösenord eller certifikat.</span><span class="sxs-lookup"><span data-stu-id="40ef4-112">A secret is anything that you want to tightly control access to, such as API keys, passwords, or certificates.</span></span> <span data-ttu-id="40ef4-113">Det ger högst tillgänglig, skalbar och säker lagring såsom definieras i  Federal Information Processing Standards (FIPS) 140-2 nivå 2-verifierade Maskinvarusäkerhetsmoduler (HSM).</span><span class="sxs-lookup"><span data-stu-id="40ef4-113">This provides highly available and scalable secure storage, as defined in Federal Information Processing Standards (FIPS) 140-2 Level 2 validated Hardware Security Modules (HSMs).</span></span> <span data-ttu-id="40ef4-114">Med Key Vault kan du behålla fullständig kontroll över de nycklar som används för att kryptera dina data, och även hantera och granska din nyckelanvändning.</span><span class="sxs-lookup"><span data-stu-id="40ef4-114">Using Key Vault, you keep full control of the keys used to encrypt your data, and you can manage and audit your key usage.</span></span> 

> [!NOTE]
> <span data-ttu-id="40ef4-115">Azure Disk Encryption kräver att ditt nyckelvalv och dina virtuella datorer finns i samma Azure-region. Det säkerställer att krypteringshemligheter inte korsar regionala gränser.</span><span class="sxs-lookup"><span data-stu-id="40ef4-115">Azure Disk Encryption requires that your key vault and your VMs are in the same Azure region; this ensures that encryption secrets do not cross regional boundaries.</span></span>

<span data-ttu-id="40ef4-116">Du kan konfigurera och hantera ditt nyckelvalv med:</span><span class="sxs-lookup"><span data-stu-id="40ef4-116">You can configure and manage your key vault with:</span></span>

#### <a name="powershell"></a><span data-ttu-id="40ef4-117">PowerShell</span><span class="sxs-lookup"><span data-stu-id="40ef4-117">Powershell</span></span>

```powershell
New-AzureRmKeyVault -Location <location> `
    -ResourceGroupName <resource-group> `
    -VaultName "myKeyVault" `
    -EnabledForDiskEncryption
```

### <a name="azure-cli"></a><span data-ttu-id="40ef4-118">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="40ef4-118">Azure CLI</span></span>

```azurecli
az keyvault create \
    --name "myKeyVault" \
    --resource-group <resource-group> \
    --location <location> \
    --enabled-for-disk-encryption True
```

### <a name="azure-portal"></a><span data-ttu-id="40ef4-119">Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="40ef4-119">Azure portal</span></span>

<span data-ttu-id="40ef4-120">Ett Azure-nyckelvalv är en resurs som kan skapas i Azure-portalen med den normala resursskapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="40ef4-120">An Azure Key Vault is a resource that can be created in the Azure portal using the normal resource creation process.</span></span>

1. <span data-ttu-id="40ef4-121">Klick på **Skapa en resurs** i sidofältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="40ef4-121">Click **Create a resource** in the sidebar on the left.</span></span>

1. <span data-ttu-id="40ef4-122">Sök efter ”Key vault” (nyckelvalv).</span><span class="sxs-lookup"><span data-stu-id="40ef4-122">Search for "Key vault".</span></span> <span data-ttu-id="40ef4-123">Klicka på **Skapa** i informationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="40ef4-123">Click **Create** in the details window.</span></span>

    ![Skärmbild visar nyckelvalv i Azure Marketplace](../media/3-create-keyvault.png)

1. <span data-ttu-id="40ef4-125">Ange uppgifterna för det nya nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="40ef4-125">Enter the details for the new Key Vault:</span></span>
    - <span data-ttu-id="40ef4-126">Ange ett **Namn** för nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="40ef4-126">Enter a **Name** for the Key Vault</span></span>
    - <span data-ttu-id="40ef4-127">Välj den prenumeration som det ska placeras i (standardvärdet är din aktuella prenumeration).</span><span class="sxs-lookup"><span data-stu-id="40ef4-127">Select the subscription to place it in (defaults to your current subscription).</span></span>
    - <span data-ttu-id="40ef4-128">Välj en **resursgrupp** eller skapa en ny resursgrupp som ska äga nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="40ef4-128">Select a **Resource Group**, or create a new resource group to own the Key Vault.</span></span>
    - <span data-ttu-id="40ef4-129">Välj en **plats** för nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="40ef4-129">Select a **Location** for the Key Vault.</span></span> <span data-ttu-id="40ef4-130">Se till att välja den plats som den virtuella datorn finns på.</span><span class="sxs-lookup"><span data-stu-id="40ef4-130">Make sure to select the location the VM is in.</span></span>
    - <span data-ttu-id="40ef4-131">Du kan välja antingen Standard eller Premium för prisnivån.</span><span class="sxs-lookup"><span data-stu-id="40ef4-131">You can choose either Standard or Premium for the pricing tier.</span></span> <span data-ttu-id="40ef4-132">Den största skillnaden är att Premium-nivån tillåter nycklar som backas av maskinvarukryptering.</span><span class="sxs-lookup"><span data-stu-id="40ef4-132">The main difference is that the premium tier allows for Hardware-encryption backed keys.</span></span>

1. <span data-ttu-id="40ef4-133">Du måste ändra åtkomstprinciperna så att de stöder diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="40ef4-133">You must change the Access policies to support Disk Encryption.</span></span> <span data-ttu-id="40ef4-134">Som standard läggs _ditt_ kontot till i principen.</span><span class="sxs-lookup"><span data-stu-id="40ef4-134">By default it adds _your_ account to the policy.</span></span>
    - <span data-ttu-id="40ef4-135">Välja **Åtkomstprinciper**</span><span class="sxs-lookup"><span data-stu-id="40ef4-135">Select **Access policies**</span></span>
    - <span data-ttu-id="40ef4-136">Klicka på **Avancerade åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="40ef4-136">Click **Advanced access policies**.</span></span>
    - <span data-ttu-id="40ef4-137">Markera **Enable access to Azure Disk Encryption for volume encryption** (Aktivera åtkomst till Azure Disk Encryption för volymkryptering).</span><span class="sxs-lookup"><span data-stu-id="40ef4-137">Check the **Enable access to Azure Disk Encryption for volume encryption**.</span></span>
    - <span data-ttu-id="40ef4-138">Du kan ta bort ditt konto om du vill. Det behövs inte om du endast planerar att använda nyckelvalvet för diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="40ef4-138">You can remove your account if you like, it's not necessary if you only intend to use the Key Vault for disk encryption.</span></span>
    - <span data-ttu-id="40ef4-139">Spara ändringarna genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="40ef4-139">Click **OK** to save the changes.</span></span>

    ![Skärmbild som visar avancerade egenskaper för nyckelvalv med alternativet Azure Disk Encryption kontrollerat och markerat](../media/3-configure-access-policy.png)

1. <span data-ttu-id="40ef4-141">Klicka på **Skapa** för att skapa det nya nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="40ef4-141">Click **Create** to create the new Key Vault.</span></span>

## <a name="enabling-access-policies-in-the-key-vault"></a><span data-ttu-id="40ef4-142">Aktivera åtkomstprinciper i nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="40ef4-142">Enabling access policies in the key vault</span></span>
<span data-ttu-id="40ef4-143">Azure behöver ha åtkomst till krypteringsnycklarna eller hemligheterna i ditt nyckelvalv för att göra dem tillgängliga för den virtuella datorn för start och dekryptering av volymerna.</span><span class="sxs-lookup"><span data-stu-id="40ef4-143">Azure needs access to the encryption keys or secrets in your key vault to make them available to the VM for booting and decrypting the volumes.</span></span> <span data-ttu-id="40ef4-144">Vi gick igenom detta för portalen när vi ändrade **Avancerade åtkomstprinciper** ovan.</span><span class="sxs-lookup"><span data-stu-id="40ef4-144">We covered this for the portal when we changed the **Advanced access policies** above.</span></span>

<span data-ttu-id="40ef4-145">Det finns tre principer som du kan aktivera.</span><span class="sxs-lookup"><span data-stu-id="40ef4-145">There are three policies you can enable.</span></span>
1. <span data-ttu-id="40ef4-146">**Diskkryptering** – krävs för Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="40ef4-146">**Disk encryption** - Required for Azure Disk encryption.</span></span>
1. <span data-ttu-id="40ef4-147">**Distribution** – (valfritt) gör så att resursprovidern Microsoft.Compute kan hämta hemligheter från det här nyckelvalvet när nyckelvalvet refereras till under resursskapande, till exempel när en virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="40ef4-147">**Deployment** - (Optional) Enables the Microsoft.Compute resource provider to retrieve secrets from this key vault when this key vault is referenced in resource creation, for example when creating a virtual machine.</span></span>
1. <span data-ttu-id="40ef4-148">**Malldistribution** – (valfritt) gör så att Azure Resource Manager kan hämta hemligheter från det här nyckelvalvet när nyckelvalvet refereras till i en malldistribution.</span><span class="sxs-lookup"><span data-stu-id="40ef4-148">**Template deployment** - (Optional) Enables Azure Resource Manager to get secrets from this key vault when this key vault is referenced in a template deployment.</span></span>

<span data-ttu-id="40ef4-149">Så här aktiverar du principen för diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="40ef4-149">Here's how to enable the disk encryption policy.</span></span> <span data-ttu-id="40ef4-150">De andra två är liknande men använder olika flaggor.</span><span class="sxs-lookup"><span data-stu-id="40ef4-150">The other two are similar but use different flags.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <keyvault-name> -ResourceGroupName <resource-group> -EnabledForDiskEncryption
```

```azurecli
az keyvault update --name <keyvault-name> --resource-group <resource-group> --enabled-for-disk-encryption "true"
```

## <a name="encrypting-an-existing-vm-disk"></a><span data-ttu-id="40ef4-151">Kryptera en befintlig virtuell datordisk</span><span class="sxs-lookup"><span data-stu-id="40ef4-151">Encrypting an existing VM disk</span></span>

<span data-ttu-id="40ef4-152">När du har konfigurerat nyckelvalvet kan du kryptera den virtuella datorn med antingen Azure CLI eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="40ef4-152">Once you have the Key Vault setup, you can encrypt the VM using either Azure CLI or Azure PowerShell.</span></span> <span data-ttu-id="40ef4-153">Första gången du krypterar en virtuell dator i Windows kan du välja att kryptera alla diskar eller endast OS-disken.</span><span class="sxs-lookup"><span data-stu-id="40ef4-153">The first time you encrypt a Windows VM, you can choose to encrypt either all disks or the OS disk only.</span></span> <span data-ttu-id="40ef4-154">På vissa Linux-distributioner går det endast att kryptera datadiskarna.</span><span class="sxs-lookup"><span data-stu-id="40ef4-154">On some Linux distributions, only the data disks may be encrypted.</span></span> <span data-ttu-id="40ef4-155">För att vara kvalificerade för kryptering måste dina Windows-diskar formateras som NTFS-volymer.</span><span class="sxs-lookup"><span data-stu-id="40ef4-155">To be eligible for encryption, your Windows disks must be formatted as NTFS volumes.</span></span>

> [!WARNING]
> <span data-ttu-id="40ef4-156">Du måste ta en ögonblicksbild eller en säkerhetskopia av de hanterade diskarna innan du kan aktivera kryptering.</span><span class="sxs-lookup"><span data-stu-id="40ef4-156">You must take a snapshot or a backup of managed disks before you can turn on encryption.</span></span> <span data-ttu-id="40ef4-157">Flaggan `SkipVmBackup` som anges nedan berättar för verktyget att säkerhetskopieringen har slutförts på hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="40ef4-157">The `SkipVmBackup` flag specified below tells the tool that the backup is complete on managed disks.</span></span> <span data-ttu-id="40ef4-158">Utan säkerhetskopieringen kan du inte återställa den virtuella datorn om krypteringen av någon anledning misslyckas.</span><span class="sxs-lookup"><span data-stu-id="40ef4-158">Without the backup, you will be unable to recover the VM if the encryption fails for some reason.</span></span>

<span data-ttu-id="40ef4-159">Med PowerShell används `Set-AzureRmVmDiskEncryptionExtension` cmdlet för att aktivera kryptering.</span><span class="sxs-lookup"><span data-stu-id="40ef4-159">With PowerShell, use the `Set-AzureRmVmDiskEncryptionExtension` cmdlet to enable encryption.</span></span>

```powershell

Set-AzureRmVmDiskEncryptionExtension `
    -ResourceGroupName <resource-group> `
    -VMName <vm-name> `
    -VolumeType [All | OS | Data]
    -DiskEncryptionKeyVaultId <keyVault.ResourceId> `
    -DiskEncryptionKeyVaultUrl <keyVault.VaultUri> `
     -SkipVmBackup
```

<span data-ttu-id="40ef4-160">För Azure CLI används `az vm encryption enable` kommandot för att aktivera kryptering.</span><span class="sxs-lookup"><span data-stu-id="40ef4-160">For the Azure CLI, use the `az vm encryption enable` command to enable encryption.</span></span>

```azurecli
az vm encryption enable \
    --resource-group <resource-group> \
    --name <vm-name> \
    --disk-encryption-keyvault <keyvault-name> \
    --volume-type [all | os | data] \
    --skipvmbackup
```

## <a name="viewing-the-status-of-the-disk"></a><span data-ttu-id="40ef4-161">Visa status för disken</span><span class="sxs-lookup"><span data-stu-id="40ef4-161">Viewing the status of the disk</span></span>

<span data-ttu-id="40ef4-162">Du kan kontrollera huruvida specifika diskar är krypterade.</span><span class="sxs-lookup"><span data-stu-id="40ef4-162">You can check whether specific disks are encrypted or not.</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName <resource-group> -VMName <vm-name>
```

```azurecli
az vm encryption status --resource-group <resource-group> --name <vm-name>
```

<span data-ttu-id="40ef4-163">Båda dessa kommandon returnerar status för varje disk som är ansluten till den angivna virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="40ef4-163">Both of these commands will return the status of each disk attached to the specified VM.</span></span>

## <a name="decrypting-drives"></a><span data-ttu-id="40ef4-164">Dekryptera enheter</span><span class="sxs-lookup"><span data-stu-id="40ef4-164">Decrypting drives</span></span>

<span data-ttu-id="40ef4-165">Du kan upphäva krypteringen med hjälp av PowerShell `Disable-AzureRmVMDiskEncryption`.</span><span class="sxs-lookup"><span data-stu-id="40ef4-165">You can reverse the encryption through PowerShell using `Disable-AzureRmVMDiskEncryption`.</span></span>

```powershell
Disable-AzureRmVMDiskEncryption -ResourceGroupName <resource-group> -VMName <vm-name>
```

<span data-ttu-id="40ef4-166">För Azure CLI används `vm encryption disable` kommandot.</span><span class="sxs-lookup"><span data-stu-id="40ef4-166">For the Azure CLI, use the `vm encryption disable` command.</span></span>

```azurecli
az vm encryption disable --resource-group <resource-group> --name <vm-name>
```

<span data-ttu-id="40ef4-167">Dessa kommandon inaktiverar kryptering för volymer av alla typer för den angivna virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="40ef4-167">These commands disable encryption for volumes of type all for the specified virtual machine.</span></span> <span data-ttu-id="40ef4-168">Precis som krypteringsversionen kan du ange en `-VolumeType`-parameter `[All | OS | Data]` för att bestämma vilka diskar som ska dekrypteras.</span><span class="sxs-lookup"><span data-stu-id="40ef4-168">Just like the encrypt version, you can specify a `-VolumeType` parameter `[All | OS | Data]` to decide what disks to decrypt.</span></span> <span data-ttu-id="40ef4-169">Standardvärdet är `All` om inget annat anges.</span><span class="sxs-lookup"><span data-stu-id="40ef4-169">It defaults to `All` if not supplied.</span></span>

> [!WARNING]
> <span data-ttu-id="40ef4-170">Att inaktivera kryptering av datadiskar på Windows-datorer när både operativsystemet och datadiskarna har krypterats fungerar inte som förväntat.</span><span class="sxs-lookup"><span data-stu-id="40ef4-170">Disabling data disk encryption on Windows VM when both OS and data disks have been encrypted doesn't work as expected.</span></span> <span data-ttu-id="40ef4-171">Istället måste du inaktivera kryptering på alla diskar.</span><span class="sxs-lookup"><span data-stu-id="40ef4-171">You must disable encryption on all disks instead.</span></span>

<span data-ttu-id="40ef4-172">Nu provar vi några av dessa kommandon på en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="40ef4-172">Let's try some of these commands out on a new VM.</span></span>