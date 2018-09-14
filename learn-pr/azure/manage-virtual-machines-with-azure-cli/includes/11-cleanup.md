<span data-ttu-id="5b9b9-101">Du har skapat en ny virtuell Linux-dator, ändrat dess storlek, stoppat och startat den och uppdaterat konfigurationen med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-101">You've created a new Linux virtual machine, changed its size, stopped and started it, and updated the configuration with the Azure CLI.</span></span>

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

<span data-ttu-id="5b9b9-102">När du arbetar i din egen prenumeration kan du köra följande Azure CLI-kommando för att ta bort resursgruppen och alla associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-102">When you are working in your own subscription, you can execute the following Azure CLI command to delete the resource group and all associated resources.</span></span>

```azurecli
az group delete --name [resource-group-name] --no-wait
```

<span data-ttu-id="5b9b9-103">Svara ”yes” på frågan när den visas, eller använd parametern `--yes` för att svara automatiskt på prompten.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-103">Answer "yes" to the prompt when shown, or use the `--yes` parameter to auto-answer the prompt.</span></span>

<span data-ttu-id="5b9b9-104">Det här kommandot tar bort alla resurser som är associerade med resursgruppen och garanterar att de frigörs i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-104">This command deletes all of the resources associated with the resource group, and is guaranteed to deallocate them in the correct order.</span></span> <span data-ttu-id="5b9b9-105">Parametern `--no-wait` ser till att Azure CLI inte blockerar under tiden som borttagningen utförs.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-105">The `--no-wait` parameter keeps the Azure CLI from blocking while the deletion takes place.</span></span> <span data-ttu-id="5b9b9-106">Lämna den inaktiverad för att vänta tills resurserna har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-106">Leave it off to wait for the resources to be deleted.</span></span> <span data-ttu-id="5b9b9-107">Du kan också använda kommandot `az group wait -n [resource-group-name] --deleted` senare i skriptet för att vänta tills borttagningen är klar.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-107">Or use the `az group wait -n [resource-group-name] --deleted` command later in your script to wait for the deletion to finish.</span></span>


## <a name="further-reading"></a><span data-ttu-id="5b9b9-108">Ytterligare läsning</span><span class="sxs-lookup"><span data-stu-id="5b9b9-108">Further reading</span></span>

- [<span data-ttu-id="5b9b9-109">Översikt över Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5b9b9-109">Azure CLI overview</span></span>](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [<span data-ttu-id="5b9b9-110">Azure CLI-kommandoreferens</span><span class="sxs-lookup"><span data-stu-id="5b9b9-110">Azure CLI command reference</span></span>](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
