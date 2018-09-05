Nu är du redo att konfigurera dina utgivar- och konsumentappar för din händelsehubb.

I den här övningen ska du konfigurera dessa appar för att skicka eller ta emot meddelanden via din händelsehubb. Dessa appar lagras på en GitHub-lagringsplats.

Du konfigurerar två separata appar, en fungerar som avsändare (**SimpleSend**), den andra som mottagare (**EventProcessorSample**) av meddelanden. Det här är Java-appar, vilket gör att du kan göra allt i webbläsaren. Men samma konfiguration krävs för alla plattformar, till exempel .NET.

## <a name="create-a-general-purpose-standard-storage-account"></a>Skapa ett allmänt standardlagringskonto

Java-mottagarappen, som du konfigurerar i den här enheten, lagrar meddelanden i Azure Blob Storage. Blob Storage kräver ett lagringskonto.

1. Skapa ett lagringskonto (generell användning V2) i en resursgrupp med följande kommando:

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name>  --location <location> --sku Standard_RAGRS --encryption blob
    ```

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |--name (krävs)  |Ange ett namn för lagringskontot.|
    |--resource-group (krävs)  |Ange resursgruppen som du skapade i föregående enhet.|
    |--location (valfritt)    |Ange den plats som du använde för att skapa resursgruppen i den föregående enheten.|

1. Lista över alla åtkomstnycklar som är associerade med ditt lagringskonto med följande kommando:

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |--account-name (krävs)  |Ange namnet på lagringskontot.|
    |--resource-group (krävs)  |Ange resursgruppen som du skapade i föregående enhet.|

     Åtkomstnycklar som är associerade med lagringskontot listas. Kopiera och spara värdet för **key** för framtida användning. Du behöver den här nyckeln för att få åtkomst till ditt lagringskonto.

1. Visa anslutningssträngen för ditt lagringskonto med följande kommando:

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |-n (krävs)  |Ange namnet på lagringskontot.|
    |-g (krävs)  |Ange namnet på resursgruppen.|

    Det här kommandot returnerar anslutningsinformation för lagringskontot. Kopiera och spara värdet för **connectionString**.

1. Skapa en container som kallas **messages** på lagringskontot med följande kommando. Använd **connectionString** du kopierade i föregående steg:

    ```azurecli
    az storage container create -n messages --connection-string "<connection string>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a>Klona Event Hubs GitHub-databasen

Använd följande steg för att klona Event Hubs GitHub-databasen.

1. Logga in på Azure Cloud Shell (Bash).

1. Källfilerna för appen du bygger i den här övningen finns på en [GitHub-lagringsplats](https://github.com/Azure/azure-event-hubs). Använd följande kommandon för att se till att du är i din arbetskatalog i Cloud Shell, och klona sedan den här lagringsplatsen:

    ```azurecli
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    Lagringsplatsen klonas till `/home/<username>/azure-event-hubs`.

## <a name="use-nano-to-edit-simplesendjava"></a>Använda nano till att redigera SimpleSend.java

Använd **nano**-redigeraren till att redigera SimpleSend-appen och lägg till Event Hubs-namnrymden, händelsehubbens namn, namnet på principen för delad åtkomst och primärnyckel. Huvudkommandon visas längst ned i redigeringsfönstret. I den här enheten måste du skriva ut dina redigeringar med CTRL+O och sedan RETUR för att bekräfta utdatafilens namn, och avsluta redigeraren med CTRL+X.

1. Ändra till mappen **SimpleSend** med hjälp av följande kommando:

    ```azurecli
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. Öppna **SimpleSend.java**-filen i **nano**-redigeraren med följande kommando:

    ```azurecli
    nano SimpleSend.java
    ```

1. Leta upp och ersätt följande strängar i nano-redigeraren:

    - `"Your Event Hubs namespace name"` med namnet på händelsehubbens namnrymd.
    - `"Your event hub"` med namnet på händelsehubben.
    - `"Your primary SAS key"` med värdet för nyckeln **primaryKey** för händelsehubbens namnrymd som du sparade tidigare.
    - `"Your policy name"` med **Välj RootManageSharedAccessKey**.
 
        När du skapar en Event Hubs-namnrymd skapas en 256-bitars SAS-nyckel med namnet **RootManageSharedAccessKey** som har ett associerat par primära och sekundära nycklar som tilldelar behörigheter för att skicka, lyssna och hantera till namnrymden. I den föregående enheten visade du nyckeln med ett Azure CLI-kommando, och du kan även hitta den här nyckeln genom att öppna sidan **Policyer för delad åtkomst** för din Event Hubs-namnrymd i Azure Portal.

    ![Konfigurationsinformation för avsändarappen](../media-draft/5-sender-configure.png)

1. Spara **SimpleSend.java** med hjälp av följande kommando och avsluta nano:

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## <a name="use-maven-to-build-simplesendjava"></a>Använda Maven för att bygga SimpleSend.java

Nu ska du bygga Java-appen med kommandot **mvn**.

1. Ändra till huvudmappen för **SimpleSend** med hjälp av följande kommando:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. Bygg Java SimpleSend-appen med följande kommando. Det här ser till att appen använder anslutningsinformationen för händelsehubben:

    ```azurecli
    mvn clean package -DskipTests
    ```

    Byggprocessen kan ta flera minuter att slutföra. Kontrollera att meddelandet **[INFO] BUILD SUCCESS** visas innan du fortsätter.

    ![Byggresultat för avsändarappen](../media-draft/5-sender-build.png)

## <a name="use-nano-to-edit-eventprocessorsamplejava"></a>Använda nano till att redigera EventProcessorSample.java

Nu ska du konfigurera en app av typen **mottagare** (kallas även **prenumeranter** eller **konsumenter**) för att mata in data från händelsehubben.

För mottagarappen finns det två tillgängliga metoder, **EventHubReceiver** och **EventProcessorHost**. EventProcessorHost byggs ovanpå EventHubReceiver men har ett enklare programmatiskt gränssnitt än EventHubReceiver. EventProcessorHost kan distribuera meddelandepartitioner till flera instanser av EventProcessorHost med samma lagringskonto.

I den här enheten använder du metoden EventProcessorHost. Du använder återigen nano och redigerar appen EventProcessorSample för att lägga till Event Hubs-namnrymden, händelsehubbens namn, namn och primärnyckel för principen för delad åtkomst, lagringskontots namn, anslutningssträng och containernamn.

1. Ändra mappen **EventProcessorSample** med följande kommando:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. Öppna filen **EventProcessorSample.java** i **nano**-redigeraren med följande kommando:

    ```azurecli
    nano EventProcessorSample.java
    ```
1. Leta upp och ersätt följande strängar i nano-redigeraren:

    - `----ServiceBusNamespaceName----` med namnet på Event Hubs-namnrymden.
    - `----EventHubName----` med namnet på händelsehubben.
    - `----SharedAccessSignatureKeyName----` med **Välj RootManageSharedAccessKey**.
    - `----SharedAccessSignatureKey----` med värdet för nyckeln **primaryKey** för Event Hubs-namnrymden som du sparade tidigare.
    - `----AzureStorageConnectionString----` med lagringskontots anslutningssträng som du sparade tidigare.
    - `----StorageContainerName----` med **messages**.
    - `----HostNamePrefix----` med namnet på ditt lagringskonto.

    ![Konfigurationsinformation för mottagarapp](../media-draft/5-receiver-configure.png)

1. Spara **EventProcessorSample.java** med följande kommando och avsluta nano:

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## <a name="use-maven-to-build-eventprocessorsamplejava"></a>Använda Maven för att bygga EventProcessorSample.java

1. Ändra huvudmappen för **EventProcessorSample** med följande kommando:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. Bygg Java SimpleSend-appen med följande kommando. Det här ser till att appen använder anslutningsinformationen för händelsehubben:

    ```azurecli
    mvn clean package -DskipTests
    ```

    Byggprocessen kan ta flera minuter att slutföra. Kontrollera att meddelandet **[INFO] BUILD SUCCESS** visas innan du fortsätter.

    ![Byggresultat för mottagarapp](../media-draft/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a>Starta avsändar- och mottagarappen

1. Kör Java-appen via kommandoraden genom att använda **java**-kommandot och ange ett .jar-paket. Använd följande kommandon för att starta SimpleSend-appen:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. Tryck på RETUR när du ser **Överföringen är klar...**.

    ![Körningsresultat för avsändarappen](../media-draft/5-sender-run.png)

1. Starta EventProcessorSample-appen med följande kommando.

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. När meddelanden slutar visas i konsolen trycker du på RETUR.

    ![Körningsresultat för mottagarapp](../media-draft/5-receiver-run.png)

## <a name="summary"></a>Sammanfattning

Nu har du konfigurerat en avsändarapp redo att skicka meddelanden till händelsehubben. Du har även konfigurerat en mottagarapp redo att ta emot meddelanden från händelsehubben.
