Raspberry Pi-tavlor har fått mycket uppmärksamhet på sistone för att testa teorier eller för att skapa häftiga saker. Kostnaden för de här korten är låga, men vissa kan ändå vara intresserade av att testa funktionen hos Raspberry Pi innan de investerar i en.

Microsoft har skapat en [Raspberry Pi Azure IoT-simulator](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true) online som låter användare kontrollera den emulerade maskinvaran via kod. Emulatorn visar en bild av en Raspberry Pi som är ansluten till en sensor för temperatur, fuktighet och tryck samt en röd LED via kopplingsdäck så att kretsarna vara kopplas ihop. Den sidopanel som visas gör att användare kan ange Node.js JavaScript-kod för att styra LED:n och samla in testdata från den simulerade sensorn.

![Raspberry Pi-simulatorn](../media/RaspberryPiSimulator.png)

## <a name="raspberry-pi-azure-iot-online-simulator"></a>Raspberry Pi Azure IoT Onlinesimulator

Vid den första körningen kör simulatorn ett exempelprogram för temperaturavläsning som visas via kommandoraden. Samma exempelprogram kan även köras på en verklig Pi eftersom simulatorn är utformad så att användare kan testa kod innan de överför den till en riktig enhet.

Det finns tre områden i webbsimulatorn:

1. **Sammansättningsområde**. Det här är där du kan se din enhets status. Som standard är det en Pi som ansluter med en BME280-sensor och ett LED-ljus. Den här konfigurationen är inte anpassningsbar just nu.
2. **Kodområde**. En kodredigerare online så att du kan skapa en app på Raspberry Pi med Node.js. Standardexempelprogrammet hjälper till att samla in sensordata från BME280-sensorn och skickar det till din Azure IoT Hub.
3. **Integrerat konsolfönster**. Det är här du kan se utdata från din app. Det finns tre funktioner i konsolen:
    - `run` – Kör exempelkoden (när exemplet körs så är koden skrivskyddad).
    - `Stop` – Stoppar exempelkoden som körs.
    - `Reset` – Återställer koden.

Nu när du har en översikt över Raspberry Pi-simulatorn så kommer vi att utforska IoT Hub i Azure där du kommer att skapa en ny resurs för att hämta data från simulatorn.

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>
-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->