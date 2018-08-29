Du har skapat en ny virtuell Linux-dator, ändrat dess storlek, stoppat och startat den och uppdaterat konfigurationen med Azure CLI.

## <a name="cleanup"></a>Rensa

Nu när vi är klara med den virtuella datorn och inte längre behöver den ska vi ta bort resurserna. Borttagningen är viktig för beräknings- och lagringsresurser som kan fortsätta att debiteras mot ditt konto. 

Du kan ta bort enskilda resurser med kommandot `delete`, men det enklaste sättet att ta bort allt är med `az group delete`. Kör följande kommando i Azure CLI:

```azurecli
az group delete --name ExerciseResources --no-wait
```

Svara ”yes” på frågan när den visas, eller använd parametern `--yes` för att svara automatiskt på prompten.

Det här kommandot tar bort alla resurser som är associerade med resursgruppen och garanterar att de frigörs i rätt ordning. Parametern `--no-wait` ser till att Azure CLI inte blockerar under tiden som borttagningen utförs. Lämna den inaktiverad för att vänta tills resurserna har tagits bort. Du kan också använda kommandot `az group wait -n ExerciseResources --deleted` senare i skriptet för att vänta tills borttagningen är klar.


## <a name="further-reading"></a>Ytterligare läsning

* [Översikt över Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
* [Azure CLI-kommandoreferens](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
