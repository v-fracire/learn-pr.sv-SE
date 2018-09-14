Nu när vi har en app kan behöver vi ett Azure storage-konto för att arbeta med. Vi har skapat en med hjälp av Azure-portalen i den **skapa ett Azure storage-konto** modulen. Nu ska vi använda Azure CLI för den här gången.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Använda Azure CLI för att skapa ett Azure storage-konto

Vi använder den `az storage account create` kommando för att skapa ett nytt lagringskonto. Det tar flera parametrar som vi antingen måste du ange (eller bör) att konfigurera den som vi vill.

> [!div class="mx-tableFixed"]
> | Alternativ | Beskrivning |
> |--------|-------------|
> | `--name` | En **lagringskontonamn**. Namnet används för att generera en offentlig URL som används för att komma åt data på kontot. Det måste vara unikt inom alla befintliga lagringskontonamn i Azure. Det måste vara 3 och 24 tecken långt och får bara innehålla gemena bokstäver och siffror. |
> | `--resource-group` | Använd <rgn>[Sandbox resursgruppens namn]</rgn> att placera storage-konto i kostnadsfria sandbox-miljön. |
> | `--location` | Välj en plats nära dig (se nedan). |
> | `--kind` | Detta avgör lagringskontot _typ_. Alternativ inkluderar `BlobStorage`, `Storage`, och `StorageV2`. |
> | `--sku` | Detta avgör konto prestanda och replikering lagringsmodellen. Alternativ inkluderar `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, och `Standard_ZRS`. |
> | `--access-tier` | Den **åtkomstnivå** är endast används för Blob storage, tillgängliga alternativ är [`Cool` | `Hot`]. Den **frekventa åtkomstnivån** är perfekt för data som används ofta och **kalla åtkomstnivåer** är bättre för data som sällan används. Observera att detta endast anger den _standard_ värdet&mdash;när du skapar en Blob kan du ange ett annat värde för data. |
    
Använda tabellen ovan för att skapa en kommandorad i Cloud Shell till höger för att skapa kontot.
- Använd ett unikt namn. Vi rekommenderar att något som liknar ”photostore” med din initialer och ett slumptal. Du får ett fel om det inte är unikt.
- Normalt skulle du skapa en ny resursgrupp för att hålla din appresurser, men i det här fallet använder Sandbox-resursgrupp.
- Använd ”Standard_LRS” för den **sku**. Detta använder standard storage med lokal replikering, vilket är bra för det här exemplet.
- Använd ”kalla” för den **åtkomst till nivån**.

### <a name="selecting-a-location"></a>Att välja en plats
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>Exempelkommando

```azurecli
az storage account create \
        --name <name> \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \ 
        --access-tier Cool
```

> [!TIP]
> Om du är intresserad av att utforska alternativ för storage-konto kan du se till att gå igenom den **skapa ett Azure storage-konto** där vi gå igenom dem i detalj.

Det tar några minuter att distribuera kontot. Medan Azure arbetar med som, låt oss utforska API: er som vi ska använda med det här kontot.