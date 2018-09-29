<span data-ttu-id="0ee5a-101">I den här enheten ska du använda en Azure Resource Manager-mall för att dekryptera den virtuella Windows-dator som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-101">In this unit, you'll use an Azure Resource Manager template to decrypt our Windows VM we created earlier.</span></span> <span data-ttu-id="0ee5a-102">Vi krypterade operativsystemet på den virtuella Windows-datorn.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-102">We encrypted the OS drive on our Windows VM.</span></span> <span data-ttu-id="0ee5a-103">Men OS-enheten skulle inte ha någon konfidentiell information, så vi kunde lämna den okrypterad.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-103">However, the OS drive won't have any confidential information on it, so we could leave it unencrypted.</span></span> <span data-ttu-id="0ee5a-104">Vi ska använda en mall för att dekryptera operativsystemenheten.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-104">Let's use a template to decrypt the OS drive.</span></span>

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a><span data-ttu-id="0ee5a-105">Konfigurera och distribuera en virtuell dator med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="0ee5a-105">Configure and deploy a new VM using an Azure Resource Manager template</span></span>

<span data-ttu-id="0ee5a-106">Vi ska använda en mall som Microsoft har publicerat på Github för att skapa en ny virtuell dator där kryptering är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-106">We're going to use a template Microsoft has published on Github to create a new VM with encryption turned on by default.</span></span>

1. <span data-ttu-id="0ee5a-107">Logga in på [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du aktiverade sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-107">Sign into the [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) with the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="0ee5a-108">Klick på **Skapa en resurs** i det vänstra sidofältet.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-108">Click **Create a Resource** in the left sidebar.</span></span>

1. <span data-ttu-id="0ee5a-109">Skriv **Mall** i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-109">Type **Template** in the search box.</span></span>

1. <span data-ttu-id="0ee5a-110">Välj **Malldistribution** i listan som visas och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-110">Select **Template Deployment** from the resulting list and click **Create**.</span></span>

    ![Skärmbild som visar objektet Malldistribution valt med knappen Skapa markerad](../media/6-create-template.png)

1. <span data-ttu-id="0ee5a-112">I sökrutan **Välj en mall** börjar du skriva ”201-decrypt” och välj mallen ”201-decrypt-running-windows-vm-without-aad”.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-112">In the **Select a template** search box, start typing "201-decrypt" and select the "201-decrypt-running-windows-vm-without-aad" template.</span></span>

    ![Skärmvild som visar sökrutan Välj en mall med automatisk komplettering](../media/6-custom-deployment.png)

1. <span data-ttu-id="0ee5a-114">Klicka på **Välj mall** för att starta template runner.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-114">Click **Select Template** to launch the template runner.</span></span>

1. <span data-ttu-id="0ee5a-115">Ange följande information i inställningsvyn:</span><span class="sxs-lookup"><span data-stu-id="0ee5a-115">In the settings view, enter the following information:</span></span>
    - <span data-ttu-id="0ee5a-116">Välj _Concierge-prenumeration_ i **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-116">Select _Concierge Subscription_ for the **Subscription**.</span></span>
    - <span data-ttu-id="0ee5a-117">Välj din resursgrupp som skapats i sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-117">Select your Resource Group, created in the sandbox.</span></span>
    - <span data-ttu-id="0ee5a-118">Välj den plats där du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-118">Select the location you created the VM in.</span></span>
    - <span data-ttu-id="0ee5a-119">Ange ”fmdata vm01” för **namnet på den virtuell datorn**.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-119">Enter "fmdata-vm01" for the **VM Name**.</span></span>
    - <span data-ttu-id="0ee5a-120">Lämna **Volymtypen** som _Alla_.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-120">Leave the **Volume Type** as _All_.</span></span>

1. <span data-ttu-id="0ee5a-121">Markera kryssrutan **Jag godkänner villkoren**.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-121">Select the **I agree to the terms and conditions** check box.</span></span>
1. <span data-ttu-id="0ee5a-122">Klicka på **Köp** för att köra mallen.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-122">Click **Purchase** to run the template.</span></span> <span data-ttu-id="0ee5a-123">Obs! Det här kostar ingenting – det är en standardknapp.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-123">Note that there is no cost to this - it's a standard button.</span></span>

<span data-ttu-id="0ee5a-124">Det kan ta några minuter att slutföra distributionen.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-124">The deployment may take a few minutes to complete.</span></span>

## <a name="verify-the-encryption-status-of-the-vm"></a><span data-ttu-id="0ee5a-125">Kontrollera krypteringsstatusen för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="0ee5a-125">Verify the encryption status of the VM</span></span>

1. <span data-ttu-id="0ee5a-126">Klicka på **Virtuella datorer** på sidopanelen i Azure-portalen och välj din virtuella dator **fmdata-vm01**.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-126">In the sidebar of the Azure portal, click **Virtual machines** and select your VM **fmdata-vm01**.</span></span> <span data-ttu-id="0ee5a-127">Alternativt kan du söka efter den virtuella datorn med namnet från **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-127">Alternatively, you can search for your VM by name from **All Resources**.</span></span>

1. <span data-ttu-id="0ee5a-128">På bladet **Virtuella datorer**, under **INSTÄLLNINGAR**, klickar du på **Diskar**.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-128">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="0ee5a-129">På bladet **Diskar** ser du att krypteringsstatusen för OS-disken nu är **Aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="0ee5a-129">On the **Disks** blade, notice the OS disk encryption status is now **Disabled**.</span></span>
