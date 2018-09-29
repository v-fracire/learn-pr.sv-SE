<span data-ttu-id="d160c-101">MEAN-komponentstacken kräver en server.</span><span class="sxs-lookup"><span data-stu-id="d160c-101">The MEAN stack of components requires a server.</span></span> <span data-ttu-id="d160c-102">Det kan vara en Linux-dator eller en virtuell dator som du kör i ditt eget serverrum, eller som du konfigurerar på en molnbaserad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d160c-102">It could be a Linux machine or virtual machine running in your own server room, or it can be configured on a cloud-based virtual machine.</span></span> <span data-ttu-id="d160c-103">I den här modulen konfigurerar vi stacken som ska köras på en virtuell Ubuntu Linux-dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="d160c-103">In this module, we will set up the stack to run on an Ubuntu Linux virtual machine running on Azure.</span></span>

<span data-ttu-id="d160c-104">I den här kursdelen skapar du en ny virtuell Ubuntu Linux-dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="d160c-104">In this unit, you will be creating a new Ubuntu Linux virtual machine hosted on Azure.</span></span> <span data-ttu-id="d160c-105">Du kan också installera komponenterna i MEAN-stacken på en befintlig virtuell dator eller på en fysisk värddator.</span><span class="sxs-lookup"><span data-stu-id="d160c-105">You could also install your MEAN stack components on an existing virtual machine or physical host machine.</span></span> <span data-ttu-id="d160c-106">Genom att skapa en ny virtuell dator i den här övningen kan du koppla samman alla komponenter i en Azure-resursgrupp för enklare hantering och rensning när du har slutfört övningarna.</span><span class="sxs-lookup"><span data-stu-id="d160c-106">By creating a new one with this exercise, you can tie together all the components into an Azure resource group for easier management and clean-up after you complete the exercises.</span></span>

## <a name="provision-an-ubuntu-linux-vm"></a><span data-ttu-id="d160c-107">Etablera en virtuell Ubuntu Linux-dator</span><span class="sxs-lookup"><span data-stu-id="d160c-107">Provision an Ubuntu Linux VM</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="creating-a-resource-group"></a><span data-ttu-id="d160c-108">Skapa resursgruppen</span><span class="sxs-lookup"><span data-stu-id="d160c-108">Creating a resource group</span></span>

<span data-ttu-id="d160c-109">Normalt sett är det första du gör när du skapar en ny uppsättning resurser att skapa en _resursgrupp_ som alla ingår i.</span><span class="sxs-lookup"><span data-stu-id="d160c-109">Normally, the first thing you'll do when creating a new set of resources is create a _resource group_ to own them all.</span></span> <span data-ttu-id="d160c-110">Det här är ett onödigt steg i sandbox-miljön i Azure, men när du arbetar med din egen prenumeration ska du använda följande kommando för att skapa en resursgrupp på en plats nära dig.</span><span class="sxs-lookup"><span data-stu-id="d160c-110">This is an unnecessary step in the Azure sandbox, but when you are working in your own subscription use the following command to create a resource group in a location near you.</span></span>

```azurecli
az group create --name <resource-group-name> --location <resource-group-location>
```

> [!IMPORTANT]
> <span data-ttu-id="d160c-111">Du behöver inte skapa en resursgrupp med sandbox-miljön i Azure.</span><span class="sxs-lookup"><span data-stu-id="d160c-111">You do not need to create a resource group with the Azure sandbox.</span></span> <span data-ttu-id="d160c-112">Använd i stället resursgruppen **<rgn>[resursgrupp för sandbox-miljö]</rgn>** som har skapats i förväg.</span><span class="sxs-lookup"><span data-stu-id="d160c-112">Instead use the pre-created resource group named **<rgn>[sandbox Resource Group]</rgn>**.</span></span>

1. <span data-ttu-id="d160c-113">Skapa en ny virtuell Ubuntu Linux-dator genom att ange följande kommando till höger i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="d160c-113">In the Cloud Shell on the right, type the following command to create a new Ubuntu Linux VM.</span></span> <span data-ttu-id="d160c-114">Ange ett administratörsanvändarnamn och administratörslösenord för `<vm-admin-username>` och `<vm-admin-password>`.</span><span class="sxs-lookup"><span data-stu-id="d160c-114">Substitute your preferred admin username and password for `<vm-admin-username>` and `<vm-admin-password>`.</span></span>

    ```azurecli
    az vm create \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    <span data-ttu-id="d160c-115">Skriv ned administratörsanvändarnamnet och administratörslösenordet så att du kan ansluta till den här virtuella datorn senare.</span><span class="sxs-lookup"><span data-stu-id="d160c-115">Take note of your admin username and password to allow you to connect to this VM later.</span></span>

    <span data-ttu-id="d160c-116">Det här kommandot tar ungefär två minuter att köra.</span><span class="sxs-lookup"><span data-stu-id="d160c-116">This command takes about two minutes to complete.</span></span> <span data-ttu-id="d160c-117">När kommandot har slutförts returneras utdata liknande de nedan.</span><span class="sxs-lookup"><span data-stu-id="d160c-117">When the command finishes, the resulting output will look similar to this.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<resource group location>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<rgn>[sandbox resource group name]</rgn>",
        "zones": ""
    }
    ```

    <span data-ttu-id="d160c-118">Du bör även spara den offentliga IP-adressen för den nyligen skapade virtuella datorn för att kunna ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d160c-118">You will also want to save the public IP address of the newly created VM in order to connect to the VM.</span></span>

1. <span data-ttu-id="d160c-119">Prova att ansluta till den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d160c-119">Try connecting to your new VM.</span></span>

    <span data-ttu-id="d160c-120">Från Cloud Shell kör du följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="d160c-120">From the Cloud Shell, run the following command.</span></span> <span data-ttu-id="d160c-121">Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.</span><span class="sxs-lookup"><span data-stu-id="d160c-121">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    <span data-ttu-id="d160c-122">Första gången du ansluter till datorn uppmanas du ange om du litar på fjärrdatorn.</span><span class="sxs-lookup"><span data-stu-id="d160c-122">The first time you connect to the machine, you'll be asked if you trust the remote machine.</span></span> <span data-ttu-id="d160c-123">Om du svarar `yes` sparas fingeravtrycket för datorns ECDSA-nyckel lokalt, för att efterföljande anslutningar ska anses vara betrodda.</span><span class="sxs-lookup"><span data-stu-id="d160c-123">By answering `yes`, the machine's ECDSA key fingerprint will be saved locally, so subsequent connections will be trusted.</span></span> <span data-ttu-id="d160c-124">Du uppmanas sedan att lösenordet, vilket du ser varje gång du ansluter.</span><span class="sxs-lookup"><span data-stu-id="d160c-124">You'll then be prompted for your password, which you'll see every time you connect.</span></span>

    <span data-ttu-id="d160c-125">Om allt ser bra ut skriver du `exit` för att stänga SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="d160c-125">If everything looks fine, type `exit` to close the SSH session.</span></span>

1. <span data-ttu-id="d160c-126">Öppna port 80 på den virtuella datorn för att tillåta inkommande HTTP-trafik till det nya webbprogrammet som du kommer att skapa.</span><span class="sxs-lookup"><span data-stu-id="d160c-126">Open port 80 on the VM to allow incoming HTTP traffic to the new web application that you will create.</span></span>

    ```azurecli
    az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name MeanDemo
    ```

    <span data-ttu-id="d160c-127">Det här kommandot öppnar HTTP-porten på den virtuella datorn, som fick namnet ”MeanDemo” när den skapades.</span><span class="sxs-lookup"><span data-stu-id="d160c-127">This command will open up the HTTP port on your VM that was named "MeanDemo" when it was created.</span></span>

## <a name="summary"></a><span data-ttu-id="d160c-128">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d160c-128">Summary</span></span>

<span data-ttu-id="d160c-129">Nu när den nya virtuella Ubuntu Linux-datorn är klar kan vi ansluta till den för att börja installera de olika komponenterna i MEAN-stacken.</span><span class="sxs-lookup"><span data-stu-id="d160c-129">With your new Ubuntu Linux VM ready to go, we can now connect to it to start installing the various components of the MEAN stack.</span></span>