I den här utbildningsenheten utför du en del grundläggande felsökning som att hämta containerloggar, containerhändelser och koppla dem till en containerinstans. När du har gått igenom modulen bör du kunna grunderna i att felsöka containerinstanser.

## <a name="create-a-container"></a>Skapa en container

Börja med att skapa containern vi ska använda. Om du fortfarande har den första containern vi skapade i modulen kan du hoppa över det här steget:

```azurecli
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --ports 80 --ip-address Public
```

## <a name="get-logs-from-a-container-instance"></a>Hämta loggar från en containerinstans

Om du vill visa loggar från programkoden i en container kan du använda kommandot `az container logs`:

```azazurecli
az container logs --resource-group myResourceGroup --name mycontainer
```

Här är loggutdata från exempelcontainern när webbappen har använts några gånger:

```bash
listening on port 80
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://23.101.136.193/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:25 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:27 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
```

## <a name="get-container-events"></a>Hämta containerhändelser

Med kommandot `az container attach` kan du visa diagnostisk information när containern startas. När containern har startats strömmas även STDOUT och STDERR till den lokala konsolen:

```azazurecli
az container attach --resource-group myResourceGroup --name mycontainer
```

Exempel på utdata:


```bash
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-08-20 21:43:26+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:32+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Created container
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Started container

Start streaming logs:
listening on port 80
listening on port 80
```

## <a name="execute-a-command-in-a-container"></a>Köra ett kommando i en container

Azure Container Instances har stöd för att köra kommandon i aktiva containers. Under apputveckling och felsökning är det särskilt användbart att kunna köra kommandon i containers som redan har startats. Den här funktionen används oftast till att starta ett interaktivt gränssnitt så att du kan felsöka problem i containern som körs.

Det här exemplet startar en interaktiv terminalsession med containern som körs:

```azurecli
az container exec --resource-group myResourceGroup --name mycontainer --exec-command /bin/sh
```

När kommandot har slutförts arbetar du i praktiken inuti containern. I det här exemplet kördes kommandot `ls` för att visa innehållet i arbetskatalogen:

```bash
usr/src/app # ls
index.html         node_modules       package.json
index.js           package-lock.json
```

Ange `exit` för att stoppa fjärrsessionen.

## <a name="monitor-container-cpu-and-memory"></a>Övervaka containerns CPU och minne

Du kanske vill hämta mått om användningen av processorkraft och minne. Då måste du först hämta Azure-containerinstansens ID. I det här exemplet ligger ID:t i en variabel med namnet `CONTAINER_ID`:

```azurecli
CONTAINER_ID=$(az container show --resource-group myResourceGroup --name mycontainer --query id --output tsv)
```

Nu kan använda kommandot `az monitor metrics list` till att hämta information om processoranvändningen:

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric CPUUsage --output table
```

Exempel på utdata:

```bash
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
az monitor metrics list --resource $CONTAINER_ID --metric MemoryUsage --output table
```

Exempel på utdata:

```bash
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

Den här informationen är också tillgänglig i Azure Portal. En grafisk representation av processor- och minnesanvändningen finns på sidan Översikt för en containerinstans i Azure Portal.

![Vy i Azure Portal över processor- och minnesanvändning för Azure Container Instances](../media-draft/cpu-memory.png)

## <a name="clean-up"></a>Rensa
<!---TODO: Update for sandbox?--->

Detta är den sista utbildningsenheten i modulen för Azure Container Instances. Nu kan du rensa alla resurser du har skapat genom att ta bort resursgruppen. Det gör du med kommandot **az group delete**:

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du lärt dig mer om felsökningsåtgärder som att hämta containerloggar, containerhändelser och att ansluta till en containerinstans.