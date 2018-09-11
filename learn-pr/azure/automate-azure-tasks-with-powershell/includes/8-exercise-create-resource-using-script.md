I den här övningen fortsätter du med exemplet på ett företag som skapar administrationsverktyg för Linux. Kom ihåg att du tänker använda virtuella Linux-datorer så att potentiella kunder kan testa din programvara. Du har en resursgrupp som är redo och nu är det dags att skapa de virtuella datorerna.

Företaget har betalat för en monter på en stor Linux-mässa. Du planerar en demonstration med tre terminaler som var och en är ansluten till en separat virtuell Linux-dator. I slutet av varje dag vill du ta bort de virtuella datorerna och återskapa dem, så att de börjar från början varje morgon. Att skapa de virtuella datorerna manuellt efter jobbet när du är trött kan riskera att fel uppstår. Du vill i stället skriva ett PowerShell-skript som skapar de virtuella datorerna automatiskt.

## <a name="write-a-script-that-creates-virtual-machines"></a>Skriva ett skript som skapar virtuella datorer

Följ dessa steg för att skriva skriptet:

1. Skapa en ny textfil med namnet **ConferenceDailyReset.ps1**.

2. Ange parametern i en variabel:

    ```powershell
    param([string]$resourceGroup)
    ```

3. Autentisera till Azure med dina autentiseringsuppgifter:

    ```powershell
    Connect-AzureRmAccount
    ```

4. Fråga efter ett användarnamn och lösenord för den virtuella datorns administratörskonto och ange resultatet i en variabel:

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

5. Skapa en loop som körs tre gånger:

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

6. I loopens brödtext skapar du ett namn för varje virtuell dator och lagrar dem i en variabel:

    ```powershell
    $vmName = "ConferenceDemo" + $i
    ```

7. Skapa därefter en virtuell dator med hjälp av variabeln `$vmName`:

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
   ```

8. Spara filen.

Det färdiga skriptet bör se ut så här:

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

Connect-AzureRmAccount

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i

    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a>Köra skriptet

Starta PowerShell och gå till den katalog där du sparade skriptfilen. Använd följande kommando för att köra skriptet:

```powershell
.\ConferenceDailyReset.ps1 TrialsResourceGroup
```

Det kan ta några minuter att slutföra skriptet. När det är klart kontrollerar du att det har körts:

1. Logga in på Azure-portalen i en webbläsare.
2. Klicka på **Resursgrupper** i det vänstra navigeringsfönstret.
3. I listan med resursgrupper klickar du på **TrialsResourceGroup**. I listan med resurser bör du se de nyligen skapade virtuella datorerna och deras associerade resurser.

## <a name="summary"></a>Sammanfattning
Du skrev ett skript som automatiserade skapandet av tre virtuella datorer i resursgruppen med en skriptparameter. Skriptet är kort och enkelt, men automatiserar en process som tar lång tid att slutföra manuellt med portalen.