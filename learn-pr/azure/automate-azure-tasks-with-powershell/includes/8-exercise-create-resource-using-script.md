I den här kursdelen fortsätter du med exemplet på ett företag som skapar administrationsverktyg för Linux. Kom ihåg att du tänker använda virtuella Linux-datorer så att potentiella kunder kan testa din programvara. Du har en resursgrupp som är redo och nu är det dags att skapa de virtuella datorerna.

Företaget har betalat för en monter på en stor Linux-mässa. Du planerar en demonstration med tre terminaler som var och en är ansluten till en separat virtuell Linux-dator. I slutet av varje dag vill du ta bort de virtuella datorerna och återskapa dem, så att de börjar från början varje morgon. Att skapa de virtuella datorerna manuellt efter jobbet när du är trött kan riskera att fel uppstår. Du vill i stället skriva ett PowerShell-skript som skapar de virtuella datorerna automatiskt.

## <a name="write-a-script-that-creates-virtual-machines"></a>Skriva ett skript som skapar virtuella datorer

Följ dessa anvisningar i Cloud Shell till höger för att skriva skriptet:

1. Växla till arbetsmappen i Cloud Shell.

    ```powershell
    cd $HOME\clouddrive
    ```

1. Skapa en ny textfil med namnet **ConferenceDailyReset.ps1**.

    ```powershell
    touch "./ConferenceDailyReset.ps1"
    ```

1. Öppna den inbyggda redigeraren och välj filen **ConferenceDailyReset.ps1**.

    ```powershell
    code "./ConferenceDailyReset.ps1"
    ```
    > [!TIP]
    > Det inbyggda Cloud Shell stöder även vim, nano och emacs om du föredrar att använda en av dessa redigerare.

1. Börja med att spara indataparametern i en variabel. Lägg till följande rad i skriptet:

    ```powershell
    param([string]$resourceGroup)
    ```

    > [!NOTE]
    > Normalt skulle du behöva autentisera med Azure med dina autentiseringsuppgifter via `Connect-AzureRmAccount`, och detta skulle kunna göras i skriptet. I Cloud Shell-miljön är du dock redan autentiserad, så det behövs inte.

1. Fråga efter ett användarnamn och lösenord för den virtuella datorns administratörskonto och ange resultatet i en variabel:

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. Skapa en loop som körs tre gånger:

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. I loopens brödtext skapar du ett namn för varje virtuell dator och lagrar dem i en variabel och skriver ut den i konsolen:

    ```powershell
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    ```

1. Skapa därefter en virtuell dator med hjälp av variabeln `$vmName`:

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
   ```

1. Spara filen. Du kan använda menyn ”...” i det övre högra hörnet i redigeraren. Det finns även vanliga kortkommandon för Spara.

Det färdiga skriptet bör se ut så här:

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a>Köra skriptet

Stäng redigeraren via snabbmenyn ”...”.

1. Kör skriptet.

    ```powershell
    .\ConferenceDailyReset.ps1 <rgn>[sandbox resource group name]</rgn>
    ```
    
Skriptet kommer att ta flera minuter att slutföra. När det är klart kontrollerar du att det har körts genom att titta på de resurser som du nu har i resursgruppen:

```powershell
Get-AzureRmResource -ResourceType Microsoft.Compute/virtualMachines
```

Du bör se tre virtuella datorer – var och en med ett unikt namn.

Du skrev ett skript som automatiserade skapandet av tre virtuella datorer i resursgruppen med en skriptparameter. Skriptet är kort och enkelt, men automatiserar en process som tar lång tid att slutföra manuellt med portalen.