<span data-ttu-id="f823d-101">Du är nu redo att skapa en ny händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="f823d-101">You're now ready to create a new event hub.</span></span> <span data-ttu-id="f823d-102">När du har skapat händelsehubben använder du Azure Portal för att visa den nya hubben.</span><span class="sxs-lookup"><span data-stu-id="f823d-102">After creating the event hub, you'll use the Azure portal to view your new hub.</span></span>

<span data-ttu-id="f823d-103">Du ska skapa en händelsehubb med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f823d-103">You'll create an event hub using the Azure CLI.</span></span> <span data-ttu-id="f823d-104">I den här övningen ska du använda Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="f823d-104">For this exercise, use the Azure CLI 2.0.</span></span> 

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="f823d-105">Skapa ett Event Hubs-namnområde</span><span class="sxs-lookup"><span data-stu-id="f823d-105">Create an Event Hubs namespace</span></span>

<span data-ttu-id="f823d-106">Följ stegen nedan för att skapa ett Event Hubs-namnområde med hjälp av Bash-gränssnittet som stöds av Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="f823d-106">Use the following steps to create an Event Hubs namespace using Bash shell supported by Azure Cloud shell:</span></span>

1. <span data-ttu-id="f823d-107">Logga in till Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="f823d-107">Sign in to the Cloud Shell (Bash).</span></span>  

1. <span data-ttu-id="f823d-108">Skapa en Azure-resursgrupp med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f823d-108">Create an Azure resource group using the following command:</span></span>

    ```azurecli
        az group create --name <resource group name> --location <location>
    ```

    |<span data-ttu-id="f823d-109">Parameter</span><span class="sxs-lookup"><span data-stu-id="f823d-109">Parameter</span></span>      |<span data-ttu-id="f823d-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f823d-110">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f823d-111">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-111">--name (required)</span></span>      |<span data-ttu-id="f823d-112">Ange ett nytt resursgruppsnamn.</span><span class="sxs-lookup"><span data-stu-id="f823d-112">Enter a new resource group name.</span></span>|
    |<span data-ttu-id="f823d-113">--location (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-113">--location (required)</span></span>     |<span data-ttu-id="f823d-114">Ange platsen för ditt närmaste Azure-datacenter, till exempel westus (västra USA).</span><span class="sxs-lookup"><span data-stu-id="f823d-114">Enter the location of your nearest Azure datacenter, for example, westus.</span></span>|

1. <span data-ttu-id="f823d-115">Skapa Event Hubs-namnområdet med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f823d-115">Create the Event Hubs namespace using the following command:</span></span>

    ```azurecli
        az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

    |<span data-ttu-id="f823d-116">Parameter</span><span class="sxs-lookup"><span data-stu-id="f823d-116">Parameter</span></span>      |<span data-ttu-id="f823d-117">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f823d-117">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f823d-118">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-118">--name (required)</span></span>      |<span data-ttu-id="f823d-119">Ange ett unikt namn för Event Hubs-namnområdet på mellan 6 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="f823d-119">Enter a 6-50 characters-long unique name for your Event Hubs namespace.</span></span> <span data-ttu-id="f823d-120">Namnet får endast innehålla bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="f823d-120">The name should contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="f823d-121">Det måste börja med en bokstav och sluta med en bokstav eller siffra.</span><span class="sxs-lookup"><span data-stu-id="f823d-121">It should start with a letter and end with a letter or number.</span></span>|
    |<span data-ttu-id="f823d-122">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-122">--resource-group (required)</span></span>  |<span data-ttu-id="f823d-123">Ange resursgruppen som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="f823d-123">Enter the resource group you created in step 1.</span></span>
    |<span data-ttu-id="f823d-124">--l (valfritt)</span><span class="sxs-lookup"><span data-stu-id="f823d-124">--l (optional)</span></span>     |<span data-ttu-id="f823d-125">Ange platsen för ditt närmaste Azure-datacenter, till exempel westus (västra USA).</span><span class="sxs-lookup"><span data-stu-id="f823d-125">Enter the location of your nearest Azure datacenter, for example, westus.</span></span>|

1. <span data-ttu-id="f823d-126">Hämta anslutningssträngen för Event Hubs-namnområdet med hjälp av följande kommando.</span><span class="sxs-lookup"><span data-stu-id="f823d-126">Fetch the connection string for your Event Hubs namespace using the following command.</span></span> <span data-ttu-id="f823d-127">Du behöver den för att konfigurera program att skicka och ta emot meddelanden med hjälp av din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="f823d-127">You'll need this to configure applications to send and receive messages using your event hub.</span></span>

    ```azurecli
        az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

    |<span data-ttu-id="f823d-128">Parameter</span><span class="sxs-lookup"><span data-stu-id="f823d-128">Parameter</span></span>      |<span data-ttu-id="f823d-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f823d-129">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f823d-130">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-130">--resource-group (required)</span></span>  |<span data-ttu-id="f823d-131">Ange resursgruppen som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="f823d-131">Enter the resource group you created in step 1.</span></span>|
    |<span data-ttu-id="f823d-132">--namespace-name (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-132">--namespace-name (required)</span></span>      |<span data-ttu-id="f823d-133">Ange namnområdet som du skapade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="f823d-133">Enter the namespace you created in step 2.</span></span>|

    <span data-ttu-id="f823d-134">Det här kommandot returnerar anslutningssträngen för Event Hubs-namnområdet som du ska använda senare för att konfigurera dina program för utgivare och konsumenter.</span><span class="sxs-lookup"><span data-stu-id="f823d-134">This command returns the connection string for your Event Hubs namespace that you'll use later to configure your publisher and consumer applications.</span></span> <span data-ttu-id="f823d-135">Spara värdet för följande nycklar för användning senare.</span><span class="sxs-lookup"><span data-stu-id="f823d-135">Save the value of the following keys for later use.</span></span>

    - <span data-ttu-id="f823d-136">**primaryConnectionString**</span><span class="sxs-lookup"><span data-stu-id="f823d-136">**primaryConnectionString**</span></span>
    - <span data-ttu-id="f823d-137">**primaryKey**</span><span class="sxs-lookup"><span data-stu-id="f823d-137">**primaryKey**</span></span>

## <a name="create-an-event-hub"></a><span data-ttu-id="f823d-138">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="f823d-138">Create an event hub</span></span>

<span data-ttu-id="f823d-139">Följ stegen nedan för att skapa din nya händelsehubb:</span><span class="sxs-lookup"><span data-stu-id="f823d-139">Use the following steps to create your new event hub:</span></span>

1. <span data-ttu-id="f823d-140">Skapa en ny händelsehubb med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f823d-140">Create a new event hub using the following command:</span></span>

    ```azurecli
        az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

    |<span data-ttu-id="f823d-141">Parameter</span><span class="sxs-lookup"><span data-stu-id="f823d-141">Parameter</span></span>      |<span data-ttu-id="f823d-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f823d-142">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f823d-143">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-143">--name (required)</span></span>  |<span data-ttu-id="f823d-144">Ange ett namn för din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="f823d-144">Enter a name for your event hub.</span></span>|
    |<span data-ttu-id="f823d-145">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-145">--resource-group (required)</span></span>  |<span data-ttu-id="f823d-146">Ange resursgruppen som du skapade i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="f823d-146">Enter the resource group you created in the previous procedure.</span></span>|
    |<span data-ttu-id="f823d-147">--namespace-name (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-147">--namespace-name (required)</span></span>      |<span data-ttu-id="f823d-148">Ange namnområdet som du skapade i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="f823d-148">Enter the namespace you created in the previous procedure.</span></span>|

1. <span data-ttu-id="f823d-149">Visa information om händelsehubben med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f823d-149">View the details of your event hub using the following command:</span></span> 

    ```azurecli
        az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>
    ```

    |<span data-ttu-id="f823d-150">Parameter</span><span class="sxs-lookup"><span data-stu-id="f823d-150">Parameter</span></span>      |<span data-ttu-id="f823d-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f823d-151">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f823d-152">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-152">--resource-group (required)</span></span>  |<span data-ttu-id="f823d-153">Ange resursgruppen som du skapade i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="f823d-153">Enter the resource group that you created in the previous procedure.</span></span>|
    |<span data-ttu-id="f823d-154">--namespace-name (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-154">--namespace-name (required)</span></span>      |<span data-ttu-id="f823d-155">Ange namnområdet som du skapade i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="f823d-155">Enter the namespace you created in the previous procedure.</span></span>|
    |<span data-ttu-id="f823d-156">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="f823d-156">--name  (required)</span></span>|<span data-ttu-id="f823d-157">Ange namnet på händelsehubben som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="f823d-157">Enter the name of the event hub you created in step 1.</span></span>|

## <a name="view-the-event-hub-in-the-azure-portal"></a><span data-ttu-id="f823d-158">Visa händelsehubben på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f823d-158">View the event hub in the Azure portal</span></span>

<span data-ttu-id="f823d-159">Följ stegen nedan för att visa händelsehubben på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f823d-159">Use the following steps view your event hub in the Azure portal.</span></span>

1. <span data-ttu-id="f823d-160">Leta upp Event Hubs-namnområdet med hjälp av sökfältet överst på [Azure Portal](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="f823d-160">Find your Event Hubs namespace using the Search bar at the top of the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="f823d-161">Klicka på namnområdet för att öppna det.</span><span class="sxs-lookup"><span data-stu-id="f823d-161">Click your namespace to open it.</span></span>

1. <span data-ttu-id="f823d-162">Klicka på **Event Hubs** från **Event Hubs-namnområde** > **ENTITETER**.</span><span class="sxs-lookup"><span data-stu-id="f823d-162">From **Event Hubs Namespace** > **ENTITIES**, click **Event Hubs**.</span></span>
    <span data-ttu-id="f823d-163">Din händelsehubb visas med statusen **Aktiv** och standardvärden för **Kvarhållning av meddelanden** (*7*) och **Antal partitioner** (*4*).</span><span class="sxs-lookup"><span data-stu-id="f823d-163">Your event hub displays with a status of **Active**, and default values for **Message Retention** (*7*) and **Partition Count** of (*4*).</span></span>

    ![Händelsehubb på Azure Portal](../media-draft/3-event-hub.png)

## <a name="summary"></a><span data-ttu-id="f823d-165">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f823d-165">Summary</span></span>

<span data-ttu-id="f823d-166">Nu har du skapat en ny händelsehubb och har all nödvändig information som krävs för att konfigurera dina program för utgivare och konsumenter.</span><span class="sxs-lookup"><span data-stu-id="f823d-166">You've now created a new event hub, and you've all the necessary information ready to configure your publisher and consumer applications.</span></span>
