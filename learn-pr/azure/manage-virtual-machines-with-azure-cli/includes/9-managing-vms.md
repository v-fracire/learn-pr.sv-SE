<span data-ttu-id="1a8a8-101">En av de viktigaste åtgärderna du förmodligen vill göra medan de virtuella datorerna körs är att kunna starta och stoppa dem.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-101">One of the main tasks you'll want to do while running virtual machines is to start and stop them.</span></span>

## <a name="stopping-a-vm"></a><span data-ttu-id="1a8a8-102">Stoppa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1a8a8-102">Stopping a VM</span></span>

<span data-ttu-id="1a8a8-103">Vi kan stoppa en virtuell dator som körs med kommandot `vm stop`.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-103">We can stop a running VM with the `vm stop` command.</span></span> <span data-ttu-id="1a8a8-104">Du måste skicka namn och resursgrupp eller unikt ID för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="1a8a8-104">You must pass the name and resource group, or the unique ID for the VM:</span></span>

```azurecli
az vm stop -n SampleVM -g ExerciseResources
```

<span data-ttu-id="1a8a8-105">Vi kan kontrollera att den har stoppats genom att försöka pinga den offentliga IP-adressen med `ssh`, eller med kommandot `vm get-instance-view`.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-105">We can verify it has stopped by attempting to ping the public IP address, using `ssh`, or through the `vm get-instance-view` command.</span></span> <span data-ttu-id="1a8a8-106">Den sista metoden returnerar samma grundläggande data som `vm show`, men innehåller även information om själva instansen.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-106">This final approach returns the same basic data as `vm show` but includes details about the instance itself.</span></span> <span data-ttu-id="1a8a8-107">Försök med att skriva följande kommando i Azure Cloud Shell för att se aktuell körningsstatus för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="1a8a8-107">Try typing the following command into Azure Cloud Shell to see the current running state of your VM:</span></span>

```azurecli
az vm get-instance-view -n SampleVM -g ExerciseResources --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```

<span data-ttu-id="1a8a8-108">Det här kommandot ska returnera `VM stopped` som ett resultat.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-108">This command should return `VM stopped` as the result.</span></span>

## <a name="starting-a-vm"></a><span data-ttu-id="1a8a8-109">Starta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1a8a8-109">Starting a VM</span></span>

<span data-ttu-id="1a8a8-110">Vi kan göra det omvända med kommandot `vm start`.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-110">We can do the reverse through the `vm start` command.</span></span>

```azurecli
az vm start -n SampleVM -g ExerciseResources
```

<span data-ttu-id="1a8a8-111">Det här kommandot startar en stoppad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-111">This command will start a stopped VM.</span></span> <span data-ttu-id="1a8a8-112">Vi kan kontrollera det med frågan `vm get-instance-view`, som nu borde returnera `VM running`.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-112">We can verify it through the `vm get-instance-view` query, which should now return `VM running`.</span></span>

## <a name="restarting-a-vm"></a><span data-ttu-id="1a8a8-113">Starta om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1a8a8-113">Restarting a VM</span></span>

<span data-ttu-id="1a8a8-114">Slutligen om vi har gjort ändringar som kräver en omstart, kan vi starta om en virtuell dator med hjälp av kommandot `vm restart`.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-114">Finally, we can restart a VM if we have made changes that require a reboot using the `vm restart` command.</span></span> <span data-ttu-id="1a8a8-115">Du kan lägga till `--no-wait`-flaggan om du vill att Azure CLI ska returneras omedelbart, utan att vänta tills den virtuella datorn har startats om.</span><span class="sxs-lookup"><span data-stu-id="1a8a8-115">You can add the `--no-wait` flag if you want the Azure CLI to return immediately without waiting for the VM to reboot.</span></span>

