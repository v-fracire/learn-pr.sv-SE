<span data-ttu-id="5aa84-101">Anta att du har ett program för säljteamet i ditt globala företag.</span><span class="sxs-lookup"><span data-stu-id="5aa84-101">Suppose you have an application for the sales team in your global company.</span></span> <span data-ttu-id="5aa84-102">Varje teammedlem har en mobiltelefon där din app ska installeras.</span><span class="sxs-lookup"><span data-stu-id="5aa84-102">Each team member has a mobile phone where your app will be installed.</span></span> <span data-ttu-id="5aa84-103">En webbtjänst på Azure implementerar affärslogiken för ditt program och lagrar information i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5aa84-103">A web service hosted in Azure implements the business logic for your application and stores information in Azure SQL Database.</span></span> <span data-ttu-id="5aa84-104">Det finns en instans av webbtjänsten för varje geografisk region.</span><span class="sxs-lookup"><span data-stu-id="5aa84-104">There is one instance of the web service for each geographical region.</span></span> <span data-ttu-id="5aa84-105">Du har identifierat följande syften för att skicka meddelanden mellan mobilappen och webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="5aa84-105">You have identified the following purposes for sending messages between the mobile app and the web service:</span></span>

- <span data-ttu-id="5aa84-106">Meddelanden som är relaterade till enskilda försäljningar måste skickas endast till webbtjänstinstansen i användarens region.</span><span class="sxs-lookup"><span data-stu-id="5aa84-106">Messages that relate to individual sales must be sent only to the web service instance in the user's region.</span></span>
- <span data-ttu-id="5aa84-107">Meddelanden som är relaterade till försäljningsresultat måste skickas till alla instanser av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="5aa84-107">Messages that relate to sales performance must be sent to all instances of the web service.</span></span>

<span data-ttu-id="5aa84-108">Du har valt att implementera en Service Bus-kö för det första användningsfallet och Service Bus-ämnet för det andra.</span><span class="sxs-lookup"><span data-stu-id="5aa84-108">You have decided to implement a Service Bus queue for the first use case and the Service Bus topic for the second use case.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="5aa84-109">I den här övningen skapar du ett Service Bus-namnområde som innehåller både en kö och ett ämne med prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="5aa84-109">In this exercise, you will create a Service Bus namespace, which will contain both a queue and a topic with subscriptions.</span></span>

## <a name="create-a-service-bus-namespace"></a><span data-ttu-id="5aa84-110">Skapa ett namnområde för Service Bus</span><span class="sxs-lookup"><span data-stu-id="5aa84-110">Create a Service Bus namespace</span></span>

<span data-ttu-id="5aa84-111">I Azure Service Bus är ett namnområde en container med ett unikt fullständigt kvalificerat domännamn för köer, ämnen och reläer.</span><span class="sxs-lookup"><span data-stu-id="5aa84-111">In Azure Service Bus, a namespace is a container, with a unique fully qualified domain name, for queues, topics, and relays.</span></span> <span data-ttu-id="5aa84-112">Du börjar med att skapa namnområdet.</span><span class="sxs-lookup"><span data-stu-id="5aa84-112">You must start by creating the namespace.</span></span>

<span data-ttu-id="5aa84-113">Varje namnområde har också primära och sekundära krypteringsnycklar för signatur för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5aa84-113">Each namespace also has primary and secondary shared access signature encryption keys.</span></span> <span data-ttu-id="5aa84-114">En skickande eller mottagande komponent måste ange dessa nycklar när den ansluter för att få åtkomst till objekten inom namnområdet.</span><span class="sxs-lookup"><span data-stu-id="5aa84-114">A sending or receiving component must provide these keys when it connects to gain access to the objects within the namespace.</span></span>

<span data-ttu-id="5aa84-115">Om du vill skapa en Service Bus-namnrymd med hjälp av Azure Portal gör du så här:</span><span class="sxs-lookup"><span data-stu-id="5aa84-115">To create a Service Bus namespace by using the Azure portal, follow these steps:</span></span>

1. <span data-ttu-id="5aa84-116">Logga in på [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="5aa84-116">Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).</span></span>

1. <span data-ttu-id="5aa84-117">I det vänstra navigeringsfönstret klickar du på **Alla tjänster**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-117">In the navigation on the left, click **All services**.</span></span>

1. <span data-ttu-id="5aa84-118">I bladet **Alla tjänster** bläddrar du ned till avsnittet **Integrering** och klickar sedan på **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-118">In the **All Services** blade, scroll down to the **INTEGRATION** section, and then click **Service Bus**.</span></span>

    ![Skapa ett namnområde för Service Bus](../media/3-create-namespace-1.png)

1. <span data-ttu-id="5aa84-120">Längst upp till vänster på bladet **Service Bus** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-120">In the top left of the **Service Bus** blade, click **Add**.</span></span>

1. <span data-ttu-id="5aa84-121">Ange ett unikt namn för namnrymden i textrutan **Namn**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-121">In the **Name** text box, type a unique name for the namespace.</span></span> <span data-ttu-id="5aa84-122">Exempel: ”salesteamapp” + *dina initialer* + *aktuellt datum*.</span><span class="sxs-lookup"><span data-stu-id="5aa84-122">For example: "salesteamapp" + *your initials* + *current date*.</span></span>

1. <span data-ttu-id="5aa84-123">I listrutan **Prisnivå**, väljer du **Standard**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-123">In the **Pricing tier** drop-down list, select **Standard**.</span></span>

1. <span data-ttu-id="5aa84-124">I listrutan **Prenumeration** väljer du din prenumeration (”Concierge-prenumeration”).</span><span class="sxs-lookup"><span data-stu-id="5aa84-124">In the **Subscription** drop-down list, select your subscription ("Concierge subscription").</span></span>

1. <span data-ttu-id="5aa84-125">Under **Resursgrupp** väljer du **Använd befintlig** och sedan ”<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>”.</span><span class="sxs-lookup"><span data-stu-id="5aa84-125">Under **Resource group**, select **Use existing** and choose "<rgn>[sandbox resource group name]</rgn>".</span></span>

1. <span data-ttu-id="5aa84-126">I listrutan **Plats** väljer du en plats nära dig från listan nedan.</span><span class="sxs-lookup"><span data-stu-id="5aa84-126">In the **Location** drop-down list, select a location near you from the below list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="5aa84-127">Klicka på **Skapa** för att skapa Service Bus-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="5aa84-127">Click **Create** to create the Service Bus namespace.</span></span>

    ![Skapa ett namnområde för Service Bus](../media/3-create-namespace-2.png)

## <a name="create-a-service-bus-queue"></a><span data-ttu-id="5aa84-129">Skapa en Service Bus-kö</span><span class="sxs-lookup"><span data-stu-id="5aa84-129">Create a Service Bus queue</span></span>

<span data-ttu-id="5aa84-130">Nu när du har ett namnområde, kan du skapa en kö för meddelanden om enskild försäljning.</span><span class="sxs-lookup"><span data-stu-id="5aa84-130">Now that you have a namespace, you can create a queue for messages about individual sales.</span></span> <span data-ttu-id="5aa84-131">Det gör du genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="5aa84-131">To do this, follow these steps:</span></span>

1. <span data-ttu-id="5aa84-132">På bladet **Service Bus** klickar du på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-132">In the **Service Bus** blade, click **Refresh**.</span></span> <span data-ttu-id="5aa84-133">Det namnområde som du nyss skapade visas.</span><span class="sxs-lookup"><span data-stu-id="5aa84-133">The namespace you just created is displayed.</span></span>

1. <span data-ttu-id="5aa84-134">Klicka på det namnområde som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="5aa84-134">Click the namespace you just created.</span></span>

1. <span data-ttu-id="5aa84-135">Längst upp till vänster på namnområdesbladet klickar du på **+ Kö**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-135">In the top left of the namespace blade, click **+ Queue**.</span></span>

1. <span data-ttu-id="5aa84-136">Skriv **salesmessages** i textrutan **Namn** på bladet **Skapa kö** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-136">In the **Create queue** blade, in the **Name** text box, type **salesmessages**, and then click **Create**.</span></span> <span data-ttu-id="5aa84-137">Azure skapar kön i ditt namnområde.</span><span class="sxs-lookup"><span data-stu-id="5aa84-137">Azure creates the queue in your namespace.</span></span>

    ![Skapa en kö](../media/3-create-queue.png)

## <a name="create-a-service-bus-topic-and-subscriptions"></a><span data-ttu-id="5aa84-139">Skapa ett Service Bus-ämne och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="5aa84-139">Create a Service Bus topic and subscriptions</span></span>

<span data-ttu-id="5aa84-140">Du bör också skapa ett ämne som ska användas för meddelanden som är relaterade till försäljningsresultat.</span><span class="sxs-lookup"><span data-stu-id="5aa84-140">You also want to create a topic that will be used for messages that relate to sales performance.</span></span> <span data-ttu-id="5aa84-141">Flera instanser av webbtjänsten för affärslogik kommer att prenumerera på det här ämnet från olika länder.</span><span class="sxs-lookup"><span data-stu-id="5aa84-141">Multiple instances of the business logic web service will subscribe to this topic from different countries.</span></span> <span data-ttu-id="5aa84-142">Varje meddelande levereras till flera instanser.</span><span class="sxs-lookup"><span data-stu-id="5aa84-142">Each message will be delivered to multiple instances.</span></span>

<span data-ttu-id="5aa84-143">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="5aa84-143">Follow these steps:</span></span>

1. <span data-ttu-id="5aa84-144">På bladet **Service Bus-namnområde** klickar du på **+ Ämne**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-144">In the **Service Bus Namespace** blade, click **+ Topic**.</span></span>

1. <span data-ttu-id="5aa84-145">Skriv **salesperformancemessages** i textrutan **Namn** på bladet **Skapa ämne** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-145">In the **Create topic** blade, in the **Name** text box, type **salesperformancemessages**, and then click **Create**.</span></span> <span data-ttu-id="5aa84-146">Azure skapar ämnet i ditt namnområde.</span><span class="sxs-lookup"><span data-stu-id="5aa84-146">Azure creates the topic in your namespace.</span></span>

    ![Skapa ett ämne](../media/3-create-topic.png)

1. <span data-ttu-id="5aa84-148">Klicka på **Ämnen** under **Entiteter** på bladet **Service Bus-namnrymd** när ämnet har skapats.</span><span class="sxs-lookup"><span data-stu-id="5aa84-148">When the topic has been created, in the **Service Bus Namespace** blade, under **Entities**, click **Topics**.</span></span>

1. <span data-ttu-id="5aa84-149">Klicka på **salesperformancemessages** i listan med ämnen och klicka sedan på **+ Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-149">In the list of topics, click **salesperformancemessages**, and then click **+ Subscription**.</span></span>

1. <span data-ttu-id="5aa84-150">Skriv **Americas** i textrutan **Namn** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-150">In the **Name** text box, type **Americas**, and then click **Create**.</span></span>

1. <span data-ttu-id="5aa84-151">Klicka på **+ Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-151">Click **+ Subscription**.</span></span>

1. <span data-ttu-id="5aa84-152">Skriv **EuropeAndAfrica** i textrutan **Namn** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5aa84-152">In the **Name** text box, type **EuropeAndAfrica**, and then click **Create**.</span></span>

<span data-ttu-id="5aa84-153">Du har skapat den infrastruktur som krävs för att använda Service Bus för att öka flexibiliteten för din säljstyrkas distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="5aa84-153">You have built the infrastructure required to use Service Bus to increase the resilience of your sales force distributed application.</span></span> <span data-ttu-id="5aa84-154">Du har skapat en kö för meddelanden om enskild försäljning och ett ämne för meddelanden om försäljningsresultat.</span><span class="sxs-lookup"><span data-stu-id="5aa84-154">You have created a queue for messages about individual sales and a topic for messages about sales performance.</span></span> <span data-ttu-id="5aa84-155">Avsnittet innehåller flera prenumerationer eftersom meddelanden som skickas till detta ämne kan levereras till flera mottagande webbtjänster över hela världen.</span><span class="sxs-lookup"><span data-stu-id="5aa84-155">The topic includes multiple subscriptions because messages sent to that topic can be delivered to multiple recipient web services around the world.</span></span>
