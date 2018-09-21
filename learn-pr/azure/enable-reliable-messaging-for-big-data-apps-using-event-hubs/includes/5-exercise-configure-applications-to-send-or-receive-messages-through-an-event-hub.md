Nu är du redo att konfigurera dina utgivar- och konsumentappar för din händelsehubb.

I den här enheten ska du konfigurera dessa appar för att skicka eller ta emot meddelanden via din händelsehubb. Dessa appar lagras på en GitHub-lagringsplats.

Du konfigurerar två separata appar, en fungerar som avsändare (**SimpleSend**), den andra som mottagare (**EventProcessorSample**) av meddelanden. Det här är Java-appar, vilket gör att du kan göra allt i webbläsaren. Men samma konfiguration krävs för alla plattformar, till exempel .NET.

## <a name="create-a-general-purpose-standard-storage-account"></a>Skapa ett allmänt standardlagringskonto

Java-mottagarappen, som du konfigurerar i den här enheten, lagrar meddelanden i Azure Blob Storage. Blob Storage kräver ett lagringskonto.

1. Skapa ett lagringskonto (generell användning V2) med kommandot `storage account create`. Kom ihåg att vi ställer in en standardresursgrupp och -plats, så även om de här parametrarna normalt _krävs_, kan vi lämna dem.

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |--name (krävs)  | Ett namn på lagringskontot. |
    |--resource-group (krävs)  |Resursgruppens ägare. Vi använder den förinställda Sandbox-resursgruppen.|
    |--location (valfritt)    |En valfri plats om du vill ha lagringskonto på en specifik plats jämfört med platsen för resursgruppen.|

    Ange lagringskontonamn till en variabel. Det måste bestå av gemena bokstäver, siffror, med bindestreck tillåtna. Det måste också vara unikt i Azure.

    ```azurecli
    STORAGE_NAME=[name]
    ```

    Kör sedan detta kommando för att skapa lagringskontot.

    ```azurecli
    az storage account create --name $STORAGE_NAME --sku Standard_RAGRS --encryption blob
    ```

    > [!TIP]
    > Om det inte går att skapa för lagringskonto, ändra miljövariabeln och försök igen.

1. Rada upp alla åtkomstnycklar som är associerade med ditt lagringskonto med kommandot `account keys list`. Det tar ditt kontonamn och resursgruppen (som standard).

    ```azurecli
    az storage account keys list --account-name $STORAGE_NAME
    ```

     Åtkomstnycklar som är associerade med lagringskontot listas. Kopiera och spara värdet för **key** för framtida användning. Du behöver den här nyckeln för att få åtkomst till ditt lagringskonto.

1. Visa anslutningssträngen för ditt lagringskonto med följande kommando:

    ```azurecli
    az storage account show-connection-string -n $STORAGE_NAME
    ```

    Det här kommandot returnerar anslutningsinformation för lagringskontot. Kopiera och spara _värdet_ för **connectionString**. Det bör se ut ungefär så här:

    ```output
    "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storage_account_name;AccountKey=VZjXuMeuDqjCkT60xX6L5fmtXixYuY2wiPmsrXwYHIhwo736kSAUAj08XBockRZh7CZwYxuYBPe31hi8XfHlWw=="
    ```

1. Skapa en container som kallas **messages** på lagringskontot med följande kommando. Använd **connectionString** du kopierade i föregående steg:

    ```azurecli
    az storage container create -n messages --connection-string "<connection string here>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a>Klona Event Hubs GitHub-databasen

Använd följande steg för att klona Event Hubs GitHub-lagringsplatsen med `git`. Du kan köra det här direkt i Cloud Shell.

1. Källfilerna för de program som du bygger i den här enheten finns på en [GitHub-lagringsplats](https://github.com/Azure/azure-event-hubs). Använd följande kommandon för att se till att du är i din arbetskatalog i Cloud Shell, och klona sedan den här lagringsplatsen:

    ```bash
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    Databasen klonas till arbetsmappen.

## <a name="edit-simplesendjava"></a>Redigera SimpleSend.java

Vi ska använda den inbyggda kodredigeraren i Cloud Shell. Detta baseras på Monaco-redigeraren och liknar Visual Studio Code men är helt online.

Vi använder redigeraren till att ändra SimpleSend-appen och lägg till Event Hubs-namnrymden, händelsehubbens namn, namnet på principen för delad åtkomst och primärnyckel. De huvudsakliga kommandona visas längst ned i redigerarfönstret. 

Du måste skriva ut dina ändringar med hjälp av <kbd>Ctrl + O</kbd>, och sedan <kbd>RETUR</kbd> för att bekräfta namnet på utdatafilen och avsluta redigeraren med <kbd>Ctrl + X</kbd>. Redigeraren har också en ”...”-meny i det övre högra hörnet för alla redigeringskommandon.

1. Ändra till mappen **SimpleSend**.

    ```bash
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. Öppna kodredigeraren i den aktuella mappen. Detta visar en lista över filer till vänster och ett redigeringsutrymme till höger.

    ```bash
    code .
    ```

1. Öppna filen **SimpleSend.java** genom att välja den från listan.

1. Leta upp och ersätt följande strängar i redigeraren:

    - `"Your Event Hubs namespace name"` med namnet på händelsehubbens namnrymd.
    - `"Your Event Hub"` med namnet på händelsehubben.
    - `"Your policy name"` med **Välj RootManageSharedAccessKey**.
    - `"Your primary SAS key"` med värdet för nyckeln **primaryKey** för händelsehubbens namnrymd som du sparade tidigare.
 
    > [!TIP]
    > Till skillnad från terminalfönstret kan redigeraren använda vanliga kortkommandon för att kopiera och klistra in för ditt operativsystem.

    Om du har glömt vissa av dem kan du växla till terminalfönstret under redigeraren och använda kommandot `echo` för att lista ut miljövariablerna. Exempel:

    ```bash
    echo $NS_NAME
    ```
    När du skapar en Event Hubs-namnrymd skapas en 256-bitars SAS-nyckel med namnet **RootManageSharedAccessKey** som har ett associerat par primära och sekundära nycklar som tilldelar behörigheter för att skicka, lyssna och hantera till namnrymden. I den föregående enheten visade du nyckeln med ett Azure CLI-kommando, och du kan även hitta den här nyckeln genom att öppna sidan **Policyer för delad åtkomst** för din Event Hubs-namnrymd i Azure Portal.

1. Spara **SimpleSend.java** antingen via menyn ”...” eller snabbtangenten (<kbd>Ctrl + S</kbd> i Windows och Linux, <kbd>Cmd + S</kbd> på macOS).

1. Stäng redigeraren via menyn ”...” eller kortkommandot <kbd>CTRL + Q</kbd>.

## <a name="use-maven-to-build-simplesendjava"></a>Använda Maven för att bygga SimpleSend.java

Nu ska du bygga Java-appen med kommandot **mvn**.

1. Ändra tillbaka till huvudmappen **SimpleSend**.

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. Skapa SimpleSend för Java-program. Det här ser till att appen använder anslutningsinformationen för händelsehubben:

    ```bash
    mvn clean package -DskipTests
    ```

    Byggprocessen kan ta flera minuter att slutföra. Kontrollera att meddelandet **[INFO] BUILD SUCCESS** visas innan du fortsätter.

    ![Byggresultat för avsändarappen](../media/5-sender-build.png)

## <a name="edit-eventprocessorsamplejava"></a>Redigera EventProcessorSample.java

Nu ska du konfigurera en app av typen **mottagare** (kallas även **prenumeranter** eller **konsumenter**) för att mata in data från händelsehubben.

För mottagarappen finns det två tillgängliga metoder, **EventHubReceiver** och **EventProcessorHost**. EventProcessorHost byggs ovanpå EventHubReceiver men har ett enklare programmatiskt gränssnitt än EventHubReceiver. EventProcessorHost kan distribuera meddelandepartitioner till flera instanser av EventProcessorHost med samma lagringskonto.

I den här enheten använder du metoden EventProcessorHost. Du redigerar appen EventProcessorSample för att lägga till Event Hubs-namnrymden, händelsehubbens namn, namn och primärnyckel för principen för delad åtkomst, lagringskontots namn, anslutningssträng och containernamn.

1. Ändra mappen **EventProcessorSample** med följande kommando:

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. Öppna kodredigeraren.

    ```bash
    code .
    ```
    
1. Välj filen **EventProcessorSample.java**.

1. Leta upp och ersätt följande strängar i redigeraren:

    - `----ServiceBusNamespaceName----` med namnet på Event Hubs-namnrymden.
    - `----EventHubName----` med namnet på händelsehubben.
    - `----SharedAccessSignatureKeyName----` med **Välj RootManageSharedAccessKey**.
    - `----SharedAccessSignatureKey----` med värdet för nyckeln **primaryKey** för Event Hubs-namnrymden som du sparade tidigare.
    - `----AzureStorageConnectionString----` med lagringskontots anslutningssträng som du sparade tidigare.
    - `----StorageContainerName----` med **messages**.
    - `----HostNamePrefix----` med namnet på ditt lagringskonto.

1. Spara **EventProcessorSample.java** antingen via menyn ”...” eller snabbtangenten (<kbd>Ctrl + S</kbd> i Windows och Linux, <kbd>Cmd + S</kbd> på macOS).

1. Stäng redigeraren.

## <a name="use-maven-to-build-eventprocessorsamplejava"></a>Använda Maven för att bygga EventProcessorSample.java

1. Ändra huvudmappen för **EventProcessorSample** med följande kommando:

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. Bygg Java SimpleSend-appen med följande kommando. Det här ser till att appen använder anslutningsinformationen för händelsehubben:

    ```bash
    mvn clean package -DskipTests
    ```

    Byggprocessen kan ta flera minuter att slutföra. Kontrollera att meddelandet **[INFO] BUILD SUCCESS** visas innan du fortsätter.

    ![Byggresultat för mottagarapp](../media/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a>Starta avsändar- och mottagarappen

1. Kör Java-appen via kommandoraden genom att använda **java**-kommandot och ange ett .jar-paket. Använd följande kommandon för att starta SimpleSend-appen:

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. Tryck på <kbd>RETUR</kbd> när du ser **Överföringen är klar...**.

    ```output
    jar-with-dependencies.jar
    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    2018-09-18T19:42:15.146Z: Send Complete...
    ```

1. Starta EventProcessorSample-appen med följande kommando.

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. När meddelanden inte visas längre i konsolen, trycker du på <kbd>RETUR</kbd> eller <kbd>CTRL + C</kbd> för att avsluta programmet.

    ```output
    ...
    SAMPLE: Partition 0 checkpointing at 1064,19
    SAMPLE (3,1120,20): "Message 80"
    SAMPLE (3,1176,21): "Message 84"
    SAMPLE (3,1232,22): "Message 88"
    SAMPLE (3,1288,23): "Message 92"
    SAMPLE (3,1344,24): "Message 96"
    SAMPLE: Partition 3 checkpointing at 1344,24
    SAMPLE (2,1120,20): "Message 83"
    SAMPLE (2,1176,21): "Message 87"
    SAMPLE (2,1232,22): "Message 91"
    SAMPLE (2,1288,23): "Message 95"
    SAMPLE (2,1344,24): "Message 99"
    SAMPLE: Partition 2 checkpointing at 1344,24
    SAMPLE: Partition 1 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE (0,1120,20): "Message 81"
    SAMPLE (0,1176,21): "Message 85"
    SAMPLE: Partition 0 batch size was 10 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 got event batch
    SAMPLE (0,1232,22): "Message 89"
    SAMPLE (0,1288,23): "Message 93"
    SAMPLE (0,1344,24): "Message 97"
    SAMPLE: Partition 0 checkpointing at 1344,24
    SAMPLE: Partition 3 batch size was 8 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 2 batch size was 9 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    ```

## <a name="summary"></a>Sammanfattning

Nu har du konfigurerat en avsändarapp redo att skicka meddelanden till händelsehubben. Du har även konfigurerat en mottagarapp redo att ta emot meddelanden från händelsehubben.