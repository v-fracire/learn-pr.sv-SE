<span data-ttu-id="74d74-101">Ni ska titta på en annan typ av situation.</span><span class="sxs-lookup"><span data-stu-id="74d74-101">Let's consider an alternate scenario.</span></span> <span data-ttu-id="74d74-102">Din organisation kanske är en skola som använder virtuella Windows-datorer till att starta testmiljöer där studenterna kan installera webbappar, konfigurera domäner och utforska tjänster och funktioner i Windows utan att skolans datorer påverkas.</span><span class="sxs-lookup"><span data-stu-id="74d74-102">Perhaps your organization is a school that uses Windows virtual machines to spin up test environments on which students install web apps, configure domains, and explore Windows services and features without affecting the school's computers.</span></span> <span data-ttu-id="74d74-103">Lärarna kan sedan ansluta till de här virtuella datorerna via RDP när de ska kontrollera hur det går för studenterna.</span><span class="sxs-lookup"><span data-stu-id="74d74-103">Teachers can then connect to these VMs by using RDP and check on student progress.</span></span> <span data-ttu-id="74d74-104">Vi ska utforska hur du kan skapa något som liknar detta med Azure.</span><span class="sxs-lookup"><span data-stu-id="74d74-104">Let's explore how you might build something like this with Azure.</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="74d74-105">Skapa en virtuell Windows-dator</span><span class="sxs-lookup"><span data-stu-id="74d74-105">Create a Windows VM</span></span>

<span data-ttu-id="74d74-106">Så här skapar du en virtuell Windows-dator:</span><span class="sxs-lookup"><span data-stu-id="74d74-106">To create a Windows VM, complete the following steps:</span></span>

1. <span data-ttu-id="74d74-107">Logga in på Azure via [Azure Portal](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="74d74-107">Log onto Azure through the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="74d74-108">Klicka på **Skapa en resurs** uppe till vänster i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="74d74-108">Click **Create a resource** in the upper left corner of the Azure portal.</span></span>

1. <span data-ttu-id="74d74-109">I **sökfältet** anger du **Windows Server 2016 Datacenter** och klickar på länken med samma namn.</span><span class="sxs-lookup"><span data-stu-id="74d74-109">In the **search bar**, enter  **Windows Server 2016 Datacenter**  and then click on the link with the same title.</span></span>

### <a name="configure-the-vm-settings"></a><span data-ttu-id="74d74-110">Konfigurera inställningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="74d74-110">Configure the VM settings</span></span>

1. <span data-ttu-id="74d74-111">Under **Grundläggande inställningar**, i fältet **Namn**, anger du ett namn på den virtuella datorn, till exempel ”StudentVM”.</span><span class="sxs-lookup"><span data-stu-id="74d74-111">Under **Basics**, in the **Name** field, enter a name for your VM, such as "StudentVM."</span></span>

1. <span data-ttu-id="74d74-112">I fältet **Typ av virtuell datordisk** klickar du på listrutan så att du ser alternativen.</span><span class="sxs-lookup"><span data-stu-id="74d74-112">In the **VM Disk Type** field, click the drop-down menu to see the options.</span></span> <span data-ttu-id="74d74-113">Se till att **SSD** är valt.</span><span class="sxs-lookup"><span data-stu-id="74d74-113">Ensure that **SSD** is selected.</span></span>

1. <span data-ttu-id="74d74-114">I fältet **Användarnamn** anger du ett lämpligt användarnamn för inloggning på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="74d74-114">In the **Username** field, enter a suitable user name to use to sign in to the VM.</span></span>

1. <span data-ttu-id="74d74-115">I fältet **Lösenord** anger du ett lösenord som är minst 12 tecken långt.</span><span class="sxs-lookup"><span data-stu-id="74d74-115">In the **Password** field, enter a password that's at least 12 characters long.</span></span> <span data-ttu-id="74d74-116">Det måste dessutom innehålla versaler och gemener, siffror och specialtecken.</span><span class="sxs-lookup"><span data-stu-id="74d74-116">It must also have upper and lowercase characters, numbers, and special characters.</span></span>

1. <span data-ttu-id="74d74-117">Under **Resursgrupp** klickar du på **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="74d74-117">Under **Resource group**, click **Create new**.</span></span> <span data-ttu-id="74d74-118">Ge resursgruppen ett namn, till exempel ”myTestRG”.</span><span class="sxs-lookup"><span data-stu-id="74d74-118">Give the resource group a name, such as "myTestRG."</span></span>

1. <span data-ttu-id="74d74-119">Välj en lämplig plats för den virtuella datorn och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="74d74-119">Select a suitable location for the VM to be created, and then click **OK**.</span></span>

### <a name="select-the-vm-image-size-and-options"></a><span data-ttu-id="74d74-120">Välj storlek och alternativ för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="74d74-120">Select the VM image size and options</span></span>

1. <span data-ttu-id="74d74-121">På sidan **Välj en storlek** klickar du på avbildningen **B1s** och sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="74d74-121">In the **Choose a size** page, click the **B1s** image, and then click **Select**.</span></span>

   > [!Note] 
   > <span data-ttu-id="74d74-122">B1-avbildningen har endast 1 GB RAM och genererar minnesfel när du först loggar in.</span><span class="sxs-lookup"><span data-stu-id="74d74-122">The B1 image has only 1 GB of RAM and will result in memory errors when you first sign in.</span></span> <span data-ttu-id="74d74-123">Du kommer dock ändra storlek på avbildningen i en senare övning.</span><span class="sxs-lookup"><span data-stu-id="74d74-123">However, you can resize the image later as part of a later lab.</span></span>

1. <span data-ttu-id="74d74-124">På sidan **Inställningar**, under **Välj offentliga inkommande portar**, väljer du **Inga offentliga inkommande portar**.</span><span class="sxs-lookup"><span data-stu-id="74d74-124">In the **Settings** page, under **Select Public Inbound Ports**, select **No public inbound ports**.</span></span> <span data-ttu-id="74d74-125">Vi konfigurerar RDP-åtkomsten senare.</span><span class="sxs-lookup"><span data-stu-id="74d74-125">We'll configure RDP access later.</span></span>

### <a name="finish-configuring-the-vm-and-create-the-image"></a><span data-ttu-id="74d74-126">Slutför konfigurationen av den virtuella datorn och skapa avbildningen</span><span class="sxs-lookup"><span data-stu-id="74d74-126">Finish configuring the VM and create the image</span></span>

1. <span data-ttu-id="74d74-127">Bläddra längst ned på sidan och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="74d74-127">Scroll to the bottom and click **OK**.</span></span>

1. <span data-ttu-id="74d74-128">Kontrollera dina inställningar under **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="74d74-128">Under **Create**, check the settings that you configured.</span></span> <span data-ttu-id="74d74-129">Klicka på **Skapa** längst ned.</span><span class="sxs-lookup"><span data-stu-id="74d74-129">At the bottom, click **Create**.</span></span> <span data-ttu-id="74d74-130">På instrumentpanelen i Azure ser du den virtuella dator som distribueras.</span><span class="sxs-lookup"><span data-stu-id="74d74-130">The Azure dashboard will show the VM that's being deployed.</span></span> <span data-ttu-id="74d74-131">Det här kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="74d74-131">This may take several minutes.</span></span>

## <a name="summary"></a><span data-ttu-id="74d74-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="74d74-132">Summary</span></span>

<span data-ttu-id="74d74-133">Nu har du skapat en virtuell Windows Server-dator som passar för studenternas behov och som kan nås från skolans nätverk.</span><span class="sxs-lookup"><span data-stu-id="74d74-133">You've now created a Windows Server virtual machine that's suitable for student requirements and accessible from the school network.</span></span> <span data-ttu-id="74d74-134">I nästa övning får du lära dig att ansluta till och hantera den virtuella datorn via RDP.</span><span class="sxs-lookup"><span data-stu-id="74d74-134">In the next unit, you will look at how you connect to and manage that VM by using RDP.</span></span>
