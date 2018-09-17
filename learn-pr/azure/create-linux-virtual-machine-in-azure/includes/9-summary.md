<span data-ttu-id="63729-101">I den här modulen har du lärt dig hur du skapar en virtuell Linux-dator med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="63729-101">In this module, you learned how to create a Linux VM using the Azure portal.</span></span> <span data-ttu-id="63729-102">Därefter anslöt du till den offentliga IP-adressen för den virtuella datorn och hanterade den med en SSH-anslutning.</span><span class="sxs-lookup"><span data-stu-id="63729-102">You then connected to the public IP address of the VM and managed it with an SSH connection.</span></span> 

<span data-ttu-id="63729-103">Du lärde dig också att du kan interagera med operativsystemet och programvara på den virtuella datorn med hjälp av SSH, och att du kan konfigurera virtuell maskinvara och anslutningar via portalen.</span><span class="sxs-lookup"><span data-stu-id="63729-103">You learned that while SSH allows us to interact with the operating system and software of the virtual machine, the portal will enable us to configure the virtual hardware and connectivity.</span></span> <span data-ttu-id="63729-104">Vi hade även kunnat använda PowerShell eller Azure CLI om vi hade föredragit att använda en kommandorads- eller skriptmiljö.</span><span class="sxs-lookup"><span data-stu-id="63729-104">We also could have used PowerShell or the Azure CLI, if a command-line or scriptable environment were preferred.</span></span>

## <a name="clean-up"></a><span data-ttu-id="63729-105">Rensa</span><span class="sxs-lookup"><span data-stu-id="63729-105">Clean up</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="63729-106">Du debiteras för virtuella datorer när de körs och för lagring utifrån din faktiska användning.</span><span class="sxs-lookup"><span data-stu-id="63729-106">You are charged for VMs while they run and for the storage based on how much you use.</span></span> <span data-ttu-id="63729-107">Du bör alltid stoppa och frigöra virtuella datorer när du inte använder dem, och när du inte längre behöver resurser är det en bra idé att ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="63729-107">Always stop and deallocate VMs when you aren't using them, and when you no longer need the resources, it's a good idea to delete them.</span></span> <span data-ttu-id="63729-108">Om du vill ta bort alla resurser som du har skapat kan du ta bort dem en i taget eller helt enkelt ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="63729-108">To remove all the resources that you created, you can delete them one by one or delete the resource group:</span></span>

1. <span data-ttu-id="63729-109">Logga in på Microsoft Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="63729-109">Sign in to the Azure portal.</span></span>

1. <span data-ttu-id="63729-110">Välj **Alla tjänster** på menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="63729-110">On the left menu, select **All Services**.</span></span>

1. <span data-ttu-id="63729-111">Välj **Resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="63729-111">Select **Resource Groups**.</span></span>

1. <span data-ttu-id="63729-112">Hitta resursgruppen som du skapade i den första övningen.</span><span class="sxs-lookup"><span data-stu-id="63729-112">Find the resource group that you created in the first exercise.</span></span> <span data-ttu-id="63729-113">Klicka på ellipsen (...) till höger i listvyn.</span><span class="sxs-lookup"><span data-stu-id="63729-113">Click the ellipsis (...) on the right side of the list view.</span></span>

1. <span data-ttu-id="63729-114">Välj **Ta bort resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="63729-114">Select **Delete resource group**.</span></span>

1. <span data-ttu-id="63729-115">På nästa skärm anger du resursgruppens namn för att bekräfta borttagningen.</span><span class="sxs-lookup"><span data-stu-id="63729-115">On the next screen, enter the resource group name to confirm the deletion.</span></span>

1. <span data-ttu-id="63729-116">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="63729-116">Click **Delete**.</span></span>
