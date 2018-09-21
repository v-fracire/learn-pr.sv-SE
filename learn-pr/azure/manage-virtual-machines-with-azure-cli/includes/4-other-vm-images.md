Vi använde `Debian` för avbildningen för att skapa den virtuella datorn. Azure har flera VM-standardavbildningar som du kan använda för att skapa en virtuell dator. 

## <a name="listing-images"></a>Visa bilder i en lista

Du kan hämta en lista över tillgängliga VM-avbildningar med följande kommando. 

```azurecli
az vm image list --output table
```

Det genererar de populäraste avbildningarna som är en del av en offlinelista som är inbyggd i Azure CLI. Det finns dock _hundratals_ tillgängliga alternativ för avbildningar på Azure Marketplace. 

## <a name="getting-all-images"></a>Hämta alla avbildningar

Du kan hämta en fullständig lista genom att lägga till `--all`-flaggan i kommandot. Eftersom listan med avbildningar på Marketplace är mycket stor är det praktiskt att filtrera listan med alternativen `--publisher`, `--sku` eller `–-offer`.

Prova till exempel följande kommando för att se _alla_ tillgängliga Wordpress-avbildningar:

```azurecli
az vm image list --sku Wordpress --output table --all
```

Eller det här kommandot för att se alla avbildningar som tillhandahålls av Microsoft:

```azurecli
az vm image list --publisher Microsoft --output table --all
```

## <a name="location-specific-images"></a>Platsspecifika avbildningar

Vissa avbildningar är bara tillgängliga på vissa platser. Försök att lägga till flaggan `--location [location]` till kommandot för att begränsa resultatet till dem som är tillgängliga i regionen där du vill skapa den virtuella datorn. Skriv exempelvis följande i Azure Cloud Shell för att få en lista över de avbildningar som är tillgängliga i regionen `eastus`.

```azurecli
az vm image list --location eastus --output table
```

Prova att titta på några av avbildningarna på de andra tillgängliga Azure Sandbox-platserna:

[!include[](../../../includes/azure-sandbox-regions-note.md)]

> [!TIP]
> Det här är standardavbildningarna som Azure tillhandahåller. Tänk på att du även kan [skapa och ladda upp dina egna anpassade avbildningar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) för att skapa virtuella datorer baserat på unika konfigurationer eller mindre vanliga versioner eller distributioner av ett operativsystem.

Kommandofönstret är förmodligen fullt nu, så du kan skriva `clear` för att rensa skärmen om du vill.