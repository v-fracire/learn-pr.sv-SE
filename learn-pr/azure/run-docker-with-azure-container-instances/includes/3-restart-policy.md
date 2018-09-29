<span data-ttu-id="20807-101">Eftersom det går snabbt och är enkelt att distribuera containrar i Azure Container Instances är det en bra lösning för att köra engångsuppgifter som att skapa, testa och rendera avbildningar.</span><span class="sxs-lookup"><span data-stu-id="20807-101">The ease and speed of deploying containers in Azure Container Instances makes it a great fit for executing run-once tasks like build, test, and image rendering.</span></span>

<span data-ttu-id="20807-102">Du kan konfigurera en omstartsprincip, så du kan ange att containern ska stoppas när processen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="20807-102">With a configurable restart policy, you can specify that your containers are stopped when their processes have completed.</span></span> <span data-ttu-id="20807-103">Eftersom du faktureras per sekund för containerinstanser debiteras du endast för de beräkningsresurser som används när containern kör dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="20807-103">Because container instances are billed by the second, you're charged only for the compute resources used while the container executing your task is running.</span></span>

## <a name="container-restart-policies"></a><span data-ttu-id="20807-104">Principer för containeromstart</span><span class="sxs-lookup"><span data-stu-id="20807-104">Container restart policies</span></span>

<span data-ttu-id="20807-105">Azure Container Instances har tre alternativ omstartsprinciper:</span><span class="sxs-lookup"><span data-stu-id="20807-105">Azure Container Instances has three restart-policy options:</span></span>

| <span data-ttu-id="20807-106">Omstartsprincip</span><span class="sxs-lookup"><span data-stu-id="20807-106">Restart policy</span></span>   | <span data-ttu-id="20807-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="20807-107">Description</span></span> |
| ---------------- | :---------- |
| `Always` | <span data-ttu-id="20807-108">Containrar i containergruppen startas alltid om.</span><span class="sxs-lookup"><span data-stu-id="20807-108">Containers in the container group are always restarted.</span></span> <span data-ttu-id="20807-109">Den här principen passar för långvariga uppgifter, till exempel en webbserver.</span><span class="sxs-lookup"><span data-stu-id="20807-109">This policy makes sense for long-running tasks such as a web server.</span></span> <span data-ttu-id="20807-110">Det här är **standardvärdet** som används om du inte anger någon omstartsprincip när du skapar containern.</span><span class="sxs-lookup"><span data-stu-id="20807-110">This is the **default** setting applied when no restart policy is specified at container creation.</span></span> |
| `Never` | <span data-ttu-id="20807-111">Containers i containergruppen startas aldrig om.</span><span class="sxs-lookup"><span data-stu-id="20807-111">Containers in the container group are never restarted.</span></span> <span data-ttu-id="20807-112">Containers körs högst en gång.</span><span class="sxs-lookup"><span data-stu-id="20807-112">The containers run at most once.</span></span> |
| `OnFailure` | <span data-ttu-id="20807-113">Containers i containergruppen startas bara om när processen som körs i containern inte slutförs utan fel (när den avslutas med en annan slutkod än noll).</span><span class="sxs-lookup"><span data-stu-id="20807-113">Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code).</span></span> <span data-ttu-id="20807-114">Containrar körs minst en gång.</span><span class="sxs-lookup"><span data-stu-id="20807-114">The containers are run at least once.</span></span> <span data-ttu-id="20807-115">Den här principen fungerar bra för containrar som kör kortvariga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="20807-115">This policy works well for containers that run short-lived tasks.</span></span> |

## <a name="run-to-completion"></a><span data-ttu-id="20807-116">Kör till slutförande</span><span class="sxs-lookup"><span data-stu-id="20807-116">Run to completion</span></span>

<span data-ttu-id="20807-117">Om du vill se omstartsprincipen i praktiken skapar du en containerinstans från avbildningen *microsoft/aci-wordcount* och anger principen *OnFailure*.</span><span class="sxs-lookup"><span data-stu-id="20807-117">To see the restart policy in action, create a container instance from the *microsoft/aci-wordcount* image and specify the *OnFailure* restart policy.</span></span> <span data-ttu-id="20807-118">Den här exempelcontainern kör ett Python-skript som analyserar texten i Shakespeares Hamlet, skriver ut de 10 vanligaste orden till STDOUT och sedan avslutas.</span><span class="sxs-lookup"><span data-stu-id="20807-118">This example container runs a Python script that analyzes the text of Shakespeare's Hamlet, writes the 10 most common words to STDOUT, and then exits.</span></span>

<span data-ttu-id="20807-119">Kör exempelcontainern med kommandot `az container create`:</span><span class="sxs-lookup"><span data-stu-id="20807-119">Run the example container with the following `az container create` command:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --location eastus
```

<span data-ttu-id="20807-120">Azure Container Instances startar containern och stoppar den sedan när appen (eller skriptet i det här fallet) avslutas.</span><span class="sxs-lookup"><span data-stu-id="20807-120">Azure Container Instances starts the container then stops it when its application (or script, in this case) exits.</span></span> <span data-ttu-id="20807-121">När Azure Container Instances stoppar en container vars omstartsprincip är *Aldrig* eller *OnFailure* sätts containerns status till **Avslutad**.</span><span class="sxs-lookup"><span data-stu-id="20807-121">When Azure Container Instances stops a container whose restart policy is *Never* or *OnFailure*, the container's status is set to **Terminated**.</span></span>

<span data-ttu-id="20807-122">Du kan kontrollera statusen för en container med kommandot `az container show`:</span><span class="sxs-lookup"><span data-stu-id="20807-122">You can check a container's status with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

<span data-ttu-id="20807-123">När exempelcontainerns status blir **Avslutad** kan du se utdata för uppgiften i containerloggarna.</span><span class="sxs-lookup"><span data-stu-id="20807-123">Once the example container's status shows **Terminated**, you can see its task output by viewing the container logs.</span></span> <span data-ttu-id="20807-124">Kör kommandot `az container logs` för att visa utdata för skriptet:</span><span class="sxs-lookup"><span data-stu-id="20807-124">Run the `az container logs` command to view the script's output:</span></span>

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo
```

<span data-ttu-id="20807-125">Utdata:</span><span class="sxs-lookup"><span data-stu-id="20807-125">Output:</span></span>

```json
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```