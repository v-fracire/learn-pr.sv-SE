<span data-ttu-id="89c1c-101">I den här kursdelen installerar du MongoDB på din virtuella Ubuntu Linux-dator, som ska fungera som ett datalager för ditt kommande exempel på ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="89c1c-101">In this unit, you will install MongoDB on your Ubuntu Linux virtual machine to act as a data store for your upcoming sample web application.</span></span>

## <a name="install-mongodb"></a><span data-ttu-id="89c1c-102">Installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="89c1c-102">Install MongoDB</span></span>

1. <span data-ttu-id="89c1c-103">Från Cloud Shell, SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="89c1c-103">From Cloud Shell, SSH into your VM.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="89c1c-104">Importera krypteringsnyckeln för MongoDB-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="89c1c-104">Import the encryption key for the MongoDB repository.</span></span> <span data-ttu-id="89c1c-105">Då kan pakethanteraren verifiera att de mongodb-paketen som du installerar kommer från MongoDB Inc.</span><span class="sxs-lookup"><span data-stu-id="89c1c-105">This will allow the package manager to verify that the mongodb packages you install are coming from MongoDB Inc.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    <span data-ttu-id="89c1c-106">Kommandot **sudo** innebär att vi vill köra det angivna kommandot med administratörsprivilegier.</span><span class="sxs-lookup"><span data-stu-id="89c1c-106">The **sudo** command means that we want to run the specified command with administrative privileges.</span></span>

1. <span data-ttu-id="89c1c-107">Registrera MongoDB Ubuntu-lagringsplatsen så att pakethanteraren kan hitta mongodb-paketen.</span><span class="sxs-lookup"><span data-stu-id="89c1c-107">Register the MongoDB Ubuntu repository so the package manager can locate the mongodb packages.</span></span>

    > [!NOTE]
    > <span data-ttu-id="89c1c-108">Det här kommandot är olika för olika versioner av Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="89c1c-108">This command is different for different versions of Ubuntu.</span></span> <span data-ttu-id="89c1c-109">För att ta reda på vilken version av Ubuntu du använder så kör du: `uname -v`.</span><span class="sxs-lookup"><span data-stu-id="89c1c-109">To find out which version of Ubuntu you're using, run: `uname -v`.</span></span>
    > <span data-ttu-id="89c1c-110">Utdatan ser ut ungefär så här: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span><span class="sxs-lookup"><span data-stu-id="89c1c-110">This command will output something like this: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span></span>
    >
    > <span data-ttu-id="89c1c-111">Den visar att vi kör Ubuntu version 16.04.1.</span><span class="sxs-lookup"><span data-stu-id="89c1c-111">This output indicates that we're running Ubuntu version 16.04.1.</span></span>
    > <span data-ttu-id="89c1c-112">Se dokumentationen om att [Installera MongoDB Community Edition på Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) för att se det exakta kommandot för din version.</span><span class="sxs-lookup"><span data-stu-id="89c1c-112">Refer to the [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) documentation to get the exact command for your version.</span></span>

    <span data-ttu-id="89c1c-113">På Ubuntu 16.04 kör vi det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="89c1c-113">On Ubuntu 16.04, we run this command:</span></span>

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. <span data-ttu-id="89c1c-114">Uppdatera paketdatabasen så att vi har den senaste paketinformationen.</span><span class="sxs-lookup"><span data-stu-id="89c1c-114">Update the package database so we have the latest package information.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="89c1c-115">Installera MongoDB-paketet till vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="89c1c-115">Install the MongoDB package onto our VM.</span></span>

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. <span data-ttu-id="89c1c-116">Starta MongoDB-tjänsten så att du kan ansluta till den senare.</span><span class="sxs-lookup"><span data-stu-id="89c1c-116">Start the MongoDB service so you can connect to it later.</span></span>

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a><span data-ttu-id="89c1c-117">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="89c1c-117">Summary</span></span>

<span data-ttu-id="89c1c-118">Nu har vi MongoDB installerat på vår virtuella Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="89c1c-118">We now have MongoDB installed on our Ubuntu Linux VM.</span></span> <span data-ttu-id="89c1c-119">MongoDB fungerar som reservdatalager för den information du sparar och hämtar i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="89c1c-119">MongoDB will serve as your backing data store for the information you save and retrieve in your web application.</span></span>