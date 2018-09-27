<span data-ttu-id="da6ce-101">Nu när den virtuella datorn är igång ska vi göra något med den.</span><span class="sxs-lookup"><span data-stu-id="da6ce-101">Now that your VM is up and running, let's do something with it.</span></span> <span data-ttu-id="da6ce-102">Här installerar du en webbserver och levererar en grundläggande webbsida som visar den virtuella datorns värdnamn.</span><span class="sxs-lookup"><span data-stu-id="da6ce-102">Here you'll install a web server and serve up a basic web page that displays the VM's hostname.</span></span>

<span data-ttu-id="da6ce-103">Du har flera alternativ när du ska konfigurera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="da6ce-103">To configure a VM, you have several choices.</span></span> <span data-ttu-id="da6ce-104">Du kan ansluta direkt och interaktivt konfigurera systemet.</span><span class="sxs-lookup"><span data-stu-id="da6ce-104">You can connect directly and interactively configure your system.</span></span> <span data-ttu-id="da6ce-105">I till exempel Windows-system kan du skapa en fjärrskrivbordssession för att ansluta till användargränssnittet för din Windows-fjärrdator, som om du skulle sitta vid den.</span><span class="sxs-lookup"><span data-stu-id="da6ce-105">For example, on Windows systems you can create a Remote Desktop session to connect to the UI of your remote Windows computer as if you were seated at it.</span></span> <span data-ttu-id="da6ce-106">På Linux-system kan du skapa en SSH-anslutning för att arbeta säkert med Linux-fjärrsystemet via terminalen.</span><span class="sxs-lookup"><span data-stu-id="da6ce-106">On Linux systems, you can create an SSH connection to securely work with your remote Linux system from the terminal.</span></span>

<span data-ttu-id="da6ce-107">Manuell konfiguration är en bra början men när du lägger till system kan du automatisera dina distributioner.</span><span class="sxs-lookup"><span data-stu-id="da6ce-107">Manual configuration is a good start, but as you add systems, you can automate your deployments.</span></span> <span data-ttu-id="da6ce-108">Automatisering involverar att köra upprepade processer som program och skript som tar hand om grovjobbet åt dig.</span><span class="sxs-lookup"><span data-stu-id="da6ce-108">Automation involves running repeatable processes such as programs and scripts that take care of the heavy lifting for you.</span></span>

<span data-ttu-id="da6ce-109">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="da6ce-109">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="da6ce-110">Här fjärrkonfigurerar du IIS via en Cloud Shell-session med hjälp av en funktion i Windows-baserade virtuella Azure-datorer som kallas för tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="da6ce-110">Here, you'll configure IIS remotely from your Cloud Shell session using a feature of Windows-based Azure virtual machines called the Custom Script Extension.</span></span>

<span data-ttu-id="da6ce-111">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="da6ce-111">::: zone-end</span></span>

<span data-ttu-id="da6ce-112">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="da6ce-112">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="da6ce-113">Här fjärrkonfigurerar du Nginx via en Cloud Shell-session med hjälp av en funktion i Linux-baserade virtuella Azure-datorer som kallas för tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="da6ce-113">Here, you'll configure Nginx remotely from your Cloud Shell session using a feature of Linux-based Azure virtual machines called the Custom Script Extension.</span></span>

<span data-ttu-id="da6ce-114">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="da6ce-114">::: zone-end</span></span>

<span data-ttu-id="da6ce-115">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="da6ce-115">::: zone pivot="windows-cloud"</span></span>

## <a name="what-is-iis"></a><span data-ttu-id="da6ce-116">Vad är IIS?</span><span class="sxs-lookup"><span data-stu-id="da6ce-116">What is IIS?</span></span>

<span data-ttu-id="da6ce-117">Internet Information Services, eller IIS, är en webb som körs på Windows.</span><span class="sxs-lookup"><span data-stu-id="da6ce-117">Internet Information Services, or IIS, is a web server that runs on Windows.</span></span> <span data-ttu-id="da6ce-118">Du kan använda IIS till att leverera standardwebbinnehåll (HTML, CSS och JavaScript) eller köra ASP.NET och andra typer av webbprogram.</span><span class="sxs-lookup"><span data-stu-id="da6ce-118">You can use IIS to serve standard web content (HTML, CSS, and JavaScript) or run ASP.NET and other kinds of web applications.</span></span> <span data-ttu-id="da6ce-119">IIS levereras med Windows Server men du måste aktivera det för att börja leverera webbsidor.</span><span class="sxs-lookup"><span data-stu-id="da6ce-119">IIS comes with Windows Server, but you need to activate it to start serving web pages.</span></span>

## <a name="whats-the-custom-script-extension"></a><span data-ttu-id="da6ce-120">Vad är tillägget för anpassat skript?</span><span class="sxs-lookup"><span data-stu-id="da6ce-120">What's the Custom Script Extension?</span></span>

<span data-ttu-id="da6ce-121">Tillägget för anpassat skript är ett enkelt sätt att ladda ned och köra skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="da6ce-121">The Custom Script Extension is an easy way to download and run scripts on your Azure VMs.</span></span> <span data-ttu-id="da6ce-122">Det är bara ett av många sätt som du kan konfigurera systemet när din virtuella dator är igång.</span><span class="sxs-lookup"><span data-stu-id="da6ce-122">It's just one of the many ways you can configure the system once your VM is up and running.</span></span>

<span data-ttu-id="da6ce-123">Du kan lagra skript i Azure Storage eller på en offentlig plats som GitHub.</span><span class="sxs-lookup"><span data-stu-id="da6ce-123">You can store your scripts in Azure storage or in a public location such as GitHub.</span></span> <span data-ttu-id="da6ce-124">Du kan köra skript manuellt eller som en del av en mer automatiserad distribution.</span><span class="sxs-lookup"><span data-stu-id="da6ce-124">You can run scripts manually or as part of a more automated deployment.</span></span> <span data-ttu-id="da6ce-125">Här kör du ett Azure CLI-kommando för att ladda ned ett PowerShell-skript från GitHub och köra det på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="da6ce-125">Here, you'll run an Azure CLI command to download a PowerShell script from GitHub and execute it on your VM.</span></span> <span data-ttu-id="da6ce-126">Skriptet konfigurerar IIS.</span><span class="sxs-lookup"><span data-stu-id="da6ce-126">The script configures IIS.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="da6ce-127">Konfigurera IIS</span><span class="sxs-lookup"><span data-stu-id="da6ce-127">Configure IIS</span></span>

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

<span data-ttu-id="da6ce-128">När använder du tillägget för anpassat skript till att fjärrkonfigurera IIS på den virtuella datorn via Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="da6ce-128">Here you'll use the Custom Script Extension to configure IIS remotely on your VM from Cloud Shell.</span></span> <span data-ttu-id="da6ce-129">Du konfigurera även brandväggen att tillåta inkommande nätverksåtkomst på port 80 (HTTP).</span><span class="sxs-lookup"><span data-stu-id="da6ce-129">You'll also configure the firewall to allow inbound network access on port 80 (HTTP).</span></span>

1. <span data-ttu-id="da6ce-130">Kör det här `az vm extension set`-kommandot via Cloud Shell för att ladda ned och köra ett PowerShell-skript som installerar IIS och konfigurerar en grundläggande startsida.</span><span class="sxs-lookup"><span data-stu-id="da6ce-130">From Cloud Shell, run this `az vm extension set` command to download and execute a PowerShell script that installs IIS and configures a basic home page.</span></span>

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    <span data-ttu-id="da6ce-131">Processen för att konfigurera IIS, ange innehållet för startsidan och starta tjänsten tar ett par minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="da6ce-131">The process to configure IIS, set the contents of the homepage, and start the service takes a couple minutes to complete.</span></span>

    <span data-ttu-id="da6ce-132">Under tiden kan du [undersöka PowerShell-skriptet](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) på en separat webbläsarflik om du vill.</span><span class="sxs-lookup"><span data-stu-id="da6ce-132">In the meantime, you can [examine the PowerShell script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) from a separate browser tab if you'd like.</span></span> <span data-ttu-id="da6ce-133">Skriptet installerar IIS och konfigurerar startsidan att visa ett välkomstmeddelande tillsammans med datornamnet för den virtuell datorn, myVM.</span><span class="sxs-lookup"><span data-stu-id="da6ce-133">The script installs IIS and configures the home page to display a welcome message along with the VM's computer name, "myVM".</span></span>

1. <span data-ttu-id="da6ce-134">Kör det här `az vm open-port`-kommandot för att öppna port 80 (HTTP) via brandväggen.</span><span class="sxs-lookup"><span data-stu-id="da6ce-134">Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a><span data-ttu-id="da6ce-135">Kontrollera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="da6ce-135">Verify the configuration</span></span>

<span data-ttu-id="da6ce-136">Nu när IIS har konfigurerats ska vi verifiera att den körs.</span><span class="sxs-lookup"><span data-stu-id="da6ce-136">Now that IIS is set up, let's verify that it's running.</span></span>

1. <span data-ttu-id="da6ce-137">Kör det här `az vm list-ip-addresses`-kommandot för att lista den virtuella datorns offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="da6ce-137">Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.</span></span>

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > <span data-ttu-id="da6ce-138">Detta `--query`-argument gör det här kommandot lite komplext.</span><span class="sxs-lookup"><span data-stu-id="da6ce-138">This `--query` argument makes this command a bit complex.</span></span> <span data-ttu-id="da6ce-139">Men du kommer att bli ett proffs på det här när du utforskar Azure djupare.</span><span class="sxs-lookup"><span data-stu-id="da6ce-139">But you'll be a pro at this as you dig in and explore Azure.</span></span>

    <span data-ttu-id="da6ce-140">Du ser den virtuella datorns offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="da6ce-140">You see your VM's public IP address.</span></span> <span data-ttu-id="da6ce-141">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="da6ce-141">Here's an example.</span></span>

    ```output
    104.211.9.245
    ```

1. <span data-ttu-id="da6ce-142">Öppna en ny webbläsarflik och navigera till den virtuella datorns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="da6ce-142">In a new browser tab, navigate to your VM's IP address.</span></span> <span data-ttu-id="da6ce-143">Du ser ett välkomstmeddelande och namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="da6ce-143">You see your welcome message and your VM's name.</span></span>

    ![](../media/4-iis-browser.png)

<span data-ttu-id="da6ce-144">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="da6ce-144">::: zone-end</span></span>

<span data-ttu-id="da6ce-145">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="da6ce-145">::: zone pivot="linux-cloud"</span></span>

## <a name="what-is-nginx"></a><span data-ttu-id="da6ce-146">Vad är Nginx?</span><span class="sxs-lookup"><span data-stu-id="da6ce-146">What is Nginx?</span></span>

<span data-ttu-id="da6ce-147">Nginx (uttalas ”engine-x”) är en populär webbserver med öppen källkod som körs på Unix, Linux, macOS och Windows.</span><span class="sxs-lookup"><span data-stu-id="da6ce-147">Nginx (pronounced "engine-x") is a popular, free, open-source web server that runs on Unix, Linux, macOS, and Windows.</span></span> <span data-ttu-id="da6ce-148">Här använder du Nginx till att leverera en grundläggande webbsida.</span><span class="sxs-lookup"><span data-stu-id="da6ce-148">Here you'll use Nginx to serve a basic web page.</span></span>

## <a name="whats-the-custom-script-extension"></a><span data-ttu-id="da6ce-149">Vad är tillägget för anpassat skript?</span><span class="sxs-lookup"><span data-stu-id="da6ce-149">What's the Custom Script Extension?</span></span>

<span data-ttu-id="da6ce-150">Tillägget för anpassat skript är ett enkelt sätt att ladda ned och köra skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="da6ce-150">The Custom Script Extension is an easy way to download and run scripts on your Azure VMs.</span></span> <span data-ttu-id="da6ce-151">Det är bara ett av många sätt som du kan konfigurera systemet när din virtuella dator är igång.</span><span class="sxs-lookup"><span data-stu-id="da6ce-151">It's just one of the many ways you can configure the system once your VM is up and running.</span></span>

<span data-ttu-id="da6ce-152">Du kan lagra skript i Azure Storage eller på en offentlig plats som GitHub.</span><span class="sxs-lookup"><span data-stu-id="da6ce-152">You can store your scripts in Azure storage or in a public location such as GitHub.</span></span> <span data-ttu-id="da6ce-153">Du kan köra skript manuellt eller som en del av en mer automatiserad distribution.</span><span class="sxs-lookup"><span data-stu-id="da6ce-153">You can run scripts manually or as part of a more automated deployment.</span></span> <span data-ttu-id="da6ce-154">Här kör du ett Azure CLI-kommando för att ladda ned ett Bash-skript från GitHub och köra det på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="da6ce-154">Here, you'll run an Azure CLI command to download a Bash script from GitHub and execute it on your VM.</span></span> <span data-ttu-id="da6ce-155">Skriptet konfigurerar Nginx.</span><span class="sxs-lookup"><span data-stu-id="da6ce-155">The script configures Nginx.</span></span>

## <a name="configure-nginx"></a><span data-ttu-id="da6ce-156">Konfigurera Nginx</span><span class="sxs-lookup"><span data-stu-id="da6ce-156">Configure Nginx</span></span>

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

<span data-ttu-id="da6ce-157">När använder du tillägget för anpassat skript till att fjärrkonfigurera Nginx på den virtuella datorn via Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="da6ce-157">Here you'll use the Custom Script Extension to configure Nginx remotely on your VM from Cloud Shell.</span></span> <span data-ttu-id="da6ce-158">Du konfigurera även brandväggen att tillåta inkommande nätverksåtkomst på port 80 (HTTP).</span><span class="sxs-lookup"><span data-stu-id="da6ce-158">You'll also configure the firewall to allow inbound network access on port 80 (HTTP).</span></span>

1. <span data-ttu-id="da6ce-159">Kör det här `az vm extension set`-kommandot via Cloud Shell för att ladda ned och köra ett Bash-skript som installerar Nginx och konfigurerar en grundläggande startsida.</span><span class="sxs-lookup"><span data-stu-id="da6ce-159">From Cloud Shell, run this `az vm extension set` command to download and execute a Bash script that installs Nginx and configures a basic home page.</span></span>

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    <span data-ttu-id="da6ce-160">Processen för att konfigurera Nginx, ange innehållet för startsidan och starta tjänsten tar ett par minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="da6ce-160">The process to configure Nginx, set the contents of the homepage, and start the service takes a couple minutes to complete.</span></span>

    <span data-ttu-id="da6ce-161">Under tiden kan du [undersöka Bash-skriptet](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) på en separat webbläsarflik om du vill.</span><span class="sxs-lookup"><span data-stu-id="da6ce-161">In the meantime, you can [examine the Bash script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) from a separate browser tab if you'd like.</span></span> <span data-ttu-id="da6ce-162">Skriptet installerar Nginx och konfigurerar startsidan att visa ett välkomstmeddelande tillsammans med datornamnet för den virtuell datorn, myVM.</span><span class="sxs-lookup"><span data-stu-id="da6ce-162">The script installs Nginx and configures the home page to display a welcome message along with the VM's computer name, "myVM".</span></span>

1. <span data-ttu-id="da6ce-163">Kör det här `az vm open-port`-kommandot för att öppna port 80 (HTTP) via brandväggen.</span><span class="sxs-lookup"><span data-stu-id="da6ce-163">Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a><span data-ttu-id="da6ce-164">Kontrollera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="da6ce-164">Verify the configuration</span></span>

<span data-ttu-id="da6ce-165">Nu när Nginx har konfigurerats ska vi verifiera att den körs.</span><span class="sxs-lookup"><span data-stu-id="da6ce-165">Now that Nginx is set up, let's verify that it's running.</span></span>

1. <span data-ttu-id="da6ce-166">Kör det här `az vm list-ip-addresses`-kommandot för att lista den virtuella datorns offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="da6ce-166">Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.</span></span>

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > <span data-ttu-id="da6ce-167">Detta `--query`-argument gör det här kommandot lite komplext.</span><span class="sxs-lookup"><span data-stu-id="da6ce-167">This `--query` argument makes this command a bit complex.</span></span> <span data-ttu-id="da6ce-168">Men du kommer att bli ett proffs på det här när du utforskar Azure djupare.</span><span class="sxs-lookup"><span data-stu-id="da6ce-168">But you'll be a pro at this as you dig in and explore Azure.</span></span>

    <span data-ttu-id="da6ce-169">Du ser den virtuella datorns offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="da6ce-169">You see your VM's public IP address.</span></span> <span data-ttu-id="da6ce-170">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="da6ce-170">Here's an example.</span></span>

    ```output
    137.135.110.210
    ```

1. <span data-ttu-id="da6ce-171">Öppna en ny webbläsarflik och navigera till den virtuella datorns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="da6ce-171">In a new browser tab, navigate to your VM's IP address.</span></span> <span data-ttu-id="da6ce-172">Du ser ett välkomstmeddelande och namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="da6ce-172">You see your welcome message and your VM's name.</span></span>

    ![](../media/4-nginx-browser.png)

<span data-ttu-id="da6ce-173">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="da6ce-173">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="da6ce-174">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="da6ce-174">Summary</span></span>

<span data-ttu-id="da6ce-175">Din virtuella dator är igång kan nu leverera webbsidor, men vad betyder det för dig?</span><span class="sxs-lookup"><span data-stu-id="da6ce-175">Your VM is running and can now serve up web pages, but what does that mean for you?</span></span>

<span data-ttu-id="da6ce-176">Kom ihåg att varje resa börjar med grunderna, och nästan alla bra innovationer som föds i molnet, hos både stora och små företag, startar med en ett upplägg som liknar ditt.</span><span class="sxs-lookup"><span data-stu-id="da6ce-176">Remember, every journey starts with the basics, and almost any great innovation born in the cloud, from companies big and small, started with a similar setup to yours.</span></span> <span data-ttu-id="da6ce-177">När din idé utvecklas börjar den få en positiv effekt på verksamheten och dina användare.</span><span class="sxs-lookup"><span data-stu-id="da6ce-177">As your idea evolves, it begins making a positive impact on your business and your users.</span></span>