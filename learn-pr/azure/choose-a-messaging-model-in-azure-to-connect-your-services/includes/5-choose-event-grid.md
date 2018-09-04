Många program använder en publicera-prenumerera-modell för att meddela distribuerade komponenter om att något har hänt eller att ett objekt har ändrats. Anta att du har ett program för musikdelning med ett webb-API som körs i Azure. När en ny låt laddas upp av en användare måste du meddela alla mobilappar som finns installerade på enheter för användare över hela världen som är intresserade av den genre som just laddats upp.

I den här arkitekturen behöver utgivaren av ljudfilen inte känna till de prenumeranter som är intresserade av den musik som delas. Dessutom vill vi ha en 1:många-relation där vi kan ha flera prenumeranter som kan välja om de är intresserade av den här nya musikstilen. Azure Event Grid är en perfekt lösning för den här typen av arkitektur.

## <a name="what-is-event-grid"></a>Vad är Event Grid?
Event Grid är en tjänst som distribuerar _händelser_ från olika källor, till exempel Azure Blob-lagringskonton eller Azure Media Services, till olika prenumeranter som exempelvis Azure Functions eller WebHooks.

### <a name="what-is-an-event-source"></a>Vad är en händelsekälla?
En händelsekälla är en komponent som kan generera en händelse (kallas ibland ”utgivare”). Till exempel kan en användare lägga till en ny låt till ett Blob-lagringskonto via vårt Webb-API. Detta genererar en händelse som Event Grid kan distribuera till intresserade händelseprenumeranter. Utgivare skickar händelser men har inga förväntningar om vad som tar emot eller bearbetar dem.

### <a name="what-is-an-event-subscriber"></a>Vad är en händelseprenumerant?
En händelseprenumerant är en komponent som kan ta emot händelser från Event Grid. Till exempel kan Azure Functions köra kod som svar på den nya låt som läggs till i Blob-lagringskontot. Prenumeranter kan välja vilka händelser de vill hantera, så meddelar Event Grid på ett effektivt sätt alla berörda prenumeranter när en ny händelse är tillgänglig – det krävs ingen avsökning.

Event Grid stöder de flesta Azure-tjänster som utgivare eller prenumerant och kan även användas med tjänster från tredje part. Det tillhandahåller ett dynamiskt skalbart meddelandesystem till låg kostnad som gör att utgivare kan meddela prenumeranter om statusändringar.

![Diagram över Event Grid-källor och prenumeranter](../images/6-event-grid.png)

> [!NOTE]
> Event Grid skickar en händelse för att meddela prenumeranter om något som ändras – men det _faktiska objekt_ som ändrades ingår inte i händelseleveransen.

## <a name="types-of-event-sources"></a>Typer av händelsekällor
Händelser kan genereras av följande Azure-resurstyper:

- **Azure-prenumerationer och resursgrupper.** Prenumerationer och resursgrupper genererar händelser som är relaterade till hanteringsåtgärder i Azure. När en användare skapar en virtuell dator genererar exempelvis den här källan en händelse.
- **Lagringskonton.** Lagringskonton kan generera händelser när användarna lägger till blobbar, filer, tabellposter eller kömeddelanden. Du kan använda både blobbkonton och konton för generell användning som händelsekällor.
- **Media Services.** Media Services är värd för video- och ljudmedia och innehåller avancerade hanteringsfunktioner för mediefiler. Media Services kan generera händelser när ett kodningsjobb startas eller slutförs i en videofil.
- **Azure IoT Hub.** IoT Hub kommunicerar med och samlar in telemetri från IoT-enheter. Den kan generera händelser när sådan kommunikation tas emot.
- **Anpassade händelser.** Anpassade händelser kan genereras med hjälp av REST API eller med Azure SDK för Java, GO, .NET, Node, Python och Ruby. Du kan till exempel skapa en anpassad händelse i Web Apps-funktionen i Azure App Service. Detta kan inträffa i arbetsrollen när den tar emot ett meddelande från en lagringskö.

Den här omfattande integreringen med olika händelsekällor inom Azure säkerställer att Event Grid kan distribuera händelser som är relaterade till nästan vilken Azure-resurs som helst.

## <a name="topics"></a>Ämnen
I Event Grid består ett ämne av en samling med relaterade händelser. När du publicerar händelser från en källa väljer du om dessa händelser bara behöver ett enda ämne eller om de ska delas upp i flera ämnen. Komponenter som tar emot och hanterar händelser prenumererar på ämnen för att avgöra vilka händelser de ska ta emot.

## <a name="subscriptions"></a>Prenumerationer
Händelsehanterare använder prenumerationer för att informera Event Grid om vilka händelser i ett ämne de ska ta emot. Prenumerationen bestämmer även slutpunkten – vilket är den plats som Event Grid skickar händelsemeddelanden till. En prenumeration kan även filtrera händelserna efter typ eller ämne så att du kan vara säker på att en händelsehanterare endast tar emot relevanta händelser.

## <a name="event-handlers"></a>Händelsehanterare
Följande objekttyper i Azure kan ta emot och hantera händelser från Event Grid:

- **Azure Functions.** En Azure-funktion består av anpassad kod som körs i Azure utan någon virtuell värdserver eller container. Använd en Azure-funktion som en händelsehanterare när du vill koda ett anpassat svar på händelsen.
- **Webhooks.** En webhook är ett webb-API som implementerar en push-arkitektur.
- **Azure Logic Apps.** En Azure-logikapp är värd för en affärsprocess som ett arbetsflöde.
- **Microsoft Flow.** Flow är också värd för arbetsflöden men är enklare för icke-teknisk personal att använda.

## <a name="should-you-use-event-grid"></a>Bör du använda Event Grid?
Använd Event Grid när du behöver dessa funktioner:

- **Enkelhet.** Det är mycket enkelt att ansluta källor till prenumeranter i Event Grid.
- **Avancerad filtrering.** Prenumerationer har noggrann kontroll över de händelser som de tar emot från ett ämne.
- **Förgrening.** Du kan prenumerera på ett obegränsat antal slutpunkter till samma händelser och ämnen.
- **Tillförlitlighet.** Event Grid försöker upprepat leverera händelser i upp till 24 timmar för varje prenumeration.
- **Betala per händelse.** Betala endast för det antal händelser som du överför.

## <a name="summary"></a>Sammanfattning
Event Grid är ett enkelt och mångsidigt distributionssystem för händelser. Du kan använda det till att leverera distinkta händelser till prenumeranter, som får händelserna på ett pålitligt och snabbt sätt. Vi har ytterligare en meddelandemodell att titta på – hur gör vi om vi vill leverera en stor _ström_ med händelser? I det här scenariot är Event Grid inte den ideala lösningen eftersom den är utformad för leverans av en händelse i taget. I stället vänder vi oss till en annan Azure-tjänst: Event Hubs.