<span data-ttu-id="5afd5-101">Det sista vi ska prova i vår virtuella dator är att installera en webbserver.</span><span class="sxs-lookup"><span data-stu-id="5afd5-101">The last thing we want to try on our VM is to install a web server.</span></span> <span data-ttu-id="5afd5-102">En av de enklaste paketen att installera är `nginx`.</span><span class="sxs-lookup"><span data-stu-id="5afd5-102">One of the easiest packages to install is `nginx`.</span></span>

1. <span data-ttu-id="5afd5-103">Leta reda på den offentliga IP-adressen till din virtuella Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="5afd5-103">Locate the public IP address of your Linux virtual machine.</span></span> <span data-ttu-id="5afd5-104">Kom ihåg att du kan använda kommandot `vm list-ip-addresses` till att leta upp den.</span><span class="sxs-lookup"><span data-stu-id="5afd5-104">Remember you can use the `vm list-ip-addresses` command to look it up.</span></span>

1. <span data-ttu-id="5afd5-105">Därefter öppnar du en `ssh`-anslutning till datorn, precis som du gjorde när vi testade den.</span><span class="sxs-lookup"><span data-stu-id="5afd5-105">Next, open an `ssh` connection to the machine like you did when we tested it.</span></span> <span data-ttu-id="5afd5-106">Kom ihåg att du måste kunna ange administratörsnamnet (”**aldis**”).</span><span class="sxs-lookup"><span data-stu-id="5afd5-106">Remember you will need to pass in the admin name ("**aldis**").</span></span>

1. <span data-ttu-id="5afd5-107">I gränssnittet som visas kör du följande kommando för att installera `nginx`-webbservern.</span><span class="sxs-lookup"><span data-stu-id="5afd5-107">In the presented shell, execute the following command to install the `nginx` web server.</span></span>

```azurecli
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. <span data-ttu-id="5afd5-108">I Azure Cloud Shell använder du `curl` till att läsa standardsidan från din Linux-webbserver.</span><span class="sxs-lookup"><span data-stu-id="5afd5-108">In Azure Cloud Shell, use `curl` to read the default page from your Linux web server.</span></span> <span data-ttu-id="5afd5-109">Du kan också öppna en ny webbläsarflik och bläddra till den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="5afd5-109">Alternatively, you can open a new browser tab and browse to the public IP address.</span></span>

```azurecli
curl 168.61.54.62
```

<span data-ttu-id="5afd5-110">Detta kommer att misslyckas eftersom den virtuella Linux-datorn inte visar port 80 (`http`) genom den inbyggda brandväggen.</span><span class="sxs-lookup"><span data-stu-id="5afd5-110">It will fail because the Linux virtual machine doesn't expose port 80 (`http`) through the built-in firewall.</span></span> <span data-ttu-id="5afd5-111">Som tur är har Azure CLI ett kommando för detta: `vm open-port`.</span><span class="sxs-lookup"><span data-stu-id="5afd5-111">Luckily, the Azure CLI has a command for that: `vm open-port`.</span></span> 

1. <span data-ttu-id="5afd5-112">Skriv följande i Cloud Shell för att öppna port 80:</span><span class="sxs-lookup"><span data-stu-id="5afd5-112">Type the following into Cloud Shell to open port 80:</span></span>

```
az vm open-port --port 80 --resource-group ExerciseResources --name SampleVM
```

<span data-ttu-id="5afd5-113">Försök slutligen med `curl` igen.</span><span class="sxs-lookup"><span data-stu-id="5afd5-113">Finally, try `curl` again.</span></span> <span data-ttu-id="5afd5-114">Den här gången bör den returnera data.</span><span class="sxs-lookup"><span data-stu-id="5afd5-114">This time it should return data.</span></span> <span data-ttu-id="5afd5-115">Du kan se sidan i en webbläsare också.</span><span class="sxs-lookup"><span data-stu-id="5afd5-115">You can see the page in a browser as well.</span></span>
