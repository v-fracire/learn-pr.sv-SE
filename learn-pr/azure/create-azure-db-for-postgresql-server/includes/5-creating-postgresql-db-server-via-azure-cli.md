Anta att du använder en lokal PostgreSQL-databas. Ditt företag planerar nu att utöka stödet för enheter, tillgänglighet, dataspårning och bearbetningsfunktioner genom att flytta servern till Azure. Du undersöker hur mycket arbete som krävs för att automatisera skapandet av en Azure Database for PostgreSQL.

Det är enkelt att skapa en Azure Database for PostgreSQL-server med Azure-portalen. Det kan vara trögt att skapa flera databaser och köra pågående underhåll med endast portalen. Du använder Azure CLI för att skapa skript när du vill automatisera hanteringsuppgifter.

Det går att automatisera skapandet av nästan alla resurser i Microsoft Azure med hjälp av Azure CLI. I den här enheten lär du dig att automatisera hanteringen av dina Azure Database for PostgreSQL-servrar med hjälp av Azure CLI.

## <a name="what-is-azure-cli"></a>Vad är Azure CLI?

[Azure CLI](https://docs.microsoft.com/cli/azure/) är Microsofts plattformsoberoende kommandoradsmiljö för att hantera Azure-resurser. Du kan använda Azure CLI från webbläsaren med Azure Cloud Shell, eller så kan du installera Azure CLI lokalt på Mac OS X, Linux eller Windows. Azure CLI körs från en lokal kommandorad med hjälp av Bash eller PowerShell. Att köra Azure CLI lokalt kräver dock ytterligare konfiguration. Vi använder Azure Cloud Shell för att köra Azure CLI-kommandon.

## <a name="what-is-azure-cloud-shell"></a>Vad är Azure Cloud Shell?

Azure Cloud Shell är en webbläsarbaserad gränssnittsupplevelse som hanteras i molnet och gör att du kan ansluta till Azure med en autentiserad session. Du kan köra Azure CLI-kommandon för att automatisera hanteringen av en Azure Database for PostgreSQL. Vanliga Azure CLI-verktyg förinstalleras och konfigureras i Cloud Shell och kan användas med kontot.

> [!NOTE]
> Cloud Shell kräver en Azure-lagringsresurs för att spara eventuella filer som du skapar när du arbetar i Cloud Shell. När du startar Cloud Shell för första gången uppmanas du att skapa en resursgrupp, ett lagringskonto och en Azure Files-resurs för din räkning. Detta är ett engångssteg och kopplas automatiskt till alla framtida Cloud Shell-sessioner.

## <a name="create-an-azure-database-for-postgresql-server-using-azure-cli"></a>Skapa en Azure Database for PostgreSQL-server med Azure CLI

Du använder Azure Cloud Shell för att skapa en Azure Database for PostgreSQL med hjälp av Azure CLI. Vi tittar på de steg som du behöver vidta.

Först loggar du in på Azure-portalen.

Öppna Open Shell från Azure-portalen. Öppna webbläsaren, gå till [Azure-portalen](https://portal.azure.com?azure-portal=true) och klicka på Open Cloud Shell-knappen:

![Cloud Shell-knapp](../media-draft/cloud-shell-button.png)

I Cloud Shell kan du köra kommandon i antingen `bash` eller `PowerShell`. Vi använder kommandoradsalternativet `bash` för alla exempel.

Om du har flera prenumerationer ska du se till att aktivera lämplig prenumeration med följande kommando och ersätta nollorna med ditt prenumerations-ID.

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

Du kör följande kommando för att lista alla dina prenumerationer.

   ```bash
   az account list --output table
   ```

Nästa steg är att skapa en resursgrupp för att hantera servern och det ställe där resursgruppen kommer att finnas. Som du kanske minns använder du en resursgrupp för att hantera alla resurser som är relaterade till din server. Med platsalternativet kan du ange det ställe där servern skapas fysiskt. Du kör du nästa kommando och ersätter `<resource_group_name>` respektive `<location>` med lämpliga värden.

   ```bash
   az group create --name <resourcegroup> --location <location>
   ```

Det sista steget är att skapa Azure Database for PostgreSQL-servern.

   Hjälpen för användningen av Azure CLI-kommandon för att skapa servern som visar alla tillgängliga parametrar ser ut som följande exempel:

   ```bash
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

   Följande kommandorad visar den nödvändiga uppsättningen parametrar för att skapa en Azure Database for PostgreSQL-server. Observera att visa parameter är valfria och inte listas.

   ```bash
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a>Beskrivningar av parametrar

Parametern `--resource-group <resource_group_name>` anger den resursgrupp där server ska skapas.

`admin-user` och `admin-password` för servern som du anger krävs för att kunna logga in på servern och dess databaser. Anteckna den här informationen för senare bruk när du interagerar med den nya servern.

Parametern `--sku-name` används i för att ange en del av prisnivån, i det här fallet beräkningsresursen. Värdet följer mönstret `{pricing tier}_{compute generation}_{vCores}`.

Exempel:

- `--sku-name B_Gen4_4` mappar till Basic, Gen 4 och 4 vCores.
- `--sku-name GP_Gen5_32` mappar till generell användning, Gen 5 och 32 vCores.
- `--sku-name MO_Gen5_2` mappar till minnesoptimerad, Gen 5 och 2 vCores.

Som du kanske kommer ihåg diskuterade vi tre prisnivåer för den enhet där vi skapar servern med hjälp av portalen.

Vi antar att du vill använda en Basic-beräkningsresurs med Gen 5 och 1 vCore. I så fall anger du parametern som `--sku-name B_Gen5_1`.

Parametern `--storage-size` används även för att ange en del av prisnivån. Om värdet inte anges så används standardvärdet 5 120 MB. Giltiga lagringsstorlekar börjar från 5 120 MB och ökar i steg om 1 024 MB upp till 1 048 576 MB.

Parametern `--backup-retention` används när du behöver ange kvarhållningsperioden för säkerhetskopior som anges i dagar. Om värdet inte anges så används standardvärdet sju dagar.

Parametern `--version` används för att ange den högre version av PostgreSQL som du vill använda.

Nu har du sett åtgärderna för att skapa en Azure Database for PostgreSQL med hjälp av Azure CLI. I nästa enhet skapar du en Azure Database for PostgreSQL-server med Azure hjälp av CLI.