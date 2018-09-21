Det första vi måste göra är att skapa en tom Azure Cosmos DB-databas och en samling som vi kan arbeta med. Vi vill att den motsvarar den du skapade i den föregående modulen i den här utbildningsvägen: en databas med namnet **”Produkter”** och en samling med namnet **”Kläder”**. Använd följande instruktioner och Azure Cloud Shell på höger sida av skärmen om du vill återskapa databasen.

## <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a>Skapa ett Azure Cosmos DB-konto och en Cosmos DB-databas med Azure CLI

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="select-a-subscription"></a>Välja en prenumeration

Om du har använt Azure ett tag kan du ha flera tillgängliga prenumerationer. Så är det ofta för utvecklare, som kan ha en prenumeration för Visual Studio och en annan för företagets resurser.

Azure Sandbox har redan valt Concierge-prenumerationen för dig i Cloud Shell. Du kan bekräfta prenumerationsinställningen med hjälp av de här anvisningarna. När du arbetar med din egen prenumeration kan du också byta prenumeration med Azure CLI med hjälp av följande anvisningar.

1. Börja med att visa en lista över tillgängliga prenumerationer.

    ```azurecli
    az account list --output table
    ```

   Om du arbetar med en Concierge-prenumeration bör det vara den enda prenumerationen som listas.

1. Om du sedan inte vill använda standardprenumerationen kan du ändra den med kommandot `account set`:

    ```azurecli
    az account set --subscription "<subscription name>"
    ```
    
1. Hämta den resursgrupp som har skapats för dig i sandbox-miljön. Om du använder en egen prenumeration hoppar du över det här steget och anger istället ett unikt namn som du vill använda i miljövariabeln `RESOURCE_GROUP` nedan. Anteckna namnet på resursgruppen. Det är här vi ska skapa vår databas.

    ```azurecli
    az group list --out table
    ```
### <a name="setup-environment-variables"></a>Konfigurera miljövariabler

1. Ange några miljövariabler så att du inte behöver skriva in de vanliga värdena varje gång. Starta genom att ange ett namn för Azure Cosmos DB-konto, till exempel `export NAME="mycosmosdbaccount"`. Fältet får bara innehålla gemener, siffror och bindestreck och måste vara mellan 3 och 31 tecken.

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

1. Ange resursgrupp för att använda den befintliga resursgruppen för sandbox-miljön.

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    ```

1. Välj regionen som är närmast dig och ställ in miljövariabeln, till exempel `export LOCATION="EastUS"`.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

1. Ange en variabel för databasens namn. Kalla den ”Produkt” så att den stämmer överens med databasen som vi skapade i föregående modul.

    ```azurecli
    export DB_NAME="Products"
    ```

### <a name="create-a-resource-group-in-your-subscription"></a>Skapa en resursgrupp i din prenumeration

När du skapar en Cosmos DB i din egen prenumeration bör du skapa en ny resursgrupp som rymmer alla relaterade resurser.

> [!IMPORTANT]
> Om du använder Azure Sandbox som tillhandahålls via Microsoft Learn behöver du inte utföra det här steget. Kontrollera i stället att variabeln `RESOURCE_GROUP` ovan har angetts för **<rgn>[Resursgruppnamn för sandbox-miljö</rgn>**.

I din egen prenumeration använder du följande kommando för att skapa resursgruppen. 

```azurecli
az group create --name <name> --location <location>
```

### <a name="create-the-azure-cosmos-db-account"></a>Skapa Azure Cosmos DB-kontot

1. Skapa Azure Cosmos DB-kontot med kommandot `cosmosdb create`. Kommandot använder följande parametrar och kan köras utan ändringar om du anger miljövariablerna enligt dessa rekommendationer.
    - `--name`: Unikt namn på resursen.
    - `--kind`: Typ av databas, använd _GlobalDocumentDB_.
    - `--resource-group`: Resursgruppen. Använd **<rgn>[Resursgrupp för sandbox-miljö]</rgn>**. Den bör tilldelas variabeln `RESOURCE_GROUP`.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

    Det tar ett par minuter för kommandot att slutföras. Inställningarna för det nya kontot visas i Cloud Shell när det har distribuerats.

1. Skapa databasen `Products` i kontot.

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

1. Avslutningsvis skapar du samlingen `Clothing`.

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

Nu när vi har vårt Azure Cosmos DB-konto, vår databas och vår samling är det dags att lägga till lite data!