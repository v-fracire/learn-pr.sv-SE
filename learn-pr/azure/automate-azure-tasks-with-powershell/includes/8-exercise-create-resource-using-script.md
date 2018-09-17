<span data-ttu-id="11bae-101">I den här kursdelen fortsätter du med exemplet på ett företag som skapar administrationsverktyg för Linux.</span><span class="sxs-lookup"><span data-stu-id="11bae-101">In this unit, you will continue with the example of a company that makes Linux admin tools.</span></span> <span data-ttu-id="11bae-102">Kom ihåg att du tänker använda virtuella Linux-datorer så att potentiella kunder kan testa din programvara.</span><span class="sxs-lookup"><span data-stu-id="11bae-102">Recall that you plan to use Linux VMs to let potential customers test your software.</span></span> <span data-ttu-id="11bae-103">Du har en resursgrupp som är redo och nu är det dags att skapa de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="11bae-103">You have a resource group ready and now it is time to create the VMs.</span></span>

<span data-ttu-id="11bae-104">Företaget har betalat för en monter på en stor Linux-mässa.</span><span class="sxs-lookup"><span data-stu-id="11bae-104">Your company has paid for a booth at a big Linux trade show.</span></span> <span data-ttu-id="11bae-105">Du planerar en demonstration med tre terminaler som var och en är ansluten till en separat virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="11bae-105">You plan a demo area containing three terminals each connected to a separate Linux VM.</span></span> <span data-ttu-id="11bae-106">I slutet av varje dag vill du ta bort de virtuella datorerna och återskapa dem, så att de börjar från början varje morgon.</span><span class="sxs-lookup"><span data-stu-id="11bae-106">At the end of each day, you want to delete the VMs and recreate them, so they start fresh every morning.</span></span> <span data-ttu-id="11bae-107">Att skapa de virtuella datorerna manuellt efter jobbet när du är trött kan riskera att fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="11bae-107">Creating the VMs manually after work when you are tired would be error prone.</span></span> <span data-ttu-id="11bae-108">Du vill i stället skriva ett PowerShell-skript som skapar de virtuella datorerna automatiskt.</span><span class="sxs-lookup"><span data-stu-id="11bae-108">You want to write a PowerShell script to automate the VM creation process.</span></span>

## <a name="write-a-script-that-creates-virtual-machines"></a><span data-ttu-id="11bae-109">Skriva ett skript som skapar virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="11bae-109">Write a script that creates Virtual Machines</span></span>

<span data-ttu-id="11bae-110">Följ dessa steg för att skriva skriptet:</span><span class="sxs-lookup"><span data-stu-id="11bae-110">Follow these steps to write the script:</span></span>

1. <span data-ttu-id="11bae-111">Skapa en ny textfil med namnet **ConferenceDailyReset.ps1**.</span><span class="sxs-lookup"><span data-stu-id="11bae-111">Create a new text file named **ConferenceDailyReset.ps1**.</span></span>

1. <span data-ttu-id="11bae-112">Ange parametern i en variabel:</span><span class="sxs-lookup"><span data-stu-id="11bae-112">Capture the parameter in a variable:</span></span>

    ```powershell
    param([string]$resourceGroup)
    ```

1. <span data-ttu-id="11bae-113">Autentisera till Azure med dina autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="11bae-113">Authenticate with Azure using your credentials:</span></span>

    ```powershell
    Connect-AzureRmAccount
    ```

1. <span data-ttu-id="11bae-114">Fråga efter ett användarnamn och lösenord för den virtuella datorns administratörskonto och ange resultatet i en variabel:</span><span class="sxs-lookup"><span data-stu-id="11bae-114">Prompt for a username and password for the VM's admin account and capture the result in a variable:</span></span>

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. <span data-ttu-id="11bae-115">Skapa en loop som körs tre gånger:</span><span class="sxs-lookup"><span data-stu-id="11bae-115">Create a loop that executes three times:</span></span>

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. <span data-ttu-id="11bae-116">I loopens brödtext skapar du ett namn för varje virtuell dator och lagrar dem i en variabel:</span><span class="sxs-lookup"><span data-stu-id="11bae-116">In the loop body, create a name for each VM and store it in a variable:</span></span>

    ```powershell
    $vmName = "ConferenceDemo" + $i
    ```

1. <span data-ttu-id="11bae-117">Skapa därefter en virtuell dator med hjälp av variabeln `$vmName`:</span><span class="sxs-lookup"><span data-stu-id="11bae-117">Next, create a VM using the `$vmName` variable:</span></span>

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
   ```

1. <span data-ttu-id="11bae-118">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="11bae-118">Save the file.</span></span>

<span data-ttu-id="11bae-119">Det färdiga skriptet bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="11bae-119">The completed script should look like this:</span></span>

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

Connect-AzureRmAccount

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i

    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a><span data-ttu-id="11bae-120">Köra skriptet</span><span class="sxs-lookup"><span data-stu-id="11bae-120">Execute the script</span></span>

<span data-ttu-id="11bae-121">Starta PowerShell och gå till den katalog där du sparade skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="11bae-121">Launch PowerShell and change to the directory where you saved the script file.</span></span> <span data-ttu-id="11bae-122">Använd följande kommando för att köra skriptet:</span><span class="sxs-lookup"><span data-stu-id="11bae-122">To run the script, execute the following command:</span></span>

```powershell
.\ConferenceDailyReset.ps1 TrialsResourceGroup
```

<span data-ttu-id="11bae-123">Det kan ta några minuter att slutföra skriptet.</span><span class="sxs-lookup"><span data-stu-id="11bae-123">The script may take a few minutes to complete.</span></span> <span data-ttu-id="11bae-124">När det är klart kontrollerar du att det har körts:</span><span class="sxs-lookup"><span data-stu-id="11bae-124">When it is finished, verify that it ran successfully:</span></span>

<!---TODO: Update for sandbox?--->
1. <span data-ttu-id="11bae-125">Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true) i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="11bae-125">In a browser, sign into the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="11bae-126">Klicka på **Resursgrupper** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="11bae-126">In the navigation on the left, click **Resource Groups**.</span></span>

1. <span data-ttu-id="11bae-127">I listan med resursgrupper klickar du på **TrialsResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="11bae-127">In the list of resource groups, click **TrialsResourceGroup**.</span></span> <span data-ttu-id="11bae-128">I listan med resurser bör du se de nyligen skapade virtuella datorerna och deras associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="11bae-128">In the list of resources, you should see the newly created VMs and their associated resources.</span></span>

## <a name="summary"></a><span data-ttu-id="11bae-129">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="11bae-129">Summary</span></span>
<span data-ttu-id="11bae-130">Du skrev ett skript som automatiserade skapandet av tre virtuella datorer i resursgruppen med en skriptparameter.</span><span class="sxs-lookup"><span data-stu-id="11bae-130">You wrote a script that automated the creation of three VMs in the resource group indicated by a script parameter.</span></span> <span data-ttu-id="11bae-131">Skriptet är kort och enkelt, men automatiserar en process som tar lång tid att slutföra manuellt med portalen.</span><span class="sxs-lookup"><span data-stu-id="11bae-131">The script is short and simple but automates a process that would take a long time to complete manually with the portal.</span></span>