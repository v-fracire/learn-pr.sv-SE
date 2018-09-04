<span data-ttu-id="0ed2c-101">I den föregående enheten arbetade du med fördefinierade containeravbildningar för att utföra vissa grundläggande Docker-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-101">In the last unit, you worked with pre-created container images to perform some basic Docker operations.</span></span> <span data-ttu-id="0ed2c-102">I den här enheten skapar du anpassade containeravbildningar, överför dessa avbildningar till ett offentligt containerregister och kör containrarna från avbildningarna.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-102">In this unit, you will create custom container images, push these images to a public container registry, and run containers from these images.</span></span>

<span data-ttu-id="0ed2c-103">Containeravbildningar kan skapas manuellt eller med hjälp av det som kallas en Dockerfile för att automatisera processen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-103">Container images can be created by hand or using what's called a Dockerfile to automate the process.</span></span> <span data-ttu-id="0ed2c-104">Den föredragna metoden är att använda en Dockerfile, men den här enheten visar båda metoderna.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-104">The preferred method is using a Dockerfile, but this unit will demonstrate both methods.</span></span> <span data-ttu-id="0ed2c-105">Avsikten är att en förståelse för den manuella processen hjälper dig att bättre förstå det som händer när du använder en Dockerfile för automatisering.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-105">The intention is that having an understanding of the manual process will help you to better understand what's occurring when using a Dockerfile for automation.</span></span>

## <a name="manual-image-creation"></a><span data-ttu-id="0ed2c-106">Skapa avbildning manuellt</span><span class="sxs-lookup"><span data-stu-id="0ed2c-106">Manual image creation</span></span>

<span data-ttu-id="0ed2c-107">När du skapar en containeravbildning manuellt vidtas följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-107">When manually creating a container image, the following actions are taken:</span></span>

- <span data-ttu-id="0ed2c-108">Starta en instans av en container.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-108">Start an instance of a container.</span></span>
- <span data-ttu-id="0ed2c-109">Upprätta en terminalsession med containern.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-109">Establish a terminal session with the container.</span></span>
- <span data-ttu-id="0ed2c-110">Ändra containern genom att installera programvara och göra ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-110">Modify the container by installing software and making configuration changes.</span></span>
- <span data-ttu-id="0ed2c-111">Spara containern till en ny avbildning med kommandot `docker capture`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-111">Capturing the container into a new image using the `docker capture` command.</span></span>

<span data-ttu-id="0ed2c-112">I det första exemplet startar du en instans av en container som kör Python, skapar ett hello world-program och sparar sedan containern till en ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-112">In this first example, you start an instance of a container that's running Python, create a hello world application, and then capture the container to a new image.</span></span>

<span data-ttu-id="0ed2c-113">Kör först en container från NGINX-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-113">First, run a container from the NGINX image.</span></span> <span data-ttu-id="0ed2c-114">Det här kommandot lite ser annorlunda ut än de kommandon som du körde i den föregående enheten.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-114">This command looks a bit different from the commands that you ran in the previous unit.</span></span> <span data-ttu-id="0ed2c-115">Eftersom du ska upprätta en terminalsession med den container som körs anges argumenten `-t` och `-i`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-115">Because you want to establish a terminal session with the running container, the `-t` and `-i` arguments are provided.</span></span> <span data-ttu-id="0ed2c-116">Tillsammans instruerar de här argumenten Docker att allokera en pseudoterminal som kommer att finnas kvar i ett körningstillstånd.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-116">Together, these arguments instruct Docker to allocate a pseudo terminal that will remain in a runnings state.</span></span> <span data-ttu-id="0ed2c-117">Med andra ord skapar argumenten `-t` och `-i` en interaktiv session med den container som körs.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-117">In other words, the `-t` and `-i` arguments create an interactive session with the running container.</span></span>

<span data-ttu-id="0ed2c-118">Du anger sedan att `python`-containeravbildningen används och att den process som ska köras inuti containern är `bash`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-118">You then specify that the `python` container image is used, and the process to run inside of the container is `bash`.</span></span>

```bash
docker run --name python-demo -ti python bash
```

<span data-ttu-id="0ed2c-119">När kommandot har körts bör terminalsessionen växla till containerpseudoterminalen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-119">After the command is run, your terminal session should switch to the containers pseudo terminal.</span></span> <span data-ttu-id="0ed2c-120">Detta kan ses utifrån terminalfönstret, som bör ha ändrats till något som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-120">This can be seen by the terminal prompt, which should have changed to something similar to the following:</span></span>

```bash
root@d8ccada9c61e:/#
```

<span data-ttu-id="0ed2c-121">I det här skedet arbetar du inuti container.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-121">At this point, you're working inside the container.</span></span> <span data-ttu-id="0ed2c-122">Arbete i en container påminner om arbete i ett virtuellt eller ett fysiskt system.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-122">You should find that working inside a container is much like working inside a virtual or physical system.</span></span> <span data-ttu-id="0ed2c-123">Exempelvis kan du lista, skapa och ta bort filer, installera programvara och göra ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-123">For instance, you can list, create, and delete files, install software, and make configuration changes.</span></span> <span data-ttu-id="0ed2c-124">För det här enkla exemplet skapas ett Python-baserat hello world-skript.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-124">For this simple example, a Python-based hello world script is created.</span></span> <span data-ttu-id="0ed2c-125">Detta kan göras med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-125">This can be done with the following command:</span></span>

```bash
echo 'print("Hello World!")' > hello.py
```

<span data-ttu-id="0ed2c-126">Testa skriptet medan du fortfarande är i containern genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-126">To test the script while you're still in the container, run the following command:</span></span>

```bash
python hello.py
```

<span data-ttu-id="0ed2c-127">Detta genererar följande utdata:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-127">This will produce the following output:</span></span>

```bash
Hello World!
```

<span data-ttu-id="0ed2c-128">När du har bekräftat att skriptet fungerar som förväntat stänger du containern genom att skriva `exit`:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-128">When you're satisfied that the script functions as expected, exit out of the container by typing `exit`:</span></span>

```bash
exit
```

<span data-ttu-id="0ed2c-129">När du är tillbaka i terminalen för det lokala systemet använder du kommandot `docker ps` för att lista alla containrar som körs:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-129">Back in the terminal of your local system, use the `docker ps` command to list all running containers:</span></span>

```bash
docker ps
```

<span data-ttu-id="0ed2c-130">Observera att inget körs.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-130">Notice that nothing is running.</span></span> <span data-ttu-id="0ed2c-131">När du angav `exit` i den container som körs slutfördes Bash-processen, vilket sedan stoppade containern.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-131">When you entered `exit` in the running container, the Bash process completed, which then stopped the container.</span></span> <span data-ttu-id="0ed2c-132">Detta är förväntat beteende och är ok.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-132">This is the expected behavior and is ok.</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

<span data-ttu-id="0ed2c-133">Använd `docker ps` igen och inkludera argumentet `-a`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-133">Use `docker ps` again and include the `-a` argument.</span></span> <span data-ttu-id="0ed2c-134">Det här kommandot returnerar en lista över alla containrar, oavsett om de körs.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-134">This command will return a list of all containers, regardless if they're running.</span></span>

```bash
docker ps -a
```

<span data-ttu-id="0ed2c-135">Observera att en container med namnet *python-demo* har statusen *Exited* (Stängd).</span><span class="sxs-lookup"><span data-stu-id="0ed2c-135">Notice that a container with the name *python-demo* has a status of *Exited*.</span></span> <span data-ttu-id="0ed2c-136">Den här containern är den stoppade instansen av den container som du precis stängde.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-136">This container is the stopped instance of the container that you just exited from.</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
```

<span data-ttu-id="0ed2c-137">Du kan skapa en ny containeravbildning från den här containern med kommandot `docker commit`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-137">To create a new container image from this container, use the `docker commit` command.</span></span> <span data-ttu-id="0ed2c-138">I följande exempel instruerar *docker commit* att en avbildning med namnet *python-customn* ska skapas från *python-demo*-containrarna.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-138">The following example instructs *docker commit* to create an image named *python-custom* from the *python-demo* containers.</span></span>

```bash
docker commit python-demo python-custom
```

<span data-ttu-id="0ed2c-139">När kommandot är klart bör du se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-139">After the command completes, you should see output similar to the following:</span></span>

```bash
sha256:91a0cf9aa9857bebcd7ebec3418970f97f043e31987fd4a257c8ac8c8418dc38
```

<span data-ttu-id="0ed2c-140">Kör nu `docker images` för att se en lista över containerbildningar:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-140">Now run `docker images` to see a list of container images:</span></span>

```bash
docker images
```

<span data-ttu-id="0ed2c-141">Du bör nu se den anpassade Python-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-141">You should now see the custom Python image.</span></span>

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python-custom       latest              1f231e7127a1        6 seconds ago       922MB
python              latest              638817465c7d        24 hours ago        922MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

<span data-ttu-id="0ed2c-142">Kör en container från den nya avbildningen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-142">Run a container from the new image.</span></span> <span data-ttu-id="0ed2c-143">Du behöver även ange vilket kommando eller vilken process som ska köras inuti containern.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-143">You also need to specify what command or process to run inside of the container.</span></span> <span data-ttu-id="0ed2c-144">Med det här exemplet kör du `python hello.py`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-144">With this example, run `python hello.py`.</span></span>


```bash
docker run python-custom python hello.py
```

<span data-ttu-id="0ed2c-145">Containern startar och skickar hello world-meddelandet som utdata.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-145">The container will start and output the hello world message.</span></span> <span data-ttu-id="0ed2c-146">Python-processen är sedan klar, och containern stoppas.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-146">The Python process is then complete and the container stops.</span></span>

```bash
Hello World!
```

## <a name="automated-image-creation"></a><span data-ttu-id="0ed2c-147">Skapa en avbildning automatiskt</span><span class="sxs-lookup"><span data-stu-id="0ed2c-147">Automated image creation</span></span>

<span data-ttu-id="0ed2c-148">I föregående avsnitt skapades en containeravbildning manuellt.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-148">In the last section, a container image was manually created.</span></span> <span data-ttu-id="0ed2c-149">Den här metoden fungerar bra för att skapa enskilda eller experimentella avbildningar, men den är inte hållbar i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-149">While this method works great for one-off or experiential image creation, it's not sustainable in a production environment.</span></span> <span data-ttu-id="0ed2c-150">Det går att automatisera skapandet av containeravbildningar med hjälp av en *Dockerfile* och kommandot `docker build`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-150">Container image creation can be automated using a *Dockerfile* and the `docker build` command.</span></span> <span data-ttu-id="0ed2c-151">Kommandot *docker build* startar i princip en container, kör de instruktioner som finns i *Dockerfile* och sparar sedan resultaten till en ny containeravbildning.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-151">The *docker build* command essentially starts a container, runs the instructions found in the *Dockerfile*, and then captures the results to a new container image.</span></span>

<span data-ttu-id="0ed2c-152">Skapa en fil med namnet *Dockerfile* och ange följande text:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-152">Create a file named *Dockerfile* and enter the following text:</span></span>

```bash
FROM python

WORKDIR ./app

RUN echo 'print("Hello World!")' > hello.py

CMD python hello.py
```

<span data-ttu-id="0ed2c-153">Instruktionen *FROM* anger den basavbildning som ska användas när avbildningar skapas.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-153">The *FROM* statement specifies the base image to be used during image creations.</span></span> <span data-ttu-id="0ed2c-154">Instruktionen *RUN* kör kommandon inuti containern.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-154">The *RUN* statement runs commands inside of the container.</span></span> <span data-ttu-id="0ed2c-155">*RUN* kan användas för att installera programvara, göra ändringar i konfigurationen och rensa containern före överföringshändelsen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-155">*RUN* can be used to install software, make configuration changes, and clean up the container before the capture event.</span></span> <span data-ttu-id="0ed2c-156">Instruktionen *CMD* anger den process som ska köras när en container startas.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-156">The *CMD* statement specifies the process that should run when a container is started.</span></span>

<span data-ttu-id="0ed2c-157">Använd kommandot `docker build` för att skapa en ny containeravbildning med hjälp av de instruktioner som anges i Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-157">Use the `docker build` command to create a new container image using the instructions specified in the Dockerfile.</span></span>

```bash
docker build -t python-dockerfile .
```

<span data-ttu-id="0ed2c-158">Du bör se utdata som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-158">You should see output similar to the following.</span></span>

```bash
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

<span data-ttu-id="0ed2c-159">Använd kommandot `docker images` för att returnera en lista över containeravbildningar.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-159">Use the `docker images` command to return a list of container images.</span></span>

```bash
docker images
```

<span data-ttu-id="0ed2c-160">Du bör nu se den anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-160">You should now see the custom image.</span></span>

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
python-dockerfile   latest              98c39b91770f        About a minute ago   922MB
python              latest              638817465c7d        26 hours ago         922MB
alpine              latest              11cd0b38bc3c        2 weeks ago          4.41MB
```

<span data-ttu-id="0ed2c-161">Använd kommandot `docker run` för att köra en container från den anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-161">Use the `docker run` command to run a container from the custom image.</span></span>

<span data-ttu-id="0ed2c-162">Lägg märkte till att det inte har angetts några argument till kommandot `docker run`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-162">Notice here that no arguments have been provided to the `docker run` command.</span></span> <span data-ttu-id="0ed2c-163">Till skillnad från när du manuellt skapar en containeravbildning gör Dockerfile att du kan inkludera ett kommando som körs när containern startas.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-163">Unlike when you manually create a container image, a Dockerfile allows you to include a command to run when the container starts.</span></span> <span data-ttu-id="0ed2c-164">I det här fallet är det angivna kommandot `python hello.py`, vilket gör att containern kör Python-skriptet.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-164">In this case, the specified command is `python hello.py`, which causes the container to run the Python script.</span></span>

```bash
docker run python-dockerfile
```

<span data-ttu-id="0ed2c-165">När du har kört kommandot bör du se containerutdata.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-165">After you run the command, you should see the container output.</span></span>

```bash
Hello World!
```

## <a name="push-the-image-to-a-public-registry"></a><span data-ttu-id="0ed2c-166">Överföra avbildningen till ett offentligt register</span><span class="sxs-lookup"><span data-stu-id="0ed2c-166">Push the image to a public registry</span></span>

<span data-ttu-id="0ed2c-167">Docker Hub är ett offentligt containerregister.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-167">Docker Hub is a public container registry.</span></span> <span data-ttu-id="0ed2c-168">För att kunna överföra containeravbildningar till Docker Hub måste du ha ett Docker-konto.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-168">In order to push container images to Docker Hub, you must have a Docker account.</span></span> <span data-ttu-id="0ed2c-169">Vid behov kan du besöka [Docker Hub-webbplatsen](https://hub.docker.com/) för att registrera ett konto.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-169">If needed, visit the [Docker Hub site](https://hub.docker.com/) to register for an account.</span></span>

<span data-ttu-id="0ed2c-170">När du har ett Docker Hub-konto måste containeravbildningen taggas med kontonamnet.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-170">After you have a Docker Hub account, the container image must be tagged with the account name.</span></span> <span data-ttu-id="0ed2c-171">Det gör du med kommandot `docker tag`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-171">To do so, use the `docker tag` command.</span></span>

<span data-ttu-id="0ed2c-172">I följande exempel är *python-dockerfile*-avbildningen taggad med ett Docker Hub-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-172">In the following example, the *python-dockerfile* image is tagged with a Docker Hub account name.</span></span> <span data-ttu-id="0ed2c-173">Ersätt `<account name>` med ditt Docker Hub-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-173">Replace `<account name>` with your Docker Hub account name.</span></span>

```bash
docker tag python-dockerfile <account name>/python-dockerfile
```

<span data-ttu-id="0ed2c-174">Därefter kontrollerar du att du är inloggad i Docker Hub med hjälp av kommandot `docker login`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-174">Next, make sure that you are logged into Docker Hub using the `docker login` command.</span></span>

```bash
docker login
```

<span data-ttu-id="0ed2c-175">Slutligen överför *python-dockerfile*-avbildningen till Docker Hub med kommandot `docker push`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-175">Finally push the *python-dockerfile* image to Docker Hub using the `docker push` command.</span></span> <span data-ttu-id="0ed2c-176">Ersätt `<account name>` med ditt Docker Hub-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-176">Replace `<account name>` with your Docker Hub account name.</span></span>

```bash
docker push <account name>/python-dockerfile
```

<span data-ttu-id="0ed2c-177">Medan containeravbildningen överförs till Docker Hub visas utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="0ed2c-177">While the container image is being uploaded to Docker Hub, you will see output similar to the following:</span></span>

```bash
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
```

<span data-ttu-id="0ed2c-178">Containeravbildningen lagras nu på Docker Hub och kan nås från valfri Internetansluten dator med hjälp av `docker pull` eller `docker run`.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-178">The container image is now stored in Docker Hub and can be accessed from any internet-connected machine using `docker pull` or `docker run`.</span></span>

## <a name="summary"></a><span data-ttu-id="0ed2c-179">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0ed2c-179">Summary</span></span>

<span data-ttu-id="0ed2c-180">I den här enheten skapade du två containeravbildningar.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-180">In this unit, you created two container images.</span></span> <span data-ttu-id="0ed2c-181">Den första skapades manuellt, och den andra automatiserades med hjälp av en Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="0ed2c-181">The first was created manually and the second was automated using a Dockerfile.</span></span>
