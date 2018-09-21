<span data-ttu-id="099a8-101">I den här kursdelen fortsätter du med exemplet på ett företag som skapar administrationsverktyg för Linux.</span><span class="sxs-lookup"><span data-stu-id="099a8-101">In this unit, you will continue with the example of a company that makes Linux admin tools.</span></span> <span data-ttu-id="099a8-102">Kom ihåg att du tänker använda virtuella Linux-datorer så att potentiella kunder kan testa din programvara.</span><span class="sxs-lookup"><span data-stu-id="099a8-102">Recall that you plan to use Linux VMs to let potential customers test your software.</span></span> <span data-ttu-id="099a8-103">Du har en resursgrupp som är redo och nu är det dags att skapa de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="099a8-103">You have a resource group ready and now it is time to create the VMs.</span></span>

<span data-ttu-id="099a8-104">Företaget har betalat för en monter på en stor Linux-mässa.</span><span class="sxs-lookup"><span data-stu-id="099a8-104">Your company has paid for a booth at a big Linux trade show.</span></span> <span data-ttu-id="099a8-105">Du planerar en demonstration med tre terminaler som var och en är ansluten till en separat virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="099a8-105">You plan a demo area containing three terminals each connected to a separate Linux VM.</span></span> <span data-ttu-id="099a8-106">I slutet av varje dag vill du ta bort de virtuella datorerna och återskapa dem, så att de börjar från början varje morgon.</span><span class="sxs-lookup"><span data-stu-id="099a8-106">At the end of each day, you want to delete the VMs and recreate them, so they start fresh every morning.</span></span> <span data-ttu-id="099a8-107">Att skapa de virtuella datorerna manuellt efter jobbet när du är trött kan riskera att fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="099a8-107">Creating the VMs manually after work when you are tired would be error prone.</span></span> <span data-ttu-id="099a8-108">Du vill i stället skriva ett PowerShell-skript som skapar de virtuella datorerna automatiskt.</span><span class="sxs-lookup"><span data-stu-id="099a8-108">You want to write a PowerShell script to automate the VM creation process.</span></span>

## <a name="write-a-script-that-creates-virtual-machines"></a><span data-ttu-id="099a8-109">Skriva ett skript som skapar virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="099a8-109">Write a script that creates Virtual Machines</span></span>

<span data-ttu-id="099a8-110">Följ dessa anvisningar i Cloud Shell till höger för att skriva skriptet:</span><span class="sxs-lookup"><span data-stu-id="099a8-110">Follow these steps in the Cloud Shell on the right to write the script:</span></span>

1. <span data-ttu-id="099a8-111">Växla till arbetsmappen i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="099a8-111">Switch to your home folder in the Cloud Shell.</span></span>

    ```powershell
    cd $HOME\clouddrive
    ```

1. <span data-ttu-id="099a8-112">Skapa en ny textfil med namnet **ConferenceDailyReset.ps1**.</span><span class="sxs-lookup"><span data-stu-id="099a8-112">Create a new text file named **ConferenceDailyReset.ps1**.</span></span>

    ```powershell
    touch "./ConferenceDailyReset.ps1"
    ```

1. <span data-ttu-id="099a8-113">Öppna den inbyggda redigeraren och välj filen **ConferenceDailyReset.ps1**.</span><span class="sxs-lookup"><span data-stu-id="099a8-113">Open the integrated editor and select the **ConferenceDailyReset.ps1** file.</span></span>

    ```powershell
    code "./ConferenceDailyReset.ps1"
    ```
    > [!TIP]
    > <span data-ttu-id="099a8-114">Det inbyggda Cloud Shell stöder även vim, nano och emacs om du föredrar att använda en av dessa redigerare.</span><span class="sxs-lookup"><span data-stu-id="099a8-114">The integrated Cloud Shell also supports vim, nano, and emacs if you'd prefer to use one of those editors.</span></span>

1. <span data-ttu-id="099a8-115">Börja med att spara indataparametern i en variabel.</span><span class="sxs-lookup"><span data-stu-id="099a8-115">Start by capturing the input parameter in a variable.</span></span> <span data-ttu-id="099a8-116">Lägg till följande rad i skriptet:</span><span class="sxs-lookup"><span data-stu-id="099a8-116">Add the following line to your script:</span></span>

    ```powershell
    param([string]$resourceGroup)
    ```

    > [!NOTE]
    > <span data-ttu-id="099a8-117">Normalt skulle du behöva autentisera med Azure med dina autentiseringsuppgifter via `Connect-AzureRmAccount`, och detta skulle kunna göras i skriptet.</span><span class="sxs-lookup"><span data-stu-id="099a8-117">Normally, you'd have to authenticate with Azure using your credentials using `Connect-AzureRmAccount` and this could be done in the script.</span></span> <span data-ttu-id="099a8-118">I Cloud Shell-miljön är du dock redan autentiserad, så det behövs inte.</span><span class="sxs-lookup"><span data-stu-id="099a8-118">However, in the Cloud Shell environment you will already be authenticated so this is unnecessary.</span></span>

1. <span data-ttu-id="099a8-119">Fråga efter ett användarnamn och lösenord för den virtuella datorns administratörskonto och ange resultatet i en variabel:</span><span class="sxs-lookup"><span data-stu-id="099a8-119">Prompt for a username and password for the VM's admin account and capture the result in a variable:</span></span>

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. <span data-ttu-id="099a8-120">Skapa en loop som körs tre gånger:</span><span class="sxs-lookup"><span data-stu-id="099a8-120">Create a loop that executes three times:</span></span>

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. <span data-ttu-id="099a8-121">I loopens brödtext skapar du ett namn för varje virtuell dator och lagrar dem i en variabel och skriver ut den i konsolen:</span><span class="sxs-lookup"><span data-stu-id="099a8-121">In the loop body, create a name for each VM and store it in a variable and output it to the console:</span></span>

    ```powershell
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    ```

1. <span data-ttu-id="099a8-122">Skapa därefter en virtuell dator med hjälp av variabeln `$vmName`:</span><span class="sxs-lookup"><span data-stu-id="099a8-122">Next, create a VM using the `$vmName` variable:</span></span>

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
   ```

1. <span data-ttu-id="099a8-123">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="099a8-123">Save the file.</span></span> <span data-ttu-id="099a8-124">Du kan använda menyn ”...” i det övre högra hörnet i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="099a8-124">You can use the "..." menu at the top right corner of the editor.</span></span> <span data-ttu-id="099a8-125">Det finns även vanliga kortkommandon för Spara.</span><span class="sxs-lookup"><span data-stu-id="099a8-125">There are also common accelerator keys for Save.</span></span>

<span data-ttu-id="099a8-126">Det färdiga skriptet bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="099a8-126">The completed script should look like this:</span></span>

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a><span data-ttu-id="099a8-127">Köra skriptet</span><span class="sxs-lookup"><span data-stu-id="099a8-127">Execute the script</span></span>

<span data-ttu-id="099a8-128">Stäng redigeraren via snabbmenyn ”...”.</span><span class="sxs-lookup"><span data-stu-id="099a8-128">Close the editor through the "..." context menu.</span></span>

1. <span data-ttu-id="099a8-129">Kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="099a8-129">Execute the script.</span></span>

    ```powershell
    .\ConferenceDailyReset.ps1 <rgn>[Sandbox resource group name]</rgn>
    ```
    
<span data-ttu-id="099a8-130">Skriptet kommer att ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="099a8-130">The script will take several minutes to complete.</span></span> <span data-ttu-id="099a8-131">När det är klart kontrollerar du att det har körts genom att titta på de resurser som du nu har i resursgruppen:</span><span class="sxs-lookup"><span data-stu-id="099a8-131">When it is finished, verify that it ran successfully by looking at the resources you now have in your resource group:</span></span>

```powershell
Get-AzureRmResource -ResourceType Microsoft.Compute/virtualMachines
```

<span data-ttu-id="099a8-132">Du bör se tre virtuella datorer – var och en med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="099a8-132">You should see three VMs, each with a unique name.</span></span>

<span data-ttu-id="099a8-133">Du skrev ett skript som automatiserade skapandet av tre virtuella datorer i resursgruppen med en skriptparameter.</span><span class="sxs-lookup"><span data-stu-id="099a8-133">You wrote a script that automated the creation of three VMs in the resource group indicated by a script parameter.</span></span> <span data-ttu-id="099a8-134">Skriptet är kort och enkelt, men automatiserar en process som tar lång tid att slutföra manuellt med portalen.</span><span class="sxs-lookup"><span data-stu-id="099a8-134">The script is short and simple but automates a process that would take a long time to complete manually with the portal.</span></span>