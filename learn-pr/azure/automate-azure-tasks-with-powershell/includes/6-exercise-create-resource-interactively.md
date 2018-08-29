Anta att du arbetar på ett företag som tillverkar en uppsättning verktyg för Linux-administration. Ditt jobb är att hjälpa potentiella kunder att testa programvaran innan de köper den. Eftersom programvaran gör ändringar i operativsystemet på rotnivå har du valt att skapa en virtuell Linux-dator för varje utvärderingsversion. Du skapar de virtuella datorer som behövs och tar bort dem i slutet av provperioden. På så sätt börjar varje kund börjar med en ren version av operativsystemet. 

Du håller de här virtuella datorerna åtskilda från företagets egna virtuella testdatorer genom att skapa en dedikerad resursgrupp för dem. Du behöver bara en resursgrupp, så det är lämpligt att använda Azure PowerShell i interaktivt läge för den här uppgiften.

## <a name="steps-to-create-a-resource-group"></a>Steg för att skapar en resursgrupp

1. Starta PowerShell.

1. Importera modulen till den aktuella sessionen så att du kommer åt cmdletarna i Azure.

   ```powershell
   Import-Module AzureRM
   ```

1. Anslut till Azure med kommandot nedan. När du har angett kommandot autentiserar du dig med dina autentiseringsuppgifter för Azure.

   ```powershell
   Connect-AzureRmAccount
   ```

1. Skapa en resursgrupp.

    ```powershell
    New-AzureRmResourceGroup -Name "TrialsResourceGroup" -Location "East US"
    ```

1. Kontrollera att resursgruppen har skapats.

    ```powershell
    Get-AzureRmResource | Format-Table
    ```
Du kan också kontrollera att resursgruppen har skapats i Azure Portal. Det gör du genom att logga in på portalen och gå till avsnittet **Resursgrupper** (se nedan). Den nya resursgruppen ska visas i listan.

![Använda portalen till att visa resursgrupper](../media-drafts/6-listing-resource-groups.png)

## <a name="summary"></a>Sammanfattning
I den här övningen visas ett vanligt mönster för en interaktiv PowerShell-session. Du använde en vanlig cmdlet till att importera AzureRM-modulen och sedan Azure PowerShell-cmdletar till att utföra en viss uppgift. Nu har du en resursgrupp i din prenumeration och är redo att skapa virtuella datorer.