<span data-ttu-id="88605-101">Med Azure Container Registry Build kan du skapa containeravbildningar i molnet utan att behöva några lokala Docker-verktyg.</span><span class="sxs-lookup"><span data-stu-id="88605-101">With Azure Container Registry Build, container images can be built in the cloud, without the need for local Docker tooling.</span></span> <span data-ttu-id="88605-102">I Container Registry Build kan du även integrera DevOps-processer med automatiserad kompilering när källkod bekräftas.</span><span class="sxs-lookup"><span data-stu-id="88605-102">Container Registry Build also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="88605-103">I den här utbildningsenheten kommer du att skapa en containeravbildning automatiskt med Azure Container Registry Build.</span><span class="sxs-lookup"><span data-stu-id="88605-103">In this unit, you automate the creation of a container image using Azure Container Registry Build.</span></span>

## <a name="create-a-container-image-with-build"></a><span data-ttu-id="88605-104">Skapa en containeravbildning med Build</span><span class="sxs-lookup"><span data-stu-id="88605-104">Create a container image with Build</span></span>

<span data-ttu-id="88605-105">I Azure Container Registry Build anger du skapandeinstruktioner i en vanlig Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="88605-105">When using Azure Container Registry Build, a standard Dockerfile is used to provide build instructions.</span></span> <span data-ttu-id="88605-106">Du kan använda valfri Dockerfile som för närvarande används i din miljö i Azure Container Registry Build, även om skapandet görs i flera steg.</span><span class="sxs-lookup"><span data-stu-id="88605-106">Any Dockerfile currently used in your environment, including multistaged builds, works with Azure Container Registry Build.</span></span>

<span data-ttu-id="88605-107">Skapa en fil med namnet `Dockerfile` och öppna den i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="88605-107">Create a file named `Dockerfile` and open it with any text editor.</span></span>

```bash
code Dockerfile
```

<span data-ttu-id="88605-108">Kopiera följande innehåll, spara filen och avsluta textredigeraren.</span><span class="sxs-lookup"><span data-stu-id="88605-108">Copy in the following contents, save the file, and exit the text editor.</span></span> <span data-ttu-id="88605-109">Denna Dockerfile lägger till en Node.js-app i avbildningen `node:9-alpine` och konfigurerar containern så att appen körs via port 80.</span><span class="sxs-lookup"><span data-stu-id="88605-109">This Dockerfile adds a Node.js application to the `node:9-alpine` image and configures the container to server the application on port 80.</span></span>

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="88605-110">Kör följande kommando för att skapa containeravbildningen från din Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="88605-110">Run the following command to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

<span data-ttu-id="88605-111">När den här åtgärden körs ser du att avbildningen skapas och push-överförs till containerregistret.</span><span class="sxs-lookup"><span data-stu-id="88605-111">As this operation runs, you will see the image being built and pushed to your container registry.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="88605-112">Verifiera avbildningen</span><span class="sxs-lookup"><span data-stu-id="88605-112">Verify the image</span></span>

<span data-ttu-id="88605-113">När skapandeåtgärden har slutförts kör du följande kommando för att verifiera att avbildningen har skapats och lagras i registret.</span><span class="sxs-lookup"><span data-stu-id="88605-113">When the build operation has completed, run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="88605-114">Utdata bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="88605-114">The output should look similar to the following:</span></span>

```console
Result
-------------
helloacrbuild
```

<span data-ttu-id="88605-115">Avbildningen `helloacrbuild` är nu redo att användas.</span><span class="sxs-lookup"><span data-stu-id="88605-115">The `helloacrbuild` image is now ready to be used.</span></span>

## <a name="summary"></a><span data-ttu-id="88605-116">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="88605-116">Summary</span></span>

<span data-ttu-id="88605-117">I den här utbildningsenheten använde du Azure Container Registry Build till att skapa en containeravbildning automatiskt.</span><span class="sxs-lookup"><span data-stu-id="88605-117">In this unit, Azure Container Registry Build was used to automate the creation of a container image.</span></span> <span data-ttu-id="88605-118">Avbildningen lagrades sedan i containerregistret i Azure.</span><span class="sxs-lookup"><span data-stu-id="88605-118">This image was then stored in the Azure container registry.</span></span> <span data-ttu-id="88605-119">I nästa utbildningsenhet distribuerar du en instans av den nya containeravbildningen till Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="88605-119">In the next unit, you will deploy an instance of the new container image to Azure Container Instances.</span></span>