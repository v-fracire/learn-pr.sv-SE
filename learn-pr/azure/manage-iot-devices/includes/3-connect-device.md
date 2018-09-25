Att samla in väderdata är en viktig uppgift eftersom vädret kan påverka allt från trafikmönster till hur HVAC-system (värme, ventilation och luftkonditionering) inom detaljhandeln används. I den här övningen interagerar du med Raspberry Pi-onlinesimulatorn som du lärde känna i den tidigare enheten för att samla in simulerade väderdata och via Azure IoT Hub.

[!include[](../../../includes/azure-sandbox-activate.md)]

Den här övningen körs i en simulerad miljö, men det program som körs på den simulerade enheten kan överföras till en verklig enhet i framtiden.

## <a name="create-an-iot-hub"></a>Skapa en IoT-hubb
Azure IoT Hub innehåller funktionerna och en modell för utökningsbarhet som gör det möjligt enhets- och backend-utvecklare att bygga robusta lösningar för enhetshantering. Enheter är allt från begränsade sensorer och enskilda mikrostyrenheter för ett särskilt ändamål till kraftfulla gateways som dirigerar kommunikationen för grupper av enheter. Dessutom varierar användningsfall och krav för IoT-operatörer avsevärt mellan olika branscher. Trots variationen ger enhetshantering med IoT Hub funktioner, mönster och kodbibliotek för att serva en mängd olika enheter och slutanvändare.

För att börja samla in data från Raspberry Pi-simulatorn så måste du först skapa en IoT-hubb.

1. Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

2. Välj **Skapa en resurs** längst upp till vänster i Azure Portal.

3. Välj **Sakernas Internet**, och välj sedan **IoT Hub**.

![Skärmbild av navigering i Azure-portal till IoT Hub](../media/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. I fönsterrutan **IoT-hubb** anger du följande information för IoT-hubben:

   - **Prenumeration**: använd standard-prenumerationen för det här exemplet.
   - **Resursgrupp**: använd den befintliga resursgruppen. Genom att lägga relaterade resurser i samma grupp så kan du hantera dem tillsammans. Till exempel tas alla resurser som ingår i gruppen bort om resursgruppen tas bort.
   - **Namn**: Skapa ett unikt namn för din IoT-hubb. Om namnet som du anger är tillgängligt visas en grön bockmarkering.
   - **Region**: Välj den plats som är närmast dig från följande lista.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    > [!IMPORTANT]
    > IoT-hubben kommer att kunna identifieras offentligt som en DNS-slutpunkt, så se till att undvika känslig information när du namnger det.

    ![Fönster med grundläggande info om IoT Hub](./../media/dbb7319388673b8ee0e0b407536156c0.png)

1. Välj **Nästa: Storlek och skalning** för att fortsätta att skapa IoT-hubben.
2. Välj **pris- och skalningsnivå**. I det här exemplet väljer du nivån **F1 – kostnadsfri**.

    ![Fönster för storlek och skala för IoT Hub](../media/b506eb3293fa4aa9d4785ad498fc476c.png)

3. Välj **Granska + skapa**.

4. Gå igenom informationen om IoT-hubben och klicka sedan på **Skapa**. Det kan ta några minuter tills IoT-hubben skapas. Du kan övervaka förloppet i fönsterrutan **Meddelanden**.

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up exercise. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a>Registrera en enhet
En enhet måste vara registrerad vid din IoT-hubb innan den kan ansluta.

1. I navigeringsmenyn för din IoT-hubb öppnar du **IoT-enheter** och klickar på **Lägg till** för att registrera en enhet i din IoT-hubb.

   ![Lägga till en enhet i IoT-enheter för IoT-hubben](../media/ee5f177abcf06b86dd007fce3b8448ad.png)

2. Ange ett **enhets-ID** för den nya enheten. Enhets-ID är skiftlägeskänsliga.

    > [!IMPORTANT]
    > Enhets-ID visas kanske i de loggar som samlas in för support och felsökning, så se till att undvika känslig information när du namnger det.

3. Klicka på **Spara**.
4. När enheten har skapats öppnar du enheten från listan i fönsterrutan **IoT-enheter**.
5. Kopiera **Anslutningssträng – primärnyckel** eftersom du behöver den senare.

   ![Hämta enhetens anslutningssträng](../media/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a>Skicka simulerad telemetri

1. Öppna [Raspberry Pi Azure IoT-simulatorn](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).
1. Ersätt platshållaren på rad 15 med den anslutningssträng för Azure IoT Hub-enheten du just kopierade.
1. Klicka på `Run`-knappen eller skriv in `npm start` i konsolfönstret för att köra programmet.

    ![Ersätt enhetens anslutningssträng](../media/Line15.png)

1. Du bör se följande utdata som visar de sensordata och de meddelanden som skickas till din IoT-hubb.

    ![Resultat – sensordata som skickas från Raspberry Pi till din IoT-hubb](../media/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a>Läs telemetrin från din hubb
Så vad är det som händer? IoT-hubben tar emot enhet-till-moln-meddelanden som skickas från den simulerade enheten. Om du vill se det, tar vi en titt på hur Azure IoT Hub bearbetar inkommande data. I din IoT Hub under **övervakning**, väljer du **mått**. Ge det ett par minuter som du vill vänta tills data kommer in i bilden.

![IoT Hub-mått](../media/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
