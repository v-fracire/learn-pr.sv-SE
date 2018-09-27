Här utför du en del grundläggande felsökning som att hämta containerloggar, containerhändelser och koppla dem till en containerinstans. När du har gått igenom modulen bör du kunna grunderna i att felsöka containerinstanser.

## <a name="create-a-container"></a>Skapa en container

Börja med att skapa följande container: 

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/sample-aks-helloworld \
    --ports 80 \
    --ip-address Public \
    --location eastus
```

## <a name="get-logs-from-a-container-instance"></a>Hämta loggar från en containerinstans

Om du vill visa loggar från programkoden i en container kan du använda kommandot `az container logs`:

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

Exempel på utdata:

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

## <a name="get-container-events"></a>Hämta containerhändelser

Med kommandot `az container attach` kan du visa diagnostisk information när containern startas. När containern har startats strömmas även STDOUT och STDERR till den lokala konsolen:

```azurecli
az container attach \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

Exempel på utdata:

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
> Om du vill koppla från en ansluten container trycker du på <kbd>Ctrl+C</kbd>.

## <a name="execute-a-command-in-a-container"></a>Köra ett kommando i en container

Azure Container Instances har stöd för att köra kommandon i aktiva containers. Under apputveckling och felsökning är det särskilt användbart att kunna köra kommandon i containers som redan har startats. Den här funktionen används oftast till att starta ett interaktivt gränssnitt så att du kan felsöka problem i en container som körs.

Det här exemplet startar en interaktiv terminalsession med containern som körs:

```azurecli
az container exec \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --exec-command /bin/sh
```

När kommandot har slutförts arbetar du i praktiken inuti containern. I det här exemplet kördes kommandot `ls` för att visa innehållet i arbetskatalogen:

```output
# ls
__pycache__  main.py  prestart.sh  static  templates  uwsgi.ini
```

Kör `exit` om du vill stoppa fjärrsessionen.

## <a name="monitor-container-cpu-and-memory"></a>Övervaka containerns CPU och minne

Du kanske vill hämta mått om användningen av processorkraft och minne. Då måste du först hämta Azure-containerinstansens ID. I det här exemplet ligger ID:t i en variabel med namnet `CONTAINER_ID`:

```azurecli
CONTAINER_ID=$(az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name mycontainer --query id --output tsv)
```

Nu kan använda kommandot `az monitor metrics list` till att hämta information om processoranvändningen:

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric CPUUsage \
    --output table
```

Exempel på utdata:

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

Du kan använda följande kommando till att hämta information om minnesanvändningen:

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric MemoryUsage \
    --output table
```

Exempel på utdata:

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

Den här informationen är också tillgänglig i Azure-portalen. En grafisk representation av CPU- och minnesanvändningen finns på översiktssidan för en containerinstans i Azure-portalen.

![Vy i Azure-portalen över CPU- och minnesanvändning för Azure Container Instances](../media/6-cpu-memory.png)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
