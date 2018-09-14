Anta att du använder en lokal PostgreSQL-databas. Ditt företag nu tittar på utökar stödet för enheter, tillgänglighet, spårnings-data och funktioner för textbearbetning genom att flytta servern till Azure. Du kan undersöka hur mycket arbete som krävs för att automatisera skapandet av en Azure Database for PostgreSQL-server.

Det är enkelt att skapa en enda Azure Database for PostgreSQL-server med Azure-portalen. Skapa mer än en databas och köra pågående Underhåll med hjälp av endast portal bli tedious. Du använder Azure CLI för att skapa skript när du vill automatisera hanteringsuppgifter.

Skapa nästan alla resurser i Microsoft Azure kan automatiseras med hjälp av Azure CLI. I den här enheten lär du dig att automatisera hanteringen av din Azure Database for PostgreSQL-servrar med hjälp av Azure CLI.

## <a name="what-is-the-azure-cli"></a>Vad är Azure CLI?

Den [Azure CLI](https://docs.microsoft.com/cli/azure/) är Microsofts plattformsoberoende kommandoradsmiljö för att hantera Azure-resurser. Du kan använda Azure CLI från din webbläsare med Azure Cloud Shell eller du kan installera Azure CLI lokalt på Mac OS X, Linux eller Windows. Azure CLI körs från en lokal med bash eller PowerShell-kommandoraden. Kör Azure CLI lokalt kräver ytterligare inställningar. Vi använder Azure Cloud Shell för att köra Azure CLI-kommandon.

## <a name="what-is-azure-cloud-shell"></a>Vad är Azure Cloud Shell?

Azure Cloud Shell är en webbläsarbaserad skalupplevelse som är värd för molnet och gör att du kan ansluta till Azure med en autentiserad session. Du kan köra Azure CLI-kommandon för att automatisera hanteringen av en Azure Database for PostgreSQL-server. Vanliga Azure CLI-verktyg förinstallerat och konfigureras i Cloud Shell som du kan använda med ditt konto.

> [!NOTE]
> Cloudshell kräver en Azure storage-resurs för att spara alla filer som du skapar när du arbetar i Cloud Shell. Vid första start Cloud Shell uppmanas du för att skapa en resursgrupp, storage-konto och Azure Files dela å dina vägnar. Detta är ett enstaka steg och bifogas automatiskt för alla framtida Cloud Shell-sessioner.

## <a name="create-an-azure-database-for-postgresql-server-using-the-azure-cli"></a>Skapa en Azure Database for PostgreSQL-server med Azure CLI

Du använder Azure Cloud Shell-terminalen till höger för att skapa en Azure Database for PostgreSQL-server med Azure CLI.

Det ser ut som i följande exempel i Azure CLI-servern skapa användning kommandohjälp som visar alla tillgängliga parametrar:

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

Följande kommandorad visas den nödvändiga uppsättningen parametrar för att skapa en Azure Database for PostgreSQL-server. Du märker att vissa parametrar är valfria och inte är angivna.

   ```azurecli
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a>Beskrivningar av parametern

Den `--resource-group <resource_group_name>` parametern anger den resursgrupp som du skapade servern.

Servern `admin-user` och `admin-password` som du anger för att kunna logga in på servern och databaserna. Kom ihåg eller Skriv den här informationen för senare när du interagerar med den nya servern.

Du använder den `--sku-name` parametern beräkningsresurs du vill ange en del av prisnivån i det här fallet. Värdet följer reglerna från `{pricing tier}_{compute generation}_{vCores}`.

Exempel:

- `--sku-name B_Gen4_4` mappar till Basic, Gen 4 och 4 vCores.
- `--sku-name GP_Gen5_32` mappar till generell användning, Gen 5 och 32 vCores.
- `--sku-name MO_Gen5_2` mappar till minnesoptimerad, Gen 5 och 2 vCores.

Kom ihåg att vi diskuterade tre prisnivåer för enheten där vi skapade servern med hjälp av portalen.

Anta att du vill använda en grundläggande Gen 5 och 1 virtuell kärna beräkningsresurs. Du måste ange parametern som `--sku-name B_Gen5_1`.

Du använder den `--storage-size` parametern för att ange en del av prisnivå. Om värdet har inte angetts standard 5,120 MB. Giltigt lagringskontonamn storlekar sträcker sig från 5,120 MB och ökar i steg om 1 024 MB upp till 1 048 576 MB.

Den `--backup-retention` parametern används när du behöver att ange kvarhållningsperioden för säkerhetskopior, som anges i dagar. Om värdet inte anges standardvärdet sju dagar.

Du använder den `--version` parametern för att ange den huvudsakliga versionen av PostgreSQL som du vill använda.

Du har nu fått veta hur du skapar en Azure Database for PostgreSQL-server med Azure CLI. I nästa enhet, ska du skapa en Azure Database for PostgreSQL-server med Azure CLI.