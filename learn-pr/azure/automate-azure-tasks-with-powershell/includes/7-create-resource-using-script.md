<span data-ttu-id="e88ff-101">Komplexa och repetitiva aktiviteter kan ofta kräva mycket administration.</span><span class="sxs-lookup"><span data-stu-id="e88ff-101">Complex or repetitive tasks often take a great deal of administrative time.</span></span> <span data-ttu-id="e88ff-102">Organisationer vill gärna automatisera sådana uppgifter för att minska kostnaderna och undvika fel.</span><span class="sxs-lookup"><span data-stu-id="e88ff-102">Organizations prefer to automate these tasks to reduce costs and avoid errors.</span></span>

<span data-ttu-id="e88ff-103">Det här är viktigt i CRM-exemplet (Customer Relationship Management).</span><span class="sxs-lookup"><span data-stu-id="e88ff-103">This is important in the Customer Relationship Management (CRM) company example.</span></span> <span data-ttu-id="e88ff-104">Där får du testa din programvara på flera virtuella Linux-datorer (VM) som du tar bort och återskapar upprepade gånger.</span><span class="sxs-lookup"><span data-stu-id="e88ff-104">There, you test your software on multiple Linux Virtual Machines (VMs) that you need to continuously delete and recreate.</span></span> <span data-ttu-id="e88ff-105">Du vill använda ett PowerShell-skript för att automatisera skapandet av de virtuella datorerna och skapa dem manuellt varje gång som vi just gjorde.</span><span class="sxs-lookup"><span data-stu-id="e88ff-105">You want to use a PowerShell script to automate the creation of the VMs vs. creating them manually each time like we just did.</span></span>

<span data-ttu-id="e88ff-106">Förutom grundåtgärden att skapa en virtuell dator finns ytterligare några krav på skriptet.</span><span class="sxs-lookup"><span data-stu-id="e88ff-106">Beyond the core operation of creating a VM you have a few additional requirements for your script.</span></span> 
- <span data-ttu-id="e88ff-107">Du kommer att skapa flera virtuella datorer, så det är bra att placera åtgärden i en loop</span><span class="sxs-lookup"><span data-stu-id="e88ff-107">You will create multiple VMs, so you want to put the creation inside a loop</span></span>
- <span data-ttu-id="e88ff-108">Du skapar de virtuella datorerna i tre olika resursgrupper, så namnet på resursgruppen bör överföras till skriptet som en parameter</span><span class="sxs-lookup"><span data-stu-id="e88ff-108">You need to create VMs in three different resource groups, so the name of the resource group should be passed to the script as a parameter</span></span>

<span data-ttu-id="e88ff-109">I det här avsnittet visas hur du skriver och kör ett Azure PowerShell-skript som uppfyller de här kraven.</span><span class="sxs-lookup"><span data-stu-id="e88ff-109">In this section, you will see how to write and execute an Azure PowerShell script that meets these requirements.</span></span>

## <a name="what-is-a-powershell-script"></a><span data-ttu-id="e88ff-110">Vad är ett PowerShell-skript?</span><span class="sxs-lookup"><span data-stu-id="e88ff-110">What is a PowerShell script?</span></span>
<span data-ttu-id="e88ff-111">Ett PowerShell-skript är en textfil som innehåller kommandon och styrelement.</span><span class="sxs-lookup"><span data-stu-id="e88ff-111">A PowerShell script is a text file containing commands and control constructs.</span></span> <span data-ttu-id="e88ff-112">Kommandona anropar cmdletar.</span><span class="sxs-lookup"><span data-stu-id="e88ff-112">The commands are invocations of cmdlets.</span></span> <span data-ttu-id="e88ff-113">Styrelementen är programkonstruktioner som loopar, variabler, parametrar, kommentarer och annat som finns i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e88ff-113">The control constructs are programming features like loops, variables, parameters, comments, etc. supplied by PowerShell.</span></span>

<span data-ttu-id="e88ff-114">PowerShell-skriptfiler har filtillägget **.ps1**.</span><span class="sxs-lookup"><span data-stu-id="e88ff-114">PowerShell script files have a **.ps1** file extension.</span></span> <span data-ttu-id="e88ff-115">Du kan skapa och spara de här filerna i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="e88ff-115">You can create and save these files with any text editor.</span></span> 

> [!TIP]
> <span data-ttu-id="e88ff-116">Om du skriver PowerShell-skript i Windows kan du använda Windows integrerade skriptmiljö för PowerShell (ISE).</span><span class="sxs-lookup"><span data-stu-id="e88ff-116">If you’re writing PowerShell scripts under Windows, you can use the Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="e88ff-117">Den här redigeraren har funktioner som syntaxfärgning och en lista med tillgängliga cmdletar.</span><span class="sxs-lookup"><span data-stu-id="e88ff-117">This editor provides features such as syntax coloring and a list of available cmdlets.</span></span>
>
<span data-ttu-id="e88ff-118">I följande bild visas Windows PowerShell Integrated Scripting Environment (ISE) med ett exempelskript som ansluter till Azure och skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e88ff-118">The following screenshot shows the Windows PowerShell Integrated Scripting Environment (ISE) with a sample script to connect to Azure and create a virtual machine in Azure.</span></span>

>![Skärmbild av Windows PowerShell Integrated Scripting Environment med ett skript som skapar en virtuell dator i redigeringsfönstret.](../media/7-windows-powershell-ise-screenshot.png)

<span data-ttu-id="e88ff-120">När du har skrivit skriptet kör du det från PowerShell-kommandoraden genom att skriva en punkt och ett bakstreck följt av namnet på filen:</span><span class="sxs-lookup"><span data-stu-id="e88ff-120">Once you have written the script, execute it from the PowerShell command line by passing the name of the file preceded by a dot and a backslash:</span></span>

```powershell
.\myScript.ps1
```

## <a name="powershell-techniques"></a><span data-ttu-id="e88ff-121">PowerShell-tekniker</span><span class="sxs-lookup"><span data-stu-id="e88ff-121">PowerShell techniques</span></span>
<span data-ttu-id="e88ff-122">PowerShell har många funktioner som finns i vanliga programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="e88ff-122">PowerShell has many features found in typical programming languages.</span></span> <span data-ttu-id="e88ff-123">Du kan definiera variabler, använda grenar och loopar, inhämta kommandoradsparametrar, skriva funktioner, lägga till kommentarer och så vidare.</span><span class="sxs-lookup"><span data-stu-id="e88ff-123">You can define variables, use branches and loops, capture command-line parameters, write functions, add comments, and so on.</span></span> <span data-ttu-id="e88ff-124">Vi kommer att använda tre funktioner i vårt skript: variabler, loopar och parametrar.</span><span class="sxs-lookup"><span data-stu-id="e88ff-124">We will need three features for our script: variables, loops, and parameters.</span></span>

### <a name="variables"></a><span data-ttu-id="e88ff-125">Variabler</span><span class="sxs-lookup"><span data-stu-id="e88ff-125">Variables</span></span>
<span data-ttu-id="e88ff-126">Som du såg i det senaste momentet har PowerShell stöd för variabler.</span><span class="sxs-lookup"><span data-stu-id="e88ff-126">As you saw in the last unit, PowerShell supports variables.</span></span> <span data-ttu-id="e88ff-127">Använd **$** för att deklarera en variabel och **=** för att tilldela ett värde.</span><span class="sxs-lookup"><span data-stu-id="e88ff-127">Use **$** to declare a variable and **=** to assign a value.</span></span> <span data-ttu-id="e88ff-128">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e88ff-128">For example:</span></span>

```powershell
$loc = "East US"
$iterations = 3
```

<span data-ttu-id="e88ff-129">Variabler kan innehålla objekt.</span><span class="sxs-lookup"><span data-stu-id="e88ff-129">Variables can hold objects.</span></span> <span data-ttu-id="e88ff-130">Följande definition anger exempelvis variabeln **adminCredential** till objektet som returneras av cmdleten **Get-Credential**.</span><span class="sxs-lookup"><span data-stu-id="e88ff-130">For example, the following definition sets the **adminCredential** variable to the object returned by the **Get-Credential** cmdlet.</span></span>

```powershell
$adminCredential = Get-Credential
```

<span data-ttu-id="e88ff-131">Du kan komma åt värdet som lagras i en variabel genom att använda prefixet **$** och variabelns namn som i exemplet nedan:</span><span class="sxs-lookup"><span data-stu-id="e88ff-131">To obtain the value stored in a variable, use the **$** prefix and its name as shown below:</span></span> 

```powershell
$loc = "East US"
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location $loc
```

### <a name="loops"></a><span data-ttu-id="e88ff-132">Loopar</span><span class="sxs-lookup"><span data-stu-id="e88ff-132">Loops</span></span>
<span data-ttu-id="e88ff-133">PowerShell har flera loopar: **For**, **Do...While**, **For...Each** och så vidare.</span><span class="sxs-lookup"><span data-stu-id="e88ff-133">PowerShell has several loops: **For**, **Do...While**, **For...Each**, and so on.</span></span> <span data-ttu-id="e88ff-134">**For**-loopen passar bäst för våra behov eftersom vi ska köra en cmdlet ett visst antal gånger.</span><span class="sxs-lookup"><span data-stu-id="e88ff-134">The **For** loop is the best match for our needs because we will execute a cmdlet a fixed number of times.</span></span>

<span data-ttu-id="e88ff-135">Den huvudsakliga syntaxen visas nedan. Exemplet körs i två iterationer och skriver ut värdet för **i** varje gång.</span><span class="sxs-lookup"><span data-stu-id="e88ff-135">The core syntax is shown below; the example runs for two iterations and prints the value of **i** each time.</span></span> <span data-ttu-id="e88ff-136">Jämförelseoperatorerna skrivs som **-lt** för ”mindre än”, **-le** för ”mindre än eller lika med”, **eq** för ”lika med”, **ne** för ”inte lika med” och så vidare.</span><span class="sxs-lookup"><span data-stu-id="e88ff-136">The comparison operators are written **-lt** for "less than", **-le** for "less than or equal", **eq** for "equal", **ne** for "not equal", etc.</span></span>

```powershell
For ($i = 1; $i -lt 3; $i++)
{
    $i
}
```

### <a name="parameters"></a><span data-ttu-id="e88ff-137">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e88ff-137">Parameters</span></span>
<span data-ttu-id="e88ff-138">När du kör ett skript kan du skicka argument på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="e88ff-138">When you execute a script, you can pass arguments on the command line.</span></span> <span data-ttu-id="e88ff-139">Du kan ange namn för varje parameter så att deras värden kan användas i skriptet.</span><span class="sxs-lookup"><span data-stu-id="e88ff-139">You can provide names for each parameter to help the script extract the values.</span></span> <span data-ttu-id="e88ff-140">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e88ff-140">For example:</span></span>

```powershell
.\setupEnvironment.ps1 -size 5 -location "East US"
```

<span data-ttu-id="e88ff-141">I skriptet tar du värdena och gör dem till variabler.</span><span class="sxs-lookup"><span data-stu-id="e88ff-141">Inside the script, you capture the values into variables.</span></span> <span data-ttu-id="e88ff-142">I det här exemplet matchas parametrarna med hjälp av namnen:</span><span class="sxs-lookup"><span data-stu-id="e88ff-142">In this example, the parameters are matched by name:</span></span>

```powershell
param([string]$location, [int]$size)
```

<span data-ttu-id="e88ff-143">Du kan utelämna namnen på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="e88ff-143">You can omit the names from the command line.</span></span> <span data-ttu-id="e88ff-144">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e88ff-144">For example:</span></span>

```powershell
.\setupEnvironment.ps1 5 "East US"
```

<span data-ttu-id="e88ff-145">När parametrarna inte har namn används deras placering till matchning i skriptet:</span><span class="sxs-lookup"><span data-stu-id="e88ff-145">Inside the script, you rely on position for matching when the parameters are unnamed:</span></span>

```powershell
param([int]$size, [string]$location)
```

<span data-ttu-id="e88ff-146">Vi kan ta dessa parametrar som indata och använda en loop för att skapa en uppsättning virtuella datorer från de angivna parametrarna.</span><span class="sxs-lookup"><span data-stu-id="e88ff-146">We could take these parameters as input, and use a loop to create a set of VMs from the given parameters.</span></span> <span data-ttu-id="e88ff-147">Vi ska testa detta härnäst.</span><span class="sxs-lookup"><span data-stu-id="e88ff-147">We'll try that next.</span></span>

<span data-ttu-id="e88ff-148">I PowerShell och Azure PowerShell har du alla verktyg du behöver för att automatisera Azure.</span><span class="sxs-lookup"><span data-stu-id="e88ff-148">The combination of PowerShell and Azure PowerShell gives you all the tools you need to automate Azure.</span></span> <span data-ttu-id="e88ff-149">I vårt CRM-exempel kommer vi att kunna skapa flera virtuella Linux-datorer. Vi använder en parameter som gör skriptet generellt och en loop som gör att vi inte behöver upprepa koden.</span><span class="sxs-lookup"><span data-stu-id="e88ff-149">In our CRM example, we will be able to create multiple Linux VMs using a parameter to keep the script generic and a loop to avoid repeated code.</span></span> <span data-ttu-id="e88ff-150">Det innebär att en åtgärd som tidigare var komplicerad nu kan köras i ett enda steg.</span><span class="sxs-lookup"><span data-stu-id="e88ff-150">This means that a formerly complex operation can now be executed in a single step.</span></span>