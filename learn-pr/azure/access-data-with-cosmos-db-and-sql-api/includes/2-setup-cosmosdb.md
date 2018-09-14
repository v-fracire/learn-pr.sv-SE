> [!NOTE]
> Om du fortsätter från **Skapa en Azure Cosmos DB-databas som kan skalas** och inte tog bort Cosmos DB-databasen och samlingen som du skapade kan du hoppa över den här kursdelen och fortsätta med att lägga till data med Datautforskaren.

Det första vi måste göra är att skapa en tom Cosmos DB-databas och en samling som vi kan arbeta med. Vi vill att den motsvarar den du skapade i den föregående modulen i den här utbildningsvägen: en databas med namnet **”Produkter”** och en samling med namnet **”Kläder”**. Använd följande instruktioner och Azure Cloud Shell på höger sida av skärmen om du vill återskapa databasen.

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a>Skapa ett Cosmos DB-konto och en Cosmos DB-databas med Azure CLI

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<!--
TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well.

1. Start by selecting the correct subscription - you want to select the subscription ID associated with your free education access subscription.

    ```azurecli
    az account list --output table
    ```

1. Make sure you see "sandbox" in the subscription list and set it as the current one to use:

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. Get the Resource Group that has been created for you. If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below. Take note of the Resource Group name. This is where we will create our database.

    ```azurecli
    az group list --out table
    ```
-->

1. Ange några miljövariabler så att du inte behöver ange vanliga värden varje gång.

    > [!IMPORTANT]
    > Se till att ändra de här värdena till lämpliga värden för sessionen. Ersätt till exempel den `<comsos db name>` värdet med namnet som du vill att din Cosmos DB kan använda.

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```

2. Ange sedan en variabel för databasens namn. Kalla den ”Användare” så att den stämmer överens med databasen som vi skapade i föregående modul.

    ```azurecli
    export DB_NAME="Products"
    ```

<!-- 

TODO: Pre-sandbox text to be worked back in.

1. If you are doing this on your own subscription, and you are using a _new_ Resource Group (recommended), then use the following command to create the Resource Group. **Important:** If you are using the free education resources provided by Microsoft Learn, then you do not need to execute this step. Instead, make sure the `RESOURCE_GROUP` variable above is set to your assigned resource group.

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
-->

3. Skapa sedan Cosmos DB-kontot. Det här kan ta några minuter.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

4. Skapa databasen `Products` i kontot.

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

5. Avslutningsvis skapar du samlingen `Clothing`.

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

Nu när vi har vårt Cosmos DB-konto, vår databas och vår samling är det dags att lägga till lite data!