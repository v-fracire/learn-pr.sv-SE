<span data-ttu-id="89514-101">Vår virtuella Linux-dator har distribuerats och körs men den har inte konfigurerats att utföra arbete.</span><span class="sxs-lookup"><span data-stu-id="89514-101">We have our Linux VM deployed and running, but it's not configured to do any work.</span></span> <span data-ttu-id="89514-102">Nu ska vi ansluta till den med SSH och konfigurera Apache, så att vi har en aktiv server.</span><span class="sxs-lookup"><span data-stu-id="89514-102">Let's connect to it with SSH and configure Apache, so we have a running web server.</span></span>

## <a name="connect-to-the-vm-with-ssh"></a><span data-ttu-id="89514-103">Ansluta till den virtuella datorn med SSH</span><span class="sxs-lookup"><span data-stu-id="89514-103">Connect to the VM with SSH</span></span>

<span data-ttu-id="89514-104">Om du vill ansluta till en virtuell Azure-dator med en SSH-klient behöver du:</span><span class="sxs-lookup"><span data-stu-id="89514-104">To connect to an Azure VM with an SSH client, you will need:</span></span>

- <span data-ttu-id="89514-105">SSH-klientprogramvaran (finns på de flesta moderna operativsystem)</span><span class="sxs-lookup"><span data-stu-id="89514-105">SSH client software (present on most modern operating systems)</span></span>
- <span data-ttu-id="89514-106">Den offentliga IP-adressen för den virtuella datorn (eller privat om den virtuella datorn är konfigurerad för att ansluta till nätverket)</span><span class="sxs-lookup"><span data-stu-id="89514-106">The public IP address of the VM (or private if the VM is configured to connect to your network)</span></span>

### <a name="get-the-public-ip-address"></a><span data-ttu-id="89514-107">Hämta den offentliga IP-adressen</span><span class="sxs-lookup"><span data-stu-id="89514-107">Get the public IP address</span></span>

1. <span data-ttu-id="89514-108">Se till att [översiktspanelen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) för den virtuella dator du skapade tidigare är öppen i **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="89514-108">In the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true), ensure the **Overview** panel for the virtual machine that you created earlier is open.</span></span> <span data-ttu-id="89514-109">Du hittar den virtuella datorn under **Alla resurser** om du behöver öppna den.</span><span class="sxs-lookup"><span data-stu-id="89514-109">You can find the VM under **All Resources** if you need to open it.</span></span> <span data-ttu-id="89514-110">Översiktspanelen har mycket information om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="89514-110">The overview panel has a lot of information about the VM.</span></span>

    - <span data-ttu-id="89514-111">Du kan se om den virtuella datorn körs</span><span class="sxs-lookup"><span data-stu-id="89514-111">You can see whether the VM is running</span></span>
    - <span data-ttu-id="89514-112">Stoppa eller starta om den</span><span class="sxs-lookup"><span data-stu-id="89514-112">Stop or restart it</span></span>
    - <span data-ttu-id="89514-113">Hämta den offentliga IP-adressen för att ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="89514-113">Get the public IP address to connect to the VM</span></span>
    - <span data-ttu-id="89514-114">Visa aktiviteten för CPU, disk och nätverk</span><span class="sxs-lookup"><span data-stu-id="89514-114">See the activity of the CPU, disk, and network</span></span>

1. <span data-ttu-id="89514-115">Klicka på knappen **Anslut** högst upp i fönstret.</span><span class="sxs-lookup"><span data-stu-id="89514-115">Click the **Connect** button at the top of the pane.</span></span>

1. <span data-ttu-id="89514-116">På bladet **Anslut till virtuell dator** noterar du inställningarna för **IP-adress** och **portnummer**.</span><span class="sxs-lookup"><span data-stu-id="89514-116">In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings.</span></span> <span data-ttu-id="89514-117">På fliken **SSH** fliken finns också kommandot som du behöver köra lokalt för att ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="89514-117">On the **SSH** tab, you will also find the command you need to execute locally to connect to the VM.</span></span> <span data-ttu-id="89514-118">Kopiera detta till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="89514-118">Copy this to the clipboard.</span></span>

## <a name="connect-with-ssh"></a><span data-ttu-id="89514-119">Ansluta med SSH</span><span class="sxs-lookup"><span data-stu-id="89514-119">Connect with SSH</span></span>

1. <span data-ttu-id="89514-120">Klistra in den kommandorad som du fick från SSH-fliken i Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="89514-120">Paste the command line you got from the SSH tab into Azure Cloud Shell.</span></span> <span data-ttu-id="89514-121">Det bör se ut ungefär så här. Men den har en annan IP-adress (och kanske ett annat användarnamn om du inte använde **jim**!):</span><span class="sxs-lookup"><span data-stu-id="89514-121">It should look something like this; however, it will have a different IP address (and perhaps a different username if you didn't use **jim**!):</span></span>

    ```bash
    ssh jim@137.117.101.249
    ```

1. <span data-ttu-id="89514-122">Det här kommandot öppnar en Secure Shell-anslutning och placerar dig i en traditionell kommandotolk för Linux.</span><span class="sxs-lookup"><span data-stu-id="89514-122">This command will open a Secure Shell connection and place you at a traditional shell command prompt for Linux.</span></span>

1. <span data-ttu-id="89514-123">Prova att köra några Linux-kommandon</span><span class="sxs-lookup"><span data-stu-id="89514-123">Try executing a few Linux commands</span></span>
    - <span data-ttu-id="89514-124">`ls -la /` för att visa diskens rot</span><span class="sxs-lookup"><span data-stu-id="89514-124">`ls -la /` to show the root of the disk</span></span>
    - <span data-ttu-id="89514-125">`ps -l` för att visa alla processer som körs</span><span class="sxs-lookup"><span data-stu-id="89514-125">`ps -l` to show all the running processes</span></span>
    - <span data-ttu-id="89514-126">`dmesg` för att lista alla kernelmeddelanden</span><span class="sxs-lookup"><span data-stu-id="89514-126">`dmesg` to list all the kernel messages</span></span>
    - <span data-ttu-id="89514-127">`lsblk` för att lista alla blockenheter – här ser du dina enheter</span><span class="sxs-lookup"><span data-stu-id="89514-127">`lsblk` to list all the block devices - here you will see your drives</span></span>

<span data-ttu-id="89514-128">Mer intressant att observera i listan över enheter är vad som _saknas_.</span><span class="sxs-lookup"><span data-stu-id="89514-128">The more interesting thing to observe in the list of drives is what is _missing_.</span></span> <span data-ttu-id="89514-129">Observera att vår **Data**-enhet (`sdc`) finns men har inte monterats i filsystemet.</span><span class="sxs-lookup"><span data-stu-id="89514-129">Notice that our **Data** drive (`sdc`) is present but not mounted into the file system.</span></span> <span data-ttu-id="89514-130">Azure har lagt till en VHD men har inte initierat den.</span><span class="sxs-lookup"><span data-stu-id="89514-130">Azure added a VHD but didn't initialize it.</span></span>

## <a name="initialize-data-disks"></a><span data-ttu-id="89514-131">Initiera datadiskar</span><span class="sxs-lookup"><span data-stu-id="89514-131">Initialize data disks</span></span>

<span data-ttu-id="89514-132">Alla ytterligare enheter som du skapar från början måste initieras och formateras.</span><span class="sxs-lookup"><span data-stu-id="89514-132">Any additional drives you create from scratch will need to be initialized and formatted.</span></span> <span data-ttu-id="89514-133">Processen för att gör detta är identisk med en fysisk disk:</span><span class="sxs-lookup"><span data-stu-id="89514-133">The process for doing this is identical to a physical disk:</span></span>

1. <span data-ttu-id="89514-134">Först identifierar du disken.</span><span class="sxs-lookup"><span data-stu-id="89514-134">First, identify the disk.</span></span> <span data-ttu-id="89514-135">Vi gjorde det ovan.</span><span class="sxs-lookup"><span data-stu-id="89514-135">We did that above.</span></span> <span data-ttu-id="89514-136">Du kan också använda `dmesg | grep SCSI` som listar alla meddelanden från kärnan för SCSI-enheter.</span><span class="sxs-lookup"><span data-stu-id="89514-136">You could also use `dmesg | grep SCSI`, which will list all the messages from the kernel for SCSI devices.</span></span>

1. <span data-ttu-id="89514-137">När du känner till enheten (`sdc`) måste du initiera, vilket du kan göra med `fdisk`.</span><span class="sxs-lookup"><span data-stu-id="89514-137">Once you know the drive (`sdc`) you need to initialize, you can use `fdisk` to do that.</span></span> <span data-ttu-id="89514-138">Du måste köra kommandot med `sudo` och ange den disk du vill partitionera.</span><span class="sxs-lookup"><span data-stu-id="89514-138">You will need to run the command with `sudo` and supply the disk you want to partition.</span></span> <span data-ttu-id="89514-139">Vi kan använda följande kommando för att skapa en ny primär partition:</span><span class="sxs-lookup"><span data-stu-id="89514-139">We can use the following command to create a new primary partition:</span></span>

    ```bash
    (echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
    ```

1. <span data-ttu-id="89514-140">Nu måste vi skriva ett filsystem till partitionen med kommandot `mkfs`.</span><span class="sxs-lookup"><span data-stu-id="89514-140">Next, we need to write a file system to the partition with the `mkfs` command.</span></span>

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```

1. <span data-ttu-id="89514-141">Slutligen måste vi montera enheten till filsystemet.</span><span class="sxs-lookup"><span data-stu-id="89514-141">Finally, we need to mount the drive to the file system.</span></span> <span data-ttu-id="89514-142">Anta att vi har en `data`-mapp.</span><span class="sxs-lookup"><span data-stu-id="89514-142">Let's assume we will have a `data` folder.</span></span> <span data-ttu-id="89514-143">Nu ska vi skapa monteringspunktmappen och montera enheten.</span><span class="sxs-lookup"><span data-stu-id="89514-143">Let's create the mount point folder and mount the drive.</span></span>

    ```bash
    sudo mkdir /data & sudo mount /dev/sdc1 /data
    ```

    > [!TIP]
    > <span data-ttu-id="89514-144">Vi har initierats disken och monterat den.</span><span class="sxs-lookup"><span data-stu-id="89514-144">We initialized the disk and mounted it.</span></span> <span data-ttu-id="89514-145">Om du vill veta mer om den här processen kan du gå igenom modulen **Lägg till diskar i Azure Virtual Machines**.</span><span class="sxs-lookup"><span data-stu-id="89514-145">If you are interested in more details on this process go through the **Add and size disks in Azure virtual machines** module.</span></span> <span data-ttu-id="89514-146">Den här uppgiften gås igenom mer ingående där.</span><span class="sxs-lookup"><span data-stu-id="89514-146">This task is covered in more detail there.</span></span>

## <a name="install-software-onto-the-vm"></a><span data-ttu-id="89514-147">Installera programvara på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="89514-147">Install software onto the VM</span></span>

<span data-ttu-id="89514-148">Som du ser kan du med SSH arbeta med den virtuella Linux-datorn precis som en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="89514-148">As you can see, SSH allows you to work with the Linux VM just like a local computer.</span></span> <span data-ttu-id="89514-149">Du kan administrera den här virtuella datorn precis som andra Linux-datorer: installera programvara, konfigurera roller, justera funktioner och andra dagliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="89514-149">You can administer this VM as you would any other Linux computer: installing software, configuring roles, adjusting features, and other everyday tasks.</span></span> <span data-ttu-id="89514-150">Nu ska vi fokusera ett tag på att installera programvara.</span><span class="sxs-lookup"><span data-stu-id="89514-150">Let's focus on installing software for a moment.</span></span>

<span data-ttu-id="89514-151">Det finns flera alternativ för att installera programvara på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="89514-151">You have several options to install software onto the VM.</span></span> <span data-ttu-id="89514-152">Först kan du, som nämnt, använda `scp` till att kopiera lokala filer från din dator till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="89514-152">First, as mentioned, you can use `scp` to copy local files from your machine to the VM.</span></span> <span data-ttu-id="89514-153">Då kan du kopiera över de data eller anpassade program som du vill köra.</span><span class="sxs-lookup"><span data-stu-id="89514-153">This lets you copy over data or custom applications you want to run.</span></span>

<span data-ttu-id="89514-154">Du kan även installera programvara via Secure Shell.</span><span class="sxs-lookup"><span data-stu-id="89514-154">You can also install software through Secure Shell.</span></span> <span data-ttu-id="89514-155">Azure-datorer är som standard anslutna till Internet.</span><span class="sxs-lookup"><span data-stu-id="89514-155">Azure machines are, by default, internet connected.</span></span> <span data-ttu-id="89514-156">Du kan använda standardkommandon för att installera populära programvarupaket direkt från standardlagringsplatser.</span><span class="sxs-lookup"><span data-stu-id="89514-156">You can use standard commands to install popular software packages directly from standard repositories.</span></span> <span data-ttu-id="89514-157">Vi använder den här metoden för att installera Apache.</span><span class="sxs-lookup"><span data-stu-id="89514-157">Let's use this approach to install Apache.</span></span>

### <a name="install-the-apache-web-server"></a><span data-ttu-id="89514-158">Installera Apache-webbservern</span><span class="sxs-lookup"><span data-stu-id="89514-158">Install the Apache web server</span></span>

<span data-ttu-id="89514-159">Apache är tillgänglig i Ubuntus standardlagringsplatser för programvara, så vi installerar det med hjälp av konventionella pakethanteringsverktyg:</span><span class="sxs-lookup"><span data-stu-id="89514-159">Apache is available within Ubuntu's default software repositories, so we will install it using conventional package management tools:</span></span>

1. <span data-ttu-id="89514-160">Starta genom att uppdatera det lokala paketindexet för att spegla de senaste överordnade ändringarna:</span><span class="sxs-lookup"><span data-stu-id="89514-160">Start by updating the local package index to reflect the latest upstream changes:</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="89514-161">Installera sedan Apache:</span><span class="sxs-lookup"><span data-stu-id="89514-161">Next, install Apache:</span></span>

    ```bash
    sudo apt-get install apache2 -y
    ```

1. <span data-ttu-id="89514-162">Det bör starta automatiskt och statusen kan kontrolleras med `systemctl`:</span><span class="sxs-lookup"><span data-stu-id="89514-162">It should start automatically - we can check the status using `systemctl`:</span></span>

    ```bash
    sudo systemctl status apache2 --no-pager
    ```

    <span data-ttu-id="89514-163">Något sådant här bör returneras:</span><span class="sxs-lookup"><span data-stu-id="89514-163">This should return something like:</span></span>

    ```output
    apache2.service - The Apache HTTP Server
       Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
      Drop-In: /lib/systemd/system/apache2.service.d
               └─apache2-systemd.conf
       Active: active (running) since Mon 2018-09-03 21:00:03 UTC; 1min 34s ago
     Main PID: 11156 (apache2)
        Tasks: 55 (limit: 4915)
       CGroup: /system.slice/apache2.service
               ├─11156 /usr/sbin/apache2 -k start
               ├─11158 /usr/sbin/apache2 -k start
               └─11159 /usr/sbin/apache2 -k start

    test-web-eus-vm1 systemd[1]: Starting The Apache HTTP Server...
    test-web-eus-vm1 apachectl[11129]: AH00558: apache2: Could not reliably determine the server's fully qua
    test-web-eus-vm1 systemd[1]: Started The Apache HTTP Server.
    ```
    > [!NOTE]
    > <span data-ttu-id="89514-164">Det är enkelt att köra den här typen av kommandon, men det är en manuell process. Om vissa program alltid behöver installeras kan du automatisera processen med hjälp av skript.</span><span class="sxs-lookup"><span data-stu-id="89514-164">It's trivial to execute commands like this, however it's a manual process - if we always need to install some software, you might consider automating the process using scripting.</span></span>

1. <span data-ttu-id="89514-165">Slutligen kan vi försöka hämta standardsidan via den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="89514-165">Finally, we can try retrieving the default page through the public IP address.</span></span> <span data-ttu-id="89514-166">Men även om servern körs på den virtuella datorn, får du inte en giltig anslutning eller ett svar.</span><span class="sxs-lookup"><span data-stu-id="89514-166">However, even though the web server is running on the VM, you won't get a valid connection or response.</span></span> <span data-ttu-id="89514-167">Vet du varför?</span><span class="sxs-lookup"><span data-stu-id="89514-167">Do you know why?</span></span>

<span data-ttu-id="89514-168">Vi måste utföra ytterligare ett steg för att kunna interagera med webbservern.</span><span class="sxs-lookup"><span data-stu-id="89514-168">We need to perform one more step to be able to interact with the web server.</span></span> <span data-ttu-id="89514-169">Våra virtuella nätverk blockerar inkommande begäranden – det är standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="89514-169">Our virtual network is blocking the inbound request - this is the default behavior.</span></span> <span data-ttu-id="89514-170">Vi kan ändra det i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="89514-170">We can change that through configuration.</span></span> <span data-ttu-id="89514-171">Låt oss titta på det härnäst.</span><span class="sxs-lookup"><span data-stu-id="89514-171">Let's look at that next.</span></span>