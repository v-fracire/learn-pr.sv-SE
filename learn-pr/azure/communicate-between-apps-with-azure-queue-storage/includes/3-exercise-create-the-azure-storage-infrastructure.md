Du har upptäckt att trafiktoppar kan överbelasta mellannivån. För att hantera detta har du bestämt dig för att lägga till en kö mellan klientdelen och mellannivån i ditt program för artikeluppladdning.

Det första steget när du skapar en kö är att skapa ett Azure Storage-konto som ska lagra våra data.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-storage-account-with-the-azure-cli"></a>Skapa ett lagringskonto med Azure CLI

> [!TIP] 
> Du skulle normalt sett starta ett nytt projekt genom att skapa en _resursgrupp_ för alla associerade resurser. I det här fallet vi ska använda Azure sandbox-miljön som tillhandahåller en resursgrupp med namnet <rgn>[Sandbox resursgruppens namn]</rgn>.

1. Välj Bash i Cloud Shell till höger om du tillfrågas.

1. Använd den `az storage account create` kommando för att skapa lagringskontot. Du behöver ange flera parametrar:

| Parameter | Värde |
|-----------|-------|
| `--name`  | Ställer in namnet. Kom ihåg att lagringskonton använder namnet till att generera en offentlig URL, så det måste vara unikt. Namnet på lagringskontot måste dessutom vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. Vi rekommenderar att du använder prefixet **articles** med ett slumptal som suffix, men du kan använda vad du vill. |
| `-g`        | Lagret den **resursgrupp**, använda <rgn>[Sandbox resursgruppens namn]</rgn> som värde. |
| `--kind`    | Anger den **typ av Lagringskonto** -Använd _StorageV2_ att skapa ett allmänt V2-konto. |
| `-l`        | Anger den **plats** oberoende av ägare för resursgruppen. Det här är valfritt, men du kan använda det om du vill placera i kön i en annan region än resursgruppen. |
| `--sku`     | Anger den **replikering och lagringstyp**, standard _Standard_RAGRS_. Vi använder _Standard_LRS_, vilket innebär att kontot bara är lokalt redundant i datacentret. |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Här är ett kommandoradsexempel med ovanstående parametrar. Se till att ändra den `--name` och `--location` parametrar om du vill kopiera och klistra in det här kommandot.

```azurecli
az storage account create --name [unique-name] -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 -l [location-name] --sku Standard_LRS
```