I den här modulen har du sett hur köer i Azure-lagringskonton används till att lagra meddelanden mellan komponenter i ett distribuerat program. När du använder köer på det här sättet blir det distribuerade programmet säkrare och mer beständigt vid fel och i perioder med hög efterfrågan. Om du använder Microsoft Azure Storage-klientbiblioteket för .NET kan du enkelt skriva kod i C# eller VB.NET som skapar köer, lägger till meddelanden eller hämtar och tar bort meddelanden från köer.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

När du arbetar i din egen prenumeration kan du köra följande Azure CLI-kommando för att ta bort resursgruppen och alla associerade resurser.

```azurecli
az group delete --name [resource-group-name] --yes --no-wait
```

Den valfria `--no-wait` alternativet anger shell inte vänta på Azure för att slutföra åtgärden.