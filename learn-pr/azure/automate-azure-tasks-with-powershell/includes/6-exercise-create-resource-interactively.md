Kom ihåg vårt ursprungliga scenario – vi skapade virtuella datorer för att testa vår CRM-programvara. När en ny version är tillgänglig vill vi förbättra vår nya virtuella dator så att vi kan testa hela installationen från en ren avbildning. När vi är klara vill vi ta bort den virtuella datorn.

Nu ska vi prova de kommandon som du använder för att skapa en virtuell dator.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm-with-azure-powershell"></a>Skapa en virtuell Linux-dator med Azure PowerShell

Eftersom vi använder sandbox-miljön för Azure kan behöver du inte skapa en resursgrupp. Välj istället **resursgruppen<rgn> </rgn>[Resursgruppsnamn för sandbox-miljö]**. Dessutom bör du vara medveten om platsbegränsningar.

Nu ska vi skapa en ny virtuell Azure-dator med PowerShell.

1. Använd cmdlet:en `New-AzureRmVm` för att skapa en virtuell dator.
    - Använd den resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**.
    - Namnge den virtuella datorn – vanligtvis du vill använda ett mer beskrivande namn som identifierar den virtuella datorns syfte, plats, och antal instanser (om det finns fler än en). Vi använder ”testvm-eus-01” för ”virtuell testdator East US instans 1”. Använd ditt eget namn baserat på var den virtuella datorn finns.
    - Välj en plats nära dig i följande lista som är tillgänglig i Azure-sandbox-miljön. Ändra värdet i nedanstående exempelkommando om du använder kopiera och klistra in.

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - Använd ”UbuntuLTS” för en här avbildningen – Detta är Ubuntu Linux.
    - Använd cmdlet:en `Get-Credential` och mata in resultaten i parametern `Credential`.
    - Lägg till parametern `-OpenPorts` och ”22” som port – vi kan nu logga in med SSH på datorn.
 
    ```powershell
    New-AzureRmVm -ResourceGroupName <rgn>[Sandbox resource group name]</rgn> -Name "testvm-eus-01" -Credential (Get-Credential) -Location "East US" -Image UbuntuLTS -OpenPorts 22
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
    
1. Det här kan ta några minuter. När detta sker kan du fråga och tilldela objektet från den virtuella datorn till en variabel (`$vm`).

    ```powershell
    $vm = Get-AzureRmVM -Name "testvm-eus-01" -ResourceGroupName <rgn>[Sandbox resource group name]</rgn>
    ```
    
1. Hämta sedan värdet för att dumpa information om den virtuella datorn:

    ```powershell
    $vm
    ```

    Du bör se något liknande följande:

    ```output
    ResourceGroupName : <rgn>[Sandbox resource group name]</rgn>
    Id                : /subscriptions/xxxxxxxx-xxxx-aaaa-bbbb-cccccccccccc/resourceGroups/<rgn>[Sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/testvm-eus-01
    VmId              : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    Name              : testvm-eus-01
    Type              : Microsoft.Compute/virtualMachines
    Location          : eastus
    Tags              : {}
    HardwareProfile   : {VmSize}
    NetworkProfile    : {NetworkInterfaces}
    OSProfile         : {ComputerName, AdminUsername, LinuxConfiguration, Secrets}
    ProvisioningState : Succeeded
    StorageProfile    : {ImageReference, OsDisk, DataDisks}
    ```
    
1. Du kan få åtkomst till komplexa objekt via en punktsyntax (”.”). Till exempel, för att visa egenskaperna i objektet `VMSize` som är associerade med området HardwareProfile kan du skriva:

    ```powershell
    $vm.HardwareProfile
    ```

1. Eller för att få information om en av diskarna:

    ```powershell
    $vm.StorageProfile.OsDisk
    ```

1. Du kan även skicka objekt från virtuella datorer till andra cmdlet:ar. Till exempel, detta kommer att hämta din virtuella dators offentliga IP-adress:

    ```powershell
    $vm | Get-AzureRmPublicIpAddress
    ```

1. Med IP-adressen kan du ansluta till den virtuella datorn med SSH. Till exempel om du använde användarnamnet ”bob”, och IP-adressen är ”205.22.16.5” ansluter kommandot till Linux-datorn:

```powershell
ssh bob@205.22.16.5
```

## <a name="delete-a-vm"></a>Ta bort en virtuell dator

Nu ska vi ta bort den virtuella datorn, bara för att testa några fler kommandon. Först ska vi stänga av den.

```powershell
Stop-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

Nu ska vi ta bort den virtuella datorn med cmdlet:en `Remove-AzureRmVM` :

```powershell
Remove-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

Testa det här kommandot för att lista alla resurser i resursgruppen:

```powershell
Get-AzureRmResource -ResourceGroupName $vm.ResourceGroupName | ft
```

Du bör se en massa resurser (diskar, virtuella nätverk osv.) som fortfarande finns. 

```output
Microsoft.Compute/disks
Microsoft.Network/networkInterfaces
Microsoft.Network/networkSecurityGroups
Microsoft.Network/publicIPAddresses
Microsoft.Network/virtualNetworks
```

Detta beror på att `Remove-AzureRmVM` kommandot _endast tar bort den virtuella datorn_. Den tar inte bort någon annan resurs. Nu skulle vi vanligtvis bara ta bort resursgruppen och var klara. Men vi ska rensa den manuellt som övning. Du bör se ett mönster i kommandona.

1. Ta bort nätverksgränssnittet.

    ```powershell
    $vm | Remove-AzureRmNetworkInterface –Force
    ```
    
1. Ta bort hanterade OS-diskar och lagringskonton

    ```powershell
    Get-AzureRmDisk -ResourceGroupName $vm.ResourceGroupName -DiskName $vm.StorageProfile.OSDisk.Name | Remove-AzureRmDisk -Force
    ```

1. Ta sedan bort det virtuella nätverket.

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmVirtualNetwork -Force
    ```

1. Ta bort nätverkssäkerhetsgruppen.

    ```powershell
    Get-AzureRmNetworkSecurityGroup -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmNetworkSecurityGroup -Force
    ```

1. Och till sist, den offentliga IP-adressen.

    ```powershell
    Get-AzureRmPublicIpAddress -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmPublicIpAddress -Force
    ```

Nu bör vi ha fått med alla resurser. Kontrollera resursgruppen för att vara på den säkra sidan. Vi utförde en mängd manuella kommandon här, men en bättre metod hade varit att skriva ett _skript_ så att vi kunde återanvända logiken senare för att skapa eller ta bort en virtuell dator. Låt oss titta på skript med PowerShell.