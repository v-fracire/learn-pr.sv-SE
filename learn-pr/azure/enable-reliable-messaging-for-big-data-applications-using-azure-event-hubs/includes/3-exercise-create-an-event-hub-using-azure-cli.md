Du är nu redo att skapa en ny händelsehubb. När du har skapat händelsehubben använder du Azure Portal för att visa den nya hubben.

Du ska skapa en händelsehubb med hjälp av Azure CLI. I den här övningen ska du använda Azure CLI 2.0. 

## <a name="create-an-event-hubs-namespace"></a>Skapa ett Event Hubs-namnområde

Följ stegen nedan för att skapa ett Event Hubs-namnområde med hjälp av Bash-gränssnittet som stöds av Azure Cloud Shell:

1. Logga in till Cloud Shell (Bash).  

1. Skapa en Azure-resursgrupp med hjälp av följande kommando:

    ```azurecli
        az group create --name <resource group name> --location <location>
    ```

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |--name (krävs)      |Ange ett nytt resursgruppsnamn.|
    |--location (krävs)     |Ange platsen för ditt närmaste Azure-datacenter, till exempel westus (västra USA).|

1. Skapa Event Hubs-namnområdet med hjälp av följande kommando:

    ```azurecli
        az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |--name (krävs)      |Ange ett unikt namn för Event Hubs-namnområdet på mellan 6 och 50 tecken. Namnet får endast innehålla bokstäver, siffror och bindestreck. Det måste börja med en bokstav och sluta med en bokstav eller siffra.|
    |--resource-group (krävs)  |Ange resursgruppen som du skapade i steg 1.
    |--l (valfritt)     |Ange platsen för ditt närmaste Azure-datacenter, till exempel westus (västra USA).|

1. Hämta anslutningssträngen för Event Hubs-namnområdet med hjälp av följande kommando. Du behöver den för att konfigurera program att skicka och ta emot meddelanden med hjälp av din händelsehubb.

    ```azurecli
        az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |--resource-group (krävs)  |Ange resursgruppen som du skapade i steg 1.|
    |--namespace-name (krävs)      |Ange namnområdet som du skapade i steg 2.|

    Det här kommandot returnerar anslutningssträngen för Event Hubs-namnområdet som du ska använda senare för att konfigurera dina program för utgivare och konsumenter. Spara värdet för följande nycklar för användning senare.

    - **primaryConnectionString**
    - **primaryKey**

## <a name="create-an-event-hub"></a>Skapa en händelsehubb

Följ stegen nedan för att skapa din nya händelsehubb:

1. Skapa en ny händelsehubb med hjälp av följande kommando:

    ```azurecli
        az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |--name (krävs)  |Ange ett namn för din händelsehubb.|
    |--resource-group (krävs)  |Ange resursgruppen som du skapade i föregående procedur.|
    |--namespace-name (krävs)      |Ange namnområdet som du skapade i föregående procedur.|

1. Visa information om händelsehubben med hjälp av följande kommando: 

    ```azurecli
        az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>
    ```

    |Parameter      |Beskrivning|
    |---------------|-----------|
    |--resource-group (krävs)  |Ange resursgruppen som du skapade i föregående procedur.|
    |--namespace-name (krävs)      |Ange namnområdet som du skapade i föregående procedur.|
    |--name (krävs)|Ange namnet på händelsehubben som du skapade i steg 1.|

## <a name="view-the-event-hub-in-the-azure-portal"></a>Visa händelsehubben på Azure Portal

Följ stegen nedan för att visa händelsehubben på Azure Portal.

1. Leta upp Event Hubs-namnområdet med hjälp av sökfältet överst på [Azure Portal](https://portal.azure.com?azure-portal=true).

1. Klicka på namnområdet för att öppna det.

1. Klicka på **Event Hubs** från **Event Hubs-namnområde** > **ENTITETER**.
    Din händelsehubb visas med statusen **Aktiv** och standardvärden för **Kvarhållning av meddelanden** (*7*) och **Antal partitioner** (*4*).

    ![Händelsehubb på Azure Portal](../media-draft/3-event-hub.png)

## <a name="summary"></a>Sammanfattning

Nu har du skapat en ny händelsehubb och har all nödvändig information som krävs för att konfigurera dina program för utgivare och konsumenter.
