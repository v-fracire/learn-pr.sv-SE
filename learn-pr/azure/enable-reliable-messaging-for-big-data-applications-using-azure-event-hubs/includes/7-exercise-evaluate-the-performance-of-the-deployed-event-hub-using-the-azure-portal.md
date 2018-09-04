<span data-ttu-id="aed4b-101">I den här övningen ska du använda Azure Portal för att kontrollera att din händelsehubb fungerar och att den uppfyller dina förväntningar.</span><span class="sxs-lookup"><span data-stu-id="aed4b-101">In this exercise, you'll use the Azure portal to verify your event hub is working and performing according to the desired expectations.</span></span> <span data-ttu-id="aed4b-102">Du ska också testa hur meddelanden från händelsehubben fungerar när den är tillfälligt otillgänglig och använda Event Hubs-mått för att kontrollera händelsehubbens prestanda.</span><span class="sxs-lookup"><span data-stu-id="aed4b-102">You'll also test how event hub messaging works when it's temporarily unavailable and use Event Hubs metrics to check the performance of your event hub.</span></span>

## <a name="view-event-hub-activity"></a><span data-ttu-id="aed4b-103">Visa aktivitet på händelsehubb</span><span class="sxs-lookup"><span data-stu-id="aed4b-103">View event hub activity</span></span>

1. <span data-ttu-id="aed4b-104">Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="aed4b-104">Sing in to your [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>
1. <span data-ttu-id="aed4b-105">Leta upp din händelsehubb med hjälp av sökfältet och öppna den.</span><span class="sxs-lookup"><span data-stu-id="aed4b-105">Find your event hub, using the Search bar, and open it.</span></span>

1. <span data-ttu-id="aed4b-106">Kontrollera antalet meddelanden på sidan Översikt.</span><span class="sxs-lookup"><span data-stu-id="aed4b-106">On the Overview page, view the message counts.</span></span>

    ![Visa meddelanden från händelsehubb](../media-draft/6-view-messages.png)

1. <span data-ttu-id="aed4b-108">Programmen SimpleSend och EventProcessorSample har konfigurerats att skicka och ta emot 100 meddelanden.</span><span class="sxs-lookup"><span data-stu-id="aed4b-108">The SimpleSend and EventProcessorSample applications are configured to send/receive 100 messages.</span></span> <span data-ttu-id="aed4b-109">Du ser att händelsehubben har bearbetat 100 meddelanden från SimpleSend-programmet och skickat 100 meddelanden till EventProcessorSample-programmet.</span><span class="sxs-lookup"><span data-stu-id="aed4b-109">You'll see that the event hub has processed 100 messages from the SimpleSend application, and has transmitted 100 messages to the EventProcessorSample application.</span></span>

## <a name="test-event-hub-resilience"></a><span data-ttu-id="aed4b-110">Testa händelsehubbens återhämtning</span><span class="sxs-lookup"><span data-stu-id="aed4b-110">Test event hub resilience</span></span>

<span data-ttu-id="aed4b-111">Följ stegen nedan för att se vad som händer när ett program skickar meddelanden till en händelsehubb när den är tillfälligt otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="aed4b-111">Use the following steps to see what happens when an application sends messages to an event hub while it's temporarily unavailable.</span></span>

1. <span data-ttu-id="aed4b-112">Skicka om meddelanden till händelsehubben med hjälp av SimpleSend-programmet.</span><span class="sxs-lookup"><span data-stu-id="aed4b-112">Resend messages to the event hub using the SimpleSend application.</span></span> <span data-ttu-id="aed4b-113">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="aed4b-113">Use the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. <span data-ttu-id="aed4b-114">Tryck på Retur när du ser **Överföringen är klar**.</span><span class="sxs-lookup"><span data-stu-id="aed4b-114">When you see **Send Complete...**, press ENTER.</span></span>

1. <span data-ttu-id="aed4b-115">Klicka på **Event Hubs-instans** > **INSTÄLLNINGAR** > **Egenskaper** på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="aed4b-115">In the Azure portal, click **Event Hubs Instance** > **SETTINGS** > **Properties**.</span></span>
1. <span data-ttu-id="aed4b-116">Klicka på **Inaktiverad** under Event Hub state (Event Hub-tillstånd).</span><span class="sxs-lookup"><span data-stu-id="aed4b-116">Under Event Hub state, click **Disabled**.</span></span>

    ![Inaktivera händelsehubb](../media-draft/7-disable-event-hub.png)

<span data-ttu-id="aed4b-118">Vänta minst fem minuter.</span><span class="sxs-lookup"><span data-stu-id="aed4b-118">Wait for a minimum of five minutes.</span></span>

1. <span data-ttu-id="aed4b-119">Klicka på **Aktiv** under Event Hub state (Event Hub-tillstånd) för att återaktivera händelsehubben och spara dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="aed4b-119">Click **Active** under Event Hub state to re-enable your event hub and save your changes.</span></span>
1. <span data-ttu-id="aed4b-120">Kör programmet EventProcessorSample igen för att ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="aed4b-120">Rerun the EventProcessorSample application to receive messages.</span></span> <span data-ttu-id="aed4b-121">Använd följande kommando.</span><span class="sxs-lookup"><span data-stu-id="aed4b-121">Use the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. <span data-ttu-id="aed4b-122">När meddelanden slutar att visas i konsolen trycker du på Retur.</span><span class="sxs-lookup"><span data-stu-id="aed4b-122">When messages stop being displayed to the console, press ENTER.</span></span>

1. <span data-ttu-id="aed4b-123">Leta upp händelsehubbens **_namnområde_** på Azure Portal och öppna det.</span><span class="sxs-lookup"><span data-stu-id="aed4b-123">In the Azure portal, find your event hub **_namespace_** and open it.</span></span> 

1. <span data-ttu-id="aed4b-124">Klicka på **Event Hubs-namnområde** > **ÖVERVAKNING** > **Mått (förhandsversion)**.</span><span class="sxs-lookup"><span data-stu-id="aed4b-124">Click **Event Hubs Namespace** > **MONITORING** > **Metrics (preview)**.</span></span>

    ![Använda Event Hub-mått](../media-draft/7-event-hub-metrics.png)

1. <span data-ttu-id="aed4b-126">Välj **Inkommande meddelanden** i listan **Mått** och klicka på **Lägg till mått**.</span><span class="sxs-lookup"><span data-stu-id="aed4b-126">From the **Metric** list, select **Incoming Messages** and click **Add Metric**.</span></span>
1. <span data-ttu-id="aed4b-127">Välj **Utgående meddelanden** i listan **Mått** och klicka på **Lägg till mått**.</span><span class="sxs-lookup"><span data-stu-id="aed4b-127">From the **Metric** list, select **Outgoing Messages** and click **Add Metric**.</span></span>
1. <span data-ttu-id="aed4b-128">Klicka på **Senaste 24 timmarna (automatiskt)** överst i diagrammet och ändra tidsperioden till **Senaste 30 minuterna**.</span><span class="sxs-lookup"><span data-stu-id="aed4b-128">At the top of the chart, click **Last 24 hours (Automatic)** to change the time period to **Last 30 minutes**.</span></span>
1. <span data-ttu-id="aed4b-129">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="aed4b-129">Click **Apply**.</span></span>

<span data-ttu-id="aed4b-130">Du ser att även om meddelandena skickades innan händelsehubben tillfälligt kopplades från, så överfördes alla 100 meddelanden korrekt.</span><span class="sxs-lookup"><span data-stu-id="aed4b-130">You'll see that though the messages were sent before the event hub was taken offline for a period, all 100 messages were successfully transmitted.</span></span>

## <a name="summary"></a><span data-ttu-id="aed4b-131">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="aed4b-131">Summary</span></span>

<span data-ttu-id="aed4b-132">I den här kursdelen har du använt Event Hubs-mått för att testa att din händelsehubb bearbetar meddelandena som skickas och tas emot korrekt.</span><span class="sxs-lookup"><span data-stu-id="aed4b-132">In this unit, you used the Event Hubs metrics to test that your event hub is successfully processing the sending and receiving messages.</span></span>
