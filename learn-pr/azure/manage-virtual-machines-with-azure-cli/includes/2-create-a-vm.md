<span data-ttu-id="0413a-101">Vi börjar med den mest uppenbara uppgiften: skapa en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="0413a-101">Let's start with the most obvious task: creating an Azure Virtual Machine.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="logins-subscriptions-and-resource-groups"></a><span data-ttu-id="0413a-102">Inloggningar, prenumerationer och resursgrupper</span><span class="sxs-lookup"><span data-stu-id="0413a-102">Logins, subscriptions, and resource groups</span></span>

<span data-ttu-id="0413a-103">Du får arbeta i Azure Cloud Shell till höger.</span><span class="sxs-lookup"><span data-stu-id="0413a-103">You'll be working in the Azure Cloud Shell on the right.</span></span> <span data-ttu-id="0413a-104">När du har aktiverat sandbox-miljön loggas du in på Azure med en kostnadsfri prenumeration som hanteras av Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="0413a-104">Once you activate the sandbox, you'll be logged into Azure with a free subscription managed by Microsoft Learn.</span></span> <span data-ttu-id="0413a-105">Du behöver inte logga in på Azure på egen hand eller välja en prenumeration – det görs åt dig.</span><span class="sxs-lookup"><span data-stu-id="0413a-105">You don't have to log into Azure on your own, or select a subscription - this will be done for you.</span></span> <span data-ttu-id="0413a-106">Dessutom skulle du normalt skapa en _resursgrupp_ för nya resurser.</span><span class="sxs-lookup"><span data-stu-id="0413a-106">In addition, normally you would create a _resource group_ to hold new resources.</span></span> <span data-ttu-id="0413a-107">I den här modulen skapar Azure-sandbox-miljön en resursgrupp åt dig som används för att köra alla kommandon.</span><span class="sxs-lookup"><span data-stu-id="0413a-107">In this module, the Azure Sandbox will create a resource group for you which will be used to execute all the commands.</span></span>

## <a name="create-a-linux-vm-with-the-azure-cli"></a><span data-ttu-id="0413a-108">Skapa en virtuell Linux-dator med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0413a-108">Create a Linux VM with the Azure CLI</span></span>

<span data-ttu-id="0413a-109">Azure CLI innehåller kommandot `vm` som du kan använda för att arbeta med virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="0413a-109">The Azure CLI includes the `vm` command to work with virtual machines in Azure.</span></span> <span data-ttu-id="0413a-110">Det finns flera underkommandon för att utföra specifika aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="0413a-110">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="0413a-111">De vanligaste är:</span><span class="sxs-lookup"><span data-stu-id="0413a-111">The most common include:</span></span>

| <span data-ttu-id="0413a-112">Underkommando</span><span class="sxs-lookup"><span data-stu-id="0413a-112">Sub-command</span></span> | <span data-ttu-id="0413a-113">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0413a-113">Description</span></span> |
|-------------|-------------|
| `create`    | <span data-ttu-id="0413a-114">Skapa en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0413a-114">Create a new virtual machine</span></span> |
| `deallocate` | <span data-ttu-id="0413a-115">Frigöra en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0413a-115">Deallocate a virtual machine</span></span> |
| `delete` | <span data-ttu-id="0413a-116">Ta bort en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0413a-116">Delete a virtual machine</span></span> |
| `list` | <span data-ttu-id="0413a-117">Visa en lista över de virtuella datorerna i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="0413a-117">List the created virtual machines in your subscription</span></span> |
| `open-port` | <span data-ttu-id="0413a-118">Öppna en specifik nätverksport för inkommande trafik</span><span class="sxs-lookup"><span data-stu-id="0413a-118">Open a specific network port for inbound traffic</span></span> |
| `restart` | <span data-ttu-id="0413a-119">Starta om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0413a-119">Restart a virtual machine</span></span> |
| `show` | <span data-ttu-id="0413a-120">Hämta information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0413a-120">Get the details for a virtual machine</span></span> |
| `start` | <span data-ttu-id="0413a-121">Starta en stoppad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0413a-121">Start a stopped virtual machine</span></span> |
| `stop` | <span data-ttu-id="0413a-122">Stoppa en virtuell dator som körs</span><span class="sxs-lookup"><span data-stu-id="0413a-122">Stop a running virtual machine</span></span> |
| `update` | <span data-ttu-id="0413a-123">Uppdatera en egenskap för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0413a-123">Update a property of a virtual machine</span></span> |

> [!NOTE]
> <span data-ttu-id="0413a-124">En fullständig kommandolista finns i [referensdokumentationen för Azure CLI](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="0413a-124">For a complete list of commands, you can check the [Azure CLI reference documentation](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span></span>

<span data-ttu-id="0413a-125">Låt oss börja med det första: `az vm create`.</span><span class="sxs-lookup"><span data-stu-id="0413a-125">Let's start with the first one: `az vm create`.</span></span> <span data-ttu-id="0413a-126">Det här kommandot används för att skapa en virtuell dator i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0413a-126">This command is used to create a virtual machine in a resource group.</span></span> <span data-ttu-id="0413a-127">Du kan använda flera parametrar för att konfigurera alla aspekter av den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0413a-127">There are several parameters you can pass to configure all the aspects of the new VM.</span></span> <span data-ttu-id="0413a-128">De tre parametrar som måste anges är:</span><span class="sxs-lookup"><span data-stu-id="0413a-128">The three parameters that must be supplied are:</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="0413a-129">Parameter</span><span class="sxs-lookup"><span data-stu-id="0413a-129">Parameter</span></span> | <span data-ttu-id="0413a-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0413a-130">Description</span></span> |
> |-----------|-------------|
> | `resource-group` | <span data-ttu-id="0413a-131">Den resursgrupp som ska äga den virtuella datorn använder **<rgn>[sandbox-resursgrupp]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="0413a-131">The resource group that will own the virtual machine, use **<rgn>[Sandbox Resource Group]</rgn>**.</span></span> |
> | `name` | <span data-ttu-id="0413a-132">Namnet på den virtuella datorn – måste vara unikt inom resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0413a-132">The name of the virtual machine - must be unique within the resource group.</span></span> |
> | `image` | <span data-ttu-id="0413a-133">Avbildningen av operativsystemet som ska användas för att skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0413a-133">The operating system image to use to create the VM.</span></span> |
> | `location` | <span data-ttu-id="0413a-134">Regionen som den virtuella datorn ska placeras i.</span><span class="sxs-lookup"><span data-stu-id="0413a-134">The region to place the VM in.</span></span> <span data-ttu-id="0413a-135">Vanligtvis är det nära konsumenten av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0413a-135">Typically this would be close to the consumer of the VM.</span></span> <span data-ttu-id="0413a-136">I den här övningen ska du välja en plats i närheten i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="0413a-136">In this exercise, choose a location nearby from the following list.</span></span> |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="0413a-137">Dessutom är det bra att lägga till flaggan `--verbose` så att du kan följa förloppet när den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="0413a-137">In addition, it's helpful to add the `--verbose` flag to see progress while the VM is being created.</span></span> 

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="0413a-138">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="0413a-138">Create a Linux virtual machine</span></span>

<span data-ttu-id="0413a-139">Nu ska vi skapa en ny virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="0413a-139">Let's create a new Linux virtual machine.</span></span> <span data-ttu-id="0413a-140">Utför följande kommando i Azure Cloud Shell för att skapa en Debian Linux-dator på platsen ”USA, västra”.</span><span class="sxs-lookup"><span data-stu-id="0413a-140">Execute the following command in Azure Cloud Shell to create a Debian Linux machine in the "West US" location.</span></span> <span data-ttu-id="0413a-141">Ändra plats om det inte är i närheten.</span><span class="sxs-lookup"><span data-stu-id="0413a-141">Change the location if that one isn't nearby.</span></span>

```azurecli
az vm create --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --location westus --verbose 
```

[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


<span data-ttu-id="0413a-142">Det här kommandot skapar en ny **Debian**-baserad virtuell Linux-dator med namnet `SampleVM`.</span><span class="sxs-lookup"><span data-stu-id="0413a-142">This command will create a new **Debian** Linux virtual machine with the name `SampleVM`.</span></span> <span data-ttu-id="0413a-143">Observera att verktyget Azure CLI väntar medan den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="0413a-143">Notice that the Azure CLI tool waits while the VM is being created.</span></span> <span data-ttu-id="0413a-144">Du kan lägga till alternativet `--no-wait` för att instruera Azure CLI-verktyget att återgå omedelbart och låta Azure fortsätta skapa den virtuella datorn i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="0413a-144">You can add the `--no-wait` option to tell the Azure CLI tool to return immediately and have Azure continue creating the VM in the background.</span></span> <span data-ttu-id="0413a-145">Det är användbart om du kör kommandot i ett skript.</span><span class="sxs-lookup"><span data-stu-id="0413a-145">This is useful if you're executing the command in a script.</span></span> <span data-ttu-id="0413a-146">Senare i skriptet använder du kommandot `azure vm wait --name [vm-name]` för att vänta tills den virtuella datorn har skapats.</span><span class="sxs-lookup"><span data-stu-id="0413a-146">Later in the script, use the `azure vm wait --name [vm-name]` command to wait for the VM to finish being created.</span></span>

<span data-ttu-id="0413a-147">Om du tittar på de utförliga svaren ser du också att namnet `SampleVM` används för att namnge olika beroenden för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0413a-147">If you look at the verbose responses, you will also see that the `SampleVM` name is used to name various dependencies for the VM.</span></span>

```output
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

<span data-ttu-id="0413a-148">Du kan åsidosätta dessa automatiskt genererade resursnamn med hjälp av valfria parametrar till `vm create`, till exempel `--vnet-name` och `--public-ip-address-dns-name`.</span><span class="sxs-lookup"><span data-stu-id="0413a-148">You can override these auto-generated resource names using optional parameters to `vm create`, such as `--vnet-name` and `--public-ip-address-dns-name`.</span></span>

<span data-ttu-id="0413a-149">Vi anger namnet på administratörskontot till **”aldis”** med hjälp av flaggan `admin-username`.</span><span class="sxs-lookup"><span data-stu-id="0413a-149">We are specifying the administrator account name through the `admin-username` flag to be **"aldis"**.</span></span> <span data-ttu-id="0413a-150">Om du utelämnar detta använder kommandot `vm create` _ditt aktuella användarnamn_.</span><span class="sxs-lookup"><span data-stu-id="0413a-150">If you omit this, the `vm create` command will use your _current user name_.</span></span> <span data-ttu-id="0413a-151">Eftersom reglerna för kontonamn är olika för varje operativsystem, är det säkrast att ange ett specifikt namn.</span><span class="sxs-lookup"><span data-stu-id="0413a-151">Since the rules for account names are different for each OS, it's safer to specify a specific name.</span></span> 

> [!NOTE]
> <span data-ttu-id="0413a-152">Vanliga namn, till exempel ”rot” och ”admin” tillåts inte för de flesta avbildningar.</span><span class="sxs-lookup"><span data-stu-id="0413a-152">Common names such as "root" and "admin" are not allowed for most images.</span></span>

<span data-ttu-id="0413a-153">Vi använder också flaggan `generate-ssh-keys`.</span><span class="sxs-lookup"><span data-stu-id="0413a-153">We are also using the `generate-ssh-keys` flag.</span></span> <span data-ttu-id="0413a-154">Den här parametern används för Linux-distributioner och skapar ett säkerhetsnyckelpar så att vi kan använda verktyget `ssh` för att få fjärråtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0413a-154">This parameter is used for Linux distributions and creates a pair of security keys so we can use the `ssh` tool to access the virtual machine remotely.</span></span> <span data-ttu-id="0413a-155">De två filerna placeras i mappen `.ssh` på din dator och på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0413a-155">The two files are placed into the `.ssh` folder on your machine and in the VM.</span></span> <span data-ttu-id="0413a-156">Om du redan har en SSH-nyckel med namnet `id_rsa` i målmappen, används den nyckeln i stället för att en ny nyckel genereras.</span><span class="sxs-lookup"><span data-stu-id="0413a-156">If you already have an SSH key named `id_rsa` in the target folder, then it will be used rather than having a new key generated.</span></span>

<span data-ttu-id="0413a-157">När den virtuella datorn har skapats får du ett JSON-svar som innehåller den virtuella datorns aktuella tillstånd och dess offentliga och privata IP-adresser som tilldelats av Azure:</span><span class="sxs-lookup"><span data-stu-id="0413a-157">Once it finishes creating the VM, you will get a JSON response which includes the current state of the virtual machine and its public and private IP addresses assigned by Azure:</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-58-F8-45",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
  "resourceGroup": "2568d0d0-efe3-4d04-a08f-df7f009f822a",
  "zones": ""
}
```
