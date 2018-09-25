## <a name="get-an-access-key"></a>Hämta en åtkomstnyckel

Om du inte redan har gjort det kör du följande kommando i Azure Cloud Shell för att lagra API-åtkomstnyckeln i en variabel som heter `key`. Vi använder den här variabeln i efterföljande anrop.

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--query key1 -o tsv)
```
