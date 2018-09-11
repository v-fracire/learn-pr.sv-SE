> [!NOTE]
> Om du fortsätter från **Skapa en Azure Cosmos DB-databas som kan skalas** och inte tog bort Cosmos DB-databasen och samlingen som du skapade kan du hoppa över den här kursdelen och fortsätta med att lägga till data med Datautforskaren.

Det första vi måste göra är att skapa en tom Cosmos DB-databas och en samling som vi kan arbeta med. Vi vill att den motsvarar den du skapade i den föregående modulen i den här utbildningsvägen: en databas med namnet **”Produkter”** och en samling med namnet **”Kläder”**. Använd följande instruktioner och Azure Cloud Shell på höger sida av skärmen om du vill återskapa databasen.

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a>Skapa ett Cosmos DB-konto och en Cosmos DB-databas med Azure CLI

1. Börja med att välja rätt prenumeration – välj det prenumerations-ID som är kopplat till din kostnadsfria prenumeration för åtkomst till utbildning.

    ```azurecli
    az account list --output table
    ```

1. Kontrollera att sandbox-miljö eller begränsat läge visas i prenumerationslistan, och ange att det är den som ska användas nu: <!-- TODO: get official name here -->

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. Hämta den resursgrupp som har skapats för dig. Om du använder en egen prenumeration hoppar du över det här steget och ger i stället ett unikt namn som du vill använda i miljövariabeln `RESOURCE_GROUP` nedan. Anteckna namnet på resursgruppen. Det är här vi ska skapa vår databas. <!-- Do we get a token for this? -->

    ```azurecli
    az group list --out table
    ```

1. Gör det lite enklare genom att ange några miljövariabler så att du inte behöver skriva in värden som är vanliga varje gång. 

    > [!IMPORTANT]
    > Se till att ändra de här värdena till lämpliga värden för sessionen. Ersätt till exempel värdet `<resource group>` med namnet på den resursgrupp som du identifierade ovan.

    ```azurecli
    export RESOURCE_GROUP="<resource group>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```
    
1. Ange sedan en variabel för databasens namn. Kalla den ”Användare” så att den stämmer överens med databasen som vi skapade i föregående modul.

    ```azurecli
    export DB_NAME="Products"
    ```
    
1. Om du gör det här med din egen prenumeration och använder en _ny_ resursgrupp (rekommenderas) skapar du resursgruppen med följande kommando. **Viktigt:** Om du använder de kostnadsfria utbildningsresurserna som tillhandahålls via Microsoft Learn behöver du inte utföra det här steget. Kontrollera i stället att variabeln `RESOURCE_GROUP` ovan har angetts för din tilldelade resursgrupp.

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. Skapa sedan Cosmos DB-kontot. Det här kan ta några minuter.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```
    
1. Skapa databasen `Products` i kontot.

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```
    
1. Avslutningsvis skapar du samlingen `Clothing`.

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

Nu när vi har vårt Cosmos DB-konto, vår databas och vår samling är det dags att lägga till lite data!