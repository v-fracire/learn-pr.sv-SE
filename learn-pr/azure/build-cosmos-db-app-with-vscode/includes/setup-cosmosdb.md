> [!NOTE]
> Om du fortsätter på från **skapa en Azure Cosmos DB-databas som de inbyggda** eller **Insert och fråga data i Azure Cosmos DB database** och redan har ett Azure Cosmos DB-konto och sedan kan du hoppa över det här enhet och flytta på för att skapa Visual Studio Code.

Det första vi behöver göra är att skapa ett Azure Cosmos DB-konto.

# <a name="create-an-azure-cosmos-db-account"></a>Skapa ett Azure Cosmos DB-konto

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
      
1. Om du gör det här med din egen prenumeration och använder en _ny_ resursgrupp (rekommenderas) skapar du resursgruppen med följande kommando. **Viktigt:** Om du använder de kostnadsfria utbildningsresurserna som tillhandahålls via Microsoft Learn behöver du inte utföra det här steget. Kontrollera i stället att variabeln `RESOURCE_GROUP` ovan har angetts för din tilldelade resursgrupp.

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. Skapa sedan Cosmos DB-kontot. Det här kan ta några minuter.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

Nu när vi har vår Cosmos DB-konto, kan du sätta igång i Visual Studio Code!
