> [!NOTE]
> <span data-ttu-id="a2ae4-101">Om du fortsätter från **Skapa en Azure Cosmos DB-databas som kan skalas** och inte tog bort Cosmos DB-databasen och samlingen som du skapade kan du hoppa över den här kursdelen och fortsätta med att lägga till data med Datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-101">If you are continuing on from **Create an Azure Cosmos DB database built to scale** and you did not delete the Cosmos DB database + collection that you created, then you can skip this unit and move on to adding data with the Data Explorer.</span></span>

<span data-ttu-id="a2ae4-102">Det första vi måste göra är att skapa en tom Cosmos DB-databas och en samling som vi kan arbeta med.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-102">The first thing we need to do is create an empty Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="a2ae4-103">Vi vill att den motsvarar den du skapade i den föregående modulen i den här utbildningsvägen: en databas med namnet **”Produkter”** och en samling med namnet **”Kläder”**.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-103">We want it to match the one you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="a2ae4-104">Använd följande instruktioner och Azure Cloud Shell på höger sida av skärmen om du vill återskapa databasen.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-104">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="a2ae4-105">Skapa ett Cosmos DB-konto och en Cosmos DB-databas med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a2ae4-105">Create a Cosmos DB account + database with the Azure CLI</span></span>

1. <span data-ttu-id="a2ae4-106">Börja med att välja rätt prenumeration – välj det prenumerations-ID som är kopplat till din kostnadsfria prenumeration för åtkomst till utbildning.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-106">Start by selecting the correct subscription - you want to select the subscription ID associated with your free education access subscription.</span></span>

    ```azurecli
    az account list --output table
    ```

1. <span data-ttu-id="a2ae4-107">Kontrollera att sandbox-miljö eller begränsat läge visas i prenumerationslistan, och ange att det är den som ska användas nu: <!-- TODO: get official name here --></span><span class="sxs-lookup"><span data-stu-id="a2ae4-107">Make sure you see "sandbox" in the subscription list and set it as the current one to use: <!-- TODO: get official name here --></span></span>

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. <span data-ttu-id="a2ae4-108">Hämta den resursgrupp som har skapats för dig.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-108">Get the Resource Group that has been created for you.</span></span> <span data-ttu-id="a2ae4-109">Om du använder en egen prenumeration hoppar du över det här steget och ger i stället ett unikt namn som du vill använda i miljövariabeln `RESOURCE_GROUP` nedan.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-109">If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below.</span></span> <span data-ttu-id="a2ae4-110">Anteckna namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-110">Take note of the Resource Group name.</span></span> <span data-ttu-id="a2ae4-111">Det är här vi ska skapa vår databas.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-111">This is where we will create our database.</span></span> <!-- Do we get a token for this? -->

    ```azurecli
    az group list --out table
    ```

1. <span data-ttu-id="a2ae4-112">Gör det lite enklare genom att ange några miljövariabler så att du inte behöver skriva in värden som är vanliga varje gång.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-112">To make this a bit easier, set a few environment variables so you don't have to type the common values each time.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="a2ae4-113">Se till att ändra de här värdena till lämpliga värden för sessionen.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-113">Make sure to change these values to ones appropriate for your session.</span></span> <span data-ttu-id="a2ae4-114">Ersätt till exempel värdet `<resource group>` med namnet på den resursgrupp som du identifierade ovan.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-114">For example, replace the `<resource group>` value with the Resource Group name you identified above.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<resource group>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```
    
1. <span data-ttu-id="a2ae4-115">Ange sedan en variabel för databasens namn.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-115">Next, set a variable for the database name.</span></span> <span data-ttu-id="a2ae4-116">Kalla den ”Användare” så att den stämmer överens med databasen som vi skapade i föregående modul.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-116">Name it "Users" so it matches the database we created in the last module.</span></span>

    ```azurecli
    export DB_NAME="Products"
    ```
    
1. <span data-ttu-id="a2ae4-117">Om du gör det här med din egen prenumeration och använder en _ny_ resursgrupp (rekommenderas) skapar du resursgruppen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-117">If you are doing this on your own subscription, and you are using a _new_ Resource Group (recommended), then use the following command to create the Resource Group.</span></span> <span data-ttu-id="a2ae4-118">**Viktigt:** Om du använder de kostnadsfria utbildningsresurserna som tillhandahålls via Microsoft Learn behöver du inte utföra det här steget.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-118">**Important:** If you are using the free education resources provided by Microsoft Learn, then you do not need to execute this step.</span></span> <span data-ttu-id="a2ae4-119">Kontrollera i stället att variabeln `RESOURCE_GROUP` ovan har angetts för din tilldelade resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-119">Instead, make sure the `RESOURCE_GROUP` variable above is set to your assigned resource group.</span></span>

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. <span data-ttu-id="a2ae4-120">Skapa sedan Cosmos DB-kontot.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-120">Next, create the Cosmos DB account.</span></span> <span data-ttu-id="a2ae4-121">Det här kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-121">This will take a few minutes to complete.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```
    
1. <span data-ttu-id="a2ae4-122">Skapa databasen `Products` i kontot.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-122">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```
    
1. <span data-ttu-id="a2ae4-123">Avslutningsvis skapar du samlingen `Clothing`.</span><span class="sxs-lookup"><span data-stu-id="a2ae4-123">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="a2ae4-124">Nu när vi har vårt Cosmos DB-konto, vår databas och vår samling är det dags att lägga till lite data!</span><span class="sxs-lookup"><span data-stu-id="a2ae4-124">Now that we have our Cosmos DB account, database, and collection, let's go add some data!</span></span>