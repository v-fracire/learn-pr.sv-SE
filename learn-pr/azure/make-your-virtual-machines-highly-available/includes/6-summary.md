Här har du lärt dig att använda en lastbalanserare som en del av en lösning för att leverera hög tillgänglighet för virtuella datorer i Azure. Det ingår i själva lastbalanseraren, de associerade virtuella nätverken, regler för att kontrollera balansalgoritmen och hälsotillståndsavsökningar som identifierar vilka virtuella datorer som körs korrekt.

## <a name="clean-up"></a>Rensa
<!---TODO: Update for sandbox?--->

Körning av virtuella datorer och skalningsuppsättningar medför kostnader för din prenumeration. Du bör ta bort de resurser du inte behöver så att du undviker onödiga kostnader. Det enklaste sättet att rensa i din Azure-prenumeration är att ta bort den associerade resursgruppen. Då tas även alla resurser i gruppen bort. När du är färdig med den här modulen kör du följande Azure PowerShell-cmdlet:

```powershell
Remove-AzureRmResourceGroup -Name woodgrove-RG
```

När du uppmanas att bekräfta borttagningen svarar du **Ja**. Det kan ta flera minuter att köra kommandot och ta bort resurserna.