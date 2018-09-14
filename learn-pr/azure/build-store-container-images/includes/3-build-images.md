<span data-ttu-id="b87b6-101">Ditt företag använder sig av containeravbildningar för att hantera compute-arbetsbelastningar i företaget.</span><span class="sxs-lookup"><span data-stu-id="b87b6-101">Your company makes use of container images to manage compute workloads in the company.</span></span> <span data-ttu-id="b87b6-102">Du kan använda lokala Docker-verktyg för att bygga dina containeravbildningar.</span><span class="sxs-lookup"><span data-stu-id="b87b6-102">You use local Docker tooling to build your container images.</span></span> <span data-ttu-id="b87b6-103">Beslutet att använda ett Azure Container Registry låter dig nu skapa containeravbildningar i molnet.</span><span class="sxs-lookup"><span data-stu-id="b87b6-103">The decision to make use of an Azure Container Registry now allows you to build container images in the cloud.</span></span> 

<span data-ttu-id="b87b6-104">Du kan nu använda Azure Container Registry Build för att skapa dessa containers.</span><span class="sxs-lookup"><span data-stu-id="b87b6-104">You can now use Azure Container Registry Build to build these containers.</span></span> <span data-ttu-id="b87b6-105">I Container Registry Build kan du även integrera DevOps-processer med automatiserad kompilering när källkod checkas in.</span><span class="sxs-lookup"><span data-stu-id="b87b6-105">Container Registry Build also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="b87b6-106">Låt oss automatisera skapandet av en containeravbildning med Azure Container Registry Build.</span><span class="sxs-lookup"><span data-stu-id="b87b6-106">Let's automate the creation of a container image using Azure Container Registry Build.</span></span>

## <a name="create-a-container-image-with-azure-container-registry-build"></a><span data-ttu-id="b87b6-107">Skapa en containeravbildning med Azure Container Registry Build</span><span class="sxs-lookup"><span data-stu-id="b87b6-107">Create a container image with Azure Container Registry Build</span></span>

<span data-ttu-id="b87b6-108">Du kan använda en standard Docker-fil för att ge kompileringsinstruktioner.</span><span class="sxs-lookup"><span data-stu-id="b87b6-108">You use a standard Dockerfile to provide build instructions.</span></span> <span data-ttu-id="b87b6-109">Azure Container Registry Build låter dig återanvända valfri Docker-fil som finns i din miljö, inklusive kompileringar i flera steg.</span><span class="sxs-lookup"><span data-stu-id="b87b6-109">Azure Container Registry Build allows you to reuse any Dockerfile currently in your environment, including multi-staged builds.</span></span>

<span data-ttu-id="b87b6-110">Vi anvädner oss av en ny Docker-fil i vårt exempel.</span><span class="sxs-lookup"><span data-stu-id="b87b6-110">We'll use new Dockerfile for our example.</span></span> 

<span data-ttu-id="b87b6-111">Det första steget är att skapa en ny fil med namnet `Dockerfile`.</span><span class="sxs-lookup"><span data-stu-id="b87b6-111">The first step is to create a new file named `Dockerfile`.</span></span> <span data-ttu-id="b87b6-112">Du kan använda valfri textredigerare för att redigera filen.</span><span class="sxs-lookup"><span data-stu-id="b87b6-112">You can use any text editor to edit the file.</span></span> <span data-ttu-id="b87b6-113">Vi använder Visual Studio Code för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="b87b6-113">We'll use Visual Studio Code for this example.</span></span>

```bash
code Dockerfile
```

<span data-ttu-id="b87b6-114">Kopiera följande innehåll till din nya Docker-fil.</span><span class="sxs-lookup"><span data-stu-id="b87b6-114">Copy the following contents to your new Dockerfile.</span></span> <span data-ttu-id="b87b6-115">Se till att säkra filen.</span><span class="sxs-lookup"><span data-stu-id="b87b6-115">Make sure to safe the file.</span></span> 

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="b87b6-116">Den här konfigurationen lägger till ett Node.js-program till avbildningen `node:9-alpine`.</span><span class="sxs-lookup"><span data-stu-id="b87b6-116">This configuration adds a Node.js application to the `node:9-alpine` image.</span></span> <span data-ttu-id="b87b6-117">Sedan konfigureras containern för att leverera programmet på port 80 via instruktionen *EXPONERA*.</span><span class="sxs-lookup"><span data-stu-id="b87b6-117">Then configures the container to serve the application on port 80 via the *EXPOSE* instruction.</span></span>

<span data-ttu-id="b87b6-118">Kör nu Azure CLI-kommandot `az acr build` för att skapa containeravbildningen från Docker-filen.</span><span class="sxs-lookup"><span data-stu-id="b87b6-118">Now run the Azure CLI command, `az acr build`, to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

<span data-ttu-id="b87b6-119">När det här kommandot körs ser du att avbildningen skapas och push-överförs till ditt containerregister.</span><span class="sxs-lookup"><span data-stu-id="b87b6-119">You'll see the image being built and pushed to your Container Registry as you run the command.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="b87b6-120">Verifiera avbildningen</span><span class="sxs-lookup"><span data-stu-id="b87b6-120">Verify the image</span></span>

<span data-ttu-id="b87b6-121">Kör följande kommando för att verifiera att avbildningen har skapats och lagrats i registret.</span><span class="sxs-lookup"><span data-stu-id="b87b6-121">Run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="b87b6-122">Utdata bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="b87b6-122">The output should look similar to the following:</span></span>

```console
Result
-------------
helloacrbuild
```

<span data-ttu-id="b87b6-123">Avbildningen `helloacrbuild` är nu redo att användas.</span><span class="sxs-lookup"><span data-stu-id="b87b6-123">The `helloacrbuild` image is now ready to be used.</span></span>
