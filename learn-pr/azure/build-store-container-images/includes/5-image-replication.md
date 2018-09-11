Ett bra tips är att placera ett containerregister i varje region där avbildningar körs, så att åtgärder utförs närmare nätverket och du får snabba och tillförlitliga överföringar på avbildningsnivå. Med georeplikering kan ett Azure-containerregister fungera som ett enda register för flera regioner, med regionala register som har flera huvudregister.

Ett georeplikerat register ger följande fördelar:

* Namn på register/avbildningar/taggar kan användas i flera regioner
* Nätverksnära registeråtkomst från regionala distributioner
* Inga ytterligare avgifter för utgående trafik eftersom avbildningarna hämtas från ett lokalt, replikerat register i samma region som containervärden
* Enkel hantering av ett register i flera regioner

## <a name="replicate-image-to-multiple-locations"></a>Replikera avbildningar till flera platser

Använd kommandot `az acr replication create` till att replikera dina containeravbildningar till en annan region. I det här exemplet skapas en replikering för regionen `japaneast`. Ersätt `<acrName>` med namnet på ditt containerregister.

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

Utdata bör se ut ungefär så här:

```console
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myACR0007/replications/japaneast",
  "location": "japaneast",
  "name": "japaneast",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": {
    "displayStatus": "Syncing",
    "message": null,
    "timestamp": "2018-08-15T20:22:09.275792+00:00"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries/replications"
}
```

Om du vill se en lista med alla containeravbildningsrepliker använder du kommandot `az acr replication list`. Ersätt `<acrName>` med namnet på ditt containerregister.

```azurecli
az acr replication list --registry <acrName> --output table
```

Utdata bör se ut ungefär så här:

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

Även om du inte använde Azure-portalen i den här övningen är det bra att se hur det ser ut där.

Om du väljer `Replications` för ett Azure-containerregister i Azure-portalen ser du en karta över aktuella replikeringar. Du kan replikera containeravbildningar till ytterligare regioner genom att välja regionerna på kartan.

![Karta över containerreplikering i Azure-portalen](../media/replication-map.png)

## <a name="clean-up"></a>Rensa

Det här är den sista utbildningsenheten i modulen Azure Container Registry. Nu kan du rensa alla resurser du har skapat genom att ta bort resursgruppen. Det gör du med kommandot `az group delete`.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a>Sammanfattning

I den här modulen har du replikerat en containeravbildning till flera Azure-regioner med Azure CLI.