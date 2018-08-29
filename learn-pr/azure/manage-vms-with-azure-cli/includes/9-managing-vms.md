En av de viktigaste åtgärderna för att köra virtuella datorer är att kunna starta och stoppa dem.

## <a name="stopping-a-vm"></a>Stoppa en virtuell dator

Vi kan stoppa en virtuell dator som körs med kommandot `vm stop`. Du måste skicka namn och resursgrupp eller unikt ID för den virtuella datorn:

```azurecli
az vm stop -n SampleVM -g ExerciseResources
```

Vi kan kontrollera att den har stoppats genom att försöka pinga den offentliga IP-adressen med `ssh`, eller med kommandot `vm get-instance-view`. Den sista metoden returnerar samma grundläggande data som `vm show`, men innehåller även information om själva instansen. Försök med att skriva följande kommando i Azure Cloud Shell för att se aktuell körningsstatus för den virtuella datorn:

```azurecli
az vm get-instance-view -n SampleVM -g ExerciseResources --query "instanceView.statuses[?starts_with(code, 'PowerState/') == `true`].displayStatus" -o tsv
```

Det här kommandot ska returnera `VM stopped` som ett resultat.

## <a name="starting-a-vm"></a>Starta en virtuell dator

Vi kan göra det omvända med kommandot `vm start`.

```azurecli
az vm start -n SampleVM -g ExerciseResources
```

Det här kommandot startar en stoppad virtuell dator. Vi kan kontrollera det med frågan `vm get-instance-view`, som nu borde returnera `VM running`.

## <a name="restarting-a-vm"></a>Starta om en virtuell dator

Slutligen om vi har gjort ändringar som kräver en omstart, kan vi starta om en virtuell dator med hjälp av kommandot `vm restart`. Du kan lägga till `--no-wait`-flaggan om du vill att Azure CLI ska returneras omedelbart, utan att vänta tills den virtuella datorn har startats om.

