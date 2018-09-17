Azure tillhandahåller kryptering för lagringstjänst (SSE) och Azure Disk Encryption (ADE) för att skydda virtuella Azure-diskar. Dessa tekniker samverkar för att ge stark 256-bitars kryptering som ett led i en djupskyddsmetod för Virtuella Azure-diskar. Du måste ha slutfört förhandskraven för Azure Disk Encryption för att kunna aktivera diskkryptering. Konfigurationsskriptet som krävs för Azure Disk Encryption kan automatisera den här processen. När du aktiverar kryptering på nya virtuella datorer kan du använda en Azure Resource Manager-mall. Detta säkerställer att dina data är krypterade vid distributionspunkten, vilket eliminerar alla sårbarheter.

## <a name="clean-up"></a>Rensa
<!---TODO: Update for sandbox?---> Drift av virtuella Azure-datorer och den tillhörande lagringen medför kostnader mot din prenumeration. Undvik onödiga kostnader genom att ta bort resurser som du inte behöver. Det enklaste sättet att städa i din Azure-prenumeration är att ta bort den associerade resursgruppen. Då tas även alla resurser i gruppen bort. När du är klar med den här modulen kör du följande Azure PowerShell-cmdlet:

   ```powershell
   Remove-AzureRmResourceGroup -Name moneyapprg
   ```

När du uppmanas att bekräfta borttagningen svarar du **Ja**. Det kan ta flera minuter att köra kommandot och ta bort resurserna. 

Om du får ett meddelande om att borttagningen misslyckats kan du behöva ta bort låsen från nyckelvalvet innan du försöker ta bort resursgruppen på nytt.
