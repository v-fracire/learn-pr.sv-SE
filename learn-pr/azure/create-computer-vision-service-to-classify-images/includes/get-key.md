## <a name="get-an-access-key"></a><span data-ttu-id="61fff-101">Hämta en åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="61fff-101">Get an access key</span></span>

<span data-ttu-id="61fff-102">Om du inte redan har gjort det kör du följande kommando i Azure Cloud Shell för att lagra API-åtkomstnyckeln i en variabel som heter `key`.</span><span class="sxs-lookup"><span data-stu-id="61fff-102">If you haven't done so already, run the following command in Azure Cloud Shell to store the API access key in a variable called `key`.</span></span> <span data-ttu-id="61fff-103">Vi använder den här variabeln i efterföljande anrop.</span><span class="sxs-lookup"><span data-stu-id="61fff-103">We'll use this variable in subsequent calls.</span></span>

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--query key1 -o tsv)
```
