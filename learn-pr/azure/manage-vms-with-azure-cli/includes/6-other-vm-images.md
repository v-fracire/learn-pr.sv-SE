Vi använde `Debian` för avbildningen för att skapa den virtuella datorn. Azure har flera VM-standardavbildningar som du kan använda för att skapa en virtuell dator. 

Du kan hämta en lista över tillgängliga avbildningar med kommandot `az vm image list --output table`. Det genererar de populäraste avbildningarna som är en del av en offlinelista som är inbyggd i Azure CLI. Det finns dock _hundratals_ tillgängliga alternativ för avbildning på Azure Marketplace. 

> [!TIP]
> Du kan hämta en fullständig lista genom att lägga till `--all`-flaggan till kommandot. Eftersom listan med avbildningar på Marketplace är mycket stor är det praktiskt att filtrera listan med alternativen `--publisher` eller `–-offer`.

Vissa avbildningar är bara tillgängliga på vissa platser. Försök att lägga till flaggan `--location [location]` till kommandot för att begränsa resultatet till dem som är tillgängliga i regionen där du vill skapa den virtuella datorn. Skriv exempelvis följande i Azure Cloud Shell för att få en lista över avbildningarna som är tillgängliga i regionen `eastus`.

```azurecli
az vm image list --location eastus --output table
```

> [!NOTE]
> Det här är standardavbildningarna som Azure tillhandahåller. Tänk på att du även kan [skapa och ladda upp dina egna anpassade avbildningar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) för att skapa virtuella datorer baserat på unika konfigurationer eller mindre vanliga versioner eller distributioner av ett operativsystem.