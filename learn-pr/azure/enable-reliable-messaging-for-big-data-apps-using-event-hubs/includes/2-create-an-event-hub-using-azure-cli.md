Ditt team har beslutat sig för att utnyttja funktionerna i Azure Event Hubs för att hantera och bearbeta de ökade transaktionsvolymerna som kommer igenom systemet.

En händelsehubb är en Azure-resurs, så det första steget är att skapa en ny hubb i Azure och konfigurera den för att möta de specifika kraven i dina appar.

## <a name="what-is-an-azure-event-hub"></a>Vad är en Azure-händelsehubb?

Azure Event Hubs är en molnbaserad händelsebearbetningstjänst som kan ta emot och bearbeta flera miljoner händelser per sekund. Event Hubs fungerar som en ytterdörr för en händelsepipeline, där den tar emot inkommande data och lagrar dem tills bearbetningsresurser är tillgängliga.

En entitet som skickar data till händelsehubbarna kallas för en *utgivare* och en entitet som läser data från händelsehubbarna kallas för *konsument* eller *prenumerant*. Azure Event Hubs finns mellan dessa två entiteter för att dela upp produktionen (från utgivare) och konsumtion (till prenumerant) för en händelseström. Denna frånkoppling hjälper till att hantera scenarier när antalet händelser som genereras är mycket högre än förbrukningen. Här är en bild som visar rollen för en händelsehubb.

![En bild som visar en Azure-händelsehubb placerad mellan fyra utgivare och två prenumeranter. Händelsehubben tar emot flera händelser från utgivarna, serialiserar händelserna i dataströmmar och gör dataströmmarna tillgängliga för prenumeranterna.](../media/2-event-hub-overview.png)

### <a name="events"></a>Händelser

En **händelse** är ett litet paket med information (ett *datagram*) som innehåller ett meddelande. Händelser kan publiceras enskilt eller i batchar men en publicering (enskild eller batch) får inte överskrida 256 KB.

### <a name="publishers-and-subscribers"></a>Utgivare och prenumeranter

Händelseutgivare är en app eller enhet som kan skicka händelser med HTTPS eller Advanced Message Queuing Protocol (AQMP) 1.0.

För utgivare som skickar data ofta, har AMQP bättre prestanda. Men det innebär högre initiala sessionskostnader, eftersom en beständig dubbelriktad socket och TLS (Transport Layer Security) eller SSL/TLS måste konfigureras först. 

För mer återkommande publicering är HTTPS det bästa alternativet. Även om HTTPS kräver ytterligare SSL-omkostnader för varje begäran så finns inga kostnader för sessionsinitiering.

> [!NOTE] 
> Befintliga Kafka-baserade klienter som använder Apache Kafka 1.0 och nyare klientversioner kan också fungera som Event Hubs-utgivare.

Händelseprenumeranter är appar som använder någon av två programmetoder för att ta emot och bearbeta händelser från en händelsehubb.

- **EventHubReceiver** – en enkel metod som ger obegränsade hanteringsalternativ.
- **EventProcessorHost** – en effektiv metod som vi använder senare i den här modulen.

### <a name="consumer-groups"></a>Konsumentgrupper

En **konsumentgrupp** för en händelsehubb representerar en specifik vy över en händelsehubbdataström. Genom att använda separata konsumentgrupper kan flera prenumerantappar bearbeta en händelseström separat och utan att påverka andra appar. Men användningen av flera konsumentgrupper är inte ett krav och för många appar räcker det med en standardkonsumentgrupp.

### <a name="pricing"></a>Prissättning

Det finns prisnivåer för Azure Event Hubs: Basic, Standard och Dedicated. Nivåerna skiljer sig när det gäller anslutningar som stöds, antalet tillgängliga konsumentgrupper och dataflöde. När du använder Azure CLI för att skapa ett namnområde för Event Hubs och inte anger en prisnivå tilldelas **Standard** (20 konsumentgrupper, 1 000 förmedlade anslutningar).

## <a name="creating-and-configuring-a-new-azure-event-hubs"></a>Skapa och konfigurera en ny Azure Event Hubs

Att skapa en ny Azure Event Hubs består av två huvudsteg. Det första steget är att definiera en Event Hubs-**namnrymd**. Det andra steget är att skapa en händelsehubb i den namnrymden.

### <a name="defining-an-event-hubs-namespace"></a>Definiera en Event Hubs-namnrymd

En Event Hubs-namnrymd är en innehållsentitet för att hantera en eller flera händelsehubbar. Att skapa en Event Hubs-namnrymd innebär normalt följande:

1. Definiera inställningar på namnrymdsnivå. Vissa inställningar, som namnrymdskapacitet (konfigureras med **dataflödesenheter**), prisnivå och prestandamått definieras på namnrymdsnivå. Detta gäller för alla händelsehubbar i den namnrymden. Om du inte definierar dessa inställningar används ett standardvärde: *1* för kapacitet och *Standard* för prisnivå.

    Du kan inte ändra dataflödesenheten när du angett den. Du måste balansera konfigurationen mot dina Azure-budgetförväntningar. Du kan överväga att konfigurera olika händelsehubbar för olika dataflödeskrav. Om du till exempel har en app för försäljningsdata och du planerar att ha två händelsehubbar, en för insamling med högt dataflöde av telemetri för försäljningsdata i realtid och en för sällan förekommande händelselogginsamling, skulle det vara en bra idé att använda en separat namnrymd för varje hubb. På så sätt behöver du bara konfigurera (och betala för) kapacitet för högt dataflöde på telemetrihubben.

1. Välj ett unikt namn för namnrymden. Du kommer åt namnrymd via följande URL: *_namnrymd_.servicebus.windows.net*

1. Definiera följande valfria egenskaper:

    - Aktivera Kafka. Det här alternativet låter Kafka-appar publicera händelser på händelsehubben.
    - Gör den här namnrymdszonen redundant. Zonredundans replikerar data i olika datacenter med egen oberoende infrastruktur för ström, nätverk och kylning.
    - Aktivera automatisk ökning och Öka automatiskt högst antal dataflödesenheter. Automatisk ökning ger ett automatiskt uppskalningsalternativ genom att öka antalet dataflödesenheter upp till ett maxvärde. Det är användbart för att undvika begränsning i situationer när inkommande eller utgående datahastigheter överskrider det angivna antalet dataflödesenheter.

### <a name="azure-cli-commands-for-creating-an-event-hubs-namespace"></a>Azure CLI-kommandon som ska skapa en Event Hubs-namnrymd

Skapa en ny Event Hubs-namnrymd med `az eventhubs namespace`-kommandona. Här följer en kort beskrivning av de underordnade kommandon som vi använder i den här övningen.

| Kommando | Beskrivning |
|---------|-------------|
| `create` | Skapa Event Hubs-namnrymden. |
| `authorization-rule` | Alla händelsehubbar inom samma Event Hubs-namnrymd delar samma autentiseringsuppgifter för anslutning. Du behöver dessa autentiseringsuppgifter när du konfigurerar appar för att skicka och ta emot meddelanden med händelsehubben. Det här kommandot returnerar anslutningssträngen för din Event Hubs-namnrymd. |

### <a name="configuring-a-new-event-hub"></a>Konfigurera en ny händelsehubb

När Event Hubs-namnrymden har skapats kan du skapa en händelsehubb. När du skapar en ny händelsehubb finns det flera obligatoriska parametrar.

Följande parametrar krävs för att skapa en händelsehubb:

- **Namn på händelsehubben** – ett händelsehubbnamn som är unikt i prenumerationen och:
  - är mellan 1 och 50 tecken långt
  - bara innehåller bokstäver, siffror, punkter, bindestreck och understreck
  - börjar och slutar med en bokstav eller siffra
- **Antal partitioner** – antalet partitioner som krävs i en händelsehubb (mellan 2 och 32). Det här ska vara direkt relaterat till det förväntade antalet samtidiga konsumenter. Det här kan inte ändras när hubben har skapats. Partitionen separerar en meddelandeström, så att konsument- eller mottagarappen bara behöver läsa en specifik delmängd av dataströmmen. Om den inte definieras används standardvärdet *4*.
- **Kvarhållning av meddelanden** – antalet dagar (mellan 1 och 7) som meddelanden är tillgängliga, om dataströmmen av någon anledning behöver återupprepas. Om den inte definieras används standardvärdet *7*.

Du kan även konfigurera en händelsehubb för att strömma data till ett Azure Blob Storage- eller Azure Data Lake Store-konto.

### <a name="azure-cli-commands-for-creating-an-event-hub"></a>Azure CLI-kommandon för att skapa en händelsehubb

Om du vill skapa en ny händelsehubb med Azure CLI använder du `az eventhubs eventhub`-kommandouppsättningen. Här följer en kort beskrivning av de underordnade kommandon som vi kommer att använda:

| Kommando | Beskrivning |
|---------|-------------|
| `create` | Detta skapar händelsehubben i en angiven namnrymd. |
| `show` | Detta visar information om din händelsehubb. |

## <a name="summary"></a>Sammanfattning

Om du vill distribuera Azure Event Hubs måste du konfigurera en Event Hubs-namnrymd och sedan konfigurera själva händelsehubben. I nästa avsnitt kommer du att gå igenom de detaljerade konfigurationsstegen i skapandet av en ny namnrymd och händelsehubb.