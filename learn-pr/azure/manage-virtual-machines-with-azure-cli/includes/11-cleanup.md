Du har skapat en ny virtuell Linux-dator, ändrat dess storlek, stoppat och startat den och uppdaterat konfigurationen med Azure CLI.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

När du arbetar i din egen prenumeration kan du köra följande Azure CLI-kommando för att ta bort resursgruppen och alla associerade resurser.

```azurecli
az group delete --name [resource-group-name] --no-wait
```

Svara ”yes” på frågan när den visas, eller använd parametern `--yes` för att svara automatiskt på prompten.

Det här kommandot tar bort alla resurser som är associerade med resursgruppen och garanterar att de frigörs i rätt ordning. Parametern `--no-wait` ser till att Azure CLI inte blockerar under tiden som borttagningen utförs. Lämna den inaktiverad för att vänta tills resurserna har tagits bort. Du kan också använda kommandot `az group wait -n [resource-group-name] --deleted` senare i skriptet för att vänta tills borttagningen är klar.


## <a name="further-reading"></a>Ytterligare läsning

- [Översikt över Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Azure CLI-kommandoreferens](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
