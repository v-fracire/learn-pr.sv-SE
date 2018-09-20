Många program består av program som körs på flera olika datorer eller enheter. I sådana distribuerade program måste meddelanden skickas mellan komponenterna i nätverk och över långa avstånd. Även om de finns på samma server eller i samma datacenter, kräver löst sammansatta arkitekturer mekanismer för att komponenterna ska kunna kommunicera. En tillförlitlig meddelandehantering är ofta ett kritiskt problem.

Anta att du arbetar på ett programvaruföretag som utvecklar ett program för musikdelning. Musiker kan ladda upp musik som de skapar till din plattform med hjälp av en webbklient eller en mobilapp. De kan lyssna på och kommentera sådant som andra medlemmar har gjort. Programmet består av en webbplats som körs hos din Internetleverantör, en mobilapp som körs på användarnas mobila enheter, ett webb-API som körs i Azure och en Azure SQL-databas där data lagras.

Du har märkt att när det är hög efterfrågan laddas vissa musikfiler inte upp och vissa kommentarer publiceras inte. Ditt test visar att dessa problem orsakas av utelämnade meddelanden mellan klientdelskomponenterna och webb-API:n. Du tänker lösa dessa problem genom att använda en eller flera av följande tekniker: Azure Storage-köer, Azure Event Hubs, Azure Event Grid och Azure Service Bus.

Här lär du dig att välja rätt meddelandeteknik i Azure för varje kommunikation i ett distribuerat program.

## <a name="learning-objectives"></a>Utbildningsmål
I den här modulen kommer du att göra följande:

- Beskriva händelser och meddelanden samt utmaningarna med att använda dem i ett distribuerat program.
- Identifiera scenarier där en Storage-kö är den bästa meddelandetekniken för ett program.
- Identifiera scenarier där Event Grid är den bästa meddelandetekniken för ett program.
- Identifiera scenarier där Event Hubs är den bästa meddelandetekniken för ett program.
- Identifiera scenarier där Service Bus är den bästa meddelandetekniken för ett program.