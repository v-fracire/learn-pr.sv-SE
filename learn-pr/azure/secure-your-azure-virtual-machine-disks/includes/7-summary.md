Azure tillhandahåller Storage Service Encryption (SSE) och Azure Disk Encryption (ADE) för att skydda virtuella Azure-diskar. Dessa tekniker fungerar tillsammans för att ge starkt 256-bitars kryptering, som en del av en metod för skydd på djupet för skydd av Virtuella Azure-diskar. Det krävs att du har slutfört Azure Disk Encryption förutsättningarna för att aktivera diskkryptering. Nödvändiga konfigurationsskriptet för Azure Disk Encryption kan automatisera processen. När du aktiverar kryptering på den nya virtuella datorer, kan du använda en Azure Resource Manager-mall. Detta säkerställer att dina data är krypterad distributionspunkten lämnar inga sårbarheter.

## <a name="clean-up"></a>Rensa
<!---TODO: Update for sandbox?---> Köra virtuella Azure-datorer och den associerade lagringen medför kostnader mot din prenumeration. Ta bort de resurser du inte behöver så att du undviker onödiga kostnader. Det enklaste sättet att rensa i din Azure-prenumeration är att ta bort den associerade resursgruppen. Då tas även alla resurser i gruppen bort. När du är färdig med den här modulen kör du följande Azure PowerShell-cmdlet:

   ```powershell
   Remove-AzureRmResourceGroup -Name moneyapprg
   ```

När du uppmanas att bekräfta borttagningen besvara **Ja**. Det kan ta flera minuter att köra kommandot och ta bort resurserna. 

Om du får ett meddelande kunde inte tas bort kan behöva du ta bort lås i nyckelvalvet innan du kan ta bort resursgruppen.
