Anta att ditt företag har beslutat att implementera Azure Disk Encryption (ADE) för alla virtuella datorer. Du behöver utvärdera hur du distribuerar kryptering till befintliga virtuella datorvolymer. Här ska vi titta på kraven för ADE och de steg som behövs för att kryptera diskar på befintliga virtuella Linux- och Windows-datorer.

## <a name="azure-disk-encryption-prerequisites"></a>Förhandskrav för Azure Disk Encryption

Innan du kan kryptera dina virtuella datordiskar behöver du göra följande:

1. Skapa ett nyckelvalv.
1. Konfigurera åtkomstprincipen för nyckelvalv så att den stöder diskkryptering.
1. Använda nyckelvalvet för att lagra krypteringsnycklarna för ADE.

### <a name="azure-key-vault"></a>Azure Key Vault

De krypteringsnycklar som används av ADE kan lagras i ett Azure Key Vault. Azure Key Vault är ett verktyg för att lagra och komma åt hemligheter på ett säkert sätt. En hemlighet är något som du vill begränsa åtkomst till, till exempel API-nycklar, lösenord eller certifikat. Det ger högst tillgänglig, skalbar och säker lagring enligt definition i  Federal Information Processing Standards (FIPS) 140-2 nivå 2-verifierade Maskinvarusäkerhetsmoduler (HSM). Med Key Vault kan du behålla fullständig kontroll över de nycklar som används för att kryptera dina data, och även hantera och granska din nyckelanvändning. 

> [!NOTE]
> Azure Disk Encryption kräver att ditt nyckelvalv och dina virtuella datorer finns i samma Azure-region. Det säkerställer att krypteringshemligheter inte korsar regionala gränser.

Du kan konfigurera och hantera ditt nyckelvalv med:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmKeyVault -Location <location> `
    -ResourceGroupName <resource-group> `
    -VaultName "myKeyVault" `
    -EnabledForDiskEncryption
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az keyvault create \
    --name "myKeyVault" \
    --resource-group <resource-group> \
    --location <location> \
    --enabled-for-disk-encryption True
```

### <a name="azure-portal"></a>Azure-portalen

Ett Azure-nyckelvalv är en resurs som kan skapas i Azure-portalen med den normala resursskapandeprocessen.

1. Klick på **Skapa en resurs** i sidofältet till vänster.

1. Sök efter ”Key vault” (nyckelvalv). Klicka på **Skapa** i informationsfönstret.

    ![Skärmbild som visar nyckelvalv i Azure Marketplace](../media/3-create-keyvault.png)

1. Ange uppgifterna för det nya nyckelvalvet:
    - Ange ett **Namn** för nyckelvalvet
    - Välj den prenumeration som det ska placeras i (standardvärdet är din aktuella prenumeration).
    - Välj en **resursgrupp** eller skapa en ny resursgrupp som ska äga nyckelvalvet.
    - Välj en **plats** för nyckelvalvet. Se till att välja den plats som den virtuella datorn finns på.
    - Du kan välja antingen Standard eller Premium för prisnivån. Den största skillnaden är att Premium-nivån tillåter nycklar som backas av maskinvarukryptering.

1. Du måste ändra åtkomstprinciperna så att de stöder diskkryptering. Som standard läggs _ditt_ kontot till i principen.
    - Välja **Åtkomstprinciper**
    - Klicka på **Avancerade åtkomstprinciper**.
    - Markera **Enable access to Azure Disk Encryption for volume encryption** (Aktivera åtkomst till Azure Disk Encryption för volymkryptering).
    - Du kan ta bort ditt konto om du vill. Det behövs inte om du endast planerar att använda nyckelvalvet för diskkryptering.
    - Spara ändringarna genom att klicka på **OK**.

    ![Skärmbild som visar avancerade egenskaper för nyckelvalv med alternativet Azure Disk Encryption markerat](../media/3-configure-access-policy.png)

1. Klicka på **Skapa** för att skapa det nya nyckelvalvet.

## <a name="enabling-access-policies-in-the-key-vault"></a>Aktivera åtkomstprinciper i nyckelvalvet
Azure behöver ha åtkomst till krypteringsnycklarna eller hemligheterna i ditt nyckelvalv för att göra dem tillgängliga för den virtuella datorn för start och dekryptering av volymerna. Vi gick igenom detta för portalen när vi ändrade **Avancerade åtkomstprinciper** ovan.

Det finns tre principer som du kan aktivera.
1. **Diskkryptering** – krävs för Azure Disk Encryption.
1. **Distribution** – (valfritt) gör så att resursprovidern Microsoft.Compute kan hämta hemligheter från det här nyckelvalvet när nyckelvalvet refereras till under resursskapande, till exempel när en virtuell dator skapas.
1. **Malldistribution** – (valfritt) gör så att Azure Resource Manager kan hämta hemligheter från det här nyckelvalvet när nyckelvalvet refereras till i en malldistribution.

Så här aktiverar du principen för diskkryptering. De andra två är liknande men använder olika flaggor.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <keyvault-name> -ResourceGroupName <resource-group> -EnabledForDiskEncryption
```

```azurecli
az keyvault update --name <keyvault-name> --resource-group <resource-group> --enabled-for-disk-encryption "true"
```

## <a name="encrypting-an-existing-vm-disk"></a>Kryptera en befintlig virtuell datordisk

När du har konfigurerat nyckelvalvet kan du kryptera den virtuella datorn med antingen Azure CLI eller Azure PowerShell. Första gången du krypterar en virtuell dator i Windows kan du välja att kryptera alla diskar eller endast OS-disken. På vissa Linux-distributioner går det endast att kryptera datadiskarna. Om dina Windows-diskar ska vara kvalificerade för kryptering måste de formateras som NTFS-volymer.

> [!WARNING]
> Du måste ta en ögonblicksbild eller en säkerhetskopia av de hanterade diskarna innan du kan aktivera kryptering. Flaggan `SkipVmBackup` som anges nedan berättar för verktyget att säkerhetskopieringen har slutförts på hanterade diskar. Utan säkerhetskopieringen kan du inte återställa den virtuella datorn om krypteringen av någon anledning misslyckas.

Med PowerShell används `Set-AzureRmVmDiskEncryptionExtension`-cmdlet för att aktivera kryptering.

```powershell

Set-AzureRmVmDiskEncryptionExtension `
    -ResourceGroupName <resource-group> `
    -VMName <vm-name> `
    -VolumeType [All | OS | Data]
    -DiskEncryptionKeyVaultId <keyVault.ResourceId> `
    -DiskEncryptionKeyVaultUrl <keyVault.VaultUri> `
     -SkipVmBackup
```

För Azure CLI används `az vm encryption enable`-kommandot för att aktivera kryptering.

```azurecli
az vm encryption enable \
    --resource-group <resource-group> \
    --name <vm-name> \
    --disk-encryption-keyvault <keyvault-name> \
    --volume-type [all | os | data] \
    --skipvmbackup
```

## <a name="viewing-the-status-of-the-disk"></a>Visa status för disken

Du kan kontrollera huruvida specifika diskar är krypterade.

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName <resource-group> -VMName <vm-name>
```

```azurecli
az vm encryption status --resource-group <resource-group> --name <vm-name>
```

Båda dessa kommandon returnerar status för varje disk som är ansluten till den angivna virtuella datorn.

## <a name="decrypting-drives"></a>Dekryptera enheter

Du kan upphäva krypteringen med hjälp av PowerShell `Disable-AzureRmVMDiskEncryption`.

```powershell
Disable-AzureRmVMDiskEncryption -ResourceGroupName <resource-group> -VMName <vm-name>
```

För Azure CLI används `vm encryption disable`-kommandot.

```azurecli
az vm encryption disable --resource-group <resource-group> --name <vm-name>
```

Dessa kommandon inaktiverar kryptering för volymer av alla typer för den angivna virtuella datorn. Precis som krypteringsversionen kan du ange en `-VolumeType`-parameter `[All | OS | Data]` för att bestämma vilka diskar som ska dekrypteras. Standardvärdet är `All` om inget annat anges.

> [!WARNING]
> Att inaktivera kryptering av datadiskar på Windows-datorer när både operativsystemet och datadiskarna har krypterats fungerar inte som förväntat. Istället måste du inaktivera kryptering på alla diskar.

Nu provar vi några av dessa kommandon på en ny virtuell dator.