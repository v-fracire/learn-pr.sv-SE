Anta att du utvecklar ett ekonomiprogram för nystartade företag. Du vill skydda kundernas data och har därför bestämt dig för att implementera Azure Disk Encryption (ADE) på alla operativsystem och datadiskar på de servrar som ska vara värdar för programmet. Enligt dina efterlevnadskrav måste du också ansvara för din egen krypteringsnyckel.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

I den här modulen får du kryptera diskar på en befintlig virtuell dator och hantera krypteringsnycklar med ditt eget Azure Key Vault.

## <a name="prepare-the-environment"></a>Förbereda miljön

Vi börjar med att distribuera en ny virtuell Windows-dator i en virtuell Azure-dator.

### <a name="deploy-windows-vm"></a>Distribuera en virtuell Windows-dator

Använd Azure PowerShell till att skapa och distribuera en ny virtuell Windows-dator.

1. Börja med att bestämma var du vill placera de nya resurserna. Välj en plats nära dig i listan nedan.

    <!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]
    

1. Definiera en PowerShell-variabel som innehåller den valda platsen. Här definieras den som ”USA, östra”, ändra det till den plats du vill använda.

    ```powershell
    $location = "eastus"
    ```
    
    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Definiera sedan några mer praktiska variabler för att fånga _namnet_ på den virtuella datorn och _resursgruppen_. Observera att vi använder den i förväg skapade resursgruppen här. Normalt skulle du skapa en _ny_ resursgrupp i din prenumeration med hjälp av `New-AzureRmResourceGroup`.

    ```powershell
    $vmName = "fmdata-vm01"
    $rgName = "<rgn>[sandbox Resource Group]</rgn>"
    ```
    
1. Använd `New-AzureRmVm` för att skapa en ny virtuell dator.
    
    ```powershell
    New-AzureRmVm `
        -ResourceGroupName $rgName `
        -Name $vmName `
        -Location $location `
        -OpenPorts 3389
    ```
    
    Ange ett användarnamn och lösenord för den virtuella datorn när du uppmanas till det av Cloud Shell. Detta används som det första skapade kontot för den virtuella datorn.
    
    > [!NOTE]
    > Det här kommandot använder vissa standardinställningar eftersom vi inte har angett så många alternativ. Mer specifikt skapas en _Windows 2016 Server_-avbildning med storleken _Standard_DS1_v2_. Kom ihåg att virtuella datorer på Basic-nivån inte stöder ADE om du vill ange storleken på den virtuella datorn.

1. När den virtuella datorn är klar med distributionen registrerar du informationen om den virtuella datorn i en variabel. Du kan använda den här variabeln till att utforska det som har skapats.

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```
    
1. Du kan se de OS-diskar som är anslutna till den virtuella datorn:

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
        
1. Kontrollera den aktuella statusen för kryptering på OS-disken (och eventuella datadiskar).

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  `
        -ResourceGroupName $rgName `
        -VMName $vmName
    ```

    Som du ser är diskarna just nu _okrypterade_. 

    ```output
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : NotEncrypted
    OsVolumeEncryptionSettings :
    ProgressMessage            : No Encryption extension or metadata found on the VM
    ```
    
Vi ändrar på det.
    
## <a name="encrypt-the-vm-disks-with-azure-disk-encryption"></a>Kryptera VM-diskarna med Azure Disk Encryption

Vi behöver skydda dessa data, så vi krypterar diskarna. Kom ihåg att det finns flera åtgärder som vi behöver utföra:

1. Skapa ett nyckelvalv.
1. Ange nyckelvalvet så att det stöder diskkryptering.
1. Berätta för Azure att kryptera VM-diskarna med hjälp av nyckeln som lagras i nyckelvalvet.

> [!TIP]
> Vi går igenom stegen separat, men när du gör detta i din egen prenumeration kan du använda ett praktiskt PowerShell-skript som är länkat i sammanfattningen av den här modulen.

### <a name="create-a-key-vault"></a>Skapa ett nyckelvalv

För att skapa ett nyckelvalv i Azure Key Vault måste vi aktivera tjänsten i vår prenumeration. Det här är ett engångskrav.

> [!TIP]
> Beroende på din prenumeration kan du behöva aktivera **Microsoft.KeyVault**-providern med cmdleten `Register-AzureRmResourceProvider`. Detta är inte nödvändigt i Azure-sandboxprenumerationen.

1. Bestäm ett namn på det nya nyckelvalvet. Det måste vara unikt och kan bestå av mellan 3 och 24 tecken, innehålla siffror, bokstäver, och bindestreck. Prova att lägga till några slumptal i slutet, där du ersätter den ”1234” nedan.

    ```powershell
    $keyVaultName = "mvmdsk-kv-1234"
    ```
        
1. Skapa ett nyckelvalv i Azure Key Vault med `New-AzureRmKeyVault`.
    - Se till att det placeras i samma resursgrupp _och_ på samma plats som den virtuella datorn.
    - Aktivera nyckelvalvet för användning med diskkryptering. 
    - Ange ett unikt namn på nyckelvalvet.

    ```powershell
    New-AzureRmKeyVault -VaultName $keyVaultName `
        -Location $location `
        -ResourceGroupName $rgName `
        -EnabledForDiskEncryption
    ```

    Du får en varning från det här kommandot om att inga användare har åtkomst.

    ```output
    WARNING: Access policy is not set. No user or application have access permission to use this vault. This can happen if the vault was created by a service principal. Please use Set-AzureRmKeyVaultAccessPolicy to set access policies.
    ```

    Detta är okej eftersom vi bara använder valvet för att lagra krypteringsnycklar för den virtuella datorn och användarna inte behöver åtkomst till dessa data.

### <a name="encrypt-the-disk"></a>Kryptera disken

Vi är nästan redo att kryptera diskarna. Innan vi gör det vill vi utfärda en varning om att skapa säkerhetskopior.

> [!IMPORTANT]
> Om detta vore ett produktionssystem skulle vi behöva utföra en säkerhetskopiering av de hanterade diskarna – antingen med Azure Backup eller genom att skapa en ögonblicksbild. Du kan skapa ögonblicksbilder i Azure-portalen eller via kommandoraden. I PowerShell använder du cmdleten `New-AzureRmSnapshot`. Eftersom det här är en enkel övning och vi kommer att slänga dessa data när du är klar hoppar vi över det här steget. 

1. Börja med att definiera en variabel som innehåller informationen om nyckelvalvet.

    ```powershell
    $keyVault = Get-AzureRmKeyVault `
        -VaultName $keyVaultName `
        -ResourceGroupName $rgName
    ```

1. Använd sedan cmdleten `Set-AzureRmVmDiskEncryptionExtension` till att kryptera VM-diskarna.
    - Med parametern `VolumeType` kan du ange vilka diskar som ska krypteras: [_Alla_ | _OS_ | _Data_]. Värdet _Alla_ ställs in som standard. Du kan endast kryptera datadiskar för vissa distributioner av Linux.
    - Du måste ange flaggan `SkipVmBackup` för hanterade diskar, annars misslyckas kommandot eftersom det inte finns någon ögonblicksbild.

    ```powershell
    Set-AzureRmVmDiskEncryptionExtension `
        -ResourceGroupName $rgName `
        -VMName $vmName `
        -VolumeType All `
        -DiskEncryptionKeyVaultId $keyVault.ResourceId `
        -DiskEncryptionKeyVaultUrl $keyVault.VaultUri `
        -SkipVmBackup
    ```

1. Denna cmdlet varnar dig om att den virtuella datorn måste kopplas från och att uppgiften kan ta flera minuter att slutföra. Gå vidare och låt den fortsätta.

    ```output
    Enable AzureDiskEncryption on the VM
    This cmdlet prepares the VM and enables encryption which may reboot the machine and takes 10-15 minutes to
    finish. Please save your work on the VM before confirming. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    ```
    
1. När den är klar kan du kontrollera krypteringsstatusen igen.

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
    ```

    Nu bör diskarna vara krypterade. Eventuella anslutna datadiskar som är synliga för Windows krypteras också.

    ```output
    OsVolumeEncrypted          : Encrypted
    DataVolumesEncrypted       : NoDiskFound
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : Provisioning succeeded
    ```

> [!NOTE]        
> Nya diskar som har lagts till efter krypteringen kommer _inte_ att krypteras automatiskt. Du kan köra cmdleten `Set-AzureRmVMDiskEncryptionExtension` igen för att kryptera nya diskar. Se till att ange ett nytt sekvensnummer om du lägger till diskar till en virtuell dator som har diskar som redan krypterats. Dessutom krypteras inte diskar som inte är synliga för operativsystemet – disken måste vara korrekt partitionerad, formaterad och monterad för att kunna upptäckas av Bitlocker-tillägget.