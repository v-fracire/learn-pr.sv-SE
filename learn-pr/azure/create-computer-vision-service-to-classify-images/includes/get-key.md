## <a name="get-an-access-key"></a>Hämta en åtkomstnyckel

Om du inte redan gjort det, kör du följande kommando i Azure Cloud Shell för att lagra API-åtkomstnyckel i en variabel med namnet `key`. Vi använder den här variabeln i efterföljande anrop.

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--query key1 -o tsv)
```
