<span data-ttu-id="f0b3f-101">Innan vi går igenom några av de vanliga sätten att köra, visa och ta bort containrar ska vi titta på en grundläggande Nginx-konfiguration som körs på en Docker-container som finns på en virtuell Ubuntu-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="f0b3f-101">Before we cover some of the common ways to run, list, and delete containers, let's see a basic Nginx configuration running on a Docker container that's hosted on an Ubuntu virtual machine (VM).</span></span>

<span data-ttu-id="f0b3f-102">I den här delen får du lära dig att:</span><span class="sxs-lookup"><span data-stu-id="f0b3f-102">In this part, you'll learn how to:</span></span>

1. <span data-ttu-id="f0b3f-103">Skapa en virtuell Linux-dator som har konfigurerats för att installera Docker när den virtuella datorn först startas.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-103">Create a Linux VM that's configured to install Docker when the VM first boots.</span></span>
1. <span data-ttu-id="f0b3f-104">Anslut till den virtuella datorn och starta en Docker-container som kör Nginx.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-104">Connect to your VM and start a Docker container running Nginx.</span></span>
1. <span data-ttu-id="f0b3f-105">Använd portbindningen för att göra din webbserver åtkomlig utanför den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-105">Use port binding to make your web server accessible from outside the VM.</span></span>

## <a name="what-are-containers"></a><span data-ttu-id="f0b3f-106">Vad är containrar?</span><span class="sxs-lookup"><span data-stu-id="f0b3f-106">What are containers?</span></span>

<span data-ttu-id="f0b3f-107">Containrar är en virtualiseringsmiljö, men till skillnad från virtuella datorer har de inget operativsystem.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-107">A container is a virtualization environment but, unlike a virtual machine, it does not always include a full operating system.</span></span> <span data-ttu-id="f0b3f-108">I stället refererar de till operativsystemet i den värdmiljö som kör containern.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-108">Instead, it references the operating system of the host environment that runs that container.</span></span> <span data-ttu-id="f0b3f-109">Till exempel om fem containrar körs på en server med en specifik Linux-kärna så körs alla fem containrarna på samma kärna.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-109">For example, if you run five containers on a server with a specific Linux kernel, all five containers are running on that same kernel.</span></span>

<span data-ttu-id="f0b3f-110">Med containrar kan du paketera ditt program och allt som det kräver för att köras in på något som kallas en _containeravbildning_.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-110">Containers enable you to package your application and everything that it needs to run into what's known as a _container image_.</span></span> <span data-ttu-id="f0b3f-111">Containrar är isolerade vilket innebär att de inte stör andra containrar som körs på samma system.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-111">Containers are isolated, which means they don't interfere with any other container running on the same system.</span></span> <span data-ttu-id="f0b3f-112">Containeravbildningar är också portabla.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-112">Container images are also portable.</span></span> <span data-ttu-id="f0b3f-113">Du kan ladda upp dina avbildningar till ett containerregister, till exempel Docker Hub eller Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-113">You can upload your images to a container registry, such as Docker Hub or Azure Container Registry.</span></span> <span data-ttu-id="f0b3f-114">Du kan då köra dina containrar i molnet och de fungera på exakt samma sätt som i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-114">You can then run your containers in the cloud and expect them to behave the exact same way as they do in your development environment.</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

<span data-ttu-id="f0b3f-115">Containrar möjliggör snabb iteration.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-115">Containers enable rapid iteration.</span></span> <span data-ttu-id="f0b3f-116">Eftersom containrar inte behöver inkludera ett operativsystem är de normalt mycket mindre än fullständiga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-116">Because containers don't need to include an operating system, they're typically much smaller than full VMs.</span></span> <span data-ttu-id="f0b3f-117">Det gör att du kan starta och stoppa containrar snabbt, ofta inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-117">This enables you to start and stop containers quickly, often in seconds.</span></span>

## <a name="understand-the-setup"></a><span data-ttu-id="f0b3f-118">Förstå systemet</span><span class="sxs-lookup"><span data-stu-id="f0b3f-118">Understand the setup</span></span>

<span data-ttu-id="f0b3f-119">I utbildningssyfte tillhandahåller vi en sandbox-miljö för dig att arbeta i.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-119">For learning purposes, we provide a sandbox environment for you to work in.</span></span> <span data-ttu-id="f0b3f-120">Den här miljön innehåller Azure Cloud Shell, en webbläsarbaserad kommandoradsmiljö för att hantera och utveckla Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-120">This environment includes Azure Cloud Shell, a browser-based command-line experience for managing and developing Azure resources.</span></span>

<span data-ttu-id="f0b3f-121">Från Cloud Shell skapar du en virtuell Linux-dator som innehåller Docker.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-121">From Cloud Shell, you'll create a Linux VM that includes Docker.</span></span> <span data-ttu-id="f0b3f-122">Du ansluter till din virtuella Linux-dator via SSH så att du kan skapa och använda Docker-containrar interaktivt.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-122">You'll connect to your Linux VM over SSH so that you can interactively create and use Docker containers.</span></span>

<span data-ttu-id="f0b3f-123">Betrakta den virtuella Linux-datorn som din utvecklingsmiljö och en plats att lära dig om containrar.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-123">Think of this Linux VM as your development environment and a space to learn about containers.</span></span> <span data-ttu-id="f0b3f-124">I praktiken skulle du installera Docker på den dator som du använder för att utveckla dina appar.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-124">In practice, you would install Docker on the computer you use to develop your apps.</span></span> <span data-ttu-id="f0b3f-125">Docker körs på Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-125">Docker runs on Windows, macOS, and Linux.</span></span>

<span data-ttu-id="f0b3f-126">Vi tillhandahåller resurser där du kan lära dig mer om att köra Docker i din lokala utvecklingsmiljö i slutet av den här modulen.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-126">We'll provide resources where you can learn more about running Docker in your local development environment at the end of this module.</span></span>

## <a name="what-is-nginx"></a><span data-ttu-id="f0b3f-127">Vad är Nginx?</span><span class="sxs-lookup"><span data-stu-id="f0b3f-127">What is Nginx?</span></span>

<span data-ttu-id="f0b3f-128">Nginx (uttalas ”engine-x”) är en populär webbserver med öppen källkod som körs på Unix, Linux, macOS och Windows.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-128">Nginx (pronounced "engine-x") is a popular, free, open-source web server that runs on Unix, Linux, macOS, and Windows.</span></span> <span data-ttu-id="f0b3f-129">Att konfigurera en webbserver är ett bra sätt att se containrar i arbete.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-129">Configuring a web server is a good way to see containers in action.</span></span> <span data-ttu-id="f0b3f-130">Här använder du Nginx till att leverera en grundläggande webbsida.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-130">Here you'll use Nginx to serve a basic web page.</span></span>

## <a name="what-is-cloud-init"></a><span data-ttu-id="f0b3f-131">Vad är cloud-init?</span><span class="sxs-lookup"><span data-stu-id="f0b3f-131">What is cloud-init?</span></span>

<span data-ttu-id="f0b3f-132">Med Cloud-init kan du anpassa en virtuell Linux-dator när den startas för första gången.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-132">Cloud-init enables you to customize a Linux VM as it boots for the first time.</span></span> <span data-ttu-id="f0b3f-133">Du kan använda cloud-init till att installera paket och skriva filer eller för att konfigurera användare och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-133">You can use cloud-init to install packages and write files, or to configure users and security.</span></span> <span data-ttu-id="f0b3f-134">Här ska du använda ett cloud-init-skript för att installera Docker på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-134">Here you'll use a cloud-init script to install Docker on your VM.</span></span>

## <a name="what-is-port-binding"></a><span data-ttu-id="f0b3f-135">Vad är portbindning?</span><span class="sxs-lookup"><span data-stu-id="f0b3f-135">What is port binding?</span></span>

<span data-ttu-id="f0b3f-136">Med portbindningen kan du vidarebefordra nätverkstrafik till en container från dess värd.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-136">Port binding enables you to forward network traffic to a container from its host.</span></span>

<span data-ttu-id="f0b3f-137">En Docker-container körs i ett lokalt nätverk på containerns värdsystem.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-137">A Docker container runs on a local network on the container's host system.</span></span> <span data-ttu-id="f0b3f-138">I det här fallet är containerns nätverk på din virtuella Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-138">In this case, the container's network will be on your Linux VM.</span></span>

<span data-ttu-id="f0b3f-139">Du vet att webbförfrågningar normalt körs över port 80 (HTTP).</span><span class="sxs-lookup"><span data-stu-id="f0b3f-139">You know that web requests typically run over port 80 (HTTP).</span></span> <span data-ttu-id="f0b3f-140">Containern är inte tillgänglig för omvärlden eftersom den körs i ett lokalt nätverk i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-140">Because the container is running on a local network inside your VM, the container is not accessible to the outside world.</span></span>

<span data-ttu-id="f0b3f-141">Med Docker kan du publicera eller exponera en port så att den blir tillgänglig utanför den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-141">Docker enables you to publish, or expose, a port to make it accessible from outside the VM.</span></span> <span data-ttu-id="f0b3f-142">Här ska du konfigurerar din container så att trafik till port 8080 på den virtuella datorn kommer att vidarebefordras till port 80 på containern.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-142">Here, you'll configure your container so that network traffic to port 8080 on your VM will be forwarded to port 80 on your container.</span></span>

## <a name="create-a-virtual-machine-to-host-docker"></a><span data-ttu-id="f0b3f-143">Skapa en virtuell dator som värd för Docker</span><span class="sxs-lookup"><span data-stu-id="f0b3f-143">Create a virtual machine to host Docker</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="create-the-cloud-init-script"></a><span data-ttu-id="f0b3f-144">Skapa cloud-init-skriptet</span><span class="sxs-lookup"><span data-stu-id="f0b3f-144">Create the cloud-init script</span></span>

1. <span data-ttu-id="f0b3f-145">Navigera till mappen **clouddrive**.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-145">Navigate to the **clouddrive** folder.</span></span>

    ```azurecli
    cd clouddrive
    ```

1. <span data-ttu-id="f0b3f-146">Skapa en ny mapp med namnet **vm-config**.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-146">Create a new folder named **vm-config**.</span></span>

    ```azurecli
    mkdir vm-config
    ```

1. <span data-ttu-id="f0b3f-147">Navigera till den nya mappen.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-147">Navigate to the new folder.</span></span>

    ```azurecli
    cd vm-config
    ```

1. <span data-ttu-id="f0b3f-148">Skapa en ny fil med Cloud Shells integrerade redigerare.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-148">Create a new file using Cloud Shell's integrated editor.</span></span>

    ```azurecli
    code .
    ```

1. <span data-ttu-id="f0b3f-149">Klistra in det här cloud-init-skriptet i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-149">Paste this cloud-init script into the editor.</span></span>

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```yaml
    #cloud-config
    package_upgrade: true
    write_files:
      - path: /etc/systemd/system/docker.service.d/docker.conf
        content: |
          [Service]
          ExecStart=
          ExecStart=/usr/bin/dockerd
      - path: /etc/docker/daemon.json
        content: |
          {
            "hosts": ["fd://","tcp://127.0.0.1:2375"]
          }
    runcmd:
      - curl -sSL https://get.docker.com/ | sh
      - usermod -aG docker azureuser
    ```

1. <span data-ttu-id="f0b3f-150">Spara filen som **docker-vm-config.txt** (<kbd>Ctrl+S</kbd> eller högerklicka och välj **Spara**).</span><span class="sxs-lookup"><span data-stu-id="f0b3f-150">Save the file as **docker-vm-config.txt** (<kbd>Ctrl+S</kbd> or right click and select **Save**).</span></span>

1. <span data-ttu-id="f0b3f-151">Stäng redigeraren (<kbd>Ctrl+Q</kbd> eller högerklicka och välj **Avsluta**).</span><span class="sxs-lookup"><span data-stu-id="f0b3f-151">Close the editor (<kbd>Ctrl+Q</kbd> or right click and select **Quit**).</span></span>

### <a name="create-the-vm"></a><span data-ttu-id="f0b3f-152">Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f0b3f-152">Create the VM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f0b3f-153">Normalt sett skulle du skapa en resursgrupp för alla dina relaterade Azure-resurser med kommandot `az group create`, men för de här övningarna har en resursgrupp redan skapats åt dig.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-153">Normally, you would create a resource group for all your related Azure resources with the `az group create` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="f0b3f-154">Använd ”**<rgn>[resursgruppsnamn för sandbox-miljö]</rgn>**” som din resursgrupp i alla steg.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-154">Use "**<rgn>[sandbox resource group name]</rgn>**" as your resource group in all the steps.</span></span>

1. <span data-ttu-id="f0b3f-155">Kör kommandot `az vm create` för att skapa din virtuella Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-155">Run this `az vm create` command to create your Linux VM.</span></span>

    ```azurecli
    az vm create \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys \
      --custom-data docker-vm-config.txt
    ```

<span data-ttu-id="f0b3f-156">`--custom-data`-argumentet anger cloud-init-skriptet som installerar Docker när den virtuella datorn först startas.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-156">The `--custom-data` argument specifies the cloud-init script that installs Docker when the VM first boots.</span></span> <span data-ttu-id="f0b3f-157">Processen tar några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-157">The process will take a few minutes to complete.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="f0b3f-158">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f0b3f-158">Connect to the VM</span></span>

<span data-ttu-id="f0b3f-159">Här ska du skapa en SSH-anslutning till den virtuella datorn så att du kan konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-159">Here you'll create an SSH connection to your VM so that you can configure it.</span></span>

1. <span data-ttu-id="f0b3f-160">Kör det här `az vm show`-kommandot för att hämta den virtuella datorns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-160">Run this `az vm show` command to get your VM's IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. <span data-ttu-id="f0b3f-161">SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-161">SSH into the VM.</span></span> <span data-ttu-id="f0b3f-162">Ersätt **IP-adressen** med din adress.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-162">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

1. <span data-ttu-id="f0b3f-163">Verifiera Docker-versionen.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-163">Verify the Docker version.</span></span>

    ```bash
    docker version
    ```

    > [!NOTE]
    > <span data-ttu-id="f0b3f-164">Om kommandot misslyckas eller om du ser ett felmeddelande om att ansluta till Docker-daemon-socket betyder det att cloud-init-skriptet inte har slutförts ännu.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-164">If the command fails, or you see an error message about connecting to the Docker daemon socket, that means that the cloud-init script hasn't yet completed.</span></span> <span data-ttu-id="f0b3f-165">Även fast det finns olika sätt att vänta på att skriptet ska slutföras ska du tills vidare köra `exit` för att lämna din SSH-session och försöka ansluta igen om en minut eller två.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-165">Although there are ways to wait for the script to complete, for now run `exit` to leave your SSH session and try reconnecting in a minute or two.</span></span>

## <a name="start-nginx"></a><span data-ttu-id="f0b3f-166">Starta Nginx</span><span class="sxs-lookup"><span data-stu-id="f0b3f-166">Start Nginx</span></span>

<span data-ttu-id="f0b3f-167">Här ska du starta en Docker-container som kör Nginx.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-167">Here you'll start a Docker container that's running Nginx.</span></span>

1. <span data-ttu-id="f0b3f-168">Kör det här `docker run`-kommandot för att starta en Docker-container som kör Nginx.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-168">Run this `docker run` command to start a Docker container running Nginx.</span></span>

    ```bash
    docker run -d -p 8080:80 --name nginx nginx
    ```

    <span data-ttu-id="f0b3f-169">Kommandot laddar ned en Docker-avbildning som är förkonfigurerad med Nginx från [Docker Hub](https://hub.docker.com/_/nginx?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="f0b3f-169">The command downloads a Docker image that's preconfigured with Nginx from [Docker Hub](https://hub.docker.com/_/nginx?azure-portal=true).</span></span>

    <span data-ttu-id="f0b3f-170">`--name`-argumentet tilldelas namnet ”nginx” till din Docker-container så att du kan arbeta med den senare.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-170">The `--name` argument assigns the name "nginx" to your Docker container so you can work with it later.</span></span>

    <span data-ttu-id="f0b3f-171">`-p`-argumentet som vidarekopplar nätverkstrafik till port 8080 på den virtuella datorn till port 80 på containern.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-171">The `-p` argument forwards network traffic to port 8080 on your VM to port 80 on your container.</span></span>

1. <span data-ttu-id="f0b3f-172">Kör `curl` för att verifiera att Nginx är aktiv och tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-172">Run `curl` to verify that Nginx is running and accessible.</span></span>

    ```bash
    curl http://localhost:8080
    ```

1. <span data-ttu-id="f0b3f-173">Avsluta SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-173">Exit your SSH session.</span></span>

    ```bash
    exit
    ```

## <a name="access-your-web-server-from-a-web-browser"></a><span data-ttu-id="f0b3f-174">Öppna din webbserver från en webbläsare</span><span class="sxs-lookup"><span data-stu-id="f0b3f-174">Access your web server from a web browser</span></span>

<span data-ttu-id="f0b3f-175">Kom ihåg att du konfigurerade din behållare så att trafik på port 8080 vidarekopplas till port 80 på containern.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-175">Recall that you configured your container so that network traffic on port 8080 is forwarded to port 80 on your container.</span></span>

<span data-ttu-id="f0b3f-176">Om du vill se portmappning i arbete ska du här öppna port 8080 till den virtuella datorn via Azure-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-176">To see port mapping in action, here you'll open port 8080 to your VM through the Azure firewall.</span></span> <span data-ttu-id="f0b3f-177">Öppna sedan din webbserver från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-177">Then you'll access your web server from a browser.</span></span>

1. <span data-ttu-id="f0b3f-178">Kör det här `az vm open-port`-kommandot för att öppna port 8080 på din virtuella dator via brandväggen.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-178">Run this `az vm open-port` command to open port 8080 on your VM through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --port 8080 \
      --priority 1001
    ```

1. <span data-ttu-id="f0b3f-179">Öppna en webbläsare och navigera till den virtuella datorns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-179">From a web browser, navigate to your VM's IP address.</span></span> <span data-ttu-id="f0b3f-180">Se till att inkludera port 8080, till exempel, **http://40.121.106.164:8080**.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-180">Be sure to include port 8080, for example,  **http://40.121.106.164:8080**.</span></span>

    <span data-ttu-id="f0b3f-181">Du ser standardstartsidan.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-181">You see the default home page.</span></span>

    ![Webbplatsen som körs i en webbläsare](../media/2-nginx-browser.png)

## <a name="stop-the-container"></a><span data-ttu-id="f0b3f-183">Stoppa containern</span><span class="sxs-lookup"><span data-stu-id="f0b3f-183">Stop the container</span></span>

<span data-ttu-id="f0b3f-184">Stoppa containern nu.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-184">Now let's stop the container.</span></span>

1. <span data-ttu-id="f0b3f-185">Logga först in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-185">First, log back in to your VM.</span></span> <span data-ttu-id="f0b3f-186">Här är en kort repetition.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-186">Here's a refresher.</span></span>

    <span data-ttu-id="f0b3f-187">Hämta IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-187">Get the IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

    <span data-ttu-id="f0b3f-188">SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-188">SSH into the VM.</span></span> <span data-ttu-id="f0b3f-189">Ersätt **IP-adressen** med din adress.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-189">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

1. <span data-ttu-id="f0b3f-190">Kör `docker stop nginx` för att stoppa containern med namnet ”nginx”.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-190">Run `docker stop nginx` to stop the container named "nginx".</span></span>

    ```bash
    docker stop nginx
    ```

<span data-ttu-id="f0b3f-191">Håll din SSH-anslutning öppen för nästa del.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-191">Keep your SSH connection open for the next part.</span></span>

<span data-ttu-id="f0b3f-192">Grattis!</span><span class="sxs-lookup"><span data-stu-id="f0b3f-192">Congratulations!</span></span> <span data-ttu-id="f0b3f-193">Du har skapat en virtuell dator och använt den som värd för en Nginx-containeriserad webbserver.</span><span class="sxs-lookup"><span data-stu-id="f0b3f-193">You've successfully created a virtual machine and used it to host an Nginx containerized web server.</span></span>
