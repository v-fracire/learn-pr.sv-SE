Genom att ställa in miljövariabler i dina containerinstanser kan du skapa en dynamisk konfiguration av programmet eller skriptet som körs av containern. Miljövariablerna anges med hjälp av Azure CLI, PowerShell eller Azure-portalen när containern skapas. Säkra miljövariabler används till att förhindra att känslig information visas i utdata från containeråtgärderna.

I den här utbildningsenheten skapar du en Cosmos DB-instans och sedan en Azure Container Instance-körning med anslutningsinformationen för Azure Cosmos DB-instansen som är lagrad som miljövariabler. En app i containern använder variablerna till att skriva och läsa data från Cosmos DB. I den här utbildningsenheten skapar du både en miljövariabel och en säker miljövariabel.

## <a name="deploy-azure-cosmos-db"></a>Distribuera Azure Cosmos DB

Skapa en Azure Cosmos DB-instans med kommandot `az Azure Cosmos DB create`. I det här exemplet placeras dessutom adressen till Azure Cosmos DB-slutpunkten i en variabel med namnet *COSMOS_DB_ENDPOINT*.

Kommandot tar några minuter att slutföra:

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query documentEndpoint -o tsv)
```

Hämta sedan anslutningsnyckeln för Azure Cosmos DB med kommandot `az cosmosdb list-keys` och lagra den i en variabel med namnet *COSMOS_DB_MASTERKEY*:

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a>Distribuera en containerinstans

Skapa en Azure-containerinstans med kommandot `az container create`. Observera att två miljövariabler skapas, `COSMOS_DB_ENDPOINT` och `COSMOS_DB_ENDPOINT`. De här variablerna innehåller de värden som behövs för att ansluta till Azure Cosmos DB-instansen:

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

När du har skapat containern kan du hämta IP-adressen med kommandot `az container show`:

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query ipAddress.ip --output tsv
```

Öppna en webbläsare och navigera till containerns IP-adress. Du bör se nedanstående app. När du röstar lagras rösten i Azure Cosmos DB-instansen.

![Azure-röstningsprogram med två alternativ, katter eller hundar.](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a>Säkra miljövariabler

I föregående övning skapade vi en container med anslutningsinformation för Azure Cosmos DB lagrad i två miljövariabler. Som standard visas miljövariabler i klartext i Azure Portal och i kommandoradsverktyg.

Om du till exempel hämtar information om containern som skapades i föregående övning med kommandot `az container show` visas miljövariablerna i klartext:

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query containers[0].environmentVariables
```

Exempel på utdata:

```json
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": "https://aci-cosmos.documents.azure.com:443/"
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": "Xm5BwdLlCllBvrR26V00000000S2uOusuglhzwkE7dOPMBQ3oA30n3rKd8PKA13700000000095ynys863Ghgw=="
  }
]
```

Med säkra miljövariabler förhindrar du att utdata visas i klartext. Om du vill använda säkra miljövariabler ersätter du argumentet `--environment-variables` med argumentet `--secure-environment-variables`.

Kör följande exempel för att skapa en container med namnet *aci-demo-secure* som använder säkra miljövariabler:

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

När containern nu returneras med kommandot `az container show` visas inte miljövariablerna:

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo-secure --query containers[0].environmentVariables
```

Exempel på utdata:

```json
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": null
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": null
  }
]
```

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten skapade du en Azure Cosmos DB-instans och en Azure-containerinstans. Du ställde in miljövariabler i containerinstansen så att appen som kördes i containern kunde ansluta till Azure Cosmos DB-instansen.

I nästa utbildningsenhet monterar du datavolymer vid en Azure-containerinstans för bevarande av data.
