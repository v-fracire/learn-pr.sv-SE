Du har upptäckt att trafiktoppar kan överbelasta mellannivån. För att hantera detta har du bestämt dig för att lägga till en kö mellan klientdelen och mellannivån i ditt program för artikeluppladdning.

Det första steget när du skapar en kö är att skapa ett Azure Storage-konto som ska lagra våra data.

## <a name="create-a-storage-account-with-the-azure-cli"></a>Skapa ett lagringskonto med Azure CLI

Vi börjar med att skapa en Azure-resursgrupp för lagringskontot.

1. Välj Bash i Cloud Shell till höger om du tillfrågas.

1. Använd Azure CLI-kommandot `az group create` till att skapa en ny resursgrupp. Ge den namnet **ExerciseResources** och lägg den på en plats nära dig. 
    - I exemplet nedan används ”eastus” som plats.

    ```azurecli
    az group create -n ExerciseResources --location eastus
    ```
        
1. Kör sedan kommandot `az storage account create`för att skapa det faktiska lagringskontot. Du behöver ange flera parametrar:

| Parameter | Värde |
|-----------|-------|
| `--name`  | Ställer in namnet. Kom ihåg att lagringskonton använder namnet till att generera en offentlig URL, så det måste vara unikt. Namnet på lagringskontot måste dessutom vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. Vi rekommenderar att du använder prefixet **articles** med ett slumptal som suffix, men du kan använda vad du vill. |
| `-g`        | Tillhandahåller resursgruppen, använd **ExerciseResources** som värde. |
| `--kind`    | Anger typen av lagringskonto – använd **StorageV2** för att skapa ett allmänt V2-konto. |
| `-l`        | Anger platsen oberoende av resursgruppens ägare. Det här är valfritt, men du kan använda det om du vill placera i kön i en annan region än resursgruppen. |
| `--sku`     | Anger kontotypen, standardvärdet är **Standard_RAGRS** vilket är mer än vad som behövs för den här demonstrationen. Vi använder **Standard_LRS**, vilket innebär att kontot bara är lokalt redundant i datacentret. |

Här är ett kommandoradsexempel med ovanstående parametrar. Se till att du ändrar parametern `--name` till något unikt om du kopierar och klistrar in det här kommandot.

```azurecli
az storage account create --name <name> -g ExerciseResources --kind StorageV2 -l eastus --sku Standard_LRS
```