<span data-ttu-id="eea30-101">Fram tills nu har du laddat ned och kört Docker-avbildningar som skapats av andra.</span><span class="sxs-lookup"><span data-stu-id="eea30-101">Up until now, you downloaded and ran Docker images created by others.</span></span> <span data-ttu-id="eea30-102">Här ska du skapa dina egna containeravbildningar.</span><span class="sxs-lookup"><span data-stu-id="eea30-102">Here, you'll build your own container images.</span></span> <span data-ttu-id="eea30-103">Du lär dig följande:</span><span class="sxs-lookup"><span data-stu-id="eea30-103">You'll learn how to:</span></span>

* <span data-ttu-id="eea30-104">Skapa en avbildning med hjälp av en manuell process.</span><span class="sxs-lookup"><span data-stu-id="eea30-104">Create an image using a manual process.</span></span>
* <span data-ttu-id="eea30-105">Skapa samma avbildning med hjälp av en mer automatiserad process som kallas Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="eea30-105">Create the same image using a more automated process, called a Dockerfile.</span></span>
* <span data-ttu-id="eea30-106">Publicera en avbildning i Docker Hub, ett offentligt containerregister.</span><span class="sxs-lookup"><span data-stu-id="eea30-106">Publish your image to Docker Hub, a public container registry.</span></span>

## <a name="what-is-a-dockerfile"></a><span data-ttu-id="eea30-107">Vad är en Dockerfile?</span><span class="sxs-lookup"><span data-stu-id="eea30-107">What is a Dockerfile?</span></span>

<span data-ttu-id="eea30-108">En Dockerfile är en textfil som beskriver allt som containern ska köra.</span><span class="sxs-lookup"><span data-stu-id="eea30-108">A Dockerfile is a text file that describes everything your container needs to run.</span></span>

<span data-ttu-id="eea30-109">En instruktion i en Dockerfile kan definiera:</span><span class="sxs-lookup"><span data-stu-id="eea30-109">An instruction in the Dockerfile can define:</span></span>

* <span data-ttu-id="eea30-110">Den överordnade avbildning som din avbildning baseras på.</span><span class="sxs-lookup"><span data-stu-id="eea30-110">The parent image your image is based on.</span></span>
* <span data-ttu-id="eea30-111">Ett programvarupaket som ska installeras.</span><span class="sxs-lookup"><span data-stu-id="eea30-111">A software package to install.</span></span>
* <span data-ttu-id="eea30-112">En konfigurationsfil eller en annan fil som din app ska köra.</span><span class="sxs-lookup"><span data-stu-id="eea30-112">A configuration or other file your app needs to run.</span></span>
* <span data-ttu-id="eea30-113">Nätverksinställningar, till exempel en port som du vill göra tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="eea30-113">Networking settings, such as a port you want to make available.</span></span>
* <span data-ttu-id="eea30-114">Kommandot som ska köras när containern startas.</span><span class="sxs-lookup"><span data-stu-id="eea30-114">The command to run when the container starts.</span></span>

<span data-ttu-id="eea30-115">Du kan skapa en containeravbildning med hjälp av en manuell process eller använda en Dockerfile för att automatisera processen.</span><span class="sxs-lookup"><span data-stu-id="eea30-115">You can create a container image using a manual process or use a Dockerfile to automate the process.</span></span> <span data-ttu-id="eea30-116">Ett bra sätt att få förståelse för processen är att skapa en Docker-avbildning manuellt.</span><span class="sxs-lookup"><span data-stu-id="eea30-116">Creating a Docker image manually is a good way to understand the process.</span></span> <span data-ttu-id="eea30-117">Om du använder en Dockerfile blir processen snabbare och enklare att upprepa.</span><span class="sxs-lookup"><span data-stu-id="eea30-117">Using a Dockerfile makes the process faster and easier to repeat.</span></span>

## <a name="what-is-docker-hub"></a><span data-ttu-id="eea30-118">Vad är Docker Hub?</span><span class="sxs-lookup"><span data-stu-id="eea30-118">What is Docker Hub?</span></span>

<span data-ttu-id="eea30-119">Docker Hub är en plats där du kan lagra och ladda ned Docker-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="eea30-119">Docker Hub is a place for you to store and download Docker images.</span></span> <span data-ttu-id="eea30-120">Du kan ladda ned Docker-avbildningar som har skapats av Docker och Docker-communityn, till exempel Nginx-avbildningen som du använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="eea30-120">You can download Docker images that have been created by Docker and the Docker community, such as the Nginx image you used previously.</span></span> <span data-ttu-id="eea30-121">Du kan också dela dina egna avbildningar i Docker-communityn eller bara med ditt team.</span><span class="sxs-lookup"><span data-stu-id="eea30-121">You can also share your own images with the Docker community or just with your team.</span></span>

## <a name="create-an-image-manually"></a><span data-ttu-id="eea30-122">Skapa en avbildning manuellt</span><span class="sxs-lookup"><span data-stu-id="eea30-122">Create an image manually</span></span>

<span data-ttu-id="eea30-123">Här ska du skapa en Docker-avbildning som kör ett grundläggande Python-program.</span><span class="sxs-lookup"><span data-stu-id="eea30-123">Here you'll create a Docker image that runs a basic Python application.</span></span>

1. <span data-ttu-id="eea30-124">Starta genom att hämta en containerinstans från [python](https://hub.docker.com/_/python/?azure-portal=true)-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="eea30-124">Start by bringing up a container instance from the [python](https://hub.docker.com/_/python/?azure-portal=true) image.</span></span>

    ```bash
    docker run --name python-demo -it python bash
    ```

    <span data-ttu-id="eea30-125">Det tar en liten stund för Docker att hämta avbildningen från Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="eea30-125">It'll take a few moments for Docker to pull down this image from Docker Hub.</span></span> <span data-ttu-id="eea30-126">Medan du väntar kan vi titta på vad kommandot gör.</span><span class="sxs-lookup"><span data-stu-id="eea30-126">While you wait, let's look at what the command does.</span></span>

    * <span data-ttu-id="eea30-127">Argumentet `--name` anger din containers namn, ”python-demo”.</span><span class="sxs-lookup"><span data-stu-id="eea30-127">The `--name` argument specifies your container's name, "python-demo".</span></span>
    * <span data-ttu-id="eea30-128">Argumenten `-i` och `-t` kombineras till ett argument, `-it`.</span><span class="sxs-lookup"><span data-stu-id="eea30-128">The `-i` and `-t` arguments are combined into one argument, `-it`.</span></span> <span data-ttu-id="eea30-129">Dessa argument skapar en interaktiv session med den container som körs.</span><span class="sxs-lookup"><span data-stu-id="eea30-129">These arguments create an interactive session with the running container.</span></span>
      * <span data-ttu-id="eea30-130">`-i` skapar en interaktiv session med containern.</span><span class="sxs-lookup"><span data-stu-id="eea30-130">`-i` creates an interactive session with the container.</span></span>
      * <span data-ttu-id="eea30-131">`-t` skapar en pseudoterminal som förblir i ett körningstillstånd.</span><span class="sxs-lookup"><span data-stu-id="eea30-131">`-t` creates a pseudo terminal that remains in a running state.</span></span>
    * <span data-ttu-id="eea30-132">`python` anger basavbildningen.</span><span class="sxs-lookup"><span data-stu-id="eea30-132">`python` specifies the base image.</span></span> <span data-ttu-id="eea30-133">Docker laddar ned den här avbildningen från Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="eea30-133">Docker downloads this image from Docker Hub.</span></span> <span data-ttu-id="eea30-134">Avbildningen är förkonfigurerad med en miljö för Python-programmering.</span><span class="sxs-lookup"><span data-stu-id="eea30-134">This image is preconfigued with an environment for Python programming.</span></span>
    * <span data-ttu-id="eea30-135">`bash` anger vilket program ska köras.</span><span class="sxs-lookup"><span data-stu-id="eea30-135">`bash` specifies the program to run.</span></span>

    <span data-ttu-id="eea30-136">Tänk på den här konfigurationen som en interaktiv miljö där du kan skriva Python-appar.</span><span class="sxs-lookup"><span data-stu-id="eea30-136">Think of this setup as an interactive environment for you to write Python apps.</span></span> <span data-ttu-id="eea30-137">När din app är i drift kan du skapa en avbildning, eller en ögonblicksbild, av din miljö.</span><span class="sxs-lookup"><span data-stu-id="eea30-137">Once you have your app up and running, you can capture an image, or snapshot, of your environment.</span></span> <span data-ttu-id="eea30-138">När du förbättrar din app kan du skapa ytterligare avbildningar åt dig eller ditt team.</span><span class="sxs-lookup"><span data-stu-id="eea30-138">As you improve your app, you can create additional images for you or your team.</span></span>

    <span data-ttu-id="eea30-139">Din terminalsession växlar över till containerns pseudoterminal.</span><span class="sxs-lookup"><span data-stu-id="eea30-139">Your terminal session switches to the container's pseudo terminal.</span></span> <span data-ttu-id="eea30-140">Frågan ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="eea30-140">Your prompt resembles this:</span></span>

    ```docker
    root@d8ccada9c61e:/#
    ```

1. <span data-ttu-id="eea30-141">Från Docker-sessionen kör du detta för att skapa en katalog med namnet `./app` och flytta till den.</span><span class="sxs-lookup"><span data-stu-id="eea30-141">From your Docker session, run this to create a directory named `./app` and move to it.</span></span>

    ```bash
    mkdir ./app && cd ./app
    ```

    <span data-ttu-id="eea30-142">Även om det inte krävs ger katalogen `./app` dig en plats där du lägga till Python-koden.</span><span class="sxs-lookup"><span data-stu-id="eea30-142">Although not required, the `./app` directory gives you a place to add your Python code.</span></span>

1. <span data-ttu-id="eea30-143">Kör detta om du vill skapa ett grundläggande Python-program.</span><span class="sxs-lookup"><span data-stu-id="eea30-143">Run this to create a very basic Python program.</span></span>

    ```bash
    echo 'print("Hello World!")' > hello.py
    ```

    <span data-ttu-id="eea30-144">Det här programmet skriver ut ”Hello World!”</span><span class="sxs-lookup"><span data-stu-id="eea30-144">This program prints "Hello World!"</span></span> <span data-ttu-id="eea30-145">på terminalen.</span><span class="sxs-lookup"><span data-stu-id="eea30-145">to the terminal.</span></span>

    > [!TIP]
    > <span data-ttu-id="eea30-146">I praktiken kan du _montera_ en katalog på din arbetsstation till en katalog i din container.</span><span class="sxs-lookup"><span data-stu-id="eea30-146">In practice, you can _mount_ a directory on your workstation to a directory on your container.</span></span> <span data-ttu-id="eea30-147">På så sätt kan du använda din favoritredigerare på din arbetsstation för att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="eea30-147">This enables you to use your favorite editor on your workstation to write code.</span></span> <span data-ttu-id="eea30-148">När du sparar filen visas filen även i din container.</span><span class="sxs-lookup"><span data-stu-id="eea30-148">When you save your file, that file also appears on your container.</span></span>

1. <span data-ttu-id="eea30-149">Kör följande för att testa ditt Python-program.</span><span class="sxs-lookup"><span data-stu-id="eea30-149">Run the following to try out your Python program.</span></span>

    ```bash
    python hello.py
    ```

    <span data-ttu-id="eea30-150">Du ser det här.</span><span class="sxs-lookup"><span data-stu-id="eea30-150">You see this.</span></span>

    ```output
    Hello World!
    ```

1. <span data-ttu-id="eea30-151">Avsluta den interaktiva sessionen.</span><span class="sxs-lookup"><span data-stu-id="eea30-151">Exit your interactive session.</span></span>

    ```bash
    exit
    ```

1. <span data-ttu-id="eea30-152">Från din Linux-VM kör du `docker ps` för att lista alla containrar som körs:</span><span class="sxs-lookup"><span data-stu-id="eea30-152">From your Linux VM, run `docker ps` to list all running containers:</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="eea30-153">Du ser att inget körs.</span><span class="sxs-lookup"><span data-stu-id="eea30-153">You see that nothing is running.</span></span> 

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```

    <span data-ttu-id="eea30-154">Det beror på att om containern avslutas så avslutas Bash-sessionen.</span><span class="sxs-lookup"><span data-stu-id="eea30-154">That's because exiting the container ends your Bash session.</span></span> <span data-ttu-id="eea30-155">När programmet eller tjänsten som containern kör avslutas, stoppar Docker containern.</span><span class="sxs-lookup"><span data-stu-id="eea30-155">When the application or service your container runs exits, Docker stops the container.</span></span>

1. <span data-ttu-id="eea30-156">Kör `docker ps` en andra gång och inkludera argumentet `-a`.</span><span class="sxs-lookup"><span data-stu-id="eea30-156">Run `docker ps` a second time and include the `-a` argument.</span></span> <span data-ttu-id="eea30-157">Det här kommandot returnerar alla containrar, som körs eller har stoppats.</span><span class="sxs-lookup"><span data-stu-id="eea30-157">This command returns all containers, running or stopped.</span></span>

    ```bash
    docker ps -a
    ```

    <span data-ttu-id="eea30-158">Du ser något som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="eea30-158">You see something like this:</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
    cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
    ```

   <span data-ttu-id="eea30-159">Din container, **python-demo**, har statusen **Avslutades**.</span><span class="sxs-lookup"><span data-stu-id="eea30-159">Your container, **python-demo**, has a status of **Exited**.</span></span>

1. <span data-ttu-id="eea30-160">Kör `docker commit` för att skapa en avbildning från din container.</span><span class="sxs-lookup"><span data-stu-id="eea30-160">Run `docker commit` to create an image from your container.</span></span>

   ```bash
   docker commit python-demo python-custom
   ```

   <span data-ttu-id="eea30-161">Här skapar Docker en avbildning med namnet **python-custom** från din container med namnet **python-demo**.</span><span class="sxs-lookup"><span data-stu-id="eea30-161">Here, Docker creates an image named **python-custom** from your container named **python-demo**.</span></span> <span data-ttu-id="eea30-162">Avbildningens namn kan vara vad som helst.</span><span class="sxs-lookup"><span data-stu-id="eea30-162">The image name can be whatever you want.</span></span>

1. <span data-ttu-id="eea30-163">Kör `docker images` för att lista dina avbildningar.</span><span class="sxs-lookup"><span data-stu-id="eea30-163">Run `docker images` to list your images.</span></span>

    ```bash
    docker images
    ```

    <span data-ttu-id="eea30-164">Du ser avbildningen **python-custom**.</span><span class="sxs-lookup"><span data-stu-id="eea30-164">You see the **python-custom** image.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-custom       latest              1f231e7127a1        6 seconds ago       922MB
    python              latest              638817465c7d        24 hours ago        922MB
    ```

1. <span data-ttu-id="eea30-165">Använd `docker run` för att testa avbildningen.</span><span class="sxs-lookup"><span data-stu-id="eea30-165">Use `docker run` to test out your image.</span></span>

    ```bash
    docker run python-custom python app/hello.py
    ```

    <span data-ttu-id="eea30-166">Kom ihåg att `docker run` tar kommandot för att köra det som sitt sista argument.</span><span class="sxs-lookup"><span data-stu-id="eea30-166">Recall that `docker run` takes the command to run as its final argument.</span></span> <span data-ttu-id="eea30-167">Här är kommandot `python app/hello.py`.</span><span class="sxs-lookup"><span data-stu-id="eea30-167">Here, the command is `python app/hello.py`.</span></span>

    <span data-ttu-id="eea30-168">Du ser det här.</span><span class="sxs-lookup"><span data-stu-id="eea30-168">You see this.</span></span>

    ```output
    Hello World!
    ```

    <span data-ttu-id="eea30-169">Eftersom du inte kör den här containern interaktivt, körs Python-programmet och containern stoppas.</span><span class="sxs-lookup"><span data-stu-id="eea30-169">Because you're not running this container interactively, the Python program runs and the container stops.</span></span>

## <a name="use-a-dockerfile-to-create-an-image-automatically"></a><span data-ttu-id="eea30-170">Använda en Dockerfile för att skapa en avbildning automatiskt</span><span class="sxs-lookup"><span data-stu-id="eea30-170">Use a Dockerfile to create an image automatically</span></span>

<span data-ttu-id="eea30-171">Här ska du använda en mer automatiserad process för att skapa samma Docker-avbildning som kör ditt Python-program.</span><span class="sxs-lookup"><span data-stu-id="eea30-171">Here you'll use a more automated process to create the same Docker image that runs your Python program.</span></span>

<span data-ttu-id="eea30-172">Att skapa en avbildning manuellt är ett bra sätt att experimentera och utforska nya funktioner.</span><span class="sxs-lookup"><span data-stu-id="eea30-172">Building an image manually is a great way to experiment and explore new features.</span></span> <span data-ttu-id="eea30-173">Men anta att du vill dela processen med ditt team eller göra den upprepningsbar.</span><span class="sxs-lookup"><span data-stu-id="eea30-173">But say you want to share the process with your team or make it repeatable.</span></span> <span data-ttu-id="eea30-174">Anta exempelvis att du vill skapa en ny avbildning varje kväll som kör den senaste versionen av programvaran som ditt team bygger upp.</span><span class="sxs-lookup"><span data-stu-id="eea30-174">For example, say you want to create a new image each evening that runs the latest version of the software your team is building.</span></span>

<span data-ttu-id="eea30-175">Det är där Dockerfile kommer in.</span><span class="sxs-lookup"><span data-stu-id="eea30-175">That's where the Dockerfile comes in.</span></span> <span data-ttu-id="eea30-176">Tänk på en Dockerfile som ett sätt att beskriva instruktioner som du annars skulle utföra manuellt.</span><span class="sxs-lookup"><span data-stu-id="eea30-176">Think of a Dockerfile as a way to describe instructions you would otherwise do manually.</span></span>

1. <span data-ttu-id="eea30-177">Kör detta kommando från din virtuella Linux-dator för att skapa en Dockerfile som kör ditt Python-program.</span><span class="sxs-lookup"><span data-stu-id="eea30-177">From your Linux VM, run this command to create a Dockerfile that runs your Python program.</span></span>

    ```bash
    cat << EOF > Dockerfile
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    EOF
    ```

    <span data-ttu-id="eea30-178">Det här exemplet är ett enkelt sätt för dig att skapa filen.</span><span class="sxs-lookup"><span data-stu-id="eea30-178">This example is just an easy way for you to create the file.</span></span> <span data-ttu-id="eea30-179">I vanliga fall skulle du använda valfri kodredigerare för att skapa den.</span><span class="sxs-lookup"><span data-stu-id="eea30-179">In practice, you would use your favorite code editor to create it.</span></span>

    <span data-ttu-id="eea30-180">Skriv ut din Dockerfile till konsolen.</span><span class="sxs-lookup"><span data-stu-id="eea30-180">Print your Dockerfile to the console.</span></span>

    ```bash
    cat Dockerfile
    ```

    <span data-ttu-id="eea30-181">Du ser det här.</span><span class="sxs-lookup"><span data-stu-id="eea30-181">You see this.</span></span>

    ```output
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    ```

    <span data-ttu-id="eea30-182">Lås oss ta en titt på vad en Dockerfile gör.</span><span class="sxs-lookup"><span data-stu-id="eea30-182">Let's break down what the Dockerfile does.</span></span>

    * <span data-ttu-id="eea30-183">`FROM` anger den överordnade avbildningen som du baserar den här avbildningen på.</span><span class="sxs-lookup"><span data-stu-id="eea30-183">`FROM` specifies the parent image to base this image on.</span></span> <span data-ttu-id="eea30-184">Här är den överordnade avbildningen **python**.</span><span class="sxs-lookup"><span data-stu-id="eea30-184">Here, the parent image is **python**.</span></span>
    * <span data-ttu-id="eea30-185">`WORKDIR` anger arbetskatalogen för `RUN`- och `CMD`-instruktioner som visas senare i Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="eea30-185">`WORKDIR` specifies the working directory for any `RUN` and `CMD` statements that appear later in the Dockerfile.</span></span> <span data-ttu-id="eea30-186">Docker skapar den här katalogen åt dig, i det här fallet `./app`.</span><span class="sxs-lookup"><span data-stu-id="eea30-186">Docker creates this directory for you, in this case, `./app`.</span></span>
    * <span data-ttu-id="eea30-187">`RUN` anger ett kommando som ska köras i containern.</span><span class="sxs-lookup"><span data-stu-id="eea30-187">`RUN` specifies a command to run in the container.</span></span> <span data-ttu-id="eea30-188">`RUN` kan användas för att installera programvara, göra ändringar i konfigurationen och rensa containern före överföringen.</span><span class="sxs-lookup"><span data-stu-id="eea30-188">You can use `RUN` to install software, make configuration changes, and cleanup the container before it's captured.</span></span> <span data-ttu-id="eea30-189">Här ska vi skriva ett grundläggande Python-program till `./app/hello.py`.</span><span class="sxs-lookup"><span data-stu-id="eea30-189">Here, we write a basic Python program to `./app/hello.py`.</span></span>
    * <span data-ttu-id="eea30-190">`CMD` anger processen som ska köras när containern startas.</span><span class="sxs-lookup"><span data-stu-id="eea30-190">`CMD` specifies the process to run when the container starts.</span></span> <span data-ttu-id="eea30-191">Här kör vi `python hello.py` från katalogen `/.app`.</span><span class="sxs-lookup"><span data-stu-id="eea30-191">Here, we run `python hello.py` from the `/.app` directory.</span></span>

    <span data-ttu-id="eea30-192">Du har antagligen lagt märke till att dessa steg är nästan exakt desamma som du använde för att skapa avbildningen manuellt.</span><span class="sxs-lookup"><span data-stu-id="eea30-192">You've probably noticed that these are nearly the exact same steps you took to build the image manually.</span></span> <span data-ttu-id="eea30-193">Genom att ange de här stegen som kod blir processen enklare att dela och upprepa.</span><span class="sxs-lookup"><span data-stu-id="eea30-193">Expressing these steps as code makes the process easier to share and repeat.</span></span>

1. <span data-ttu-id="eea30-194">Kör `docker build` för att skapa avbildningen med hjälp av de instruktioner som anges i Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="eea30-194">Run `docker build` to create the image, using the instructions specified in the Dockerfile.</span></span>

    ```bash
    docker build -t python-dockerfile .
    ```

    <span data-ttu-id="eea30-195">Det tar vanligtvis bara några sekunder för Docker för att skapa din avbildning.</span><span class="sxs-lookup"><span data-stu-id="eea30-195">It will typically take only a few seconds for Docker to build your image.</span></span>

    <span data-ttu-id="eea30-196">Du bör se utdata enligt följande.</span><span class="sxs-lookup"><span data-stu-id="eea30-196">You see output like the following.</span></span>

    ```output
    Sending build context to Docker daemon  2.048kB
    Step 1/4 : FROM python
     ---> 638817465c7d
    Step 2/4 : WORKDIR ./app
     ---> Running in 990d17e86466
    Removing intermediate container 990d17e86466
     ---> 59a074a092cc
    Step 3/4 : RUN echo 'print("Hello World!")' > hello.py
     ---> Running in aed707c53bc5
    Removing intermediate container aed707c53bc5
     ---> d7f55a9d0e85
    Step 4/4 : CMD python hello.py
     ---> Running in e87ec55a8d36
    Removing intermediate container e87ec55a8d36
     ---> 98c39b91770f
    Successfully built 98c39b91770f
    Successfully tagged python-dockerfile:latest
    ```

1. <span data-ttu-id="eea30-197">Använd `docker images` för att lista dina avbildningar.</span><span class="sxs-lookup"><span data-stu-id="eea30-197">Use the `docker images` to list your images.</span></span>

    ```bash
    docker images
    ```

    <span data-ttu-id="eea30-198">Du ser avbildningen **python-dockerfile** som du precis skapat.</span><span class="sxs-lookup"><span data-stu-id="eea30-198">You see the **python-dockerfile** image you just built.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-dockerfile   latest              8ed5f85c5e5b        24 seconds ago      923MB
    python-custom       latest              6789cbe2cceb        4 minutes ago       923MB
    python              latest              a9d071760c82        2 weeks ago         923MB
    ```

1. <span data-ttu-id="eea30-199">Använd `docker run` för att testa containern.</span><span class="sxs-lookup"><span data-stu-id="eea30-199">Use `docker run` to test out your container.</span></span>

    ```bash
    docker run python-dockerfile
    ```

    <span data-ttu-id="eea30-200">Du ser det här.</span><span class="sxs-lookup"><span data-stu-id="eea30-200">You see this.</span></span>

    ```output
    Hello World!
    ```

    <span data-ttu-id="eea30-201">Till skillnad från när du testade avbildningen du skapade manuellt anger du inte här kommandot som ska köras, `python app/hello.py`.</span><span class="sxs-lookup"><span data-stu-id="eea30-201">Unlike when you tested the image you created manually, here you don't specify the command to run, `python app/hello.py`.</span></span> <span data-ttu-id="eea30-202">Det anges i din Dockerfile, så att kommandot byggs in i avbildningen.</span><span class="sxs-lookup"><span data-stu-id="eea30-202">Your Dockerfile specifies that, so the command is built into the image.</span></span>

## <a name="publish-your-image-to-docker-hub"></a><span data-ttu-id="eea30-203">Publicera avbildningen till Docker Hub</span><span class="sxs-lookup"><span data-stu-id="eea30-203">Publish your image to Docker Hub</span></span>

<span data-ttu-id="eea30-204">När du ska publicera en avbildning till Docker Hub behöver du ett konto.</span><span class="sxs-lookup"><span data-stu-id="eea30-204">To publish an image to Docker Hub, you need an account.</span></span> <span data-ttu-id="eea30-205">Här kan du konfigurera ett Docker Hub-konto och publicera din **python-dockerfile**-avbildning till Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="eea30-205">Here you'll set up your Docker Hub account and publish your **python-dockerfile** image to Docker Hub.</span></span>

1. <span data-ttu-id="eea30-206">[Skapa ditt Docker Hub-konto](https://hub.docker.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="eea30-206">[Create your Docker Hub account](https://hub.docker.com?azure-portal=true).</span></span>

1. <span data-ttu-id="eea30-207">Exportera namnet på ditt Docker Hub-konto som en miljövariabel.</span><span class="sxs-lookup"><span data-stu-id="eea30-207">Export your Docker Hub account name as an environment variable.</span></span> <span data-ttu-id="eea30-208">Ersätt **account-name** med ditt kontonamn.</span><span class="sxs-lookup"><span data-stu-id="eea30-208">Replace **account-name** with your account name.</span></span>

    ```bash
    export docker_account=account-name
    ```

    <span data-ttu-id="eea30-209">Den här miljövariabeln gör det enklare för dig att köra kommandona som följer.</span><span class="sxs-lookup"><span data-stu-id="eea30-209">This environment variable will make it easier for you to run the commands that follow.</span></span>

1. <span data-ttu-id="eea30-210">Kör `docker tag` för att tagga avbildningen med ditt Docker Hub-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="eea30-210">Run `docker tag` to tag your image with your Docker Hub account name.</span></span>

    ```bash
    docker tag python-dockerfile $docker_account/python-dockerfile
    ```

1. <span data-ttu-id="eea30-211">Kör `docker login` för att logga in på Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="eea30-211">Run `docker login` to log in to Docker Hub.</span></span>

    ```bash
    docker login
    ```

    <span data-ttu-id="eea30-212">Ange användarnamn och lösenord när du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="eea30-212">When prompted, enter your username and password.</span></span>

1. <span data-ttu-id="eea30-213">Kör `docker push` för att push-överföra, eller ladda upp, din **python-dockerfile**-avbildning till Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="eea30-213">Run `docker push` to push, or upload, your **python-dockerfile** image to Docker Hub.</span></span>

    ```bash
    docker push $docker_account/python-dockerfile
    ```

    <span data-ttu-id="eea30-214">Du ser utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="eea30-214">You see output similar to the following:</span></span>

    ```output
    The push refers to repository [docker.io/account/python-dockerfile]
    f39073ca4d5a: Pushed
    9dfcec2738a9: Pushed
    ffab8273c674: Mounted from account/python
    e6f6cbe5e14e: Mounted from account/python
    3eb255f8a6ac: Mounted from account/python
    b860b6c48eec: Mounted from account/python
    1fa8778eb779: Mounted from account/python
    fa0c3f992cbd: Mounted from account/python
    ce6466f43b11: Mounted from account/python
    719d45669b35: Mounted from account/python
    3b10514a95be: Mounted from account/python
    latest: digest: sha256:b816c32382d06ac74530d56f2a7b83000c86174218f430c6bb5039fdcfba38aa size: 2631
    ```

    <span data-ttu-id="eea30-215">Docker-avbildningen lagras nu på Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="eea30-215">Your Docker image is now stored in Docker Hub.</span></span> <span data-ttu-id="eea30-216">Du kan använda `docker pull` eller `docker run` från en annan dator för att ladda ned eller köra avbildningen från Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="eea30-216">You can use `docker pull` or `docker run` from another computer to download or run your image from Docker Hub.</span></span> <span data-ttu-id="eea30-217">Här följer två exempel:</span><span class="sxs-lookup"><span data-stu-id="eea30-217">Here are two examples:</span></span>

    1. <span data-ttu-id="eea30-218">Med det här exemplet hämtas den senaste avbildningen från Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="eea30-218">This example pulls the latest image from Docker Hub.</span></span>

        ```bash
        docker pull $docker_account/python-dockerfile
        ```

    1. <span data-ttu-id="eea30-219">Med det här exemplet körs containern.</span><span class="sxs-lookup"><span data-stu-id="eea30-219">This example runs the container.</span></span>

        ```bash
        docker run $docker_account/python-dockerfile
        ```

1. <span data-ttu-id="eea30-220">Testa din container.</span><span class="sxs-lookup"><span data-stu-id="eea30-220">Test out your container.</span></span>

    <span data-ttu-id="eea30-221">Gå till [ditt Docker Hub-konto](https://hub.docker.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="eea30-221">Navigate to [your Docker Hub account](https://hub.docker.com?azure-portal=true).</span></span> <span data-ttu-id="eea30-222">Du ser Docker-avbildningen på fliken **Centrallager**.</span><span class="sxs-lookup"><span data-stu-id="eea30-222">From the **Repositories** tab, you see your Docker image.</span></span>

    ![Docker-avbildningen på Docker Hub](../media/4-docker-hub-python-dockerfile.png)
