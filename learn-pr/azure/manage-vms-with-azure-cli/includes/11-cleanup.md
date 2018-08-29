<span data-ttu-id="e081e-101">Du har skapat en ny virtuell Linux-dator, ändrat dess storlek, stoppat och startat den och uppdaterat konfigurationen med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e081e-101">You've created a new Linux virtual machine, changed its size, stopped and started it, and updated the configuration with the Azure CLI.</span></span>

## <a name="cleanup"></a><span data-ttu-id="e081e-102">Rensa</span><span class="sxs-lookup"><span data-stu-id="e081e-102">Cleanup</span></span>

<span data-ttu-id="e081e-103">Nu när vi är klara med den virtuella datorn och inte längre behöver den ska vi ta bort resurserna.</span><span class="sxs-lookup"><span data-stu-id="e081e-103">Now that we're finished with the VM and no longer need it, let's delete the resources.</span></span> <span data-ttu-id="e081e-104">Borttagningen är viktig för beräknings- och lagringsresurser som kan fortsätta att debiteras mot ditt konto.</span><span class="sxs-lookup"><span data-stu-id="e081e-104">Cleanup is important for compute and storage resources that can continue to be billed against your account.</span></span> 

<span data-ttu-id="e081e-105">Du kan ta bort enskilda resurser med kommandot `delete`, men det enklaste sättet att ta bort allt är med `az group delete`.</span><span class="sxs-lookup"><span data-stu-id="e081e-105">You can delete individual resources with the `delete` command, but the easiest way to remove everything is with `az group delete`.</span></span> <span data-ttu-id="e081e-106">Kör följande kommando i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="e081e-106">Execute the following command in the Azure CLI:</span></span>

```azurecli
az group delete --name ExerciseResources --no-wait
```

<span data-ttu-id="e081e-107">Svara ”yes” på frågan när den visas, eller använd parametern `--yes` för att svara automatiskt på prompten.</span><span class="sxs-lookup"><span data-stu-id="e081e-107">Answer "yes" to the prompt when shown, or use the `--yes` parameter to auto-answer the prompt.</span></span>

<span data-ttu-id="e081e-108">Det här kommandot tar bort alla resurser som är associerade med resursgruppen och garanterar att de frigörs i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="e081e-108">This command deletes all of the resources associated with the resource group, and is guaranteed to deallocate them in the correct order.</span></span> <span data-ttu-id="e081e-109">Parametern `--no-wait` ser till att Azure CLI inte blockerar under tiden som borttagningen utförs.</span><span class="sxs-lookup"><span data-stu-id="e081e-109">The `--no-wait` parameter keeps the Azure CLI from blocking while the deletion takes place.</span></span> <span data-ttu-id="e081e-110">Lämna den inaktiverad för att vänta tills resurserna har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="e081e-110">Leave it off to wait for the resources to be deleted.</span></span> <span data-ttu-id="e081e-111">Du kan också använda kommandot `az group wait -n ExerciseResources --deleted` senare i skriptet för att vänta tills borttagningen är klar.</span><span class="sxs-lookup"><span data-stu-id="e081e-111">Or use the `az group wait -n ExerciseResources --deleted` command later in your script to wait for the deletion to finish.</span></span>


## <a name="further-reading"></a><span data-ttu-id="e081e-112">Ytterligare läsning</span><span class="sxs-lookup"><span data-stu-id="e081e-112">Further reading</span></span>

* [<span data-ttu-id="e081e-113">Översikt över Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e081e-113">Azure CLI overview</span></span>](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
* [<span data-ttu-id="e081e-114">Azure CLI-kommandoreferens</span><span class="sxs-lookup"><span data-stu-id="e081e-114">Azure CLI command reference</span></span>](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
