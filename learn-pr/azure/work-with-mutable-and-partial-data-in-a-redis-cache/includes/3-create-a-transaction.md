<span data-ttu-id="33fda-101">Vi börjar med att skapa en Azure Redis Cache-instans och sedan skapar vi en enkel transaktion som infogar två datavärden i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="33fda-101">Let's start by creating an instance of Azure Redis Cache, and then create a simple transaction that inserts two data values into the cache.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="33fda-102">Skapa en Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="33fda-102">Create an Azure Redis Cache</span></span>

<span data-ttu-id="33fda-103">Vi börjar med en skapa en Azure Redis Cache med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="33fda-103">Let's start by creating an Azure Redis Cache with the Azure CLI.</span></span> <span data-ttu-id="33fda-104">Använd Cloud Shell till höger i webbläsarfönstret för att interagera med Azure.</span><span class="sxs-lookup"><span data-stu-id="33fda-104">Use the Cloud Shell on the right side of the browser window to interact with Azure.</span></span>

<span data-ttu-id="33fda-105">Vi använder kommandot `az redis create` till att skapa en ny Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="33fda-105">We'll use the `az redis create` command to create a new Azure Redis Cache.</span></span> <span data-ttu-id="33fda-106">Det tar flera parametrar.</span><span class="sxs-lookup"><span data-stu-id="33fda-106">It takes several parameters.</span></span> <span data-ttu-id="33fda-107">Här är de vanligaste (en fullständig lista finns i dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="33fda-107">Here are the most common (to get a full list, check the documentation).</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="33fda-108">Parameter</span><span class="sxs-lookup"><span data-stu-id="33fda-108">Parameter</span></span> | <span data-ttu-id="33fda-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="33fda-109">Description</span></span> |
> |-----------|-------------|
> | `--name`    | <span data-ttu-id="33fda-110">Namnet på cachen – det här måste vara globalt unik och bestå av bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="33fda-110">The name of the cache - this must be globally unique and composed of letters, numbers, and dashes.</span></span> |
> | `--resource-group` | <span data-ttu-id="33fda-111">Använd den färdiga resursgruppen **<rgn>[namn på sandbox-resursgrupp]</rgn>** som är en del av Azure-sandboxmiljön.</span><span class="sxs-lookup"><span data-stu-id="33fda-111">Use the pre-created Resource Group **<rgn>[Sandbox resource group name]</rgn>**, which is part of the Azure Sandbox.</span></span> |
> | `--location` | <span data-ttu-id="33fda-112">Ange den plats där cacheminnet ska finnas.</span><span class="sxs-lookup"><span data-stu-id="33fda-112">Specify the location where the cache should be located.</span></span> <span data-ttu-id="33fda-113">Normalt väljer du en plats nära datakonsumenterna.</span><span class="sxs-lookup"><span data-stu-id="33fda-113">Normally, you will want to choose a location close to the data consumers.</span></span> <span data-ttu-id="33fda-114">I det här fallet begränsas du till de platser som är tillgängliga i Azure-sandboxmiljön.</span><span class="sxs-lookup"><span data-stu-id="33fda-114">In this case, you are limited to the locations available in the Azure Sandbox.</span></span> <span data-ttu-id="33fda-115">Välj den som är närmast dig.</span><span class="sxs-lookup"><span data-stu-id="33fda-115">Select the closest one to you.</span></span> |
> | `--size` | <span data-ttu-id="33fda-116">Storleken på Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="33fda-116">The size of the Azure Redis Cache.</span></span> <span data-ttu-id="33fda-117">Giltiga värden är [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4].</span><span class="sxs-lookup"><span data-stu-id="33fda-117">Valid values are [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4].</span></span> |
> | `--sku` | <span data-ttu-id="33fda-118">SKU för Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="33fda-118">The Azure Redis Cache SKU.</span></span> <span data-ttu-id="33fda-119">Giltiga värden är [Basic, Standard, Premium].</span><span class="sxs-lookup"><span data-stu-id="33fda-119">Valid values are [Basic, Standard, Premium].</span></span> |

### <a name="select-a-location"></a><span data-ttu-id="33fda-120">Välja en plats</span><span class="sxs-lookup"><span data-stu-id="33fda-120">Select a location</span></span>
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="33fda-121">Skapa ett cacheminne med följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="33fda-121">Create a cache using the following options:</span></span>
    - <span data-ttu-id="33fda-122">Storlek: C0</span><span class="sxs-lookup"><span data-stu-id="33fda-122">Size: C0</span></span>
    - <span data-ttu-id="33fda-123">SKU: Basic</span><span class="sxs-lookup"><span data-stu-id="33fda-123">SKU: Basic</span></span>

1. <span data-ttu-id="33fda-124">Här är ett exempel på en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="33fda-124">Here's an example command line.</span></span> <span data-ttu-id="33fda-125">Ersätt parametern `[name]` med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="33fda-125">Make sure to replace the `[name]` parameter with a unique name.</span></span> <span data-ttu-id="33fda-126">Du kan ersätta platsen om du vill använda en annan region än USA, östra.</span><span class="sxs-lookup"><span data-stu-id="33fda-126">You can replace the location if you want a different region than East US.</span></span>

    ```azurecli
    REDIS_NAME=[name]

    az redis create \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location eastus \
        --vm-size C0 \
        --sku Basic \
    ```

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="33fda-127">Skapa en .NET Core-konsolapp</span><span class="sxs-lookup"><span data-stu-id="33fda-127">Create a .NET Core console application</span></span>

<span data-ttu-id="33fda-128">Skapa sedan en .NET Core-konsolapp, som används till att infoga datavärden i Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="33fda-128">Next, create a .NET Core console application, which will be used to insert data values into our Azure Redis Cache.</span></span>

1. <span data-ttu-id="33fda-129">Skapa en ny .NET Core-app med integrerat Cloud Shell till höger i fönstret.</span><span class="sxs-lookup"><span data-stu-id="33fda-129">Create a new .NET Core application using the integrated Cloud Shell on the right hand side of the window.</span></span> <span data-ttu-id="33fda-130">Ge den namnet ”RedisData”.</span><span class="sxs-lookup"><span data-stu-id="33fda-130">Name it "RedisData".</span></span>

    ```bash
    cd ~
    dotnet new console --name RedisData
    ```

1. <span data-ttu-id="33fda-131">Ändra i den nya katalogen som skapats för appen.</span><span class="sxs-lookup"><span data-stu-id="33fda-131">Change into the new directory created for your app.</span></span>

    ```bash
    cd RedisData
    ```

1. <span data-ttu-id="33fda-132">Skapa och kör appen – utdata bör vara ”Hello World!”</span><span class="sxs-lookup"><span data-stu-id="33fda-132">Build and run the application - it should output "Hello World!"</span></span>

    ```bash
    dotnet run
    ```

## <a name="add-the-servicestackredis-nuget-package"></a><span data-ttu-id="33fda-133">Lägg till ServiceStack.Redis NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="33fda-133">Add the ServiceStack.Redis NuGet package</span></span>

<span data-ttu-id="33fda-134">Nu när vi har en konsolapp måste vi lägga till **ServiceStack.Redis** NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="33fda-134">Now that we have our console application, we need to add the **ServiceStack.Redis** NuGet package.</span></span> <span data-ttu-id="33fda-135">Då kan vi ansluta till Azure Redis Cache och utfärda kommandon i C#.</span><span class="sxs-lookup"><span data-stu-id="33fda-135">This will allow us to connect to the Azure Redis Cache and issue commands in C#.</span></span>

1. <span data-ttu-id="33fda-136">Lägg till NuGet-paketet **ServiceStack.Redis** med hjälp av terminalgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="33fda-136">Add the NuGet package **ServiceStack.Redis** using the terminal shell.</span></span>

    ```bash
    dotnet add package ServiceStack.Redis
    ```

1. <span data-ttu-id="33fda-137">Skapa och kör appen igen för att kontrollera att allt kompileras.</span><span class="sxs-lookup"><span data-stu-id="33fda-137">Build and run the application again to make sure it all compiles.</span></span> <span data-ttu-id="33fda-138">Utdata bör fortfarande vara ”Hello World!”</span><span class="sxs-lookup"><span data-stu-id="33fda-138">It should still output "Hello World!"</span></span>

## <a name="get-your-azure-redis-cache-connection-string"></a><span data-ttu-id="33fda-139">Hämta Azure Redis Cache-anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="33fda-139">Get your Azure Redis Cache connection string</span></span>

<span data-ttu-id="33fda-140">För att ansluta till Azure Redis Cache behöver du en anslutningssträng som innehåller lösenordet och URL:en.</span><span class="sxs-lookup"><span data-stu-id="33fda-140">To connect to your Azure Redis Cache, you need a connection string that contains your password and URL.</span></span> <span data-ttu-id="33fda-141">**ServiceStack.Redis** har sitt eget format för anslutningssträng: `[password]@[hostname]:[sslport]?ssl=true`.</span><span class="sxs-lookup"><span data-stu-id="33fda-141">**ServiceStack.Redis** has its own connection string format: `[password]@[hostname]:[sslport]?ssl=true`.</span></span>

<span data-ttu-id="33fda-142">Du kan hämta lösenordet med Azure Portal eller med kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="33fda-142">You can retrieve your password with the Azure portal, or with the command line.</span></span> <span data-ttu-id="33fda-143">Vi använder det senare här eftersom vi använde portalmetoden i modulen ”Optimera webbprogram genom att cachelagra skrivskyddade data med Redis”.</span><span class="sxs-lookup"><span data-stu-id="33fda-143">Let's use the latter here, since we used the portal approach in the "Optimize your web applications by caching read-only data with Redis" module.</span></span>

<span data-ttu-id="33fda-144">Hämta åtkomstnycklarna med kommandot `az redis list-keys`.</span><span class="sxs-lookup"><span data-stu-id="33fda-144">Use the `az redis list-keys` command to get the access keys.</span></span> <span data-ttu-id="33fda-145">Kör dessa kommandon för att hämta den primära nyckeln, lagra den i en variabel med namnet `REDIS_KEY` och visa den:</span><span class="sxs-lookup"><span data-stu-id="33fda-145">Run these commands to get the primary key, store it in a variable called `REDIS_KEY`, and display it:</span></span>

```azurecli
REDIS_KEY=$(az redis list-keys \
    --name "$REDIS_NAME" \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query primaryKey \
    --output tsv)

echo $REDIS_KEY
```

<span data-ttu-id="33fda-146">Kör sedan detta kommando för att samla anslutningssträngen och visa den på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="33fda-146">Next, run this command to put the connection string together and display it on the command line.</span></span> <span data-ttu-id="33fda-147">Observera att värdnamnet är namnet på cachen följt av `.redis.cache.windows.net` och porten är **6380**, standardporten för Redis SSL.</span><span class="sxs-lookup"><span data-stu-id="33fda-147">Note that the hostname is the name of the cache followed by `.redis.cache.windows.net`, and the port is **6380**, the default Redis SSL port.</span></span>

```bash
echo "$REDIS_KEY"@"$REDIS_NAME".redis.cache.windows.net:6380?ssl=true
```

<span data-ttu-id="33fda-148">Kopiera anslutningssträngen till urklipp &mdash; du kommer att använda det i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="33fda-148">Copy the connection string to the clipboard &mdash; you'll be using it in the next step.</span></span>

## <a name="add-the-connection-string-to-your-app"></a><span data-ttu-id="33fda-149">Lägga till anslutningssträngen i appen</span><span class="sxs-lookup"><span data-stu-id="33fda-149">Add the connection string to your app</span></span>

1. <span data-ttu-id="33fda-150">Anslut Cloud Shell-redigeraren från programmappen.</span><span class="sxs-lookup"><span data-stu-id="33fda-150">Open the Cloud Shell editor from the application folder.</span></span>

    ```bash
    cd ~/RedisData
    code .
    ```

1. <span data-ttu-id="33fda-151">Välj källfilen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="33fda-151">Select the **Program.cs** source file.</span></span>

1. <span data-ttu-id="33fda-152">Skapa följande fält i klassen `Program` och klistra in anslutningssträngen som värde.</span><span class="sxs-lookup"><span data-stu-id="33fda-152">Create the following field in the `Program` class and paste in your connection string as the value.</span></span>

    ```csharp
    static string redisConnectionString = "<connection string>";
    ```

    <span data-ttu-id="33fda-153">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="33fda-153">Here's an example:</span></span>

    ```csharp
    static string redisConnectionString = "ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6380?ssl=true";
    ```

## <a name="insert-two-data-values-into-your-azure-redis-cache"></a><span data-ttu-id="33fda-154">Infoga två datavärden i Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="33fda-154">Insert two data values into your Azure Redis Cache</span></span>

<span data-ttu-id="33fda-155">Slutligen ska vi lägga till data i Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="33fda-155">Finally, we're going to add data into your Azure Redis Cache.</span></span>

1. <span data-ttu-id="33fda-156">Lägg till följande `using`-instruktion högst upp i filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="33fda-156">Add the following `using` statement to the top of the **Program.cs** file.</span></span>

    ```csharp
    using ServiceStack.Redis;
    ```

1. <span data-ttu-id="33fda-157">Ersätt innehållet i metoden `Main` med följande kod.</span><span class="sxs-lookup"><span data-stu-id="33fda-157">Replace the contents of the `Main` method with the following code.</span></span> <span data-ttu-id="33fda-158">Detta använder en transaktion för att lägga till två värden.</span><span class="sxs-lookup"><span data-stu-id="33fda-158">This will use a transaction to add two values.</span></span>

    ```csharp
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    if (transactionResult)
    {
        Console.WriteLine("Transaction committed");
    }
    else
    {
        Console.WriteLine("Transaction failed to commit");
    }
    ```

1. <span data-ttu-id="33fda-159">Innan du bygger och kör programmet bör du kontrollera att Redis-cachen har etablerats fullständigt.</span><span class="sxs-lookup"><span data-stu-id="33fda-159">Before building and running the application, check to make sure that the Redis cache has been fully provisioned.</span></span> <span data-ttu-id="33fda-160">Det kan ta några minuter efter att `az redis create` har slutförts.</span><span class="sxs-lookup"><span data-stu-id="33fda-160">It can sometimes take a few minutes after `az redis create` completes.</span></span> <span data-ttu-id="33fda-161">Kör följande kommando för att kontrollera statusen.</span><span class="sxs-lookup"><span data-stu-id="33fda-161">Run the following command to check the status.</span></span>

    ```azcli
    az redis show \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --query provisioningState
    ```

    <span data-ttu-id="33fda-162">Om statusen är `Creating` ska du kontrollera igen efter ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="33fda-162">If the status is `Creating`, try checking again in a couple of minutes.</span></span> <span data-ttu-id="33fda-163">Den visar `Succeeded` när den är klar.</span><span class="sxs-lookup"><span data-stu-id="33fda-163">It will show `Succeeded` when finished.</span></span>

1. <span data-ttu-id="33fda-164">Kör programmet och kontrollera att konsolen säger **Transaktionen har checkats in**.</span><span class="sxs-lookup"><span data-stu-id="33fda-164">Run the application and verify that the console says **Transaction committed**.</span></span>

    ```bash
    dotnet run
    ```

## <a name="verify-your-data"></a><span data-ttu-id="33fda-165">Verifiera dina data</span><span class="sxs-lookup"><span data-stu-id="33fda-165">Verify your data</span></span>

<span data-ttu-id="33fda-166">Vi slutför genom att verifiera att data vi har lagt till finns i Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="33fda-166">To finish off, let's verify that the data we added is in our Azure Redis Cache.</span></span>

1. <span data-ttu-id="33fda-167">Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="33fda-167">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="33fda-168">Leta reda på Azure Redis Cache genom att välja **Alla resurser** i sidofältet till vänster och med hjälp av filterfältet till vänster för att välja Azure Redis Cache-instanser.</span><span class="sxs-lookup"><span data-stu-id="33fda-168">Locate your Azure Redis Cache by selecting **All Resources** in the left-hand sidebar, and using the filter box on the left to select Azure Redis Cache instances.</span></span> <span data-ttu-id="33fda-169">Du kan alternativt använda sökrutan högst upp och skriva namnet på cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="33fda-169">Alternatively, you can use the search box at the top, and type the name of the cache.</span></span>

1. <span data-ttu-id="33fda-170">Välj din Azure Redis Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="33fda-170">Select your Azure Redis Cache instance.</span></span>

1. <span data-ttu-id="33fda-171">På bladet **Översikt** för Azure Redis Cache väljer du **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="33fda-171">In the **Overview** blade for your Azure Redis Cache, select **Console**.</span></span> <span data-ttu-id="33fda-172">Azure Redis Cache-konsolen öppnas, där du kan ange Azure Redis Cache-kommandon på låg nivå.</span><span class="sxs-lookup"><span data-stu-id="33fda-172">This will open an Azure Redis Cache console, which allows you to enter low-level Azure Redis Cache commands.</span></span>

1. <span data-ttu-id="33fda-173">Ange **get MyKey1**.</span><span class="sxs-lookup"><span data-stu-id="33fda-173">Type **get MyKey1**.</span></span> <span data-ttu-id="33fda-174">Kontrollera att det returnerade värdet är **MyValue1**.</span><span class="sxs-lookup"><span data-stu-id="33fda-174">Verify that the value returned is **MyValue1**.</span></span>

1. <span data-ttu-id="33fda-175">Ange **get MyKey2**.</span><span class="sxs-lookup"><span data-stu-id="33fda-175">Type **get MyKey2**.</span></span> <span data-ttu-id="33fda-176">Kontrollera att det returnerade värdet är **MyValue2**.</span><span class="sxs-lookup"><span data-stu-id="33fda-176">Verify that the value returned is **MyValue2**.</span></span>

    ![Skärmbild av Azure Redis Cache-konsolen som visar värdena MyKey1 och MyKey2.](../media/4-redis-console.png)