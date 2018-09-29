<span data-ttu-id="7365f-101">I den här delen använder du Azure-portalen för att kontrollera att din händelsehubb fungerar och att den uppfyller dina förväntningar.</span><span class="sxs-lookup"><span data-stu-id="7365f-101">In this unit, you'll use the Azure portal to verify your Event Hub is working and performing according to the desired expectations.</span></span> <span data-ttu-id="7365f-102">Du ska också testa hur meddelanden från händelsehubben fungerar när den är tillfälligt otillgänglig och använda Event Hubs-mått för att kontrollera händelsehubbens prestanda.</span><span class="sxs-lookup"><span data-stu-id="7365f-102">You'll also test how Event Hub messaging works when it's temporarily unavailable and use Event Hubs metrics to check the performance of your Event Hub.</span></span>

## <a name="view-event-hub-activity"></a><span data-ttu-id="7365f-103">Visa aktivitet på händelsehubb</span><span class="sxs-lookup"><span data-stu-id="7365f-103">View Event Hub activity</span></span>

1. <span data-ttu-id="7365f-104">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="7365f-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="7365f-105">Leta upp din händelsehubb med hjälp av sökfältet och öppna den.</span><span class="sxs-lookup"><span data-stu-id="7365f-105">Find your Event Hub, using the Search bar, and open it.</span></span>

1. <span data-ttu-id="7365f-106">Kontrollera antalet meddelanden på sidan Översikt.</span><span class="sxs-lookup"><span data-stu-id="7365f-106">On the Overview page, view the message counts.</span></span>

    ![Skärmbild av Azure-portalen som visar ett Event Hub-namnområde med antal meddelanden](../media/6-view-messages.png)

1. <span data-ttu-id="7365f-108">Programmen SimpleSend och EventProcessorSample har konfigurerats att skicka och ta emot 100 meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7365f-108">The SimpleSend and EventProcessorSample applications are configured to send/receive 100 messages.</span></span> <span data-ttu-id="7365f-109">Du ser att händelsehubben har bearbetat 100 meddelanden från SimpleSend-programmet och skickat 100 meddelanden till EventProcessorSample-programmet.</span><span class="sxs-lookup"><span data-stu-id="7365f-109">You'll see that the Event Hub has processed 100 messages from the SimpleSend application and has transmitted 100 messages to the EventProcessorSample application.</span></span>

## <a name="test-event-hub-resilience"></a><span data-ttu-id="7365f-110">Testa händelsehubbens återhämtningsförmåga</span><span class="sxs-lookup"><span data-stu-id="7365f-110">Test Event Hub resilience</span></span>

<span data-ttu-id="7365f-111">Följ stegen nedan för att se vad som händer när ett program skickar meddelanden till en händelsehubb när den är tillfälligt otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7365f-111">Use the following steps to see what happens when an application sends messages to an Event Hub while it's temporarily unavailable.</span></span>

1. <span data-ttu-id="7365f-112">Skicka om meddelanden till händelsehubben med hjälp av SimpleSend-programmet.</span><span class="sxs-lookup"><span data-stu-id="7365f-112">Resend messages to the Event Hub using the SimpleSend application.</span></span> <span data-ttu-id="7365f-113">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7365f-113">Use the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="7365f-114">Tryck på <kbd>RETUR</kbd> när du ser **Överföringen är klar...**.</span><span class="sxs-lookup"><span data-stu-id="7365f-114">When you see **Send Complete...**, press <kbd>ENTER</kbd>.</span></span>

1. <span data-ttu-id="7365f-115">Välj din händelsehubb på skärmen **översikt** – detta visar information om händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="7365f-115">Select your Event Hub in the **Overview** screen - this will show details specific to the Event Hub.</span></span> <span data-ttu-id="7365f-116">Du kan också hämta den här skärmen via posten **Händelsehubbar** på namnområdessidan.</span><span class="sxs-lookup"><span data-stu-id="7365f-116">You can also get to this screen through the **Event Hubs** entry from the namespace page.</span></span>

1. <span data-ttu-id="7365f-117">Välj **Inställningar** > **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="7365f-117">Select **Settings** > **Properties**.</span></span>

1. <span data-ttu-id="7365f-118">Klicka på **Inaktiverad** under Event Hub state (Event Hub-tillstånd).</span><span class="sxs-lookup"><span data-stu-id="7365f-118">Under Event Hub state, click **Disabled**.</span></span> <span data-ttu-id="7365f-119">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7365f-119">Save the changes.</span></span>

    ![Inaktivera händelsehubb](../media/7-disable-event-hub.png)

    <span data-ttu-id="7365f-121">**Vänta minst fem minuter.**</span><span class="sxs-lookup"><span data-stu-id="7365f-121">**Wait for a minimum of five minutes.**</span></span>

1. <span data-ttu-id="7365f-122">Klicka på **Aktiv** under Event Hub state (tillstånd för händelsehubb) för att återaktivera händelsehubben och spara dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="7365f-122">Click **Active** under Event Hub state to re-enable your Event Hub and save your changes.</span></span>

1. <span data-ttu-id="7365f-123">Kör programmet EventProcessorSample igen för att ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7365f-123">Rerun the EventProcessorSample application to receive messages.</span></span> <span data-ttu-id="7365f-124">Använd följande kommando.</span><span class="sxs-lookup"><span data-stu-id="7365f-124">Use the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="7365f-125">När meddelanden slutar att visas i konsolen trycker du på <kbd>RETUR</kbd>.</span><span class="sxs-lookup"><span data-stu-id="7365f-125">When messages stop being displayed to the console, press <kbd>ENTER</kbd>.</span></span>

1. <span data-ttu-id="7365f-126">Gå tillbaka till händelsehubbens namnområde på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7365f-126">Back in the Azure portal, go back to your Event Hub Namespace.</span></span> <span data-ttu-id="7365f-127">Om du befinner dig på händelseportalsidan, kan du använda länken överst på skärmen för att gå bakåt.</span><span class="sxs-lookup"><span data-stu-id="7365f-127">If you are still on the Event Hub page, you can use the breadcrumb on the top of the screen to go backwards.</span></span> <span data-ttu-id="7365f-128">Du kan också söka efter namnområdet och välja det.</span><span class="sxs-lookup"><span data-stu-id="7365f-128">Or you can search for the namespace and select it.</span></span>

1. <span data-ttu-id="7365f-129">Klicka på måttet **ÖVERVAKNING** >  **(förhandsgranskning)**.</span><span class="sxs-lookup"><span data-stu-id="7365f-129">Click **MONITORING** > **Metrics (preview)**.</span></span>

    ![Skärmbild som visar mått för händelseövervakning med där antalet inkommande och utgående meddelanden visas.](../media/7-event-hub-metrics.png)

1. <span data-ttu-id="7365f-131">Välj **Inkommande meddelanden** i listan **Mått** och klicka på **Lägg till mått**.</span><span class="sxs-lookup"><span data-stu-id="7365f-131">From the **Metric** list, select **Incoming Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="7365f-132">Välj **Utgående meddelanden** i listan **Mått** och klicka på **Lägg till mått**.</span><span class="sxs-lookup"><span data-stu-id="7365f-132">From the **Metric** list, select **Outgoing Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="7365f-133">Klicka på **Senaste 24 timmarna (automatiskt)** överst i diagrammet och ändra tidsperioden till **Senaste 30 minuterna** för att expandera datadiagrammet.</span><span class="sxs-lookup"><span data-stu-id="7365f-133">At the top of the chart, click **Last 24 hours (Automatic)** to change the time period to **Last 30 minutes** to expand the data graph.</span></span>

<span data-ttu-id="7365f-134">Du ser att även om meddelandena skickades innan händelsehubben tillfälligt kopplades från så överfördes alla 100 meddelanden korrekt.</span><span class="sxs-lookup"><span data-stu-id="7365f-134">You'll see that though the messages were sent before the Event Hub was taken offline for a period, all 100 messages were successfully transmitted.</span></span>

## <a name="summary"></a><span data-ttu-id="7365f-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7365f-135">Summary</span></span>

<span data-ttu-id="7365f-136">I den här kursdelen har du använt Event Hubs-mått för att testa att din händelsehubb bearbetar de meddelanden som skickas och tas emot korrekt.</span><span class="sxs-lookup"><span data-stu-id="7365f-136">In this unit, you used the Event Hubs metrics to test that your Event Hub is successfully processing the sending and receiving messages.</span></span>
