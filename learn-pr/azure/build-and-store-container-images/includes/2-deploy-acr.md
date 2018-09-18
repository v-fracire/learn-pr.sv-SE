I den här utbildningsenheten skapar du ett Azure-containerregister med Azure CLI.

## <a name="create-an-azure-container-registry"></a>Skapa ett Azure-containerregister

Innan du skapar Azure-containerregistret måste du ha en *resursgrupp* att distribuera den till. En resursgrupp är en logisk samling där alla Azure-resurser distribueras och hanteras.

Skapa en resursgrupp med kommandot `az group create`. I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i regionen *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

När du har skapat resursgruppen skapar du ett Azure-containerregister med kommandot `az acr create`. Namnet på containerregistret måste vara unikt i Azure och innehålla mellan 5 och 50 alfanumeriska tecken. Ersätt `<acrName>` med ett unikt namn för ditt register.

I det här exemplet distribueras en premium-SKU för registret. Premium-SKU:n krävs för georeplikering. Mer information om SKU:er för Container Registry finns i [SKU:er i Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-skus)

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Premium
```

Här är exempel på utdata för ett nytt Azure-containerregister:

```console
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

## <a name="summary"></a>Sammanfattning

I den här enheten skapade du ett Azure-containerregister med Azure CLI.