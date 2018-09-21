I den här modulen skrev vi ett skript som automatiserar skapandet av flera virtuella datorer. Även om skriptet var relativt kort märker du vilken potentiell kraft det har när du kombinerar loopar, variabler och funktioner från PowerShell med cmdletar från Azure PowerShell.

Azure PowerShell är ett bra automationsalternativ för administratörer med PowerShell-erfarenhet. Kombinationen av ren syntax och ett kraftfullt skriptspråk gör det värt att överväga även om du är nybörjare på PowerShell. Den här nivån av automatisering för tidskrävande uppgifter som lätt drabbas av fel minskar den administrativa tiden och ökar kvaliteten.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

När du kör i din egen prenumeration kan du använda följande PowerShell-cmdlet för att ta bort resursgruppen (och alla relaterade resurser).

```powershell
Remove-AzureRmResourceGroup -Name MyResourceGroupName
```

När du uppmanas att bekräfta borttagningen svarar du **Ja**, eller så kan du lägga till parametern `-Force` om du vill hoppa över uppmaningen. Det här kommandot kan ta flera minuter att slutföra.