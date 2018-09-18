<span data-ttu-id="ec464-101">I den här modulen har du sett hur köer i Azure-lagringskonton används till att skicka meddelanden mellan komponenter i ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="ec464-101">In this module, you've seen how queues in Azure storage accounts are used to pass messages between components in a distributed application.</span></span> <span data-ttu-id="ec464-102">Att använda köer på det här sättet kan göra ett distribuerat program säkrare och motståndskraftigare mot fel och perioder med hög efterfrågan.</span><span class="sxs-lookup"><span data-stu-id="ec464-102">Using queues in this way can help to make a distributed application more reliable and resilient to failures and periods of high demand.</span></span> <span data-ttu-id="ec464-103">Om du använder Microsoft Azure Storage-klientbiblioteket för .NET kan du enkelt skriva kod i C# eller VB.NET som skapar köer, lägger till meddelanden, eller hämtar och tar bort meddelanden från köer.</span><span class="sxs-lookup"><span data-stu-id="ec464-103">If you use the Microsoft Azure Storage Client Library for .NET, you can easily write C# or VB.NET code that creates queues, adds messages, or retrieves and removes messages from queues.</span></span>

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

<span data-ttu-id="ec464-104">När du arbetar i din egen prenumeration kan du köra följande Azure CLI-kommando för att ta bort resursgruppen och alla associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="ec464-104">When you are working in your own subscription, you can execute the following Azure CLI command to delete the resource group and all associated resources.</span></span>

```azurecli
az group delete --name [resource-group-name] --yes --no-wait
```

<span data-ttu-id="ec464-105">Det valfria `--no-wait`-alternativet talar om för gränssnittet att inte vänta på att Azure slutför åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ec464-105">The optional `--no-wait` option tells the shell to not wait for Azure to complete the operation.</span></span>