Nu när du har en fungerande container-utvecklingsmiljö kan vi titta på några vanliga sätt att köra, lista, och ta bort containrar.

Här lär du dig att göra följande:

* Köra en grundläggande container.
* Ladda ned containeravbildningar.
* Ta bort containrar och de associerade bilderna.

## <a name="whats-a-container-image"></a>Vad är en containeravbildning?

En container_avbildning_ innehåller det grundläggande operativsystemet och eventuella ytterligare processer, program och konfigurationer. Likt en virtuell dator är en _container_ en instans som körs av en avbildning.

## <a name="connect-to-your-linux-vm"></a>Ansluta till den virtuella Linux-datorn

Om du har kopplat från SSH-sessionen som du skapade i den föregående delen måste du logga in. Här är en kort repetition.

1. Hämta IP-adressen.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. SSH till den virtuella datorn. Ersätt **IP-adressen** med din egen.

    ```bash
    ssh azureuser@ip-address
    ```

## <a name="create-and-run-a-basic-container"></a>Skapa och kör en grundläggande container

Här kör du en container som baseras på Alpine Linux. Alpine Linux är populärt på grund av storleken &mdash; containeravbildningar kan vara så små som 5 MB.

Kör kommandot `docker run` om du vill skapa en container som baseras på Alpine Linux.

```bash
docker run alpine echo "Hello World"
```

Du ser utdata som liknar det här:

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

Kommandot `docker run` skapar en container, kör det angivna kommandot och förstör sedan containern.

Här laddar Docker ned [alpine](https://hub.docker.com/_/alpine?azure-portal=true)-avbildningen från Docker Hub, startar containern och skriver sedan ut ”Hello World” till konsolen.

I det här fallet körs kommandot `echo` en kort stund och avslutas sedan. I den föregående delen körs Nginx i förgrunden och håller därmed igång containern tills du stoppar containern eller Nginx-tjänsten.

## <a name="get-container-images"></a>Hämta containeravbildningar

Containeravbildningar lagras i ett register för containeravbildningar. I det föregående exemplet hämtade Docker **alpine**-avbildningen från Docker Hub, Dockers offentliga containerregister.

Kör `docker images`, så visas en lista med avbildningar som har laddats ned till systemet.

```bash
docker images
```

Du ser två bilder &mdash; **alpine** och **nginx**.

```output
REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
alpine              latest              196d12cf6ab1        8 days ago4.41MB
nginx               latest              06144b287844        2 weeks ago109MB
```

Använd kommandot `docker search` om du vill söka efter en containeravbildning. Kör till exempel följande om du vill få en lista med alla containeravbildningar som innehåller `nginx` i namnet.

```bash
docker search nginx --limit 5
```

Argumentet `--limit` i det här exemplet begränsar sökåtgärden till de första fem resultaten.

Du ser något som liknar det här.

```output
NAME                                     DESCRIPTION         STARS               OFFICIAL            AUTOMATED
nginx                                    Official build of Nginx.         9672                [OK]
jwilder/nginx-proxy                      Automated Nginx reverse proxy for docker con…   1415                                    [OK]
richarvey/nginx-php-fpm                  Container running Nginx + PHP-FPM capable of…   615                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion   LetsEncrypt container to use with nginx as p…   411                                     [OK]
bitnami/nginx                            Bitnami nginx Docker Image         58                                      [OK]
```

Kommandon som `docker run` hämtar avbildningen åt dig om du inte har den lokalt. Men det är vanligt att ladda ned en avbildning innan den används. Det här säkerställer att du har den senaste versionen.

Du gör det här med kommandot `docker pull`. Kör följande om du vill hämta den senaste **nginx**-avbildningen från Docker Hub.

```bash
docker pull nginx
```

I det här exemplet ser du att du har den senaste versionen av **nginx**-avbildningen.

```output
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
Status: Image is up to date for nginx:latest
```

## <a name="run-and-list-a-container"></a>Köra och lista en container

I den föregående delen satt du igång Nginx genom att använda kommandot `docker run`. Nu kör vi kommandot en gång till och tar en närmare titt på vad som händer.

1. Sätt igång en container som kör Nginx genom att köra det här kommandot från SSH-anslutningen.

    ```bash
    docker run -d -p 8080:80 nginx
    ```

    * Argumentet `-d` anger att containern kommer att köras i ett frånkopplat läge, vilket innebär att containern körs i bakgrunden. Om Nginx-processen stoppas eller kraschar kommer även själva containern att stoppas.
    * Argumentet `-p` anger att nätverkstrafik som kommer till port 8080 i containervärden, i det här fallet den virtuella Linux-datorn, vidarebefordras till port 80 i containern. Du använde det här argumentet i en tidigare del för få åtkomst till webbservern från webbläsaren.
    * Argumentet `nginx` är namnet på containeravbildningen som ska köras.

    Du kan utforska den fullständiga listan av `docker run`-alternativ i [docker run-referensen](https://docs.docker.com/engine/reference/run?azure-portal=true) i ett senare skede.

    Kommandot `docker run` visar en unik identifierare för containern. Exempel:

    ```output
    9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466
    ```

1. Kör `docker ps` om du vill lista de containrar som körs i ditt system.

    ```bash
    docker ps
    ```

    Du ser Nginx-containern som du nyss startade. Observera att containern har både ett ID och ett namn. Du kan använda något av dessa värden om du vill hantera containern. Notera container-ID:t. Du kommer att använda det senare.

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS              PORTS                  NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   4 minutes ago     Up 4 minutes        0.0.0.0:8080->80/tcp   compassionate_allen
    ```

1. På samma sätt som du gjorde tidigare kan du navigera till den virtuella datorns IP-adress i en webbläsare om du vill se webbservern som körs. Glöm inte att ange port 8080 som en del av URL:en.

    ![Webbplatsen som körs i en webbläsare](../media/2-nginx-browser.png)

## <a name="delete-containers"></a>Ta bort containrar

Du använder kommandot `docker rm` om du vill ta bort en container. Du kan ange containern med containerns namn eller ID.

1. Prova att ta bort containern som kör Nginx genom att köra `docker rm`. Kör `docker ps` om du behöver hitta ID:t. Här är ett exempel.

    ```bash
    docker rm 9d7327e31212
    ```

    Du ser att containern inte kan tas bort eftersom den körs.

    ```output
    Error response from daemon: You cannot remove a running container 9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466. Stop the container before attempting removal or force remove
    ```

1. Stoppa containern genom att köra `docker stop`. Här är ett exempel.

    ```bash
    docker stop 9d7327e31212
    ```
    
1. Kör `docker ps` om du vill verifiera att containern inte längre körs.

    ```bash
    docker ps
    ```

    Du ser det här.

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATEDSTATUS              PORTS               NAMES
    ```

1. Kör `docker ps` en gång till, men ange den här gången flaggan `-a`. Då visas alla containrar, också de som har stoppats.

    ```bash
    docker ps -a
    ```

    Du ser utdata som liknar det här:

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS                          PORTS               NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   11 minutes ago     Exited (0) About a minute ago                       compassionate_allen
    df8aeea0320f        alpine              "echo 'Hello World'"     About an hour ago   Exited (0) About an hour ago                        objective_payne
    3a7efb75c288        nginx               "nginx -g 'daemon of…"   About an hour ago   Exited (0) About an hour ago                        nginx
    ```

    Utdatan innehåller Nginx-containern som du nyss stoppade, samt de containrar du körde före det.

1. Kör `docker rm` en gång till. Ersätt ID:t som visas med ett eget.

    ```bash
    docker rm 9d7327e31212
    ```

1. Kör följande `docker rm`-kommando om du vill ta bort _alla_ aktiva containrar.

    ```bash
    docker rm $(docker ps -aq)
    ```

1. Kör `docker ps -a` igen så ser du att det inte finns några aktiva containrar som körs eller har stoppats.

    ```bash
    docker ps -a
    ```

## <a name="delete-a-container-image"></a>Ta bort en containeravbildning

Du använder kommandot `docker rmi` om du vill ta bort en containeravbildning.

Du kan endast ta bort en avbildning om ingen container som körs eller har stoppats som har startats från den avbildningen är aktiv. Argumentet `-f` framtvingar ändå en borttagning av alla associerade containrar och kommer sedan att ta bort containeravbildningen.

1. Kör `docker rmi nginx` om du vill ta bort Nginx-avbildningen från den virtuella Linux-datorn.

    ```bash
    docker rmi nginx
    ```

    Du ser utdata som liknar det här:

    ```output
    Untagged: nginx:latest
    Untagged: nginx@sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
    Deleted: sha256:06144b2878448774e55577ae7d66b5f43a87c2e44322b3884e4e6c70d070b262
    Deleted: sha256:824a442ee3c96744d75be3737a22cc6a47aebad1b30be67f3c2f8f29cb0aa879
    Deleted: sha256:8e3d1e9e4945f930c93c30617512998437f6edafd86676770d29b1581f2520bb
    Deleted: sha256:8b15606a9e3e430cb7ba739fde2fbb3734a19f8a59a825ffa877f9be49059817
    ```

1. Kör `docker images` om du vill visa en lista med alla avbildningar på den virtuella datorn. 

    ```bash
    docker images
    ```

    Du ser **alpine**-avbildningen.

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
    alpine              latest              196d12cf6ab1        8 days ago4.41MB
    ```

1. Kör följande `docker rmi`-kommando om du vill ta bort _alla_avbildningar från den virtuella datorn.

    ```bash
    docker rmi $(docker images -q)
    ```

1. Kör `docker images` igen så ser du att det inte finns några avbildningar på den virtuella datorn.

    ```bash
    docker images
    ```
