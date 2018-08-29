Vårt mål är att skapa en ny virtuell Azure-dator. Vi behöver ange flera uppgifter för att identifiera resursplats, operativsystem som ska användas och maskinvarukonfiguration som behövs för den virtuella datorn. Vi börjar med **resursgruppen**.

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure använder _resursgrupper_ till att gruppera relaterade resurser, till exempel virtuella datorer och databaser. Resursgruppen identifierar även en specifik plats (s.k. ”region”) som avgör vilket datacenter resursen placeras i.

Eftersom vi experimenterar börjar vi med att skapa en ny resursgrupp med namnet `ExerciseResources` och vi placerar den i regionen `eastus`.

> [!NOTE]
> Du måste använda det här exakta namnet på din nya resursgrupp, eftersom Microsoft Learn-systemet letar efter det här resursnamnet senare. 

Skriv följande Azure CLI-kommando i Azure Cloud Shell för att skapa resursgruppen i din prenumeration.

```azurecli
az group create --name ExerciseResources --location eastus
```

Detta returnerar ett JSON-block som visar att resursgruppen har skapats. Det bör se ut ungefär så här:

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources",
  "location": "eastus",
  "managedBy": null,
  "name": "ExerciseResources",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Observera att det även returnerar den unika identifieraren för prenumerationen, platsen och namnet som en del av svaret. Du kan använda dem för att kontrollera att den skapades i rätt prenumeration och på rätt plats.

Nu när vi har en resursgrupp ska vi skapa en ny virtuell dator.