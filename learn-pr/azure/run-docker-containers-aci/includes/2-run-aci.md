Med Azure Container Instances är det enkelt att skapa och hantera Docker-containrar i Azure, utan att du behöver etablera virtuella datorer eller använda en högnivåtjänst. I den här utbildningsenheten skapar du en container i Azure och gör den tillgänglig på internet via ett fullständigt kvalificerat domännamn (FQDN).

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure Container Instances måste precis som alla Azure-resurser placeras i en resursgrupp, som är en logisk samling där Azure-resurser distribueras och hanteras.

Skapa en resursgrupp med kommandot `az group create`.

I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i regionen *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="creat-a-container"></a>Skapa en container

Du kan skapa en container genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp i kommandot az container create. Du kan också göra containern tillgänglig på internet genom att ange en DNS-namnetikett. I det här exemplet distribuerar du en container som är värd för en liten webbapp.

Kör följande kommando för att starta en containerinstans. Värdet *--dns-name-label* måste vara unikt i den Azure-region där du skapar instansen, så du kan behöva ändra värdet för att det ska vara unikt.

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

Efter några sekunder bör du få ett svar på din begäran. Först har containern statusen **Creating** (Skapas), men den bör starta inom några sekunder. Du kan kontrollera statusen med kommandot `az container show`:

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

När du kör kommandot visas containerns fullständiga domännamn (FQDN) och dess etableringsstatus:

```bash
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

När containern övergår till statusen Succeeded (Lyckades) navigerar du till domännamnet i en webbläsare:

![Skärmbild av webbläsare med en app som körs i en Azure-containerinstans](../media-draft/aci-app-browser.png)

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du skapat en Azure-containerinstans som kör en webbserver och en app. Du använde också appen via FQDN-värdet för containerinstansen.

I nästa utbildningsenhet konfigurerar du containerinstansens omstartsprincip.
