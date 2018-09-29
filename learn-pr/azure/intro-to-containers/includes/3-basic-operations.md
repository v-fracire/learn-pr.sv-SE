<span data-ttu-id="37d2e-101">Nu när du har en fungerande container-utvecklingsmiljö kan vi titta på några vanliga sätt att köra, lista, och ta bort containrar.</span><span class="sxs-lookup"><span data-stu-id="37d2e-101">Now that you have a functioning container development environment, let's look at some common ways to run, list, and delete containers.</span></span>

<span data-ttu-id="37d2e-102">Här lär du dig att göra följande:</span><span class="sxs-lookup"><span data-stu-id="37d2e-102">Here, you'll learn how to:</span></span>

* <span data-ttu-id="37d2e-103">Köra en grundläggande container.</span><span class="sxs-lookup"><span data-stu-id="37d2e-103">Run basic containers.</span></span>
* <span data-ttu-id="37d2e-104">Ladda ned containeravbildningar.</span><span class="sxs-lookup"><span data-stu-id="37d2e-104">Download container images.</span></span>
* <span data-ttu-id="37d2e-105">Ta bort containrar och de associerade bilderna.</span><span class="sxs-lookup"><span data-stu-id="37d2e-105">Delete containers and their associated images.</span></span>

## <a name="whats-a-container-image"></a><span data-ttu-id="37d2e-106">Vad är en containeravbildning?</span><span class="sxs-lookup"><span data-stu-id="37d2e-106">What's a container image?</span></span>

<span data-ttu-id="37d2e-107">En container_avbildning_ innehåller det grundläggande operativsystemet och eventuella ytterligare processer, program och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="37d2e-107">A container _image_ includes the base operating system and any additional processes, applications, and configurations.</span></span> <span data-ttu-id="37d2e-108">Likt en virtuell dator är en _container_ en instans som körs av en avbildning.</span><span class="sxs-lookup"><span data-stu-id="37d2e-108">Much like a virtual machine, a _container_ is a running instance of an image.</span></span>

## <a name="connect-to-your-linux-vm"></a><span data-ttu-id="37d2e-109">Ansluta till den virtuella Linux-datorn</span><span class="sxs-lookup"><span data-stu-id="37d2e-109">Connect to your Linux VM</span></span>

<span data-ttu-id="37d2e-110">Om du har kopplat från SSH-sessionen som du skapade i den föregående delen måste du logga in.</span><span class="sxs-lookup"><span data-stu-id="37d2e-110">If you disconnected from the SSH session you created in the last part, you will need to log in.</span></span> <span data-ttu-id="37d2e-111">Här är en kort repetition.</span><span class="sxs-lookup"><span data-stu-id="37d2e-111">Here's a refresher.</span></span>

1. <span data-ttu-id="37d2e-112">Hämta IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="37d2e-112">Get the IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. <span data-ttu-id="37d2e-113">SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="37d2e-113">SSH into the VM.</span></span> <span data-ttu-id="37d2e-114">Ersätt **IP-adressen** med din egen.</span><span class="sxs-lookup"><span data-stu-id="37d2e-114">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

## <a name="create-and-run-a-basic-container"></a><span data-ttu-id="37d2e-115">Skapa och kör en grundläggande container</span><span class="sxs-lookup"><span data-stu-id="37d2e-115">Create and run a basic container</span></span>

<span data-ttu-id="37d2e-116">Här kör du en container som baseras på Alpine Linux.</span><span class="sxs-lookup"><span data-stu-id="37d2e-116">Here you'll run a container that's based on Alpine Linux.</span></span> <span data-ttu-id="37d2e-117">Alpine Linux är populärt på grund av storleken &mdash; containeravbildningar kan vara så små som 5 MB.</span><span class="sxs-lookup"><span data-stu-id="37d2e-117">Alpine Linux is popular because of its size &mdash; container images can be as small as 5 MB.</span></span>

<span data-ttu-id="37d2e-118">Kör kommandot `docker run` om du vill skapa en container som baseras på Alpine Linux.</span><span class="sxs-lookup"><span data-stu-id="37d2e-118">Run this `docker run` command to create a container based on Alpine Linux.</span></span>

```bash
docker run alpine echo "Hello World"
```

<span data-ttu-id="37d2e-119">Du ser utdata som liknar det här:</span><span class="sxs-lookup"><span data-stu-id="37d2e-119">You see output similar to this:</span></span>

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

<span data-ttu-id="37d2e-120">Kommandot `docker run` skapar en container, kör det angivna kommandot och förstör sedan containern.</span><span class="sxs-lookup"><span data-stu-id="37d2e-120">The `docker run` command creates a container, runs the provided command, and then destroys the container.</span></span>

<span data-ttu-id="37d2e-121">Här laddar Docker ned [alpine](https://hub.docker.com/_/alpine?azure-portal=true)-avbildningen från Docker Hub, startar containern och skriver sedan ut ”Hello World” till konsolen.</span><span class="sxs-lookup"><span data-stu-id="37d2e-121">Here, Docker downloads the [alpine](https://hub.docker.com/_/alpine?azure-portal=true) image from Docker Hub, starts the container, and then prints "Hello World" to the console.</span></span>

<span data-ttu-id="37d2e-122">I det här fallet körs kommandot `echo` en kort stund och avslutas sedan.</span><span class="sxs-lookup"><span data-stu-id="37d2e-122">In this case, the `echo` command runs briefly and then exits.</span></span> <span data-ttu-id="37d2e-123">I den föregående delen körs Nginx i förgrunden och håller därmed igång containern tills du stoppar containern eller Nginx-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="37d2e-123">In the previous part, Nginx runs in the foreground and therefore keeps your container alive until you stop the container or stop the Nginx service.</span></span>

## <a name="get-container-images"></a><span data-ttu-id="37d2e-124">Hämta containeravbildningar</span><span class="sxs-lookup"><span data-stu-id="37d2e-124">Get container images</span></span>

<span data-ttu-id="37d2e-125">Containeravbildningar lagras i ett register för containeravbildningar.</span><span class="sxs-lookup"><span data-stu-id="37d2e-125">Container images are stored in a container image registry.</span></span> <span data-ttu-id="37d2e-126">I det föregående exemplet hämtade Docker **alpine**-avbildningen från Docker Hub, Dockers offentliga containerregister.</span><span class="sxs-lookup"><span data-stu-id="37d2e-126">In the previous example, Docker pulls the **alpine** image from Docker Hub, Docker's public container registry.</span></span>

<span data-ttu-id="37d2e-127">Kör `docker images`, så visas en lista med avbildningar som har laddats ned till systemet.</span><span class="sxs-lookup"><span data-stu-id="37d2e-127">Run `docker images` to see a list of images that have been downloaded to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="37d2e-128">Du ser två bilder &mdash; **alpine** och **nginx**.</span><span class="sxs-lookup"><span data-stu-id="37d2e-128">You see two images &mdash; **alpine** and **nginx**.</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
alpine              latest              196d12cf6ab1        8 days ago4.41MB
nginx               latest              06144b287844        2 weeks ago109MB
```

<span data-ttu-id="37d2e-129">Använd kommandot `docker search` om du vill söka efter en containeravbildning.</span><span class="sxs-lookup"><span data-stu-id="37d2e-129">To search for a container image, use the `docker search` command.</span></span> <span data-ttu-id="37d2e-130">Kör till exempel följande om du vill få en lista med alla containeravbildningar som innehåller `nginx` i namnet.</span><span class="sxs-lookup"><span data-stu-id="37d2e-130">For example, run the following to list all container images that include `nginx` in their names.</span></span>

```bash
docker search nginx --limit 5
```

<span data-ttu-id="37d2e-131">Argumentet `--limit` i det här exemplet begränsar sökåtgärden till de första fem resultaten.</span><span class="sxs-lookup"><span data-stu-id="37d2e-131">The `--limit` argument in this example restricts the search operation to the first five results.</span></span>

<span data-ttu-id="37d2e-132">Du ser något som liknar det här.</span><span class="sxs-lookup"><span data-stu-id="37d2e-132">You see something similar to this.</span></span>

```output
NAME                                     DESCRIPTION         STARS               OFFICIAL            AUTOMATED
nginx                                    Official build of Nginx.         9672                [OK]
jwilder/nginx-proxy                      Automated Nginx reverse proxy for docker con…   1415                                    [OK]
richarvey/nginx-php-fpm                  Container running Nginx + PHP-FPM capable of…   615                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion   LetsEncrypt container to use with nginx as p…   411                                     [OK]
bitnami/nginx                            Bitnami nginx Docker Image         58                                      [OK]
```

<span data-ttu-id="37d2e-133">Kommandon som `docker run` hämtar avbildningen åt dig om du inte har den lokalt.</span><span class="sxs-lookup"><span data-stu-id="37d2e-133">Commands like `docker run` pull down the image for you if you don't have it locally.</span></span> <span data-ttu-id="37d2e-134">Men det är vanligt att ladda ned en avbildning innan den används.</span><span class="sxs-lookup"><span data-stu-id="37d2e-134">But it's common to download an image before you use it.</span></span> <span data-ttu-id="37d2e-135">Det här säkerställer att du har den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="37d2e-135">This ensures you have the latest version.</span></span>

<span data-ttu-id="37d2e-136">Du gör det här med kommandot `docker pull`.</span><span class="sxs-lookup"><span data-stu-id="37d2e-136">To do so, you use the `docker pull` command.</span></span> <span data-ttu-id="37d2e-137">Kör följande om du vill hämta den senaste **nginx**-avbildningen från Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="37d2e-137">Run the following to pull the latest **nginx** image from Docker Hub.</span></span>

```bash
docker pull nginx
```

<span data-ttu-id="37d2e-138">I det här exemplet ser du att du har den senaste versionen av **nginx**-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="37d2e-138">This example shows that you have the latest version of the **nginx** image.</span></span>

```output
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
Status: Image is up to date for nginx:latest
```

## <a name="run-and-list-a-container"></a><span data-ttu-id="37d2e-139">Köra och lista en container</span><span class="sxs-lookup"><span data-stu-id="37d2e-139">Run and list a container</span></span>

<span data-ttu-id="37d2e-140">I den föregående delen satt du igång Nginx genom att använda kommandot `docker run`.</span><span class="sxs-lookup"><span data-stu-id="37d2e-140">In the previous part, you used the `docker run` command to bring up Nginx.</span></span> <span data-ttu-id="37d2e-141">Nu kör vi kommandot en gång till och tar en närmare titt på vad som händer.</span><span class="sxs-lookup"><span data-stu-id="37d2e-141">Let's run that command a second time and take a closer look into what's happening.</span></span>

1. <span data-ttu-id="37d2e-142">Sätt igång en container som kör Nginx genom att köra det här kommandot från SSH-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="37d2e-142">From your SSH connection, run this command to bring up a container running Nginx.</span></span>

    ```bash
    docker run -d -p 8080:80 nginx
    ```

    * <span data-ttu-id="37d2e-143">Argumentet `-d` anger att containern kommer att köras i ett frånkopplat läge, vilket innebär att containern körs i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="37d2e-143">The `-d` argument specifies that the container will run in a detached mode, which means the container runs in the background.</span></span> <span data-ttu-id="37d2e-144">Om Nginx-processen stoppas eller kraschar kommer även själva containern att stoppas.</span><span class="sxs-lookup"><span data-stu-id="37d2e-144">If the Nginx process stops or crashes, the container itself is also stopped.</span></span>
    * <span data-ttu-id="37d2e-145">Argumentet `-p` anger att nätverkstrafik som kommer till port 8080 i containervärden, i det här fallet den virtuella Linux-datorn, vidarebefordras till port 80 i containern.</span><span class="sxs-lookup"><span data-stu-id="37d2e-145">The `-p` argument specifies that network traffic arriving to port 8080 on the container host, your Linux VM in this case, is forwarded to port 80 on the container.</span></span> <span data-ttu-id="37d2e-146">Du använde det här argumentet i en tidigare del för få åtkomst till webbservern från webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="37d2e-146">You used this argument in the previous part to access the web server from your browser.</span></span>
    * <span data-ttu-id="37d2e-147">Argumentet `nginx` är namnet på containeravbildningen som ska köras.</span><span class="sxs-lookup"><span data-stu-id="37d2e-147">The `nginx` argument is the name of the container image to run.</span></span>

    <span data-ttu-id="37d2e-148">Du kan utforska den fullständiga listan av `docker run`-alternativ i [docker run-referensen](https://docs.docker.com/engine/reference/run?azure-portal=true) i ett senare skede.</span><span class="sxs-lookup"><span data-stu-id="37d2e-148">Later, you can explore the complete list of `docker run` options in the [docker run reference](https://docs.docker.com/engine/reference/run?azure-portal=true).</span></span>

    <span data-ttu-id="37d2e-149">Kommandot `docker run` visar en unik identifierare för containern.</span><span class="sxs-lookup"><span data-stu-id="37d2e-149">The `docker run` command displays a unique identifier for the container.</span></span> <span data-ttu-id="37d2e-150">Exempel:</span><span class="sxs-lookup"><span data-stu-id="37d2e-150">For example:</span></span>

    ```output
    9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466
    ```

1. <span data-ttu-id="37d2e-151">Kör `docker ps` om du vill lista de containrar som körs i ditt system.</span><span class="sxs-lookup"><span data-stu-id="37d2e-151">Run `docker ps` to list the running containers on your system.</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="37d2e-152">Du ser Nginx-containern som du nyss startade.</span><span class="sxs-lookup"><span data-stu-id="37d2e-152">You see the Nginx container you just started.</span></span> <span data-ttu-id="37d2e-153">Observera att containern har både ett ID och ett namn.</span><span class="sxs-lookup"><span data-stu-id="37d2e-153">Notice that the container has both an ID and a name.</span></span> <span data-ttu-id="37d2e-154">Du kan använda något av dessa värden om du vill hantera containern.</span><span class="sxs-lookup"><span data-stu-id="37d2e-154">You can use either one of these values to manage the container.</span></span> <span data-ttu-id="37d2e-155">Notera container-ID:t.</span><span class="sxs-lookup"><span data-stu-id="37d2e-155">Note the container ID.</span></span> <span data-ttu-id="37d2e-156">Du kommer att använda det senare.</span><span class="sxs-lookup"><span data-stu-id="37d2e-156">You'll use it later.</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS              PORTS                  NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   4 minutes ago     Up 4 minutes        0.0.0.0:8080->80/tcp   compassionate_allen
    ```

1. <span data-ttu-id="37d2e-157">På samma sätt som du gjorde tidigare kan du navigera till den virtuella datorns IP-adress i en webbläsare om du vill se webbservern som körs.</span><span class="sxs-lookup"><span data-stu-id="37d2e-157">As you did previously, you can navigate to your VM's IP address in a browser to see the running web server.</span></span> <span data-ttu-id="37d2e-158">Glöm inte att ange port 8080 som en del av URL:en.</span><span class="sxs-lookup"><span data-stu-id="37d2e-158">Don't forget to specify port 8080 as part of the URL.</span></span>

    ![Webbplatsen som körs i en webbläsare](../media/2-nginx-browser.png)

## <a name="delete-containers"></a><span data-ttu-id="37d2e-160">Ta bort containrar</span><span class="sxs-lookup"><span data-stu-id="37d2e-160">Delete containers</span></span>

<span data-ttu-id="37d2e-161">Du använder kommandot `docker rm` om du vill ta bort en container.</span><span class="sxs-lookup"><span data-stu-id="37d2e-161">You use the `docker rm` command to delete a container.</span></span> <span data-ttu-id="37d2e-162">Du kan ange containern med containerns namn eller ID.</span><span class="sxs-lookup"><span data-stu-id="37d2e-162">You can specify the container by its name or ID.</span></span>

1. <span data-ttu-id="37d2e-163">Prova att ta bort containern som kör Nginx genom att köra `docker rm`.</span><span class="sxs-lookup"><span data-stu-id="37d2e-163">Try running `docker rm` to delete your container running Nginx.</span></span> <span data-ttu-id="37d2e-164">Kör `docker ps` om du behöver hitta ID:t.</span><span class="sxs-lookup"><span data-stu-id="37d2e-164">Run `docker ps` if you need to find the ID.</span></span> <span data-ttu-id="37d2e-165">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="37d2e-165">Here's an example.</span></span>

    ```bash
    docker rm 9d7327e31212
    ```

    <span data-ttu-id="37d2e-166">Du ser att containern inte kan tas bort eftersom den körs.</span><span class="sxs-lookup"><span data-stu-id="37d2e-166">You see that the container can't be removed because it's in the running state.</span></span>

    ```output
    Error response from daemon: You cannot remove a running container 9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466. Stop the container before attempting removal or force remove
    ```

1. <span data-ttu-id="37d2e-167">Stoppa containern genom att köra `docker stop`.</span><span class="sxs-lookup"><span data-stu-id="37d2e-167">Run `docker stop` to stop the container.</span></span> <span data-ttu-id="37d2e-168">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="37d2e-168">Here's an example.</span></span>

    ```bash
    docker stop 9d7327e31212
    ```
    
1. <span data-ttu-id="37d2e-169">Kör `docker ps` om du vill verifiera att containern inte längre körs.</span><span class="sxs-lookup"><span data-stu-id="37d2e-169">Run `docker ps` to verify that the container is no longer running.</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="37d2e-170">Du ser det här.</span><span class="sxs-lookup"><span data-stu-id="37d2e-170">You see this.</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATEDSTATUS              PORTS               NAMES
    ```

1. <span data-ttu-id="37d2e-171">Kör `docker ps` en gång till, men ange den här gången flaggan `-a`.</span><span class="sxs-lookup"><span data-stu-id="37d2e-171">Run `docker ps` a second time, but this time provide th `-a` flag.</span></span> <span data-ttu-id="37d2e-172">Då visas alla containrar, också de som har stoppats.</span><span class="sxs-lookup"><span data-stu-id="37d2e-172">This lists all containers, even those that are stopped.</span></span>

    ```bash
    docker ps -a
    ```

    <span data-ttu-id="37d2e-173">Du ser utdata som liknar det här:</span><span class="sxs-lookup"><span data-stu-id="37d2e-173">You see output similar to this:</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS                          PORTS               NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   11 minutes ago     Exited (0) About a minute ago                       compassionate_allen
    df8aeea0320f        alpine              "echo 'Hello World'"     About an hour ago   Exited (0) About an hour ago                        objective_payne
    3a7efb75c288        nginx               "nginx -g 'daemon of…"   About an hour ago   Exited (0) About an hour ago                        nginx
    ```

    <span data-ttu-id="37d2e-174">Utdatan innehåller Nginx-containern som du nyss stoppade, samt de containrar du körde före det.</span><span class="sxs-lookup"><span data-stu-id="37d2e-174">The output includes the Nginx container you just stopped as well as the other containers you ran prior.</span></span>

1. <span data-ttu-id="37d2e-175">Kör `docker rm` en gång till.</span><span class="sxs-lookup"><span data-stu-id="37d2e-175">Run `docker rm` a second time.</span></span> <span data-ttu-id="37d2e-176">Ersätt ID:t som visas med ett eget.</span><span class="sxs-lookup"><span data-stu-id="37d2e-176">Replace the ID shown with one of yours.</span></span>

    ```bash
    docker rm 9d7327e31212
    ```

1. <span data-ttu-id="37d2e-177">Kör följande `docker rm`-kommando om du vill ta bort _alla_ aktiva containrar.</span><span class="sxs-lookup"><span data-stu-id="37d2e-177">Run the following `docker rm` command to delete _all_ active containers.</span></span>

    ```bash
    docker rm $(docker ps -aq)
    ```

1. <span data-ttu-id="37d2e-178">Kör `docker ps -a` igen så ser du att det inte finns några aktiva containrar som körs eller har stoppats.</span><span class="sxs-lookup"><span data-stu-id="37d2e-178">Run `docker ps -a` again and you see that there are no active containers running or stopped.</span></span>

    ```bash
    docker ps -a
    ```

## <a name="delete-a-container-image"></a><span data-ttu-id="37d2e-179">Ta bort en containeravbildning</span><span class="sxs-lookup"><span data-stu-id="37d2e-179">Delete a container image</span></span>

<span data-ttu-id="37d2e-180">Du använder kommandot `docker rmi` om du vill ta bort en containeravbildning.</span><span class="sxs-lookup"><span data-stu-id="37d2e-180">You use the `docker rmi` command to delete a container image.</span></span>

<span data-ttu-id="37d2e-181">Du kan endast ta bort en avbildning om ingen container som körs eller har stoppats som har startats från den avbildningen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="37d2e-181">You can only delete an image if no container started from that image, running or stopped, is active.</span></span> <span data-ttu-id="37d2e-182">Argumentet `-f` framtvingar ändå en borttagning av alla associerade containrar och kommer sedan att ta bort containeravbildningen.</span><span class="sxs-lookup"><span data-stu-id="37d2e-182">However, the `-f` argument forces the removal of all associated containers and will then remove the container image.</span></span>

1. <span data-ttu-id="37d2e-183">Kör `docker rmi nginx` om du vill ta bort Nginx-avbildningen från den virtuella Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="37d2e-183">Run `docker rmi nginx` to remove the Nginx image from your Linux VM.</span></span>

    ```bash
    docker rmi nginx
    ```

    <span data-ttu-id="37d2e-184">Du ser utdata som liknar det här:</span><span class="sxs-lookup"><span data-stu-id="37d2e-184">The output you see resembles this:</span></span>

    ```output
    Untagged: nginx:latest
    Untagged: nginx@sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
    Deleted: sha256:06144b2878448774e55577ae7d66b5f43a87c2e44322b3884e4e6c70d070b262
    Deleted: sha256:824a442ee3c96744d75be3737a22cc6a47aebad1b30be67f3c2f8f29cb0aa879
    Deleted: sha256:8e3d1e9e4945f930c93c30617512998437f6edafd86676770d29b1581f2520bb
    Deleted: sha256:8b15606a9e3e430cb7ba739fde2fbb3734a19f8a59a825ffa877f9be49059817
    ```

1. <span data-ttu-id="37d2e-185">Kör `docker images` om du vill visa en lista med alla avbildningar på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="37d2e-185">Run `docker images` to list the images on your VM.</span></span> 

    ```bash
    docker images
    ```

    <span data-ttu-id="37d2e-186">Du ser **alpine**-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="37d2e-186">You see the **alpine** image.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
    alpine              latest              196d12cf6ab1        8 days ago4.41MB
    ```

1. <span data-ttu-id="37d2e-187">Kör följande `docker rmi`-kommando om du vill ta bort _alla_avbildningar från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="37d2e-187">Run the following `docker rmi` command to delete _all_ images from your VM.</span></span>

    ```bash
    docker rmi $(docker images -q)
    ```

1. <span data-ttu-id="37d2e-188">Kör `docker images` igen så ser du att det inte finns några avbildningar på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="37d2e-188">Run `docker images` again and you see that there are no images on the VM.</span></span>

    ```bash
    docker images
    ```
