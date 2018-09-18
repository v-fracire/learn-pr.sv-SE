<span data-ttu-id="c9329-101">Nu när du har en fungerande utvecklingsmiljö för containrar kan vi ta en snabb titt på vissa grundläggande containeråtgärder.</span><span class="sxs-lookup"><span data-stu-id="c9329-101">Now that you have a functioning container development environment, let's take a quick spin through some basic container operations.</span></span> <span data-ttu-id="c9329-102">Det här kursdelen innehåller över huvud taget inte någon fullständig lista med Docker-funktioner.</span><span class="sxs-lookup"><span data-stu-id="c9329-102">This unit doesn't include a complete list of Docker capabilities (not even close).</span></span> <span data-ttu-id="c9329-103">Avsnittet förbereder dig på att köra, lista och ta bort containrar.</span><span class="sxs-lookup"><span data-stu-id="c9329-103">This unit will prepare you to run, list, and delete containers.</span></span> <span data-ttu-id="c9329-104">I resten av modulen visas fler containeråtgärder.</span><span class="sxs-lookup"><span data-stu-id="c9329-104">Throughout the remainder of this module, you will gain additional exposure to container operations.</span></span>

## <a name="run-a-basic-container"></a><span data-ttu-id="c9329-105">Köra en grundläggande container</span><span class="sxs-lookup"><span data-stu-id="c9329-105">Run a basic container</span></span>

<span data-ttu-id="c9329-106">Innan vi fördjupar oss i att köra och hantera containrar, ska vi snabbt se hur enkelt det är att köra en container.</span><span class="sxs-lookup"><span data-stu-id="c9329-106">Before digging into the details of running and managing containers, let's quickly see just how easy it is to run a container.</span></span>

<span data-ttu-id="c9329-107">Skapa din första container med nedanstående kommando.</span><span class="sxs-lookup"><span data-stu-id="c9329-107">Create your first container with the following command.</span></span>

```bash
docker run alpine echo "Hello World"
```

<span data-ttu-id="c9329-108">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="c9329-108">You should see output similar to the following:</span></span>

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

<span data-ttu-id="c9329-109">Kommandot `docker run` skapar en instans av en container.</span><span class="sxs-lookup"><span data-stu-id="c9329-109">The `docker run` command creates an instance of a container.</span></span> <span data-ttu-id="c9329-110">I det här fallet har containern skapats från en containeravbildning med namnet `alpine`, som laddades ned till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c9329-110">In this case, the container was created from a container image named `alpine`, which was downloaded to your local system.</span></span> <span data-ttu-id="c9329-111">När containern startades kördes kommandot `echo "Hello World"` inuti containern och utdatan skickades till din terminal.</span><span class="sxs-lookup"><span data-stu-id="c9329-111">After the container started, the `echo "Hello World"` command was run inside of the container and the output echoed to your terminal.</span></span>

<span data-ttu-id="c9329-112">Just nu behöver du inte fundera över de tekniska detaljerna för dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c9329-112">At this point, don't worry about the technical details of each of these actions.</span></span> <span data-ttu-id="c9329-113">De kommer att beskrivas senare i modulen.</span><span class="sxs-lookup"><span data-stu-id="c9329-113">They will be detailed throughout this module.</span></span>

## <a name="get-container-images"></a><span data-ttu-id="c9329-114">Hämta containeravbildningar</span><span class="sxs-lookup"><span data-stu-id="c9329-114">Get container images</span></span>

<span data-ttu-id="c9329-115">Som du såg i ”Hello World”-exemplet körs containrar från en containeravbildning.</span><span class="sxs-lookup"><span data-stu-id="c9329-115">As you saw in the 'Hello World' example, containers are run from a container image.</span></span> <span data-ttu-id="c9329-116">Dessa avbildningar innehåller containerns grundläggande operativsystem och eventuella ytterligare processer, program och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="c9329-116">These images include the container base operating system and any additional processes, applications, and configurations.</span></span> <span data-ttu-id="c9329-117">Containeravbildningar lagras i ett register för containeravbildningar.</span><span class="sxs-lookup"><span data-stu-id="c9329-117">Container images are stored in a container image registry.</span></span> <span data-ttu-id="c9329-118">I ”Hello World”-exemplet hämtades avbildningen *alpine* från Docker Hub, vilket är ett offentligt containerregister.</span><span class="sxs-lookup"><span data-stu-id="c9329-118">In the 'Hello World' example, the *alpine* image was pulled from Docker Hub, which is a public container registry.</span></span>

<span data-ttu-id="c9329-119">Nu ska vi se hur man söker efter och laddar ned en containeravbildning som skapats i förväg.</span><span class="sxs-lookup"><span data-stu-id="c9329-119">Let's see how to search for and download a pre-created container image.</span></span>

<span data-ttu-id="c9329-120">Kör följande kommando för att se en lista med avbildningar som har laddats ned till datorn.</span><span class="sxs-lookup"><span data-stu-id="c9329-120">Run the following command to see a list of images that have been downloaded to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="c9329-121">Om du har följt med bör du nu se alpine-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="c9329-121">If you've been following along, you should see the alpine image.</span></span> <span data-ttu-id="c9329-122">Den här avbildningen hämtades när ”Hello World”-exemplet kördes.</span><span class="sxs-lookup"><span data-stu-id="c9329-122">This image was downloaded when the 'Hello World' example was run.</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

<span data-ttu-id="c9329-123">Använd kommandot `docker search` om du vill söka efter en containeravbildning.</span><span class="sxs-lookup"><span data-stu-id="c9329-123">To search for a container image, use the `docker search` command.</span></span> <span data-ttu-id="c9329-124">Du kan exempelvis använda följande exempel för att lista alla containeravbildningar som innehåller `nginx` i namnet.</span><span class="sxs-lookup"><span data-stu-id="c9329-124">For instance, use the following example to list all container images that include `nginx` in the name.</span></span>

```bash
docker search nginx
```

<span data-ttu-id="c9329-125">Resultatet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c9329-125">The output should look similar to the following:</span></span>

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

<span data-ttu-id="c9329-126">Om du vill förhandsnedladda en avbildning innan du kör den, använder du kommandot `docker pull`.</span><span class="sxs-lookup"><span data-stu-id="c9329-126">If you'd like to pre-download an image prior to running it, use the `docker pull` command.</span></span> <span data-ttu-id="c9329-127">I följande exempel hämtas avbildningen *nginx* till ditt system.</span><span class="sxs-lookup"><span data-stu-id="c9329-127">The following example pulls the *nginx* image to your system.</span></span>

```bash
docker pull nginx
```

<span data-ttu-id="c9329-128">Resultatet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c9329-128">The output should look similar to the following:</span></span>

```output
Using default tag: latest
latest: Pulling from library/nginx
be8881be8156: Pull complete
f2f27ed9664f: Extracting [===============>                                   ]  6.652MB/22.14MB
54ff137eb1b2: Download complete
```

<span data-ttu-id="c9329-129">Kör `docker images` igen för att lista alla avbildningar i systemet.</span><span class="sxs-lookup"><span data-stu-id="c9329-129">Run `docker images` again to list all of the images on your system.</span></span> <span data-ttu-id="c9329-130">Du kommer att se att avbildningen *nginx* har lagts till i systemet.</span><span class="sxs-lookup"><span data-stu-id="c9329-130">You'll see that the *nginx* image has been added to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="c9329-131">Resultatet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c9329-131">The output should look similar to the following:</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              c82521676580        26 hours ago        109MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

## <a name="run-containers"></a><span data-ttu-id="c9329-132">Köra containrar</span><span class="sxs-lookup"><span data-stu-id="c9329-132">Run containers</span></span>

<span data-ttu-id="c9329-133">Nu när du har identifierat och hämtat avbildningen *nginx*, kör du en container från avbildningen.</span><span class="sxs-lookup"><span data-stu-id="c9329-133">Now that you've identified and downloaded the *nginx* image, run a container from the image.</span></span> <span data-ttu-id="c9329-134">När du använder Docker CLI till att köra en containeravbildning, använder du kommandot `docker run`.</span><span class="sxs-lookup"><span data-stu-id="c9329-134">When using the Docker CLI to run a container image, use the `docker run` command.</span></span>

<span data-ttu-id="c9329-135">I följande exempel anger argumentet `-d` att containern ska köras i ett frånkopplat läge.</span><span class="sxs-lookup"><span data-stu-id="c9329-135">In the following example, the `-d` argument specifies that the container will run in a detached mode.</span></span> <span data-ttu-id="c9329-136">I den här konfigurationen kör containern en angiven process.</span><span class="sxs-lookup"><span data-stu-id="c9329-136">In this configuration, the container runs a specified process.</span></span> <span data-ttu-id="c9329-137">Om processen stoppas eller kraschar kommer själva containern att stoppas.</span><span class="sxs-lookup"><span data-stu-id="c9329-137">If that process stops or crashes, the container itself is stopped.</span></span> <span data-ttu-id="c9329-138">Argumentet `-p 8080:80` anger att nätverkstrafik som kommer till port 8080 i containervärden, vilket är utvecklingssystemet i det här fallet, vidarebefordras till port 80 i containern.</span><span class="sxs-lookup"><span data-stu-id="c9329-138">The `-p 8080:80` argument specifies that network traffic arriving to port 8080 on the container host, your development system in this case, is forwarded to port 80 of the container.</span></span> <span data-ttu-id="c9329-139">Slutligen är argumentet `ngingx` namnet på containeravbildningen som ska köras.</span><span class="sxs-lookup"><span data-stu-id="c9329-139">Finally, the `ngingx` argument is the name of the container image to run.</span></span>

<span data-ttu-id="c9329-140">En fullständig lista med `docker run`-argument finns i [docker run-referensen](https://docs.docker.com/engine/reference/run/).</span><span class="sxs-lookup"><span data-stu-id="c9329-140">For a complete list of `docker run` arguments, see the [docker run reference](https://docs.docker.com/engine/reference/run/).</span></span>

```bash
docker run -d -p 8080:80 nginx
```

<span data-ttu-id="c9329-141">Den här åtgärden returnerar ett fullständigt container-ID.</span><span class="sxs-lookup"><span data-stu-id="c9329-141">This operation returns the full container ID.</span></span>

```output
bd2424bfe7a5423d7d65efdf0b1622770d59e212db7b82862c3129fb630b5721
```

<span data-ttu-id="c9329-142">En lista med containrar som körs i ditt system med hjälp av kommandot `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="c9329-142">List the running containers on your system using the `docker ps` command.</span></span>

```bash
docker ps
```

<span data-ttu-id="c9329-143">Du bör se att en container körs, vilket är NGINX-containern som kördes i det senaste steget.</span><span class="sxs-lookup"><span data-stu-id="c9329-143">You should see a single running container, which is the NGINX container run in the last step.</span></span> <span data-ttu-id="c9329-144">Observera att containern har både ett ID och ett namn.</span><span class="sxs-lookup"><span data-stu-id="c9329-144">Notice that the container has both an ID and a Name.</span></span> <span data-ttu-id="c9329-145">Något av dessa värden kan användas för att hantera containern.</span><span class="sxs-lookup"><span data-stu-id="c9329-145">Either one of these values can be used to manage the container.</span></span> <span data-ttu-id="c9329-146">Anteckna container-ID:t.</span><span class="sxs-lookup"><span data-stu-id="c9329-146">Take note of the container ID.</span></span> <span data-ttu-id="c9329-147">Det här värdet kommer att användas senare i kursdelen.</span><span class="sxs-lookup"><span data-stu-id="c9329-147">This value will be used later in the unit.</span></span>

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   37 minutes ago      Up 37 minutes       0.0.0.0:8080->80/tcp   gallant_engelbart
```

<span data-ttu-id="c9329-148">Testa containern genom att öppna en webbläsare och ange http://localhost:8080 som adress.</span><span class="sxs-lookup"><span data-stu-id="c9329-148">To test the container, open a browser and enter http://localhost:8080 for the address.</span></span> <span data-ttu-id="c9329-149">Du bör då se standardwebbplatsen för NGINX.</span><span class="sxs-lookup"><span data-stu-id="c9329-149">After it completes, you should see the NGINX default website.</span></span>

![Microsoft Edge med välkomstskärmen för NGINX](../media-draft/3-nginx.png)

## <a name="delete-containers"></a><span data-ttu-id="c9329-151">Ta bort containrar</span><span class="sxs-lookup"><span data-stu-id="c9329-151">Delete containers</span></span>

<span data-ttu-id="c9329-152">När du är klar med en container kan den tas bort genom att du anger containerns namn eller ID i kommandot `docker rm`.</span><span class="sxs-lookup"><span data-stu-id="c9329-152">When you're done working with a container, it can be deleted by providing the container name or ID to the `docker rm` command.</span></span> <span data-ttu-id="c9329-153">Utför åtgärden igen med container-ID:t för containern som kör NGINX.</span><span class="sxs-lookup"><span data-stu-id="c9329-153">Try out this operation with the container ID of the container running NGINX.</span></span> <span data-ttu-id="c9329-154">Ersätt ID:t i det här exemplet med ID:t från din miljö.</span><span class="sxs-lookup"><span data-stu-id="c9329-154">Replace the ID in this example with the ID from your environment.</span></span>

```bash
docker rm bd2424bfe7a5
```

<span data-ttu-id="c9329-155">Observera att containern inte kan tas bort eftersom den körs.</span><span class="sxs-lookup"><span data-stu-id="c9329-155">Notice that the container can't be removed because it's in a running state.</span></span>

```output
Error response from daemon: You cannot remove a running container a31c5a5f2a8d6e420435bfcadbe158fa6a26ed29c005a892171505cc0c2861b2. Stop the container before attempting removal or force remove
```

<span data-ttu-id="c9329-156">Stoppa containern med kommandot `docker stop`.</span><span class="sxs-lookup"><span data-stu-id="c9329-156">Stop the container with the `docker stop` command.</span></span>

```bash
docker stop bd2424bfe7a5
```

<span data-ttu-id="c9329-157">Observera att om du i det här läget kör `docker ps` för att visa alla containrar, visas inte nginx-containern.</span><span class="sxs-lookup"><span data-stu-id="c9329-157">Notice at this point, if you run `docker ps` to list all containers, the nginx container is not listed.</span></span>

```bash
docker ps
```

<span data-ttu-id="c9329-158">Resultatet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c9329-158">The output should look similar to the following:</span></span>

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

<span data-ttu-id="c9329-159">Om du vill returnera en lista med alla containrar, inklusive containrar som har stoppats, lägger du till argumentet `-a` i kommandot `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="c9329-159">To return a list of all containers, including containers in a stopped state, add the `-a` argument to the `docker ps` command.</span></span>

```bash
docker ps -a
```

<span data-ttu-id="c9329-160">Resultatet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c9329-160">The output should look similar to the following:</span></span>

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   13 seconds ago      Exited (0) 3 seconds ago                       focused_spence
```

<span data-ttu-id="c9329-161">Försök att utföra borttagningen igen.</span><span class="sxs-lookup"><span data-stu-id="c9329-161">Try the delete operation again.</span></span> <span data-ttu-id="c9329-162">Ersätt ID:t i det här exemplet med ID:t från din miljö.</span><span class="sxs-lookup"><span data-stu-id="c9329-162">Replace the ID in this example with the ID from your environment.</span></span>

```bash
docker rm bd2424bfe7a5
```

<span data-ttu-id="c9329-163">Den här åtgärden returnerar container-ID:t.</span><span class="sxs-lookup"><span data-stu-id="c9329-163">This operation returns the container ID.</span></span>

```output
bd2424bfe7a5
```

## <a name="delete-a-container-image"></a><span data-ttu-id="c9329-164">Ta bort en containeravbildning</span><span class="sxs-lookup"><span data-stu-id="c9329-164">Delete a container image</span></span>

<span data-ttu-id="c9329-165">När du är klar med en containeravbildning kan den tas bort med kommandot `docker rmi`.</span><span class="sxs-lookup"><span data-stu-id="c9329-165">When you're done working with a container image, it can be removed with the `docker rmi` command.</span></span> <span data-ttu-id="c9329-166">Om någon container (som körs eller har stoppats) har startats från containeravbildningen, kan inte avbildningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="c9329-166">If any container (running or stopped) has been started from the container image, the image can't be deleted.</span></span> <span data-ttu-id="c9329-167">Containrarna måste först tas bort.</span><span class="sxs-lookup"><span data-stu-id="c9329-167">The containers first need to be removed.</span></span> <span data-ttu-id="c9329-168">När argumentet `-f` läggs till i kommandot `docker rmi` framtvingas en borttagning av alla associerade containrar och därefter tas containeravbildningen bort.</span><span class="sxs-lookup"><span data-stu-id="c9329-168">Adding the `-f` argument to the `docker rmi` command will force the removal of all associated containers, and will then remove the container image.</span></span>

<span data-ttu-id="c9329-169">Ta bort NGINX-containeravbildningen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="c9329-169">Remove the NGINX container image with the following command.</span></span>

```bash
docker rmi nginx
```

<span data-ttu-id="c9329-170">Resultatet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c9329-170">The output should look similar to the following:</span></span>

```output
Untagged: nginx:latest
Untagged: nginx@sha256:4a5573037f358b6cdfa2f3e8a9c33a5cf11bcd1675ca72ca76fbe5bd77d0d682
Deleted: sha256:8b89e48b5f157d9455c963b57c85d21e2337c58b8c983bc06f88476610adc129
Deleted: sha256:119ded3eca5e85ef43ee966e74564c604ccda064d955a8c5ed762e1d5e87f428
Deleted: sha256:6ece91c2763d826487e707f7b8ec063742ad0ee56cc9e605465cce95550c9a7f
Deleted: sha256:cdb3f9544e4c61d45da1ea44f7d92386639a052c620d1550376f22f5b46981af
```

## <a name="summary"></a><span data-ttu-id="c9329-171">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c9329-171">Summary</span></span>

<span data-ttu-id="c9329-172">I det här avsnittet har du lärt dig några grundläggande Docker-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c9329-172">In this unit, you learned about some basic Docker operations.</span></span> <span data-ttu-id="c9329-173">I nästa avsnitt skapar du en anpassad containeravbildning.</span><span class="sxs-lookup"><span data-stu-id="c9329-173">In the next unit, you will create a custom container image.</span></span>