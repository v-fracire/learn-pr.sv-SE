De data som en enhet kan utbyta med ditt program har angetts i en mall för enheten som definierar beteende och kapaciteter för en enhet eller i det här fallet en kaffe dator i Azure IoT Central. När du skapar en enhetsmall genereras en simulerad enhet från mallen. Den simulerade enheten genererar telemetri som hjälper dig att testa hur ditt program innan en fysisk/real-enhet är ansluten. 

I den här enheten, kan du skapa en mall för enheten för en kaffe-dator som anger följande funktioner och beteenden:
* **Mätning av faktisk användning**: data som kommer från din enhet. Du kan lägga till flera mått till mallen för enheten att matcha funktionerna i din enhet.
    * Mätningar av telemetri: numeriska datapunkter som din enhet som samlar in över tid. De är representeras som en löpande ström. I det här scenariot telemetri mätning av faktisk användning är air fuktighet och vatten temperatur. 

    * Plats för mätning av faktisk användning: tillståndet för enheten eller dess komponenter under en viss tidsperiod. I det här scenariot ställer du in tillstånd som Brewing/inte bryggning Cup identifierade/Cup inte identifieras

* **Inställningar för**: du använda inställningar för att skicka konfigurationsdata till en enhet från ditt program. I det här scenariot justera optimala water temperaturen i inställningar och skicka den till den kaffe-datorn. När inställningen har uppdaterats kan den markeras som väntar i Användargränssnittet tills enheten bekräftar att den har svarat på ändringen.

* **Egenskaper för**: enhetsmetadata som är associerat med enheten. Det finns två typer av egenskaper.
    * Du använder *programegenskaper* att samla in information om din enhet i ditt program. I det här scenariot kan använda du egenskaper för program för att ange perfekt water temperaturintervall maskinens kaffe. Programegenskaper lagras i programmet och synkroniseras inte med enheten. 

    * Du använder *enhetsegenskaper* för göra så att en enhet kan skicka egenskapsvärden till programmet. De här egenskaperna kan bara ändras av enheten. I det här scenariot konfigurerar du enhetsegenskap som heter enheten garantin inte längre i IoT Central. Fältet enhet garantin längre förblir tom tills kaffe-dator är ansluten till IoT Central. När du är ansluten, skickar den kaffe datorn garantistatus till programmet. 

* **Kommandon**: du använda kommandon för att hantera din enhet från ditt program. Du kan köra kommandon direkt på enheten från molnet för att styra enheterna. I det här scenariot kan köra du kommandon på datorn kaffe inställd underhåll eller starta bryggning. 

## <a name="create-a-device-template-for-the-coffee-maker"></a>Skapa en enhetsmall för kaffebryggaren
En enhet mall definierar beteende och kapaciteter för en enhet eller i det här fallet kaffe flöde.

1. Gå till startsidan och välj **skapar mallar för enheten**.

1. Ange *anslutna kaffe Maker* för din anpassade mall. 
 
1. Välj **Skapa**. Du har skapat en tom enheten mall för kaffe-maker definierar beteende och kapaciteter för datorn. 

## <a name="define-telemetry-measurement-temperature-and-humidity"></a>Definiera telemetri för mätning av temperatur och luftfuktighet
1.  I enhetsmallen **Ansluten kaffebryggare** kontrollerar du att du är på sidan **Mått**, där du definierar telemetrin. 

1.  Lägg till telemetri-mätning temperatur, Välj **+ ny mätning**. Välj sedan **telemetri** som måttenhet.

1.  Varje typ av telemetri som du definierar för en enhetsmall innehåller konfigurationsalternativ som:
    * Visningsalternativ.
    * Information om telemetrin.
    * Simuleringsparametrar.

    Använd informationen i följande tabell för att konfigurera din temperatur och fuktighet telemetri. När du skapar telemetri objekt kan du behöva lägga till ett nytt mått genom att välja **+ ny mätning** för varje objekt i tabellen.
    
    |Visningsnamn|Fältnamn|Enheter|Min|Max|Antal decimaler|
    |---|---|---|---|---|---|
    |Vattentemperatur|vattentemperatur|Celsius|86|100|1|
    |Luftfuktighet|luftfuktighet|%|20|100|0|
   
    Du kan även välja en färg för telemetrivisningen. Om du vill spara telemetri-definition, Välj **spara**. När du skapar flera definitioner av mått, inställningar, egenskaper och kommandon i den återstående enheten, Kom ihåg att spara när du är klar.  
    
    ![Skapa en enhetsmall](../images/2-device-template-a.png)

    Ange fältnamn exakt som de visas i tabellen i mallen för enheten. Om egenskapsnamnen i motsvarande enhet koden inte matchar fältnamnen kan inte visas telemetri i programmet. Gör samma när du anger inställningar och egenskapsinformation. 

## <a name="define-state-measurement-for-brewingnot-brewing-cup-detectedcup-not-detected"></a>Definiera tillståndsmätning för brygger/brygger inte, kopp identifierad/kopp inte identifierad
Lägg till följande tillstånd i den **mätningar av** sidan genom att välja **+ ny mätning**. Välj sedan **Tillstånd** som måttyp:
    
   |Visningsnamn|Fältnamn|Värde 1|Visningsnamn 1|Värde 2|Visningsnamn 2|
   |---|---|---|---|---|---|
   |Brygger|tillståndBrygger|sant|Brygger|falskt|Brygger inte|
   |Kopp identifierad|tillståndKoppIdentifierad|sant|Kopp identifierad|falskt|Kopp inte identifierad|


På tillstånd > bryggning sidan du lägga till värdet som SANT. Lägga till andra värdet som FALSKT med alternativa visningsnamn som inte bryggning genom att klicka på **+** bredvid **värden**.

> [!NOTE]
> När du har definierat telemetri och tillstånd Se simulerade data genereras från mallen enheten på enhetens skärm. Simulerade data kan du testa hur ditt program innan du ansluter en fysisk enhet till IoT Central. 

## <a name="use-settings-to-set-the-optimal-temperature-of-the-coffee-machine"></a>Använd Inställningar för att ange den optimala temperaturen för kaffebryggaren
Gå till sidan Inställningar på fliken bredvid mätning av faktisk användning. Aktivera **Designläge**. Lägg till följande **nummer** inställningen **biblioteket** på den **inställningar** sidan:

|Visningsnamn|Fältnamn|Enheter|Decimaler|Min|Max|Inledande|
|---|---|---|---|---|---|---|---|
|Optimal temperatur|angeTemperatur|Celsius|1|86|100|95|

## <a name="use-properties-to-store-warranty-info-and-water-temperature-range"></a>Använda egenskaper för att lagra garantiinformation och vattentemperaturinternvall

Lägg till följande **nummer** egenskaper på den **egenskaper** sidan genom att första aktivera **designläget**:

|Visningsnamn|Fältnamn|Enheter|Antal decimaler|Min|Max|Inledande
|---|---|---|---|---|---|---|
|Kaffebryggarens lägsta temperatur|egenskapLägstaTemperatur|Celsius|1|88|92|90|
|Kaffebryggarens högsta temperatur|egenskapHögstaTemperatur|Celsius|1|96|99|98| 

Lägg till följande **enhetsegenskap** på sidan **Egenskaper**:

   |Visningsnamn|Fältnamn|Datatyp|
   |---|---|---|
   |Enhetens garanti har upphört|egenskapGarantiUpphört|nummer|

> [!NOTE]
> Enhetsegenskap skickas av enheten, i det här fallet din kaffebryggare. När du ansluter kaffebryggaren till Azure IoT Central, skickas enhetsegenskapen Garanti till programmet och visas i fältet Enhetens garanti har upphört. 

## <a name="use-commands-to-set-maintenance-mode-and-start-brewing"></a>Använda kommandon för att ange underhållsläget och starta bryggningen

Lägg till följande kommandon på sidan **Kommandon** genom att först aktivera **Designläge**.

|Visningsnamn|Fältnamn|Standardvärde för tidsgräns|Datatyp|
|---|---|---|---|---|---|---|
|Ange underhållsläge|cmdSetMaintenance|30|text| 
|Starta bryggning|komStartaBryggning|30|text|

## <a name="summary"></a>Sammanfattning

I den här enheten skapade du en ny enhetstyp, en kaffe-dator med hjälp av mallen för enheten. I mallen enhet angett du som en kaffe dator kan utbyta data med ditt program. Du har definierat telemetridata som temperatur och luftfuktighet samt tillståndet – om kaffebryggaren brygger eller inte. Ytterligare definierade du beteende och kapaciteter för kaffe datorn genom att konfigurera inställningar, egenskaper och kommandon. 

