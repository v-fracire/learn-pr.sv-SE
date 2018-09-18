<span data-ttu-id="b3d7f-101">Nu när en virtuell dator har skapats kan vi få information om den med hjälp av andra kommandon.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-101">Now that a virtual machine has been created, we can get information about it through other commands.</span></span>

<span data-ttu-id="b3d7f-102">Låt oss börja med `vm list`.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-102">Let's start with `vm list`.</span></span>

```azurecli
az vm list
```

<span data-ttu-id="b3d7f-103">Det här kommandot returnerar _alla_ virtuella datorer som definierats i den här prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-103">This command will return _all_ virtual machines defined in this subscription.</span></span> <span data-ttu-id="b3d7f-104">Utdata kan filtreras till en specifik resursgrupp med hjälp av parametern `--resource-group`.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-104">The output can be filtered to a specific resource group through the `--resource-group` parameter.</span></span> 

## <a name="output-types"></a><span data-ttu-id="b3d7f-105">Utdatatyper</span><span class="sxs-lookup"><span data-stu-id="b3d7f-105">Output types</span></span>
<span data-ttu-id="b3d7f-106">Observera att standardsvarstypen för alla kommandon som vi har gått igenom hittills är JSON.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-106">Notice that the default response type for all the commands we've done so far is JSON.</span></span> <span data-ttu-id="b3d7f-107">Det passar bra för skript, men många tycker att det är svårare att läsa.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-107">This is great for scripting - but most people find it harder to read.</span></span> <span data-ttu-id="b3d7f-108">Du kan ändra utdataformatet för svar med hjälp av flaggan `--output`.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-108">You can change the output style for any response through the `--output` flag.</span></span> <span data-ttu-id="b3d7f-109">Prova till exempel följande kommando i Azure Cloud Shell för att se ett annat utdataformat.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-109">For example, try the following command in Azure Cloud Shell to see the different output style.</span></span>

```azurecli
az vm list --output table
```

<span data-ttu-id="b3d7f-110">Tillsammans med `table` kan du ange `json` (standard), `jsonc` (färglagd JSON) eller `tsv` (tabellavgränsade värden).</span><span class="sxs-lookup"><span data-stu-id="b3d7f-110">Along with `table`, you can specify `json` (the default), `jsonc` (colorized JSON), or `tsv` (Table-Separated Values).</span></span> <span data-ttu-id="b3d7f-111">Prova några varianter med kommandot ovan så ser du skillnaden.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-111">Try a few variations with the above command to see the difference.</span></span>

## <a name="getting-the-ip-address"></a><span data-ttu-id="b3d7f-112">Hämta IP-adressen</span><span class="sxs-lookup"><span data-stu-id="b3d7f-112">Getting the IP address</span></span>

<span data-ttu-id="b3d7f-113">Ett annat användbart kommando är `vm list-ip-addresses`, som visar en lista över de offentliga och privata IP-adresserna för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-113">Another useful command is `vm list-ip-addresses`, which will list the public and private IP addresses for a VM.</span></span> <span data-ttu-id="b3d7f-114">Om de ändras, eller om du inte hämtade dem när de skapades, kan du hämta dem när som helst.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-114">If they change, or you didn't capture them during creation, you can retrieve them at any time.</span></span>

```azurecli
az vm list-ip-addresses -n SampleVM -o table
```

<span data-ttu-id="b3d7f-115">Detta returnerar:</span><span class="sxs-lookup"><span data-stu-id="b3d7f-115">This returns:</span></span>

```
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
SampleVM          168.61.54.62         10.0.0.4
```

> [!TIP]
> <span data-ttu-id="b3d7f-116">Observera att vi använder en förkortad syntax för `--output`-flaggan som `-o`.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-116">Notice that we are using a shorthand syntax for the `--output` flag as `-o`.</span></span> <span data-ttu-id="b3d7f-117">De flesta Azure CLI-kommandoparametrar kan förkortas till bara ett streck och en bokstav.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-117">Most parameters to Azure CLI commands can be shortened to a single dash and letter.</span></span> <span data-ttu-id="b3d7f-118">`--name` kan exempelvis förkortas till `-n` och `--resource-group` till `-g`.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-118">For example, `--name` can be shortened to `-n` and `--resource-group` to `-g`.</span></span> <span data-ttu-id="b3d7f-119">Detta är praktiskt när du skriver, men vi rekommenderar att du använder det fullständiga alternativnamnet i skript för tydlighetens skull.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-119">This is handy for typing, but we recommend using the full option name in scripts for clarity.</span></span> <span data-ttu-id="b3d7f-120">Dokumentationen innehåller detaljerad information om varje kommando.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-120">Check the documentation for details on each command.</span></span>

## <a name="getting-vm-details"></a><span data-ttu-id="b3d7f-121">Hämta information om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b3d7f-121">Getting VM details</span></span>

<span data-ttu-id="b3d7f-122">Vi kan få mer detaljerad information om en specifik virtuell dator baserat på namn eller ID med hjälp av kommandot `vm show`.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-122">We can get more detailed information about a specific virtual machine by name or ID using the `vm show` command.</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM
```

<span data-ttu-id="b3d7f-123">Detta returnerar ett ganska stort JSON-block med många olika typer av information om den virtuella datorn, inklusive anslutna lagringsenheter, nätverksgränssnitt och alla objekt-ID:n för resurser som den virtuella datorn är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-123">This will return a fairly large JSON block with all sorts of information about the VM, including attached storage devices, network interfaces, and all of the object IDs for resources that the VM is connected to.</span></span> <span data-ttu-id="b3d7f-124">Även nu kan vi ändra till ett tabellformat, men om vi gör det utelämnas nästan alla intressanta data.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-124">Again, we could change to a table format, but that omits almost all of the interesting data.</span></span> <span data-ttu-id="b3d7f-125">I stället kan vi använda ett inbyggt frågespråk för JSON kallat [JMESPath](http://jmespath.org/).</span><span class="sxs-lookup"><span data-stu-id="b3d7f-125">Instead, we can turn to a built-in query language for JSON called [JMESPath](http://jmespath.org/).</span></span>

## <a name="adding-filters-to-queries-with-jmespath"></a><span data-ttu-id="b3d7f-126">Lägga till filter till frågor med JMESPath</span><span class="sxs-lookup"><span data-stu-id="b3d7f-126">Adding filters to queries with JMESPath</span></span>

<span data-ttu-id="b3d7f-127">JMESPath är ett branschstandardiserat frågespråk som är uppbyggt kring JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-127">JMESPath is an industry-standard query language built around JSON objects.</span></span> <span data-ttu-id="b3d7f-128">Den enklaste frågan är att ange en _identifierare_ som väljer en nyckel i JSON-objektet.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-128">The simplest query is to specify an _identifier_ that selects a key in the JSON object.</span></span>

<span data-ttu-id="b3d7f-129">Anta att vi till exempel har objektet:</span><span class="sxs-lookup"><span data-stu-id="b3d7f-129">For example, given the object:</span></span>

```json
{
  "people": [
    {
      "name": "Fred",
      "age": 28
    },
    {
      "name": "Barney",
      "age": 25
    },
    {
      "name": "Wilma",
      "age": 27
    }
  ]
}
```

<span data-ttu-id="b3d7f-130">Vi kan använda frågan `people` för att returnera matrisen med värden för `people`-matrisen.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-130">We can use the query `people` to return the array of values for the `people` array.</span></span> <span data-ttu-id="b3d7f-131">Om vi bara är intresserade av _en_ av personerna kan vi använda en indexerare.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-131">If we just want _one_ of the people, we can use an indexer.</span></span> <span data-ttu-id="b3d7f-132">`people[1]` returnerar exempelvis:</span><span class="sxs-lookup"><span data-stu-id="b3d7f-132">For example, `people[1]` would return:</span></span>

```json
{
    "name": "Barney",
    "age": 25
}
```

<span data-ttu-id="b3d7f-133">Vi kan också lägga till specifika kvalificerare som returnerar en delmängd av objekten baserat på vissa kriterier.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-133">We can also add specific qualifiers that would return a subset of the objects based on some criteria.</span></span> <span data-ttu-id="b3d7f-134">Till exempel returneras följande om kvalificeraren `people[?age > '25']` läggs till:</span><span class="sxs-lookup"><span data-stu-id="b3d7f-134">For example, adding the qualifier `people[?age > '25']` would return:</span></span>

```json
[
  {
    "name": "Fred",
    "age": 28
  },
  {
    "name": "Wilma",
    "age": 27
  }
]
```

<span data-ttu-id="b3d7f-135">Slutligen kan vi begränsa resultaten genom att lägga till en ”select”: `people[?age > '25'].[name]` som bara returnerar namnen:</span><span class="sxs-lookup"><span data-stu-id="b3d7f-135">Finally, we can constrain the results by adding a select: `people[?age > '25'].[name]` that returns just the names:</span></span>

```json
[
  [
    "Fred"
  ],
  [
    "Wilma"
  ]
]
```

<span data-ttu-id="b3d7f-136">JMESQuery har flera andra intressanta frågefunktioner.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-136">JMESQuery has several other interesting query features.</span></span> <span data-ttu-id="b3d7f-137">När du har tid rekommenderar vi att du går [onlinesjälvstudien](http://jmespath.org/tutorial.html) som är tillgänglig på webbplatsen [JMESPath.org](http://jmespath.org/).</span><span class="sxs-lookup"><span data-stu-id="b3d7f-137">When you have time, check out the [online tutorial](http://jmespath.org/tutorial.html) available on the [JMESPath.org](http://jmespath.org/) site.</span></span>

## <a name="filtering-our-azure-cli-queries"></a><span data-ttu-id="b3d7f-138">Filtrera Azure CLI-frågorna</span><span class="sxs-lookup"><span data-stu-id="b3d7f-138">Filtering our Azure CLI queries</span></span>

<span data-ttu-id="b3d7f-139">När vi har en grundläggande förståelse för hur JMES-frågor fungerar kan vi lägga till filter till de data som returneras av frågor som till exempel `vm show`-kommandot.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-139">With a basic understanding of JMES queries, we can add filers to the data being returned by queries like the `vm show` command.</span></span> <span data-ttu-id="b3d7f-140">Vi kan till exempel hämta administratörens användarnamn:</span><span class="sxs-lookup"><span data-stu-id="b3d7f-140">For example, we can retrieve the admin user name:</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --query "osProfile.adminUsername"
```

<span data-ttu-id="b3d7f-141">Vi kan hämta storleken som tilldelats till vår virtuella dator:</span><span class="sxs-lookup"><span data-stu-id="b3d7f-141">We can get the size assigned to our VM:</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --query hardwareProfile.vmSize
```

<span data-ttu-id="b3d7f-142">Och om du vill hämta alla ID:n för dina nätverksgränssnitt kan du använda frågan:</span><span class="sxs-lookup"><span data-stu-id="b3d7f-142">Or to retrieve all the IDs for your network interfaces, you can use the query:</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --query "networkProfile.networkInterfaces[].id"
```

<span data-ttu-id="b3d7f-143">Den här frågetekniken fungerar med alla Azure CLI-kommandon och kan användas för att hämta specifika delar av data till kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-143">This query technique will work with any Azure CLI command and can be used to pull specific bits of data out on the command line.</span></span> <span data-ttu-id="b3d7f-144">Tekniken kan även användas för skript – du kan till exempel hämta ett värde från ditt Azure-konto och lagra det i en miljö- eller skriptvariabel.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-144">It is useful for scripting as well - for example, you can pull a value out of your Azure account and store it in an environment or script variable.</span></span> <span data-ttu-id="b3d7f-145">Om du vill använda den på det här sättet är `--output tsv`-parametern en användbar flagga som du kan lägga till (som kan förkortas till `-o tsv`).</span><span class="sxs-lookup"><span data-stu-id="b3d7f-145">If you decide to use it this way, a useful flag to add is the `--output tsv` parameter (which can be shortened to `-o tsv`).</span></span> <span data-ttu-id="b3d7f-146">När du gör det returneras resultaten som tabbavgränsade värden som endast innehåller själva datavärdena med tabbavgränsare.</span><span class="sxs-lookup"><span data-stu-id="b3d7f-146">This will return the results in tab-separated values that only include the actual data values with tab separators.</span></span>

<span data-ttu-id="b3d7f-147">Exempelvis returnerar</span><span class="sxs-lookup"><span data-stu-id="b3d7f-147">As an example,</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --query "networkProfile.networkInterfaces[].id" -o tsv
```

<span data-ttu-id="b3d7f-148">texten: `/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`</span><span class="sxs-lookup"><span data-stu-id="b3d7f-148">returns the text: `/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`</span></span>
