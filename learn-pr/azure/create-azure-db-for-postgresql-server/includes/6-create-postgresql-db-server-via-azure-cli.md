Du bestämmer dig för att skapa en Azure Database for PostgreSQL-server för att lagra vägar som hämtats från deltagare lämplighet-enheter. Baserat på historisk avbildade datavolymer, vet du dina server-lagringsutrymmen ska vara inställd på 20 GB. För att stödja kraven för bearbetning, måste du beräkna Gen 5 support med 1 virtuell kärna. Du kan också veta att du behöver en kvarhållningsperiod på 15 dagar för säkerhetskopiering av data.

Vi börjar.

Kom ihåg att du vill ange storleken på din server lagring på 20 GB compute generation 5-stöd med 1 virtuell kärna och en kvarhållningsperiod på 15 dagar för säkerhetskopiering av data.

Det finns flera parametrar som du anger:

- `--resource-group <resource_group_name>`
- `--name <new_server_name>`
- `--location <location>`
- `--admin-user <admin_user_name>`
- `--admin-password <server_admin_password>`
- `--sku-name <sku>`
- `--storage-size <size>`
- `--backup-retention <days>`
- `--version <version_number>`

Se om du kan skriva kommandot och slutföra parametrarna utan att titta på lösningen nedan. Ersätt värdena i `<>` med dina egna värden.

> [!NOTE]
> Du kan hämta en lista över alla platser med hjälp av det här kommandot `az account list-locations`. Välj den `displayName` eller `name` värde och använda den för den `<location>` parametern.

```bash
az postgres server create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
```

Du ser i systemet som tar en stund åt att bearbeta information när den körs. En JavaScript Object Notation (JSON)-sträng som beskriver servern returneras om servern har skapats. Ett felmeddelande visas om servern inte har skapat. Du använder den här felinformationen för att granska och korrigera kommandoparametrarna.

Du skapat har en PostgreSQL-server med Azure CLI. I nästa enhet visas hur du konfigurerar säkerhetsinställningar för din server.
