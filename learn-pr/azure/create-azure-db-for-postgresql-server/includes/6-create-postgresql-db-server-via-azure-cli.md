<span data-ttu-id="cbe9d-101">Du bestämmer dig för att skapa en Azure Database for PostgreSQL för att lagra vägar som hämtats från löpares träningsenheter.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-101">You decide to create an Azure Database for PostgreSQL to store routes captured from runners' fitness devices.</span></span> <span data-ttu-id="cbe9d-102">Baserat på tidigare fångade datavolymer vet du att kraven på serverlagringen ska vara inställt på 20 GB.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-102">Based on historic captured data volumes, you know your server storage requirements should be set at 20 GB.</span></span> <span data-ttu-id="cbe9d-103">För att stödja dina bearbetningskrav måste du ha stöd för Gen 5 med 1 virtuell kärna.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-103">To support your processing requirements, you need compute Gen 5 support with 1 vCore.</span></span> <span data-ttu-id="cbe9d-104">Du vet även att du behöver en kvarhållningsperiod på 15 dagar för säkerhetskopiering av data.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-104">You also know that you require a retention period of 15 days for data backups.</span></span>

> [!TIP]
> <span data-ttu-id="cbe9d-105">Alla övningar i Microsoft Learn är kostnadsfria. När du börjar utforska på egen hand behöver du dock en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-105">All of the exercises you do in Microsoft Learn are free, but once you start exploring on your own, you will need an Azure subscription.</span></span> <span data-ttu-id="cbe9d-106">Om du inte har någon än kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Det tar bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-106">If you don't have one yet, take a couple of minutes and create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="cbe9d-107">Nu sätter vi igång.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-107">Let's begin.</span></span>

<span data-ttu-id="cbe9d-108">Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="cbe9d-108">Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

<span data-ttu-id="cbe9d-109">Kom ihåg att du måste starta en Azure Cloud Shell-session.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-109">Recall that you'll need to start an Azure Cloud Shell session.</span></span> <span data-ttu-id="cbe9d-110">Markera Cloud Shell-ikonen överst på skärmen för att starta sessionen.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-110">Select the Cloud Shell icon at the top of the screen to start the session.</span></span>

![Cloud Shell-knapp](../media-draft/cloud-shell-button.png)

<span data-ttu-id="cbe9d-112">Om du inte redan har ett lagringskonto att använda med Cloud Shell måste du skapa ett med första åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-112">If you don't already have a storage account to use with Cloud Shell, you'll need to create one with first access.</span></span> <span data-ttu-id="cbe9d-113">Portalens gränssnitt vägleder dig genom processen för att skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-113">The portal interface will step you through the process of creating a storage account.</span></span>

<span data-ttu-id="cbe9d-114">I den här labbuppgiften används `bash` som kommandoradsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-114">This lab uses `bash` as the command-line environment.</span></span>

1. <span data-ttu-id="cbe9d-115">Välj den prenumeration du kommer att använda för att skapa servern.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-115">Select the subscription you'll use to create the server.</span></span>

    <span data-ttu-id="cbe9d-116">Om du har flera prenumerationer ska du se till att aktivera lämplig prenumeration med följande kommando och ersätta nollorna med ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-116">If you have several subscriptions, make sure you activate the appropriate subscription with the following command, replacing the zeros with your subscription identifier.</span></span>

    ``` bash
    az account set --subscription "00000000-0000-0000-0000-000000000000"
    ```

    <span data-ttu-id="cbe9d-117">Kom ihåg att du kan lista alla dina prenumerationer med kommandot `az account list --output table`.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-117">Recall, you can list all your subscriptions using the `az account list --output table` command.</span></span> <span data-ttu-id="cbe9d-118">Välj det prenumerations-ID du vill använda från listan.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-118">Pick the subscription identifier from this list that you'd like to use.</span></span>

1. <span data-ttu-id="cbe9d-119">Om du inte redan har skapat en resursgrupp i föregående del gör du det.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-119">If you haven't already done so in a previous unit, create a resource group.</span></span> <span data-ttu-id="cbe9d-120">Du kör sedan följande kommando.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-120">You'll run the following command.</span></span>

    ```bash
    az group create --name <resource_group_name> --location <location>
    ```

    > [!Note]
    > <span data-ttu-id="cbe9d-121">Du kan hämta en lista över alla platser med hjälp av kommandot `az account list-locations`.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-121">You can retrieve a list of all locations using this command `az account list-locations`.</span></span> <span data-ttu-id="cbe9d-122">Välj värdet `displayName` eller `name` för parametern `<location>`.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-122">Select the `displayName` or `name` value and use it for the `<location>` parameter.</span></span>

1. <span data-ttu-id="cbe9d-123">Nu är det dags att köra kommandot `az postgres server create`.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-123">You're now ready to run the `az postgres server create` command.</span></span>

    <span data-ttu-id="cbe9d-124">Kom ihåg att du vill ställa in serverstorleken på 20 GB, stöd för beräkningsgeneration 5 med 1 virtuell kärna och en kvarhållningsperiod på 15 dagar för säkerhetskopiering av data.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-124">Keep in mind you want to set your server storage size at 20 GB, compute Gen 5 support with 1 vCore and a retention period of 15 days for data backups.</span></span>

    <span data-ttu-id="cbe9d-125">Det finns flera parametrar som du anger:</span><span class="sxs-lookup"><span data-stu-id="cbe9d-125">There are several parameters that you'll specify:</span></span>

    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`

    <span data-ttu-id="cbe9d-126">Se om du kan skriva kommandot och slutföra parametrarna utan att titta på lösningen nedan.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-126">See if you can write the command and complete the parameters without looking at the solution below.</span></span> <span data-ttu-id="cbe9d-127">Ersätt värdena i `<>` med dina egna.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-127">Replace the values in `<>` with your own values.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cbe9d-128">Du kan hämta en lista över alla platser med hjälp av kommandot `az account list-locations`.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-128">You can retrieve a list of all locations using this command `az account list-locations`.</span></span> <span data-ttu-id="cbe9d-129">Välj värdet `displayName` eller `name` för parametern `<location>`.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-129">Select the `displayName` or `name` value and use it for the `<location>` parameter.</span></span>

    ```bash
    az postgres server create --resource-group <resource_group_name> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
    ```

<span data-ttu-id="cbe9d-130">Du ser att det tar några minuter för systemet att bearbeta information vid körningen.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-130">You'll see the system take a few moments to process the information when executed.</span></span> <span data-ttu-id="cbe9d-131">En JSON-sträng (Java Script Object Notation) som beskriver servern returneras om servern skapades.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-131">A Java Script Object Notation (JSON) string that describes the server is returned if the server was created.</span></span> <span data-ttu-id="cbe9d-132">Ett felmeddelande visas om servern inte har skapats.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-132">An error message is displayed if the server isn't created.</span></span> <span data-ttu-id="cbe9d-133">Du använder den här felinformationen för att granska och korrigera kommandoparametrarna.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-133">You'll use this error information to review and fix your command parameters.</span></span>

<span data-ttu-id="cbe9d-134">Du har skapat en PostgreSQL-server med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-134">You've successfully created a PostgreSQL server using the Azure CLI.</span></span> <span data-ttu-id="cbe9d-135">I nästa del får du se hur du konfigurerar serverns säkerhetsinställningar.</span><span class="sxs-lookup"><span data-stu-id="cbe9d-135">In the next unit, you'll see how to configure your server's security settings.</span></span>
