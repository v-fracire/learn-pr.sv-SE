<span data-ttu-id="b5760-101">Azure-portalen är det enklaste sättet att skapa resurser såsom virtuella datorer när du kör igång.</span><span class="sxs-lookup"><span data-stu-id="b5760-101">The Azure portal is the easiest way to create resources such as VMs when you are getting started.</span></span> <span data-ttu-id="b5760-102">Det är dock inte nödvändigtvis det effektivaste eller snabbaste sättet att arbeta med Azure, särskilt om du vill skapa flera resurser tillsammans.</span><span class="sxs-lookup"><span data-stu-id="b5760-102">However, it's not necessarily the most efficient or quickest way to work with Azure, particularly if you need to create several resources together.</span></span> <span data-ttu-id="b5760-103">I vårt fall kommer vi så småningom att skapa flera virtuella datorer för att hantera olika uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b5760-103">In our case, we will eventually be creating dozens of VMs to handle different tasks.</span></span> <span data-ttu-id="b5760-104">Det vore inte särskilt roligt att skapa dem manuellt i Azure-portalen!</span><span class="sxs-lookup"><span data-stu-id="b5760-104">Creating them manually in the Azure portal wouldn't be a fun task!</span></span>

<span data-ttu-id="b5760-105">Vi tittar på några andra metoder för att skapa och administrera resurser i Azure:</span><span class="sxs-lookup"><span data-stu-id="b5760-105">Let's look at some other ways to create and administer resources in Azure:</span></span>

- <span data-ttu-id="b5760-106">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b5760-106">Azure Resource Manager</span></span>
- <span data-ttu-id="b5760-107">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b5760-107">Azure PowerShell</span></span>
- <span data-ttu-id="b5760-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b5760-108">Azure CLI</span></span>
- <span data-ttu-id="b5760-109">Azure REST API</span><span class="sxs-lookup"><span data-stu-id="b5760-109">Azure REST API</span></span>
- <span data-ttu-id="b5760-110">Azure Client SDK</span><span class="sxs-lookup"><span data-stu-id="b5760-110">Azure Client SDK</span></span>
- <span data-ttu-id="b5760-111">Azure VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="b5760-111">Azure VM Extensions</span></span>
- <span data-ttu-id="b5760-112">Azure Automation Services</span><span class="sxs-lookup"><span data-stu-id="b5760-112">Azure Automation Services</span></span>

## <a name="azure-resource-manager"></a><span data-ttu-id="b5760-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b5760-113">Azure Resource Manager</span></span>

<span data-ttu-id="b5760-114">Anta att du vill skapa en kopia av en virtuell dator med samma inställningar.</span><span class="sxs-lookup"><span data-stu-id="b5760-114">Let's assume you want to create a copy of a VM with the same settings.</span></span> <span data-ttu-id="b5760-115">Du kan skapa en avbildning av en virtuell dator, överföra den till Azure och referera till den som underlag för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b5760-115">You could create a VM image, upload it to Azure, and reference it as the basis for your new VM.</span></span> <span data-ttu-id="b5760-116">Den här processen är ineffektiv och tidskrävande.</span><span class="sxs-lookup"><span data-stu-id="b5760-116">This process is inefficient and time-consuming.</span></span> <span data-ttu-id="b5760-117">Med Azure kan du skapa en mall och använda den för att skapa en exakt kopia av en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b5760-117">Azure provides you with the option to create a template from which to create an exact copy of a VM.</span></span>

<span data-ttu-id="b5760-118">Vanligtvis innehåller din Azure-infrastruktur många resurser, varav många är relaterade till varandra på något sätt.</span><span class="sxs-lookup"><span data-stu-id="b5760-118">Typically, your Azure infrastructure will contain many resources, many of them related to one another in some way.</span></span> <span data-ttu-id="b5760-119">Till exempel har den virtuella dator som vi skapade den virtuella datorn själv, lagring, nätverksgränssnitt, webbserver och en databasen – alla skapade tillsammans för att köra WordPress-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="b5760-119">For example, the VM we created has the virtual machine itself, storage, network interface, web server, and a database - all created together to run the WordPress site.</span></span> <span data-ttu-id="b5760-120">**Azure Resource Manager** gör det effektivare att arbeta med dessa relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="b5760-120">**Azure Resource Manager** makes working with these related resources more efficient.</span></span> <span data-ttu-id="b5760-121">Den ordnar resurser i namngivna **resursgrupper** som du kan använda för att distribuera, uppdatera eller ta bort alla resurser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="b5760-121">It organizes resources into named **resource groups** that let you deploy, update, or delete all of the resources together.</span></span> <span data-ttu-id="b5760-122">När vi skapade WordPress-webbplatsen identifierade vi resursgruppen som en del av att skapa den virtuella datorn. Resource Manager placerade därefter de associerade resurserna i samma grupp.</span><span class="sxs-lookup"><span data-stu-id="b5760-122">When we created the WordPress site, we identified the resource group as part of the VM creation, and Resource Manager placed the associated resources into the same group.</span></span>

<span data-ttu-id="b5760-123">Med Resource Manager kan du även skapa _mallar_ som kan användas för att skapa och distribuera specifika konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="b5760-123">Resource Manager also allows you to create _templates_, which can be used to create and deploy specific configurations.</span></span>

### <a name="what-are-resource-manager-templates"></a><span data-ttu-id="b5760-124">Vad är Resource Manager-mallar?</span><span class="sxs-lookup"><span data-stu-id="b5760-124">What are Resource Manager templates?</span></span>

<span data-ttu-id="b5760-125">**Resource Manager-mallar** är JSON-filer som definierar de resurser du behöver för att distribuera lösningen.</span><span class="sxs-lookup"><span data-stu-id="b5760-125">**Resource Manager templates** are JSON files that define the resources you need to deploy for your solution.</span></span>

<span data-ttu-id="b5760-126">Du kan skapa resursmallar från avsnittet **Inställningar** för en specifik virtuell dator genom att välja alternativet för automatiseringsskript.</span><span class="sxs-lookup"><span data-stu-id="b5760-126">You can create resource templates from the **Settings** section for a specific VM by selecting the Automation script option.</span></span>

![Automatiseringsskript för vår virtuella dator](../media/4-automation-script.png)

<span data-ttu-id="b5760-128">Du har möjlighet att spara resursmallen för senare användning eller omedelbart distribuera en ny virtuell dator baserat på den här mallen.</span><span class="sxs-lookup"><span data-stu-id="b5760-128">You have the option to save the resource template for later use or immediately deploy a new VM based on this template.</span></span> <span data-ttu-id="b5760-129">Till exempel skapar du kanske en virtuell dator från en mall i en testmiljö och upptäcker att den inte riktigt fungerar som ersättning för din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="b5760-129">For example, you might create a VM from a template in a test environment and find it doesn’t quite work to replace your on-premises machine.</span></span> <span data-ttu-id="b5760-130">Du kan ta bort resursgruppen, vilket tar bort alla resurser, justera mallen och försöka igen.</span><span class="sxs-lookup"><span data-stu-id="b5760-130">You can delete the resource group, which deletes all of the resources, tweak the template, and try again.</span></span> <span data-ttu-id="b5760-131">Om du bara vill göra ändringar i befintliga distribuerade resurser kan du ändra den mall som används för att skapa den och distribuera den igen.</span><span class="sxs-lookup"><span data-stu-id="b5760-131">If you only want to make changes to the existing deployed resources, you can change the template used to create it and deploy it again.</span></span> <span data-ttu-id="b5760-132">Resource Manager ändrar resurserna för att matcha den nya mallen.</span><span class="sxs-lookup"><span data-stu-id="b5760-132">Resource Manager will change the resources to match the new template.</span></span>

<span data-ttu-id="b5760-133">När det fungerar på det sätt du vill kan du använda den mallen och enkelt skapa flera versioner av infrastrukturen, till exempel mellanlagring och produktion.</span><span class="sxs-lookup"><span data-stu-id="b5760-133">Once you have it working the way you want it, you can take that template and easily re-create multiple versions of your infrastructure, such as staging and production.</span></span> <span data-ttu-id="b5760-134">Du kan parameterisera fält som den virtuella datorns namn, nätverksnamn, lagringskontonamn osv. och läsa in mallen upprepade gånger med olika parametrar för att anpassa varje miljö.</span><span class="sxs-lookup"><span data-stu-id="b5760-134">You can parameterize fields such as the VM name, network name, storage account name, etc., and load the template repeatedly, using different parameters to customize each environment.</span></span>

<span data-ttu-id="b5760-135">Du kan använda skriptverktyg för automatisering som Azure CLI, Azure PowerShell eller till och med Azures REST API:er med det programmeringsspråk du föredrar för att bearbeta resursmallarna. Detta gör det här till ett kraftfullt verktyg för att snabbt få fart på din infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="b5760-135">You can use automation scripting tools such as the Azure CLI, Azure PowerShell, or even the Azure REST APIs with your favorite programming language to process resource templates, making this a powerful tool for quickly spinning up your infrastructure.</span></span>

## <a name="azure-powershell"></a><span data-ttu-id="b5760-136">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b5760-136">Azure PowerShell</span></span>

<span data-ttu-id="b5760-137">Att skapa administrationsskript är ett kraftfullt sätt att optimera ditt arbetsflöde på.</span><span class="sxs-lookup"><span data-stu-id="b5760-137">Creating administration scripts is a powerful way to optimize your workflow.</span></span> <span data-ttu-id="b5760-138">Du kan automatisera vanliga, repetitiva uppgifter och när ett skript har verifierats kan det kan köras konsekvent, vilket minskar risken för fel.</span><span class="sxs-lookup"><span data-stu-id="b5760-138">You can automate everyday, repetitive tasks, and once a script has been verified, it will run consistently, likely reducing errors.</span></span> <span data-ttu-id="b5760-139">**Azure PowerShell** är perfekt för interaktiva engångsuppgifter och/eller automatisering av återkommande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b5760-139">**Azure PowerShell** is ideal for one-off interactive tasks and/or the automation of repeated tasks.</span></span>

> [!NOTE]
> <span data-ttu-id="b5760-140">PowerShell är ett plattformsoberoende gränssnitt som tillhandahåller tjänster som gränssnittsfönstret och kommandoparsning.</span><span class="sxs-lookup"><span data-stu-id="b5760-140">PowerShell is a cross-platform shell that provides services like the shell window and command parsing.</span></span> <span data-ttu-id="b5760-141">Azure PowerShell är ett valfritt tilläggspaket som lägger till Azure-specifika kommandon (kallas för **cmdletar**).</span><span class="sxs-lookup"><span data-stu-id="b5760-141">Azure PowerShell is an optional add-on package that adds the Azure-specific commands (referred to as **cmdlets**).</span></span> <span data-ttu-id="b5760-142">Du kan lära dig mer om att installera och använda Azure PowerShell i en separat utbildningsmodul.</span><span class="sxs-lookup"><span data-stu-id="b5760-142">You can learn more about installing and using Azure PowerShell in a separate training module.</span></span>

<span data-ttu-id="b5760-143">Du kan till exempel använda cmdleten `New-AzureRmVM` för att skapa en ny virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="b5760-143">For example, you can use the `New-AzureRmVM` cmdlet to create a new Azure virtual machine.</span></span>

```powershell
New-AzureRmVm `
    -ResourceGroupName "TestResourceGroup" `
    -Name "test-wp1-eus-vm" `
    -Location "East US" `
    -VirtualNetworkName "test-wp1-eus-network" `
    -SubnetName "default" `
    -SecurityGroupName "test-wp1-eus-nsg" `
    -PublicIpAddressName "test-wp1-eus-pubip" `
    -OpenPorts 80,3389
```

<span data-ttu-id="b5760-144">Enligt vad som visas här kan du ange olika parametrar för att hantera det stora antalet tillgängliga konfigurationsinställningar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b5760-144">As shown here, you supply various parameters to handle the large number of VM configuration settings available.</span></span> <span data-ttu-id="b5760-145">De flesta av parametrarna har rimliga värden, så du behöver bara ange de parametrar som krävs.</span><span class="sxs-lookup"><span data-stu-id="b5760-145">Most of the parameters have reasonable values; you only need to specify the required parameters.</span></span> <span data-ttu-id="b5760-146">Läs mer om att skapa och hantera virtuella datorer med Azure PowerShell i modulen **Automatisera Azure-åtgärder med hjälp av PowerShell-skript**.</span><span class="sxs-lookup"><span data-stu-id="b5760-146">Learn more about creating and managing VMs with Azure PowerShell in the **Automate Azure tasks using scripts with PowerShell** module.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="b5760-147">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b5760-147">Azure CLI</span></span>

<span data-ttu-id="b5760-148">Ett annat alternativ för Azure interaktion via skriptning och kommandorad är **Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="b5760-148">Another option for scripting and command-line Azure interaction is the **Azure CLI**.</span></span>

<span data-ttu-id="b5760-149">Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser såsom virtuella datorer och diskar från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b5760-149">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources such as virtual machines and disks from the command line.</span></span> <span data-ttu-id="b5760-150">Det finns för macOS, Linux och Windows eller i webbläsaren med hjälp av Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b5760-150">It's available for macOS, Linux, and Windows, or in the browser using the Cloud Shell.</span></span> <span data-ttu-id="b5760-151">Liksom Azure PowerShell är Azure CLI ett kraftfullt sätt att effektivisera det administrativa arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="b5760-151">Like Azure PowerShell, the Azure CLI is a powerful way to streamline your administrative workflow.</span></span> <span data-ttu-id="b5760-152">Till skillnad från Azure PowerShell behöver Azure CLI inte PowerShell för att fungera.</span><span class="sxs-lookup"><span data-stu-id="b5760-152">Unlike Azure PowerShell, the Azure CLI does not need PowerShell to function.</span></span>

<span data-ttu-id="b5760-153">Du kan till exempel skapa en virtuell Azure-dator med kommandot `az vm create`.</span><span class="sxs-lookup"><span data-stu-id="b5760-153">For example, you can create an Azure VM with the `az vm create` command.</span></span>

```azurecli
az vm create \
    --resource-group TestResourceGroup \
    --name test-wp1-eus-vm \
    --image win2016datacenter \
    --admin-username jonc \
    --admin-password aReallyGoodPasswordHere
```

<span data-ttu-id="b5760-154">Azure CLI kan användas med andra skriptspråk, till exempel Ruby och Python.</span><span class="sxs-lookup"><span data-stu-id="b5760-154">The Azure CLI can be used with other scripting languages, for example, Ruby and Python.</span></span> <span data-ttu-id="b5760-155">Båda språken används ofta på icke-Windows-baserade datorer där utvecklaren kanske inte är bekant med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b5760-155">Both languages are commonly used on non-Windows-based machines where the developer might not be familiar with PowerShell.</span></span>

<span data-ttu-id="b5760-156">Läs mer om att skapa och hantera virtuella datorer i modulen **Hantera virtuella datorer med Azure CLI-verktyget**.</span><span class="sxs-lookup"><span data-stu-id="b5760-156">Learn more about creating and managing VMs in the **Manage virtual machines with the Azure CLI tool** module.</span></span>

## <a name="programmatic-apis"></a><span data-ttu-id="b5760-157">Programmatiskt (API:er)</span><span class="sxs-lookup"><span data-stu-id="b5760-157">Programmatic (APIs)</span></span>

<span data-ttu-id="b5760-158">Både Azure PowerShell och Azure CLI är generellt sett bra alternativ om du har enkla skript som ska köras och om du vill hålla dig till kommandoradsverktygen.</span><span class="sxs-lookup"><span data-stu-id="b5760-158">Generally speaking, both Azure PowerShell and Azure CLI are good options if you have simple scripts to run and want to stick to command-line tools.</span></span> <span data-ttu-id="b5760-159">När det gäller mer komplicerade scenarier där skapandet och hanteringen av virtuella datorer utgör en del av ett större program med komplex logik krävs en annan metod.</span><span class="sxs-lookup"><span data-stu-id="b5760-159">When it comes to more complex scenarios, where the creation and management of VMs form part of a larger application with complex logic, another approach is needed.</span></span>

<span data-ttu-id="b5760-160">Du kan interagera med alla typer av resurser i Azure programmatiskt.</span><span class="sxs-lookup"><span data-stu-id="b5760-160">You can interact with every type of resource in Azure programmatically.</span></span>

### <a name="azure-rest-api"></a><span data-ttu-id="b5760-161">Azure REST API</span><span class="sxs-lookup"><span data-stu-id="b5760-161">Azure REST API</span></span>

<span data-ttu-id="b5760-162">Med Azure REST API får utvecklarna sina åtgärder kategoriserade efter resurs, samt möjlighet att skapa och hantera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b5760-162">The Azure REST API provides developers with operations categorized by resource as well as the ability to create and manage VMs.</span></span> <span data-ttu-id="b5760-163">Åtgärder görs tillgängliga som URI:er med motsvarande HTTP-metoder (`GET`, `PUT`, `POST`, `DELETE` och `PATCH`) och ett motsvarande svar.</span><span class="sxs-lookup"><span data-stu-id="b5760-163">Operations are exposed as URIs with corresponding HTTP methods (`GET`, `PUT`, `POST`, `DELETE`, and `PATCH`) and a corresponding response.</span></span>

<span data-ttu-id="b5760-164">Med Azure Compute-API:erna får du programmatisk åtkomst till virtuella datorer och deras stödresurser.</span><span class="sxs-lookup"><span data-stu-id="b5760-164">The Azure Compute APIs give you programmatic access to virtual machines and their supporting resources.</span></span> <span data-ttu-id="b5760-165">Med detta API får du åtgärder för att:</span><span class="sxs-lookup"><span data-stu-id="b5760-165">With this API, you have operations to:</span></span>

- <span data-ttu-id="b5760-166">Skapa och hantera tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="b5760-166">Create and manage availability sets</span></span>
- <span data-ttu-id="b5760-167">Lägga till och hantera VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="b5760-167">Add and manage virtual machine extensions</span></span>
- <span data-ttu-id="b5760-168">Skapa och hantera hanterade diskar, ögonblicksbilder och avbildningar</span><span class="sxs-lookup"><span data-stu-id="b5760-168">Create and manage managed disks, snapshots, and images</span></span>
- <span data-ttu-id="b5760-169">Komma åt plattformsavbildningar som är tillgängliga i Azure</span><span class="sxs-lookup"><span data-stu-id="b5760-169">Access the platform images available in Azure</span></span>
- <span data-ttu-id="b5760-170">Hämta användningsinformation för dina resurser</span><span class="sxs-lookup"><span data-stu-id="b5760-170">Retrieve usage information of your resources</span></span>
- <span data-ttu-id="b5760-171">Skapa och hantera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b5760-171">Create and manage virtual machines</span></span>
- <span data-ttu-id="b5760-172">Skapa och hantera VM-skalningsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="b5760-172">Create and manage virtual machine scale sets</span></span>

### <a name="azure-client-sdk"></a><span data-ttu-id="b5760-173">Azure Client SDK</span><span class="sxs-lookup"><span data-stu-id="b5760-173">Azure Client SDK</span></span>

<span data-ttu-id="b5760-174">Även om REST API är plattforms- och språkoberoende, vill de flesta utvecklare ha en högre abstraktionsnivå.</span><span class="sxs-lookup"><span data-stu-id="b5760-174">Even though the REST API is platform and language agnostic, most often developers will look toward a higher level of abstraction.</span></span> <span data-ttu-id="b5760-175">Azure Client SDK kapslar in Azure REST API, vilket gör det enklare för utvecklarna att interagera med Azure.</span><span class="sxs-lookup"><span data-stu-id="b5760-175">The Azure Client SDK encapsulates the Azure REST API, making it much easier for developers to interact with Azure.</span></span>

<span data-ttu-id="b5760-176">Azure Client SDK:er är tillgängliga för olika språk och ramverk, däribland .NET-baserade språk som C#, Java, Node.js, PHP, Python, Ruby och Go.</span><span class="sxs-lookup"><span data-stu-id="b5760-176">The Azure Client SDKs are available for a variety of languages and frameworks, including .NET-based languages such as C#, Java, Node.js, PHP, Python, Ruby, and Go.</span></span>

<span data-ttu-id="b5760-177">Här är ett kodfragment med C#-kod som skapar en virtuell Azure-dator med hjälp av NuGet-paketet `Microsoft.Azure.Management.Fluent`:</span><span class="sxs-lookup"><span data-stu-id="b5760-177">Here's an example snippet of C# code to create an Azure VM using the `Microsoft.Azure.Management.Fluent` NuGet package:</span></span>

```csharp
var azure = Azure
    .Configure()
    .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
    .Authenticate(credentials)
    .WithDefaultSubscription();
// ...
var vmName = "test-wp1-eus-vm";

azure.VirtualMachines.Define(vmName)
    .WithRegion(Region.USEast)
    .WithExistingResourceGroup("TestResourceGroup")
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("jonc")
    .WithAdminPassword("aReallyGoodPasswordHere")
    .WithComputerName(vmName)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

<span data-ttu-id="b5760-178">Här är samma kodavsnitt i Java med hjälp av **Azure Java SDK**:</span><span class="sxs-lookup"><span data-stu-id="b5760-178">Here's the same snippet in Java using the **Azure Java SDK**:</span></span>

```java
String vmName = "test-wp1-eus-vm";
// ...
VirtualMachine virtualMachine = azure.virtualMachines()
    .define(vmName)
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("TestResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("jonc")
    .withAdminPassword("aReallyGoodPasswordHere")
    .withComputerName(vmName)
    .withSize("Standard_DS1")
    .create();
```

## <a name="azure-vm-extensions"></a><span data-ttu-id="b5760-179">Azure VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="b5760-179">Azure VM Extensions</span></span>

<span data-ttu-id="b5760-180">Anta att du vill konfigurera och installera ytterligare programvara på din virtuella dator efter den första distributionen.</span><span class="sxs-lookup"><span data-stu-id="b5760-180">Let's assume you want to configure and install additional software on your virtual machine after the initial deployment.</span></span> <span data-ttu-id="b5760-181">Du vill att den här uppgiften ska använda en specifik konfiguration, övervakas och köras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b5760-181">You want this task to use a specific configuration, monitored and executed automatically.</span></span>

<span data-ttu-id="b5760-182">**Azure VM-tillägg** är små program som gör att du kan konfigurera och automatisera uppgifter på virtuella Azure-datorer efter den första distributionen.</span><span class="sxs-lookup"><span data-stu-id="b5760-182">**Azure VM extensions** are small applications that allow you to configure and automate tasks on Azure VMs after initial deployment.</span></span> <span data-ttu-id="b5760-183">**Azure VM-tillägg** kan köras med Azure CLI, PowerShell, Azure Resource Manager-mallar och Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b5760-183">**Azure VM extensions** can be run with the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal.</span></span>

<span data-ttu-id="b5760-184">Du paketerar tillägg med en ny VM-distribution eller kör dem mot ett befintligt system.</span><span class="sxs-lookup"><span data-stu-id="b5760-184">You bundle extensions with a new VM deployment or run them against an existing system.</span></span>

## <a name="azure-automation-services"></a><span data-ttu-id="b5760-185">Azure Automation Services</span><span class="sxs-lookup"><span data-stu-id="b5760-185">Azure Automation Services</span></span>

<span data-ttu-id="b5760-186">Att spara tid, minska fel och öka effektiviteten är några av de viktigaste driftsutmaningarna vid hantering av infrastruktur för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="b5760-186">Saving time, reducing errors, and increasing efficiency are some of the most significant operational management challenges faced when managing remote infrastructure.</span></span> <span data-ttu-id="b5760-187">Om du har flera infrastrukturtjänster bör du överväga att använda tjänster på högre nivå i Azure för att underlätta ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="b5760-187">If you have a lot of infrastructure services, you might want to consider using higher-level services in Azure to help you operate from a higher level.</span></span>

<span data-ttu-id="b5760-188">Med **Azure Automation** kan du integrera tjänster för att enkelt automatisera återkommande, tidskrävande och felbenägna hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b5760-188">**Azure Automation** allows you to integrate services that allow you to automate frequent, time-consuming, and error-prone management tasks with ease.</span></span> <span data-ttu-id="b5760-189">Tjänsterna omfattar bland annat **processautomatisering**, **konfigurationshantering** och **uppdateringshantering**.</span><span class="sxs-lookup"><span data-stu-id="b5760-189">These services include **process automation**, **configuration management**, and **update management**.</span></span>

- <span data-ttu-id="b5760-190">**Processhantering**.</span><span class="sxs-lookup"><span data-stu-id="b5760-190">**Process Management**.</span></span> <span data-ttu-id="b5760-191">Anta att du har en virtuell dator som övervakas för en särskild felhändelse.</span><span class="sxs-lookup"><span data-stu-id="b5760-191">Let's assume you have a VM that is monitored for a specific error event.</span></span> <span data-ttu-id="b5760-192">Du vill vidta åtgärder och lösa problemet så fort det rapporteras.</span><span class="sxs-lookup"><span data-stu-id="b5760-192">You want to take action and fix the problem as soon as it's reported.</span></span> <span data-ttu-id="b5760-193">Med processautomatisering kan du konfigurera bevakaruppgifter som kan svara på händelser som kan uppstå i datacentret.</span><span class="sxs-lookup"><span data-stu-id="b5760-193">Process automation allows you to set up watcher tasks that can respond to events that may occur in your datacenter.</span></span>

- <span data-ttu-id="b5760-194">**Konfigurationshantering**.</span><span class="sxs-lookup"><span data-stu-id="b5760-194">**Configuration Management**.</span></span>  <span data-ttu-id="b5760-195">Du vill kanske spåra programuppdateringar som blir tillgängliga för det operativsystem som körs på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b5760-195">Perhaps you want to track software updates that become available for the operating system that runs on your VM.</span></span> <span data-ttu-id="b5760-196">Det finns specifika uppdateringar som du kan vilja inkludera eller exkludera.</span><span class="sxs-lookup"><span data-stu-id="b5760-196">There are specific updates you may want to include or exclude.</span></span> <span data-ttu-id="b5760-197">Konfigurationshantering gör att du kan spåra de här uppdateringarna och vidta åtgärder vid behov.</span><span class="sxs-lookup"><span data-stu-id="b5760-197">Configuration management allows you to track these updates and take action as required.</span></span> <span data-ttu-id="b5760-198">Du använder **System Center Configuration Manager** för att hantera företagets PC-datorer, servrar och mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="b5760-198">You use **System Center Configuration Manager** to manage your company's PC, servers, and mobile devices.</span></span> <span data-ttu-id="b5760-199">Du kan utöka det här stödet till dina virtuella Azure-datorer med Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="b5760-199">You can extend this support to your Azure VMs with Configuration Manager.</span></span>

- <span data-ttu-id="b5760-200">**Uppdateringshantering**.</span><span class="sxs-lookup"><span data-stu-id="b5760-200">**Update Management**.</span></span> <span data-ttu-id="b5760-201">Det här används till att hantera uppdateringar och korrigeringar för dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b5760-201">This is used to manage updates and patches for your VMs.</span></span> <span data-ttu-id="b5760-202">Med den här tjänsten kan du utvärdera statusen för tillgängliga uppdateringar, schemalägga installation och granska distributionsresultat för att verifiera att uppdateringarna tillämpas korrekt.</span><span class="sxs-lookup"><span data-stu-id="b5760-202">With this service, you're able to assess the status of available updates, schedule installation, and review deployment results to verify updates applied successfully.</span></span> <span data-ttu-id="b5760-203">Uppdateringshantering omfattar tjänster som ger process- och konfigurationshantering.</span><span class="sxs-lookup"><span data-stu-id="b5760-203">Update management incorporates services that provide process and configuration management.</span></span> <span data-ttu-id="b5760-204">Du aktiverar uppdateringshantering för en virtuell dator direkt via ditt **Azure Automation**-konto.</span><span class="sxs-lookup"><span data-stu-id="b5760-204">You enable update management for a VM directly from your **Azure Automation** account.</span></span> <span data-ttu-id="b5760-205">Du kan även tillåta uppdateringshantering för en enskild virtuell dator från bladet för virtuella datorer i portalen.</span><span class="sxs-lookup"><span data-stu-id="b5760-205">You can also allow update management for a single virtual machine from the virtual machine blade in the portal.</span></span>

<span data-ttu-id="b5760-206">Som du ser erbjuder Azure en mängd olika verktyg för att skapa och administrera resurser, så att du kan integrera hanteringsåtgärder i en process _som passar dig_.</span><span class="sxs-lookup"><span data-stu-id="b5760-206">As you can see, Azure provides a variety of tools to create and administer resources so that you can integrate management operations into a process _that works for you_.</span></span> <span data-ttu-id="b5760-207">Nu ska vi undersöka några av de andra Azure-tjänsterna för att se till att dina infrastrukturresurser körs problemfritt.</span><span class="sxs-lookup"><span data-stu-id="b5760-207">Let's examine some of the other Azure services to make sure your infrastructure resources are running smoothly.</span></span>
