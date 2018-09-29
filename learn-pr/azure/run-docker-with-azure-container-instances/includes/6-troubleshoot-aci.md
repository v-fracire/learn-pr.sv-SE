<span data-ttu-id="68e84-101">Här utför du en del grundläggande felsökning som att hämta containerloggar, containerhändelser och koppla dem till en containerinstans.</span><span class="sxs-lookup"><span data-stu-id="68e84-101">Here, you will perform some basic troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span> <span data-ttu-id="68e84-102">När du har gått igenom modulen bör du kunna grunderna i att felsöka containerinstanser.</span><span class="sxs-lookup"><span data-stu-id="68e84-102">By the end of this module, you should understand basic capabilities for troubleshooting container instances.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="68e84-103">Skapa en container</span><span class="sxs-lookup"><span data-stu-id="68e84-103">Create a container</span></span>

<span data-ttu-id="68e84-104">Börja med att skapa följande container:</span><span class="sxs-lookup"><span data-stu-id="68e84-104">Start by creating the following container:</span></span> 

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/sample-aks-helloworld \
    --ports 80 \
    --ip-address Public \
    --location eastus
```

## <a name="get-logs-from-a-container-instance"></a><span data-ttu-id="68e84-105">Hämta loggar från en containerinstans</span><span class="sxs-lookup"><span data-stu-id="68e84-105">Get logs from a container instance</span></span>

<span data-ttu-id="68e84-106">Om du vill visa loggar från programkoden i en container kan du använda kommandot `az container logs`:</span><span class="sxs-lookup"><span data-stu-id="68e84-106">To view logs from your application code within a container, you can use the `az container logs` command:</span></span>

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

<span data-ttu-id="68e84-107">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="68e84-107">Example output:</span></span>

```output
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
Running inside /app/prestart.sh, you could add migrations to this file, e.g.:

#! /usr/bin/env bash

# Let the DB start
sleep 10;
# Run migrations
alembic upgrade head
```

## <a name="get-container-events"></a><span data-ttu-id="68e84-108">Hämta containerhändelser</span><span class="sxs-lookup"><span data-stu-id="68e84-108">Get container events</span></span>

<span data-ttu-id="68e84-109">Med kommandot `az container attach` kan du visa diagnostisk information när containern startas.</span><span class="sxs-lookup"><span data-stu-id="68e84-109">The `az container attach` command provides diagnostic information during container startup.</span></span> <span data-ttu-id="68e84-110">När containern har startats strömmas även STDOUT och STDERR till den lokala konsolen:</span><span class="sxs-lookup"><span data-stu-id="68e84-110">Once the container has started, it also streams STDOUT and STDERR to your local console:</span></span>

```azurecli
az container attach \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

<span data-ttu-id="68e84-111">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="68e84-111">Example output:</span></span>

```output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-09-21 23:48:14+00:00) pulling image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:09+00:00) Successfully pulled image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:12+00:00) Created container
(count: 1) (last timestamp: 2018-09-21 23:49:13+00:00) Started container

Start streaming logs:
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
```

> [!TIP]
> <span data-ttu-id="68e84-112">Om du vill koppla från en ansluten container trycker du på <kbd>Ctrl+C</kbd>.</span><span class="sxs-lookup"><span data-stu-id="68e84-112">To disconnect from an attached container, press <kbd>Ctrl+C</kbd>.</span></span>

## <a name="execute-a-command-in-a-container"></a><span data-ttu-id="68e84-113">Köra ett kommando i en container</span><span class="sxs-lookup"><span data-stu-id="68e84-113">Execute a command in a container</span></span>

<span data-ttu-id="68e84-114">Azure Container Instances har stöd för att köra kommandon i aktiva containers.</span><span class="sxs-lookup"><span data-stu-id="68e84-114">Azure Container Instances supports executing a command in a running container.</span></span> <span data-ttu-id="68e84-115">Under apputveckling och felsökning är det särskilt användbart att kunna köra kommandon i containers som redan har startats.</span><span class="sxs-lookup"><span data-stu-id="68e84-115">Running a command in a container you've already started is especially helpful during application development and troubleshooting.</span></span> <span data-ttu-id="68e84-116">Den här funktionen används oftast till att starta ett interaktivt gränssnitt så att du kan felsöka problem i en container som körs.</span><span class="sxs-lookup"><span data-stu-id="68e84-116">The most common use of this feature is to launch an interactive shell so that you can debug issues in a running container.</span></span>

<span data-ttu-id="68e84-117">Det här exemplet startar en interaktiv terminalsession med containern som körs:</span><span class="sxs-lookup"><span data-stu-id="68e84-117">This example starts an interactive terminal session with the running container:</span></span>

```azurecli
az container exec \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --exec-command /bin/sh
```

<span data-ttu-id="68e84-118">När kommandot har slutförts arbetar du i praktiken inuti containern.</span><span class="sxs-lookup"><span data-stu-id="68e84-118">Once the command has completed, you are effectively working inside of the container.</span></span> <span data-ttu-id="68e84-119">I det här exemplet kördes kommandot `ls` för att visa innehållet i arbetskatalogen:</span><span class="sxs-lookup"><span data-stu-id="68e84-119">In this example, the `ls` command was run to display the contents of the working directory:</span></span>

```output
# ls
__pycache__  main.py  prestart.sh  static  templates  uwsgi.ini
```

<span data-ttu-id="68e84-120">Kör `exit` om du vill stoppa fjärrsessionen.</span><span class="sxs-lookup"><span data-stu-id="68e84-120">Run `exit` to stop the remote session.</span></span>

## <a name="monitor-container-cpu-and-memory"></a><span data-ttu-id="68e84-121">Övervaka containerns CPU och minne</span><span class="sxs-lookup"><span data-stu-id="68e84-121">Monitor container CPU and memory</span></span>

<span data-ttu-id="68e84-122">Du kanske vill hämta mått om användningen av processorkraft och minne.</span><span class="sxs-lookup"><span data-stu-id="68e84-122">You may want to pull metrics on CPU and memory usage.</span></span> <span data-ttu-id="68e84-123">Då måste du först hämta Azure-containerinstansens ID.</span><span class="sxs-lookup"><span data-stu-id="68e84-123">To do so, first get the ID of the Azure container instance.</span></span> <span data-ttu-id="68e84-124">I det här exemplet ligger ID:t i en variabel med namnet `CONTAINER_ID`:</span><span class="sxs-lookup"><span data-stu-id="68e84-124">In this example, the ID is placed in a variable named `CONTAINER_ID`:</span></span>

```azurecli
CONTAINER_ID=$(az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name mycontainer --query id --output tsv)
```

<span data-ttu-id="68e84-125">Nu kan använda kommandot `az monitor metrics list` till att hämta information om processoranvändningen:</span><span class="sxs-lookup"><span data-stu-id="68e84-125">Now, use the `az monitor metrics list` command to pull back CPU usage information:</span></span>

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric CPUUsage \
    --output table
```

<span data-ttu-id="68e84-126">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="68e84-126">Example output:</span></span>

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

<span data-ttu-id="68e84-127">Du kan använda följande kommando till att hämta information om minnesanvändningen:</span><span class="sxs-lookup"><span data-stu-id="68e84-127">The following command can be used to get memory usage information:</span></span>

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric MemoryUsage \
    --output table
```

<span data-ttu-id="68e84-128">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="68e84-128">Example output:</span></span>

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

<span data-ttu-id="68e84-129">Den här informationen är också tillgänglig i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="68e84-129">This information is also available in the Azure portal.</span></span> <span data-ttu-id="68e84-130">En grafisk representation av CPU- och minnesanvändningen finns på översiktssidan för en containerinstans i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="68e84-130">To see a visual representation of CPU and memory usage information, visit the Azure portal overview page for a container instance.</span></span>

![Vy i Azure-portalen över CPU- och minnesanvändning för Azure Container Instances](../media/6-cpu-memory.png)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
