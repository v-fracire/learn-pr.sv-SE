Avbildning av väderdata är en viktig uppgift som väder kan påverka allt från trafikmönster till hur uppvärmning, ventilation och luftkonditionering (HVAC) system i butiker används. I den här övningen kommer du att samverka med online Raspberry Pi simulatorn lärde du dig i den föregående enheten till avbildning simulerade weather-data och via Azure IoT Hub.

Även om den här övningen körs i en simulerad miljö, kan det program som körs på den simulerade enheten överföras till en riktig enhet i framtiden.

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

## <a name="create-an-iot-hub"></a>Skapa en IoT Hub
Azure IoT Hub innehåller funktionerna och en modell för utökningsbarhet som gör det möjligt enhets- och backend-utvecklare att bygga robusta lösningar för enhetshantering. Enheter är allt från begränsade sensorer och enskilda mikrostyrenheter för ett särskilt ändamål till kraftfulla gateways som dirigerar kommunikationen för grupper av enheter. Dessutom varierar användningsfall och krav för IoT-operatörer avsevärt mellan olika branscher. Trots variationen ger enhetshantering med IoT Hub funktioner, mönster och kodbibliotek för att serva en mängd olika enheter och slutanvändare.

För att börja samla in data från Raspberry Pi-simulator, måste du först skapa en IoT-hubb.

1. Öppna [Azure-portalen](https://portal.azure.com?azure-portal=true).
2. Välj **Skapa en resurs** längst upp till vänster i Azure Portal.
3. Välj **Internet of Things**, och välj sedan **IoT Hub**.

![Skärmbild av Azure-portalnavigering till IoT Hub](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. I rutan **IoT-hubb** anger du följande information för IoT-hubben:
   
   **Prenumeration**: använda standard-prenumerationen för det här exemplet.
   **Resursgrupp**: Skapa en resursgrupp som ska innehålla IoT-hubben eller använd en befintlig. Genom att lägga till alla relaterade resurser i en grupp, till exempel *TestResources*, kan du hantera dem allihop tillsammans. Till exempel tas alla resurser som ingår i gruppen bort om resursgruppen tas bort.
   **Region**: Välj den plats som är närmast dig.
   **Namn**: Skapa ett unikt namn för din IoT-hubb. Om namnet som du anger är tillgängligt visas en grön bockmarkering.

> [!IMPORTANT]
> IoT-hubben kan identifieras offentligt som en DNS-slutpunkt, så se till att undvika känslig information när du namnger det.

   ![Fönster med grundläggande IoT Hub-inställningar](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

5. Välj **Nästa: Storlek och skalning** för att fortsätta att skapa IoT-hubben.
6. Välj **pris- och skalningsnivå**. Det här exemplet väljer du den **F1 – kostnadsfri** nivå.

   ![Fönster för IoT Hub-storlek och IoT Hub-skalning](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

7. Välj **Granska + skapa**.

8. Gå igenom informationen om IoT-hubben och klicka sedan på **Skapa**. Det kan ta några minuter innan IoT-hubben har skapats. Du kan övervaka förloppet i **meddelandefönstret**.

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up tutorial. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a>Registrera en enhet
En enhet måste vara registrerad vid din IoT-hubb innan den kan ansluta.

1. Öppna i navigeringsmenyn din IoT-hubb **IoT-enheter**, klicka sedan på **Lägg till** att registrera en enhet i IoT hub.

   ![Lägg till en enhet i IoT-enheter för IoT-hubben](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

2. Ange en **enhets-ID** för den nya enheten. Enhets-ID är skiftlägeskänsliga.

> [!IMPORTANT]
> Enhets-ID kanske visas i loggarna som samlas in för support och felsökning, så se till att undvika känslig information när du namnger det.

3. Klicka på **Spara**.
4. När enheten har skapats öppnar du enheten i listan i den **IoT-enheter** fönstret.
5. Kopiera den **anslutningssträngen---primärnyckel** för senare användning.

   ![Hämta enhetens anslutningssträng](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a>Skicka simulerad telemetri

1. Öppna den [Raspberry Pi Azure IoT-simulatorn](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).
2. Ersätt platshållaren i rad 15 med Azure IoT hub enhetens anslutningssträng du kopierade.
3. Klicka på den `Run` knapp eller typ `npm start` i konsolfönstret för att köra programmet.
   
   ![Ersätt enhetens anslutningssträng](../media-draft/Line15.png)

Du bör se följande utdata som visar sensordata och meddelanden som skickas till din IoT hub

![Resultat – sensordata som skickas från Raspberry Pi till din IoT-hubb](../media-draft/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a>Läsa telemetrin från din hubb
 Så vad som händer? IoT-hubb tar emot enhet-till-moln-meddelanden som skickas från den simulerade enheten. Om du vill se att, låt oss ta en titt på hur Azure IoT Hub bearbetar inkommande data. I din IoT-hubb under **övervakning**väljer **mått**. Ge det ett par minuter som du vill vänta tills data kommer in i bilden.
   
   ![IoT Hub-mått](../media-draft/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
