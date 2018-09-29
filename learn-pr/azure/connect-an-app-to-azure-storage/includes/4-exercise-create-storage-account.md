Nu när vi har en app behöver vi ett Azure-lagringskonto att arbeta med.

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto med hjälp av Azure CLI

Vi använder kommandot `az storage account create` för att skapa det nya lagringskontot. Det finns flera parametrar för att styra konfigurationen av lagringskontot.

> [!div class="mx-tableFixed"]
> | Alternativ | Beskrivning |
> |--------|-------------|
> | `--name` | Ett **namn på lagringskontot**. Namnet används för att generera den offentliga webbadress som används för att få åtkomst till data på kontot. Det måste vara unikt för alla befintliga lagringskontonamn i Azure. Det måste vara 3 till 24 tecken långt och får bara innehålla gemena bokstäver och siffror. |
> | `--resource-group` | Använd **<rgn>[sandbox-resursgruppnamn]</rgn>** för att placera lagringskontot i den kostnadsfria sandbox-miljön. |
> | `--location` | Välj en plats nära dig (se nedan). |
> | `--kind` | Det bestämmer lagringskontots _typ_. Alternativen inkluderar `BlobStorage`, `Storage` och `StorageV2`. |
> | `--sku` | Detta avgör lagringskontots prestanda och replikeringsmodell. Alternativ inkluderar `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS` och `Standard_ZRS`. |
> | `--access-tier` | **Åtkomstnivån** används endast för bloblagring och de tillgängliga alternativen är [`Cool` \| `Hot`]. **Frekvent åtkomstnivå** passar perfekt för data som används ofta, och **Lågfrekvent åtkomstnivå** passar bättre för data som inte används ofta. Lägg märke till att detta endast ställer in _standardvärdet_&mdash; – när du skapar en blob kan du ställa in ett annat datavärde. |
    
Använd tabellen ovan för att skapa en kommandorad i Cloud Shell till höger för att skapa kontot.
- Använd ett unikt namn. Som ett unikt namn rekommenderar vi något i stil med ”fotolagring” med dina initialer och ett slumpmässigt tal. Du får ett felmeddelande om det inte är unikt.
- Vanligtvis skulle du skapa en ny resursgrupp för dina appresurser, men i det här fallet, använd sandbox-resursgruppen "**<rgn>[sandbox-resursgruppnamn]</rgn>**".
- Använd ”Standard_LRS” för **sku**. Då används standardlagringen med lokal replikering vilket fungerar för det här exemplet.
- Använd ”Lågfrekvent” som **Åtkomstnivå**.

### <a name="selecting-a-location"></a>Välja en plats
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>Exempelkommando

```azurecli
az storage account create \
        --name <name> \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \
        --access-tier Cool
```

> [!TIP]
> Om du är intresserad av att utforska alternativen för lagringskontot ska du se till att läsa **Skapa ett Azure Storage-konto** där vi går igenom dem detaljerat.

Det tar några minuter att distribuera kontot. Medan Azure arbetar med det ska vi ta en titt på API:erna vi ska använda med kontot.
