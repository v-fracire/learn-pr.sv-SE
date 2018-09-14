Raspberry Pi tavlor har garnered mycket intresse av sen för att testa basreaktionsteorier eller händelse gör coola saker. Kostnaden på den här tavlor är mycket liten, kan vissa vara intressant att testa funktionen Raspberry Pi innan du investerar i en.

Microsoft har byggt en online [Raspberry Pi Azure IoT Simulator](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true) så att användarna kan styra emulerade maskinvaran via kod. Emulatorn visar en bild av en Raspberry Pi som är anslutna till en temperatur, fuktighet, tryck sensorn och en röd LAMPAN via breadboard så att kretsar vara kabelanslutna tillsammans. Panelen visas sida kan du ange Node.js JavaScript-kod för att styra LAMPAN och samla in dummy data från den simulerade sensorn.

![Raspberry Pi Simulator](../media-draft/RaspberryPiSimulator.png)

## <a name="raspberry-pi-azure-iot-online-simulator"></a>Raspberry Pi Azure IoT-Onlinesimulator

Vid den första körningen fungerar simulatorn en temperatur capture exempelprogrammet som visas via kommandoraden. Samma exempelprogrammet kan också köras på en verklig Pi som simulatorn är utformat så att användare kan testa kod innan de överförs till en riktig enhet.

Det finns tre områden i web-simulatorn:

1. **Sammansättningen området**. Det här är där du kan se enhetens status. Som standard är detta en Pi ansluter med en BME280 sensor och en LED ljus. Den här konfigurationen är inte anpassningsbara just nu.
2. **Koda området**. Kodredigerare online att skapa en app på Raspberry Pi med Node.js. Standard-exempelprogrammet hjälper dig att samla in sensordata från BME280 sensorn och skickar det till Azure IoT Hub.
3. **Integrerad konsolfönstret**. Det här är där du kan se utdata från din app. Det finns tre funktioner i konsolen:
    - `run` -Kör exempelkoden (när exemplet körs koden är skrivskyddad).
    - `Stop` -Stoppar exempelkoden som körs.
    - `Reset` -Återställer koden.

Nu när du har en översikt över Raspberry Pi-simulatorn vi kommer att utforska IoT Hub i Azure där du skapar en ny resurs att hämta data från simulatorn.

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>
-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->