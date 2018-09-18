Du har sett två sätt att möta efterfrågan med hjälp av virtuella datorer. Om din belastning är förutsägbar kan du använda portalen eller skript för att manuellt ändra storlek på dina virtuella datorer. För oförutsägbara efterfrågemönster är en bättre metod att använda skalningsuppsättningar med autoskalning för att automatiskt lägga till och ta bort instanser allteftersom efterfrågan ändras. Flera virtuella datorer har den ytterligare fördelen att de ökar tillgängligheten för systemet eftersom tjänsten inte störs om en virtuell dator slutar fungera.

## <a name="cleanup"></a>Rensa

När en virtuell dator etableras och körs medför den kostnader i din prenumeration. Eftersom du skapade alla virtuella datorer i samma resursgrupp är det enklast att rensa din Azure-prenumeration genom att ta bort resursgruppen. Då tas allt dess innehåll bort. Kör följande PowerShell-cmdlet när du är klar med övningen:

   ```powershell
   Remove-AzureRmResourceGroup -Name ExerciseRG
   ```

När du uppmanas att bekräfta borttagningen svarar du **Ja**. Det kan ta flera minuter att köra kommandot och ta bort resurserna.