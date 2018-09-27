<span data-ttu-id="530e7-101">Du är nu redo att skapa en ny händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="530e7-101">You're now ready to create a new Event Hub.</span></span> <span data-ttu-id="530e7-102">När du har skapat händelsehubben använder du Azure Portal för att visa den nya hubben.</span><span class="sxs-lookup"><span data-stu-id="530e7-102">After creating the Event Hub, you'll use the Azure portal to view your new hub.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="set-some-defaults-in-the-azure-cli"></a><span data-ttu-id="530e7-103">Ange några standardinställningar i Azure CLI</span><span class="sxs-lookup"><span data-stu-id="530e7-103">Set some defaults in the Azure CLI</span></span>

<span data-ttu-id="530e7-104">Låt oss börja med att tillhandahålla några standardvärden för Azure CLI i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="530e7-104">Let's start by providing some default values for the Azure CLI in the Cloud Shell.</span></span> <span data-ttu-id="530e7-105">Detta gör att du inte behöver skriva in dem varje gång.</span><span class="sxs-lookup"><span data-stu-id="530e7-105">This will keep you from having to type these in every time.</span></span> <span data-ttu-id="530e7-106">Framför allt ska vi nu ställa in _resursgrupp_ och _plats_.</span><span class="sxs-lookup"><span data-stu-id="530e7-106">In particular, let's set the _resource group_ and _location_.</span></span> <span data-ttu-id="530e7-107">Välj en plats från nedanstående lista.</span><span class="sxs-lookup"><span data-stu-id="530e7-107">Select a location from the following list.</span></span>

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="530e7-108">Sedan anger du följande kommando i Azure CLI, se till att ersätta platsen med en nära dig.</span><span class="sxs-lookup"><span data-stu-id="530e7-108">Then type the following command into the Azure CLI, make sure to replace the location with one close to you.</span></span>

```azurecli
az configure --defaults group=<rgn>[sandbox Resource Group]</rgn> location=westus2
```

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="530e7-109">Skapa en Event Hubs-namnrymd</span><span class="sxs-lookup"><span data-stu-id="530e7-109">Create an Event Hubs namespace</span></span>

<span data-ttu-id="530e7-110">Följ stegen nedan för att skapa ett Event Hubs-namnområde med hjälp av Bash-gränssnittet som stöds av Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="530e7-110">Use the following steps to create an Event Hubs namespace using bash shell supported by Azure Cloud shell:</span></span>

1. <span data-ttu-id="530e7-111">Skapa Event Hubs-namnrymden med hjälp av kommandot `az eventhubs namespace create`.</span><span class="sxs-lookup"><span data-stu-id="530e7-111">Create the Event Hubs namespace using the `az eventhubs namespace create` command.</span></span> <span data-ttu-id="530e7-112">Använd följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="530e7-112">Use the following parameters.</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="530e7-113">Parameter</span><span class="sxs-lookup"><span data-stu-id="530e7-113">Parameter</span></span>      |<span data-ttu-id="530e7-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="530e7-114">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="530e7-115">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-115">--name (required)</span></span>      |<span data-ttu-id="530e7-116">Ange ett unikt namn för Event Hubs-namnområdet på mellan 6 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="530e7-116">Enter a 6-50 characters-long unique name for your Event Hubs namespace.</span></span> <span data-ttu-id="530e7-117">Namnet får endast innehålla bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="530e7-117">The name should contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="530e7-118">Det måste börja med en bokstav och sluta med en bokstav eller siffra.</span><span class="sxs-lookup"><span data-stu-id="530e7-118">It should start with a letter and end with a letter or number.</span></span>|
    > |<span data-ttu-id="530e7-119">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-119">--resource-group (required)</span></span> | <span data-ttu-id="530e7-120">Det här är den färdiga resursgruppen för sandbox-miljön i Azure från standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="530e7-120">This will be the pre-created Azure sandbox resource group supplied from the defaults.</span></span> |
    > |<span data-ttu-id="530e7-121">--l (valfritt)</span><span class="sxs-lookup"><span data-stu-id="530e7-121">--l (optional)</span></span>     |<span data-ttu-id="530e7-122">Ange platsen för ditt närmaste datacenter för Azure, det använder din standard.</span><span class="sxs-lookup"><span data-stu-id="530e7-122">Enter the location of your nearest Azure datacenter, this will use your default.</span></span>|
    > |<span data-ttu-id="530e7-123">--sku (valfritt)</span><span class="sxs-lookup"><span data-stu-id="530e7-123">--sku (optional)</span></span> | <span data-ttu-id="530e7-124">Prisnivån för namnområdet [Basic</span><span class="sxs-lookup"><span data-stu-id="530e7-124">The pricing tier for the namespace [Basic</span></span> | <span data-ttu-id="530e7-125">Standard], blir som standard _Standard_.</span><span class="sxs-lookup"><span data-stu-id="530e7-125">Standard], defaults to _Standard_.</span></span> <span data-ttu-id="530e7-126">Detta avgör anslutningar och konsumenttröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="530e7-126">This determines the connections and consumer thresholds.</span></span> |

    <span data-ttu-id="530e7-127">Ange ett namn till en miljövariabel så att vi kan återanvända den.</span><span class="sxs-lookup"><span data-stu-id="530e7-127">Set the name into an environment variable so we can reuse it.</span></span>

    ```azurecli
    NS_NAME=myEvt-HubNs1
    ````

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```azurecli
    az eventhubs namespace create --name $NS_NAME
    ```

    > [!NOTE]
    > <span data-ttu-id="530e7-128">Azure är mycket noga namnet och CLI returnerar **Felaktig begäran** om namnet finns eller är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="530e7-128">Azure is very picky about the name and the CLI returns **Bad Request** if the name exists or is invalid.</span></span> <span data-ttu-id="530e7-129">Prova ett annat namn genom att ändra miljövariabeln och utfärda kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="530e7-129">Try a different name by changing your environment variable and reissuing the command.</span></span>


1. <span data-ttu-id="530e7-130">Hämta anslutningssträngen för Event Hubs-namnområdet med hjälp av följande kommando.</span><span class="sxs-lookup"><span data-stu-id="530e7-130">Fetch the connection string for your Event Hubs namespace using the following command.</span></span> <span data-ttu-id="530e7-131">Du behöver den för att konfigurera program att skicka och ta emot meddelanden med hjälp av din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="530e7-131">You'll need this to configure applications to send and receive messages using your Event Hub.</span></span>

    ```azurecli
    az eventhubs namespace authorization-rule keys list --name RootManageSharedAccessKey --namespace-name $NS_NAME
    ```

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="530e7-132">Parameter</span><span class="sxs-lookup"><span data-stu-id="530e7-132">Parameter</span></span>      |<span data-ttu-id="530e7-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="530e7-133">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="530e7-134">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-134">--resource-group (required)</span></span>  | <span data-ttu-id="530e7-135">Det här är den färdiga resursgruppen för sandbox-miljön i Azure från standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="530e7-135">This will be the pre-created Azure sandbox resource group supplied from the defaults.</span></span> |
    > |<span data-ttu-id="530e7-136">--namespace-name (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-136">--namespace-name (required)</span></span>  | <span data-ttu-id="530e7-137">Ange namn på namnområdet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="530e7-137">Enter the name of the namespace you created.</span></span> |

    <span data-ttu-id="530e7-138">Det här kommandot returnerar ett JSON-block med anslutningssträngen för Event Hubs-namnområdet som du ska använda senare för att konfigurera dina program för utgivare och konsumenter.</span><span class="sxs-lookup"><span data-stu-id="530e7-138">This command returns a JSON block with the connection string for your Event Hubs namespace that you'll use later to configure your publisher and consumer applications.</span></span> <span data-ttu-id="530e7-139">Spara värdet för följande nycklar för användning senare.</span><span class="sxs-lookup"><span data-stu-id="530e7-139">Save the value of the following keys for later use.</span></span>

    - <span data-ttu-id="530e7-140">**primaryConnectionString**</span><span class="sxs-lookup"><span data-stu-id="530e7-140">**primaryConnectionString**</span></span>
    - <span data-ttu-id="530e7-141">**primaryKey**</span><span class="sxs-lookup"><span data-stu-id="530e7-141">**primaryKey**</span></span>

## <a name="create-an-event-hub"></a><span data-ttu-id="530e7-142">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="530e7-142">Create an Event Hub</span></span>

<span data-ttu-id="530e7-143">Följ stegen nedan för att skapa din nya händelsehubb:</span><span class="sxs-lookup"><span data-stu-id="530e7-143">Use the following steps to create your new Event Hub:</span></span>

1. <span data-ttu-id="530e7-144">Skapa en ny händelsehubb med hjälp av kommandot `eventhub create`.</span><span class="sxs-lookup"><span data-stu-id="530e7-144">Create a new Event Hub using the `eventhub create` command.</span></span> <span data-ttu-id="530e7-145">Den behöver följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="530e7-145">It needs the following parameters:</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="530e7-146">Parameter</span><span class="sxs-lookup"><span data-stu-id="530e7-146">Parameter</span></span>      |<span data-ttu-id="530e7-147">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="530e7-147">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="530e7-148">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-148">--name (required)</span></span>  |<span data-ttu-id="530e7-149">Ange ett namn för din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="530e7-149">Enter a name for your Event Hub.</span></span>|
    > |<span data-ttu-id="530e7-150">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-150">--resource-group (required)</span></span>  |<span data-ttu-id="530e7-151">Resursgruppens ägare.</span><span class="sxs-lookup"><span data-stu-id="530e7-151">Resource group owner.</span></span>|
    > |<span data-ttu-id="530e7-152">--namespace-name (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-152">--namespace-name (required)</span></span>      |<span data-ttu-id="530e7-153">Ange namnområdet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="530e7-153">Enter the namespace you created.</span></span>|

    <span data-ttu-id="530e7-154">Nu ska vi definiera Event Hub-namnet i en miljövariabel först.</span><span class="sxs-lookup"><span data-stu-id="530e7-154">Let's define the Event Hub name in an environment variable first.</span></span>

    ```azurecli
    HUB_NAME=[name]
    ```

    ```azurecli
    az eventhubs eventhub create --name $HUB_NAME --namespace-name $NS_NAME
    ```

1. <span data-ttu-id="530e7-155">Visa information om händelsehubben med hjälp av kommandot `eventhub show`.</span><span class="sxs-lookup"><span data-stu-id="530e7-155">View the details of your Event Hub using the `eventhub show` command.</span></span> <span data-ttu-id="530e7-156">Det tar:</span><span class="sxs-lookup"><span data-stu-id="530e7-156">It takes:</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="530e7-157">Parameter</span><span class="sxs-lookup"><span data-stu-id="530e7-157">Parameter</span></span>      |<span data-ttu-id="530e7-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="530e7-158">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="530e7-159">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-159">--resource-group (required)</span></span>  |<span data-ttu-id="530e7-160">Resursgruppens ägare.</span><span class="sxs-lookup"><span data-stu-id="530e7-160">Resource group owner.</span></span>|
    > |<span data-ttu-id="530e7-161">--namespace-name (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-161">--namespace-name (required)</span></span>      |<span data-ttu-id="530e7-162">Ange namnområdet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="530e7-162">Enter the namespace you created.</span></span>|
    > |<span data-ttu-id="530e7-163">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="530e7-163">--name  (required)</span></span>|<span data-ttu-id="530e7-164">Namnet på händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="530e7-164">Name of the Event Hub.</span></span>|

    ```azurecli
    az eventhubs eventhub show --namespace-name $NS_NAME --name $HUB_NAME
    ```

## <a name="view-the-event-hub-in-the-azure-portal"></a><span data-ttu-id="530e7-165">Visa händelsehubben på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="530e7-165">View the Event Hub in the Azure portal</span></span>

<span data-ttu-id="530e7-166">Nu ska vi se hur det ser ut i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="530e7-166">Next, let's see what this looks like in the Azure portal.</span></span>

1. <span data-ttu-id="530e7-167">Logga in på [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="530e7-167">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="530e7-168">Leta upp Event Hubs-namnområdet med hjälp av sökfältet överst på portalen.</span><span class="sxs-lookup"><span data-stu-id="530e7-168">Find your Event Hubs namespace using the Search bar at the top of portal.</span></span>

1. <span data-ttu-id="530e7-169">Välj namnområdet för att öppna det.</span><span class="sxs-lookup"><span data-stu-id="530e7-169">Select your namespace to open it.</span></span>

1. <span data-ttu-id="530e7-170">Välj **Event Hubs-namnområdet** under avsnittet **ENTITETER**.</span><span class="sxs-lookup"><span data-stu-id="530e7-170">Select **Event Hubs namespace** under the **ENTITIES** section.</span></span>

1. <span data-ttu-id="530e7-171">Klicka på **Händelsehubbar**.</span><span class="sxs-lookup"><span data-stu-id="530e7-171">Click **Event Hubs**.</span></span>

    <span data-ttu-id="530e7-172">Din händelsehubb visas med statusen **Aktiv** och standardvärden för **Kvarhållning av meddelanden** (*7*) och **Antal partitioner** (*4*).</span><span class="sxs-lookup"><span data-stu-id="530e7-172">Your Event Hub displays with a status of **Active**, and default values for **Message Retention** (*7*) and **Partition Count** of (*4*).</span></span>

    ![Händelsehubb på Azure Portal](../media/3-event-hub.png)

## <a name="summary"></a><span data-ttu-id="530e7-174">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="530e7-174">Summary</span></span>

<span data-ttu-id="530e7-175">Nu har du skapat en ny händelsehubb och har all nödvändig information som krävs för att konfigurera dina program för utgivare och konsumenter.</span><span class="sxs-lookup"><span data-stu-id="530e7-175">You've now created a new Event Hub, and you've all the necessary information ready to configure your publisher and consumer applications.</span></span>
