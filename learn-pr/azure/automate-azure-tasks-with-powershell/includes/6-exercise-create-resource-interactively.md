<span data-ttu-id="db91f-101">Anta att du arbetar på ett företag som tillverkar en uppsättning verktyg för Linux-administration.</span><span class="sxs-lookup"><span data-stu-id="db91f-101">Suppose you work at a company that makes a suite of Linux admin tools.</span></span> <span data-ttu-id="db91f-102">Ditt jobb är att hjälpa potentiella kunder att testa programvaran innan de köper den.</span><span class="sxs-lookup"><span data-stu-id="db91f-102">Your job is to help potential customers try your software before they buy it.</span></span> <span data-ttu-id="db91f-103">Eftersom programvaran gör ändringar i operativsystemet på rotnivå har du valt att skapa en virtuell Linux-dator för varje utvärderingsversion.</span><span class="sxs-lookup"><span data-stu-id="db91f-103">Because the software makes root-level changes to the OS, you have decided to create a Linux VM for each trial customer.</span></span> <span data-ttu-id="db91f-104">Du skapar de virtuella datorer som behövs och tar bort dem i slutet av provperioden.</span><span class="sxs-lookup"><span data-stu-id="db91f-104">You create the VMs as needed and delete them at the end of the trial subscription.</span></span> <span data-ttu-id="db91f-105">På så sätt börjar varje kund börjar med en ren version av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="db91f-105">This way, each customer starts with a clean version of the OS.</span></span> 

<span data-ttu-id="db91f-106">Du håller de här virtuella datorerna åtskilda från företagets egna virtuella testdatorer genom att skapa en dedikerad resursgrupp för dem.</span><span class="sxs-lookup"><span data-stu-id="db91f-106">To keep these VMs separate from the VMs your company uses for internal testing, you will create a dedicated resource group to house them.</span></span> <span data-ttu-id="db91f-107">Du behöver bara en resursgrupp, så det är lämpligt att använda Azure PowerShell i interaktivt läge för den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="db91f-107">You only need one resource group so using Azure PowerShell in interactive mode is a reasonable choice for this task.</span></span>

## <a name="steps-to-create-a-resource-group"></a><span data-ttu-id="db91f-108">Steg för att skapar en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="db91f-108">Steps to create a resource group</span></span>

1. <span data-ttu-id="db91f-109">Starta PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db91f-109">Launch PowerShell.</span></span>

1. <span data-ttu-id="db91f-110">Importera modulen till den aktuella sessionen så att du kommer åt cmdletarna i Azure.</span><span class="sxs-lookup"><span data-stu-id="db91f-110">Import the module into the current session so you have access to the Azure cmdlets.</span></span>

   ```powershell
   Import-Module AzureRM
   ```

1. <span data-ttu-id="db91f-111">Anslut till Azure med kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="db91f-111">Connect to Azure using the command shown below.</span></span> <span data-ttu-id="db91f-112">När du har angett kommandot autentiserar du dig med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="db91f-112">After entering the command, authenticate by providing your Azure credentials.</span></span>

   ```powershell
   Connect-AzureRmAccount
   ```

1. <span data-ttu-id="db91f-113">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="db91f-113">Create a resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name "TrialsResourceGroup" -Location "East US"
    ```

1. <span data-ttu-id="db91f-114">Kontrollera att resursgruppen har skapats.</span><span class="sxs-lookup"><span data-stu-id="db91f-114">Verify the resource group was created successfully.</span></span>

    ```powershell
    Get-AzureRmResource | Format-Table
    ```
<span data-ttu-id="db91f-115">Du kan också kontrollera att resursgruppen har skapats i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="db91f-115">Another way to check whether the resource group was created successfully is to use the Azure Portal.</span></span> <span data-ttu-id="db91f-116">Det gör du genom att logga in på portalen och gå till avsnittet **Resursgrupper** (se nedan).</span><span class="sxs-lookup"><span data-stu-id="db91f-116">To do this, login to the Portal and navigate to the **Resource Groups** section (see below).</span></span> <span data-ttu-id="db91f-117">Den nya resursgruppen ska visas i listan.</span><span class="sxs-lookup"><span data-stu-id="db91f-117">The new resource group should be displayed in the list.</span></span>

![Använda portalen till att visa resursgrupper](../media-drafts/6-listing-resource-groups.png)

## <a name="summary"></a><span data-ttu-id="db91f-119">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="db91f-119">Summary</span></span>
<span data-ttu-id="db91f-120">I den här övningen visas ett vanligt mönster för en interaktiv PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="db91f-120">This exercise shows a common pattern for an interactive PowerShell session.</span></span> <span data-ttu-id="db91f-121">Du använde en vanlig cmdlet till att importera AzureRM-modulen och sedan Azure PowerShell-cmdletar till att utföra en viss uppgift.</span><span class="sxs-lookup"><span data-stu-id="db91f-121">You used a standard cmdlet to import the AzureRM module and then the Azure PowerShell cmdlets to perform a specific task.</span></span> <span data-ttu-id="db91f-122">Nu har du en resursgrupp i din prenumeration och är redo att skapa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="db91f-122">You now have a resource group in your subscription and are ready to create VMs.</span></span>