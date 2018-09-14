Vårt mål är att skapa en ny virtuell Azure-dator. Vi behöver ange flera uppgifter för att identifiera resursplats, operativsystem som ska användas och maskinvarukonfiguration som behövs för den virtuella datorn. Vi börjar med **resursgruppen**.

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure använder _resursgrupper_ till att gruppera relaterade resurser, till exempel virtuella datorer och databaser. Resursgruppen identifierar även en specifik plats (s.k. ”region”) som avgör vilket datacenter resursen placeras i.

> [!NOTE]
> Sandbox-miljön för Azure ger en förskapad resursgrupp med namnet <rgn>[Sandbox resursgruppens namn]</rgn>. Du behöver inte köra de här stegen. Men om du skapar din _egna_ resurser för verkliga projekt, kommer dessa vara de kommandon som du behöver utföra. Sandlådan Azure tillåter inte att du kan skapa resursgrupper direkt.

Exempelvis kan du skriva följande Azure CLI-kommando i Azure Cloud Shell för att skapa en resursgrupp i den **USA, östra** region. Du kan ersätta **[resursgrupp]** med ett giltigt namn som är unikt i aktiv prenumeration.

```azurecli
az group create --name [resource-group] --location eastus
```

Detta kommer att returnera ett JSON block om resursgruppen har skapats.

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/<resourcegroup>",
  "location": "eastus",
  "managedBy": null,
  "name": "<resourcegroup>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Observera att det även returnerar den unika identifieraren för prenumerationen, platsen och namnet som en del av svaret. Du kan använda dem för att kontrollera att den skapades i rätt prenumeration och på rätt plats.

Nu när vi vet hur du skapar en resursgrupp kan vi skapa en ny virtuell dator.