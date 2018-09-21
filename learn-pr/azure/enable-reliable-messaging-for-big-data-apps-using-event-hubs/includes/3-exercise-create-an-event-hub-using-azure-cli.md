Du är nu redo att skapa en ny händelsehubb. När du har skapat händelsehubben använder du Azure Portal för att visa den nya hubben.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="set-some-defaults-in-the-azure-cli"></a>Ange några standardinställningar i Azure CLI

Låt oss börja med att tillhandahålla några standardvärden för Azure CLI i Cloud Shell. Detta gör att du inte behöver skriva in dem varje gång. Framför allt ska vi nu ställa in _resursgrupp_ och _plats_. Välj en plats från nedanstående lista.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Sedan anger du följande kommando i Azure CLI, se till att ersätta platsen med en nära dig.

```azurecli
az configure --defaults group=<rgn>[Sandbox Resource Group]</rgn> location=westus2
```

## <a name="create-an-event-hubs-namespace"></a>Skapa en Event Hubs-namnrymd

Följ stegen nedan för att skapa ett Event Hubs-namnområde med hjälp av Bash-gränssnittet som stöds av Azure Cloud Shell:

1. Skapa Event Hubs-namnrymden med hjälp av kommandot `az eventhubs namespace create`. Använd följande parametrar.

    > [!div class="mx-tableFixed"]
    > |Parameter      |Beskrivning|
    > |---------------|-----------|
    > |--name (krävs)      |Ange ett unikt namn för Event Hubs-namnområdet på mellan 6 och 50 tecken. Namnet får endast innehålla bokstäver, siffror och bindestreck. Det måste börja med en bokstav och sluta med en bokstav eller siffra.|
    > |--resource-group (krävs) | Det här är den färdiga Azure Sandbox-resursgruppen från standardvärdena. |
    > |--l (valfritt)     |Ange platsen för ditt närmaste datacenter för Azure, det använder din standard.|
    > |--sku (valfritt) | Prisnivån för namnområdet [Basic | Standard], blir som standard _Standard_. Detta avgör anslutningar och konsumenttröskelvärden. |

    Ange ett namn till en miljövariabel så att vi kan återanvända den.

    ```azurecli
    NS_NAME=myEvt-HubNs1
    ````

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```azurecli
    az eventhubs namespace create --name $NS_NAME
    ```

    > [!NOTE] 
    > Azure är mycket noga namnet och CLI returnerar **Felaktig begäran** om namnet finns eller är ogiltig. Prova ett annat namn genom att ändra miljövariabeln och utfärda kommandot igen.


1. Hämta anslutningssträngen för Event Hubs-namnområdet med hjälp av följande kommando. Du behöver den för att konfigurera program att skicka och ta emot meddelanden med hjälp av din händelsehubb.

    ```azurecli
    az eventhubs namespace authorization-rule keys list --name RootManageSharedAccessKey --namespace-name $NS_NAME 
    ```

    > [!div class="mx-tableFixed"]
    > |Parameter      |Beskrivning|
    > |---------------|-----------|
    > |--resource-group (krävs)  | Det här är den färdiga Azure Sandbox-resursgruppen från standardvärdena. |
    > |--namespace-name (krävs)  | Ange namn på namnområdet som du skapade. |

    Det här kommandot returnerar ett JSON-block med anslutningssträngen för Event Hubs-namnområdet som du ska använda senare för att konfigurera dina program för utgivare och konsumenter. Spara värdet för följande nycklar för användning senare.

    - **primaryConnectionString**
    - **primaryKey**

## <a name="create-an-event-hub"></a>Skapa en händelsehubb

Följ stegen nedan för att skapa din nya händelsehubb:

1. Skapa en ny händelsehubb med hjälp av kommandot `eventhub create`. Den behöver följande parametrar:

    > [!div class="mx-tableFixed"]
    > |Parameter      |Beskrivning|
    > |---------------|-----------|
    > |--name (krävs)  |Ange ett namn för din händelsehubb.|
    > |--resource-group (krävs)  |Resursgruppens ägare.|
    > |--namespace-name (krävs)      |Ange namnområdet som du skapade.|

    Nu ska vi definiera Event Hub-namnet i en miljövariabel först.

    ```azurecli
    HUB_NAME=[name]
    ```

    ```azurecli
    az eventhubs eventhub create --name $HUB_NAME --namespace-name $NS_NAME
    ```

1. Visa information om händelsehubben med hjälp av kommandot `eventhub show`. Det tar:

    > [!div class="mx-tableFixed"]
    > |Parameter      |Beskrivning|
    > |---------------|-----------|
    > |--resource-group (krävs)  |Resursgruppens ägare.|
    > |--namespace-name (krävs)      |Ange namnområdet som du skapade.|
    > |--name (krävs)|Namnet på händelsehubben.|

    ```azurecli
    az eventhubs eventhub show --namespace-name $NS_NAME --name $HUB_NAME
    ```

## <a name="view-the-event-hub-in-the-azure-portal"></a>Visa händelsehubben på Azure Portal

Nu ska vi se hur det ser ut i Azure Portal. 

1. Logga in på [Azure Portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Leta upp Event Hubs-namnområdet med hjälp av sökfältet överst på portalen.

1. Välj namnområdet för att öppna det.

1. Välj **Event Hubs-namnområdet** under avsnittet **ENTITETER**.

1. Klicka på **Händelsehubbar**.

    Din händelsehubb visas med statusen **Aktiv** och standardvärden för **Kvarhållning av meddelanden** (*7*) och **Antal partitioner** (*4*).

    ![Händelsehubb på Azure Portal](../media/3-event-hub.png)

## <a name="summary"></a>Sammanfattning

Nu har du skapat en ny händelsehubb och har all nödvändig information som krävs för att konfigurera dina program för utgivare och konsumenter.
