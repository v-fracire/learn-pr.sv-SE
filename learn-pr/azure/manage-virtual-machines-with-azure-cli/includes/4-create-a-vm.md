<span data-ttu-id="f0657-101">Azure CLI innehåller kommandot `vm` som du kan använda för att arbeta med virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="f0657-101">The Azure CLI includes the `vm` command to work with virtual machines in Azure.</span></span> <span data-ttu-id="f0657-102">Det finns flera underkommandon för att utföra specifika aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f0657-102">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="f0657-103">De vanligaste är:</span><span class="sxs-lookup"><span data-stu-id="f0657-103">The most common include:</span></span>

| <span data-ttu-id="f0657-104">Underkommando</span><span class="sxs-lookup"><span data-stu-id="f0657-104">Sub-command</span></span> | <span data-ttu-id="f0657-105">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0657-105">Description</span></span> |
|-------------|-------------|
| `create`    | <span data-ttu-id="f0657-106">Skapa en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f0657-106">Create a new virtual machine</span></span> |
| `deallocate` | <span data-ttu-id="f0657-107">Frigöra en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f0657-107">Deallocate a virtual machine</span></span> |
| `delete` | <span data-ttu-id="f0657-108">Ta bort en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f0657-108">Delete a virtual machine</span></span> |
| `list` | <span data-ttu-id="f0657-109">Visa en lista över de virtuella datorerna i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="f0657-109">List the created virtual machines in your subscription</span></span> |
| `open-port` | <span data-ttu-id="f0657-110">Öppna en specifik nätverksport för inkommande trafik</span><span class="sxs-lookup"><span data-stu-id="f0657-110">Open a specific network port for inbound traffic</span></span> |
| `restart` | <span data-ttu-id="f0657-111">Starta om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f0657-111">Restart a virtual machine</span></span> |
| `show` | <span data-ttu-id="f0657-112">Hämta information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f0657-112">Get the details for a virtual machine</span></span> |
| `start` | <span data-ttu-id="f0657-113">Starta en stoppad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f0657-113">Start a stopped virtual machine</span></span> |
| `stop` | <span data-ttu-id="f0657-114">Stoppa en virtuell dator som körs</span><span class="sxs-lookup"><span data-stu-id="f0657-114">Stop a running virtual machine</span></span> |
| `update` | <span data-ttu-id="f0657-115">Uppdatera en egenskap för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f0657-115">Update a property of a virtual machine</span></span> |

> [!NOTE]
> <span data-ttu-id="f0657-116">En fullständig kommandolista finns i [referensdokumentationen för Azure CLI](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="f0657-116">For a complete list of commands, you can check the [Azure CLI reference documentation](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span></span>

<span data-ttu-id="f0657-117">Låt oss börja med det första: `az vm create`.</span><span class="sxs-lookup"><span data-stu-id="f0657-117">Let's start with the first one: `az vm create`.</span></span> <span data-ttu-id="f0657-118">Det här kommandot används för att skapa en virtuell dator i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f0657-118">This command is used to create a virtual machine in a resource group.</span></span> <span data-ttu-id="f0657-119">Du kan använda flera parametrar för att konfigurera alla aspekter av den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0657-119">There are several parameters you can pass to configure all the aspects of the new VM.</span></span> <span data-ttu-id="f0657-120">De tre parametrar som måste anges är:</span><span class="sxs-lookup"><span data-stu-id="f0657-120">The three parameters that must be supplied are:</span></span>

| <span data-ttu-id="f0657-121">Parameter</span><span class="sxs-lookup"><span data-stu-id="f0657-121">Parameter</span></span> | <span data-ttu-id="f0657-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0657-122">Description</span></span> |
|-----------|-------------|
| `resource-group` | <span data-ttu-id="f0657-123">Den resursgrupp som ska äga den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f0657-123">The resource group that will own the virtual machine</span></span> |
| `name` | <span data-ttu-id="f0657-124">Namnet på den virtuella datorn – måste vara unikt inom resursgruppen</span><span class="sxs-lookup"><span data-stu-id="f0657-124">The name of the virtual machine - must be unique within the resource group</span></span> |
| `image` | <span data-ttu-id="f0657-125">Avbildningen av operativsystemet som ska användas för att skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f0657-125">The operating system image to use to create the VM</span></span> |

<span data-ttu-id="f0657-126">Dessutom är det bra att lägga till flaggan `--verbose` så att du kan följa förloppet när den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="f0657-126">In addition, it's helpful to add the `--verbose` flag to see progress while the VM is being created.</span></span> 

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="f0657-127">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="f0657-127">Create a Linux virtual machine</span></span>

<span data-ttu-id="f0657-128">Nu ska vi skapa en ny virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="f0657-128">Let's create a new Linux virtual machine.</span></span> <span data-ttu-id="f0657-129">Kör följande kommando i Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="f0657-129">Execute the following command in Azure Cloud Shell:</span></span>

```azurecli
az vm create --resource-group ExerciseResources --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --verbose 
```

<span data-ttu-id="f0657-130">Det här kommandot skapar en ny **Debian**-baserad virtuell Linux-dator med namnet `SampleVM`.</span><span class="sxs-lookup"><span data-stu-id="f0657-130">This command will create a new **Debian** Linux virtual machine with the name `SampleVM`.</span></span> <span data-ttu-id="f0657-131">Observera att verktyget Azure CLI blockeras medan den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="f0657-131">Notice that the Azure CLI tool is blocked while the VM is being created.</span></span> <span data-ttu-id="f0657-132">Om du inte vill vänta kan du använda alternativet `--no-wait` för att instruera Azure CLI att returnera utdata direkt, till exempel om du kör kommandot i ett skript.</span><span class="sxs-lookup"><span data-stu-id="f0657-132">If you would prefer not to wait, you can use the `--no-wait` option to tell the Azure CLI tool to return immediately, for example if you're executing the command in a script.</span></span> <span data-ttu-id="f0657-133">Senare i skriptet använder du kommandot `azure vm wait --name [vm-name]` för att vänta tills den virtuella datorn har skapats.</span><span class="sxs-lookup"><span data-stu-id="f0657-133">Later in the script, use the `azure vm wait --name [vm-name]` command to wait for the VM to finish being created.</span></span>

<span data-ttu-id="f0657-134">Om du tittar på de utförliga svaren ser du också att namnet `SampleVM` används för att namnge olika beroenden för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0657-134">If you look at the verbose responses, you will also see that the `SampleVM` name is used to name various dependencies for the VM.</span></span>

```
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

<span data-ttu-id="f0657-135">Du kan åsidosätta dessa automatiskt genererade resursnamn med hjälp av valfria parametrar till `vm create`, till exempel `--vnet-name` och `--public-ip-address-dns-name`.</span><span class="sxs-lookup"><span data-stu-id="f0657-135">You can override these auto-generated resource names using optional parameters to `vm create`, such as `--vnet-name` and `--public-ip-address-dns-name`.</span></span>

<span data-ttu-id="f0657-136">Observera att vi anger namnet på administratörskontot till ”aldis” med hjälp av flaggan `admin-username`.</span><span class="sxs-lookup"><span data-stu-id="f0657-136">Notice that we are specifying the admin account name through the `admin-username` flag to be "aldis".</span></span> <span data-ttu-id="f0657-137">Om du utelämnar detta använder kommandot `vm create` _ditt aktuella användarnamn_.</span><span class="sxs-lookup"><span data-stu-id="f0657-137">If you omit this, the `vm create` command will use your _current user name_.</span></span> <span data-ttu-id="f0657-138">Eftersom reglerna för kontonamn är olika för varje operativsystem, är det säkrast att ange ett specifikt namn.</span><span class="sxs-lookup"><span data-stu-id="f0657-138">Since the rules for account names are different for each OS, it's safer to specify a specific name.</span></span> <span data-ttu-id="f0657-139">Vanliga namn, till exempel ”rot” och ”admin” tillåts inte för de flesta avbildningar.</span><span class="sxs-lookup"><span data-stu-id="f0657-139">Common names such as "root" and "admin" are not allowed for most images.</span></span>

<span data-ttu-id="f0657-140">Vi använder också flaggan `generate-ssh-keys`.</span><span class="sxs-lookup"><span data-stu-id="f0657-140">We are also using the `generate-ssh-keys` flag.</span></span> <span data-ttu-id="f0657-141">Den här parametern används för Linux-distributioner och skapar ett säkerhetsnyckelpar så att vi kan använda verktyget `ssh` för att få fjärråtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0657-141">This parameter is used for Linux distributions and creates a pair of security keys so we can use the `ssh` tool to access the virtual machine remotely.</span></span> <span data-ttu-id="f0657-142">De två filerna placeras i mappen `.ssh` på din dator och på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f0657-142">The two files are placed into the `.ssh` folder on your machine and in the VM.</span></span> <span data-ttu-id="f0657-143">Om du redan har en SSH-nyckel med namnet `id_rsa` i målmappen, används den nyckeln i stället för att en ny nyckel genereras.</span><span class="sxs-lookup"><span data-stu-id="f0657-143">If you already have an SSH key named `id_rsa` in the target folder, then it will be used rather than having a new key generated.</span></span>

<span data-ttu-id="f0657-144">När den virtuella datorn har skapats får du ett JSON-svar som innehåller den virtuella datorns aktuella tillstånd och dess offentliga och privata IP-adresser som tilldelats av Azure:</span><span class="sxs-lookup"><span data-stu-id="f0657-144">Once it finishes creating the VM, you will get a JSON response which includes the current state of the virtual machine and its public and private IP addresses assigned by Azure:</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1A-D9-74",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "168.61.54.62",
  "resourceGroup": "ExerciseResources",
  "zones": ""
}
```

> [!NOTE]
> <span data-ttu-id="f0657-145">Observera att den virtuella datorn har skapats på platsen **usaöstra**.</span><span class="sxs-lookup"><span data-stu-id="f0657-145">Notice that the VM was created in the **eastus** location.</span></span> <span data-ttu-id="f0657-146">Som standard skapas den virtuella datorn på den plats som identifieras av den ägande regionen.</span><span class="sxs-lookup"><span data-stu-id="f0657-146">By default, the virtual machine is created in the location identified by the owning region.</span></span> <span data-ttu-id="f0657-147">Ibland kan det emellertid vara bra att associera den virtuella datorn med en befintlig region, men låta den starta någon annanstans i världen.</span><span class="sxs-lookup"><span data-stu-id="f0657-147">However, sometimes you might want to associate the VM with an existing region, but actually have it spin up somewhere else in the world.</span></span> <span data-ttu-id="f0657-148">Det kan du göra genom att ange parametern `--location` som en del av kommandot `az vm create`.</span><span class="sxs-lookup"><span data-stu-id="f0657-148">You can do this by specifying the option `--location` parameter as part of the `az vm create` command.</span></span>