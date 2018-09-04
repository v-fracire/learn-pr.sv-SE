I den här modulen har du upptäckt hur köer i Azure-lagringskonton kan användas till att lagra meddelanden när de skickas mellan komponenter i ett distribuerat program. Att använda köer på det här sättet kan göra ett distribuerat program säkrare och motståndskraftigare mot fel och perioder med hög efterfrågan. Om du använder Microsoft Azure Storage-klientbiblioteket för .NET kan du enkelt skriva kod i C# eller Visual Basic som skapar köer, lägger till meddelanden eller hämtar och tar bort meddelanden från köer.

## <a name="clean-up-the-azure-subscription"></a>Rensa Azure-prenumerationen

När ett lagringskonto innehåller data medför det kostnader mot din Azure-prenumeration, men dessa sannolikt låga för liten kö med få meddelanden. Kom ihåg att ta bort kön för att undvika onödiga ändringar när du är klar med den. Eftersom du skapar alla resurser i samma resursgrupp är det enklast att rensa din Azure-prenumeration genom att ta bort resursgruppen. Då tas allt dess innehåll bort:

   ```powershell
   Remove-AzureRmResourceGroup -Name ArticleSubmissionResourceGroup
   ```

När du uppmanas att bekräfta borttagningen svarar du **Ja**.

Det kan ta flera minuter att köra kommandot och ta bort resurserna.
