Många program använder en Publicera-prenumerera på modellen för att meddela distribuerade komponenter som något hände eller som har ändrats för vissa objekt. Anta att du har ett program för musikdelning med ett webb-API som körs i Azure. När en användare laddar upp en ny Låt, måste du meddela alla mobila appar som installeras på användarens enheter över hela världen som är intresserade av att genre.

I den här arkitekturen behöver utgivaren av ljudfilen inte känna till prenumeranter intresserad av delad musik. Dessutom vill vi ha en en-till-många-relation där vi kan ha flera prenumeranter som du kan också bestämma om de är intresserad av den här nya Låt. Azure Event Grid är en perfekt lösning för den här typen av arkitektur.

## <a name="what-is-azure-event-grid"></a>Vad är Azure Event Grid?
Azure Event Grid är en fullständigt hanterad tjänst för händelsedirigering som körs ovanpå Azure Service Fabric. Event Grid distribuerar _händelser_ från olika källor, till exempel Azure Blob storage-konton eller Azure Media Services till olika hanterare, till exempel Azure Functions eller Webhooks. Event Grid har skapats för att göra det enklare att skapa händelsebaserad och program utan server i Azure.

Event Grid stöder de flesta Azure-tjänster som en utgivare eller en prenumerant och kan användas med tjänster från tredje part. Det tillhandahåller ett dynamiskt skalbart meddelandesystem till låg kostnad som gör att utgivare kan meddela prenumeranter om statusändringar. Följande bild visar Azure Event Grid tar emot meddelanden från flera källor och distribuera dem till händelsehanterare baserat på prenumerationen.

Det finns flera koncept i Azure Event Grid som ansluter en källa till en prenumerant:

1. **Händelser** – Vad hände?
1. **Händelsekällor** – där händelsen ägde rum.
1. **Ämnen** – Slutpunkten som utgivarna skickar händelser till.
1. **Händelseprenumerationer** – Den slutpunkt eller inbyggda mekanism som dirigerar händelser, ibland till flera hanterare. Prenumerationer används också av hanterare för att filtrera inkommande händelser smart.
1. **Händelsehanterare** – Den app eller tjänst som reagerar på händelsen.

![En bild som visar ett Azure Event Grid som placerats mellan flera händelsekällor och flera händelsehanterare. Händelsekällorna skicka händelser till Event Grid och Event Grid vidarebefordrar relevanta händelser till prenumeranter. Event Grid använda ämnen för att bestämma vilka händelser som ska skicka till vilka hanterare. Händelser källor tagga varje händelse med en eller flera ämnen och händelsehanterare prenumerera på ämnen som de är intresserad av.](../media-draft/5-event-grid.png)

### <a name="what-is-an-event"></a>Vad är en händelse?
**Händelser** är datameddelanden passerar genom Event Grid som beskriver vad har ägt rum. Varje händelse är självständig, kan vara upp till 64 KB och innehåller flera olika typer av information baserat på ett schema som definierats i Event Grid:

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

| Fält | Beskrivning |
|-------|-------------|
| **Avsnittet** | Fullständiga resurs-sökvägen till händelsekällan. Event Grid ger det här värdet. |
| **subject** | Publisher-definierade sökvägen till ämne för händelsen. |
| **ID** | Den unika identifieraren för händelsen. |
| **Händelsetyp** | En av typerna som registrerade händelsen för den här händelsekällan. Detta är ett värde som du kan skapa filter mot, t.ex. `CustomerCreated`, `BlobDeleted`, `HttpRequestReceived`osv. |
| **eventTime** | Den tid då händelsen genererades baserat på leverantörens UTC-tid. |
| **data** | Specifik information som är relevant för typen av händelse. Exempelvis kan en händelse om en ny fil skapas i Azure Storage innehåller information om filen, till exempel den `lastTimeModified` värde. Eller en händelse i Event Hubs har URL: en för filen avbildning. Det här fältet är valfritt. |
| **dataVersion** | Dataobjektets schemaversion. Utgivaren definierar schemaversion. |
| **metadataVersion** | Schemaversion för händelsemetadata. Event Grid definierar schemat för de översta egenskaperna. Event Grid ger det här värdet. |

> [!TIP]
> Event Grid skickar en händelse att ange något har hänt eller ändrats. Men den _faktiska objektet_ som har ändrats är inte en del av informationen om händelsen. I stället skickas en URL eller identifierare ofta för att referera till det ändrade objektet.

### <a name="what-is-an-event-source"></a>Vad är en händelsekälla?
Händelsekällor ansvarar för att skicka händelser till Event Grid. Varje händelsekälla är relaterat till en eller flera händelsetyper. Till exempel är Azure Storage händelsekällan för blob skapas händelser. IoT Hub är händelsekällan för enheten som skapade händelser. Programmet är händelsekällan för anpassade händelser som du definierar. Vi ska titta på händelsekällor i detalj om en stund.

> [!NOTE]
> Azure Event Hub definierar också konceptet med en Händelseutfärdare. En utgivare till Event Hub är användare eller organisation som bestämmer sig för att skicka händelser till Event Grid. Till exempel publicerar Microsoft händelser för flera Azure-tjänster. Du kan publicera händelser från ditt eget program. Organisationer som är värdar för tjänster utanför Azure kan publicera händelser via Event Grid. Detta hämtar ofta blandat med händelsekällan. Händelsekällan fullständigt definierat är utgivaren och den specifika tjänsten som orsakar händelsen för utgivaren. Här kan vi använder ”publisher” och ”händelsekälla” synonymt för att representera den entitet som meddelandet skickades till Händelsehubben.

### <a name="what-is-an-event-topic"></a>Vad är ett Händelseämne?
Event ämnen kategorisera händelser i grupper. Ämnen som representeras av en offentlig slutpunkt och är där händelsekällan skickar händelser _till_. När du skapar ditt program bör bestämma du hur många avsnitt för att skapa. Större lösningar skapar ett anpassat ämne för varje kategori av relaterade händelser, medan mindre lösningar kan skicka alla händelser till ett ämne. Tänk dig ett program som skickar händelser relaterade till ändra användarkonton och behandla beställningar. Det är osannolikt alla händelsehanteraren vill båda typer av händelser. Skapa två anpassade ämnen och låt händelsehanterare prenumerera på det som intresserar dem. Händelseprenumeranter filtrera händelsetyper som de vill från ett visst ämne.

Ämnen är indelade i **system** ämnen, och **anpassade** ämnen.

#### <a name="system-topics"></a>System-ämnen
System-avsnitten är inbyggda avsnitt som tillhandahålls av Azure-tjänster. Du ser inte system ämnen i Azure-prenumerationen eftersom utgivaren äger i avsnitt, men du kan prenumerera på dem. Om du vill prenumerera på, anger du information om resursen som du vill ta emot händelser från. Så länge som du har åtkomst till resursen, kan du prenumerera på händelser.

#### <a name="custom-topics"></a>Anpassade ämnen
Anpassade ämnen är program och från tredje part ämnen. När du skapar eller tilldelas åtkomst till ett anpassat ämne, visas detta anpassade ämne i din prenumeration.

### <a name="what-is-an-event-subscription"></a>Vad är en händelseprenumeration?
Händelseprenumerationer definierar en händelsehanterare vill få vilka händelser på ett ämne. En prenumeration kan också filtrera händelserna efter typ eller ämne, så att du kan kontrollera att relevanta händelser bara tar emot en händelsehanterare.

### <a name="what-is-an-event-handler"></a>Vad är en händelsehanterare?
En händelsehanterare (kallas ibland en händelse ”prenumerant”) finns någon komponent (program och resurser) som kan ta emot händelser från Event Grid. Till exempel kan Azure Functions köra kod som svar på den nya låt som läggs till i Blob-lagringskontot. Prenumeranter kan välja vilka händelser de vill hantera, så meddelar Event Grid på ett effektivt sätt alla berörda prenumeranter när en ny händelse är tillgänglig – det krävs ingen avsökning.

## <a name="types-of-event-sources"></a>Typer av händelsekällor
Händelser kan genereras av följande Azure-resurstyper:

- **Azure-prenumerationer och resursgrupper.** Prenumerationer och resursgrupper genererar händelser som är relaterade till hanteringsåtgärder i Azure. När en användare skapar en virtuell dator genererar exempelvis den här källan en händelse.
- **Behållarregister.** Azure Container Registry-tjänsten genererar händelser när avbildningarna i registret läggs till, tas bort eller ändrats.
- **Event Hub.** Event Hub kan användas för att bearbeta och lagra händelser från olika datakällor - vanligtvis loggning eller telemetri som är relaterade. Event Hub kan generera händelser till Event Grid när filer avbildas.
- **Service Bus.** Service bus kan generera händelser till Event Grid när det finns aktiva meddelanden med inga aktiva lyssnare.
- **Lagringskonton.** Lagringskonton kan generera händelser när användarna lägger till blobbar, filer, tabellposter eller kömeddelanden. Du kan använda både blob-konton och General-purpose V2-konton som händelsekällor.
- **Media Services.** Media Services är värd för video- och ljudmedia och innehåller avancerade hanteringsfunktioner för mediefiler. Media Services kan generera händelser när ett kodningsjobb startas eller slutförs i en videofil.
- **Azure IoT Hub.** IoT Hub kommunicerar med och samlar in telemetri från IoT-enheter. Den kan generera händelser när sådan kommunikation tas emot.
- **Anpassade händelser.** Anpassade händelser kan genereras med hjälp av REST API eller med Azure SDK för Java, GO, .NET, Node, Python och Ruby. Du kan till exempel skapa en anpassad händelse i Web Apps-funktionen i Azure App Service. Detta kan inträffa i arbetsrollen när den hämtar ett meddelande från en lagringskö.

Den här djupgående integrering med olika händelsekällor inom Azure säkerställer att Event Grid kan du distribuera händelser som är relaterade till nästan alla Azure-resurs.

## <a name="event-handlers"></a>Händelsehanterare
Följande objekttyper i Azure kan ta emot och hantera händelser från Event Grid:

- **Azure Functions.** En Azure-funktion består av anpassad kod som körs i Azure utan någon virtuell värdserver eller container. Använd en Azure-funktion som en händelsehanterare när du vill koda ett anpassat svar på händelsen.
- **Webhooks.** En webhook är ett webb-API som implementerar en push-arkitektur.
- **Azure Logic Apps.** En Azure-logikapp är värd för en affärsprocess som ett arbetsflöde.
- **Microsoft Flow.** Flow är också värd för arbetsflöden men är enklare för icke-teknisk personal att använda.

## <a name="should-you-use-event-grid"></a>Bör du använda Event Grid?
Använd Event Grid när du behöver dessa funktioner:

- **Enkelhet.** Det är enkelt att ansluta källor till prenumeranter i Event Grid.
- **Avancerad filtrering.** Prenumerationer har noggrann kontroll över de händelser som de tar emot från ett ämne.
- **Förgrening.** Du kan prenumerera på ett obegränsat antal slutpunkter samma händelser och ämnen.
- **Tillförlitlighet.** Event Grid försöker upprepat leverera händelser i upp till 24 timmar för varje prenumeration.
- **Betala per händelse.** Betala endast för det antal händelser som du överför.

## <a name="summary"></a>Sammanfattning
Event Grid är ett enkelt och mångsidigt distributionssystem för händelser. Du kan använda det till att leverera distinkta händelser till prenumeranter, som får händelserna på ett pålitligt och snabbt sätt. Vi har ytterligare en meddelandemodell att titta på – hur gör vi om vi vill leverera en stor _ström_ med händelser? I det här scenariot är Event Grid inte den ideala lösningen eftersom den är utformad för leverans av en händelse i taget. I stället vänder vi oss till en annan Azure-tjänst: Event Hubs.