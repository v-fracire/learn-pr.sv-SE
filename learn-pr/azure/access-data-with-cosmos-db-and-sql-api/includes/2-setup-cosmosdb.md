<span data-ttu-id="5b817-101">Det första vi måste göra är att skapa en tom Azure Cosmos DB-databas och en samling som vi kan arbeta med.</span><span class="sxs-lookup"><span data-stu-id="5b817-101">The first thing we need to do is create an empty Azure Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="5b817-102">Vi vill att den motsvarar den du skapade i den föregående modulen i den här utbildningsvägen: en databas med namnet **”Produkter”** och en samling med namnet **”Kläder”**.</span><span class="sxs-lookup"><span data-stu-id="5b817-102">We want them to match the ones you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="5b817-103">Använd följande instruktioner och Azure Cloud Shell på höger sida av skärmen om du vill återskapa databasen.</span><span class="sxs-lookup"><span data-stu-id="5b817-103">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

## <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="5b817-104">Skapa ett Azure Cosmos DB-konto och en Cosmos DB-databas med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5b817-104">Create an Azure Cosmos DB account + database with the Azure CLI</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="select-a-subscription"></a><span data-ttu-id="5b817-105">Välja en prenumeration</span><span class="sxs-lookup"><span data-stu-id="5b817-105">Select a subscription</span></span>

<span data-ttu-id="5b817-106">Om du har använt Azure ett tag kan du ha flera tillgängliga prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="5b817-106">If you've been using Azure for a while, you might have multiple subscriptions available to you.</span></span> <span data-ttu-id="5b817-107">Så är det ofta för utvecklare, som kan ha en prenumeration för Visual Studio och en annan för företagets resurser.</span><span class="sxs-lookup"><span data-stu-id="5b817-107">This is often the case for developers who might have a subscription for Visual Studio, and another for corporate resources.</span></span>

<span data-ttu-id="5b817-108">Azure Sandbox har redan valt Concierge-prenumerationen för dig i Cloud Shell. Du kan bekräfta prenumerationsinställningen med hjälp av de här anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="5b817-108">The Azure Sandbox has already selected the Concierge Subscription for you in the Cloud Shell, and you can validate the subscription setting using these steps.</span></span> <span data-ttu-id="5b817-109">När du arbetar med din egen prenumeration kan du också byta prenumeration med Azure CLI med hjälp av följande anvisningar.</span><span class="sxs-lookup"><span data-stu-id="5b817-109">Or, when you are working with your own subscription, you can use the following steps to switch subscriptions with the Azure CLI.</span></span>

1. <span data-ttu-id="5b817-110">Börja med att visa en lista över tillgängliga prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="5b817-110">Start by listing the available subscriptions.</span></span>

    ```azurecli
    az account list --output table
    ```

   <span data-ttu-id="5b817-111">Om du arbetar med en Concierge-prenumeration bör det vara den enda prenumerationen som listas.</span><span class="sxs-lookup"><span data-stu-id="5b817-111">If you're working with a Concierge Subscription, it should be the only one listed.</span></span>

1. <span data-ttu-id="5b817-112">Om du sedan inte vill använda standardprenumerationen kan du ändra den med kommandot `account set`:</span><span class="sxs-lookup"><span data-stu-id="5b817-112">Next, if the default subscription isn't the one you want to use, you can change it with the `account set` command:</span></span>

    ```azurecli
    az account set --subscription "<subscription name>"
    ```
    
1. <span data-ttu-id="5b817-113">Hämta den resursgrupp som har skapats för dig i sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="5b817-113">Get the Resource Group that has been created for you by the sandbox.</span></span> <span data-ttu-id="5b817-114">Om du använder en egen prenumeration hoppar du över det här steget och anger istället ett unikt namn som du vill använda i miljövariabeln `RESOURCE_GROUP` nedan.</span><span class="sxs-lookup"><span data-stu-id="5b817-114">If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below.</span></span> <span data-ttu-id="5b817-115">Anteckna namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="5b817-115">Take note of the Resource Group name.</span></span> <span data-ttu-id="5b817-116">Det är här vi ska skapa vår databas.</span><span class="sxs-lookup"><span data-stu-id="5b817-116">This is where we will create our database.</span></span>

    ```azurecli
    az group list --out table
    ```
### <a name="setup-environment-variables"></a><span data-ttu-id="5b817-117">Konfigurera miljövariabler</span><span class="sxs-lookup"><span data-stu-id="5b817-117">Setup environment variables</span></span>

1. <span data-ttu-id="5b817-118">Ange några miljövariabler så att du inte behöver skriva in de vanliga värdena varje gång.</span><span class="sxs-lookup"><span data-stu-id="5b817-118">Set a few environment variables so you don't have to type the common values each time.</span></span> <span data-ttu-id="5b817-119">Starta genom att ange ett namn för Azure Cosmos DB-konto, till exempel `export NAME="mycosmosdbaccount"`.</span><span class="sxs-lookup"><span data-stu-id="5b817-119">Start by setting a name for the Azure Cosmos DB account, for example `export NAME="mycosmosdbaccount"`.</span></span> <span data-ttu-id="5b817-120">Fältet får bara innehålla gemener, siffror och bindestreck och måste vara mellan 3 och 31 tecken.</span><span class="sxs-lookup"><span data-stu-id="5b817-120">The field can contain only lowercase letters, numbers and the '-' character, and must be between 3 and 31 characters.</span></span>

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

1. <span data-ttu-id="5b817-121">Ange resursgrupp för att använda den befintliga resursgruppen för sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="5b817-121">Set the resource group to use the existing Sandbox resource group.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    ```

1. <span data-ttu-id="5b817-122">Välj regionen som är närmast dig och ställ in miljövariabeln, till exempel `export LOCATION="EastUS"`.</span><span class="sxs-lookup"><span data-stu-id="5b817-122">Select the region closest to you, and set the environment variable, such as `export LOCATION="EastUS"`.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

1. <span data-ttu-id="5b817-123">Ange en variabel för databasens namn.</span><span class="sxs-lookup"><span data-stu-id="5b817-123">Set a variable for the database name.</span></span> <span data-ttu-id="5b817-124">Kalla den ”Produkt” så att den stämmer överens med databasen som vi skapade i föregående modul.</span><span class="sxs-lookup"><span data-stu-id="5b817-124">Name it "Products" so it matches the database we created in the last module.</span></span>

    ```azurecli
    export DB_NAME="Products"
    ```

### <a name="create-a-resource-group-in-your-subscription"></a><span data-ttu-id="5b817-125">Skapa en resursgrupp i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="5b817-125">Create a resource group in your subscription</span></span>

<span data-ttu-id="5b817-126">När du skapar en Cosmos DB i din egen prenumeration bör du skapa en ny resursgrupp som rymmer alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="5b817-126">When you are creating a Cosmos DB on your own subscription you will want to create a new resource group to hold all the related resources.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b817-127">Om du använder Azure Sandbox som tillhandahålls via Microsoft Learn behöver du inte utföra det här steget.</span><span class="sxs-lookup"><span data-stu-id="5b817-127">If you are using the Azure Sandbox provided by Microsoft Learn, then you do not need to execute this step.</span></span> <span data-ttu-id="5b817-128">Kontrollera i stället att variabeln `RESOURCE_GROUP` ovan har angetts för **<rgn>[Resursgruppnamn för sandbox-miljö</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="5b817-128">Instead, make sure the `RESOURCE_GROUP` variable above is set to **<rgn>[Sandbox Resource Group Name</rgn>**.</span></span>

<span data-ttu-id="5b817-129">I din egen prenumeration använder du följande kommando för att skapa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="5b817-129">In your own subscription you would use the following command to create the Resource Group.</span></span> 

```azurecli
az group create --name <name> --location <location>
```

### <a name="create-the-azure-cosmos-db-account"></a><span data-ttu-id="5b817-130">Skapa Azure Cosmos DB-kontot</span><span class="sxs-lookup"><span data-stu-id="5b817-130">Create the Azure Cosmos DB account</span></span>

1. <span data-ttu-id="5b817-131">Skapa Azure Cosmos DB-kontot med kommandot `cosmosdb create`.</span><span class="sxs-lookup"><span data-stu-id="5b817-131">Create the Azure Cosmos DB account with the `cosmosdb create` command.</span></span> <span data-ttu-id="5b817-132">Kommandot använder följande parametrar och kan köras utan ändringar om du anger miljövariablerna enligt dessa rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="5b817-132">The command uses the following parameters and can be run with no modifications if you set the environment variables as recommended.</span></span>
    - <span data-ttu-id="5b817-133">`--name`: Unikt namn på resursen.</span><span class="sxs-lookup"><span data-stu-id="5b817-133">`--name`: Unique name for the resource.</span></span>
    - <span data-ttu-id="5b817-134">`--kind`: Typ av databas, använd _GlobalDocumentDB_.</span><span class="sxs-lookup"><span data-stu-id="5b817-134">`--kind`: Kind of database, use _GlobalDocumentDB_.</span></span>
    - <span data-ttu-id="5b817-135">`--resource-group`: Resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="5b817-135">`--resource-group`: The resource group.</span></span> <span data-ttu-id="5b817-136">Använd **<rgn>[Resursgrupp för sandbox-miljö]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="5b817-136">Use **<rgn>[Sandbox Resource Group]</rgn>**.</span></span> <span data-ttu-id="5b817-137">Den bör tilldelas variabeln `RESOURCE_GROUP`.</span><span class="sxs-lookup"><span data-stu-id="5b817-137">It should be assigned to the `RESOURCE_GROUP` variable.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

    <span data-ttu-id="5b817-138">Det tar ett par minuter för kommandot att slutföras. Inställningarna för det nya kontot visas i Cloud Shell när det har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="5b817-138">The command takes a few minutes to complete and the cloud shell displays the settings for the new account once it's deployed.</span></span>

1. <span data-ttu-id="5b817-139">Skapa databasen `Products` i kontot.</span><span class="sxs-lookup"><span data-stu-id="5b817-139">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

1. <span data-ttu-id="5b817-140">Avslutningsvis skapar du samlingen `Clothing`.</span><span class="sxs-lookup"><span data-stu-id="5b817-140">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="5b817-141">Nu när vi har vårt Azure Cosmos DB-konto, vår databas och vår samling är det dags att lägga till lite data!</span><span class="sxs-lookup"><span data-stu-id="5b817-141">Now that you have your Azure Cosmos DB account, database, and collection, let's go add some data!</span></span>