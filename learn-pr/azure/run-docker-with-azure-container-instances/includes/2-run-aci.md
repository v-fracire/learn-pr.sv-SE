Här skapar du en container i Azure och gör den tillgänglig på Internet via ett fullständigt kvalificerat domännamn (FQDN).

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="why-use-azure-container-instances"></a>Varför är det bra att använda Azure Container Instances?

Azure Container Instances är användbart för scenarier som kan fungera i isolerade containrar, däribland enkla program, automatisering av uppgifter och byggjobb. Azure Container Instances har följande fördelar:

- **Snabb start**: starta containrar på bara några sekunder.
- **Fakturering per sekund**: kostnader debiteras bara när containern körs.
- **Säkerhet på hypervisornivå**: isolera programmet lika fullständigt som det skulle vara i en virtuell dator.
- **Anpassade storlekar**: ange exakta värden för processorkärnor och minne.
- **Beständig lagring**: montera Azure Files-resurser direkt till en container för att hämta och bevara tillstånd.
- **Linux och Windows**: schemalägg både Windows- och Linux-containrar med hjälp av samma API.

För scenarier där du behöver fullständig containerorkestrering, som tjänstidentifiering för flera containrar, autoskalning och koordinerade programuppgraderingar rekommenderar vi Azure Kubernetes Service (AKS).

## <a name="create-a-container"></a>Skapa en container

Du skapar en container genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp i kommandot **az container create**. Du kan också göra containern tillgänglig på Internet genom att ange en DNS-namnetikett. I det här exemplet distribuerar du en container som är värd för en liten webbapp. Du kan också välja plats för avbildningen – standardvärdet nedan är ”Östra USA”, men du kan ändra den till en plats nära dig i listan nedan.

<!-- TODO: fix region list so it's not hardcoded here --> Med den kostnadsfria sandbox-miljön kan du skapa resurser i en delmängd av Azures globala regioner. Välj en region från följande lista när du skapar resurser:

:::row:::
    :::column:::
        - Västra USA 2 - Centrala USA - Östra USA - Västeuropa - Sydostasien :::column-end:::
:::row-end:::

Kör följande kommando i Cloud Shell för att starta en containerinstans. Värdet `--dns-name-label` måste vara unikt i den Azure-region där du skapar instansen, så du kan behöva ändra `[dns-name]` till något unikt.

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label [dns-name] \
    --location eastus
```

Om några sekunder bör du få ett svar på din begäran. Först har containern statusen **Creating** (skapas) men den bör starta inom några sekunder. Du kan kontrollera statusen med kommandot `az container show`:

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

När du kör kommandot visas containerns fullständiga domännamn (FQDN) och dess etableringstillstånd:

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

När containern övergår till statusen **Succeeded** (Lyckades) navigerar du till domännamnet i en webbläsare för att verifiera att det lyckades.

Här skapade du en Azure-containerinstans för att köra en webbserver och ett program. Du använde även appen via FQDN-värdet för containerinstansen.