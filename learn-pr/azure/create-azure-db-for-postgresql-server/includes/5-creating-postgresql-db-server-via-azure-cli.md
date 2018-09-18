<span data-ttu-id="e4199-101">Anta att du använder en lokal PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e4199-101">Let's assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="e4199-102">Ditt företag planerar nu att utöka stödet för enheter, tillgänglighet, dataspårning och bearbetningsfunktioner genom att flytta servern till Azure.</span><span class="sxs-lookup"><span data-stu-id="e4199-102">Your company is now looking at expanding device support, availability, data tracking, and processing features by moving your server into Azure.</span></span> <span data-ttu-id="e4199-103">Du undersöker hur mycket arbete som krävs för att automatisera skapandet av en Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="e4199-103">You'll investigate how much effort it takes to automate the creation of an Azure Database for PostgreSQL.</span></span>

<span data-ttu-id="e4199-104">Det är enkelt att skapa en Azure Database for PostgreSQL-server med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e4199-104">Creating a single Azure Database for PostgreSQL server using the Azure portal is easy.</span></span> <span data-ttu-id="e4199-105">Det kan vara trögt att skapa flera databaser och köra pågående underhåll med endast portalen.</span><span class="sxs-lookup"><span data-stu-id="e4199-105">Creating more than one database and running ongoing maintenance using only the portal may become tedious.</span></span> <span data-ttu-id="e4199-106">Du använder Azure CLI för att skapa skript när du vill automatisera hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e4199-106">You'll use the Azure CLI to create scripts when you want to automate management tasks.</span></span>

<span data-ttu-id="e4199-107">Det går att automatisera skapandet av nästan alla resurser i Microsoft Azure med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e4199-107">Creating almost any resource within Microsoft Azure can be automated using the Azure CLI.</span></span> <span data-ttu-id="e4199-108">I den här enheten lär du dig att automatisera hanteringen av dina Azure Database for PostgreSQL-servrar med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e4199-108">In this unit, you'll learn how to automate management of your Azure Database for PostgreSQL servers using the Azure CLI.</span></span>

## <a name="what-is-azure-cli"></a><span data-ttu-id="e4199-109">Vad är Azure CLI?</span><span class="sxs-lookup"><span data-stu-id="e4199-109">What is Azure CLI?</span></span>

<span data-ttu-id="e4199-110">[Azure CLI](https://docs.microsoft.com/cli/azure/) är Microsofts plattformsoberoende kommandoradsmiljö för att hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e4199-110">[Azure CLI](https://docs.microsoft.com/cli/azure/) is Microsoft’s cross-platform command-line environment for managing Azure resources.</span></span> <span data-ttu-id="e4199-111">Du kan använda Azure CLI från webbläsaren med Azure Cloud Shell, eller så kan du installera Azure CLI lokalt på Mac OS X, Linux eller Windows.</span><span class="sxs-lookup"><span data-stu-id="e4199-111">You can use the Azure CLI from your browser with Azure Cloud Shell, or you can install Azure CLI locally on Mac OS X, Linux, or Windows.</span></span> <span data-ttu-id="e4199-112">Azure CLI körs från en lokal kommandorad med hjälp av Bash eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e4199-112">The Azure CLI is run from a local command line using bash or Powershell.</span></span> <span data-ttu-id="e4199-113">Att köra Azure CLI lokalt kräver dock ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e4199-113">Running Azure CLI locally however requires additional setup.</span></span> <span data-ttu-id="e4199-114">Vi använder Azure Cloud Shell för att köra Azure CLI-kommandon.</span><span class="sxs-lookup"><span data-stu-id="e4199-114">We'll use the Azure Cloud Shell for executing Azure CLI commands.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="e4199-115">Vad är Azure Cloud Shell?</span><span class="sxs-lookup"><span data-stu-id="e4199-115">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="e4199-116">Azure Cloud Shell är en webbläsarbaserad gränssnittsupplevelse som hanteras i molnet och gör att du kan ansluta till Azure med en autentiserad session.</span><span class="sxs-lookup"><span data-stu-id="e4199-116">Azure Cloud Shell is a browser-based shell experience that is hosted in the cloud and allows you to connect to Azure using an authenticated session.</span></span> <span data-ttu-id="e4199-117">Du kan köra Azure CLI-kommandon för att automatisera hanteringen av en Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="e4199-117">You can execute Azure CLI commands to automate the management of an Azure Database for PostgreSQL.</span></span> <span data-ttu-id="e4199-118">Vanliga Azure CLI-verktyg förinstalleras och konfigureras i Cloud Shell och kan användas med kontot.</span><span class="sxs-lookup"><span data-stu-id="e4199-118">Common Azure CLI tools are pre-installed and configured in Cloud Shell for you to use with your account.</span></span>

> [!NOTE]
> <span data-ttu-id="e4199-119">Cloud Shell kräver en Azure-lagringsresurs för att spara eventuella filer som du skapar när du arbetar i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="e4199-119">Cloud Shell requires an Azure storage resource to persist any files you create while working in the Cloud Shell.</span></span> <span data-ttu-id="e4199-120">När du startar Cloud Shell för första gången uppmanas du att skapa en resursgrupp, ett lagringskonto och en Azure Files-resurs för din räkning.</span><span class="sxs-lookup"><span data-stu-id="e4199-120">On first launch Cloud Shell prompts to create a resource group, storage account, and Azure Files share on your behalf.</span></span> <span data-ttu-id="e4199-121">Detta är ett engångssteg och kopplas automatiskt till alla framtida Cloud Shell-sessioner.</span><span class="sxs-lookup"><span data-stu-id="e4199-121">This is a one-time step and will be automatically attached for all future Cloud Shell sessions.</span></span>

## <a name="create-an-azure-database-for-postgresql-server-using-azure-cli"></a><span data-ttu-id="e4199-122">Skapa en Azure Database for PostgreSQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e4199-122">Create an Azure Database for PostgreSQL server using Azure CLI</span></span>

<span data-ttu-id="e4199-123">Du använder Azure Cloud Shell för att skapa en Azure Database for PostgreSQL med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e4199-123">You'll use Azure Cloud Shell to create an Azure Database for PostgreSQL server using Azure CLI.</span></span> <span data-ttu-id="e4199-124">Vi tittar på de steg som du behöver vidta.</span><span class="sxs-lookup"><span data-stu-id="e4199-124">Let's look at the steps you'll take.</span></span>

<span data-ttu-id="e4199-125">Först loggar du in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e4199-125">First, sign into the Azure portal.</span></span>

<span data-ttu-id="e4199-126">Öppna Open Shell från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e4199-126">Open the Cloud Shell from the Azure portal.</span></span> <span data-ttu-id="e4199-127">Öppna webbläsaren, gå till [Azure-portalen](https://portal.azure.com?azure-portal=true) och klicka på Open Cloud Shell-knappen:</span><span class="sxs-lookup"><span data-stu-id="e4199-127">Open your browser and go to [Azure portal](https://portal.azure.com?azure-portal=true) and click the Open Cloud Shell button:</span></span>

![Cloud Shell-knapp](../media-draft/cloud-shell-button.png)

<span data-ttu-id="e4199-129">I Cloud Shell kan du köra kommandon i antingen `bash` eller `PowerShell`.</span><span class="sxs-lookup"><span data-stu-id="e4199-129">Cloud Shell allows you to run your commands either in `bash` or `PowerShell`.</span></span> <span data-ttu-id="e4199-130">Vi använder kommandoradsalternativet `bash` för alla exempel.</span><span class="sxs-lookup"><span data-stu-id="e4199-130">We'll use the `bash` command-line option for all examples.</span></span>

<span data-ttu-id="e4199-131">Om du har flera prenumerationer ska du se till att aktivera lämplig prenumeration med följande kommando och ersätta nollorna med ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="e4199-131">If you have several subscriptions, make sure you activate the appropriate subscription with the following command, replacing the zeros with your subscription identifier.</span></span>

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

<span data-ttu-id="e4199-132">Du kör följande kommando för att lista alla dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="e4199-132">You'll run the following command to list all your subscriptions.</span></span>

   ```bash
   az account list --output table
   ```

<span data-ttu-id="e4199-133">Nästa steg är att skapa en resursgrupp för att hantera servern och det ställe där resursgruppen kommer att finnas.</span><span class="sxs-lookup"><span data-stu-id="e4199-133">The next step is to create a resource group to manage the server and where the resource group will be located.</span></span> <span data-ttu-id="e4199-134">Som du kanske minns använder du en resursgrupp för att hantera alla resurser som är relaterade till din server.</span><span class="sxs-lookup"><span data-stu-id="e4199-134">Recall, you'll use a resource group to manage all the resources related to your server.</span></span> <span data-ttu-id="e4199-135">Med platsalternativet kan du ange det ställe där servern skapas fysiskt.</span><span class="sxs-lookup"><span data-stu-id="e4199-135">The location option allows you to specify where the server is created physically.</span></span> <span data-ttu-id="e4199-136">Du kör du nästa kommando och ersätter `<resource_group_name>` respektive `<location>` med lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="e4199-136">You'll run the next command and replace the `<resource_group_name>` and `<location>` respectively with appropriate values.</span></span>

   ```bash
   az group create --name <resourcegroup> --location <location>
   ```

<span data-ttu-id="e4199-137">Det sista steget är att skapa Azure Database for PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="e4199-137">The last step is to create the Azure Database for PostgreSQL server.</span></span>

   <span data-ttu-id="e4199-138">Hjälpen för användningen av Azure CLI-kommandon för att skapa servern som visar alla tillgängliga parametrar ser ut som följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e4199-138">The Azure CLI server creation command usage help showing all available parameters looks like the following example:</span></span>

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

   <span data-ttu-id="e4199-139">Följande kommandorad visar den nödvändiga uppsättningen parametrar för att skapa en Azure Database for PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="e4199-139">The following command line shows the required set of parameters to create an Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="e4199-140">Observera att visa parameter är valfria och inte listas.</span><span class="sxs-lookup"><span data-stu-id="e4199-140">You'll notice some are optional parameters and aren't listed.</span></span>

   ```bash
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a><span data-ttu-id="e4199-141">Beskrivningar av parametrar</span><span class="sxs-lookup"><span data-stu-id="e4199-141">Parameter descriptions</span></span>

<span data-ttu-id="e4199-142">Parametern `--resource-group <resource_group_name>` anger den resursgrupp där server ska skapas.</span><span class="sxs-lookup"><span data-stu-id="e4199-142">The `--resource-group <resource_group_name>` parameter specifies the resource group within which to create the server.</span></span>

<span data-ttu-id="e4199-143">`admin-user` och `admin-password` för servern som du anger krävs för att kunna logga in på servern och dess databaser.</span><span class="sxs-lookup"><span data-stu-id="e4199-143">The server `admin-user` and `admin-password` that you specify is required to sign in to the server and its databases.</span></span> <span data-ttu-id="e4199-144">Anteckna den här informationen för senare bruk när du interagerar med den nya servern.</span><span class="sxs-lookup"><span data-stu-id="e4199-144">Remember or record this information for later when interacting with the new server.</span></span>

<span data-ttu-id="e4199-145">Parametern `--sku-name` används i för att ange en del av prisnivån, i det här fallet beräkningsresursen.</span><span class="sxs-lookup"><span data-stu-id="e4199-145">You use the `--sku-name` parameter is used to specify part of the pricing tier, in this case compute resource.</span></span> <span data-ttu-id="e4199-146">Värdet följer mönstret `{pricing tier}_{compute generation}_{vCores}`.</span><span class="sxs-lookup"><span data-stu-id="e4199-146">The value follows the convention `{pricing tier}_{compute generation}_{vCores}`.</span></span>

<span data-ttu-id="e4199-147">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e4199-147">Examples:</span></span>

- <span data-ttu-id="e4199-148">`--sku-name B_Gen4_4` mappar till Basic, Gen 4 och 4 vCores.</span><span class="sxs-lookup"><span data-stu-id="e4199-148">`--sku-name B_Gen4_4` maps to Basic, Gen 4, and 4 vCores.</span></span>
- <span data-ttu-id="e4199-149">`--sku-name GP_Gen5_32` mappar till generell användning, Gen 5 och 32 vCores.</span><span class="sxs-lookup"><span data-stu-id="e4199-149">`--sku-name GP_Gen5_32` maps to General Purpose, Gen 5, and 32 vCores.</span></span>
- <span data-ttu-id="e4199-150">`--sku-name MO_Gen5_2` mappar till minnesoptimerad, Gen 5 och 2 vCores.</span><span class="sxs-lookup"><span data-stu-id="e4199-150">`--sku-name MO_Gen5_2` maps to Memory Optimized, Gen 5, and 2 vCores.</span></span>

<span data-ttu-id="e4199-151">Som du kanske kommer ihåg diskuterade vi tre prisnivåer för den enhet där vi skapar servern med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="e4199-151">Recall, we discussed the three pricing tiers in the unit where we create the server using the portal.</span></span>

<span data-ttu-id="e4199-152">Vi antar att du vill använda en Basic-beräkningsresurs med Gen 5 och 1 vCore. I så fall anger du parametern som `--sku-name B_Gen5_1`.</span><span class="sxs-lookup"><span data-stu-id="e4199-152">Let's assume you want to use a Basic, Gen 5, and 1 vCore compute resource, you'll then specify the parameter as `--sku-name B_Gen5_1`.</span></span>

<span data-ttu-id="e4199-153">Parametern `--storage-size` används även för att ange en del av prisnivån.</span><span class="sxs-lookup"><span data-stu-id="e4199-153">You use the `--storage-size` parameter is also used the specify part of the pricing tier.</span></span> <span data-ttu-id="e4199-154">Om värdet inte anges så används standardvärdet 5 120 MB.</span><span class="sxs-lookup"><span data-stu-id="e4199-154">If the value isn't specified, then it defaults to 5,120 MB.</span></span> <span data-ttu-id="e4199-155">Giltiga lagringsstorlekar börjar från 5 120 MB och ökar i steg om 1 024 MB upp till 1 048 576 MB.</span><span class="sxs-lookup"><span data-stu-id="e4199-155">Valid storage sizes range from 5,120 MB and increases in additional increments of 1,024 MB up to 1,048,576 MB.</span></span>

<span data-ttu-id="e4199-156">Parametern `--backup-retention` används när du behöver ange kvarhållningsperioden för säkerhetskopior som anges i dagar.</span><span class="sxs-lookup"><span data-stu-id="e4199-156">The `--backup-retention` parameter is used when you need to specify the retention period for backups specified in days.</span></span> <span data-ttu-id="e4199-157">Om värdet inte anges så används standardvärdet sju dagar.</span><span class="sxs-lookup"><span data-stu-id="e4199-157">If the value isn't specified, then it defaults to seven days.</span></span>

<span data-ttu-id="e4199-158">Parametern `--version` används för att ange den högre version av PostgreSQL som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="e4199-158">You use the `--version` parameter is used to specify the major version of PostgreSQL you would like to use.</span></span>

<span data-ttu-id="e4199-159">Nu har du sett åtgärderna för att skapa en Azure Database for PostgreSQL med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e4199-159">You've now seen the steps to create an Azure Database for PostgreSQL using Azure CLI.</span></span> <span data-ttu-id="e4199-160">I nästa enhet skapar du en Azure Database for PostgreSQL-server med Azure hjälp av CLI.</span><span class="sxs-lookup"><span data-stu-id="e4199-160">In the next unit, you'll create an Azure Database for PostgreSQL server using Azure CLI.</span></span>