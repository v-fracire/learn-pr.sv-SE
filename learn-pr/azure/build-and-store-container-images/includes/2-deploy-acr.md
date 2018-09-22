I den här utbildningsenheten skapar du ett Azure-containerregister med Azure CLI.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]
 
## <a name="create-an-azure-container-registry"></a>Skapa ett Azure-containerregister

Vi kommer att arbeta i en kostnadsfri sandbox-miljö, så du behöver inte skapa din egen resursgrupp. Skapa ett Azure-containerregister med kommandot `az acr create`. Containerregistrets namn måste vara unikt i Azure och innehålla mellan 5 och 50 alfanumeriska tecken. Ersätt `<acrName>` med ett unikt namn för ditt register.

I det här exemplet distribueras en premiumregister-SKU. Premium-SKU:n krävs för georeplikering. Ange följande kommando i Cloud Shell-redigeringsprogrammet.

```azurecli
az acr create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <acrName> --sku Premium
```

Här följer exempel på utdata för ett nytt Azure-containerregister:

```output
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-15T19:19:07.042178+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myACR0007",
  "location": "eastus",
  "loginServer": "myacr.azurecr.io",
  "name": "myACR",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

I resten av modulen använder vi `<acrName>` som platshållare för det containerregisternamn du väljer i det här steget.

I den här enheten skapade du ett Azure-containerregister med Azure CLI.