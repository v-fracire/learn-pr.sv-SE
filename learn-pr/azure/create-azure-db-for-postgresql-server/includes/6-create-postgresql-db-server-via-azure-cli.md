Du bestämmer dig för att skapa en Azure Database for PostgreSQL-server där du ska lagra vägarna som hämtas från löparnas träningsenheter. Baserat på tidigare hämtade datavolymer vet du att kraven på serverlagringen bör vara inställda på 20 GB. För att stödja dina bearbetningskrav måste du ha stöd för Gen 5 med 1 virtuell kärna. Du vet även att du behöver en kvarhållningsperiod på 15 dagar för säkerhetskopierade data.

## <a name="create-an-azure-postgresql-database-with-the-azure-cli"></a>Skapa en Azure PostGreSQL-databas med Azure CLI

Kom ihåg att du vill ställa in serverstorleken på 20 GB, beräkna Gen 5-stöd med 1 virtuell kärna och ha en kvarhållningsperiod på 15 dagar för säkerhetskopierade data.

1. Använd `az postgres server create`-metoden till att skapa en ny databas. Du anger flera parametrar:
    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`
    
2. Se om du kan skriva kommandot och slutföra parametrarna utan att titta på lösningen nedan. Här följer några tips.
    - Ersätt `<values>` med dina egna värden. 
    - Kom ihåg att servernamnet måste bestå av gemener ”a”–”z”, siffrorna 0–9 och bindestreck.
    - Använd <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn> som resursgrupp.
    - Använd en plats från nedanstående lista:   [!include[](../../../includes/azure-sandbox-regions-note.md)]
    
```bash
az postgres server create --name <unique_server_name> \
    --resource-group <rgn>[sandbox resource group name]</rgn> \ 
    --location eastus \
    --sku-name B_Gen5_1 \
    --storage-size 20480 \
    --backup-retention 15 \
    --version 10 \
    --admin-user <admin_user_name> \
    --admin-password <server_admin_password>
```

Det tar några minuter för systemet att bearbeta informationen vid körningen. Gå vidare och vänta tills kommandot har slutförts.

När det är klart returneras en JSON-sträng (Java Script Object Notation) som beskriver servern. Om ett fel uppstod visas ett felmeddelande. Du kan använda felinformationen till att granska och korrigera kommandoparametrarna. Försök sedan igen.

JSON-objektet ser ut ungefär så här:

```json
{
  "administratorLogin": "azureuser",
  "earliestRestoreDate": "2018-09-17T00:35:50.170000+00:00",
  "fullyQualifiedDomainName": "secondserver8.postgres.database.azure.com",
  "id": "/subscriptions/xxxxx/resourceGroups/<rgn>[sandbox Resource Group]</rgn>/providers/Microsoft.DBforPostgreSQL/servers/secondserver8",
  "location": "eastus",
  "name": "secondserver8",
  "resourceGroup": "<rgn>[sandbox Resource Group]</rgn>",
  "sku": {
    "capacity": 1,
    "family": "Gen5",
    "name": "B_Gen5_1",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 15,
    "geoRedundantBackup": "Disabled",
    "storageMb": 20480
  },
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "10"
}
```

Du har skapat en PostgreSQL-server med Azure CLI. I nästa del får du se hur du konfigurerar serverns säkerhetsinställningar.
