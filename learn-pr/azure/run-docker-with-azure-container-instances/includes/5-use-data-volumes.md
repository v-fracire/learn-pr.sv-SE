Som standard är Azure-containerinstanser tillståndslösa. Om containern kraschar eller stoppas förloras hela tillståndet. Om du vill bevara tillståndet längre än containern måste du montera en volym från en extern lagring.

I den här utbildningsenheten monterar du en Azure-filresurs vid en Azure-containerinstans för lagring och hämtning av data.

## <a name="create-an-azure-file-share"></a>Skapa en Azure-filresurs

Innan du kan använda en Azure-filresurs med Azure Container Instances måste du skapa den. Kör följande skript för att skapa ett lagringskonto. Namnet på lagringskontot måste vara globalt unikt, så skriptet lägger till ett slumpmässigt värde i bassträngen:

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --location eastus --sku Standard_LRS
```

Kör följande kommando för att placera lagringskontots anslutningssträng i miljövariabeln *AZURE_STORAGE_CONNECTION_STRING*. Miljövariabeln kan tolkas i Azure CLI och kan användas i lagringsrelaterade åtgärder:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`
```

Skapa filresursen genom att köra kommandot `az storage share create`. I följande exempel skapas en resurs med namnet *aci-share-demo*:

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a>Hämta autentiseringsuppgifter för lagringen

När du ska montera en Azure-filresurs som en volym i Azure Container Instances behöver du tre värden: namnet på lagringskontot, resursnamnet och åtkomstnyckeln för lagringen.

Om du använde skriptet ovan skapades namnet på lagringskontonamnet med ett slumpmässigt värde i slutet. Du kan hämta den faktiska strängen (inklusive den slumpmässiga delen) med följande kommandon:

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Resursnamnet är redan känt (aci-share-demo), så allt som återstår är nyckeln för lagringskontot, som du kan hämta med följande kommando:

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Distribuera containern och montera volymen

När du ska montera en Azure-filresurs som en volym i en container anger du resursens och volymens monteringspunkt när du skapar containern:

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

När du har skapat containern hämtar du den offentliga IP-adressen:

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-files --query ipAddress.ip -o tsv
```

Öppna en webbläsare och navigera till containerns IP-adress. Du kommer att se ett enkelt formulär. Ange lite text och klicka på **Skicka**. Den här åtgärden skapar en fil i Azure-filresursen med den angivna texten som filinnehåll.

![Demo av filresurs i Azure Container Instances](../media-draft/files-ui.png)

Du kan kontrollera processen genom att öppna filresursen i Azure-portalen och ladda ned filen.

![Demo av exempelfil med innehåll](../media-draft/sample-text.png)

Om de filer och data som lagras i Azure-filresursen var värdefulla skulle resursen återmonteras vid en ny containerinstans för att tillhandahålla tillståndskänsliga data.


## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du skapat en Azure-filresurs och en container, och monterat filresursen vid containern. Resursen användes sedan till att lagra appdata.

I nästa utbildningsenhet går du igenom felsökning av några vanliga problem med containerinstanser.
