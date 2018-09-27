<span data-ttu-id="99201-101">Som programvaruutvecklare på ditt företag har du möjligheten att utveckla dina kunskaper och bli en del av det interna AI-teamet.</span><span class="sxs-lookup"><span data-stu-id="99201-101">As a software developer at your company, you've the opportunity to grow your skills and become part of the in-house AI team.</span></span> <span data-ttu-id="99201-102">Medan du arbetar med den här nya, spännande rollen behöver du fortfarande sköta ditt vanliga jobb.</span><span class="sxs-lookup"><span data-stu-id="99201-102">While you ramp-up on this exciting new role, you still have your day job to do.</span></span> <span data-ttu-id="99201-103">Den seniora AI-ingenjören i teamet har berättat för dig om några användbara Jupyter-notebooks som har PyTorch-baserade labbar för att träna en modell för klassificering av bilder.</span><span class="sxs-lookup"><span data-stu-id="99201-103">The senior AI engineer on your team has told you about some useful Jupyter notebooks that have PyTorch-based labs to train an image classification model.</span></span> <span data-ttu-id="99201-104">Det låter spännande, men du vill inte installera en uppsättning ramverk på din utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="99201-104">Exciting stuff, but you don't want to install a set of frameworks onto your dev rig.</span></span> <span data-ttu-id="99201-105">I stället skapar du en virtuell dator baserat på avbildningen DSVM-avbildningen (Data Science Virtual Machine).</span><span class="sxs-lookup"><span data-stu-id="99201-105">Instead, you decide to create a virtual machine based on the Data Science Virtual Machine (DSVM) image.</span></span> 

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="99201-106">Vad är Azure CLI</span><span class="sxs-lookup"><span data-stu-id="99201-106">What is the Azure CLI</span></span>

<span data-ttu-id="99201-107">Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="99201-107">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources.</span></span> <span data-ttu-id="99201-108">Det finns för macOS, Linux och Windows eller i webbläsaren med [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="99201-108">It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span> <span data-ttu-id="99201-109">Vi har fullständig information om användning av det här verktyget i modulen **Kontrollera Azure-tjänster med CLI**.</span><span class="sxs-lookup"><span data-stu-id="99201-109">We have complete coverage of using this tool in the **Control Azure services with the CLI** module.</span></span>

## <a name="managing-deployments"></a><span data-ttu-id="99201-110">Hantera distributioner</span><span class="sxs-lookup"><span data-stu-id="99201-110">Managing deployments</span></span>

<span data-ttu-id="99201-111">Azure CLI innehåller kommandot `az group deployment` för att hantera Azure Resource Manager-distributioner.</span><span class="sxs-lookup"><span data-stu-id="99201-111">The Azure CLI includes the `az group deployment` command to manage Azure Resource Manager deployments.</span></span> <span data-ttu-id="99201-112">Det finns flera underkommandon för att utföra specifika aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="99201-112">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="99201-113">De vanligaste är:</span><span class="sxs-lookup"><span data-stu-id="99201-113">The most common include:</span></span>

| <span data-ttu-id="99201-114">Underkommando</span><span class="sxs-lookup"><span data-stu-id="99201-114">Subcommand</span></span> | <span data-ttu-id="99201-115">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="99201-115">Description</span></span> |
|-------------|-------------|
| `create` | <span data-ttu-id="99201-116">Starta en distribution.</span><span class="sxs-lookup"><span data-stu-id="99201-116">Start a deployment.</span></span> |
| `list` | <span data-ttu-id="99201-117">Hämta alla distributioner för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="99201-117">Get all the deployments for a resource group.</span></span> |
| `export` | <span data-ttu-id="99201-118">Exportera den mall som används för en distribution.</span><span class="sxs-lookup"><span data-stu-id="99201-118">Export the template used for a deployment.</span></span> |

<span data-ttu-id="99201-119">En fullständig lista över tillgängliga distributionskommandon finns i [referensen för kommandon för az-gruppdistribution](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)</span><span class="sxs-lookup"><span data-stu-id="99201-119">For a complete list of available deployment commands, see the [az group deployment command reference](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)</span></span>

<span data-ttu-id="99201-120">Vi använder `az group deployment create` för att etablera vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="99201-120">We'll use `az group deployment create` to provision our virtual machine.</span></span>

## <a name="create-a-json-deployment-parameters-file"></a><span data-ttu-id="99201-121">Skapa en fil med JSON-distributionsparametrar</span><span class="sxs-lookup"><span data-stu-id="99201-121">Create a JSON deployment parameters file</span></span>

<span data-ttu-id="99201-122">Vi skapar vår virtuella dator med hjälp av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="99201-122">We're going to create our VM using an Azure Resource Manager template.</span></span> <span data-ttu-id="99201-123">Mallen definierar den Linux DSVM-avbildning som vi vill etablera.</span><span class="sxs-lookup"><span data-stu-id="99201-123">The template defines the Linux DSVM image we want to provision.</span></span> <span data-ttu-id="99201-124">Vi behöver ange vissa parametrar i mallen, till exempel den VM-storlek som ska användas, administratörens användarnamn och lösenord samt datorns värdnamn.</span><span class="sxs-lookup"><span data-stu-id="99201-124">We need to supply some parameters to the template, such as the VM size we want to use, the admin username and password, and the host name of our machine.</span></span> 

1. <span data-ttu-id="99201-125">Kör följande kommando i Azure Cloud Shell till höger om den här enheten:</span><span class="sxs-lookup"><span data-stu-id="99201-125">Execute the following command in Azure Cloud Shell to the right of this unit:</span></span>

    ```bash
    code parameter_file.json
    ```
    <span data-ttu-id="99201-126"><!-- TODO add a link to official doc that explains the built-in editor when it becomes available --> Det här kommandot öppnar en tom fil som heter `parameter_file.json` i den inbyggda redigeraren.</span><span class="sxs-lookup"><span data-stu-id="99201-126"><!-- TODO add a link to official doc that explains the built-in editor when it becomes available --> This command opens and empty file called `parameter_file.json` in the built-in editor.</span></span> 

1. <span data-ttu-id="99201-127">Klistra in följande JSON-kodavsnitt i den tomma filen i kodredigeraren.</span><span class="sxs-lookup"><span data-stu-id="99201-127">Paste the following JSON snippet into the empty file into the code editor.</span></span>

    ```json
    { 
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "adminUsername": { "value" : "<USERNAME>"},
         "adminPassword": { "value" : "<PASSWORD>"},
         "vmName": { "value" : "<HOSTNAME>"},
         "vmSize": { "value" : "Standard_DS2_v2"},
      }
    }
    ```

1. <span data-ttu-id="99201-128">Uppdatera följande parametrar i det JSON som du klistrade in i redigeraren:</span><span class="sxs-lookup"><span data-stu-id="99201-128">Update the following parameters in the JSON you pasted in the editor:</span></span>

    |<span data-ttu-id="99201-129">Parameter</span><span class="sxs-lookup"><span data-stu-id="99201-129">Parameter</span></span>  |<span data-ttu-id="99201-130">Aktuellt värde</span><span class="sxs-lookup"><span data-stu-id="99201-130">Current value</span></span>  |<span data-ttu-id="99201-131">Ditt värde</span><span class="sxs-lookup"><span data-stu-id="99201-131">Your value</span></span>  |
    |---------|---------|---------|
    |<span data-ttu-id="99201-132">adminUsername</span><span class="sxs-lookup"><span data-stu-id="99201-132">adminUsername</span></span>     |  `<USERNAME>`       |    <span data-ttu-id="99201-133">Välj ett namn för administratörsanvändaren för den nya datorn, t.ex. *azuser*.</span><span class="sxs-lookup"><span data-stu-id="99201-133">Choose a name for the admin user of this new machine, such as, *azuser*.</span></span>     |
    |<span data-ttu-id="99201-134">adminPassword</span><span class="sxs-lookup"><span data-stu-id="99201-134">adminPassword</span></span>     |  `<PASSWORD>`       |   <span data-ttu-id="99201-135">Välj ett lösenord för det här administratörsanvändarkontot.</span><span class="sxs-lookup"><span data-stu-id="99201-135">Choose a password for this admin user account.</span></span> <span data-ttu-id="99201-136">Mer information om lösenordskrav finns i avsnittet med [vanliga frågor och svar om virtuella Linux-datorer](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)</span><span class="sxs-lookup"><span data-stu-id="99201-136">To learn more about password requirements, see [Frequently asked question about Linux Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)</span></span>     |
    |<span data-ttu-id="99201-137">vmName</span><span class="sxs-lookup"><span data-stu-id="99201-137">vmName</span></span>     |   `<HOSTNAME>`      |  <span data-ttu-id="99201-138">Välj ett namn för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99201-138">Choose a name for the new virtual machine.</span></span> <span data-ttu-id="99201-139">Namnet måste börja med en bokstav och får bara innehålla gemener och siffror.</span><span class="sxs-lookup"><span data-stu-id="99201-139">Your name must begin with a letter and contain only lowercase letters and numbers.</span></span> <span data-ttu-id="99201-140">Prova ett unikt namn, till exempel ett som innehåller dina initialer och ditt födelseår.</span><span class="sxs-lookup"><span data-stu-id="99201-140">Try to choose a unique name, such as one that includes your initials and your birth year.</span></span> |
    |<span data-ttu-id="99201-141">vmSize</span><span class="sxs-lookup"><span data-stu-id="99201-141">vmSize</span></span>     |  <span data-ttu-id="99201-142">Standard_DS2_v2</span><span class="sxs-lookup"><span data-stu-id="99201-142">Standard_DS2_v2</span></span>       |  <span data-ttu-id="99201-143">Den här VM-storleken fungerar bra för den här övningen, men du kan ändra den om du vill.</span><span class="sxs-lookup"><span data-stu-id="99201-143">This VM size will work fine for this exercise, but you are free to change it.</span></span> <span data-ttu-id="99201-144">En lista över tillgängliga VM-storlekar finns i avsnittet om [storlekar för Linux-datorer i Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)</span><span class="sxs-lookup"><span data-stu-id="99201-144">A list of available vm sizes can be found here [Sizes for Linux virtual machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)</span></span>       |

1. <span data-ttu-id="99201-145">Spara dina ändringar i `parameter_file.json` och stäng textredigeraren.</span><span class="sxs-lookup"><span data-stu-id="99201-145">Save your changes in `parameter_file.json` and close the text editor.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="99201-146">Kom ihåg de värden som du valde för adminUsername, adminPassword och vmName.</span><span class="sxs-lookup"><span data-stu-id="99201-146">Remember the values you chose for adminUsername, adminPassword and vmName.</span></span> <span data-ttu-id="99201-147">Vi använder dem igen senare i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="99201-147">We'll use them again in this exercise.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="99201-148">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="99201-148">Create a resource group</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="99201-149">Normalt skulle du skapa en resursgrupp i en valfri region.</span><span class="sxs-lookup"><span data-stu-id="99201-149">Normally you'd create a resource group in a region of your choice.</span></span> <span data-ttu-id="99201-150">Dock tillhandahålls en resursgrupp som du kan använda av den sandbox-session som du är i för närvarande.</span><span class="sxs-lookup"><span data-stu-id="99201-150">However, the sandbox session you are currently in supplies a resource group for you to use.</span></span> <span data-ttu-id="99201-151">Din resursgrupp för den här sessionen är **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="99201-151">Your resource group for this session is **<rgn>[sandbox resource group name]</rgn>**.</span></span>

## <a name="deploy-the-dsvm-to-your-resource-group"></a><span data-ttu-id="99201-152">Distribuera DSVM till resursgruppen</span><span class="sxs-lookup"><span data-stu-id="99201-152">Deploy the DSVM to your resource group</span></span>

<span data-ttu-id="99201-153">Vi nu har en resursgrupp och har definierat parametrar för DSVM Resource Manager-mallen i en fil med namnet `parameter_file.json`.</span><span class="sxs-lookup"><span data-stu-id="99201-153">We now have a resource group and have defined parameters for the DSVM Resource Manager template in a file called `parameter_file.json`.</span></span> <span data-ttu-id="99201-154">Vi kör `az group deployment create` för att etablera vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="99201-154">We'll run the `az group deployment create` next to provision our virtual machine.</span></span>

1. <span data-ttu-id="99201-155">Kör följande kommando i Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="99201-155">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    az group deployment create \
    --resource-group  <rgn>[sandbox resource group name]</rgn> \
    --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json \
    --parameters parameter_file.json
    ```

    <span data-ttu-id="99201-156">Kommandot använder Resource Manager-mallen och våra parametrar för att skapa den virtuella datorn i vår resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="99201-156">The command uses the Resource Manager template and our parameters to create the virtual machine in our resource group.</span></span> 

2. <span data-ttu-id="99201-157">Det tar några minuter att distribuera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99201-157">Deploying a virtual machine takes a few minutes to complete.</span></span> <span data-ttu-id="99201-158">Konsolen visar ` - Running ..` och inte mycket annat förrän åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="99201-158">The console displays ` - Running ..` and not much else until the operation completes.</span></span> <span data-ttu-id="99201-159">När åtgärden har slutförts matas ett JSON-svar ut till skärmen.</span><span class="sxs-lookup"><span data-stu-id="99201-159">When the operation finishes, a JSON response is output to the screen.</span></span> <span data-ttu-id="99201-160">Rulla ned till slutet av JSON och kontrollera att fältet **"provisioningState"** har värdet *Succeeded* (Lyckades).</span><span class="sxs-lookup"><span data-stu-id="99201-160">Scroll to the bottom of the JSON and check that the field **"provisioningState"** has the value *Succeeded*.</span></span>

    > [!TIP]
    > <span data-ttu-id="99201-161">Om du får ett felmeddelande om att DNS-posten redan används av en annan offentlig IP-adress provar du att ändra **vmName** i `parameter_file.json` till ett annat namn som är unikt.</span><span class="sxs-lookup"><span data-stu-id="99201-161">If you get an error stating that the DNS record is already used by another public IP, try changing **vmName** in `parameter_file.json` to another name that's unique.</span></span>

3. <span data-ttu-id="99201-162">Kör följande kommando för att få information om den virtuella datorn. Ersätt `<HOSTNAME>` med det värdnamn som du har definierat för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99201-162">Execute the following command to get information about the VM, replacing `<HOSTNAME>` with the host name you defined for your VM.</span></span>

    ```azurecli
    az vm get-instance-view \
    --name <HOSTNAME> \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query instanceView.statuses[1] \
    --output table
    ```

    <span data-ttu-id="99201-163">Det här kommandot visar status för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99201-163">This command displays the status of the VM.</span></span> <span data-ttu-id="99201-164">Det ska stå *VM running* (Den virtuella dator körs).</span><span class="sxs-lookup"><span data-stu-id="99201-164">It should say *VM running*.</span></span>

<span data-ttu-id="99201-165">Grattis!</span><span class="sxs-lookup"><span data-stu-id="99201-165">Congratulations!</span></span> <span data-ttu-id="99201-166">Du har skapat och distribuerat en virtuell Linux-dator baserat på DSVM-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="99201-166">You've created and deployed a Linux VM based on the DSVM image.</span></span>

## <a name="open-the-vm-to-ssh-traffic-on-port-22"></a><span data-ttu-id="99201-167">Öppna den virtuella datorn för att SSH trafik på port 22</span><span class="sxs-lookup"><span data-stu-id="99201-167">Open the VM to ssh traffic on port 22</span></span>

<span data-ttu-id="99201-168">Som standard har vår virtuella dator inte några portar öppna.</span><span class="sxs-lookup"><span data-stu-id="99201-168">By default, our VM doesn't have any ports open.</span></span> <span data-ttu-id="99201-169">Vårt mål är att fjärransluta, starta en Jupyter Notebook-server och köra andra lokala kommandon på datorn.</span><span class="sxs-lookup"><span data-stu-id="99201-169">Our goal is to connect remotely, start a Jupyter Notebook server and run other local commands on the machine.</span></span> <span data-ttu-id="99201-170">För att kunna fjärransluta till den virtuella datorn med SSH-protokollet (Secure Shell) behöver vi öppna en port.</span><span class="sxs-lookup"><span data-stu-id="99201-170">To remote into the VM using the Secure Shell (SSH) protocol, we need to open a port.</span></span> <span data-ttu-id="99201-171">Post 22 är standardporten för SSH.</span><span class="sxs-lookup"><span data-stu-id="99201-171">Port 22 is the default port for ssh.</span></span>  

1. <span data-ttu-id="99201-172">Kör följande kommando i Azure Cloud Shell. Ersätt `<HOSTNAME>` med det namn som du gav den virtuella DSV-datorn under konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="99201-172">Execute the following command in Azure Cloud Shell, replacing `<HOSTNAME>` wit the name you gave your DSV virtual machine during setup.</span></span> 

    ```azurecli
    az vm open-port \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 22 \
    --priority 900
    ```

<span data-ttu-id="99201-173">Kommandot kan ta upp till en minut att slutföra.</span><span class="sxs-lookup"><span data-stu-id="99201-173">This command can take up to a minute to complete.</span></span> <span data-ttu-id="99201-174">När kommandot är klart returnerar det ett JSON-svar till kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="99201-174">When the command finishes, it returns a JSON response to the command line.</span></span> <span data-ttu-id="99201-175">Kontrollera att fältet **"provisioningState"** har värdet *Succeeded* (Lyckades).</span><span class="sxs-lookup"><span data-stu-id="99201-175">Check that the field **"provisioningState"** has the value *Succeeded*.</span></span> <span data-ttu-id="99201-176">Vi kommer att testa om SSH fungerar korrekt, men först öppnar vi en till port.</span><span class="sxs-lookup"><span data-stu-id="99201-176">We'll test that ssh works shortly, but first let's open one more port.</span></span>

## <a name="open-the-vm-to-access-the-jupyter-notebook-server-remotely"></a><span data-ttu-id="99201-177">Öppna den virtuella datorn för att fjärransluta till Jupyter Notebook-servern</span><span class="sxs-lookup"><span data-stu-id="99201-177">Open the VM to access the Jupyter Notebook server remotely</span></span>

<span data-ttu-id="99201-178">Som tidigare nämnts levereras DSVM-avbildningen med programvara, verktyg och exempel förinstallerat som stöder dina projekt om dataforskning, maskininlärning och djupinlärning.</span><span class="sxs-lookup"><span data-stu-id="99201-178">As mentioned previously, the DSVM image comes pre-installed with software, tools, and samples to help you with your data science, machine learning, and deep learning projects.</span></span> <span data-ttu-id="99201-179">Jupyter installeras i avbildningen tillsammans med exempelnotebooks.</span><span class="sxs-lookup"><span data-stu-id="99201-179">Jupyter is installed in the image, along with sample notebooks.</span></span> <span data-ttu-id="99201-180">Vi vill starta en Jupyter Notebook-server på den virtuella datorn och sedan fjärransluta till notebook-servern från våra lokala dator.</span><span class="sxs-lookup"><span data-stu-id="99201-180">We want to start a Jupyter notebook server on the VM and then remotely connect to the notebook server from our local machine.</span></span> <span data-ttu-id="99201-181">Som standard körs notebook-servern på port 8888.</span><span class="sxs-lookup"><span data-stu-id="99201-181">By default, the notebook server runs on port 8888.</span></span> <span data-ttu-id="99201-182">För att fjärransluta till servern behöver vi öppna den porten på vår virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="99201-182">To access the server remotely, we need to open that port on our VM.</span></span> 

1. <span data-ttu-id="99201-183">Kör följande kommando i Azure Cloud Shell. Ersätt `<HOSTNAME>` med det namn som du gav den virtuella DSVM-datorn under konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="99201-183">Execute the following command in Azure Cloud Shell, replacing `<HOSTNAME>` with the name you gave your DSVM virtual machine during setup.</span></span>

    ```azurecli
    az vm open-port \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 8888 \
    --priority 901
    ```

<span data-ttu-id="99201-184">Kommandot kan återigen ta upp till en minut att slutföra.</span><span class="sxs-lookup"><span data-stu-id="99201-184">Again, this command can take up to a minute to complete.</span></span> <span data-ttu-id="99201-185">När kommandot är klart returnerar det ett JSON-svar till kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="99201-185">When the command finishes, it returns a JSON response to the command line.</span></span> <span data-ttu-id="99201-186">Kontrollera att fältet **"provisioningState"** har värdet *Succeeded* (Lyckades).</span><span class="sxs-lookup"><span data-stu-id="99201-186">Check that the field **"provisioningState"** has the value *Succeeded*.</span></span>  

## <a name="connect-to-the-vm-with-secure-shell-ssh"></a><span data-ttu-id="99201-187">Ansluta till den virtuella datorn med SSH (Secure Shell)</span><span class="sxs-lookup"><span data-stu-id="99201-187">Connect to the VM with Secure Shell (ssh)</span></span>

1. <span data-ttu-id="99201-188">Kör följande kommando i Azure Cloud Shell för att hitta den virtuella datorns offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="99201-188">Execute the following command in Azure Cloud Shell to find the public IP address of the VM.</span></span> <span data-ttu-id="99201-189">Ersätt `<HOSTNAME>` med det namn som du gav din virtuella DSVM-dator under konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="99201-189">Replace `<HOSTNAME>` with the name you gave your DSVM virtual machine during setup.</span></span>

    ```azurecli
    az vm list-ip-addresses \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --output table
    ```

1. <span data-ttu-id="99201-190">Kör följande kommando i Cloud Shell för att logga in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99201-190">Execute the following command in the Cloud Shell to sign into the VM.</span></span> <span data-ttu-id="99201-191">Ersätt `<USERNAME>` med det användarnamn som du valde i början av den här övningen.</span><span class="sxs-lookup"><span data-stu-id="99201-191">Replace `<USERNAME>` with the username you chose at the start of this exercise.</span></span> <span data-ttu-id="99201-192">Ersätt `<IP>` med värdet från kolumnen **PublicIPAddresses** i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="99201-192">Replace `<IP>`  with the value from the **PublicIPAddresses** column of the previous step.</span></span>

    <span data-ttu-id="99201-193">Om användarnamnet som du valde till exempel var *azuser* och PublicIPAddresses hade värdet 33.165.103.23 skulle det kommandot se ut på följande vis:</span><span class="sxs-lookup"><span data-stu-id="99201-193">For example, if the username you chose was *azuser* and the PublicIPAddresses had a value of 33.165.103.23, then this command would read:</span></span>
    
    `ssh azuser@33.165.103.23`
    
    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 

1. <span data-ttu-id="99201-194">När du uppmanas anger du lösenordet för den administratörsanvändare som du valde i början av den här övningen.</span><span class="sxs-lookup"><span data-stu-id="99201-194">When prompted, enter the password for the admin user you chose at the start of this exercise.</span></span> <span data-ttu-id="99201-195">När du har loggat in bör uppmaningen ändras till formatet `username@hostname`, till exempel `azuser@js1982`.</span><span class="sxs-lookup"><span data-stu-id="99201-195">When you've signed in successfully, your prompt should change to the format `username@hostname`, for example, `azuser@js1982`.</span></span>

<span data-ttu-id="99201-196">Nästa steg är att starta Jupyter Notebook-servern på vår virtuella dator och öppna en notebook via en fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="99201-196">The next step is to start the Jupyter notebook server on our VM and open a notebook remotely.</span></span>

## <a name="start-the-jupyter-notebook-server-on-the-vm"></a><span data-ttu-id="99201-197">Starta Jupyter Notebook-servern på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="99201-197">Start the Jupyter notebook server on the VM</span></span>

<span data-ttu-id="99201-198">Det finns en uppsättning notebooks i mappen `~/notebooks` i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99201-198">There's a set of notebooks in the `~/notebooks` folder of your VM.</span></span> <span data-ttu-id="99201-199">Om vi antar att du fortfarande är inloggad via en SSH-session startar du notebook-servern och öppnar en av dessa notebooks för att kontrollera att allt fungerar.</span><span class="sxs-lookup"><span data-stu-id="99201-199">Assuming you are still logged in through an SSH session,  start the notebook server and open one of these notebooks to make sure everything is working.</span></span>


1. <span data-ttu-id="99201-200">Kör följande kommando i kommandotolken i den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="99201-200">Run the following command at the command prompt of your VM:</span></span>

    ```bash
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root
    ```

> [!CAUTION]
> <span data-ttu-id="99201-201">Åtkomst till notebook-servern i den här övningen sker via `http://`.</span><span class="sxs-lookup"><span data-stu-id="99201-201">Access to the notebook server in this exercise happens over `http://`.</span></span> <span data-ttu-id="99201-202">Om du vill köra en notebook-server offentligt bör du skydda den.</span><span class="sxs-lookup"><span data-stu-id="99201-202">If you want to run a notebook server in public, you should secure it.</span></span> <span data-ttu-id="99201-203">Mer information om hur du skyddar en notebook-server finns i den officiella Jupyter-dokumentationen på webben.</span><span class="sxs-lookup"><span data-stu-id="99201-203">For more information about securing a notebook server, see the official Jupyter documentation online.</span></span> 

<span data-ttu-id="99201-204">I det föregående kommandot startar vi Jupyter Notebook-server med kommandot `jupyter notebook`.</span><span class="sxs-lookup"><span data-stu-id="99201-204">In the preceding command, we start the Jupyter Notebook server with the `jupyter notebook` command.</span></span> <span data-ttu-id="99201-205">Vi ange tre viktiga kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="99201-205">We supply three important command-line arguments.</span></span> <span data-ttu-id="99201-206">Kom ihåg att vi är fjärrinloggade i den här datorn via en konsol.</span><span class="sxs-lookup"><span data-stu-id="99201-206">Remember, we're logged into this machine remotely through a console.</span></span> <span data-ttu-id="99201-207">Notebooks hanteras i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="99201-207">Notebooks are served in a browser.</span></span> 

 - <span data-ttu-id="99201-208">`--ip=0.0.0.0` Som standard körs en notebook-server lokalt på 127.0.0.1:8888 och är enbart tillgänglig från localhost.</span><span class="sxs-lookup"><span data-stu-id="99201-208">`--ip=0.0.0.0` By default, a notebook server runs locally at 127.0.0.1:8888 and is accessible only from localhost.</span></span> <span data-ttu-id="99201-209">Du kan komma åt notebook-servern från webbläsaren lokalt med hjälp av http://127.0.0.1:8888.</span><span class="sxs-lookup"><span data-stu-id="99201-209">You can access the notebook server from the browser locally using http://127.0.0.1:8888.</span></span> <span data-ttu-id="99201-210">När IP-adressen anges till 0.0.0.0 instrueras servern att lyssna efter trafik på alla IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="99201-210">Setting the IP Address to 0.0.0.0 tells the server to listen for traffic on all IPs.</span></span> <span data-ttu-id="99201-211">Om notebook-servern lyssnar på 0.0.0.0 kan den nås via värddatorns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="99201-211">If the notebook server listens on 0.0.0.0, it will be reachable through the IP address of the host machine.</span></span>  
 - <span data-ttu-id="99201-212">`--no-browser`  Eftersom vi vill ansluta till notebook från en annan dator via Internet konfigurerar vi notebook-servern till att inte öppna webbläsaren, vilket är standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="99201-212">`--no-browser`  Because we want to connect to the notebook from another computer over the internet, we configure the notebook server to not open the browser, which is the default behavior.</span></span> 
 - <span data-ttu-id="99201-213">`--allow-root`  I den här övningen har vi bara ett administratörskonto på den virtuella datorn, så vi vill kunna köra notebooks som rot.</span><span class="sxs-lookup"><span data-stu-id="99201-213">`--allow-root`  In this exercise, we only have an admin account on the VM, so  we want to be able to run notebooks as root.</span></span>

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a><span data-ttu-id="99201-214">Ansluta till Jupyter Notebook-servern från en fjärransluten webbläsare</span><span class="sxs-lookup"><span data-stu-id="99201-214">Connect to the Jupyter Notebook server from a remote browser</span></span>

<span data-ttu-id="99201-215">När ovanstående kommando körs på den virtuella datorn startas notebook-servern och konsolen visar en URL som du kan använda i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="99201-215">When the above command runs on the VM, the notebook server starts and the console displays a URL for you to use in a browser.</span></span> 

![Skärmbild av det meddelande som en notebook-server som körs visar i konsolen med URL:en för att komma åt servern från värddatorn](../media/notebook-url.png)

1. <span data-ttu-id="99201-217">Kopiera den URL som notebook-servern visar i adressfältet i din favoritwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="99201-217">Copy the URL the notebook server displays into the address bar of your favorite browser.</span></span> <span data-ttu-id="99201-218">Du kan även klicka på URL:en för att öppna i standardwebbläsaren.</span><span class="sxs-lookup"><span data-stu-id="99201-218">You can also click on the URL to open in your default browser.</span></span> 

    <span data-ttu-id="99201-219">Du får ett meddelande om att webbplatsen inte kan nås eftersom den URL som du fick är anslutningen till notebook-servern från värddatorn.</span><span class="sxs-lookup"><span data-stu-id="99201-219">You will receive a "Site can't be reached" message because the URL you were given is the connection to the notebook server from the host machine.</span></span>

1. <span data-ttu-id="99201-220">För att fjärransluta till servern ersätter du värdnamnet i URL:en med IP-adressen för den virtuella dator som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="99201-220">To access the server remotely,  replace the hostname in the URL with the IP address of the VM you saved earlier.</span></span> 

    <span data-ttu-id="99201-221">Här är en exempel-URL för en notebook-server.</span><span class="sxs-lookup"><span data-stu-id="99201-221">Here's a sample notebook server URL.</span></span>

    <span data-ttu-id="99201-222">"http://**ab-dsvm-4**:8888/?token={some token}"</span><span class="sxs-lookup"><span data-stu-id="99201-222">"http://**ab-dsvm-4**:8888/?token={some token}"</span></span>

    <span data-ttu-id="99201-223">I det här ersätter vi **ab-dsvm-4** med datorns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="99201-223">In this case, we would replace **ab-dsvm-4** with IP address of the machine.</span></span> <span data-ttu-id="99201-224">Om vår IP-adress är `52.175.199.43` blir URL:en:</span><span class="sxs-lookup"><span data-stu-id="99201-224">If our IP address is `52.175.199.43`, then the URL becomes:</span></span>

    <span data-ttu-id="99201-225">"http://**52.175.199.43**:8888/?token={some token}"</span><span class="sxs-lookup"><span data-stu-id="99201-225">"http://**52.175.199.43**:8888/?token={some token}"</span></span>

    <span data-ttu-id="99201-226">Se till att `:8888`, portadressen, behålls i URL:en.</span><span class="sxs-lookup"><span data-stu-id="99201-226">Make sure `:8888`, the port address, is kept in the URL.</span></span>

    > [!TIP]
    > <span data-ttu-id="99201-227">Om du inte vill använda IP-adressen kan du även använda det fullständigt kvalificerade namnet för servern, som är i formatet `<HOST NAME>.<REGION>.cloudapp.azure.com`</span><span class="sxs-lookup"><span data-stu-id="99201-227">If you don't want to use the IP address, you can also use the fully qualified name of your server which is in the form `<HOST NAME>.<REGION>.cloudapp.azure.com`</span></span>

    <span data-ttu-id="99201-228">Följande skärmbild visar hur Jupyter-instrumentpanelen ser ut i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="99201-228">The following  screenshot shows what the Jupyter dashboard looks like in your browser.</span></span>

    ![<span data-ttu-id="99201-229">Skärmbild som visar instrumentpanelen för Jupyter Notebooks.</span><span class="sxs-lookup"><span data-stu-id="99201-229">Screenshot showing Jupyter Notebooks dashboard.</span></span> ](../media/jupyter-in-browser.png)

1. <span data-ttu-id="99201-230">Gå till **notebooks/IntroToJupyterPython.ipynb** och välj den.</span><span class="sxs-lookup"><span data-stu-id="99201-230">Navigate to **notebooks/IntroToJupyterPython.ipynb** and select it.</span></span> <span data-ttu-id="99201-231">Prova den här notebook för att kontroller att allt fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="99201-231">Try out this notebook to verify everythin  works as expected.</span></span>

    <span data-ttu-id="99201-232">Grattis!</span><span class="sxs-lookup"><span data-stu-id="99201-232">Congratulations!</span></span> <span data-ttu-id="99201-233">Du har nu en DSVM-baserad virtuell dator som körs och kan arbeta fjärranslutet med Jupyter.</span><span class="sxs-lookup"><span data-stu-id="99201-233">You now have a running DSVM-based virtual machine running and can work remotely with Jupyter.</span></span> <span data-ttu-id="99201-234">I den här övningen kör vi den programvara som installerades på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99201-234">In this exercise, we're running the software that was installed on the VM.</span></span> <span data-ttu-id="99201-235">I nästa övning isolerar vi programvaran i en container på den virtuella datorn så att vi tryggt kan experimentera.</span><span class="sxs-lookup"><span data-stu-id="99201-235">In the next exercise, we'll isolate the software in a container on the VM so we can experiment with confidence.</span></span>

4. <span data-ttu-id="99201-236">När du är klar med notebook kan du stoppa Jupyter-servern med `Control-C` i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="99201-236">When you have finished with the notebook, you can stop the Jupyter server with `Control-C` in the Cloud Shell.</span></span>
