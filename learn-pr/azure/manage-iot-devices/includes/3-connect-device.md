<span data-ttu-id="0de14-101">Att samla in väderdata är en viktig uppgift eftersom vädret kan påverka allt från trafikmönster till hur HVAC-system inom detaljhandeln används.</span><span class="sxs-lookup"><span data-stu-id="0de14-101">Capturing of weather data is an important task as weather can effect everything from traffic patterns to how HVAC systems in retail stores are operated.</span></span> <span data-ttu-id="0de14-102">I den här övningen använder du Raspberry Pi-onlinesimulatorn för att samla in simulerade väderdata och samla in dessa data via Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="0de14-102">In this exercise, you will be harnessing the online Raspberry Pi simulator to capture simulated weather data and capture said data via the Azure IoT Hub.</span></span>

<span data-ttu-id="0de14-103">Den här övningen körs i en simulerad miljö, men det program som körs på den simulerade enheten kan överföras till en verklig enhet i framtiden.</span><span class="sxs-lookup"><span data-stu-id="0de14-103">While this exercise is being conducted in a simulated environment, the application running on the simulated device can be transferred to a real device in future.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="0de14-104">Skapa en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="0de14-104">Create an IoT hub</span></span>

1. <span data-ttu-id="0de14-105">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0de14-105">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>

1. <span data-ttu-id="0de14-106">Välj **Skapa en resurs** \> **Sakernas internet** \> **IoT Hub**.</span><span class="sxs-lookup"><span data-stu-id="0de14-106">Select **Create a resource** \> **Internet of Things** \> **IoT Hub**.</span></span>

![Skärmbild av navigering i Azure-portal till IoT Hub](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

1. <span data-ttu-id="0de14-108">I fönsterrutan **IoT-hubb** anger du följande information för IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="0de14-108">In the **IoT hub** pane, enter the following information for your IoT hub:</span></span>

 - <span data-ttu-id="0de14-109">**Prenumeration**: Välj den prenumeration som du vill använda för att skapa IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="0de14-109">**Subscription**: Choose the subscription that you want to use to create this IoT hub.</span></span>
 - <span data-ttu-id="0de14-110">**Resursgrupp**: Skapa en resursgrupp som ska vara värd för IoT-hubben eller använd en befintlig.</span><span class="sxs-lookup"><span data-stu-id="0de14-110">**Resource group**: Create a resource group to host the IoT hub or use an existing one.</span></span> <span data-ttu-id="0de14-111">Mer information finns i [Använda resursgrupper för att hantera Azure-resurser](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).</span><span class="sxs-lookup"><span data-stu-id="0de14-111">For more information, see [Use resource groups to manage your Azure resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).</span></span>
 - <span data-ttu-id="0de14-112">**Region**: Välj den plats som är närmast dig.</span><span class="sxs-lookup"><span data-stu-id="0de14-112">**Region**: Select the closest location to you.</span></span>
 - <span data-ttu-id="0de14-113">**Namn**: Skapa ett namn för IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="0de14-113">**Name**: Create a name for your IoT hub.</span></span> <span data-ttu-id="0de14-114">Om namnet som du anger är tillgängligt visas en grön bockmarkering.</span><span class="sxs-lookup"><span data-stu-id="0de14-114">If the name you enter is available, a green check mark appears.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0de14-115">IoT-hubben kan identifieras offentligt som en DNS-slutpunkt, så se till att undvika känslig information när du namnger det.</span><span class="sxs-lookup"><span data-stu-id="0de14-115">The IoT hub will be publicly discoverable as a DNS endpoint, so make sure to avoid any sensitive information while naming it.</span></span>

   ![Fönster med grundläggande info om IoT Hub](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

1.  <span data-ttu-id="0de14-117">Välj **Nästa: Storlek och skalning** för att fortsätta att skapa IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="0de14-117">Select **Next: Size and scale** to continue creating your IoT hub.</span></span>

1.  <span data-ttu-id="0de14-118">Välj **pris- och skalningsnivå**.</span><span class="sxs-lookup"><span data-stu-id="0de14-118">Choose your **Pricing and scale tier**.</span></span> <span data-ttu-id="0de14-119">I den här artikeln väljer du nivån **F1 – kostnadsfri** om den nivån fortfarande är tillgänglig för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0de14-119">For this article, select the **F1 - Free** tier if it's still available on your subscription.</span></span> <span data-ttu-id="0de14-120">Mer information finns i avsnittet om [pris- och skalnivåer](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="0de14-120">For more information, see the [Pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

   ![Fönster för storlek och skala för IoT Hub](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

1.  <span data-ttu-id="0de14-122">Välj **Granska + skapa**.</span><span class="sxs-lookup"><span data-stu-id="0de14-122">Select **Review + create**.</span></span>

1.  <span data-ttu-id="0de14-123">Gå igenom informationen om IoT-hubben och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0de14-123">Review your IoT hub information, then click **Create**.</span></span> <span data-ttu-id="0de14-124">Det kan ta några minuter tills IoT-hubben skapas.</span><span class="sxs-lookup"><span data-stu-id="0de14-124">Your IoT hub might take a few minutes to create.</span></span> <span data-ttu-id="0de14-125">Du kan övervaka förloppet i fönsterrutan **Meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="0de14-125">You can monitor the progress in the **Notifications** pane.</span></span>

<span data-ttu-id="0de14-126">Nu när du har skapat en IoT-hubb letar du upp viktig information som du använder för att ansluta enheter och program till IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="0de14-126">Now that you have created an IoT hub, locate the important information that you use to connect devices and applications to your IoT hub.</span></span>

<span data-ttu-id="0de14-127">I navigeringsmenyn för IoT-hubben öppnar du **Principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="0de14-127">In your IoT hub navigation menu, open **Shared access policies**.</span></span> <span data-ttu-id="0de14-128">Välj principen **iothubowner** och kopiera sedan **Anslutningssträng---primärnyckel** för IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="0de14-128">Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub.</span></span> <span data-ttu-id="0de14-129">Mer information finns i [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security) (Kontrollera åtkomsten till IoT Hub).</span><span class="sxs-lookup"><span data-stu-id="0de14-129">For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).</span></span>

> [!NOTE]
> <span data-ttu-id="0de14-130">Du behöver inte den här iothubowner-anslutningssträngen i den här konfigurationskursen.</span><span class="sxs-lookup"><span data-stu-id="0de14-130">You do not need this iothubowner connection string for this set-up tutorial.</span></span> <span data-ttu-id="0de14-131">Men du kan behöva den i vissa självstudiekurser eller olika IoT-scenarier när du har slutfört den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0de14-131">However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.</span></span>

![Hämta IoT-hubbens anslutningssträng](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a><span data-ttu-id="0de14-133">Registrera en enhet i IoT-hubben för din enhet</span><span class="sxs-lookup"><span data-stu-id="0de14-133">Register a device in the IoT hub for your device</span></span>
------------------------------------------------

1.  <span data-ttu-id="0de14-134">I navigeringsmenyn för din IoT-hubb öppnar du **IoT-enheter** och klickar på **Lägg till** för att registrera en enhet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0de14-134">In your IoT hub navigation menu, open **IoT devices**, then click **Add** to register a device in your IoT hub.</span></span>

   ![Lägga till en enhet i IoT-enheter för IoT-hubben](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

1.  <span data-ttu-id="0de14-136">Ange ett **enhets-ID** för den nya enheten.</span><span class="sxs-lookup"><span data-stu-id="0de14-136">Enter a **Device ID** for the new device.</span></span> <span data-ttu-id="0de14-137">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="0de14-137">Device IDs are case sensitive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0de14-138">Enhets-ID visas kanske i de loggar som samlas in för support och felsökning, så se till att undvika känslig information när du namnger det.</span><span class="sxs-lookup"><span data-stu-id="0de14-138">The device ID may be visible in the logs collected for customer support and troubleshooting, so make sure to avoid any sensitive information while naming it.</span></span>

1.  <span data-ttu-id="0de14-139">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0de14-139">Click **Save**.</span></span>

1.  <span data-ttu-id="0de14-140">När enheten har skapats öppnar du enheten från listan i fönsterrutan **IoT-enheter**.</span><span class="sxs-lookup"><span data-stu-id="0de14-140">After the device is created, open the device from the list in the **IoT devices** pane.</span></span>

1.  <span data-ttu-id="0de14-141">Kopiera **Anslutningssträng – primärnyckel** eftersom du behöver den senare.</span><span class="sxs-lookup"><span data-stu-id="0de14-141">Copy the **Connection string---primary key** to use later.</span></span>

   ![Hämta enhetens anslutningssträng](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="run-a-sample-application-on-pi-web-simulator"></a><span data-ttu-id="0de14-143">Köra ett exempelprogram på Pi-webbsimulatorn</span><span class="sxs-lookup"><span data-stu-id="0de14-143">Run a sample application on Pi web simulator</span></span>

1. <span data-ttu-id="0de14-144">I kodningsområdet ser du till att du arbetar med det standardmässiga exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="0de14-144">In coding area, make sure you are working on the default sample application.</span></span> <span data-ttu-id="0de14-145">Ersätt platshållaren på rad 15 med Azure IoT-hubbens anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="0de14-145">Replace the placeholder in Line 15 with the Azure IoT hub device connection string.</span></span>

    ![Ersätta enhetens anslutningssträng](../media-draft/92ea2c31d42f5b939fb5512e7220e957.png)

2.  <span data-ttu-id="0de14-147">Klicka på **Kör** eller skriv npm start för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="0de14-147">Click **Run** or type npm start to run the application.</span></span>

<span data-ttu-id="0de14-148">Du bör se följande utdata som visar de sensordata och de meddelanden som skickas till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="0de14-148">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub</span></span>

   ![Resultat – sensordata som skickas från Raspberry Pi till din IoT-hubb](../media-draft/96b28d30e317b04347abb0d613738117.png)

<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
