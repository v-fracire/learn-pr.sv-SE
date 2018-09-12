<span data-ttu-id="9c127-101">I den här utbildningsenheten utför du en del grundläggande felsökning som att hämta containerloggar, containerhändelser och koppla dem till en containerinstans.</span><span class="sxs-lookup"><span data-stu-id="9c127-101">In this unit, you will perform some basic troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span> <span data-ttu-id="9c127-102">När du har gått igenom modulen bör du kunna grunderna i att felsöka containerinstanser.</span><span class="sxs-lookup"><span data-stu-id="9c127-102">By the end of this module, you should understand basic capabilities for troubleshooting container instances.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="9c127-103">Skapa en container</span><span class="sxs-lookup"><span data-stu-id="9c127-103">Create a container</span></span>

<span data-ttu-id="9c127-104">Börja med att skapa containern vi ska använda.</span><span class="sxs-lookup"><span data-stu-id="9c127-104">Start by creating a container to use in this unit.</span></span> <span data-ttu-id="9c127-105">Om du fortfarande har den första containern vi skapade i modulen kan du hoppa över det här steget:</span><span class="sxs-lookup"><span data-stu-id="9c127-105">If you still have the first container created in this module, skip this step:</span></span>

```azurecli
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --ports 80 --ip-address Public
```

## <a name="get-logs-from-a-container-instance"></a><span data-ttu-id="9c127-106">Hämta loggar från en containerinstans</span><span class="sxs-lookup"><span data-stu-id="9c127-106">Get logs from a container instance</span></span>

<span data-ttu-id="9c127-107">Om du vill visa loggar från programkoden i en container kan du använda kommandot `az container logs`:</span><span class="sxs-lookup"><span data-stu-id="9c127-107">To view logs from your application code within a container, you can use the `az container logs` command:</span></span>

```azazurecli
az container logs --resource-group myResourceGroup --name mycontainer
```

<span data-ttu-id="9c127-108">Här är loggutdata från exempelcontainern när webbappen har använts några gånger:</span><span class="sxs-lookup"><span data-stu-id="9c127-108">The following is log output from the example container after the web app has been accessed a few times:</span></span>

```output
listening on port 80
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://23.101.136.193/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:25 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:27 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
```

## <a name="get-container-events"></a><span data-ttu-id="9c127-109">Hämta containerhändelser</span><span class="sxs-lookup"><span data-stu-id="9c127-109">Get container events</span></span>

<span data-ttu-id="9c127-110">Med kommandot `az container attach` kan du visa diagnostisk information när containern startas.</span><span class="sxs-lookup"><span data-stu-id="9c127-110">The `az container attach` command provides diagnostic information during container startup.</span></span> <span data-ttu-id="9c127-111">När containern har startats strömmas även STDOUT och STDERR till den lokala konsolen:</span><span class="sxs-lookup"><span data-stu-id="9c127-111">Once the container has started, it also streams STDOUT and STDERR to your local console:</span></span>

```azazurecli
az container attach --resource-group myResourceGroup --name mycontainer
```

<span data-ttu-id="9c127-112">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="9c127-112">Example output:</span></span>


```output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-08-20 21:43:26+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:32+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Created container
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Started container

Start streaming logs:
listening on port 80
listening on port 80
```

## <a name="execute-a-command-in-a-container"></a><span data-ttu-id="9c127-113">Köra ett kommando i en container</span><span class="sxs-lookup"><span data-stu-id="9c127-113">Execute a command in a container</span></span>

<span data-ttu-id="9c127-114">Azure Container Instances har stöd för att köra kommandon i aktiva containers.</span><span class="sxs-lookup"><span data-stu-id="9c127-114">Azure Container Instances supports executing a command in a running container.</span></span> <span data-ttu-id="9c127-115">Under apputveckling och felsökning är det särskilt användbart att kunna köra kommandon i containers som redan har startats.</span><span class="sxs-lookup"><span data-stu-id="9c127-115">Running a command in a container you've already started is especially helpful during application development and troubleshooting.</span></span> <span data-ttu-id="9c127-116">Den här funktionen används oftast till att starta ett interaktivt gränssnitt så att du kan felsöka problem i containern som körs.</span><span class="sxs-lookup"><span data-stu-id="9c127-116">The most common use of this feature is to launch an interactive shell, so that you can debug issues in a running container.</span></span>

<span data-ttu-id="9c127-117">Det här exemplet startar en interaktiv terminalsession med containern som körs:</span><span class="sxs-lookup"><span data-stu-id="9c127-117">This example starts an interactive terminal session with the running container:</span></span>

```azurecli
az container exec --resource-group myResourceGroup --name mycontainer --exec-command /bin/sh
```

<span data-ttu-id="9c127-118">När kommandot har slutförts arbetar du i praktiken inuti containern.</span><span class="sxs-lookup"><span data-stu-id="9c127-118">Once the command has completed, you are effectively working inside of the container.</span></span> <span data-ttu-id="9c127-119">I det här exemplet kördes kommandot `ls` för att visa innehållet i arbetskatalogen:</span><span class="sxs-lookup"><span data-stu-id="9c127-119">In this example, the `ls` command was run to display the contents of the working directory:</span></span>

```output
usr/src/app # ls
index.html         node_modules       package.json
index.js           package-lock.json
```

<span data-ttu-id="9c127-120">Ange `exit` för att stoppa fjärrsessionen.</span><span class="sxs-lookup"><span data-stu-id="9c127-120">Enter `exit` to stop the remote session.</span></span>

## <a name="monitor-container-cpu-and-memory"></a><span data-ttu-id="9c127-121">Övervaka containerns CPU och minne</span><span class="sxs-lookup"><span data-stu-id="9c127-121">Monitor container CPU and memory</span></span>

<span data-ttu-id="9c127-122">Du kanske vill hämta mått om användningen av processorkraft och minne.</span><span class="sxs-lookup"><span data-stu-id="9c127-122">You may want to pull metrics on CPU and memory usage.</span></span> <span data-ttu-id="9c127-123">Då måste du först hämta Azure-containerinstansens ID.</span><span class="sxs-lookup"><span data-stu-id="9c127-123">To do so, first get the ID of the Azure container instance.</span></span> <span data-ttu-id="9c127-124">I det här exemplet ligger ID:t i en variabel med namnet `CONTAINER_ID`:</span><span class="sxs-lookup"><span data-stu-id="9c127-124">In this example, the ID is placed in a variable named `CONTAINER_ID`:</span></span>

```azurecli
CONTAINER_ID=$(az container show --resource-group myResourceGroup --name mycontainer --query id --output tsv)
```

<span data-ttu-id="9c127-125">Nu kan använda kommandot `az monitor metrics list` till att hämta information om processoranvändningen:</span><span class="sxs-lookup"><span data-stu-id="9c127-125">Now, use the `az monitor metrics list` command to pull back CPU usage information:</span></span>

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric CPUUsage --output table
```

<span data-ttu-id="9c127-126">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="9c127-126">Example output:</span></span>

```output
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:39:00  CPU Usage
2018-08-20 21:40:00  CPU Usage
2018-08-20 21:41:00  CPU Usage
2018-08-20 21:42:00  CPU Usage
2018-08-20 21:43:00  CPU Usage      0.375
2018-08-20 21:44:00  CPU Usage      0.875
2018-08-20 21:45:00  CPU Usage      1
2018-08-20 21:46:00  CPU Usage      3.625
2018-08-20 21:47:00  CPU Usage      1.5
2018-08-20 21:48:00  CPU Usage      2.75
2018-08-20 21:49:00  CPU Usage      1.625
2018-08-20 21:50:00  CPU Usage      0.625
2018-08-20 21:51:00  CPU Usage      0.5
2018-08-20 21:52:00  CPU Usage      0.5
2018-08-20 21:53:00  CPU Usage      0.5
```

<span data-ttu-id="9c127-127">Du kan använda följande kommando till att hämta information om minnesanvändningen:</span><span class="sxs-lookup"><span data-stu-id="9c127-127">The following command can be used to get memory usage information:</span></span>

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric MemoryUsage --output table
```

<span data-ttu-id="9c127-128">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="9c127-128">Example output:</span></span>

```output
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:43:00  Memory Usage
2018-08-20 21:44:00  Memory Usage  0.0
2018-08-20 21:45:00  Memory Usage  15917056.0
2018-08-20 21:46:00  Memory Usage  16744448.0
2018-08-20 21:47:00  Memory Usage  16842752.0
2018-08-20 21:48:00  Memory Usage  17190912.0
2018-08-20 21:49:00  Memory Usage  17506304.0
2018-08-20 21:50:00  Memory Usage  17702912.0
2018-08-20 21:51:00  Memory Usage  17965056.0
2018-08-20 21:52:00  Memory Usage  18509824.0
2018-08-20 21:53:00  Memory Usage  18649088.0
2018-08-20 21:54:00  Memory Usage  18845696.0
2018-08-20 21:55:00  Memory Usage  19181568.0
```

<span data-ttu-id="9c127-129">Den här informationen är också tillgänglig i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9c127-129">This information is also available in the Azure portal.</span></span> <span data-ttu-id="9c127-130">En grafisk representation av processor- och minnesanvändningen finns på sidan Översikt för en containerinstans i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9c127-130">To see graphical representation of CPU and memory usage information, visit the Azure portal overview page for a container instance.</span></span>

![Vy i Azure Portal över processor- och minnesanvändning för Azure Container Instances](../media-draft/cpu-memory.png)

## <a name="clean-up"></a><span data-ttu-id="9c127-132">Rensa</span><span class="sxs-lookup"><span data-stu-id="9c127-132">Clean up</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="9c127-133">Detta är den sista utbildningsenheten i modulen för Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="9c127-133">This is the last unit of the Azure Container Instances learning module.</span></span> <span data-ttu-id="9c127-134">Nu kan du rensa alla resurser du har skapat genom att ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9c127-134">At this point, you can cleanup the created resources by deleting the resource group.</span></span> <span data-ttu-id="9c127-135">Det gör du med kommandot **az group delete**:</span><span class="sxs-lookup"><span data-stu-id="9c127-135">To do so, use the **az group delete** command:</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a><span data-ttu-id="9c127-136">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9c127-136">Summary</span></span>

<span data-ttu-id="9c127-137">I den här utbildningsenheten har du lärt dig mer om felsökningsåtgärder som att hämta containerloggar, containerhändelser och att ansluta till en containerinstans.</span><span class="sxs-lookup"><span data-stu-id="9c127-137">In this unit, you learned about several troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span>