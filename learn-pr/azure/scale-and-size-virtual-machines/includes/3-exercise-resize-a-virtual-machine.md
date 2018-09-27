I den här övningen ska du skapa en virtuell dator och sedan ändra storlek på den med hjälp av portalen och Azure PowerShell.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-vm-with-powershell"></a>Skapa en virtuell dator med PowerShell

1. Vi använder cmdleten `New-AzureRmVm` i Azure PowerShell för att skapa en virtuell dator.
    - Använd resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**.
    - Ge den namnet my-test-vm.
    - Skicka in _Standard_DS2_v2_ för **Storlek**-parametern.
    - Välj en plats nära dig i följande lista som är tillgänglig i Azure-sandbox-miljön. Ändra värdet i nedanstående exempelkommando om du använder kopiera och klistra in.

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - Använd cmdleten `Get-Credential` och mata in resultaten i parametern `Credential` som visas nedan.

       När du uppmanas att ange autentiseringsuppgifter måste du använda LocalAdmin som användarnamn och Adm1nPa$$word som lösenord.

    ```powershell
    New-AzureRmVm `
        -ResourceGroupName <rgn>[sandbox resource group name]</rgn> `
        -Name my-test-vm `
        -Credential (Get-Credential) `
        -Size "Standard_DS2_v2" `
        -Location eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


1. Vänta tills distributionen är klar innan du fortsätter med den här övningen.

## <a name="resize-using-the-portal"></a>Ändra storlek med hjälp av portalen

1. Logga in på [Azure-portalen för Sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du aktiverade sandbox-miljön med.

1. Välj **Resursgrupper** i det vänstra sidofältet.

1. Välj resursgruppen <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn> och klicka på det virtuella datorobjektet **my-test-vm** i översikten.

1. I **översikten** för den virtuella datorn bör du lägga märke till att den aktuella storleken på den virtuella datorn är _Standard DS2 v2 (2 virtuella processorer, 7 GB minne)_ vilket är vad vi skapade alldeles nyss.

1. I avsnittet **Inställningar** väljer du **Storlek**.

1. På bladet **Välj en storlek** försöker du hitta storleken **F2s_v2**. Du kommer inte se den i listan över tillgängliga storlekar eftersom den virtuella datorn körs och den storleken är från en annan familj. I vissa fall kommer du behöva stoppa den virtuella datorn om du vill se alla tillgängliga virtuella datorstorlekar.

1. Nu väljer vi en storlek som är tillgänglig för tillfället medan den här virtuella datorn körs. På bladet **Välj en storlek** väljer du _DS3_v2 Standard_ och klickar sedan på **Välj**. Observera meddelandet om att ändra storlek på den virtuella datorn.

1. På panelen **Översikt** bekräftar du att den virtuella datorn har storleksändrats till _Standard DS3 v2 (4 vCPU, 14 GB minne)_.

## <a name="resize-using-powershell"></a>Ändra storlek med hjälp av PowerShell

1. Öppna Azure Cloud Shell i Azure-portalen genom att klicka på knappen Cloud Shell i det övre verktygsfältet.

    Kontrollera att Cloud Shell är konfigurerat att använda PowerShell och inte Bash, längst upp till vänster i fönstret Cloud Shell.

1. Använd följande cmdlet för att hämta listan med tillgängliga virtuella datorstorlekar.

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    ```

1. Använd följande cmdlet för att ändra storlek på den virtuella datorn tillbaka till storleken _DS2_v2_.

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    $vm.HardwareProfile.VmSize = "Standard_DS2_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```

1. Klicka på knappen **Uppdatera** på bladet **my-test-vm** medan du väntar på att PowerShell-kommandot ska slutföras. Du bör se att den virtuella datorn startas om efter storleksändringen.

I den här övningen skapade du en virtuell dator och ändrade storlek på den med två olika verktyg. Ett bra tips att tänka på är att målstorleken kanske inte är tillgänglig medan den virtuella datorn körs. Om du stoppar den virtuella datorn kan du välja fler storlekar.