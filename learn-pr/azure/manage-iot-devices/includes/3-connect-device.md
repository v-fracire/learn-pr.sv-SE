<span data-ttu-id="ad79a-101">Att samla in väderdata är en viktig uppgift eftersom vädret kan påverka allt från trafikmönster till hur HVAC-system (värme, ventilation och luftkonditionering) inom detaljhandeln används.</span><span class="sxs-lookup"><span data-stu-id="ad79a-101">Capturing of weather data is an important task as weather can effect everything from traffic patterns to how heating, ventilation, and air conditioning (HVAC) systems in retail stores are operated.</span></span> <span data-ttu-id="ad79a-102">I den här övningen interagerar du med Raspberry Pi-onlinesimulatorn som du lärde känna i den tidigare enheten för att samla in simulerade väderdata och via Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ad79a-102">In this exercise, you will be interacting with the online Raspberry Pi simulator you learned about in the previous unit to capture simulated weather data and via the Azure IoT Hub.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="ad79a-103">Den här övningen körs i en simulerad miljö, men det program som körs på den simulerade enheten kan överföras till en verklig enhet i framtiden.</span><span class="sxs-lookup"><span data-stu-id="ad79a-103">While this exercise is being conducted in a simulated environment, the application running on the simulated device can be transferred to a real device in the future.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="ad79a-104">Skapa en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="ad79a-104">Create an IoT hub</span></span>
<span data-ttu-id="ad79a-105">Azure IoT Hub innehåller funktionerna och en modell för utökningsbarhet som gör det möjligt enhets- och backend-utvecklare att bygga robusta lösningar för enhetshantering.</span><span class="sxs-lookup"><span data-stu-id="ad79a-105">Azure IoT Hub provides the features and an extensibility model that enable device and back-end developers to build robust device management solutions.</span></span> <span data-ttu-id="ad79a-106">Enheter är allt från begränsade sensorer och enskilda mikrostyrenheter för ett särskilt ändamål till kraftfulla gateways som dirigerar kommunikationen för grupper av enheter.</span><span class="sxs-lookup"><span data-stu-id="ad79a-106">Devices range from constrained sensors and single purpose microcontrollers, to powerful gateways that route communications for groups of devices.</span></span> <span data-ttu-id="ad79a-107">Dessutom varierar användningsfall och krav för IoT-operatörer avsevärt mellan olika branscher.</span><span class="sxs-lookup"><span data-stu-id="ad79a-107">In addition, the use cases and requirements for IoT operators vary significantly across industries.</span></span> <span data-ttu-id="ad79a-108">Trots variationen ger enhetshantering med IoT Hub funktioner, mönster och kodbibliotek för att serva en mängd olika enheter och slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="ad79a-108">Despite this variation, device management with IoT Hub provides the capabilities, patterns, and code libraries to cater to a diverse set of devices and end users.</span></span>

<span data-ttu-id="ad79a-109">För att börja samla in data från Raspberry Pi-simulatorn så måste du först skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ad79a-109">In order to start collecting the data from the Raspberry Pi simulator, you need to first create an IoT hub.</span></span>

1. <span data-ttu-id="ad79a-110">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="ad79a-110">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

2. <span data-ttu-id="ad79a-111">Välj **Skapa en resurs** längst upp till vänster i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ad79a-111">Choose **Create a resource** in the upper left-hand corner of the Azure portal.</span></span>

3. <span data-ttu-id="ad79a-112">Välj **Sakernas Internet**, och välj sedan **IoT Hub**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-112">Select **Internet of Things**, and then select **IoT Hub**.</span></span>

![Skärmbild av navigering i Azure-portal till IoT Hub](../media/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. <span data-ttu-id="ad79a-114">I fönsterrutan **IoT-hubb** anger du följande information för IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="ad79a-114">In the **IoT hub** pane, enter the following information for your IoT hub:</span></span>

   - <span data-ttu-id="ad79a-115">**Prenumeration**: använd standard-prenumerationen för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ad79a-115">**Subscription**: Use the default subscription for this example.</span></span>
   - <span data-ttu-id="ad79a-116">**Resursgrupp**: använd den befintliga resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ad79a-116">**Resource group**: Use the existing resource group.</span></span> <span data-ttu-id="ad79a-117">Genom att lägga relaterade resurser i samma grupp så kan du hantera dem tillsammans.</span><span class="sxs-lookup"><span data-stu-id="ad79a-117">By putting all related resources in the same group you can manage them all together.</span></span> <span data-ttu-id="ad79a-118">Till exempel tas alla resurser som ingår i gruppen bort om resursgruppen tas bort.</span><span class="sxs-lookup"><span data-stu-id="ad79a-118">For example, deleting the resource group deletes all resources contained in that group.</span></span>
   - <span data-ttu-id="ad79a-119">**Namn**: Skapa ett unikt namn för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ad79a-119">**Name**: Create a unique name for your IoT hub.</span></span> <span data-ttu-id="ad79a-120">Om namnet som du anger är tillgängligt visas en grön bockmarkering.</span><span class="sxs-lookup"><span data-stu-id="ad79a-120">If the name you enter is available, a green check mark appears.</span></span>
   - <span data-ttu-id="ad79a-121">**Region**: Välj den plats som är närmast dig från följande lista.</span><span class="sxs-lookup"><span data-stu-id="ad79a-121">**Region**: Select the closest location to you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    > [!IMPORTANT]
    > <span data-ttu-id="ad79a-122">IoT-hubben kommer att kunna identifieras offentligt som en DNS-slutpunkt, så se till att undvika känslig information när du namnger det.</span><span class="sxs-lookup"><span data-stu-id="ad79a-122">The IoT hub will be publicly discoverable as a DNS endpoint, so make sure to avoid any sensitive information while naming it.</span></span>

    ![Fönster med grundläggande info om IoT Hub](./../media/dbb7319388673b8ee0e0b407536156c0.png)

1. <span data-ttu-id="ad79a-124">Välj **Nästa: Storlek och skalning** för att fortsätta att skapa IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="ad79a-124">Select **Next: Size and scale** to continue creating your IoT hub.</span></span>
2. <span data-ttu-id="ad79a-125">Välj **pris- och skalningsnivå**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-125">Choose your **Pricing and scale tier**.</span></span> <span data-ttu-id="ad79a-126">I det här exemplet väljer du nivån **F1 – kostnadsfri**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-126">For this example, select the **F1 - Free** tier.</span></span>

    ![Fönster för storlek och skala för IoT Hub](../media/b506eb3293fa4aa9d4785ad498fc476c.png)

3. <span data-ttu-id="ad79a-128">Välj **Granska + skapa**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-128">Select **Review + create**.</span></span>

4. <span data-ttu-id="ad79a-129">Gå igenom informationen om IoT-hubben och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-129">Review your IoT hub information, then click **Create**.</span></span> <span data-ttu-id="ad79a-130">Det kan ta några minuter tills IoT-hubben skapas.</span><span class="sxs-lookup"><span data-stu-id="ad79a-130">Your IoT hub might take a few minutes to create.</span></span> <span data-ttu-id="ad79a-131">Du kan övervaka förloppet i fönsterrutan **Meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-131">You can monitor the progress in the **Notifications** pane.</span></span>

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up exercise. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a><span data-ttu-id="ad79a-132">Registrera en enhet</span><span class="sxs-lookup"><span data-stu-id="ad79a-132">Register a device</span></span>
<span data-ttu-id="ad79a-133">En enhet måste vara registrerad vid din IoT-hubb innan den kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="ad79a-133">A device must be registered with your IoT hub before it can connect.</span></span>

1. <span data-ttu-id="ad79a-134">I navigeringsmenyn för din IoT-hubb öppnar du **IoT-enheter** och klickar på **Lägg till** för att registrera en enhet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ad79a-134">In your IoT hub navigation menu, open **IoT devices**, then click **Add** to register a device in your IoT hub.</span></span>

   ![Lägga till en enhet i IoT-enheter för IoT-hubben](../media/ee5f177abcf06b86dd007fce3b8448ad.png)

2. <span data-ttu-id="ad79a-136">Ange ett **enhets-ID** för den nya enheten.</span><span class="sxs-lookup"><span data-stu-id="ad79a-136">Enter a **Device ID** for the new device.</span></span> <span data-ttu-id="ad79a-137">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="ad79a-137">Device IDs are case sensitive.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ad79a-138">Enhets-ID visas kanske i de loggar som samlas in för support och felsökning, så se till att undvika känslig information när du namnger det.</span><span class="sxs-lookup"><span data-stu-id="ad79a-138">The device ID may be visible in the logs collected for customer support and troubleshooting, so make sure to avoid any sensitive information while naming it.</span></span>

3. <span data-ttu-id="ad79a-139">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-139">Click **Save**.</span></span>
4. <span data-ttu-id="ad79a-140">När enheten har skapats öppnar du enheten från listan i fönsterrutan **IoT-enheter**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-140">After the device is created, open the device from the list in the **IoT devices** pane.</span></span>
5. <span data-ttu-id="ad79a-141">Kopiera **Anslutningssträng – primärnyckel** eftersom du behöver den senare.</span><span class="sxs-lookup"><span data-stu-id="ad79a-141">Copy the **Connection string---primary key** to use later.</span></span>

   ![Hämta enhetens anslutningssträng](../media/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a><span data-ttu-id="ad79a-143">Skicka simulerad telemetri</span><span class="sxs-lookup"><span data-stu-id="ad79a-143">Send simulated telemetry</span></span>

1. <span data-ttu-id="ad79a-144">Öppna [Raspberry Pi Azure IoT-simulatorn](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="ad79a-144">Open the [Raspberry Pi Azure IoT Simulator](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).</span></span>
1. <span data-ttu-id="ad79a-145">Ersätt platshållaren på rad 15 med den anslutningssträng för Azure IoT Hub-enheten du just kopierade.</span><span class="sxs-lookup"><span data-stu-id="ad79a-145">Replace the placeholder in Line 15 with the Azure IoT hub device connection string you just copied.</span></span>
1. <span data-ttu-id="ad79a-146">Klicka på `Run`-knappen eller skriv in `npm start` i konsolfönstret för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="ad79a-146">Click the `Run` button or type `npm start` in the console window to run the application.</span></span>

    ![Ersätt enhetens anslutningssträng](../media/Line15.png)

1. <span data-ttu-id="ad79a-148">Du bör se följande utdata som visar de sensordata och de meddelanden som skickas till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ad79a-148">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

    ![Resultat – sensordata som skickas från Raspberry Pi till din IoT-hubb](../media/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a><span data-ttu-id="ad79a-150">Läs telemetrin från din hubb</span><span class="sxs-lookup"><span data-stu-id="ad79a-150">Read the telemetry from your hub</span></span>
<span data-ttu-id="ad79a-151">Så vad är det som händer?</span><span class="sxs-lookup"><span data-stu-id="ad79a-151">So what's happening?</span></span> <span data-ttu-id="ad79a-152">IoT-hubben tar emot enhet-till-moln-meddelanden som skickas från den simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="ad79a-152">IoT hub is receiving the device-to-cloud messages sent from the simulated device.</span></span> <span data-ttu-id="ad79a-153">Om du vill se det, tar vi en titt på hur Azure IoT Hub bearbetar inkommande data.</span><span class="sxs-lookup"><span data-stu-id="ad79a-153">In order to see that, let's take a quick look at how Azure IoT Hub is processing the incoming data.</span></span> <span data-ttu-id="ad79a-154">I din IoT Hub under **övervakning**, väljer du **mått**.</span><span class="sxs-lookup"><span data-stu-id="ad79a-154">In your IoT Hub, under **Monitoring**, select **Metrics**.</span></span> <span data-ttu-id="ad79a-155">Ge det ett par minuter som du vill vänta tills data kommer in i bilden.</span><span class="sxs-lookup"><span data-stu-id="ad79a-155">Give it a few minutes as you wait for the data to come into the picture.</span></span>

![IoT Hub-mått](../media/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
