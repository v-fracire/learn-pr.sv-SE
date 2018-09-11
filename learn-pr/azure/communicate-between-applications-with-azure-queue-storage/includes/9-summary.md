I den här modulen har du sett hur köer i Azure-lagringskonton används till att lagra meddelanden mellan komponenter i ett distribuerat program. När du använder köer på det här sättet blir det distribuerade programmet säkrare och mer beständigt vid fel och i perioder med hög efterfrågan. Om du använder Microsoft Azure Storage-klientbiblioteket för .NET kan du enkelt skriva kod i C# eller VB.NET som skapar köer, lägger till meddelanden eller hämtar och tar bort meddelanden från köer.

## <a name="clean-up-the-resources"></a>Rensa resurserna

Lagringskonton som innehåller data kan medföra kostnader för din Azure-prenumeration. Datamängderna vi har använt här är små, så kostnaden skulle vara låg, men det är alltid en bra idé att ta bort alla resurser.

Eftersom du skapade resurserna i samma resursgrupp är det enklast att ta bort hela resursgruppen. Då tas allt dess innehåll bort:

```azurecli
az group delete --name ExerciseResources --yes --no-wait
```

Med alternativet `--no-wait` kan du gå vidare till nästa modul utan att vänta tills kommandot har slutförts.