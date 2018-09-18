Du bestämmer dig för att skapa en Azure Database for PostgreSQL för att lagra vägar som hämtats från löpares träningsenheter. Baserat på tidigare fångade datavolymer vet du att kraven på serverlagringen ska vara inställt på 20 GB. För att stödja dina bearbetningskrav måste du ha stöd för Gen 5 med 1 virtuell kärna. Du vet även att du behöver en kvarhållningsperiod på 15 dagar för säkerhetskopiering av data.

> [!TIP]
> Alla övningar i Microsoft Learn är kostnadsfria. När du börjar utforska på egen hand behöver du dock en Azure-prenumeration. Om du inte har någon än kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Det tar bara några minuter.

Nu sätter vi igång.

Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).

Kom ihåg att du måste starta en Azure Cloud Shell-session. Markera Cloud Shell-ikonen överst på skärmen för att starta sessionen.

![Cloud Shell-knapp](../media-draft/cloud-shell-button.png)

Om du inte redan har ett lagringskonto att använda med Cloud Shell måste du skapa ett med första åtkomst. Portalens gränssnitt vägleder dig genom processen för att skapa ett lagringskonto.

I den här labbuppgiften används `bash` som kommandoradsmiljö.

1. Välj den prenumeration du kommer att använda för att skapa servern.

    Om du har flera prenumerationer ska du se till att aktivera lämplig prenumeration med följande kommando och ersätta nollorna med ditt prenumerations-ID.

    ``` bash
    az account set --subscription "00000000-0000-0000-0000-000000000000"
    ```

    Kom ihåg att du kan lista alla dina prenumerationer med kommandot `az account list --output table`. Välj det prenumerations-ID du vill använda från listan.

1. Om du inte redan har skapat en resursgrupp i föregående del gör du det. Du kör sedan följande kommando.

    ```bash
    az group create --name <resource_group_name> --location <location>
    ```

    > [!Note]
    > Du kan hämta en lista över alla platser med hjälp av kommandot `az account list-locations`. Välj värdet `displayName` eller `name` för parametern `<location>`.

1. Nu är det dags att köra kommandot `az postgres server create`.

    Kom ihåg att du vill ställa in serverstorleken på 20 GB, stöd för beräkningsgeneration 5 med 1 virtuell kärna och en kvarhållningsperiod på 15 dagar för säkerhetskopiering av data.

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

    Se om du kan skriva kommandot och slutföra parametrarna utan att titta på lösningen nedan. Ersätt värdena i `<>` med dina egna.

    > [!NOTE]
    > Du kan hämta en lista över alla platser med hjälp av kommandot `az account list-locations`. Välj värdet `displayName` eller `name` för parametern `<location>`.

    ```bash
    az postgres server create --resource-group <resource_group_name> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
    ```

Du ser att det tar några minuter för systemet att bearbeta information vid körningen. En JSON-sträng (Java Script Object Notation) som beskriver servern returneras om servern skapades. Ett felmeddelande visas om servern inte har skapats. Du använder den här felinformationen för att granska och korrigera kommandoparametrarna.

Du har skapat en PostgreSQL-server med Azure CLI. I nästa del får du se hur du konfigurerar serverns säkerhetsinställningar.
