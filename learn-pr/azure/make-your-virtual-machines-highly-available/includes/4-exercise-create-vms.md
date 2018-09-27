<span data-ttu-id="25f60-101">Vi har en lastbalanserare för att ta emot inkommande Internettrafik och dirigera den. Nu behöver vi några virtuella Azure-datorer att dirigera trafik till.</span><span class="sxs-lookup"><span data-stu-id="25f60-101">We have a load balancer to take inbound Internet traffic and route it, now we need some Azure VMs to route traffic to.</span></span> <span data-ttu-id="25f60-102">Vi använder Linux för det här exemplet, men exakt samma teknik skulle fungera med alla virtuella datoravbildningar.</span><span class="sxs-lookup"><span data-stu-id="25f60-102">We'll use Linux for this example, but the exact same technique would work with any virtual machine image.</span></span>

## <a name="create-a-set-of-vms"></a><span data-ttu-id="25f60-103">Skapa en uppsättning virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="25f60-103">Create a set of VMs</span></span>

<span data-ttu-id="25f60-104">Vi ska skapa tre virtuella datorer och gruppera dem i en tillgänglighetsuppsättning med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="25f60-104">We're going to create three VMs and group them into an availability set with the Azure CLI.</span></span> <span data-ttu-id="25f60-105">Man kan använda portalen för den här uppgiften, men eftersom vi ska skapa flera resurser är det mycket enklare att göra detta via ett skriptverktyg.</span><span class="sxs-lookup"><span data-stu-id="25f60-105">We could have used the portal for this task - but because we're creating multiple resources, it's much easier to do this through a scripting tool.</span></span>

<span data-ttu-id="25f60-106">En alternativ metod om du _verkligen_ föredrar att använda Azure-portalen är att använda en _mall_.</span><span class="sxs-lookup"><span data-stu-id="25f60-106">An alternative approach if you _really_ prefer to use the Azure portal would be to use a _template_.</span></span> <span data-ttu-id="25f60-107">Det här är JSON-instruktioner som kan användas för att distribuera resurser.</span><span class="sxs-lookup"><span data-stu-id="25f60-107">These are JSON instructions which can be used to deploy resources.</span></span> <span data-ttu-id="25f60-108">Vi kan definiera en mall för den virtuella datorn och sedan distribuera mallen flera gånger i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="25f60-108">We could define a template for the virtual machine and then deploy the template multiple times in the Azure portal.</span></span>

### <a name="create-some-azure-cli-defaults"></a><span data-ttu-id="25f60-109">Skapa några standardvärden för Azure CLI</span><span class="sxs-lookup"><span data-stu-id="25f60-109">Create some Azure CLI defaults</span></span>

1. <span data-ttu-id="25f60-110">Börja med att ange några standardinställningar i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="25f60-110">Start by setting some defaults in the Azure CLI.</span></span> <span data-ttu-id="25f60-111">Alla kommandon som vi använder kräver en _resursgrupp_ och en valfri _plats_.</span><span class="sxs-lookup"><span data-stu-id="25f60-111">Every command we use requires a _resource group_ and an optional _location_.</span></span> <span data-ttu-id="25f60-112">I stället för att skriva det varje gång kan vi ställa in det som en standardparameter.</span><span class="sxs-lookup"><span data-stu-id="25f60-112">Rather than typing that every time, we can set it as a default parameter.</span></span> <span data-ttu-id="25f60-113">Använd den färdiga Azure sandbox-resursgruppen och samma plats som du har valt för Azure-lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="25f60-113">Use the pre-created Azure sandbox resource group, and the same location you selected for the Azure Load Balancer.</span></span> <span data-ttu-id="25f60-114">Här är en påminnelse om de tillgängliga platserna:</span><span class="sxs-lookup"><span data-stu-id="25f60-114">As a reminder, here are the available locations:</span></span>

    [!include[](../../../includes/azure-sandbox-regions-note.md)]

1. <span data-ttu-id="25f60-115">Skriv följande kommando i Cloud Shell till höger och ersätt `<location>` platshållare med platsen som du använde för Azure-lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="25f60-115">Type this command into the Cloud Shell on the right, replacing the `<location>` placeholder with the location you used for your Azure Load Balancer.</span></span>

    ```azurecli
    az configure --defaults group=<rgn>[sandbox Resource Group</rgn> location=<location>
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

### <a name="create-an-availability-set"></a><span data-ttu-id="25f60-116">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="25f60-116">Create an availability set</span></span>

<span data-ttu-id="25f60-117">Börja med att skapa en _tillgänglighetsuppsättning_.</span><span class="sxs-lookup"><span data-stu-id="25f60-117">Let's start by creating an _availability set_.</span></span>

<span data-ttu-id="25f60-118">Vi behöver en tillgänglighetsuppsättning att gruppera våra virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="25f60-118">We will need an availability set to group our virtual machines into.</span></span> <span data-ttu-id="25f60-119">Vi kan använda `vm availability-set` kommandot för att skapa en.</span><span class="sxs-lookup"><span data-stu-id="25f60-119">We can use the `vm availability-set` command to create one.</span></span> <span data-ttu-id="25f60-120">Den behöver bara ett namn så vi använder **woodgrove-avail-set**.</span><span class="sxs-lookup"><span data-stu-id="25f60-120">It just needs a name, we'll use **woodgrove-avail-set**.</span></span>

```azurecli
az vm availability-set create --name woodgrove-avail-set
```

### <a name="create-a-configuration-script"></a><span data-ttu-id="25f60-121">Skapa ett konfigurationsskript</span><span class="sxs-lookup"><span data-stu-id="25f60-121">Create a configuration script</span></span>

<span data-ttu-id="25f60-122">Vi vill använda samma grundläggande konfiguration för var och en av de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="25f60-122">We want to use the same basic configuration for each of the VMs.</span></span>

    - <span data-ttu-id="25f60-123">Ubuntu Linux</span><span class="sxs-lookup"><span data-stu-id="25f60-123">Ubuntu Linux</span></span>
    - <span data-ttu-id="25f60-124">nginx -webbserver</span><span class="sxs-lookup"><span data-stu-id="25f60-124">nginx web server</span></span>
    - <span data-ttu-id="25f60-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="25f60-125">Node.js</span></span>
    - <span data-ttu-id="25f60-126">Grundläggande webbplats med en enda sida för testning</span><span class="sxs-lookup"><span data-stu-id="25f60-126">Basic website with a single page for testing</span></span>

<span data-ttu-id="25f60-127">Valet av OS är baserat på den avbildning som vi väljer, men de andra konfigurationselementen kräver en del skript.</span><span class="sxs-lookup"><span data-stu-id="25f60-127">The choice of OS is based on the image we select, but the other configuration elements require some scripting.</span></span> <span data-ttu-id="25f60-128">Nu ska vi skapa en lokal fil i Cloud Shell för att installera en webbserver och konfigurera den med en enkel webbplats.</span><span class="sxs-lookup"><span data-stu-id="25f60-128">Let's create a local file in the Cloud Shell to install a web server and configure it with a basic site.</span></span>

1. <span data-ttu-id="25f60-129">Öppna Cloud Shell-redigeraren genom att skriva `code` i terminalen.</span><span class="sxs-lookup"><span data-stu-id="25f60-129">Open the Cloud Shell Editor by typing `code` in the terminal.</span></span>

    ```bash
    code
    ```

    <span data-ttu-id="25f60-130">Det här är en inbyggd redigerare som du kan använda för att skapa och redigera filer direkt i Cloud Shell-miljön.</span><span class="sxs-lookup"><span data-stu-id="25f60-130">This is a built-in editor you can use to create and edit files right in the Cloud Shell environment.</span></span> <span data-ttu-id="25f60-131">Filerna sparas i Azure i ett lagringskonto som skapades när du startade Cloud Shell-miljön.</span><span class="sxs-lookup"><span data-stu-id="25f60-131">The files are saved in Azure in a Storage account created when you launched the Cloud Shell environment.</span></span>

1. <span data-ttu-id="25f60-132">Kopiera följande skript och klistra in det i fönstret redigeraren.</span><span class="sxs-lookup"><span data-stu-id="25f60-132">Copy the following script and paste it into the editor window.</span></span>

    ```script
    #cloud-config
    package_upgrade: true
    packages:
      - nginx
      - nodejs
      - npm
    write_files:
      - owner: www-data:www-data
      - path: /etc/nginx/sites-available/default
        content: |
          server {
            listen 80;
            location / {
              proxy_pass http://localhost:3000;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection keep-alive;
              proxy_set_header Host $host;
              proxy_cache_bypass $http_upgrade;
            }
          }
      - owner: azureuser:azureuser
      - path: /home/azureuser/myapp/index.js
        content: |
          var express = require('express')
          var app = express()
          var os = require('os');
          app.get('/', function (req, res) {
            res.send('Hello World from host ' + os.hostname() + '!')
          })
          app.listen(3000, function () {
            console.log('Hello world app listening on port 3000!')
          })
    runcmd:
      - service nginx restart
      - cd "/home/azureuser/myapp"
      - npm init
      - npm install express -y
      - nodejs index.js
    ```

1. <span data-ttu-id="25f60-133">Spara filen och ge den namnet **cloud-init.txt**.</span><span class="sxs-lookup"><span data-stu-id="25f60-133">Save the file - give it the name **cloud-init.txt**.</span></span> <span data-ttu-id="25f60-134">Du kan använda snabbmenyn ”...” i det övre högra hörnet av redigeraren eller använda <kbd>Ctrl+S</kbd> i Windows och Linux, och <kbd>Cmd+S</kbd> på macOS.</span><span class="sxs-lookup"><span data-stu-id="25f60-134">You can use the "..." context menu on the top/right corner of the editor, or use <kbd>Ctrl+S</kbd> in Windows and Linux, and <kbd>Cmd+S</kbd> on macOS.</span></span>

1. <span data-ttu-id="25f60-135">Avsluta redigeraren – du kan återigen använda snabbmenyn ”...” eller ett kortkommando (<kbd>Ctrl+Q</kbd>).</span><span class="sxs-lookup"><span data-stu-id="25f60-135">Exit the editor - again you can use the "..." context menu, or an accelerator key (<kbd>Ctrl+Q</kbd>).</span></span>

1. <span data-ttu-id="25f60-136">Filen ska vara i filsystemet.</span><span class="sxs-lookup"><span data-stu-id="25f60-136">The file should be on the file system.</span></span> <span data-ttu-id="25f60-137">Försök att använda `ls` kommandot för att visa innehållet i mappen.</span><span class="sxs-lookup"><span data-stu-id="25f60-137">Try using the `ls` command to list the contents of the folder.</span></span> <span data-ttu-id="25f60-138">Du kan också `cat` filen för att kontrollera innehållet.</span><span class="sxs-lookup"><span data-stu-id="25f60-138">You can also `cat` the file to verify the contents.</span></span>

### <a name="create-the-virtual-machines"></a><span data-ttu-id="25f60-139">Skapa de virtuella datorerna</span><span class="sxs-lookup"><span data-stu-id="25f60-139">Create the virtual machines</span></span>

<span data-ttu-id="25f60-140">Nu ska vi skapa tre virtuella Ubuntu Linux-datorer med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="25f60-140">Next, let's create three Ubuntu Linux virtual machines with the Azure CLI.</span></span> <span data-ttu-id="25f60-141">Vi går inte igenom alternativen här, men om du vill veta mer om hantering av virtuella datorer med CLI kan du kolla in modulen **Hantera virtuella datorer med Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="25f60-141">We won't cover the options here, but if you are interested in learning more about VM management with the CLI, check out the **Manage virtual machines with the Azure CLI** module.</span></span>

<span data-ttu-id="25f60-142">Vi måste också skapa ett virtuellt nätverksgränssnitt för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="25f60-142">We also need to create a virtual network interface for each VM.</span></span> <span data-ttu-id="25f60-143">Vi kan använda en loop och skapa dem tillsammans.</span><span class="sxs-lookup"><span data-stu-id="25f60-143">We can use a loop and create them together.</span></span>

1. <span data-ttu-id="25f60-144">Använd `vm-create` kommandot för att skapa tre virtuella datorer i en loop.</span><span class="sxs-lookup"><span data-stu-id="25f60-144">Use the `vm-create` command to create three virtual machines in a loop.</span></span> <span data-ttu-id="25f60-145">Du kan använda följande kod för att:</span><span class="sxs-lookup"><span data-stu-id="25f60-145">You can use the following code to:</span></span>
    - <span data-ttu-id="25f60-146">Skapa ett nätverkskort med namnet _woodgrove_NicX_ där X är [1,2,3].</span><span class="sxs-lookup"><span data-stu-id="25f60-146">Create a NIC named _woodgrove_NicX_ where X is [1,2,3].</span></span>
    - <span data-ttu-id="25f60-147">Skapa en virtuell dator med namnet _woodgrove-VMX_ där X är [1,2,3].</span><span class="sxs-lookup"><span data-stu-id="25f60-147">Create a VM named _woodgrove-VMX_ where X is [1,2,3].</span></span>
    - <span data-ttu-id="25f60-148">Associera varje nätverkskort till det skapade virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="25f60-148">Associate each NIC to the created VNET</span></span>
    - <span data-ttu-id="25f60-149">Associera varje virtuell dator till uppsättningen med nätverkskort och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="25f60-149">Associate each VM to the NIC and availability set.</span></span>

    ```azurecli
    for i in `seq 1 3`; do

        az network nic create --name woodgrove-Nic$i \
            --vnet-name woodgrove-VNET \
            --subnet backendSubnet

        az vm create --name woodgrove-VM$i \
            --availability-set woodgrove-avail-set \
            --nics woodgrove-Nic$i \
            --image UbuntuLTS \
            --admin-username azureuser \
            --generate-ssh-keys \
            --custom-data cloud-init.txt \
            --no-wait
    done
    ```
    > [!TIP]
    > <span data-ttu-id="25f60-150">Kommandot ställer in administratörsnamnet till ”azureuser” här, men du kan ändra till något annat.</span><span class="sxs-lookup"><span data-stu-id="25f60-150">The command is setting the administrator name to "azureuser" here, you can change that to anything you like.</span></span> <span data-ttu-id="25f60-151">Du kan också ange ett lösenord om du föredrar användarnamn/lösenord istället för SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="25f60-151">You can also supply a password if you prefer username/password over SSH keys.</span></span>

1. <span data-ttu-id="25f60-152">Detta kan ta ett tag att köra, och även när loopen är klar kommer de virtuella datorerna inte vara tillgängliga än.</span><span class="sxs-lookup"><span data-stu-id="25f60-152">This will take a while to run - and even when the loop is finished, the VMs won't quite be available.</span></span> <span data-ttu-id="25f60-153">Du kan växla till Azure-portalen och använda displayen **Alla resurser** för att se de skapade resurserna.</span><span class="sxs-lookup"><span data-stu-id="25f60-153">You can switch over to the Azure portal and use the **All Resources** display to see the created resources.</span></span>

1. <span data-ttu-id="25f60-154">Du kan också använda kommandot `vm list` för att se hur många virtuella datorer som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="25f60-154">You can also use the `vm list` command to see how many VMs have been deployed.</span></span>

    ```azurecli
    az vm list -o table
    ```

<span data-ttu-id="25f60-155">Vi kommer sedan att tala om hur man konfigurerar lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="25f60-155">Next we'll talk about how to configure the load balancer.</span></span>