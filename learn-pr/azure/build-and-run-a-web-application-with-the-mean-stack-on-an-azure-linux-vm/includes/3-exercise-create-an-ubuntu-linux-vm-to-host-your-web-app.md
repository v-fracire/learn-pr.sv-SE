<span data-ttu-id="f0223-101">MEAN-komponentstacken kräver en server.</span><span class="sxs-lookup"><span data-stu-id="f0223-101">The MEAN stack of components requires a server.</span></span> <span data-ttu-id="f0223-102">Det kan vara en Linux-dator eller en virtuell dator som du kör i ditt eget serverrum, eller som du konfigurerar på en molnbaserad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f0223-102">It could be a Linux machine or virtual machine running in your own server room, or it can be configured on a cloud-based virtual machine.</span></span> <span data-ttu-id="f0223-103">I den här modulen ska vi konfigurera stacken att köra på en virtuell Ubuntu Linux-dator som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="f0223-103">In this module, we will be setting up the stack to run on an Ubuntu Linux virtual machine running on Azure.</span></span>

<span data-ttu-id="f0223-104">I den här övningen ska du skapa en ny virtuell Ubuntu Linux-dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="f0223-104">In this exercise, you will be creating a new Ubuntu Linux virtual machine hosted on Azure.</span></span> <span data-ttu-id="f0223-105">Du kan också installera komponenterna i MEAN-stacken på en befintlig virtuell dator eller på en fysisk värddator.</span><span class="sxs-lookup"><span data-stu-id="f0223-105">You could also install your MEAN stack components on an existing virtual machine or physical host machine.</span></span> <span data-ttu-id="f0223-106">Genom att skapa en ny virtuell dator i den här övningen kan du koppla samman alla komponenter i en Azure-resursgrupp för enklare hantering och borttagning när du har slutfört övningarna.</span><span class="sxs-lookup"><span data-stu-id="f0223-106">By creating a new one with this exercise, you can tie together all the components into an Azure resource group for easier management and clean-up after you complete the exercises.</span></span>

<span data-ttu-id="f0223-107">Vi ska använda Cloud Shell-kommandoradsverktyget som är integrerat med Azure Portal för att skapa den virtuella Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="f0223-107">We will use the Cloud Shell command-line integrated into the Azure portal to create the Linux VM.</span></span>

## <a name="provision-an-ubuntu-linux-vm"></a><span data-ttu-id="f0223-108">Etablera en virtuell Ubuntu Linux-dator</span><span class="sxs-lookup"><span data-stu-id="f0223-108">Provision an Ubuntu Linux VM</span></span>

1. <span data-ttu-id="f0223-109">Gå till [Azure Portal](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="f0223-109">Go to the [Azure Portal](https://portal.azure.com?azure-portal=true).</span></span>
1. <span data-ttu-id="f0223-110">Öppna Cloud Shell från `>_`-ikonen i verktygsfältet på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f0223-110">Open the Cloud Shell from the `>_` icon in the Azure portal toolbar.</span></span>
1. <span data-ttu-id="f0223-111">I Cloud Shell kör du kommandot för att skapa en Azure-resursgrupp, som ska innehålla vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="f0223-111">Within the Cloud Shell, execute the command to create an Azure resource group, which will include our VM.</span></span> <span data-ttu-id="f0223-112">Ange ett resursgruppnamn i `<resource-group-name>` och en valfri Azure-plats i `<resource-group-location>` (till exempel `westus`).</span><span class="sxs-lookup"><span data-stu-id="f0223-112">Substitute your own resource group name for `<resource-group-name>` and your desired Azure location for `<resource-group-location>` (`westus`, for example).</span></span>

    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    <span data-ttu-id="f0223-113">Skriv ned namnet på resursgruppen eftersom vi ska använda det i andra kommandon.</span><span class="sxs-lookup"><span data-stu-id="f0223-113">Remember your resource group name as we will use it in other commands.</span></span>

1. <span data-ttu-id="f0223-114">Kör följande kommando i Cloud Shell för att skapa en ny virtuell Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="f0223-114">In the Cloud Shell, run the following command to create a new Ubuntu Linux VM.</span></span> <span data-ttu-id="f0223-115">Ange namnet på resursgruppen i `<resource-group-name>` och ett administratörsanvändarnamn och administratörslösenord i `<vm-admin-username>` och `<vm-admin-password>`.</span><span class="sxs-lookup"><span data-stu-id="f0223-115">Substitute your own resource group name for `<resource-group-name>` and your preferred admin username and password for `<vm-admin-username>` and `<vm-admin-password>`.</span></span>

    ```bash
    az vm create \
        --resource-group <resource-group-name> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    <span data-ttu-id="f0223-116">Skriv ned administratörsanvändarnamnet och administratörslösenordet så att du kan ansluta till den här virtuella datorn senare.</span><span class="sxs-lookup"><span data-stu-id="f0223-116">Take note of your admin username and password to allow you to connect to this VM later.</span></span>

    <span data-ttu-id="f0223-117">Det här kommandot tar cirka 2 minuter.</span><span class="sxs-lookup"><span data-stu-id="f0223-117">This command takes about 2 minutes to complete.</span></span> <span data-ttu-id="f0223-118">När kommandot har slutförts returneras utdata liknande de nedan.</span><span class="sxs-lookup"><span data-stu-id="f0223-118">When the command finishes, the resulting output will look similar to this.</span></span>

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

    <span data-ttu-id="f0223-119">Du bör även spara den offentliga IP-adressen för den nyligen skapade virtuella datorn för att kunna ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0223-119">You will also want to save the public IP address of the newly created VM in order to connect to the VM.</span></span>

1. <span data-ttu-id="f0223-120">Prova att ansluta till den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0223-120">Try connecting to your new VM.</span></span>

    <span data-ttu-id="f0223-121">Öppna en kommandotolk eller ett terminalfönster och kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="f0223-121">Open a command prompt/terminal window and run the following command.</span></span> <span data-ttu-id="f0223-122">Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.</span><span class="sxs-lookup"><span data-stu-id="f0223-122">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    <span data-ttu-id="f0223-123">Första gången du ansluter till datorn uppmanas du att ange om du litar på fjärrdatorn.</span><span class="sxs-lookup"><span data-stu-id="f0223-123">The first time you connect to the machine, you'll be asked if you trust the remote machine.</span></span> <span data-ttu-id="f0223-124">Om du svarar `yes` sparas fingeravtrycket för datorns ECDSA-nyckel lokalt så att efterföljande anslutningar automatiskt betraktas som betrodda.</span><span class="sxs-lookup"><span data-stu-id="f0223-124">By answering `yes`, the machine's ECDSA key fingerprint will be saved locally so subsequent connections will be trusted.</span></span>

    <span data-ttu-id="f0223-125">Om allt ser bra ut skriver du `exit` för att stänga SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="f0223-125">If everything looks fine, type `exit` to close the SSH session.</span></span>

1. <span data-ttu-id="f0223-126">Öppna port 80 för att tillåta inkommande HTTP-trafik till det nya webbprogrammet som du ska skapa.</span><span class="sxs-lookup"><span data-stu-id="f0223-126">Open port 80 to allow incoming HTTP traffic to your new web application you will create.</span></span>

    <span data-ttu-id="f0223-127">Gå tillbaka till Cloud Shell på Azure Portal och kör följande kommando med det ursprungliga resursgruppnamnet för `<resource-group-name>`.</span><span class="sxs-lookup"><span data-stu-id="f0223-127">Go back to the Cloud Shell on the Azure portal and issue the following command, using your original resource group name for `<resource-group-name>`.</span></span>

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    <span data-ttu-id="f0223-128">När du kör kommandot öppnas HTTP-porten på den virtuella datorn med namnet ”MeanDemo”.</span><span class="sxs-lookup"><span data-stu-id="f0223-128">This will open up the HTTP port on your VM that was named "MeanDemo" when it was created.</span></span>

## <a name="summary"></a><span data-ttu-id="f0223-129">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f0223-129">Summary</span></span>

<span data-ttu-id="f0223-130">Nu när den nya virtuella Ubuntu Linux-datorn är klar kan vi ansluta till den för att börja installera de olika komponenterna i MEAN-stacken.</span><span class="sxs-lookup"><span data-stu-id="f0223-130">With your new Ubuntu Linux VM ready to go, we can now connect to it to start installing the various components of the MEAN stack.</span></span>
