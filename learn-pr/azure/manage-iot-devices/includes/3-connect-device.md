Att samla in väderdata är en viktig uppgift eftersom vädret kan påverka allt från trafikmönster till hur HVAC-system inom detaljhandeln används. I den här övningen använder du Raspberry Pi-onlinesimulatorn för att samla in simulerade väderdata och samla in dessa data via Azure IoT Hub.

Den här övningen körs i en simulerad miljö, men det program som körs på den simulerade enheten kan överföras till en verklig enhet i framtiden.

## <a name="create-an-iot-hub"></a>Skapa en IoT-hubb

1. Logga in på [Azure-portalen](https://portal.azure.com/).

1. Välj **Skapa en resurs** \> **Sakernas internet** \> **IoT Hub**.

![Skärmbild av navigering i Azure-portal till IoT Hub](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

1. I fönsterrutan **IoT-hubb** anger du följande information för IoT-hubben:

 - **Prenumeration**: Välj den prenumeration som du vill använda för att skapa IoT-hubben.
 - **Resursgrupp**: Skapa en resursgrupp som ska vara värd för IoT-hubben eller använd en befintlig. Mer information finns i [Använda resursgrupper för att hantera Azure-resurser](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
 - **Region**: Välj den plats som är närmast dig.
 - **Namn**: Skapa ett namn för IoT-hubben. Om namnet som du anger är tillgängligt visas en grön bockmarkering.

> [!IMPORTANT]
> IoT-hubben kan identifieras offentligt som en DNS-slutpunkt, så se till att undvika känslig information när du namnger det.

   ![Fönster med grundläggande info om IoT Hub](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

1.  Välj **Nästa: Storlek och skalning** för att fortsätta att skapa IoT-hubben.

1.  Välj **pris- och skalningsnivå**. I den här artikeln väljer du nivån **F1 – kostnadsfri** om den nivån fortfarande är tillgänglig för din prenumeration. Mer information finns i avsnittet om [pris- och skalnivåer](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![Fönster för storlek och skala för IoT Hub](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

1.  Välj **Granska + skapa**.

1.  Gå igenom informationen om IoT-hubben och klicka sedan på **Skapa**. Det kan ta några minuter tills IoT-hubben skapas. Du kan övervaka förloppet i fönsterrutan **Meddelanden**.

Nu när du har skapat en IoT-hubb letar du upp viktig information som du använder för att ansluta enheter och program till IoT-hubben.

I navigeringsmenyn för IoT-hubben öppnar du **Principer för delad åtkomst**. Välj principen **iothubowner** och kopiera sedan **Anslutningssträng---primärnyckel** för IoT-hubben. Mer information finns i [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security) (Kontrollera åtkomsten till IoT Hub).

> [!NOTE]
> Du behöver inte den här iothubowner-anslutningssträngen i den här konfigurationskursen. Men du kan behöva den i vissa självstudiekurser eller olika IoT-scenarier när du har slutfört den här konfigurationen.

![Hämta IoT-hubbens anslutningssträng](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Registrera en enhet i IoT-hubben för din enhet
------------------------------------------------

1.  I navigeringsmenyn för din IoT-hubb öppnar du **IoT-enheter** och klickar på **Lägg till** för att registrera en enhet i din IoT-hubb.

   ![Lägga till en enhet i IoT-enheter för IoT-hubben](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

1.  Ange ett **enhets-ID** för den nya enheten. Enhets-ID är skiftlägeskänsliga.

> [!IMPORTANT]
> Enhets-ID visas kanske i de loggar som samlas in för support och felsökning, så se till att undvika känslig information när du namnger det.

1.  Klicka på **Spara**.

1.  När enheten har skapats öppnar du enheten från listan i fönsterrutan **IoT-enheter**.

1.  Kopiera **Anslutningssträng – primärnyckel** eftersom du behöver den senare.

   ![Hämta enhetens anslutningssträng](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="run-a-sample-application-on-pi-web-simulator"></a>Köra ett exempelprogram på Pi-webbsimulatorn

1. I kodningsområdet ser du till att du arbetar med det standardmässiga exempelprogrammet. Ersätt platshållaren på rad 15 med Azure IoT-hubbens anslutningssträng.

    ![Ersätta enhetens anslutningssträng](../media-draft/92ea2c31d42f5b939fb5512e7220e957.png)

2.  Klicka på **Kör** eller skriv npm start för att köra programmet.

Du bör se följande utdata som visar de sensordata och de meddelanden som skickas till din IoT-hubb

   ![Resultat – sensordata som skickas från Raspberry Pi till din IoT-hubb](../media-draft/96b28d30e317b04347abb0d613738117.png)

<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
