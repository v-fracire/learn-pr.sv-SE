I Azure IoT Central anges de data som en enhet kan utbyta med ditt program i en enhetsmall som definierar beteendet för en enhet – i det här fallet en kaffebryggare. När du skapar en enhetsmall genereras en simulerad enhet från mallen. Den simulerade enheten genererar telemetri som gör att du kan testa programmets beteende innan du ansluter en fysisk enhet. Senare i den här modulen ansluter du ett allmänt Node.js-program som representerar en fysisk kaffebryggare. 

I den här modulen skapar du en enhetsmall för en kaffebryggare som anger följande funktioner och beteenden:
* Mått 
    * Telemetri: Luftfuktighet, Vattentemperatur
    * Tillstånd: Brygger/Brygger inte, Kopp identifierad/Kopp inte identifierad
* Inställningar: Optimal temperatur
* Egenskaper: Lägsta och högsta temperatur för kaffebryggaren, Enhetens garanti har upphört
* Kommandon: Ange underhållsläge, Starta bryggning


## <a name="create-a-device-template-for-the-coffee-maker"></a>Skapa en enhetsmall för kaffebryggaren
En enhetsmall som definierar beteendet och funktionen för en enhet – i det här fallet en kaffebryggare.

1. Navigera till startsidan och välj Create Device Templates (Skapa enhetsmallar).
![Skapa en enhetsmall](../images/2-device-template-a1.png)

1. Ange ett namn på din anpassade enhet. I den här modulen anger du till exempel Ansluten kaffebryggare.
![Skapa en enhetsmall](../images/2-device-template-a2.png)
 
1. Du har skapat en tom enhetsmall för kaffebryggaren där du vill definiera bryggarens beteende och funktioner. 

## <a name="define-telemetry-measurement-temperature-and-humidity"></a>Definiera telemetri för mätning av temperatur och luftfuktighet
1.  I enhetsmallen **Ansluten kaffebryggare** kontrollerar du att du är på sidan **Mått**, där du definierar telemetrin. Varje enhetsmall som du definierar har separata sidor där du kan:
    * Ange mått som telemetri, händelse och tillstånd, som skickas av enheten.
    * Definiera de inställningar som används för att styra enheten.
    * Definiera egenskaperna som används för att registrera information om den enhet och enhetsegenskap som skickas av enheten.
    * Definiera de regler som är kopplade till enheten.
    * Anpassa enhetens instrumentpanel så att den visar relevant information om enheten. 

1.  För att lägga till temperaturtelemetrimått väljer du **Nytt mått**. Välj sedan **Telemetri** som måttenhet: ![Skapa en enhetsmall](../images/2-device-template-c.png)

1.  Varje typ av telemetri som du definierar för en enhetsmall innehåller konfigurationsalternativ som:
    * Visningsalternativ.
    * Information om telemetrin.
    * Simuleringsparametrar.

    För att konfigurera telemetri för temperatur och luftfuktighet använder du informationen i följande tabell:
    
    |Visningsnamn|Fältnamn|Enheter|Min|Max|Antal decimaler|
    |---|---|---|---|---|---|
    |Vattentemperatur|vattentemperatur|Celsius|90|98|1|
    |Luftfuktighet|luftfuktighet|%|20|100|0|
   
    Du kan även välja en färg för telemetrivisningen. Du kan spara telemetridefinitionen genom att välja **Spara**: ![Skapa en enhetsmall](../images/2-device-template-d.png)

    Ange fältnamn exakt som de visas i enhetsmallen. Om fältnamnen inte matchar kan telemetridata inte visas i programmet. Gör samma när du anger inställningar och egenskapsinformation. 

## <a name="define-state-measurement-for-brewingnot-brewing-cup-detectedcup-not-detected"></a>Definiera tillståndsmätning för brygger/brygger inte, kopp identifierad/kopp inte identifierad
Lägg till följande tillstånd på sidan **Mått** genom att välja **Nytt mått**. Välj sedan **Tillstånd** som måttyp:
    
|Visningsnamn|Fältnamn|Värde 1|Visningsnamn|Värde 2|Visningsnamn|
|---|---|---|---|---|---|
|Brygger|tillståndBrygger|sant|Brygger inte|falskt|Brygger inte|
|Kopp identifierad|tillståndKoppIdentifierad|sant|Kopp inte identifierad|falskt|Kopp inte identifierad|

![Skapa en enhetsmall](../images/2-device-template-f.png)

När du skapar en enhetsmall genereras en simulerad enhet från mallen. Den simulerade enheten genererar telemetri som gör att du kan testa programmets beteende innan du ansluter en fysisk enhet.
![Skapa en enhetsmall](../images/2-device-template-m.png)

## <a name="use-settings-to-set-the-optimal-temperature-of-the-coffee-machine"></a>Använd Inställningar för att ange den optimala temperaturen för kaffebryggaren
Navigera till Inställningar. Aktivera **Designläge**. Lägg till följande antal inställningar på sidan **Inställningar**:

|Visningsnamn|Fältnamn|Enheter|Decimaler|Min|Max|Inledande|
|---|---|---|---|---|---|---|---|
|Optimal temperatur|angeTemperatur|Celsius|1|92|99|95|

![Skapa en enhetsmall](../images/2-device-template-q.png)

## <a name="use-properties-to-store-warranty-info-and-water-temperature-range"></a>Använda egenskaper för att lagra garantiinformation och vattentemperaturinternvall
Lägg till följande **talegenskap** på sidan **Egenskaper** genom att först aktivera **Designläge**:
|Visningsnamn|Fältnamn|Enhet|Antal decimaler|Min|Max|Inledande
|---|---|---|---|---|---|---|
|Kaffebryggarens lägsta temperatur|egenskapLägstaTemperatur|Celsius|1|88|92|90|    
|Kaffebryggarens högsta temperatur|egenskapHögstaTemperatur|Celsius|1|96|99|98| 

![Skapa en enhetsmall](../images/2-device-template-n.png)

Lägg till följande **enhetsegenskap** på sidan **Egenskaper**:

|Visningsnamn|Fältnamn|Datatyp|
|---|---|---|
|Enhetens garanti har upphört|egenskapGarantiUpphört|nummer|
![Skapa en enhetsmall](../images/2-device-template-u.png)

> [!NOTE]
> Enhetsegenskap skickas av enheten, i det här fallet din kaffebryggare. När du ansluter kaffebryggaren till Azure IoT Central, skickas enhetsegenskapen Garanti till programmet och visas i fältet Enhetens garanti har upphört. 

## <a name="use-commands-to-set-maintenance-mode-and-start-brewing"></a>Använda kommandon för att ange underhållsläget och starta bryggningen

Lägg till följande kommandon på sidan **Kommandon** genom att först aktivera **Designläge**.
|Visningsnamn|Fältnamn|Inmatningsfält|Datatyp|
|---|---|---|---|---|---|---|
|Ange underhåll|komAngeUnderhåll|30|text| 
|Starta bryggning|komStartaBryggning|30|text|

![Skapa en enhetsmall](../images/2-device-template-s.png)


## <a name="summary"></a>Sammanfattning

I den här modulen har du skapat en ny enhetstyp, en kaffebryggare, med hjälp av enhetsmallen för att ange data som en kaffebryggare kan utbyta med programmet. Du har definierat telemetridata som temperatur och luftfuktighet samt tillståndet – om kaffebryggaren brygger eller inte. Du har även definierat beteendet och funktionerna för kaffebryggaren genom att konfigurera inställningar, egenskaper och kommandon. 

