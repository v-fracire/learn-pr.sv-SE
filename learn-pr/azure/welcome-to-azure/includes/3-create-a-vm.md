<span data-ttu-id="eb5bf-101">Som teknikproffs har du förmodligen specialkunskaper inom ett visst område.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-101">As a technology professional, you likely have expertise in a specific area.</span></span> <span data-ttu-id="eb5bf-102">Kanske är du lagringsadministratör eller virtualiseringsexpert, eller kanske fokuserar du på de senaste säkerhetsrutinerna.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-102">Perhaps you're a storage admin or virtualization expert, or maybe you focus on the latest security practices.</span></span> <span data-ttu-id="eb5bf-103">Om du studerar kanske du fortfarande utforskar vad som intresserar dig mest.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-103">If you're a student, you may still be exploring what interests you most.</span></span>

<span data-ttu-id="eb5bf-104">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="eb5bf-104">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="eb5bf-105">Oavsett vilken roll du har brukar de flesta som kommer igång med molnet skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-105">No matter your role, most people get started with the cloud by creating a virtual machine.</span></span> <span data-ttu-id="eb5bf-106">Här får du sätta igång en virtuell dator som kör Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-106">Here you'll bring up a virtual machine running Windows Server 2016.</span></span>

<span data-ttu-id="eb5bf-107">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb5bf-107">::: zone-end</span></span>

<span data-ttu-id="eb5bf-108">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="eb5bf-108">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="eb5bf-109">Oavsett vilken roll du har brukar de flesta som kommer igång med molnet skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-109">No matter your role, most people get started with the cloud by creating a virtual machine.</span></span> <span data-ttu-id="eb5bf-110">Här får du sätta igång en virtuell dator som kör Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-110">Here you'll bring up a virtual machine running Ubuntu 16.04.</span></span>

<span data-ttu-id="eb5bf-111">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb5bf-111">::: zone-end</span></span>

<span data-ttu-id="eb5bf-112">Det finns många sätt att skapa en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-112">There are many ways to create a virtual machine on Azure.</span></span> <span data-ttu-id="eb5bf-113">Här får du sätta upp en virtuell Windows- eller Linux-dator med hjälp av den interaktiva terminalen Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-113">Here, you'll bring up a Windows or Linux virtual machine using an interactive terminal called Cloud Shell.</span></span> <span data-ttu-id="eb5bf-114">Om du arbetar på terminalen dagligen vet du att det oftast är det snabbaste sättet att få jobbet gjort.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-114">If you work from the terminal on a daily basis, you know this is often the fastest way to get the job done.</span></span>

<span data-ttu-id="eb5bf-115">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="eb5bf-115">::: zone pivot="windows-cloud"</span></span>

> [!TIP]
> <span data-ttu-id="eb5bf-116">Föredrar du Linux eller vill du testa något nytt?</span><span class="sxs-lookup"><span data-stu-id="eb5bf-116">Prefer Linux or want to try something new?</span></span> <span data-ttu-id="eb5bf-117">Välj **Linux** högst upp på den här sidan om du vill köra en virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-117">Select **Linux** from the top of this page to run a Linux virtual machine.</span></span>

<span data-ttu-id="eb5bf-118">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb5bf-118">::: zone-end</span></span>

<span data-ttu-id="eb5bf-119">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="eb5bf-119">::: zone pivot="linux-cloud"</span></span>

> [!TIP]
> <span data-ttu-id="eb5bf-120">Föredrar du Windows eller vill du testa något nytt?</span><span class="sxs-lookup"><span data-stu-id="eb5bf-120">Prefer Windows or want to try something new?</span></span> <span data-ttu-id="eb5bf-121">Välj **Windows** högst upp på den här sidan om du vill köra en virtuell Windows Server-dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-121">Select **Windows** from the top of this page to run a Windows Server virtual machine.</span></span>

<span data-ttu-id="eb5bf-122">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb5bf-122">::: zone-end</span></span>

<span data-ttu-id="eb5bf-123">Nu ska vi gå igenom några grundläggande termer och få igång din första virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-123">Let's review some basic terms and get your first virtual machine up and running.</span></span>

## <a name="what-is-a-virtual-machine"></a><span data-ttu-id="eb5bf-124">Vad är en virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="eb5bf-124">What is a virtual machine?</span></span>

<span data-ttu-id="eb5bf-125">En virtuell dator, eller VM, är en programvaruemulering av en fysisk dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-125">A virtual machine, or VM, is a software emulation of a physical computer.</span></span> <span data-ttu-id="eb5bf-126">Eftersom virtuella datorer finns som programvara kan dussintals, hundratals eller till och med tusentals virtuella Azure-datorer genereras på några minuter och sedan tas bort när du inte längre behöver dem.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-126">Because VMs exist as software, dozens, hundreds, or even thousands of Azure VMs can be generated in minutes, then deleted when you don't need them anymore.</span></span> <span data-ttu-id="eb5bf-127">Med debitering per minut till en låg kostnad betalar du bara för de datorresurser du använder, så länge du använder dem.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-127">With low-cost, per-minute billing, you pay only for the compute resources you use, for as long as you are using them.</span></span> <span data-ttu-id="eb5bf-128">Dessutom finns det många sätt att konfigurera de virtuella datorerna så att de passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-128">Plus, there are many ways to configure the VMs to fit your needs.</span></span>

<span data-ttu-id="eb5bf-129">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="eb5bf-129">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="eb5bf-130">En ögonblicksbild av en virtuell dator som körs kallas för _avbildning_.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-130">A snapshot of a running VM is called an _image_.</span></span> <span data-ttu-id="eb5bf-131">Azure ger avbildningar för Windows och flera versioner av Linux.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-131">Azure provides images for Windows and several flavors of Linux.</span></span> <span data-ttu-id="eb5bf-132">Du kan även skapa egna förkonfigurerade avbildningar så att distributioner går snabbare.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-132">You can also create your own preconfigured images to make deployments go faster.</span></span> <span data-ttu-id="eb5bf-133">Här skapar du en virtuell Windows Server 2016-dator, som tillhandahålls av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-133">Here you'll bring up a Windows Server 2016 VM, provided by Microsoft.</span></span>

<span data-ttu-id="eb5bf-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb5bf-134">::: zone-end</span></span>

<span data-ttu-id="eb5bf-135">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="eb5bf-135">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="eb5bf-136">En ögonblicksbild av en virtuell dator som körs kallas för _avbildning_.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-136">A snapshot of a running VM is called an _image_.</span></span> <span data-ttu-id="eb5bf-137">Azure ger avbildningar för Windows och flera versioner av Linux.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-137">Azure provides images for Windows and several flavors of Linux.</span></span> <span data-ttu-id="eb5bf-138">Du kan även skapa egna förkonfigurerade avbildningar så att distributioner går snabbare.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-138">You can also create your own preconfigured images to make deployments go faster.</span></span> <span data-ttu-id="eb5bf-139">Här skapar du en virtuell Ubuntu 16.04-dator, som tillhandahålls av Canonical.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-139">Here you'll bring up an Ubuntu 16.04 VM, provided by Canonical.</span></span>

<span data-ttu-id="eb5bf-140">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb5bf-140">::: zone-end</span></span>

## <a name="what-defines-a-virtual-machine-on-azure"></a><span data-ttu-id="eb5bf-141">Vad definierar en virtuell dator på Azure?</span><span class="sxs-lookup"><span data-stu-id="eb5bf-141">What defines a virtual machine on Azure?</span></span>

<span data-ttu-id="eb5bf-142">En virtuell dator definieras av ett antal faktorer, till exempel storlek och plats.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-142">A virtual machine is defined by a number of factors, including its size and location.</span></span> <span data-ttu-id="eb5bf-143">Innan du skapar den virtuella datorn tar vi en snabb titt vad som ingår.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-143">Before you bring up your VM, let's briefly cover what's involved.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="eb5bf-144">**Storlek**</span><span class="sxs-lookup"><span data-stu-id="eb5bf-144">**Size**</span></span>
    :::column-end:::
    <span data-ttu-id="eb5bf-145">:::column span="3"::: En virtuell dators _storlek_ definierar dess processorhastighet, mängd minne och förväntade nätverksbandbredd.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-145">:::column span="3"::: A VM's _size_ defines its processor speed, amount of memory, initial amount of storage, and expected network bandwidth.</span></span> <span data-ttu-id="eb5bf-146">Vissa storlekar inbegriper även specialiserad maskinvara som grafikprocessorer för tung grafikrendering och videoredigering.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-146">Some sizes even include specialized hardware such as GPUs for heavy graphics rendering and video editing.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="eb5bf-147">**Region**</span><span class="sxs-lookup"><span data-stu-id="eb5bf-147">**Region**</span></span>
    :::column-end:::
    <span data-ttu-id="eb5bf-148">:::column span="3"::: Azure utgörs av datacenter över hela världen.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-148">:::column span="3"::: Azure is made up of data centers distributed throughout the world.</span></span> <span data-ttu-id="eb5bf-149">En _region_ är en uppsättning Azure-datacenter på en namngiven geografisk plats.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-149">A _region_ is a set of Azure data centers in a named geographic location.</span></span> <span data-ttu-id="eb5bf-150">Varje Azure-resurs, bland annat virtuella datorer, tilldelas en region.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-150">Every Azure resource, including virtual machines, is assigned a region.</span></span> <span data-ttu-id="eb5bf-151">USA, östra och Europa, norra är exempel på regioner.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-151">East US and North Europe are examples of regions.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="eb5bf-152">**Nätverk**</span><span class="sxs-lookup"><span data-stu-id="eb5bf-152">**Network**</span></span>
    :::column-end:::
    <span data-ttu-id="eb5bf-153">:::column span="3"::: En _virtuellt nätverk_ är ett logiskt isolerat nätverk på Azure.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-153">:::column span="3"::: A _virtual network_ is a logically isolated network on Azure.</span></span> <span data-ttu-id="eb5bf-154">Varje virtuella dator på Azure associeras med ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-154">Each virtual machine on Azure is associated with a virtual network.</span></span> <span data-ttu-id="eb5bf-155">Azure har brandväggar på molnnivå för dina virtuella nätverk som kallas för _nätverkssäkerhetsgrupper_.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-155">Azure provides cloud-level firewalls for your virtual networks called _network security groups_.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="eb5bf-156">**Resursgrupper**</span><span class="sxs-lookup"><span data-stu-id="eb5bf-156">**Resource groups**</span></span>
    :::column-end:::
    <span data-ttu-id="eb5bf-157">:::column span="3"::: Virtuella datorer och andra molnresurser grupperas i logiska containrar som kallas för _resursgrupper_.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-157">:::column span="3"::: Virtual machines and other cloud resources are grouped into logical containers called _resource groups_.</span></span> <span data-ttu-id="eb5bf-158">Grupper används vanligtvis för att organisera uppsättningar av resurser som distribueras tillsammans som en del av ett program eller en tjänst.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-158">Groups are typically used to organize sets of resources that are deployed together as part of an application or service.</span></span> <span data-ttu-id="eb5bf-159">Du refererar till en resursgrupp med dess namn.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-159">You refer to a resource group by its name.</span></span>
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="eb5bf-160">Vad är Azure Cloud Shell?</span><span class="sxs-lookup"><span data-stu-id="eb5bf-160">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="eb5bf-161">Azure Cloud Shell är en webbläsarbaserad kommandoradsmiljö för hantering och utveckling av Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-161">Azure Cloud Shell is a browser-based command-line experience for managing and developing Azure resources.</span></span> <span data-ttu-id="eb5bf-162">Tänk på Cloud Shell som en interaktiv konsol som du kör i molnet.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-162">Think of Cloud Shell as an interactive console that you run in the cloud.</span></span>

<span data-ttu-id="eb5bf-163">Du kan välja mellan två olika miljöer i Cloud Shell: Bash och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-163">Cloud Shell provides two experiences to choose from: Bash and PowerShell.</span></span> <span data-ttu-id="eb5bf-164">Båda har åtkomst till Azure CLI, kommandoradsgränssnittet för Azure.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-164">Both include access to the Azure CLI, the command-line interface for Azure.</span></span>

<span data-ttu-id="eb5bf-165">Du kan använda valfritt Azure-hanteringsgränssnitt, som Azure Portal, Azure CLI och Azure PowerShell, för att hantera alla typer av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-165">You can use any Azure management interface, including the Azure portal, Azure CLI, and Azure PowerShell, to manage any kind of VM.</span></span> <span data-ttu-id="eb5bf-166">I utbildningssyfte använder vi här Azure CLI för att skapa och hantera en virtuell Windows- eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-166">For learning purposes, here you'll use the Azure CLI to create and manage either a Windows or Linux VM.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-resources-in-azure"></a><span data-ttu-id="eb5bf-167">Skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="eb5bf-167">Creating resources in Azure</span></span>

<span data-ttu-id="eb5bf-168">Normalt sett är det första vi gör att skapa en _resursgrupp_ för allt som behövs för att skapa.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-168">Normally, the first thing we'd do is to create a _resource group_ to hold all the things that we need to create.</span></span> <span data-ttu-id="eb5bf-169">Detta låter oss administrera alla virtuella datorer, diskar, nätverksgränssnitt och andra element som utgör vår lösning som en enhet.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-169">This allows us to administer all the VMs, disks, network interfaces, and other elements that make up our solution as a unit.</span></span> <span data-ttu-id="eb5bf-170">Vi kan använda Azure CLI för att skapa en resursgrupp med kommandot `az group create`.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-170">We can use the Azure CLI to create a resource group with the `az group create` command.</span></span> <span data-ttu-id="eb5bf-171">Det tar en `--name` för att ge det ett unikt namn i vår prenumeration och en `--location` för att säga till Azure vilket område i världen där vi vill att resursen ska finnas som standard.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-171">It takes a `--name` to give it a unique name in our subscription, and a `--location` to tell Azure what area of the world we want the resources to be located by default.</span></span>

<span data-ttu-id="eb5bf-172">Eftersom det är en kostnadsfri Azure-Sandbox-miljö så behöver du inte göra det här steget, istället kommer du att använda den förskapade resursgruppen **<rgn>[Resursgruppens namn]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-172">Since we are in the free Azure Sandbox environment, you don't need to do this step, instead, you will use the pre-created resource group **<rgn>[Resource Group Name]</rgn>**.</span></span>

## <a name="choosing-a-location"></a><span data-ttu-id="eb5bf-173">Välj en plats</span><span class="sxs-lookup"><span data-stu-id="eb5bf-173">Choosing a location</span></span>

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="eb5bf-174">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="eb5bf-174">::: zone pivot="windows-cloud"</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="eb5bf-175">Skapa en virtuell Windows-dator</span><span class="sxs-lookup"><span data-stu-id="eb5bf-175">Create a Windows VM</span></span>

<span data-ttu-id="eb5bf-176">Nu ska vi sätta igång din virtuella Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-176">Let's get your Windows VM up and running.</span></span>

1. <span data-ttu-id="eb5bf-177">I Cloud Shell på höger sida om den här sidan, kör du följande `az vm create`-kommando för att skapa din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-177">In the Cloud Shell on the right side of this page, run the following `az vm create` command to create your VM.</span></span> <span data-ttu-id="eb5bf-178">Kommandot skapar den virtuella datorn på platsen USA, östra, du kan ändra till valfri annan plats som listas ovanför &mdash; vi rekommenderar att du väljer en nära dig.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-178">The command creates the VM in the "East US" location, you can change that to any of the locations listed above &mdash; we recommend you select one close to you.</span></span>

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --location eastus \
      --admin-username azureuser
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    <span data-ttu-id="eb5bf-179">Även om du kan ange administratörslösenordet för Windows med kommandot, kommer du här att uppmanas ange ett.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-179">Although you can specify the Windows admin password through the command, here you'll be prompted to enter one.</span></span> <span data-ttu-id="eb5bf-180">Välj ett lösenord som innehåller minst 12 tecken med en kombination av gemener och versaler, siffror och symboler.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-180">Choose a password that contains at least 12 characters with a combination of upper and lowercase letters, numbers, and symbols.</span></span> <span data-ttu-id="eb5bf-181">Använd inte ett lösenord som du använder någon annanstans.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-181">Don't use a password you use elsewhere.</span></span>

    <span data-ttu-id="eb5bf-182">Det tar fyra till fem minuter för den virtuella datorn att bli aktiv.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-182">Your VM will take four to five minutes to come up.</span></span> <span data-ttu-id="eb5bf-183">Jämför det med tiden det tar att köpa, sätta upp och konfigurera ett system i ditt datacenter.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-183">Compare that to the time it takes to purchase, rack, and configure a system in your data center.</span></span> <span data-ttu-id="eb5bf-184">Stor skillnad!</span><span class="sxs-lookup"><span data-stu-id="eb5bf-184">Quite a difference!</span></span>

<span data-ttu-id="eb5bf-185">Medan du väntar går vi igenom kommandot du precis har kört.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-185">While you're waiting, let's review the command you just ran.</span></span>

* <span data-ttu-id="eb5bf-186">Den virtuella datorn heter **myVM**.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-186">The VM is named **myVM**.</span></span> <span data-ttu-id="eb5bf-187">Namnet identifierar den virtuella datorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-187">This name identifies the VM in Azure.</span></span> <span data-ttu-id="eb5bf-188">Det blir även den virtuella datorns interna värdnamn, eller datornamn.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-188">It also becomes the VM's internal hostname, or computer name.</span></span>
* <span data-ttu-id="eb5bf-189">Resursgruppen, eller den virtuella datorns logiska container, heter **<rgn>[Namn för Sandbox-resursgruppen]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-189">The resource group, or the VM's logical container, is named **<rgn>[Sandbox resource group name]</rgn>**.</span></span>
* <span data-ttu-id="eb5bf-190">**Win2016Datacenter** Anger den virtuella Windows Server 2016-datorns avbildning.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-190">**Win2016Datacenter** specifies the Windows Server 2016 VM image.</span></span>
* <span data-ttu-id="eb5bf-191">**Standard_DS2_v2** anger storleken på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-191">**Standard_DS2_v2** refers to the size of the VM.</span></span> <span data-ttu-id="eb5bf-192">Den här storleken har två processorer och 7 GB minne.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-192">This size has two virtual CPUs and 7 GB of memory.</span></span>
* <span data-ttu-id="eb5bf-193">Med användarnamn och lösenord kan du ansluta till den virtuella datorn senare.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-193">The username and password enable you to connect to your VM later.</span></span> <span data-ttu-id="eb5bf-194">Du kan till exempel ansluta via fjärrskrivbordet eller WinRM för att arbeta med och konfigurera systemet.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-194">For example, you can connect over Remote Desktop or WinRM to work with and configure the system.</span></span>

<span data-ttu-id="eb5bf-195">Som standard tilldelar Azure en offentlig IP-adress till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-195">By default, Azure assigns a public IP address to your VM.</span></span> <span data-ttu-id="eb5bf-196">Du kan konfigurera en virtuell dator så att den är åtkomlig via internet eller bara via det interna nätverket.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-196">You can configure a VM to be accessible from the Internet or only from the internal network.</span></span>

<span data-ttu-id="eb5bf-197">Du kan även titta på den här korta videon om några av alternativen du har för att skapa och hantera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-197">You can also check out this short video about some of the options you have to create and manage VMs.</span></span>

#### <a name="options-to-create-and-manage-vms"></a><span data-ttu-id="eb5bf-198">Alternativ för att skapa och hantera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="eb5bf-198">Options to create and manage VMs</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

<span data-ttu-id="eb5bf-199">När den virtuella datorn är klar visas informationen om den.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-199">When the VM is ready, you see information about it.</span></span> <span data-ttu-id="eb5bf-200">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-200">Here's an example.</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

<span data-ttu-id="eb5bf-201">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb5bf-201">::: zone-end</span></span>

<span data-ttu-id="eb5bf-202">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="eb5bf-202">::: zone pivot="linux-cloud"</span></span>

## <a name="create-a-linux-vm"></a><span data-ttu-id="eb5bf-203">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="eb5bf-203">Create a Linux VM</span></span>

<span data-ttu-id="eb5bf-204">Nu ska vi sätta igång din virtuella Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-204">Let's get your Linux VM up and running.</span></span>

1. <span data-ttu-id="eb5bf-205">Från Cloud Shell på höger sida om den här sidan, kör du `az vm create`-kommandot för att skapa din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-205">From Cloud Shell on the right side of this page, run the `az vm create` command to create your VM.</span></span> <span data-ttu-id="eb5bf-206">Följande kommando skapar den virtuella datorn på platsen USA, östra, du kan ändra till valfri annan plats som listas ovanför. Vi rekommenderar att du väljer en nära dig.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-206">The following command creates the VM in the "East US" location, you can change that to any of the locations listed above - we recommend you select one close to you.</span></span>

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --location eastus \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    <span data-ttu-id="eb5bf-207">Det tar cirka två minuter för den virtuella datorn att bli aktiv.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-207">Your VM will take about two minutes to come up.</span></span> <span data-ttu-id="eb5bf-208">Jämför det med tiden det tar att köpa, sätta upp och konfigurera ett system i ditt datacenter.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-208">Compare that to the time it takes to purchase, rack, and configure a system in your data center.</span></span> <span data-ttu-id="eb5bf-209">Stor skillnad!</span><span class="sxs-lookup"><span data-stu-id="eb5bf-209">Quite a difference!</span></span>

<span data-ttu-id="eb5bf-210">Medan du väntar går vi igenom kommandot du precis har kört.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-210">While you're waiting, let's review the command you just ran.</span></span>

* <span data-ttu-id="eb5bf-211">Den virtuella datorn heter **myVM**.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-211">The VM is named **myVM**.</span></span> <span data-ttu-id="eb5bf-212">Namnet identifierar den virtuella datorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-212">This name identifies the VM in Azure.</span></span> <span data-ttu-id="eb5bf-213">Det blir även den virtuella datorns interna värdnamn, eller datornamn.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-213">It also becomes the VM's internal hostname, or computer name.</span></span>
* <span data-ttu-id="eb5bf-214">Resursgruppen, eller den virtuella datorns logiska container, heter **<rgn>[Namn för Sandbox-resursgruppen]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-214">The resource group, or the VM's logical container, is named **<rgn>[Sandbox resource group name]</rgn>**.</span></span>
* <span data-ttu-id="eb5bf-215">**UbuntuLTS** anger den virtuella Ubuntu 16.04 LTS-datorns avbildning.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-215">**UbuntuLTS** specifies the Ubuntu 16.04 LTS VM image.</span></span>
* <span data-ttu-id="eb5bf-216">**Standard_DS2_v2** anger storleken på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-216">**Standard_DS2_v2** refers to the size of the VM.</span></span> <span data-ttu-id="eb5bf-217">Den här storleken har två processorer och 7 GB minne.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-217">This size has two virtual CPUs and 7 GB of memory.</span></span>
* <span data-ttu-id="eb5bf-218">Alternativet `--generate-ssh-keys` skapar ett SSH-nyckelpar så att du kan logga in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-218">The `--generate-ssh-keys` option creates an SSH key pair to enable you to log in to the VM.</span></span>

<span data-ttu-id="eb5bf-219">Som standard tilldelar Azure en offentlig IP-adress till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-219">By default, Azure assigns a public IP address to your VM.</span></span> <span data-ttu-id="eb5bf-220">Du kan konfigurera en virtuell dator så att den är åtkomlig via internet eller bara via det interna nätverket.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-220">You can configure a VM to be accessible from the Internet or only from the internal network.</span></span>

<span data-ttu-id="eb5bf-221">Du kan även titta på den här korta videon om några av alternativen du har för att skapa och hantera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-221">You can also check out this short video about some of the options you have to create and manage VMs.</span></span>

#### <a name="options-to-create-and-manage-vms"></a><span data-ttu-id="eb5bf-222">Alternativ för att skapa och hantera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="eb5bf-222">Options to create and manage VMs</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

<span data-ttu-id="eb5bf-223">När den virtuella datorn är klar visas informationen om den.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-223">When the VM is ready, you see information about it.</span></span> <span data-ttu-id="eb5bf-224">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-224">Here's an example.</span></span>

```json
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

<span data-ttu-id="eb5bf-225">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="eb5bf-225">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="eb5bf-226">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="eb5bf-226">Summary</span></span>

<span data-ttu-id="eb5bf-227">Med hjälp av några få begrepp kan du skapa en virtuell dator i Azure på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-227">With just a few concepts under your belt, you're able to spin up a VM on Azure in just a few minutes.</span></span> <span data-ttu-id="eb5bf-228">Många av begreppen, som en virtuell dators storlek och brandväggsregler, känner du troligen redan till.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-228">Many of these concepts, such as a VM's size and firewall rules, are likely familiar to you already.</span></span>

<span data-ttu-id="eb5bf-229">Nu ska du installera en webbserver på den virtuella datorn och konfigurera webbservern att leverera en grundläggande webbplats.</span><span class="sxs-lookup"><span data-stu-id="eb5bf-229">Next, you'll install a web server on your VM and configure your web server to serve up a basic web site.</span></span>