Anta att du utvecklar ett ekonomiprogram för nystartade företag. Du vill kontrollera att dina kunders data är skyddad, så att du har valt att implementera Azure Disk Encryption (ADE) för alla operativsystem och diskar på de servrar som ska vara värd för det här programmet. Enligt dina efterlevnadskrav måste du också ansvara för din egen krypteringsnyckel.

I den här enheten du kryptera diskar på befintliga virtuella Windows-datorer och hantera krypteringsnycklar med din egen Azure Key Vault.

> [!IMPORTANT] 
> Den här övningen förutsätter att Azure PowerShell är installerat på datorn. Gå till [installera Azure PowerShell på Windows med PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) information om hur du installerar Azure PowerShell.

## <a name="prepare-the-environment"></a>Förbereda miljön

Vi börjar genom att distribuera en virtuell Windows-dator till en ny resursgrupp och sedan lägga till en datadisk till den virtuella datorn.

### <a name="deploy-windows-vm-using-the-azure-portal"></a>Distribuera Windows virtuell dator med Azure portal

Här använder du Azure-portalen att skapa och distribuera en virtuell Windows-dator. Starta genom att definiera grundläggande information för den virtuella datorn:

1. Öppna en webbläsare och gå till [Azure-portalen](http://portal.azure.com) och logga in med dina vanliga autentiseringsuppgifter.

1. I sidopanelen klickar du på **Virtuella datorer** och sedan på **Skapa virtuell dator**.

1. På bladet Beräkning i området **Rekommenderas** klickar du på **Windows Server**.

1. Klicka på **Windows Server 2016 Datacenter** i bladet **Windows Server**.

1. Klicka på **Skapa** i bladet **Windows Server 2016 Datacenter**.

1. I bladet **Basic** i rutan **Namn** skriver du **moneyappsvr01.**

1. I rutorna **Användarnamn** och **Lösenord** skriver du ett namn och lösenord för ett administratörskonto på den här servern.

1. Välj din Azure-prenumeration i fältet **Prenumeration**.

1. Under **resursgrupp**väljer **Skapa ny**. I rutan skriver **moneyapprg**.

1. Välj en region nära dig i listrutan **Plats**.

1. Klicka på **OK**.

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a>Välj storlek för den virtuella datorn och starta distributionen

> [!IMPORTANT]
> Kom ihåg att basic-nivån virtuella datorer inte stöder ADE.

1. På den **väljer du en storlek** bladet och välja en **Standard** SKU, till exempel **B1s**. Klicka sedan på **Välj**.

1. På den **inställningar** bladet i den **Välj offentliga inkommande portar** klickar du på **RDP**. Bläddrar sedan nedåt och klicka på **OK**.

1. På bladet **API-app** klickar du på **Skapa**.

1. Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen.

### <a name="add-a-data-disk-to-the-vm"></a>Lägg till en datadisk i en virtuell dator

1. I den vänstra menyn klickar du på **Alla resurser** och klickar sedan på **moneyappsvr01**.

1. På bladet **Virtuella datorer**, under **INSTÄLLNINGAR**, klickar du på **Diskar**.

1. Observera, på bladet **Diskar**, att operativsystemdiskens krypteringsstatus för närvarande är **Ej aktiverat** och klicka sedan på **Lägg till datadisk**.

1. Klicka på listan **Namn** och klicka sedan på **Skapa disk**.

1. I bladet **Skapa hanterad disk** i **Namn** skriver du **moneyappsvr01_data**.

1. Under **Resursgrupp** väljer du **Använd befintlig** och välj sedan **moneyapprg**.

1. Klicka på **Skapa**.

1. Vänta tills disken har skapats innan du fortsätter.

1. På bladet **diskar** klickar du på **Spara**. Observera att datadisken krypteringsstatus är för närvarande **inte aktiverat**.

## <a name="configure-disk-encryption-prerequisites"></a>Konfigurera krävs för kryptering av disk

Du kan nu använda nödvändiga konfigurationsskriptet för Azure Disk Encryption för att konfigurera alla krav för disk-kryptering. Det här skriptet skapar och förbereda key vault i samma region som den virtuella datorn.

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a>Förbered nödvändiga konfigurationsskriptet för Azure Disk Encryption

1. Gå till den [nödvändiga installationsskriptet för Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1) GitHub-sidan.

1. Klicka på **Raw** på GibHub-sidan.

1. Använd Ctrl-A för att markera all text på sidan och sedan använda Ctrl + C för att kopiera all text på sidan till Urklipp.

1. Klicka på din dator, **starta**, och bläddra till **Windows PowerShell ISE**.

1. Högerklicka på **Windows PowerShell ISE**, och klicka på **kör som administratör**.

1. I administratören: Windows PowerShell ISE-fönster, klicka på **visa**, och klicka sedan på **visa Skriptfönster**.

1. Klistra in den kopierade texten i skriptfönstret.

1. I skriptfönstret letar du reda på följande kodblock:

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. I kodblocket, ändra `$false` till `$true`.

1. Klicka på **Arkiv**, klicka sedan på **Spara som** och navigera till mappen som du vill använda för att spara skriptet.

1. I textrutan för **filnamn** anger du **ADEPrereqScript.ps1** och klickar på **Spara**.

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a>Kör de nödvändiga installationsskriptet för Azure Disk Encryption

1. I PowerShell ISE-konsolen fönstret, Skriv följande kommando och tryck på **RETUR**:

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. I PowerShell ISE-konsolen fönstret, Skriv följande kommando och tryck på **RETUR**:

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   Om dialogrutan **Ändring av körningsprincipen** visas klickar du antingen på **Ja till alla** eller **Ja** (om du inte får alternativet _Ja till alla_).

1. I PowerShell ISE-konsolen fönstret, Skriv följande kommando och tryck på **RETUR**:

   ```powershell
   Login-AzureRmAccount
   ```

1. Ange dina autentiseringsuppgifter för Azure.

1. Välj din **SubscriptionId**-sträng och kopiera den till Urklipp.

1. I PowerShell ISE, klickar du på **filen**, och klicka sedan på **kör**.

1. I konsolfönstret i den **resourceGroupName:** anger **moneyapprg**. Tryck på **RETUR**.

1. I konsolfönstret i den **keyVaultName:** anger **moneyappkv**. Tryck på **RETUR**.

1. I konsolfönstret i **plats:** anger du den plats som du använde när du skapade den virtuella datorn.

1. I konsolfönstret i **subscriptionId:** klistrar du in ditt prenumerations-ID.

1. Nyckelvalvet **moneyappkv** kommer nu att skapas. När detta har slutförts väljer du översiktstext (i grönt och kopierar den till Anteckningar.

1. Tryck på **RETUR** att fortsätta.

1. I konsolfönstret i den **aadAppName:** anger **moneyapp**. Tryck på **RETUR**.

1. Azure AD-programmet **moneyapp** skapas nu. När detta har slutförts väljer du översiktstext (i grönt och kopierar den till Anteckningar.

1. Tryck på **RETUR** att fortsätta.

### <a name="encrypt-your-vm-disks-with-powershell"></a>Kryptera dina virtuella datordiskar med PowerShell

Kontrollera krypteringsstatusen för operativsystemet och datadiskarna:

1. I PowerShell ISE-konsolen fönstret, Skriv följande kommando och tryck på **RETUR**:

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > Namnet på den virtuella datorn måste stå inom enkla citattecken.

1. I fönstret PowerShell ISE skriptet anger du följande kommando och tryck på **RETUR**:

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. I dialogrutan **Aktivera AzureDiskEncryption på den virtuella datorn** dialogrutan klickar du på **Ja** och noterar meddelandet om att krypteringen kan ta 10 – 15 minuter att slutföra.

>[!IMPORTANT]
> Vänta tills kommandot har slutförts innan du fortsätter med den här övningen.

### <a name="verify-the-encryption-status-of-your-vm-disks"></a>Kontrollera krypteringsstatusen för de virtuella datordiskarna

Växla till Azure Portal. På den **diskar** bladet för **moneyappsvr01**, Observera att status för diskkryptering för Operativsystemet och datadiskarna är nu **aktiverad**.
