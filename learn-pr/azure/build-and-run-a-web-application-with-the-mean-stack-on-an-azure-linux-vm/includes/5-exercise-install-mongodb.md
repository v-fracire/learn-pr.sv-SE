## <a name="exercise-install-mongodb"></a><span data-ttu-id="5f10d-101">Övning: Installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="5f10d-101">Exercise: Install MongoDB</span></span>

<span data-ttu-id="5f10d-102">I den här övningen installerar du MongoDB på din virtuella Ubuntu Linux-dator som fungerar som ett datalager för ditt kommande exempelwebbprogram.</span><span class="sxs-lookup"><span data-stu-id="5f10d-102">In this exercise, you will install MongoDB on your Ubuntu Linux virtual machine to act as a data store for your upcoming sample web application.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="5f10d-103">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="5f10d-103">Connect to the VM</span></span>

<span data-ttu-id="5f10d-104">För att kunna installera MongoDB måste du ansluta till den virtuella datorn med hjälp av **ssh**.</span><span class="sxs-lookup"><span data-stu-id="5f10d-104">In order to install MongoDB, you have to connect to the VM using **ssh**.</span></span> <span data-ttu-id="5f10d-105">Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.</span><span class="sxs-lookup"><span data-stu-id="5f10d-105">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-mongodb"></a><span data-ttu-id="5f10d-106">Installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="5f10d-106">Install MongoDB</span></span>

> [!Important]
> <span data-ttu-id="5f10d-107">Ubuntu tillhandahåller ett inofficiellt paket som heter **mongodb**, men det här paketet underhålls inte av MongoDB Inc.</span><span class="sxs-lookup"><span data-stu-id="5f10d-107">Ubuntu provides an unofficial package called **mongodb**, but this package is not maintained by MongoDB Inc.</span></span>

1. <span data-ttu-id="5f10d-108">Importera krypteringsnyckeln för MongoDB-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="5f10d-108">Import the encryption key for the MongoDB repository.</span></span> <span data-ttu-id="5f10d-109">Då kan pakethanteraren verifiera att de mongodb-paketen som du installerar kommer från MongoDB Inc.</span><span class="sxs-lookup"><span data-stu-id="5f10d-109">This will allow the package manager to verify that the mongodb packages you install are coming from MongoDB Inc.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    <span data-ttu-id="5f10d-110">Kommandot **sudo** innebär att vi vill köra det angivna kommandot med administratörsprivilegier.</span><span class="sxs-lookup"><span data-stu-id="5f10d-110">The **sudo** command means that we want to run the specified command with administrative privileges.</span></span>

1. <span data-ttu-id="5f10d-111">Registrera MongoDB Ubuntu-lagringsplatsen så att pakethanteraren kan hitta mongodb-paketen.</span><span class="sxs-lookup"><span data-stu-id="5f10d-111">Register the MongoDB Ubuntu repository so the package manager can locate the mongodb packages.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5f10d-112">Det här kommandot är olika för olika versioner av Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="5f10d-112">This command is different for different versions of Ubuntu.</span></span> <span data-ttu-id="5f10d-113">För att ta reda på vilken version av Ubuntu du kör så kör du: `uname -v`.</span><span class="sxs-lookup"><span data-stu-id="5f10d-113">To find out which version of Ubuntu you are running please run: `uname -v`.</span></span>
    > <span data-ttu-id="5f10d-114">Utdata blir ungefär så här: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span><span class="sxs-lookup"><span data-stu-id="5f10d-114">This will output something like this: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span></span>
    >
    > <span data-ttu-id="5f10d-115">Detta anger att vi kör Ubuntu version 16.04.1.</span><span class="sxs-lookup"><span data-stu-id="5f10d-115">This indicates that we are running Ubuntu version 16.04.1.</span></span>
    > <span data-ttu-id="5f10d-116">Se dokumentationen om att [installera MongoDB Community Edition på Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) för att se det exakta kommandot för din version</span><span class="sxs-lookup"><span data-stu-id="5f10d-116">Please refer to [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) documentation to get the exact command for your version</span></span>

    <span data-ttu-id="5f10d-117">På Ubuntu 16.04 kör vi det här:</span><span class="sxs-lookup"><span data-stu-id="5f10d-117">On Ubuntu 16.04 we run this:</span></span>

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. <span data-ttu-id="5f10d-118">Läsa in paketdatabasen igen så att vi har den senaste paketinformationen.</span><span class="sxs-lookup"><span data-stu-id="5f10d-118">Reload the package database so we have the latest package information.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="5f10d-119">Installera MongoDB-paketet till vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="5f10d-119">Install the MongoDB package onto our VM.</span></span>

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. <span data-ttu-id="5f10d-120">Starta MongoDB-tjänsten så att du kan ansluta till den senare.</span><span class="sxs-lookup"><span data-stu-id="5f10d-120">Start the MongoDB service so you can connect to it later.</span></span>

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a><span data-ttu-id="5f10d-121">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5f10d-121">Summary</span></span>

<span data-ttu-id="5f10d-122">Nu har vi MongoDB installerat på vår virtuella Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="5f10d-122">We now have MongoDB installed on our Ubuntu Linux VM.</span></span> <span data-ttu-id="5f10d-123">MongoDB fungerar som reservdatalager för den information du sparar och hämtar i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5f10d-123">MongoDB will serve as your backing data store for the information you save and retrieve in your web application.</span></span>
