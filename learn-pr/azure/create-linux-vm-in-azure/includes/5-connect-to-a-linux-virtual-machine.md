<span data-ttu-id="3b2d7-101">Nu när vi har en virtuell Linux-dator i Azure bör du som nästa steg konfigurera den för uppgifter som vi vill flytta till Azure.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-101">Now that we have a Linux VM in Azure, the next thing you’ll want to do is configure it for the tasks we want to move to Azure.</span></span>

<span data-ttu-id="3b2d7-102">Om du inte har konfigurerat plats-till-plats-VPN-anslutning till Azure kan dina virtuella datorer i Azure inte nås från det lokala nätverket.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-102">Unless you’ve set up a site-to-site VPN to Azure, your Azure VMs won’t be accessible from your local network.</span></span> <span data-ttu-id="3b2d7-103">Om du precis har kommit igång med Azure har du troligen inte en fungerande plats-till-plats-VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-103">If you’re just getting started with Azure, it’s unlikely that you have a working site-to-site VPN.</span></span> <span data-ttu-id="3b2d7-104">Så hur kan du ansluta till den virtuella datorn?</span><span class="sxs-lookup"><span data-stu-id="3b2d7-104">So how can you connect to the virtual machine?</span></span>

## <a name="azure-vm-ip-addresses"></a><span data-ttu-id="3b2d7-105">IP-adresser för virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="3b2d7-105">Azure VM IP addresses</span></span>

<span data-ttu-id="3b2d7-106">Som vi såg alldeles nyss kommunicerar virtuella Azure-datorer i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-106">As we saw a moment ago, Azure VMs communicate on a virtual network.</span></span> <span data-ttu-id="3b2d7-107">De kan också ha en valfri offentlig IP-adress som har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-107">They can also have an optional public IP address assigned to them.</span></span> <span data-ttu-id="3b2d7-108">Med en offentlig IP-adress kan vi interagera med den virtuella datorn via Internet.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-108">With a public IP, we can interact with the VM over the Internet.</span></span> <span data-ttu-id="3b2d7-109">Vi kan också konfigurera ett virtuellt privat nätverk (VPN) som ansluter det lokala nätverket till Azure. Då kan vi på ett säkert sätt ansluta till den virtuella datorn utan att exponera en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-109">Alternatively, we can set up a virtual private network (VPN) that connects our on-premises network to Azure - letting us securely connect to the VM without exposing a public IP.</span></span> <span data-ttu-id="3b2d7-110">Den här metoden beskrivs i en annan modul och är fullständigt dokumenterad om du vill utforska det alternativet.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-110">This approach is covered in another module and is fully documented if you are interested in exploring that option.</span></span>

<span data-ttu-id="3b2d7-111">En sak att tänka på med offentliga IP-adresser i Azure är att de ofta tilldelas dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-111">One thing to be aware of with public IP addresses in Azure is they are often dynamically allocated.</span></span> <span data-ttu-id="3b2d7-112">Det innebär att IP-adressen kan ändras över tid – för virtuella datorer sker det när den virtuella datorn startas om.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-112">That means the IP address can change over time - for VMs this happens when the VM is restarted.</span></span> <span data-ttu-id="3b2d7-113">Du kan betala mer för att tilldela statiska adresser om du vill ansluta direkt till en IP-adress i stället för ett namn och se till att IP-adressen inte ändras.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-113">You can pay more to assign static addresses if you want to connect directly to an IP address instead of a name and need to ensure that the IP address will not change.</span></span>

## <a name="connect-to-an-azure-linux-vm-with-ssh"></a><span data-ttu-id="3b2d7-114">Ansluta till en virtuell Azure Linux-dator med SSH</span><span class="sxs-lookup"><span data-stu-id="3b2d7-114">Connect to an Azure Linux VM with SSH</span></span>

<span data-ttu-id="3b2d7-115">Att ansluta till en virtuell dator i Azure med hjälp av SSH är enkelt.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-115">Connecting to a VM in Azure using SSH is straight forward.</span></span> <span data-ttu-id="3b2d7-116">Öppna egenskaperna för din virtuella dator i Azure-portalen och klicka på **Anslut** högst upp.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-116">In the Azure portal, open the properties of your VM and at the top, click **Connect**.</span></span> <span data-ttu-id="3b2d7-117">Då visas IP-adresser tilldelade till den virtuella datorn tillsammans med alla inloggningsuppgifterna för SSH.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-117">This will show you the IP addresses assigned to the VM along with all the login details for SSH.</span></span> 

![SSH-anslutningsinformation för virtuell Linux-dator](../media-drafts/5-connect-ssh.png)

<span data-ttu-id="3b2d7-119">Med dessa uppgifter kan vi använda följande kommando för att komma in den virtuella Linux-datorn:</span><span class="sxs-lookup"><span data-stu-id="3b2d7-119">With these details, we can use the following command to get into our Linux VM:</span></span>

```bash
ssh jim@137.117.101.249
```

<span data-ttu-id="3b2d7-120">Första gången vi ansluter ber SSH oss om att autentisera mot en okänd värd.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-120">The first time we connect, SSH will ask us about authenticating against an unknown host.</span></span> <span data-ttu-id="3b2d7-121">SSH berättar att du aldrig har anslutit till den här servern tidigare.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-121">SSH is telling you that you've never connected to this server before.</span></span> <span data-ttu-id="3b2d7-122">Om det är sant är det helt normalt och du kan svara ”ja” om du vill spara fingeravtrycket för servern i den kända värdfilen.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-122">If that's true, then it's perfectly normal, and you can respond with "yes" to save the fingerprint of the server off in the known host file.</span></span>

```output
The authenticity of host '137.117.101.249 (137.117.101.249)' can't be established.
ECDSA key fingerprint is SHA256:w1h08h4ie1iMq7ibIVSQM/PhcXFV7O7EEhjEqhPYMWY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '137.117.101.249' (ECDSA) to the list of known hosts.
```

## <a name="transferring-files-to-the-vm"></a><span data-ttu-id="3b2d7-123">Överföra filer till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="3b2d7-123">Transferring files to the VM</span></span>

<span data-ttu-id="3b2d7-124">En annan vanlig uppgift är att kopiera lokala filer och data från en server till en annan.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-124">Another common task is to copy local files or data from one server to another.</span></span> <span data-ttu-id="3b2d7-125">I vårt fall vill vi i slutänden kopiera våra webbplatsdata till den virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-125">In our case, we'll eventually want to copy our website data up to our Azure VM.</span></span> <span data-ttu-id="3b2d7-126">Med virtuella Linux-datorer och SSH kan vi använda kommandot `scp`.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-126">With Linux VMs and SSH, we can use the `scp` command.</span></span> <span data-ttu-id="3b2d7-127">Kommandot liknar att kopiera lokala filer med `cp`, men du måste ange fjärranvändaren och värden i kommandot.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-127">The command is similar to copying local files with `cp`, except you'll have to specify the remote user and host in your command.</span></span> 

<span data-ttu-id="3b2d7-128">Om du till exempel vill kopiera `somefile.txt` från vår aktuella katalog till `~/folder` på en Linux-dator med IP-adressen `192.168.1.25` kan du skriva:</span><span class="sxs-lookup"><span data-stu-id="3b2d7-128">For example, to copy `somefile.txt` from our current directory to `~/folder` on a Linux machine with the IP address `192.168.1.25`, you can type:</span></span>

```bash
scp somefile.txt someuser@192.168.1.25:~/folder/
```

<span data-ttu-id="3b2d7-129">Detta autentiserar som `someuser` på fjärrsystemet, frågar om ett lösenord eller använder din privata SSH-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-129">This will authenticate as `someuser` on the remote system, prompting for a password, or using your SSH private key.</span></span> <span data-ttu-id="3b2d7-130">Användaren måste ha skrivbehörighet för `~/folder/` på målservern.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-130">That user will need to have write permissions to `~/folder/` on the destination server.</span></span>

> [!WARNING]
> <span data-ttu-id="3b2d7-131">`scp` gör lokala filkopior om kommandoraden inte blir helt rätt.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-131">`scp` will do local file copies if you don't get the command line quite right.</span></span> <span data-ttu-id="3b2d7-132">Den vanligaste biten som saknas är målmappen.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-132">The most common missing piece is the destination folder.</span></span>

<span data-ttu-id="3b2d7-133">Nu ska vi prova att använda SSH för att ansluta till den virtuella Linux-datorn som körs.</span><span class="sxs-lookup"><span data-stu-id="3b2d7-133">Let's try using SSH to connect to our running Linux VM.</span></span>