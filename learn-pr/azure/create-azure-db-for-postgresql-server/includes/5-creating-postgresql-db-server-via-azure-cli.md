Anta att du använder en lokal PostgreSQL-databas. Ditt företag planerar nu att utöka stödet för enheter, tillgänglighet, dataspårning och bearbetningsfunktioner genom att flytta servern till Azure. Du undersöker hur mycket arbete som krävs för att automatisera skapandet av en Azure Database for PostgreSQL-server.

Det är enkelt att skapa en Azure Database for PostgreSQL-server med Azure Portal. Det kan vara tidskrävande att skapa flera databaser och köra ständigt underhåll med endast portalen. Du använder Azure CLI till att skapa skript när du vill automatisera hanteringsuppgifter.

Det går att automatisera skapandet av nästan alla resurser i Microsoft Azure med hjälp av Azure CLI. I den här kursdelen lär du dig att automatisera hanteringen av dina Azure Database for PostgreSQL-servrar med hjälp av Azure CLI.

## <a name="what-is-the-azure-cli"></a>Vad är Azure CLI?

[Azure CLI](https://docs.microsoft.com/cli/azure/) är Microsofts plattformsoberoende kommandoradsmiljö för att hantera Azure-resurser. Du kan använda Azure CLI från webbläsaren med Azure Cloud Shell, eller så kan du installera Azure CLI lokalt på Mac OS X, Linux eller Windows. Azure CLI körs från en lokal kommandorad med hjälp av Bash eller PowerShell. Att köra Azure CLI lokalt kräver ytterligare konfiguration. Vi kommer att använda Azure Cloud Shell till att köra Azure CLI-kommandon.

## <a name="what-is-azure-cloud-shell"></a>Vad är Azure Cloud Shell?

Azure Cloud Shell är en webbläsarbaserad gränssnittsupplevelse som hanteras i molnet och som innebär att du kan ansluta till Azure med en autentiserad session. Du kan köra Azure CLI-kommandon för att automatisera hanteringen av en Azure Database for PostgreSQL-server. Vanliga Azure CLI-verktyg förinstalleras och konfigureras i Cloud Shell och kan användas med kontot.

> [!NOTE]
> Cloud Shell kräver en Azure-lagringsresurs för att spara eventuella filer som du skapar när du arbetar i Cloud Shell. När du startar Cloud Shell för första gången uppmanas du att skapa en resursgrupp, ett lagringskonto och en Azure Files-resurs. Detta är en engångshändelse och kopplas automatiskt till alla framtida Cloud Shell-sessioner.

## <a name="create-an-azure-database-for-postgresql-server-using-the-azure-cli"></a>Skapa en Azure Database for PostgreSQL-server med Azure CLI

Du använder Azure Cloud Shell-terminalen till höger för att skapa en Azure Database for PostgreSQL med hjälp av Azure CLI.

Hjälpen för användningen av Azure CLI-kommandon till att skapa servern som visar alla tillgängliga parametrar ser ut som följande exempel:

```azurecli
az postgres server create [-h] [--verbose] [--debug]
                            [--output {json,jsonc,table,tsv}]
                            [--query JMESPATH]
                            --resource-group RESOURCE_GROUP_NAME --name SERVER_NAME
                            --sku-name SKU_NAME [--location LOCATION]
                            --admin-user ADMINISTRATOR_LOGIN
                            [--admin-password ADMINISTRATOR_LOGIN_PASSWORD]
                            [--backup-retention BACKUP_RETENTION]
                            [--geo-redundant-backup GEO_REDUNDANT_BACKUP]
                            [--ssl-enforcement {Enabled,Disabled}]
                            [--storage-size STORAGE_MB]
                            [--tags [TAGS [TAGS ...]]]
                            [--version VERSION]
                            [--subscription _SUBSCRIPTION]

```

Följande valfria parametrar omges av hakparenteser. Låt oss nu undersöka några av de vanligaste.

### <a name="parameters"></a>Parametrar

Parametern `--resource-group <resource_group_name>` anger den resursgrupp där servern ska skapas.

`admin-user` och `admin-password` för servern som du anger krävs för att kunna logga in på servern och dess databaser. Kom ihåg eller anteckna den här informationen för senare bruk när du interagerar med den nya servern.

Parametern `--sku-name` används för att ange en del av prisnivån, i det här fallet beräkningsresursen. Värdet följer mönstret `{pricing tier}_{compute generation}_{vCores}`.

Exempel:

- `--sku-name B_Gen4_4` mappar till Basic, Gen 4 och 4 vCores.
- `--sku-name GP_Gen5_32` mappar till generell användning, Gen 5 och 32 vCores.
- `--sku-name MO_Gen5_2` mappar till minnesoptimerad, Gen 5 och 2 virtuella kärnor.

Som du kanske kommer ihåg diskuterade vi tre prisnivåer i kursdelen där vi skapade servern med hjälp av portalen.

Vi antar att du vill använda en beräkningsresurs med Basic, Gen 5 och 1 virtuell kärna. Du anger parametern som `--sku-name B_Gen5_1`.

Parametern `--storage-size` används för att ange en del av prisnivån. Om värdet inte anges kommer standardvärdet 5 120 MB att användas. Giltiga lagringsstorlekar börjar från 5 120 MB och ökar i steg om 1 024 MB upp till 1 048 576 MB.

Parametern `--backup-retention` används när du behöver ange kvarhållningsperioden för säkerhetskopior, vilken anges i dagar. Om värdet inte anges kommer standardvärdet sju dagar att användas.

Parametern `--version` används för att ange den huvudversion av PostgreSQL som du vill använda.

Nu har du sett stegen för att skapa en Azure Database for PostgreSQL-server med hjälp av Azure CLI. I nästa kursdel skapar du en Azure Database for PostgreSQL-server med hjälp av Azure CLI.