Azure Event Hubs ger stordataprogram möjlighet att bearbeta stora mängder data. Det har även kapacitet att skala ut under perioder av exceptionellt hög efterfrågan eller när annars det behövs. Azure Event Hubs frikopplar skickande och mottagande av meddelanden för att hantera databearbetningen. På så sätt kan du eliminera risken för överbelastning av konsumentprogram och dataförlust på grund av oplanerade avbrott.

I den här modulen har du sett hur du distribuerar Azure Event Hubs som en del av en lösning för händelsebearbetning. Du har lärt dig att:

- Använda Azure CLI-kommandon för att skapa Event Hubs-namnrymder och en händelsehubb i namnrymden. 
- Konfigurera avsändar- och mottagarprogram för att skicka och ta emot meddelanden via händelsehubben.
- Använd Azure-portalen för att visa status och prestanda för din händelsehubb.

## <a name="clean-up"></a>Rensa 
<!---TODO: Do we need to include cleanup for the free education tier?--->

De resurser som du har använt för att testa din händelsehubb ökar kostnaderna för din prenumeration. Kom ihåg att ta bort de här resurserna när du är färdig med händelsehubben för att undvika onödiga kostnader.

Eftersom du skapar hubb, namnrymd och lagring i samma resursgrupp, är det lättast att rensa upp din Azure-prenumeration genom att ta bort resursgruppen. Då tas allt dess innehåll bort. 

Kör följande kommando för att ta bort resursgruppen, namnrymden, lagringskontot och alla relaterade resurser. Ersätt `myResourceGroup` med namnet på den resursgrupp du skapade:

```azurecli
az group delete --resource-group myResourceGroup
```

När du uppmanas att bekräfta borttagningen svarar du **Ja**.

Det kan ta flera minuter att köra kommandot och ta bort resurserna.
