<span data-ttu-id="53505-101">I den här kursdelen installerar du MongoDB på din virtuella Ubuntu Linux-dator, som ska fungera som ett datalager för ditt kommande exempel på ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="53505-101">In this unit, you will install MongoDB on your Ubuntu Linux virtual machine to act as a data store for your upcoming sample web application.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="53505-102">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="53505-102">Connect to the VM</span></span>

<span data-ttu-id="53505-103">För att kunna installera MongoDB måste du ansluta till den virtuella datorn med hjälp av **ssh**.</span><span class="sxs-lookup"><span data-stu-id="53505-103">In order to install MongoDB, you have to connect to the VM using **ssh**.</span></span> <span data-ttu-id="53505-104">Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.</span><span class="sxs-lookup"><span data-stu-id="53505-104">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-mongodb"></a><span data-ttu-id="53505-105">Installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="53505-105">Install MongoDB</span></span>

> [!Important]
> <span data-ttu-id="53505-106">Ubuntu tillhandahåller ett inofficiellt paket som heter **mongodb**.</span><span class="sxs-lookup"><span data-stu-id="53505-106">Ubuntu provides an unofficial package called **mongodb**.</span></span> <span data-ttu-id="53505-107">Det här paketet underhålls inte av MongoDB Inc.</span><span class="sxs-lookup"><span data-stu-id="53505-107">This package is not maintained by MongoDB Inc.</span></span>

1. <span data-ttu-id="53505-108">Importera krypteringsnyckeln för MongoDB-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="53505-108">Import the encryption key for the MongoDB repository.</span></span> <span data-ttu-id="53505-109">Då kan pakethanteraren verifiera att de mongodb-paket som du installerar kommer från MongoDB Inc.</span><span class="sxs-lookup"><span data-stu-id="53505-109">This will allow the package manager to verify that the mongodb packages you install are coming from MongoDB Inc.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    <span data-ttu-id="53505-110">Kommandot **sudo** innebär att vi vill köra det angivna kommandot med administratörsprivilegier.</span><span class="sxs-lookup"><span data-stu-id="53505-110">The **sudo** command means that we want to run the specified command with administrative privileges.</span></span>

1. <span data-ttu-id="53505-111">Registrera MongoDB Ubuntu-lagringsplatsen så att pakethanteraren kan hitta mongodb-paketen.</span><span class="sxs-lookup"><span data-stu-id="53505-111">Register the MongoDB Ubuntu repository so the package manager can locate the mongodb packages.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53505-112">Det här kommandot är olika för olika versioner av Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="53505-112">This command is different for different versions of Ubuntu.</span></span> <span data-ttu-id="53505-113">För att ta reda på vilken version av Ubuntu du använder så kör du: `uname -v`.</span><span class="sxs-lookup"><span data-stu-id="53505-113">To find out which version of Ubuntu you're using, run: `uname -v`.</span></span>
    > <span data-ttu-id="53505-114">Utdatan ser ut ungefär så här: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span><span class="sxs-lookup"><span data-stu-id="53505-114">This command will output something like this: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span></span>
    >
    > <span data-ttu-id="53505-115">Den visar att vi kör Ubuntu version 16.04.1.</span><span class="sxs-lookup"><span data-stu-id="53505-115">This output indicates that we're running Ubuntu version 16.04.1.</span></span>
    > <span data-ttu-id="53505-116">Se dokumentationen om att [Installera MongoDB Community Edition på Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) för att se det exakta kommandot för din version.</span><span class="sxs-lookup"><span data-stu-id="53505-116">Refer to the [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) documentation to get the exact command for your version.</span></span>

    <span data-ttu-id="53505-117">På Ubuntu 16.04 kör vi det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="53505-117">On Ubuntu 16.04, we run this command:</span></span>

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. <span data-ttu-id="53505-118">Läs in paketdatabasen igen så att vi har den senaste paketinformationen.</span><span class="sxs-lookup"><span data-stu-id="53505-118">Reload the package database so we have the latest package information.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="53505-119">Installera MongoDB-paketet till vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="53505-119">Install the MongoDB package onto our VM.</span></span>

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. <span data-ttu-id="53505-120">Starta MongoDB-tjänsten så att du kan ansluta till den senare.</span><span class="sxs-lookup"><span data-stu-id="53505-120">Start the MongoDB service so you can connect to it later.</span></span>

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a><span data-ttu-id="53505-121">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="53505-121">Summary</span></span>

<span data-ttu-id="53505-122">Nu har vi MongoDB installerat på vår virtuella Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="53505-122">We now have MongoDB installed on our Ubuntu Linux VM.</span></span> <span data-ttu-id="53505-123">MongoDB fungerar som reservdatalager för den information du sparar och hämtar i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="53505-123">MongoDB will serve as your backing data store for the information you save and retrieve in your web application.</span></span>