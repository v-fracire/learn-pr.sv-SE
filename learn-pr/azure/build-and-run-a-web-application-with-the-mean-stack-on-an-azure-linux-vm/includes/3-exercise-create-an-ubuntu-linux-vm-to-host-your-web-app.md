<span data-ttu-id="5413b-101">MEAN-komponentstacken kräver en server.</span><span class="sxs-lookup"><span data-stu-id="5413b-101">The MEAN stack of components requires a server.</span></span> <span data-ttu-id="5413b-102">Det kan vara en Linux-dator eller en virtuell dator som du kör i ditt eget serverrum, eller som du konfigurerar på en molnbaserad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5413b-102">It could be a Linux machine or virtual machine running in your own server room, or it can be configured on a cloud-based virtual machine.</span></span> <span data-ttu-id="5413b-103">I den här modulen konfigurerar vi stacken som ska köras på en virtuell Ubuntu Linux-dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="5413b-103">In this module, we will set up the stack to run on an Ubuntu Linux virtual machine running on Azure.</span></span>

<span data-ttu-id="5413b-104">I den här kursdelen skapar du en ny virtuell Ubuntu Linux-dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="5413b-104">In this unit, you will be creating a new Ubuntu Linux virtual machine hosted on Azure.</span></span> <span data-ttu-id="5413b-105">Du kan också installera komponenterna i MEAN-stacken på en befintlig virtuell dator eller på en fysisk värddator.</span><span class="sxs-lookup"><span data-stu-id="5413b-105">You could also install your MEAN stack components on an existing virtual machine or physical host machine.</span></span> <span data-ttu-id="5413b-106">Genom att skapa en ny virtuell dator i den här övningen kan du koppla samman alla komponenter i en Azure-resursgrupp för enklare hantering och borttagning när du har slutfört övningarna.</span><span class="sxs-lookup"><span data-stu-id="5413b-106">By creating a new one with this exercise, you can tie together all the components into an Azure resource group for easier management and clean-up after you complete the exercises.</span></span>

<span data-ttu-id="5413b-107">Vi använder Cloud Shell-kommandoraden som är integrerad i Azure Portal för att skapa den virtuella Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="5413b-107">We will use the Cloud Shell command line that's integrated into the Azure portal to create the Linux VM.</span></span>

## <a name="provision-an-ubuntu-linux-vm"></a><span data-ttu-id="5413b-108">Etablera en virtuell Ubuntu Linux-dator</span><span class="sxs-lookup"><span data-stu-id="5413b-108">Provision an Ubuntu Linux VM</span></span>

1. <span data-ttu-id="5413b-109">Gå till [Azure Portal](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="5413b-109">Go to the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>
1. <span data-ttu-id="5413b-110">Öppna Cloud Shell från vinkelparentesen (>_) i verktygsfältet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5413b-110">Open Cloud Shell from the angle bracket (>_) icon in the Azure portal toolbar.</span></span>
1. <span data-ttu-id="5413b-111">I Cloud Shell kör du kommandot för att skapa den Azure-resursgrupp som ska innehålla vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="5413b-111">In Cloud Shell, execute the command to create an Azure resource group, which will include our VM.</span></span> <span data-ttu-id="5413b-112">Byt ut `<resource-group-name>` mot namnet på din egen resursgrupp och `<resource-group-location>` mot önskad Azure-region (till exempel `westus`).</span><span class="sxs-lookup"><span data-stu-id="5413b-112">Substitute your own resource group name for `<resource-group-name>` and your desired Azure location for `<resource-group-location>` (`westus`, for example).</span></span>


    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    <span data-ttu-id="5413b-113">Skriv ned namnet på resursgruppen eftersom vi ska använda det i andra kommandon.</span><span class="sxs-lookup"><span data-stu-id="5413b-113">Remember your resource group name as we will use it in other commands.</span></span>

1. <span data-ttu-id="5413b-114">Kör följande kommando i Cloud Shell för att skapa en ny virtuell Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="5413b-114">In Cloud Shell, run the following command to create a new Ubuntu Linux VM.</span></span> <span data-ttu-id="5413b-115">Ange namnet på din egen resursgrupp i `<resource-group-name>`, samt ett administratörsanvändarnamn och administratörslösenord i `<vm-admin-username>` och `<vm-admin-password>`.</span><span class="sxs-lookup"><span data-stu-id="5413b-115">Substitute your own resource group name for `<resource-group-name>` and your preferred admin username and password for `<vm-admin-username>` and `<vm-admin-password>`.</span></span>

    ```bash
    az vm create \
        --resource-group <resource-group-name> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    <span data-ttu-id="5413b-116">Skriv ned administratörsanvändarnamnet och administratörslösenordet så att du kan ansluta till den här virtuella datorn senare.</span><span class="sxs-lookup"><span data-stu-id="5413b-116">Take note of your admin username and password to allow you to connect to this VM later.</span></span>

    <span data-ttu-id="5413b-117">Det här kommandot tar ungefär två minuter att köra.</span><span class="sxs-lookup"><span data-stu-id="5413b-117">This command takes about two minutes to complete.</span></span> <span data-ttu-id="5413b-118">När kommandot har slutförts returneras utdata liknande de nedan.</span><span class="sxs-lookup"><span data-stu-id="5413b-118">When the command finishes, the resulting output will look similar to this.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<location you chose for the resource group>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<name you chose for thr resource group>",
        "zones": ""
    }
    ```

    <span data-ttu-id="5413b-119">Du bör även spara den offentliga IP-adressen för den nyligen skapade virtuella datorn för att kunna ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5413b-119">You will also want to save the public IP address of the newly created VM in order to connect to the VM.</span></span>

1. <span data-ttu-id="5413b-120">Prova att ansluta till den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5413b-120">Try connecting to your new VM.</span></span>

    <span data-ttu-id="5413b-121">Öppna en kommandotolk eller ett terminalfönster och kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="5413b-121">Open a command prompt/terminal window and run the following command.</span></span> <span data-ttu-id="5413b-122">Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.</span><span class="sxs-lookup"><span data-stu-id="5413b-122">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    <span data-ttu-id="5413b-123">Första gången du ansluter till datorn uppmanas du ange om du litar på fjärrdatorn.</span><span class="sxs-lookup"><span data-stu-id="5413b-123">The first time you connect to the machine, you'll be asked if you trust the remote machine.</span></span> <span data-ttu-id="5413b-124">Om du svarar `yes` sparas fingeravtrycket för datorns ECDSA-nyckel lokalt, för att efterföljande anslutningar ska anses vara betrodda.</span><span class="sxs-lookup"><span data-stu-id="5413b-124">By answering `yes`, the machine's ECDSA key fingerprint will be saved locally, so subsequent connections will be trusted.</span></span>

    <span data-ttu-id="5413b-125">Om allt ser bra ut skriver du `exit` för att stänga SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="5413b-125">If everything looks fine, type `exit` to close the SSH session.</span></span>

1. <span data-ttu-id="5413b-126">Öppna port 80 för att tillåta inkommande HTTP-trafik till det nya webbprogrammet du kommer att skapa.</span><span class="sxs-lookup"><span data-stu-id="5413b-126">Open port 80 to allow incoming HTTP traffic to your new web application that you will create.</span></span>

    <span data-ttu-id="5413b-127">Gå tillbaka till Cloud Shell i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5413b-127">Go back to Cloud Shell on the Azure portal.</span></span> <span data-ttu-id="5413b-128">Kör följande kommando och byt ut `<resource-group-name>` mot det ursprungliga resursgruppnamnet.</span><span class="sxs-lookup"><span data-stu-id="5413b-128">Issue the following command using your original resource group name for `<resource-group-name>`.</span></span>

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    <span data-ttu-id="5413b-129">Det här kommandot öppnar HTTP-porten på den virtuella datorn, som fick namnet ”MeanDemo” när den skapades.</span><span class="sxs-lookup"><span data-stu-id="5413b-129">This command will open up the HTTP port on your VM that was named "MeanDemo" when it was created.</span></span>

## <a name="summary"></a><span data-ttu-id="5413b-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5413b-130">Summary</span></span>

<span data-ttu-id="5413b-131">Nu när den nya virtuella Ubuntu Linux-datorn är klar kan vi ansluta till den för att börja installera de olika komponenterna i MEAN-stacken.</span><span class="sxs-lookup"><span data-stu-id="5413b-131">With your new Ubuntu Linux VM ready to go, we can now connect to it to start installing the various components of the MEAN stack.</span></span>