<span data-ttu-id="37ec8-101">I den här modulen har du lärt dig om diskcachelagring i Azure och hur det kan förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="37ec8-101">In this module, you learned about Azure disk caching and how it potentially improves performance.</span></span> <span data-ttu-id="37ec8-102">Vi använde Azure-portalen och Azure PowerShell för att hantera diskcachelagring för vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="37ec8-102">We used the Azure portal and Azure PowerShell to manage disk caching for our VM.</span></span> 

<span data-ttu-id="37ec8-103">När du har en strategi för diskcachelagring i virtuella Azure-datorer kan du sedan snabbt och enkelt distribuera nya virtuella datorer och diskar med optimala inställningar för diskcachelagring med skript och mallar.</span><span class="sxs-lookup"><span data-stu-id="37ec8-103">Once you have Azure VM disk caching strategy in place, you can then quickly and easily deploy new VMs and disk with the optimum disk cache settings, by using scripts and templates.</span></span>

## <a name="further-reading"></a><span data-ttu-id="37ec8-104">Ytterligare läsning</span><span class="sxs-lookup"><span data-stu-id="37ec8-104">Further reading</span></span>

- [<span data-ttu-id="37ec8-105">Azure Premium Storage: Design för höga prestanda</span><span class="sxs-lookup"><span data-stu-id="37ec8-105">Azure Premium Storage: Design for High Performance</span></span>](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance)
- [<span data-ttu-id="37ec8-106">Kom igång med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="37ec8-106">Get started with Azure PowerShell</span></span>](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-6.8.1)
- [<span data-ttu-id="37ec8-107">Referens för cmdletar för Azure-dator</span><span class="sxs-lookup"><span data-stu-id="37ec8-107">Azure Computer Cmdlets Reference</span></span>](https://docs.microsoft.com/powershell/module/azurerm.compute/?view=azurermps-6.8.1#vm_disks)


## <a name="cleanup"></a><span data-ttu-id="37ec8-108">Rensa</span><span class="sxs-lookup"><span data-stu-id="37ec8-108">Cleanup</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="37ec8-109">När du kör virtuella Azure-datorer genereras kostnader i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37ec8-109">Running Azure VMs incurs costs against your subscription.</span></span> <span data-ttu-id="37ec8-110">Ta bort de resurser du inte behöver så att du undviker onödiga kostnader.</span><span class="sxs-lookup"><span data-stu-id="37ec8-110">Remove unneeded resources to avoid unnecessary charges.</span></span> <span data-ttu-id="37ec8-111">Det enklaste sättet att rensa i din Azure-prenumeration är att ta bort den resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="37ec8-111">The easiest way to clean up your Azure subscription is to remove the resource group.</span></span> <span data-ttu-id="37ec8-112">När du tar bort en resursgrupp tas alla resurser i den gruppen bort.</span><span class="sxs-lookup"><span data-stu-id="37ec8-112">Deleting a resource group deletes all the resources in that group.</span></span> <span data-ttu-id="37ec8-113">När du är färdig med den här modulen kör du följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="37ec8-113">When you're finished with this module, run the following Azure PowerShell cmdlet:</span></span>

    ```powershell
    Remove-AzureRmResourceGroup -Name "fotoshare-rg"
    ```

<span data-ttu-id="37ec8-114">När du uppmanas att bekräfta borttagningen svarar du **Ja**.</span><span class="sxs-lookup"><span data-stu-id="37ec8-114">When asked to confirm the delete, answer **Yes**.</span></span> <span data-ttu-id="37ec8-115">Det kan ta flera minuter att köra kommandot och ta bort resurserna.</span><span class="sxs-lookup"><span data-stu-id="37ec8-115">The command may take several minutes to complete as resources are deleted.</span></span>
