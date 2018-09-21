Många program använder en publicera-prenumerera-modell för att meddela distribuerade komponenter om att något har hänt eller att ett objekt har ändrats. Anta att du har ett program för musikdelning med ett webb-API som körs i Azure. När en ny låt laddas upp av en användare måste du meddela alla mobilappar som finns installerade på enheter för användare över hela världen som är intresserade av den genre.

I den här arkitekturen behöver utgivaren av ljudfilen inte känna till de prenumeranter som är intresserade av den musik som delas. Dessutom vill vi ha en en-till-många-relation där vi kan ha flera prenumeranter som kan välja om de är intresserade av den här nya låten. Azure Event Grid är en perfekt lösning för den här typen av arkitektur.

## <a name="what-is-azure-event-grid"></a>Vad är Azure Event Grid?
Azure Event Grid är en helt hanterad tjänst för händelsedirigering som körs ovanpå Azure Service Fabric. Event Grid är en tjänst som distribuerar _händelser_ från olika källor, till exempel Azure Blob-lagringskonton eller Azure Media Services, till olika prenumeranter, till exempel Azure Functions eller Webhooks. Event Grid har skapats för att göra det enklare att skapa händelsebaserade och serverfria program i Azure.

Event Grid stöder de flesta Azure-tjänster som utgivare eller prenumerant och kan även användas med tjänster från tredje part. Det tillhandahåller ett dynamiskt skalbart meddelandesystem till låg kostnad som gör att utgivare kan meddela prenumeranter om statusändringar. I följande bild visas hur Azure Event Grid tar emot meddelanden från flera källor och distribuerar dem till händelsehanterare baserat på prenumeration.

Det finns flera koncept i Azure Event Grid som ansluter en källa till en prenumerant:

1. **Händelser** – Det som hände.
1. **Händelsekällor** – där händelsen ägde rum.
1. **Ämnen** – Slutpunkten som utgivarna skickar händelser till.
1. **Händelseprenumerationer** – Den slutpunkt eller inbyggda mekanism som dirigerar händelser, ibland till flera hanterare. Prenumerationer används också av hanterarna för att filtrera inkommande händelser på ett intelligent sätt.
1. **Händelsehanterare** – Den app eller tjänst som reagerar på händelsen.

![En bild som visar Azure Event Grid placerat mellan fler händelsekällor och flera händelsehanterare. Händelsekällorna skickar händelser till Event Grid och Event Grid vidarebefordrar relevanta händelser till prenumeranterna. Event Grid använder ämnen för att bestämma vilka händelser som ska skickas till vilka hanterare. Händelsekällor taggar varje händelse med en eller flera händelser och händelsehanterare prenumererar på de ämnen som de är intresserade av.](../media/4-event-grid.png)

### <a name="what-is-an-event"></a>Vad är en händelse?
**Händelser** är datameddelanden som passerar genom Event Grid och som beskriver vad som har inträffat. Varje händelse är självständig, kan vara upp till 64 KB och innehåller flera olika typer av information baserat på ett schema som definierats i Event Grid:

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
| **Avsnitt** | Den fullständiga resurssökvägen till händelsekällan. Event Grid ger det här värdet. |
| **Ämne** | Utgivardefinierad sökväg till händelseobjektet. |
| **id** | Den unika identifieraren för händelsen. |
| **Händelsetyp** | En av de registrerade händelsetyperna för den här händelsekällan. Detta är ett värde som du kan skapa filter mot, t.ex. `CustomerCreated`, `BlobDeleted`, `HttpRequestReceived`osv. |
| **Händelsetid** | Den tid då händelsen genererades baserat på leverantörens UTC-tid. |
| **Data** | Specifik information som är relevant för typen av händelse. Till exempel, en händelse som handlar om en fil som har skapats i Azure Storage innehåller information om filen, såsom värdet `lastTimeModified`. Eller en händelse i Event Hubs har URL:en för Capture-filen. Det här fältet är valfritt. |
| **Dataversion** | Dataobjektets schemaversion. Utgivaren definierar schemaversion. |
| **Metadataversion** | Schemaversionen av händelsens metadata. Event Grid definierar schemat för de översta egenskaperna. Event Grid ger det här värdet. |

> [!TIP]
> Event Grid skickar en händelse för att ange något har hänt eller ändrats. Men det _faktiska objektet_ som har ändrats är inte del av informationen om händelsen. I stället skickas en URL eller identifierare ofta för att hänvisa till det ändrade objektet.

### <a name="what-is-an-event-source"></a>Vad är en händelsekälla?
Händelsekällor ansvarar för att skicka händelser till Event Grid. Varje händelsekälla är relaterat till en eller flera händelsetyper. Till exempel är Azure Storage händelsekällan för händelser som skapas i blobbar. IoT Hub är händelsekällan för händelser som har skapats på enheter. Ditt program är händelsekällan för anpassade händelser som du definierar. Vi kommer snart att titta närmare på händelsekällor.

> [!NOTE]
> Azure Event Hub definierar också konceptet Händelseutfärdare. En utgivare på Event Hub är den användare eller organisation som bestämmer sig för att skicka händelser till Event Grid. Microsoft publicerar till exempel händelser för flera Azure-tjänster. Du kan publicera händelser från ditt eget program. Organisationer som är värdar för tjänster utanför Azure kan publicera händelser via Event Grid. Detta förväxlas ofta med händelsekällan. Definitionen av händelsekällan är utgivaren och den specifika tjänsten som skapar händelsen åt utgivaren. Här kan vi använda ”utgivare” och ”händelsekälla” synonymt för att representera den entitet som skickade meddelandet till Event Hub.

### <a name="what-is-an-event-topic"></a>Vad är ett händelseämne?
Händelseämnen samlar händelser i grupper. Ämnen som representeras av en offentlig slutpunkt och är platsen som händelsekällan skickar händelser _till_. När du skapar ditt program bör du bestämma du hur många ämnen du vill skapa. Större lösningar skapar anpassade ämnen för varje händelsekategori medan mindre lösningar kan skicka alla händelser till samma ämne. Tänk dig ett program som skickar händelser som handlar om att ändra användarkonton och bearbeta beställningar. Det är osannolikt alla händelsehanteraren vill ha båda händelsekategorier. Skapa två anpassade ämnen och låt händelsehanteraren prenumerera på det mest relevanta. Händelseprenumeranter kan filtrera händelsetyper som de vill ta emot från ett visst ämne.

Ämnen är indelade i **system**ämnen och **anpassade** ämnen.

#### <a name="system-topics"></a>Systemämnen
Systemämnen är inbyggda ämnen som tillhandahålls av Azure-tjänster. Du ser inte systemämnen i Azure-prenumerationen eftersom utgivaren äger ämnena, men du kan prenumerera på dem. Om du vill prenumerera ska du tillhandahålla information om resursen som du vill ta emot händelser från. Så länge som du har åtkomst till resursen, kan du prenumerera på händelser.

#### <a name="custom-topics"></a>Anpassade ämnen
Anpassade ämnen kommer från program och tredje part. När du skapar eller tilldelas åtkomst till ett anpassat ämne visas detta anpassade ämne i din prenumeration.

### <a name="what-is-an-event-subscription"></a>Vad är en händelseprenumeration?
Händelseprenumerationer definierar vilka händelser som händelsehanteraren vill ta emot för ett visst ämne. En prenumeration kan även filtrera händelserna efter typ eller ämne så att du kan vara säker på att en händelsehanterare endast tar emot relevanta händelser.

### <a name="what-is-an-event-handler"></a>Vad är en händelsehanterare?
En händelsehanterare (kallas ibland en ”händelseprenumerant”) är en komponent (program och resurs) som kan ta emot händelser från Event Grid. Till exempel kan Azure Functions köra kod som svar på den nya låt som läggs till i Blob-lagringskontot. Prenumeranter kan välja vilka händelser de vill hantera, så meddelar Event Grid på ett effektivt sätt alla berörda prenumeranter när en ny händelse är tillgänglig – det krävs ingen avsökning.

## <a name="types-of-event-sources"></a>Typer av händelsekällor
Händelser kan genereras av följande Azure-resurstyper:

- **Azure-prenumerationer och resursgrupper:** Prenumerationer och resursgrupper genererar händelser som är relaterade till hanteringsåtgärder i Azure. När en användare skapar en virtuell dator genererar exempelvis den här källan en händelse.
- **Behållarregister:** Tjänsten Azure Container Registry skapar händelser när bilder i registret läggs till, tas bort eller ändras.
- **Event Hub:** Event Hub kan användas för att bearbeta och lagra händelser från olika datakällor, som vanligtvis relaterar till loggning eller telemetri. Event Hub kan generera händelser till Event Grid när filer avbildas.
- **Service Bus:** Service bus kan generera händelser till Event Grid när det finns aktiva meddelanden utan aktiva lyssnare.
- **Lagringskonton:** Lagringskonton kan generera händelser när användarna lägger till blobbar, filer, tabellposter eller kömeddelanden. Du kan använda både blobbkonton och konton för generell användning V2 som händelsekällor.
- **Media Services:** Media Services är värd för video- och ljudmedia och innehåller avancerade hanteringsfunktioner för mediefiler. Media Services kan generera händelser när ett kodningsjobb startas eller slutförs i en videofil.
- **Azure IoT Hub:** IoT Hub kommunicerar med och samlar in telemetri från IoT-enheter. Den kan generera händelser när sådan kommunikation tas emot.
- **Anpassade händelser:** Anpassade händelser kan genereras med hjälp av REST API eller med Azure SDK för Java, GO, .NET, Node, Python och Ruby. Du kan till exempel skapa en anpassad händelse i Web Apps-funktionen i Azure App Service. Detta kan inträffa i arbetsrollen när den tar emot ett meddelande från en lagringskö.

Den här omfattande integreringen med olika händelsekällor inom Azure säkerställer att Event Grid kan distribuera händelser som är relaterade till nästan vilken Azure-resurs som helst.

## <a name="event-handlers"></a>Händelsehanterare
Följande objekttyper i Azure kan ta emot och hantera händelser från Event Grid:

- **Azure-funktioner:** En Azure-funktion består av anpassad kod som körs i Azure utan någon virtuell värdserver eller container. Använd en Azure-funktion som en händelsehanterare när du vill koda ett anpassat svar på händelsen.
- **Webhooks:** En webhook är ett webb-API som implementerar en push-arkitektur.
- **Azure Logic Apps:** En Azure-logikapp är värd för en affärsprocess som ett arbetsflöde.
- **Microsoft Flow:** Flow är också värd för arbetsflöden men är enklare för icke-teknisk personal att använda.

## <a name="should-you-use-event-grid"></a>Bör du använda Event Grid?
Använd Event Grid när du behöver dessa funktioner:

- **Enkelt:** Det är mycket enkelt att ansluta källor till prenumeranter i Event Grid.
- **Avancerad filtrering:** Prenumerationer har noggrann kontroll över de händelser som de tar emot från ett ämne.
- **Sprid ut:** Du kan prenumerera på ett obegränsat antal slutpunkter till samma händelser och ämnen.
- **Tillförlitlighet** Event Grid försöker leverera händelser i upp till 24 timmar för varje prenumeration.
- **Betala per händelse** Betala endast för antalet händelser som du överför.

Event Grid är ett enkelt och mångsidigt distributionssystem för händelser. Du kan använda det till att leverera distinkta händelser till prenumeranter, som får händelserna på ett pålitligt och snabbt sätt. Vi har ytterligare en meddelandemodell att titta på – hur gör vi om vi vill leverera en stor _ström_ med händelser? I det här scenariot är Event Grid inte den ideala lösningen eftersom den är utformad för leverans av en händelse i taget. I stället vänder vi oss till en annan Azure-tjänst: Event Hubs.