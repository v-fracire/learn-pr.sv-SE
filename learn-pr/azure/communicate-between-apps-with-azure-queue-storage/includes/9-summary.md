I den här modulen har du sett hur köer i Azure-lagringskonton används till att skicka meddelanden mellan komponenter i ett distribuerat program. Att använda köer på det här sättet kan göra ett distribuerat program säkrare och motståndskraftigare mot fel och perioder med hög efterfrågan. Om du använder Microsoft Azure Storage-klientbiblioteket för .NET kan du enkelt skriva kod i C# eller VB.NET som skapar köer, lägger till meddelanden, eller hämtar och tar bort meddelanden från köer.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

När du arbetar i din egen prenumeration kan du köra följande Azure CLI-kommando för att ta bort resursgruppen och alla associerade resurser.

```azurecli
az group delete --name [resource-group-name] --yes --no-wait
```

Det valfria `--no-wait`-alternativet talar om för gränssnittet att inte vänta på att Azure slutför åtgärden.