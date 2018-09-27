<span data-ttu-id="9bfb5-101">Du bestämmer dig för att skapa en Azure Database for PostgreSQL-server där du ska lagra vägarna som hämtas från löparnas träningsenheter.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-101">You decide to create an Azure Database for PostgreSQL server to store routes captured from runners' fitness devices.</span></span> <span data-ttu-id="9bfb5-102">Baserat på tidigare hämtade datavolymer vet du att kraven på serverlagringen bör vara inställda på 20 GB.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-102">Based on historic captured data volumes, you know your server storage requirements should be set at 20 GB.</span></span> <span data-ttu-id="9bfb5-103">För att stödja dina bearbetningskrav måste du ha stöd för Gen 5 med 1 virtuell kärna.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-103">To support your processing requirements, you need compute Gen 5 support with 1 vCore.</span></span> <span data-ttu-id="9bfb5-104">Du vet även att du behöver en kvarhållningsperiod på 15 dagar för säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-104">You also know that you require a retention period of 15 days for data backups.</span></span>

## <a name="create-an-azure-postgresql-database-with-the-azure-cli"></a><span data-ttu-id="9bfb5-105">Skapa en Azure PostGreSQL-databas med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9bfb5-105">Create an Azure PostGreSQL database with the Azure CLI</span></span>

<span data-ttu-id="9bfb5-106">Kom ihåg att du vill ställa in serverstorleken på 20 GB, beräkna Gen 5-stöd med 1 virtuell kärna och ha en kvarhållningsperiod på 15 dagar för säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-106">Keep in mind you want to set your server storage size at 20 GB, compute Gen 5 support with 1 vCore and a retention period of 15 days for data backups.</span></span>

1. <span data-ttu-id="9bfb5-107">Använd `az postgres server create`-metoden till att skapa en ny databas.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-107">Use the `az postgres server create` method to create a new database.</span></span> <span data-ttu-id="9bfb5-108">Du anger flera parametrar:</span><span class="sxs-lookup"><span data-stu-id="9bfb5-108">There are several parameters that you'll specify:</span></span>
    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`
    
2. <span data-ttu-id="9bfb5-109">Se om du kan skriva kommandot och slutföra parametrarna utan att titta på lösningen nedan.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-109">See if you can build the command and complete the parameters without looking at the solution below.</span></span> <span data-ttu-id="9bfb5-110">Här följer några tips.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-110">Here are some tips.</span></span>
    - <span data-ttu-id="9bfb5-111">Ersätt `<values>` med dina egna värden.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-111">Replace the `<values>` with your own values.</span></span> 
    - <span data-ttu-id="9bfb5-112">Kom ihåg att servernamnet måste bestå av gemener ”a”–”z”, siffrorna 0–9 och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-112">Remember that the server name must be  made up of lowercase letters 'a'-'z', the numbers 0-9 and the hyphen.</span></span>
    - <span data-ttu-id="9bfb5-113">Använd <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn> som resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-113">Use <rgn>[sandbox resource group name]</rgn> as the resource group.</span></span>
    - <span data-ttu-id="9bfb5-114">Använd en plats från nedanstående lista:   [!include[](../../../includes/azure-sandbox-regions-note.md)]</span><span class="sxs-lookup"><span data-stu-id="9bfb5-114">Use a location from the following list:   [!include[](../../../includes/azure-sandbox-regions-note.md)]</span></span>
    
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

<span data-ttu-id="9bfb5-115">Det tar några minuter för systemet att bearbeta informationen vid körningen.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-115">The system will take a few moments to process the information when executed.</span></span> <span data-ttu-id="9bfb5-116">Gå vidare och vänta tills kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-116">Go ahead and wait for the command to complete.</span></span>

<span data-ttu-id="9bfb5-117">När det är klart returneras en JSON-sträng (Java Script Object Notation) som beskriver servern.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-117">Once it's done, a JavaScript Object Notation (JSON) string that describes the server is returned.</span></span> <span data-ttu-id="9bfb5-118">Om ett fel uppstod visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-118">If there was a failure, an error message is displayed.</span></span> <span data-ttu-id="9bfb5-119">Du kan använda felinformationen till att granska och korrigera kommandoparametrarna. Försök sedan igen.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-119">You can use this error information to review and fix your command parameters and try again.</span></span>

<span data-ttu-id="9bfb5-120">JSON-objektet ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="9bfb5-120">The JSON object will look something like:</span></span>

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

<span data-ttu-id="9bfb5-121">Du har skapat en PostgreSQL-server med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-121">You've successfully created a PostgreSQL server using the Azure CLI.</span></span> <span data-ttu-id="9bfb5-122">I nästa del får du se hur du konfigurerar serverns säkerhetsinställningar.</span><span class="sxs-lookup"><span data-stu-id="9bfb5-122">In the next unit, you'll see how to configure your server's security settings.</span></span>
