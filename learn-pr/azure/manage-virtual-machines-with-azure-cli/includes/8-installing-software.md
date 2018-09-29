<span data-ttu-id="e016a-101">Det sista vi ska prova i vår virtuella dator är att installera en webbserver.</span><span class="sxs-lookup"><span data-stu-id="e016a-101">The last thing we want to try on our VM is to install a web server.</span></span> <span data-ttu-id="e016a-102">En av de enklaste paketen att installera är `nginx`.</span><span class="sxs-lookup"><span data-stu-id="e016a-102">One of the easiest packages to install is `nginx`.</span></span>

## <a name="install-nginx-web-server"></a><span data-ttu-id="e016a-103">Installera NGINX-webbserver</span><span class="sxs-lookup"><span data-stu-id="e016a-103">Install NGINX web server</span></span>

1. <span data-ttu-id="e016a-104">Leta reda på den offentliga IP-adressen till din virtuella Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="e016a-104">Locate the public IP address of your Linux virtual machine.</span></span> <span data-ttu-id="e016a-105">Kom ihåg att du kan använda kommandot `vm list-ip-addresses` till att leta upp den.</span><span class="sxs-lookup"><span data-stu-id="e016a-105">Remember you can use the `vm list-ip-addresses` command to look it up.</span></span>

1. <span data-ttu-id="e016a-106">Därefter öppnar du en `ssh`-anslutning till datorn, precis som du gjorde när vi testade den.</span><span class="sxs-lookup"><span data-stu-id="e016a-106">Next, open an `ssh` connection to the machine like you did when we tested it.</span></span> <span data-ttu-id="e016a-107">Kom ihåg att du måste kunna ange administratörsnamnet (”**aldis**”).</span><span class="sxs-lookup"><span data-stu-id="e016a-107">Remember you will need to pass in the admin name ("**aldis**").</span></span>

1. <span data-ttu-id="e016a-108">I gränssnittet som visas kör du följande kommando för att installera `nginx`-webbservern.</span><span class="sxs-lookup"><span data-stu-id="e016a-108">In the presented shell, execute the following command to install the `nginx` web server.</span></span>

```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. <span data-ttu-id="e016a-109">Avsluta Secure Shell.</span><span class="sxs-lookup"><span data-stu-id="e016a-109">Exit the Secure Shell.</span></span>

```bash
exit
```

## <a name="retrieve-our-default-page"></a><span data-ttu-id="e016a-110">Hämta standardsidan</span><span class="sxs-lookup"><span data-stu-id="e016a-110">Retrieve our default page</span></span>

1. <span data-ttu-id="e016a-111">I Azure Cloud Shell använder du `curl` till att läsa standardsidan från din Linux-webbserver.</span><span class="sxs-lookup"><span data-stu-id="e016a-111">In Azure Cloud Shell, use `curl` to read the default page from your Linux web server.</span></span> <span data-ttu-id="e016a-112">Du kan också öppna en ny webbläsarflik och bläddra till den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="e016a-112">Alternatively, you can open a new browser tab and browse to the public IP address.</span></span>

```azurecli
curl 40.83.165.85
```

<span data-ttu-id="e016a-113">Detta kommer att misslyckas eftersom den virtuella Linux-datorn inte visar port 80 (`http`) genom den inbyggda brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e016a-113">It will fail because the Linux virtual machine doesn't expose port 80 (`http`) through the built-in firewall.</span></span> <span data-ttu-id="e016a-114">Som tur är har Azure CLI ett kommando för detta: `vm open-port`.</span><span class="sxs-lookup"><span data-stu-id="e016a-114">Luckily, the Azure CLI has a command for that: `vm open-port`.</span></span> 

1. <span data-ttu-id="e016a-115">Skriv följande i Cloud Shell för att öppna port 80:</span><span class="sxs-lookup"><span data-stu-id="e016a-115">Type the following into Cloud Shell to open port 80:</span></span>

```azurecli
az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM
```

<span data-ttu-id="e016a-116">Det tar en stund att lägga till nätverksregeln och öppna porten genom brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e016a-116">It will take a moment to add the network rule and open the port through the firewall.</span></span> <span data-ttu-id="e016a-117">Prova `curl` igen.</span><span class="sxs-lookup"><span data-stu-id="e016a-117">Try `curl` again.</span></span> <span data-ttu-id="e016a-118">Den här gången bör den returnera data.</span><span class="sxs-lookup"><span data-stu-id="e016a-118">This time it should return data.</span></span> <span data-ttu-id="e016a-119">Du kan se sidan i en webbläsare också.</span><span class="sxs-lookup"><span data-stu-id="e016a-119">You can see the page in a browser as well.</span></span>

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx on Debian!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx on Debian!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working on Debian. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a></p>

<p>
      Please use the <tt>reportbug</tt> tool to report bugs in the
      nginx package with Debian. However, check <a
      href="http://bugs.debian.org/cgi-bin/pkgreport.cgi?ordering=normal;archive=0;src=nginx;repeatmerged=0">existing
      bug reports</a> before reporting a new bug.
</p>

<p><em>Thank you for using debian and nginx.</em></p>


</body>
</html>
```
