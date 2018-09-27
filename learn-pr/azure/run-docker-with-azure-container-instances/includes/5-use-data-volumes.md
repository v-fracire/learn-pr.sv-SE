Som standard är Azure-containerinstanser tillståndslösa. Om containern kraschar eller stoppas förloras hela tillståndet. Om du vill bevara tillståndet längre än containerns livslängd måste du montera en volym från en extern lagring.

Här monterar du en Azure-filresurs till en Azure-containerinstans för lagring och hämtning av data.

## <a name="create-an-azure-file-share"></a>Skapa en Azure-filresurs

Kör följande skript för att skapa ett lagringskonto. Namnet på lagringskontot måste vara globalt unikt, så skriptet lägger till ett slumpmässigt värde i bassträngen:

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --sku Standard_LRS \
    --location eastus
```

Kör följande kommando för att placera lagringskontots anslutningssträng i miljövariabeln *AZURE_STORAGE_CONNECTION_STRING*. Miljövariabeln kan tolkas i Azure CLI och kan användas i lagringsrelaterade åtgärder:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv)
```

Skapa en filresurs i lagringskontot genom att köra kommandot `az storage share create`. I följande exempel skapas en resurs med namnet *aci-share-demo*:

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a>Hämta autentiseringsuppgifter för lagring

När du ska montera en Azure-filresurs som en volym i Azure Container Instances behöver du tre värden: namnet på lagringskontot, resursnamnet och åtkomstnyckeln för lagringskontot.

Om du använde skriptet ovan skapades namnet på lagringskontot med ett slumpmässigt värde i slutet. Du kan hämta den faktiska strängen (inklusive den slumpmässiga delen) med följande kommandon:

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group <rgn>[sandbox resource group name]</rgn> --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Resursnamnet är redan känt (aci-share-demo), så allt som återstår är nyckeln för lagringskontot, som du kan hämta med följande kommando:

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group <rgn>[sandbox resource group name]</rgn> --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Distribuera containern och montera volymen

När du ska montera en Azure-filresurs som en volym i en container anger du resursens och volymens monteringspunkt när du skapar containern:

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --location eastus \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

När du har skapat containern hämtar du den offentliga IP-adressen:

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --query ipAddress.ip \
    --output tsv
```

Öppna en webbläsare och navigera till containerns IP-adress. Du kommer att se ett enkelt formulär. Ange lite text och klicka på **Submit** (Skicka). Den här åtgärden skapar en fil i Azure Files-resursen som innehåller den angivna texten.

![Demo av filresurs i Azure Container Instances](../media/5-files-ui.png)

Du kan kontrollera processen genom att öppna filresursen i Azure-portalen och ladda ned filen.

1. Hämta filnamnet

    ```azurecli
    az storage file list -s aci-share-demo -o table
    ```

1. Ladda ned det, se till att ersätta `<filename>` nedan.

    ```azurecli
    az storage file download -s aci-share-demo -n <filename>
    ```
    
![Demo av exempelfil med innehåll](../media/5-sample-text.png)

Om de filer och data som lagrades i Azure Files-resursen var värdefulla skulle resursen återmonteras på en ny containerinstans för att tillhandahålla tillståndskänsliga data.