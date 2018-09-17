I den här modulen skrev vi ett skript som automatiserar skapandet av flera virtuella datorer. Även om skriptet var relativt kort märker du vilken potentiell kraft det har när du kombinerar loopar, variabler och funktioner från PowerShell med cmdletar från Azure PowerShell.

Azure PowerShell är ett bra automationsalternativ för administratörer med PowerShell-erfarenhet. Kombinationen av ren syntax och ett kraftfullt skriptspråk gör det värt att överväga även om du är nybörjare på PowerShell. Den här nivån av automatisering för tidskrävande uppgifter som lätt drabbas av fel minskar den administrativa tiden och ökar kvaliteten.

## <a name="clean-up"></a>Rensa
<!---TODO: Update for sandbox?--->

Etablerade och virtuella datorer som körs medför kostnader i din prenumeration. Du bör ta bort virtuella datorer som inte används för att undvika onödiga kostnader. Det enklaste sättet att rensa upp din Azure-prenumeration på är att ta bort den associerade resursgruppen. Detta tar även bort alla virtuella datorer i gruppen. Och du kan göra detta från PowerShell! När du är klar kör du följande Azure PowerShell-cmdlet:

```powershell
Remove-AzureRmResourceGroup -Name TrialsResourceGroup
```

När du uppmanas att bekräfta borttagningen svarar du **Ja**. Det här kommandot kan ta flera minuter att slutföra.