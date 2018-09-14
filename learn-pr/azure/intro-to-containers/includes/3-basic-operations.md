Nu när du har en fungerande container-utvecklingsmiljö kan vi titta på kommandon för att köra, lista, och ta bort behållare.

## <a name="create-and-run-a-basic-container"></a>Skapa och köra en grundläggande behållare

Skapa din första container med nedanstående kommando.

```bash
docker run alpine echo "Hello World"
```

Du bör se utdata som liknar följande:

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

Kommandot `docker run` skapar en instans av en container. I det här fallet har containern skapats från en containeravbildning med namnet `alpine`, som laddades ned till den lokala datorn. När containern startades kördes kommandot `echo "Hello World"` inuti containern och utdatan skickades till din terminal.

## <a name="get-container-images"></a>Hämta containeravbildningar

En behållaravbildning innehåller det grundläggande operativsystemet och ytterligare processer, program och konfigurationer. Containeravbildningar lagras i ett register för containeravbildningar. I exemplet ”Hello World” i *alpine* bild hämtades från Docker Hub, vilket är en offentlig container registry.

Kör följande kommando för att se en lista med avbildningar som har laddats ned till datorn.

```bash
docker images
```

Om du kör den `docker run alpine` kommandot tidigare, och du bör se alpine bilden i listan.

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

Använd kommandot `docker search` om du vill söka efter en containeravbildning. Du kan exempelvis använda följande exempel för att lista alla containeravbildningar som innehåller `nginx` i namnet.

```bash
docker search nginx
```

Resultatet bör se ut ungefär så här:

```output
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                                                  Official build of Nginx.                        9071                [OK]
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1365                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   593                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   390                                     [OK]
kong                                                   Open-source Microservice & API Management la…   207                 [OK]
webdevops/php-nginx                                    Nginx with PHP-FPM                              106                                     [OK]
kitematic/hello-world-nginx                            A light-weight nginx container that demonstr…   102
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   60                                      [OK]
bitnami/nginx                                          Bitnami nginx Docker Image                      54                                      [OK]
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          38                                      [OK]
linuxserver/nginx                                      An Nginx container, brought to you by LinuxS…   37
tobi312/rpi-nginx                                      NGINX on Raspberry Pi / armhf                   20                                      [OK]
nginxdemos/nginx-ingress                               NGINX Ingress Controller for Kubernetes . Th…   11
blacklabelops/nginx                                    Dockerized Nginx Reverse Proxy Server.          10                                      [OK]
wodby/drupal-nginx                                     Nginx for Drupal container image                9                                       [OK]
webdevops/nginx                                        Nginx container                                 8                                       [OK]
nginxdemos/hello                                       NGINX webserver that serves a simple page co…   7                                       [OK]
centos/nginx-18-centos7                                Platform for running nginx 1.8 or building n…   6
1science/nginx                                         Nginx Docker images that include Consul Temp…   4                                       [OK]
centos/nginx-112-centos7                               Platform for running nginx 1.12 or building …   3
pebbletech/nginx-proxy                                 nginx-proxy sets up a container running ngin…   2                                       [OK]
travix/nginx                                           NGinx reverse proxy                             1                                       [OK]
toccoag/openshift-nginx                                Nginx reverse proxy for Nice running on same…   1                                       [OK]
ansibleplaybookbundle/nginx-apb                        An APB to deploy NGINX                          0                                       [OK]
mailu/nginx                                            Mailu nginx frontend                            0                                       [OK]
```

Om du vill förhandsnedladda en avbildning innan du kör den, använder du kommandot `docker pull`. I följande exempel hämtas avbildningen *nginx* till ditt system.

```bash
docker pull nginx
```

Resultatet bör se ut ungefär så här:

```output
Using default tag: latest
latest: Pulling from library/nginx
be8881be8156: Pull complete
f2f27ed9664f: Extracting [===============>                                   ]  6.652MB/22.14MB
54ff137eb1b2: Download complete
```

Kör `docker images` igen för att lista alla avbildningar i systemet. Du kommer att se att avbildningen *nginx* har lagts till i systemet.

```bash
docker images
```

Resultatet bör se ut ungefär så här:

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              c82521676580        26 hours ago        109MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

## <a name="run-containers"></a>Köra containrar

Nu när du har identifierat och hämtat avbildningen *nginx*, kör du en container från avbildningen. När du använder Docker CLI till att köra en containeravbildning, använder du kommandot `docker run`.

Den `-d` argumentet anger att behållaren körs i ett frånkopplat läge. I den här konfigurationen kör containern en angiven process. Om processen stoppas eller kraschar kommer själva containern att stoppas. Den `-p 8080:80` argumentet anger att nätverkstrafik som kommer till port 8080 på behållarvärd utvecklingssystemet i det här fallet vidarebefordras till port 80 på behållaren. Slutligen är argumentet `ngingx` namnet på containeravbildningen som ska köras.

En fullständig lista med `docker run`-argument finns i [docker run-referensen](https://docs.docker.com/engine/reference/run/).

```bash
docker run -d -p 8080:80 nginx
```

Den här åtgärden returnerar ett fullständigt container-ID.

```output
bd2424bfe7a5423d7d65efdf0b1622770d59e212db7b82862c3129fb630b5721
```

En lista med containrar som körs i ditt system med hjälp av kommandot `docker ps`.

```bash
docker ps
```

Du bör se att en container körs, vilket är NGINX-containern som kördes i det senaste steget. Observera att containern har både ett ID och ett namn. Något av dessa värden kan användas för att hantera containern. Anteckna container-ID:t. Det här värdet kommer att användas senare.

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   37 minutes ago      Up 37 minutes       0.0.0.0:8080->80/tcp   gallant_engelbart
```

Testa behållaren genom att öppna en webbläsare och ange `http://localhost:8080` för adressen. När den är klar bör du se NGINX-standardwebbsidan som innehåller ett välkomstmeddelande.

## <a name="delete-containers"></a>Ta bort containrar

Du tar bort en behållare genom att ange dess namn eller ID för den `docker rm` kommando. Försök utföra åtgärden igen med behållar-ID för behållaren som kör NGINX.

```bash
docker rm bd2424bfe7a5
```

Observera att containern inte kan tas bort eftersom den körs.

```output
Error response from daemon: You cannot remove a running container a31c5a5f2a8d6e420435bfcadbe158fa6a26ed29c005a892171505cc0c2861b2. Stop the container before attempting removal or force remove
```

Stoppa containern med kommandot `docker stop`.

```bash
docker stop bd2424bfe7a5
```

Observera att om du i det här läget kör `docker ps` för att visa alla containrar, visas inte nginx-containern.

```bash
docker ps
```

Resultatet bör se ut ungefär så här:

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Om du vill returnera en lista med alla containrar, inklusive containrar som har stoppats, lägger du till argumentet `-a` i kommandot `docker ps`.

```bash
docker ps -a
```

Resultatet bör se ut ungefär så här:

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   13 seconds ago      Exited (0) 3 seconds ago                       focused_spence
```

Försök att utföra borttagningen igen. Ersätt ID:t i det här exemplet med ID:t från din miljö.

```bash
docker rm bd2424bfe7a5
```

Den här åtgärden returnerar container-ID:t.

```output
bd2424bfe7a5
```

## <a name="delete-a-container-image"></a>Ta bort en containeravbildning

Du tar bort en behållare avbildningen med hjälp av den `docker rmi` kommando. Om någon container (som körs eller har stoppats) har startats från containeravbildningen, kan inte avbildningen tas bort. Containrarna måste först tas bort. Att lägga till den `-f` argumentet för den `docker rmi` kommando tvingar fram borttagningen av alla associerade behållare och tar sedan bort behållaravbildningen.

Ta bort NGINX-containeravbildningen med följande kommando.

```bash
docker rmi nginx
```

Utdata bör se ut ungefär så här:

```output
Untagged: nginx:latest
Untagged: nginx@sha256:4a5573037f358b6cdfa2f3e8a9c33a5cf11bcd1675ca72ca76fbe5bd77d0d682
Deleted: sha256:8b89e48b5f157d9455c963b57c85d21e2337c58b8c983bc06f88476610adc129
Deleted: sha256:119ded3eca5e85ef43ee966e74564c604ccda064d955a8c5ed762e1d5e87f428
Deleted: sha256:6ece91c2763d826487e707f7b8ec063742ad0ee56cc9e605465cce95550c9a7f
Deleted: sha256:cdb3f9544e4c61d45da1ea44f7d92386639a052c620d1550376f22f5b46981af
```