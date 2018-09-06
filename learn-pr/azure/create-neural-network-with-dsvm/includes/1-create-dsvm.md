### <a name="exercise-1-create-an-ubuntu-data-science-vm"></a><span data-ttu-id="7d604-101">Övning 1: Skapa en Ubuntu Data Science VM</span><span class="sxs-lookup"><span data-stu-id="7d604-101">Exercise 1: Create an Ubuntu Data Science VM</span></span>

<span data-ttu-id="7d604-102">Data Science Virtual Machine för Linux är en VM-avbildning som gör det lättare att komma igång med datavetenskap.</span><span class="sxs-lookup"><span data-stu-id="7d604-102">The Data Science Virtual Machine for Linux is a virtual-machine image that simplifies getting started with data science.</span></span> <span data-ttu-id="7d604-103">Många verktyg har redan skapats, installerats och konfigurerats av oss för att du ska kunna komma igång så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="7d604-103">Multiple tools are already built, installed, and configured in order to get you up and running quickly.</span></span> <span data-ttu-id="7d604-104">NVIDIA GPU-drivrutinen, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads) och [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn)-biblioteket (cuDNN) ingår, liksom [Jupyter](http://jupyter.org/), flera exempel på Jupyter Notebook och [TensorFlow](https://www.tensorflow.org/).</span><span class="sxs-lookup"><span data-stu-id="7d604-104">The NVIDIA GPU driver, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads), and [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn) (cuDNN) library are also included, as are [Jupyter](http://jupyter.org/), several sample Jupyter notebooks, and [TensorFlow](https://www.tensorflow.org/).</span></span> <span data-ttu-id="7d604-105">Alla förinstallerade ramverk är GPU-kompatibla, men fungerar även bra tillsammans med vanliga processorer.</span><span class="sxs-lookup"><span data-stu-id="7d604-105">All pre-installed frameworks are GPU-enabled but work on CPUs as well.</span></span> <span data-ttu-id="7d604-106">I den här övningen ska du skapa en instans av Data Science Virtual Machine för Linux på Azure.</span><span class="sxs-lookup"><span data-stu-id="7d604-106">In this exercise, you will create an instance of the Data Science Virtual Machine for Linux on Azure.</span></span>

1. <span data-ttu-id="7d604-107">Öppna [Azure-portalen](https://portal.azure.com) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="7d604-107">Open the [Azure Portal](https://portal.azure.com) in your browser.</span></span> <span data-ttu-id="7d604-108">Om du ombeds logga in ska du göra det med autentiseringsuppgifterna för ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="7d604-108">If asked to log in, do so using your Microsoft account.</span></span>

1. <span data-ttu-id="7d604-109">Klicka på **+ Skapa en resurs** i menyn på vänster sida av portalen och skriv sedan ”data science” (utan citattecken) i sökfältet.</span><span class="sxs-lookup"><span data-stu-id="7d604-109">Click **+ Create a resource** in the menu on the left side of the portal, and then type "data science" (without quotation marks) into the search box.</span></span> <span data-ttu-id="7d604-110">Välj **Data Science Virtual Machine for Linux (Ubuntu)** i resultatlistan.</span><span class="sxs-lookup"><span data-stu-id="7d604-110">Select **Data Science Virtual Machine for Linux (Ubuntu)** from the results list.</span></span>

    ![Hitta en Ubuntu Data Science VM](../images/new-data-science-vm.png)

    <span data-ttu-id="7d604-112">_Hitta en Ubuntu Data Science VM_</span><span class="sxs-lookup"><span data-stu-id="7d604-112">_Finding the Ubuntu Data Science VM_</span></span>

1. <span data-ttu-id="7d604-113">Titta igenom listan med verktyg som medföljer den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7d604-113">Take a moment to review the list of tools included in the VM.</span></span> <span data-ttu-id="7d604-114">Klicka sedan på **Skapa** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="7d604-114">Then click **Create** at the bottom of the blade.</span></span>

1. <span data-ttu-id="7d604-115">Fyll i bladet med grundinställningar enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="7d604-115">Fill in the "Basics" blade as shown below.</span></span> <span data-ttu-id="7d604-116">Ange ett lösenord som består av minst 12 tecken med en blandning av versaler, gemener, siffror och specialtecken.</span><span class="sxs-lookup"><span data-stu-id="7d604-116">Provide a password that's at least 12 characters long containing a mix of uppercase letters, lowercase letters, numbers and special characters.</span></span> <span data-ttu-id="7d604-117">*Var noga med att komma ihåg det användarnamn och det lösenord som du anger, eftersom du behöver dem senare i labbuppgiften.*</span><span class="sxs-lookup"><span data-stu-id="7d604-117">*Be sure to remember the user name and password that you enter, because you will need them later in the lab.*</span></span>

    ![Ange grundläggande information om den virtuella datorn](../images/create-data-science-vm-1.png)

    <span data-ttu-id="7d604-119">_Ange grundinställningar_</span><span class="sxs-lookup"><span data-stu-id="7d604-119">_Entering basic settings_</span></span>

1. <span data-ttu-id="7d604-120">På bladet Choose a size (Välj en storlek) väljer du **DS1_V2 Standard**, som ger dig möjlighet att experimentera med virtuella datorer för datavetenskap till en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="7d604-120">In the "Choose a size" blade, select **DS1_V2 Standard**, which provides a low-cost way to experiment with Data Science VMs.</span></span> <span data-ttu-id="7d604-121">Klicka på knappen **Välj** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="7d604-121">Then click the **Select** button at the bottom of the blade.</span></span>

    ![Välja storlek på den virtuella datorn](../images/create-data-science-vm-2.png)

    <span data-ttu-id="7d604-123">_Välja storlek på den virtuella datorn_</span><span class="sxs-lookup"><span data-stu-id="7d604-123">_Choosing a VM size_</span></span>

1. <span data-ttu-id="7d604-124">På bladet Inställningar markerar du **SSH (22)** i listan över ingående portar så att klienterna kan ansluta till den virtuella datorn med hjälp av protokollet [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) på port 22.</span><span class="sxs-lookup"><span data-stu-id="7d604-124">In the "Settings" blade, check **SSH (22)** in the list of inbound ports so clients can connect to the VM using the [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protocol on port 22.</span></span> <span data-ttu-id="7d604-125">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d604-125">Then click **OK**.</span></span>

    ![Skapa den virtuella datorn](../images/create-data-science-vm-3.png)

    <span data-ttu-id="7d604-127">_Skapa den virtuella datorn_</span><span class="sxs-lookup"><span data-stu-id="7d604-127">_Creating the VM_</span></span>

1. <span data-ttu-id="7d604-128">Titta igenom de alternativ som du har valt för den virtuella datorn på bladet Skapa och sedan klicka på **Skapa** att starta framställningen av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7d604-128">In the "Create" blade, take a moment to review the options you selected for the VM, and click **Create** to start the VM creation process.</span></span>

    ![Skapa den virtuella datorn](../images/create-data-science-vm-4.png)

    <span data-ttu-id="7d604-130">_Skapa den virtuella datorn_</span><span class="sxs-lookup"><span data-stu-id="7d604-130">_Creating the VM_</span></span>

1. <span data-ttu-id="7d604-131">Klicka på **Resursgrupper** i menyn som visas till vänster i portalen.</span><span class="sxs-lookup"><span data-stu-id="7d604-131">Click **Resource groups** in the menu on the left side of the portal.</span></span> <span data-ttu-id="7d604-132">Klicka sedan på resursgruppen ”data-science-rg”.</span><span class="sxs-lookup"><span data-stu-id="7d604-132">Then click the "data-science-rg" resource group.</span></span>

    ![Öppna resursgruppen](../images/open-resource-group.png)

    <span data-ttu-id="7d604-134">_Öppna resursgruppen_</span><span class="sxs-lookup"><span data-stu-id="7d604-134">_Opening the resource group_</span></span>

1. <span data-ttu-id="7d604-135">Vänta tills statusen ”Distribuerar” ändras till ”Lyckades”. Detta är en indikation på att DSVM-datorn och alla stödjande Azure-resurser har skapats.</span><span class="sxs-lookup"><span data-stu-id="7d604-135">Wait until "Deploying" changes to "Succeeded" indicating that DSVM and supporting Azure resources have been created.</span></span> <span data-ttu-id="7d604-136">Distributionen tar högst 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="7d604-136">Deployment typically takes 5 minutes or less.</span></span> <span data-ttu-id="7d604-137">Med jämna mellanrum klickar du på **Uppdatera** överst på bladet för att uppdatera distributionens status.</span><span class="sxs-lookup"><span data-stu-id="7d604-137">Periodically click **Refresh** at the top of the blade to refresh the deployment status.</span></span>

    ![Övervaka distributionsstatus](../images/deployment-succeeded.png)

    <span data-ttu-id="7d604-139">_Övervaka distributionsstatus_</span><span class="sxs-lookup"><span data-stu-id="7d604-139">_Monitoring the deployment_</span></span>

<span data-ttu-id="7d604-140">När distributionen är klar kan du gå vidare till nästa övning.</span><span class="sxs-lookup"><span data-stu-id="7d604-140">Once the deployment has completed, proceed to the next exercise.</span></span>