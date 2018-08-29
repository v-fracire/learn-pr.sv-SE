Anta att du arbetar för ett rulltrappsföretag som har gjort stora investeringar i IoT-teknik. Du övervakar bearbetningen av temperatursensordata från drivhjulen i rulltrapporna. Du måste övervaka temperaturdatan och lägga till en dataflagga som visar när drivhjulen blir för varma. I underordnade system kan man med detta avgöra när underhåll krävs.

Ditt företag tar emot sensordata från flera platser och från olika rulltrappsmodeller. Datan tas emot i olika format, inklusive batchfilöverföringar, schemalagda databashämtningar, meddelanden i en kö eller inkommande data från en händelsehubb. Du vill utveckla en återanvändbar tjänst som kan bearbeta dina temperaturdata från alla dessa källor.

Att utveckla och vara värd för en tjänst som detta brukade kräva servrar och infrastruktur. Med Azure Functions får du serverlös databehandling. Du kodar logik med önskat programmeringsspråk och logiken körs när du behöver den, utan att några servrar krävs.

Du kan fokusera på att skapa användbara appar, i stället för att etablera eller underhålla servrar.

## <a name="learning-objectives"></a>Utbildningsmål
> [!div class="checklist"]
> * Bestämma om serverlös databehandling passar för din verksamhet
> * Skapa en Azure-funktionsapp i Azure-portalen
> * Köra en funktion med utlösare
> * Övervaka och testa din Azure-funktion från Azure-portalen 
