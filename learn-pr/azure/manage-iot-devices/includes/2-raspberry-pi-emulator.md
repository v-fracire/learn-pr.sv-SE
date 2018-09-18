Raspberry Pi-tavlor har fått mycket uppmärksamhet på sistone för att testa teorier eller för att skapa häftiga saker. Kostnaden för de här tavlorna är låg, men vissa kan ändå vara intresserade av att testa funktionen hos Raspberry Pi innan de investerar i en.

Microsoft har byggt en Raspberry Pi-onlinesimulator så att användarna kan styra emulerade maskinvaran via kod. Emulatorn visar en bild av en Raspberry Pi som är ansluten till en sensor för temperatur, fuktighet och tryck samt en röd LED via kopplingsdäck så att kretsarna vara kopplas ihop. Den sidopanel som visas gör att användare kan ange Node.js JavaScript-kod för att styra LED:n och samla in testdata från den simulerade sensorn.

Vid den första körningen kör simulatorn ett exempelprogram för temperaturavläsning som visas via kommandoraden. Samma exempelprogram kan även köras på en verklig Pi eftersom simulatorn är utformad så att användare kan testa kod innan de överför den till en riktig enhet.

## <a name="web-simulator"></a>Webbsimulator

Det finns tre områden i webbsimulatorn:

1.  Sammansättningsområdeet – standardkretsen är att en Pi ansluts till en BME280-sensor och en LED. Området är låst i förhandsversionen, så för tillfället går det inte att göra anpassningar.

2.  Kodningsområdet – en onlinekodredigerare så att du kan köra med Raspberry Pi. Standardexempelprogrammet hjälper till att samla in sensordata från BME280-sensorn och skickar till din Azure IoT Hub. Programmet är helt kompatibelt med verkliga Pi-enheter.

3.  Integrerat konsolfönster – visar utdata för din kod. Längst upp i det här fönstret finns tre knappar.

    -   **Run** (Kör) – kör programmet i kodningsområdet.

    -   **Reset** (Återställ) – återställ kodningsområdet till standardexempelprogrammet.

    -   **Fold/Expand** (Vik/expandera) – på höger sida finns det en knapp som viker/expanderar konsolfönstret.

[Översiktsbild av Pi-onlinesimulatorn](./../media-draft/image1.png)

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>

-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->

