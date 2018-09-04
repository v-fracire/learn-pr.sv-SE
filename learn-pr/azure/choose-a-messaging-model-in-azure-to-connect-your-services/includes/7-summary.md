I den här modulen har du utforskat fyra olika Azure-tjänster som gör det möjligt att skapa pålitliga och motståndskraftiga distribuerade program. Valet mellan dem handlar om att bestämma vilken typ av data som behöver överföras mellan komponenter (meddelanden eller händelser) och sedan vilka funktioner du behöver leverera och bearbeta data.

## <a name="clean-up-the-azure-subscription"></a>Rensa Azure-prenumerationen

När ett lagringskonto innehåller data medför det kostnader mot din Azure-prenumeration, men dessa sannolikt låga för liten kö med få meddelanden. Kom ihåg att ta bort kön för att undvika onödiga ändringar när du är klar med den. Eftersom du skapar alla resurser i samma resursgrupp är det enklast att rensa din Azure-prenumeration genom att ta bort resursgruppen. Då tas allt dess innehåll bort:

```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```

När du uppmanas att bekräfta borttagningen svarar du **Ja**.

Det kan ta flera minuter att köra kommandot och ta bort resurserna.
