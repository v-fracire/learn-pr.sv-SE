Azure tillhandahåller Storage Service Encryption (SSE) och Azure Disk Encryption (ADE) för att skydda virtuella Azure-diskar. Dessa tekniker fungerar tillsammans för att ge stark 256-bitars kryptering, som en del av en djupskyddsmetod av Virtuella Azure-diskar. Du måste ha slutfört förhandskraven för Azure Disk Encryption för att aktivera diskkryptering. Konfigurationsskriptet som krävs för Azure Disk Encryption kan automatisera den här processen. När du aktiverar kryptering på nya virtuella datorer kan du använda en ARM-mall så att dina data krypteras vid distributionspunkten och inte lämnar sårbarheter.

## <a name="cleanup"></a>Rensa
<!---TODO: Do we need to include cleanup for the free education tier?--->

Virtuella Azure-datorer och den associerade lagringen medför kostnader mot din prenumeration. Ta bort de resurser du inte behöver så att du undviker onödiga kostnader. Det enklaste sättet att rensa i din Azure-prenumeration är att ta bort den associerade resursgruppen. Då tas även alla resurser i gruppen bort. När du är färdig med den här modulen kör du följande Azure PowerShell-cmdlet:

   ```powershell
   Remove-AzureRmResourceGroup -Name moneyapprg
   ```

När du uppmanas att bekräfta borttagningen svarar du **Ja**. Det kan ta flera minuter att köra kommandot och ta bort resurserna. 

Om du får ett meddelande om att borttagningen misslyckades kan du behöva ta bort låsen från nyckelvalvet innan du kan ta bort resursgruppen.
