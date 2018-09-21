Anta att du kör en social nätverksplats för experter. Du tillåter att användarna överför sina porträttbilder som ska publiceras på deras profiler. Om du vill minska belastningen på webbservern kan du skapa en serverlös serverdel med Azure Functions för att bearbeta dessa data. Du vill skapa en avbildningsminiatyr och sedan spara den i det permanenta lagringsutrymmet. 

Kraften i Azure Functions kommer till största delen från integreringar av erbjudanden med en mängd datakällor och tjänster. De här integreringarna definieras med hjälp av bindningar. Med hjälp av bindningar kan utvecklare interagera med andra datakällor och tjänster utan att behöva bekymra sig om hur data flödar till och från deras funktion.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att:

- Utforska vilka typer av datakällor som kan nås via bindningar
- Läsa data från Azure Cosmos DB med hjälp av Azure Functions
- Lagra data i Azure Cosmos-databasen med hjälp av Azure Functions
- Skicka meddelanden till Azure Queue Storage med hjälp av Azure Functions
