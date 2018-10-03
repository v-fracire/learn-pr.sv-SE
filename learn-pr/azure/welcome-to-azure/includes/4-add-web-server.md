<span data-ttu-id="ae4f9-101">Nu när den virtuella datorn är igång ska vi göra något med den.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-101">Now that your VM is up and running, let's do something with it.</span></span> <span data-ttu-id="ae4f9-102">Här installerar du en webbserver och levererar en grundläggande webbsida som visar den virtuella datorns värdnamn.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-102">Here you'll install a web server and serve up a basic web page that displays the VM's hostname.</span></span>

<span data-ttu-id="ae4f9-103">Du har flera alternativ när du ska konfigurera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-103">To configure a VM, you have several choices.</span></span> <span data-ttu-id="ae4f9-104">Du kan ansluta direkt och interaktivt konfigurera systemet.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-104">You can connect directly and interactively configure your system.</span></span> <span data-ttu-id="ae4f9-105">I till exempel Windows-system kan du skapa en fjärrskrivbordssession för att ansluta till användargränssnittet för din Windows-fjärrdator, som om du skulle sitta vid den.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-105">For example, on Windows systems you can create a Remote Desktop session to connect to the UI of your remote Windows computer as if you were seated at it.</span></span> <span data-ttu-id="ae4f9-106">På Linux-system kan du skapa en SSH-anslutning för att arbeta säkert med Linux-fjärrsystemet via terminalen.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-106">On Linux systems, you can create an SSH connection to securely work with your remote Linux system from the terminal.</span></span>

<span data-ttu-id="ae4f9-107">Manuell konfiguration är en bra början men när du lägger till system kan du automatisera dina distributioner.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-107">Manual configuration is a good start, but as you add systems, you can automate your deployments.</span></span> <span data-ttu-id="ae4f9-108">Automatisering involverar att köra upprepade processer som program och skript som tar hand om grovjobbet åt dig.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-108">Automation involves running repeatable processes such as programs and scripts that take care of the heavy lifting for you.</span></span>

::: zone pivot="windows-cloud"

<span data-ttu-id="ae4f9-109">Här fjärrkonfigurerar du IIS via en Cloud Shell-session med hjälp av en funktion i Windows-baserade virtuella Azure-datorer som kallas för tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-109">Here, you'll configure IIS remotely from your Cloud Shell session using a feature of Windows-based Azure virtual machines called the Custom Script Extension.</span></span>

::: zone-end

::: zone pivot="linux-cloud"

<span data-ttu-id="ae4f9-110">Här fjärrkonfigurerar du Nginx via en Cloud Shell-session med hjälp av en funktion i Linux-baserade virtuella Azure-datorer som kallas för tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-110">Here, you'll configure Nginx remotely from your Cloud Shell session using a feature of Linux-based Azure virtual machines called the Custom Script Extension.</span></span>

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a><span data-ttu-id="ae4f9-111">Vad är IIS?</span><span class="sxs-lookup"><span data-stu-id="ae4f9-111">What is IIS?</span></span>

<span data-ttu-id="ae4f9-112">Internet Information Services, eller IIS, är en webb som körs på Windows.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-112">Internet Information Services, or IIS, is a web server that runs on Windows.</span></span> <span data-ttu-id="ae4f9-113">Du kan använda IIS till att leverera standardwebbinnehåll (HTML, CSS och JavaScript) eller köra ASP.NET och andra typer av webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-113">You can use IIS to serve standard web content (HTML, CSS, and JavaScript) or run ASP.NET and other kinds of web applications.</span></span> <span data-ttu-id="ae4f9-114">IIS levereras med Windows Server men du måste aktivera det för att börja leverera webbsidor.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-114">IIS comes with Windows Server, but you need to activate it to start serving web pages.</span></span>

## <a name="whats-the-custom-script-extension"></a><span data-ttu-id="ae4f9-115">Vad är tillägget för anpassat skript?</span><span class="sxs-lookup"><span data-stu-id="ae4f9-115">What's the Custom Script Extension?</span></span>

<span data-ttu-id="ae4f9-116">Tillägget för anpassat skript är ett enkelt sätt att ladda ned och köra skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-116">The Custom Script Extension is an easy way to download and run scripts on your Azure VMs.</span></span> <span data-ttu-id="ae4f9-117">Det är bara ett av många sätt som du kan konfigurera systemet när din virtuella dator är igång.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-117">It's just one of the many ways you can configure the system once your VM is up and running.</span></span>

<span data-ttu-id="ae4f9-118">Du kan lagra skript i Azure Storage eller på en offentlig plats som GitHub.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-118">You can store your scripts in Azure storage or in a public location such as GitHub.</span></span> <span data-ttu-id="ae4f9-119">Du kan köra skript manuellt eller som en del av en mer automatiserad distribution.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-119">You can run scripts manually or as part of a more automated deployment.</span></span> <span data-ttu-id="ae4f9-120">Här kör du ett Azure CLI-kommando för att ladda ned ett PowerShell-skript från GitHub och köra det på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-120">Here, you'll run an Azure CLI command to download a PowerShell script from GitHub and execute it on your VM.</span></span> <span data-ttu-id="ae4f9-121">Skriptet konfigurerar IIS.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-121">The script configures IIS.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="ae4f9-122">Konfigurera IIS</span><span class="sxs-lookup"><span data-stu-id="ae4f9-122">Configure IIS</span></span>

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

<span data-ttu-id="ae4f9-123">När använder du tillägget för anpassat skript till att fjärrkonfigurera IIS på den virtuella datorn via Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-123">Here you'll use the Custom Script Extension to configure IIS remotely on your VM from Cloud Shell.</span></span> <span data-ttu-id="ae4f9-124">Du konfigurera även brandväggen att tillåta inkommande nätverksåtkomst på port 80 (HTTP).</span><span class="sxs-lookup"><span data-stu-id="ae4f9-124">You'll also configure the firewall to allow inbound network access on port 80 (HTTP).</span></span>

1. <span data-ttu-id="ae4f9-125">Kör det här `az vm extension set`-kommandot via Cloud Shell för att ladda ned och köra ett PowerShell-skript som installerar IIS och konfigurerar en grundläggande startsida.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-125">From Cloud Shell, run this `az vm extension set` command to download and execute a PowerShell script that installs IIS and configures a basic home page.</span></span>

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    <span data-ttu-id="ae4f9-126">Processen för att konfigurera IIS, ange innehållet för startsidan och starta tjänsten tar ett par minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-126">The process to configure IIS, set the contents of the homepage, and start the service takes a couple minutes to complete.</span></span>

    <span data-ttu-id="ae4f9-127">Under tiden kan du [undersöka PowerShell-skriptet](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) på en separat webbläsarflik om du vill.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-127">In the meantime, you can [examine the PowerShell script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) from a separate browser tab if you'd like.</span></span> <span data-ttu-id="ae4f9-128">Skriptet installerar IIS och konfigurerar startsidan att visa ett välkomstmeddelande tillsammans med datornamnet för den virtuell datorn, myVM.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-128">The script installs IIS and configures the home page to display a welcome message along with the VM's computer name, "myVM".</span></span>

1. <span data-ttu-id="ae4f9-129">Kör det här `az vm open-port`-kommandot för att öppna port 80 (HTTP) via brandväggen.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-129">Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a><span data-ttu-id="ae4f9-130">Kontrollera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="ae4f9-130">Verify the configuration</span></span>

<span data-ttu-id="ae4f9-131">Nu när IIS har konfigurerats ska vi verifiera att den körs.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-131">Now that IIS is set up, let's verify that it's running.</span></span>

1. <span data-ttu-id="ae4f9-132">Kör det här `az vm list-ip-addresses`-kommandot för att lista den virtuella datorns offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-132">Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.</span></span>

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > <span data-ttu-id="ae4f9-133">Detta `--query`-argument gör det här kommandot lite komplext.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-133">This `--query` argument makes this command a bit complex.</span></span> <span data-ttu-id="ae4f9-134">Men du kommer att bli ett proffs på det här när du utforskar Azure djupare.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-134">But you'll be a pro at this as you dig in and explore Azure.</span></span>

    <span data-ttu-id="ae4f9-135">Du kan se din virtuella dators offentliga IP-adress, till exempel 104.211.9.245.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-135">You see your VM's public IP address, for example, 104.211.9.245.</span></span>

1. <span data-ttu-id="ae4f9-136">Öppna en ny webbläsarflik och navigera till den virtuella datorns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-136">In a new browser tab, navigate to your VM's IP address.</span></span> <span data-ttu-id="ae4f9-137">Du ser ett välkomstmeddelande och namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-137">You see your welcome message and your VM's name.</span></span>

    ![](../media/4-iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a><span data-ttu-id="ae4f9-138">Vad är Nginx?</span><span class="sxs-lookup"><span data-stu-id="ae4f9-138">What is Nginx?</span></span>

<span data-ttu-id="ae4f9-139">Nginx (uttalas ”engine-x”) är en populär webbserver med öppen källkod som körs på Unix, Linux, macOS och Windows.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-139">Nginx (pronounced "engine-x") is a popular, free, open-source web server that runs on Unix, Linux, macOS, and Windows.</span></span> <span data-ttu-id="ae4f9-140">Här använder du Nginx till att leverera en grundläggande webbsida.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-140">Here you'll use Nginx to serve a basic web page.</span></span>

## <a name="whats-the-custom-script-extension"></a><span data-ttu-id="ae4f9-141">Vad är tillägget för anpassat skript?</span><span class="sxs-lookup"><span data-stu-id="ae4f9-141">What's the Custom Script Extension?</span></span>

<span data-ttu-id="ae4f9-142">Tillägget för anpassat skript är ett enkelt sätt att ladda ned och köra skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-142">The Custom Script Extension is an easy way to download and run scripts on your Azure VMs.</span></span> <span data-ttu-id="ae4f9-143">Det är bara ett av många sätt som du kan konfigurera systemet när din virtuella dator är igång.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-143">It's just one of the many ways you can configure the system once your VM is up and running.</span></span>

<span data-ttu-id="ae4f9-144">Du kan lagra skript i Azure Storage eller på en offentlig plats som GitHub.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-144">You can store your scripts in Azure storage or in a public location such as GitHub.</span></span> <span data-ttu-id="ae4f9-145">Du kan köra skript manuellt eller som en del av en mer automatiserad distribution.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-145">You can run scripts manually or as part of a more automated deployment.</span></span> <span data-ttu-id="ae4f9-146">Här kör du ett Azure CLI-kommando för att ladda ned ett Bash-skript från GitHub och köra det på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-146">Here, you'll run an Azure CLI command to download a Bash script from GitHub and execute it on your VM.</span></span> <span data-ttu-id="ae4f9-147">Skriptet konfigurerar Nginx.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-147">The script configures Nginx.</span></span>

## <a name="configure-nginx"></a><span data-ttu-id="ae4f9-148">Konfigurera Nginx</span><span class="sxs-lookup"><span data-stu-id="ae4f9-148">Configure Nginx</span></span>

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

<span data-ttu-id="ae4f9-149">När använder du tillägget för anpassat skript till att fjärrkonfigurera Nginx på den virtuella datorn via Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-149">Here you'll use the Custom Script Extension to configure Nginx remotely on your VM from Cloud Shell.</span></span> <span data-ttu-id="ae4f9-150">Du konfigurera även brandväggen att tillåta inkommande nätverksåtkomst på port 80 (HTTP).</span><span class="sxs-lookup"><span data-stu-id="ae4f9-150">You'll also configure the firewall to allow inbound network access on port 80 (HTTP).</span></span>

1. <span data-ttu-id="ae4f9-151">Kör det här `az vm extension set`-kommandot via Cloud Shell för att ladda ned och köra ett Bash-skript som installerar Nginx och konfigurerar en grundläggande startsida.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-151">From Cloud Shell, run this `az vm extension set` command to download and execute a Bash script that installs Nginx and configures a basic home page.</span></span>

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    <span data-ttu-id="ae4f9-152">Processen för att konfigurera Nginx, ange innehållet för startsidan och starta tjänsten tar ett par minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-152">The process to configure Nginx, set the contents of the homepage, and start the service takes a couple minutes to complete.</span></span>

    <span data-ttu-id="ae4f9-153">Under tiden kan du [undersöka Bash-skriptet](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) på en separat webbläsarflik om du vill.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-153">In the meantime, you can [examine the Bash script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) from a separate browser tab if you'd like.</span></span> <span data-ttu-id="ae4f9-154">Skriptet installerar Nginx och konfigurerar startsidan att visa ett välkomstmeddelande tillsammans med datornamnet för den virtuell datorn, myVM.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-154">The script installs Nginx and configures the home page to display a welcome message along with the VM's computer name, "myVM".</span></span>

1. <span data-ttu-id="ae4f9-155">Kör det här `az vm open-port`-kommandot för att öppna port 80 (HTTP) via brandväggen.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-155">Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a><span data-ttu-id="ae4f9-156">Kontrollera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="ae4f9-156">Verify the configuration</span></span>

<span data-ttu-id="ae4f9-157">Nu när Nginx har konfigurerats ska vi verifiera att den körs.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-157">Now that Nginx is set up, let's verify that it's running.</span></span>

1. <span data-ttu-id="ae4f9-158">Kör det här `az vm list-ip-addresses`-kommandot för att lista den virtuella datorns offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-158">Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.</span></span>

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > <span data-ttu-id="ae4f9-159">Detta `--query`-argument gör det här kommandot lite komplext.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-159">This `--query` argument makes this command a bit complex.</span></span> <span data-ttu-id="ae4f9-160">Men du kommer att bli ett proffs på det här när du utforskar Azure djupare.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-160">But you'll be a pro at this as you dig in and explore Azure.</span></span>

    <span data-ttu-id="ae4f9-161">Du kan se din virtuella dators offentliga IP-adress, till exempel 104.211.9.245.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-161">You see your VM's public IP address, for example, 104.211.9.245.</span></span>

1. <span data-ttu-id="ae4f9-162">Öppna en ny webbläsarflik och navigera till den virtuella datorns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-162">In a new browser tab, navigate to your VM's IP address.</span></span> <span data-ttu-id="ae4f9-163">Du ser ett välkomstmeddelande och namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-163">You see your welcome message and your VM's name.</span></span>

    ![](../media/4-nginx-browser.png)

::: zone-end

## <a name="summary"></a><span data-ttu-id="ae4f9-164">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ae4f9-164">Summary</span></span>

<span data-ttu-id="ae4f9-165">Din virtuella dator är igång kan nu leverera webbsidor, men vad betyder det för dig?</span><span class="sxs-lookup"><span data-stu-id="ae4f9-165">Your VM is running and can now serve up web pages, but what does that mean for you?</span></span>

<span data-ttu-id="ae4f9-166">Kom ihåg att varje resa börjar med grunderna, och nästan alla bra innovationer som föds i molnet, hos både stora och små företag, startar med en ett upplägg som liknar ditt.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-166">Remember, every journey starts with the basics, and almost any great innovation born in the cloud, from companies big and small, started with a similar setup to yours.</span></span> <span data-ttu-id="ae4f9-167">När din idé utvecklas börjar den få en positiv effekt på verksamheten och dina användare.</span><span class="sxs-lookup"><span data-stu-id="ae4f9-167">As your idea evolves, it begins making a positive impact on your business and your users.</span></span>