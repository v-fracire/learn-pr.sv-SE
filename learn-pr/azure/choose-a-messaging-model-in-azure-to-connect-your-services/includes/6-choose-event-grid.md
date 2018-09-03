Många program använder händelser för att meddela distribuerade komponenter om att något har hänt eller att ett objekt har ändrats. Du kan använda Azure Event Grid för att göra det lättare att distribuera dessa händelser via Azure.

Anta att du har ett program för musikdelning med ett webb-API som körs i Azure. När en ny ljudfil laddas upp av en användare, måste du meddela alla mobilappar som finns installerade på användarnas enheter över hela världen.

Du kan använda prenumerationer i Event Grid för att uppnå detta tillförlitligt och snabbt.

## <a name="what-is-event-grid"></a>Vad är Event Grid?

Event Grid är en tjänst som distribuerar händelser från källor, till exempel Azure Blob Storage-konton eller Azure Media Services, till prenumeranter som exempelvis Azure Functions eller webhooks.

En händelsekälla är en komponent som kan generera en händelse. En användare kan till exempel lägga till en ny mediefil till ett Blob Storage-konto. Detta genererar en händelse som Event Grid kan distribuera.

En händelseprenumerant är en komponent som kan ta emot händelser från Event Grid. Functions kan till exempel köra kod som svarar på den nya mediefilen i Blob Storage-kontot.

Med Event Grid är det enkelt att ansluta källor till flera prenumeranter.

![Diagram över Event Grid-källor och prenumeranter](../images/6-event-grid.png)

## <a name="event-sources"></a>Händelsekällor

Händelser kan genereras av följande Azure-resurstyper:

- **Azure-prenumerationer och resursgrupper.** Prenumerationer och resursgrupper genererar händelser som är relaterade till hanteringsåtgärder i Azure. När en användare skapar en virtuell dator, genererar exempelvis den här källan en händelse.
- **Lagringskonton.** Lagringskonton kan generera händelser när användarna lägger till blobar, filer, tabellposter eller kömeddelanden. Du kan använda både blobkonton och konton av allmän typ som händelsekällor.
- **Media Services.** Media Services är värd för video- och ljudmedia och innehåller avancerade hanteringsfunktioner för mediefiler. Media Services kan generera händelser när ett kodningsjobb startas eller slutförs i en videofil.
- **Azure IoT Hub.** IoT Hub kommunicerar med och samlar in telemetri från IoT-enheter. Den kan generera händelser när sådan kommunikation tas emot.
- **Anpassade händelser.** Anpassade händelser kan genereras med hjälp av REST API eller med Azure SDK för Java, GO, .NET, Node, Python och Ruby. Du kan till exempel skapa en anpassad händelse i funktionen Webbappar i Azure App Service. Detta kan inträffa i arbetsrollen när den tar emot ett meddelande från en lagringskö.

Den här omfattande integreringen med olika händelsekällor inom Azure säkerställer att Event Grid kan distribuera händelser som är relaterade till nästan alla Azure-resurser.

## <a name="topics"></a>Ämnen

I Event Grid består ett ämne av en samling med relaterade händelser. När du publicerar händelser från en källa bestämmer du om dessa händelser bara behöver ett enda ämne eller om de ska delas upp i flera ämnen. Komponenter som tar emot och hanterar händelser prenumererar på ämnen för att avgöra vilka händelser de ska ta emot.

## <a name="subscriptions"></a>Prenumerationer

Händelsehanterare använder prenumerationer för att Event Grid ska veta vilka händelser i ett ämne som de ska ta emot. Prenumerationen åtgärdar även slutpunkten – vilket är den plats som Event Grid skickar händelsemeddelanden till. En prenumeration kan också filtrera händelserna efter typ eller efter ämne så att du kan vara säker på att en händelsehanterare endast tar emot relevanta händelser.

## <a name="event-handlers"></a>Händelsehanterare

Följande objekttyper i Azure kan ta emot och hantera händelser från Event Grid:

- **Azure Functions.** En Azure-funktion består av anpassad kod som körs i Azure, utan någon virtuell värdserver eller container. Använd en Azure-funktion som en händelsehanterare när du vill koda ett anpassat svar på händelsen.
- **Webhooks.** En webhook är ett webb-API som implementerar en push-arkitektur.
- **Azure Logic Apps.** En Azure logikapp är värd för en affärsprocess som ett arbetsflöde.
- **Microsoft Flow.** Flow är också värd för arbetsflöden, men är enklare för icke-teknisk personal att använda.

## <a name="how-to-choose"></a>Så här väljer du

Använd Event Grid när du behöver dessa funktioner:

- **Enkelhet.** Det är mycket enkelt att ansluta källor till prenumeranter i Event Grid.
- **Avancerad filtrering.** Prenumerationer har noggrann kontroll över de händelser som de tar emot från ett ämne.
- **Förgrening.** Du kan prenumerera på ett obegränsat antal slutpunkter till samma händelser och ämnen.
- **Tillförlitlighet.** Event Grid försöker leverera händelser i upp till 24 timmar för varje prenumeration.
- **Betala per händelse.** Betala endast för antalet händelser som du överför.

## <a name="summary"></a>Sammanfattning

Event Grid är ett enkelt och mångsidigt distributionssystem för händelser. Du kan använda det till att publicera händelser till prenumeranter, som får händelserna på ett pålitligt och snabbt sätt.