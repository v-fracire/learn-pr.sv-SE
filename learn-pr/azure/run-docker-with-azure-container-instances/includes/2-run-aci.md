Azure Container Instances gör det enkelt att skapa och hantera Docker-containers i Azure, utan att behöva etablera virtuella datorer eller gå upp till en högre tjänstnivå. I den här utbildningsenheten skapar du en container i Azure och gör den tillgänglig på internet via ett fullständigt kvalificerat domännamn (FQDN).

## <a name="create-a-container"></a>Skapa en container

[!include[](../../../includes/azure-sandbox-activate.md)]

Du kan skapa en container genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp i kommandot **az container create**. Du kan också göra containern tillgänglig på Internet genom att ange en DNS-namnetikett. I det här exemplet distribuerar du en container som är värd för en liten webbapp.

Kör följande kommando i Cloud Shell för att starta en behållarinstans. Värdet *--dns-name-label* måste vara unikt i den Azure-region där du skapar instansen, så du kan behöva ändra värdet för att det ska vara unikt:

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

Om några sekunder bör du få ett svar på din begäran. Först har containern statusen **Creating** (skapas) men den bör starta inom några sekunder. Du kan kontrollera statusen med kommandot `az container show`:

```azurecli
az container show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

När du kör kommandot visas containerns fullständiga domännamn (FQDN) och dess etableringsstatus:

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

När containern övergår till statusen **Lyckades** går du till dess FQDN i din webbläsare:

![Skärmbild av webbläsare med en app som körs i en Azure-containerinstans](../media-draft/aci-app-browser.png)

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du skapat en Azure-containerinstans som kör en webbserver och en app. Du använde också appen via FQDN-värdet för containerinstansen.

I nästa utbildningsenhet konfigurerar du containerinstansens omstartsprincip.