I Azure IoT Central anges de data som en enhet kan utbyta med ditt program i en enhetsmall som definierar beteendet för enheten – i det här fallet en kaffebryggare. När du skapar en enhetsmall genereras en simulerad enhet från mallen. Den simulerade enheten genererar telemetri som gör att du kan testa programmets beteende innan du ansluter en fysisk enhet. 

I den här lektionen skapar du en enhetsmall för en kaffebryggare som anger följande funktioner och beteenden:
* **Mätning**: data som kommer från din enhet. Du kan lägga till flera mått i enhetsmallen som matchar enhetens funktioner.
    * Telemetrimått: de numeriska datapunkter som enheten samlar in över tid. De representeras som en kontinuerlig ström. I det här scenariot gäller telemetrimätningarna luftfuktighet och vattentemperatur. 

    * Tillståndsmått: enhetens eller vissa komponenters tillstånd sett över tid. I det här scenariot ställer du in tillstånden Brewing/Not Brewing och Cup Detected/Cup Not Detected.

* **Inställningar**: du använder inställningar till att skicka konfigurationsdata till en enhet från programmet. I det här scenariot justerar du optimal vattentemperatur i inställningarna och skickar inställningen till kaffebryggaren. När inställningen ändras markeras den som väntande i användargränssnittet tills enheten bekräftar att den har bearbetat inställningsändringen.

* **Egenskaper**: metadata som hör till enheten. Det finns två typer av egenskaper.
    * Du använder *programegenskaper* till att registrera information om enheten i programmet. I det här scenariot använder du programegenskaper till att ange kaffebryggarens optimala intervall för vattentemperatur. Programegenskaper lagras i programmet och synkroniseras inte med enheten. 

    * Du använder *enhetsegenskaper* så att enheten ska kunna skicka egenskapsvärden till programmet. De här egenskaperna kan bara ändras av enheten. I det här scenariot konfigurerar du en enhetsegenskap med namnet Device Warranty Expired i IoT Central. Fältet Device Warranty Expired förblir tomt tills kaffebryggaren ansluts till IoT Central. När den är ansluten skickar kaffebryggaren sin garantistatus till programmet. 

* **Kommandon**: du använder kommandon till att fjärrhantera din enhet från programmet. Du kan köra styrkommandon direkt på enheten från molnet. I det här scenariot kör du kommandon på kaffebryggaren för att sätta den i underhållsläge eller starta en bryggningscykel. 

## <a name="create-a-device-template-for-the-coffee-maker"></a>Skapa en enhetsmall för kaffebryggaren
En enhetsmall definierar beteendet och funktionen hos en enhet, i det här fallet en kaffebryggare.

1. Navigera till startsidan och välj **Skapa enhetsmallar**.

1. Ange *Connected Coffee Maker* för din anpassade enhetsmall. 
 
1. Välj **Skapa**. Du har nu skapat en tom enhetsmall för kaffebryggaren där du ska definiera bryggarens beteende och funktioner. 

## <a name="define-telemetry-measurement-temperature-and-humidity"></a>Definiera telemetrimätningar för temperatur och luftfuktighet
1.  I enhetsmallen **Connected Coffee Maker** kontrollerar du att du är på sidan **Mått**, där du definierar telemetrin. 

1.  För att lägga till temperaturmåttet väljer du **Nytt mått**. Välj sedan **Telemetri** som måttenhet.

1.  Varje typ av telemetri du definierar för en enhetsmall innehåller konfigurationsalternativ som:
    * Visningsalternativ.
    * Information om telemetrin.
    * Simuleringsparametrar.

    För att konfigurera telemetrin för temperatur och luftfuktighet använder du informationen i följande tabell: När du skapar telemetriposterna måste du lägga till ett nytt mått genom att välja **+ Nytt mått** för varje post i tabellen.
    
    |Visningsnamn|Fältnamn|Enheter|Min|Max|Antal decimaler|
    |---|---|---|---|---|---|
    |Water Temperature|waterTemperature|Celsius|86|100|1|
    |Air Humidity|airHumidity|%|20|100|0|
   
    Du kan även välja en färg för telemetrivisningen. För att spara telemetridefinition väljer du **Spara**. När du skapar fler definitioner av mått, inställningar, egenskaper och kommandon för enheten ska du komma ihåg att spara efter hand.  
    
    ![Skapa en enhetsmall](../images/2-device-template-a.png)

    Ange fältnamnen i enhetsmallen precis som de visas i tabellen. Om fältnamnen inte matchar egenskapsnamnen i motsvarande enhetskod kan telemetridata inte visas i programmet. Gör på samma sätt när du anger inställningar och egenskaper. 

## <a name="define-state-measurement-for-brewingnot-brewing-cup-detectedcup-not-detected"></a>Definiera tillståndsmåtten Brewing/Not Brewing och Cup Detected/Cup Not Detected
Lägg till följande tillstånd på sidan **Mått** genom att välja **+ Nytt mått**. Välj sedan **Tillstånd** som måttyp:
    
   |Visningsnamn|Fältnamn|Värde 1|Visningsnamn 1|Värde 2|Visningsnamn 2|
   |---|---|---|---|---|---|
   |Brewing|stateBrewing|true|Brewing|false|Not Brewing|
   |Cup Detected|stateCupDetected|true|Cup Detected|false|Cup Not Detected|


På sidan Tillstånd > Brewing lägger du till värdet som true. Lägg till det andra värdet som false med det alternativa visningsnamnet Not Brewing genom att klicka på **+** bredvid **Värden**.

> [!NOTE]
> När du har definierat telemetri och tillstånd ser du de simulerade data som genereras via enhetsmallen på enhetsskärmen. Dessa simulerade data gör att du kan testa programmet innan du ansluter en fysisk enhet till IoT Central. 

## <a name="use-settings-to-set-the-optimal-temperature-of-the-coffee-machine"></a>Använd Inställningar för att ange den optimala temperaturen för kaffebryggaren
Gå till sidan Inställningar på fliken bredvid Mått. Aktivera **Designläge**. Lägg till följande **Nummer**-inställning under **Bibliotek** på sidan **Inställningar**:

|Visningsnamn|Fältnamn|Enheter|Decimaler|Min|Max|Initialt värde|
|---|---|---|---|---|---|---|---|
|Optimal Temperature|setTemperature|Celsius|1|86|100|95|

## <a name="use-properties-to-store-warranty-info-and-water-temperature-range"></a>Använda egenskaper för att lagra garantiinformation och vattentemperaturinternvall

Lägg till följande **Nummer**-egenskaper på sidan **Egenskaper** genom att först aktivera **Designläge**:

|Visningsnamn|Fältnamn|Enheter|Antal decimaler|Min|Max|Initialt värde
|---|---|---|---|---|---|---|
|Coffee Makers Min Temperature|propertyMinTemperature|Celsius|1|88|92|90|
|Coffee Makers Max Temperature|propertyMaxTemperature|Celsius|1|96|99|98| 

Lägg till följande **enhetsegenskap** på sidan **Egenskaper**:

   |Visningsnamn|Fältnamn|Datatyp|
   |---|---|---|
   |Device Warranty Expired|propertyWarrantyExpired|nummer|

> [!NOTE]
> Enhetsegenskapen skickas av enheten, i det här fallet din kaffebryggare. När du ansluter kaffebryggaren till Azure IoT Central, skickas enhetsegenskapen Garanti till programmet och visas i fältet Enhetens garanti har upphört. 

## <a name="use-commands-to-set-maintenance-mode-and-start-brewing"></a>Använda kommandon för att ange underhållsläget och starta bryggningen

Lägg till följande kommandon på sidan **Kommandon** genom att först aktivera **Designläge**.

|Visningsnamn|Fältnamn|Standardvärde för tidsgräns|Datatyp|
|---|---|---|---|---|---|---|
|Set Maintenance Mode|cmdSetMaintenance|30|text| 
|Start Brewing|cmdStartBrewing|30|text|

## <a name="summary"></a>Sammanfattning

I den här lektionen har du skapat en ny enhetstyp, en kaffebryggare, med hjälp av enhetsmallen. I enhetsmallen angav du vilka data som kaffebryggaren kan utbyta med ditt program. Du har definierat telemetridata som temperatur och luftfuktighet samt olika tillstånd, som om kaffebryggaren brygger eller inte. Du har även definierat beteendet och funktionerna för kaffebryggaren genom att konfigurera inställningar, egenskaper och kommandon. 

