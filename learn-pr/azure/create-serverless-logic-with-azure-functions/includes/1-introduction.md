Anta att du arbetar för ett företag inom rulltrappor som har investerat i IoT-teknik för att övervaka sin produkt på fältet. Du övervakar bearbetningen av temperatursensordata från drivhjulen i rulltrapporna. Du övervakar temperaturdata och lägger till en dataflagga som visar när drivhjulen blir för varma. I underordnade system kan man med dessa data avgöra när underhåll krävs.

Ditt företag tar emot sensordata från flera platser och från olika rulltrappsmodeller. Data tas emot i olika format, inklusive batchfiluppladdningar, schemalagda databashämtningar, meddelanden i en kö och inkommande data från en händelsehubb. Du vill utveckla en återanvändbar tjänst som kan bearbeta dina temperaturdata från alla dessa källor.

När du utformar en sådan tjänst med traditionella strategier för företagsarkitektur behöver du tänka på serverinfrastruktur och -underhåll direkt: hitta nödvändig maskinvara, planera installation, samordna hantering med IT-avdelningen, osv. Ett alternativ till allt det arbetet är **serverlös databehandling**. Med serverlös databehandling hanterar molnleverantören etablering och underhåll av infrastrukturen så att du kan fokusera helt på att skapa applogiken. Azure Functions är en viktig del av erbjudandet med serverlös databehandling från Azure och gör att du kan köra delar av kod eller *funktioner*, skrivna på det språk du föredrar, i molnet.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att:

- Bestämma om serverlös databehandling passar för din verksamhet.
- Skapa en Azure-funktionsapp på Azure-portalen.
- Köra en funktion med utlösare.
- Övervaka och testa din Azure-funktion från Azure-portalen.
