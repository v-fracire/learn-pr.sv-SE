Ditt team har beslutat sig för att utnyttja funktionerna i Azure Event Hubs för att hantera och bearbeta de ökade transaktionsvolymerna som kommer igenom systemet.

En händelsehubb är en Azure-resurs, så det första steget är att skapa en ny hubb i Azure och konfigurera den för att möta de specifika kraven i dina appar.

## <a name="what-is-an-azure-event-hub"></a>Vad är en Azure-händelsehubb?

Azure Event Hubs är en molnbaserad händelsebearbetningstjänst som kan ta emot och bearbeta flera miljoner händelser per sekund. Event Hubs fungerar som en ytterdörr för en händelsepipeline, där den tar emot inkommande data och lagrar dem tills bearbetningsresurser är tillgängliga.

En entitet som skickar data till händelsehubbarna kallas för en *utgivare* och en entitet som läser data från händelsehubbarna kallas för *konsument* eller *prenumerant*. Azure Event Hubs finns mellan dessa två entiteter för att dela upp produktionen (från utgivare) och konsumtion (till prenumerant) för en händelseström. Denna frånkoppling hjälper till att hantera scenarier när frekvensen för händelseproduktion är mycket högre än konsumtionen.

![Utgivare skickar flera händelser till en enda händelsehubb och gör data tillgängliga för prenumeranter](../media-draft/2-event-hub-overview.png "händelsehubböversikt")

### <a name="events"></a>Händelser

En **händelse** är ett lite paket som innehåller ett meddelande för en ändring och inte detaljerna för exakt vad som har ändrats. Du kan inte konfigurera en konsumentapp att initiera någon form av fråga för att ta reda på informationen, men det här är inget krav och Event Hubs hanterar inte det här. Händelser kan publiceras enskilt eller i batchar men en publicering (enskild eller batch) får inte överskrida 256 KB.

Tvärtom innehåller **meddelandet** i Message Queuing (i Azure Service Bus) data och händelsen, och utgivaren av meddelandet förväntar att konsumenten bearbetar meddelandet på ett visst sätt.

### <a name="publishers-and-subscribers"></a>Utgivare och prenumeranter

Händelseutgivare är en app eller enhet som kan skicka händelsedata med HTTPS eller Advanced Message Queuing Protocol (AQMP) 1.0. 

För utgivare som skickar data ofta har AMQP bättre prestanda. Men det innebär högre initiala sessionskostnader, eftersom en beständig dubbelriktad socket och TLS (Transport Layer Security) eller SSL/TLS måste konfigureras först. 

För mer återkommande publicering är HTTPS det bästa alternativet. HTTPS kräver ytterligare SSL-omkostnader för varje begäran men här finns inga kostnader för sessionsinitiering.

> [!NOTE] 
> Befintliga Kafka-baserade appar som använder Apache Kafka 1.0 och nyare klientversioner kan också fungera som Event Hubs-utgivare.

Händelseprenumeranter är appar som använder någon av två programmetoder för att ta emot och bearbeta händelser från en händelsehubb.

- **EventHubReceiver** – en enkel metod som ger obegränsade hanteringsalternativ.
- **EventProcessorHost** – en effektiv metod som vi använder senare i den här modulen.

### <a name="consumer-groups"></a>Konsumentgrupper

En **konsumentgrupp** för händelsehubbar representerar en specifik vy över en händelsehubbdataström. Genom att använda separata konsumentgrupper kan flera prenumerantappar bearbeta en händelseström separat och utan att påverka andra appar. Men användningen av flera konsumentgrupper är inte ett krav och för många appar räcker det med en standardkonsumentgrupp.

### <a name="pricing"></a>Prissättning

Det finns prisnivåer för Azure Event Hubs: Basic, Standard och Dedicated. Nivåerna skiljer sig när det gäller anslutningar som stöds, antalet tillgängliga konsumentgrupper och dataflöde. När du använder Azure CLI till att skapa en Event Hubs-namnrymd och inte anger en prisnivå tilldelas **Standard** (20 konsumentgrupper, 1 000 förmedlade anslutningar).

## <a name="creating-and-configuring-a-new-azure-event-hubs"></a>Skapa och konfigurera en ny Azure Event Hubs

Att skapa en ny Azure Event Hubs består av två huvudsteg. Det första steget är att definiera en Event Hubs-**namnrymd**. Det andra steget är att skapa en händelsehubb i den namnrymden.

### <a name="defining-an-event-hubs-namespace"></a>Definiera en Event Hubs-namnrymd

En Event Hubs-namnrymd är en innehållsentitet för att hantera en eller flera händelsehubbar. 

Att skapa en Event Hubs-namnrymd innebär normalt följande:

1. Definiera inställningar på namnrymdsnivå. Vissa inställningar, som namnrymdskapacitet (konfigureras med **dataflödesenheter**), prisnivå och prestandamått definieras på namnrymdsnivå. Detta gäller för alla händelsehubbar i den namnrymden. Om du inte definierar dessa inställningar används ett standardvärde: *1* för kapacitet och *Standard* för prisnivå.

    Du kan inte ändra dataflödesenheten när du angett den. Du måste balansera konfigurationen mot dina Azure-budgetförväntningar. Du kan överväga att konfigurera olika händelsehubbar för olika dataflödeskrav. Om du till exempel har en app för försäljningsdata och du planerar att ha två händelsehubbar, en för insamling med högt dataflöde av telemetri för försäljningsdata i realtid och en för sällan förekommande händelselogginsamling, skulle det vara en bra idé att använda en separat namnrymd för varje hubb. På så sätt behöver du bara konfigurera (och betala för) kapacitet för högt dataflöde på telemetrihubben.

1. Välj ett unikt namn för namnrymden. Du kommer åt namnrymd via följande URL: *_namnrymd_.servicebus.windows.net*

1. Definiera följande valfria egenskaper:

    - Aktivera Kafka. Det här aktiverar Kafka-apparna att publicera händelser på händelsehubben.
    - Gör den här namnrymdszonen redundant. Zonredundans replikerar data i olika datacenter med egen oberoende infrastruktur för ström, nätverk och kylning.
    - Aktivera automatisk ökning och Öka automatiskt högst antal dataflödesenheter. Automatisk ökning ger ett automatiskt uppskalningsalternativ genom att öka antalet dataflödesenheter upp till ett maxvärde. Det är användbart för att undvika begränsning i situationer när inkommande eller utgående datahastigheter överskrider det angivna antalet dataflödesenheter.

### <a name="azure-cli-commands-for-creating-an-event-hubs-namespace"></a>Azure CLI-kommandon för att skapa en Event Hubs-namnrymd

Använd följande kommandon för att skapa en Event Hubs-namnrymd:

1. I Azure är en resursgrupp en container med relaterade Azure-resurser som underlättar hanteringen. Om det behövs skapar du en ny resursgrupp för din händelsehubb.

    ```azurecli
    az group create --name <resource group name> --location <location>
    ```

1. Skapa Event Hubs-namnrymden, med samma resursgrupp och plats som i föregående steg.

    ```azurecli
    az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

1. Alla händelsehubbar inom samma Event Hubs-namnrymd delar samma autentiseringsuppgifter för anslutning. Du behöver dessa autentiseringsuppgifter när du konfigurerar appar för att skicka och ta emot meddelanden med händelsehubben. Använd följande kommando för att returnera anslutningssträngen för Event Hubs-namnrymden, med samma resursgrupp och Event Hubs-namnrymdsnamn som tidigare.

    ```azurecli
    az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

### <a name="configuring-a-new-event-hub"></a>Konfigurera en ny händelsehubb

När Event Hubs-namnrymden har skapats kan du skapa en händelsehubb. När du skapar en ny händelsehubb finns det flera obligatoriska parametrar.

Följande parametrar krävs för att skapa en händelsehubb:

- **Namn på händelsehubb** – händelsehubbnamn som är unikt i prenumerationen och:
  - är mellan 1 och 50 tecken långt
  - bara innehåller bokstäver, siffror, punkter, bindestreck och understreck
  - börjar och slutar med en bokstav eller siffra
- **Antal partitioner** – antalet partitioner som krävs i en händelsehubb (mellan 2 och 32). Det här ska vara direkt relaterat till det förväntade antalet samtidiga konsumenter. Det här kan inte ändras när hubben har skapats. Partitionen separerar en meddelandeström, så att konsument- eller mottagarappen bara behöver läsa en specifik delmängd av dataströmmen. Om den inte definieras används standardvärdet *4*.
- **Kvarhållning av meddelanden** – antalet dagar (mellan 1 och 7) som meddelanden är tillgängliga, om dataströmmen av någon anledning behöver återupprepas. Om den inte definieras används standardvärdet *7*.

Du kan även konfigurera en händelsehubb för att strömma data till ett Azure Blob Storage- eller Azure Data Lake Store-konto.

### <a name="azure-cli-commands-for-creating-an-event-hub"></a>Azure CLI-kommandon för att skapa en händelsehubb

Om du vill skapa en ny händelsehubb använder du följande kommandon:

1. Skapa en händelsehubb, med samma resursgrupp och plats som du använde när du skapade namnrymden.

    ```azurecli
    az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

1. Visa informationen för händelsehubben i namnrymden, med samma resursgrupp och händelsehubbnamn och namnrymdsnamn som tidigare.

    ```azurecli
    az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>

## Summary

To deploy Azure Event Hubs, you must configure an Event Hubs namespace and then configure the event hub itself. In the next section, you will go through the detailed configuration steps to create a new namespace and event hub.