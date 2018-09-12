<span data-ttu-id="c8678-101">Eftersom det går snabbt att distribuera containers i Azure Container Instances är det en bra plattform för att köra engångsuppgifter som att skapa, testa och återge avbildningar i en containerinstans.</span><span class="sxs-lookup"><span data-stu-id="c8678-101">The ease and speed of deploying containers in Azure Container Instances provides a compelling platform for executing run-once tasks like build, test, and image rendering in a container instance.</span></span>

<span data-ttu-id="c8678-102">Du kan konfigurera en omstartsprincip, så du kan ange att containern ska stoppas när processen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="c8678-102">With a configurable restart policy, you can specify that your containers are stopped when their processes have completed.</span></span> <span data-ttu-id="c8678-103">Eftersom du faktureras per sekund för containerinstanser debiteras du endast för de beräkningsresurser som används när containern kör dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="c8678-103">Because container instances are billed by the second, you're charged only for the compute resources used while the container executing your task is running.</span></span>

## <a name="container-restart-policies"></a><span data-ttu-id="c8678-104">Principer för containeromstart</span><span class="sxs-lookup"><span data-stu-id="c8678-104">Container restart policies</span></span>

<span data-ttu-id="c8678-105">När du skapar en container i Azure Container Instances kan ange du en av tre principinställningar för omstarter:</span><span class="sxs-lookup"><span data-stu-id="c8678-105">When you create a container in Azure Container Instances, you can specify one of three restart policy settings:</span></span>

| <span data-ttu-id="c8678-106">Omstartsprincip</span><span class="sxs-lookup"><span data-stu-id="c8678-106">Restart policy</span></span>   | <span data-ttu-id="c8678-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c8678-107">Description</span></span> |
| ---------------- | :---------- |
| `Always` | <span data-ttu-id="c8678-108">Containers i containergruppen startas alltid om.</span><span class="sxs-lookup"><span data-stu-id="c8678-108">Containers in the container group are always restarted.</span></span> <span data-ttu-id="c8678-109">Det här är **standardvärdet** som används om du inte anger någon omstartsprincip när du skapar containern.</span><span class="sxs-lookup"><span data-stu-id="c8678-109">This is the **default** setting applied when no restart policy is specified at container creation.</span></span> |
| `Never` | <span data-ttu-id="c8678-110">Containers i containergruppen startas aldrig om.</span><span class="sxs-lookup"><span data-stu-id="c8678-110">Containers in the container group are never restarted.</span></span> <span data-ttu-id="c8678-111">Containers körs högst en gång.</span><span class="sxs-lookup"><span data-stu-id="c8678-111">The containers run at most once.</span></span> |
| `OnFailure` | <span data-ttu-id="c8678-112">Containers i containergruppen startas bara om när processen som körs i containern inte slutförs utan fel (när den avslutas med en annan slutkod än noll).</span><span class="sxs-lookup"><span data-stu-id="c8678-112">Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code).</span></span> <span data-ttu-id="c8678-113">Containrar körs minst en gång.</span><span class="sxs-lookup"><span data-stu-id="c8678-113">The containers are run at least once.</span></span> |

<span data-ttu-id="c8678-114">I föregående enhet i den här modulen skapade du en container utan någon angiven omstartsprincip.</span><span class="sxs-lookup"><span data-stu-id="c8678-114">In the previous unit of this module, a container was created without a specified restart policy.</span></span> <span data-ttu-id="c8678-115">Som standard fick den här containern omstartsprincipen *Alltid*.</span><span class="sxs-lookup"><span data-stu-id="c8678-115">By default, this container received the *Always* restart policy.</span></span> <span data-ttu-id="c8678-116">Eftersom arbetsbelastningen i containern körs länge (en webbserver) är den här principen lämplig.</span><span class="sxs-lookup"><span data-stu-id="c8678-116">Because the workload in the container is long running (a web server), this policy makes sense.</span></span>

## <a name="run-to-completion"></a><span data-ttu-id="c8678-117">Kör till slutförande</span><span class="sxs-lookup"><span data-stu-id="c8678-117">Run to completion</span></span>

<span data-ttu-id="c8678-118">Om du vill se omstartsprincipen i praktiken skapar du en containerinstans från avbildningen *microsoft/aci-wordcount* och anger principen *OnFailure*.</span><span class="sxs-lookup"><span data-stu-id="c8678-118">To see the restart policy in action, create a container instance from the *microsoft/aci-wordcount* image and specify the *OnFailure* restart policy.</span></span> <span data-ttu-id="c8678-119">Den här exempelcontainern kör ett Python-skript som analyserar texten i Shakespeares Hamlet, skriver ut de 10 vanligaste orden till STDOUT och sedan avslutas.</span><span class="sxs-lookup"><span data-stu-id="c8678-119">This example container runs a Python script that analyzes the text of Shakespeare's Hamlet, writes the 10 most common words to STDOUT, and then exits.</span></span>

<span data-ttu-id="c8678-120">Kör exempelcontainern med kommandot `az container create`:</span><span class="sxs-lookup"><span data-stu-id="c8678-120">Run the example container with the following `az container create` command:</span></span>

```azureclu
az container create \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

<span data-ttu-id="c8678-121">Azure Container Instances startar containern och stoppar den när appen (eller skriptet i det här fallet) avslutas.</span><span class="sxs-lookup"><span data-stu-id="c8678-121">Azure Container Instances starts the container and then stops it when its application (or script, in this case) exits.</span></span> <span data-ttu-id="c8678-122">När Azure Container Instances stoppar en container vars omstartsprincip är *Aldrig* eller *OnFailure* sätts containerns status till **Avslutad**.</span><span class="sxs-lookup"><span data-stu-id="c8678-122">When Azure Container Instances stops a container whose restart policy is *Never* or *OnFailure*, the container's status is set to **Terminated**.</span></span>

<span data-ttu-id="c8678-123">Du kan kontrollera statusen för en container med kommandot `az container show`:</span><span class="sxs-lookup"><span data-stu-id="c8678-123">You can check a container's status with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

<span data-ttu-id="c8678-124">När exempelcontainerns status blir **Avslutad** kan du se utdata för uppgiften i containerloggarna.</span><span class="sxs-lookup"><span data-stu-id="c8678-124">Once the example container's status shows **Terminated**, you can see its task output by viewing the container logs.</span></span> <span data-ttu-id="c8678-125">Kör kommandot **az container logs** för att visa utdata för skriptet:</span><span class="sxs-lookup"><span data-stu-id="c8678-125">Run the **az container logs** command to view the script's output:</span></span>

```azurecli
az container logs --resource-group myResourceGroup --name mycontainer-restart-demo
```

<span data-ttu-id="c8678-126">Utdata:</span><span class="sxs-lookup"><span data-stu-id="c8678-126">Output:</span></span>

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

## <a name="summary"></a><span data-ttu-id="c8678-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c8678-127">Summary</span></span>

<span data-ttu-id="c8678-128">I den här utbildningsenheten skapade du en containerinstans med omstartsprincipen *OnFailure*.</span><span class="sxs-lookup"><span data-stu-id="c8678-128">In this unit, you created a container instance with a restart policy of *OnFailure*.</span></span> <span data-ttu-id="c8678-129">Den här konfigurationen fungerar bra för containers som kör kortvariga aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c8678-129">This configuration works well for containers that run short-lived tasks.</span></span>

<span data-ttu-id="c8678-130">I nästa utbildningsenhet ställer du in miljövariabler för Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="c8678-130">In the next unit, you will set environment variables in Azure Container Instances.</span></span>
