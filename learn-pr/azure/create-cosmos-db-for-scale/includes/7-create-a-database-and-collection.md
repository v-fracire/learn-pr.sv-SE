<span data-ttu-id="d323b-101">Nu när du förstår hur enheter för programbegäran används för att fastställa databasens dataflöde och hur partitionsnyckeln skapar utskalningsstrategin för din databas, är du redo att skapa din databas och samling.</span><span class="sxs-lookup"><span data-stu-id="d323b-101">Now that you understand how request units are used to determine database throughput and how the partition key creates the scale-out strategy for your database, you're ready to create your database and collection.</span></span>

## <a name="creating-your-database-and-collection"></a><span data-ttu-id="d323b-102">Skapa databas och samling</span><span class="sxs-lookup"><span data-stu-id="d323b-102">Creating your database and collection</span></span>

1. <span data-ttu-id="d323b-103">På Azure-portalen väljer du **Datautforskaren** från Cosmos DB-resursen och klickar sedan på **Ny samling** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="d323b-103">In the Azure Portal, select **Data Explorer** from your Cosmos DB resource and then click the **New Collection** button in the toolbar.</span></span>
    
    <span data-ttu-id="d323b-104">Området **Lägg till samling** visas längst till höger.</span><span class="sxs-lookup"><span data-stu-id="d323b-104">The **Add Collection** area is displayed on the far right.</span></span> <span data-ttu-id="d323b-105">Du kan behöva rulla till höger för att se den.</span><span class="sxs-lookup"><span data-stu-id="d323b-105">You may need to scroll right to see it.</span></span>

    ![Datautforskaren i Azure-portalen, bladet Lägg till samling](../media/5-create-a-database-and-collection/azure-cosmosdb-data-explorer.png)

2. <span data-ttu-id="d323b-107">På sidan **Lägg till samling** anger du inställningarna för den nya samlingen.</span><span class="sxs-lookup"><span data-stu-id="d323b-107">In the **Add collection** page, enter the settings for the new collection.</span></span>

    <span data-ttu-id="d323b-108">Inställning</span><span class="sxs-lookup"><span data-stu-id="d323b-108">Setting</span></span> | <span data-ttu-id="d323b-109">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="d323b-109">Suggested value</span></span> | <span data-ttu-id="d323b-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d323b-110">Description</span></span>
    --------|-----------------|-------------
    <span data-ttu-id="d323b-111">Databas-id</span><span class="sxs-lookup"><span data-stu-id="d323b-111">Database id</span></span>      | <span data-ttu-id="d323b-112">Användare</span><span class="sxs-lookup"><span data-stu-id="d323b-112">Users</span></span>         | <span data-ttu-id="d323b-113">Ange *Användare* som namn på den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="d323b-113">Enter *Users* as the name for the new database.</span></span> <span data-ttu-id="d323b-114">Databasnamn måste innehålla mellan 1 och 255 tecken och får inte innehålla /, \\, #, ? eller avslutande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="d323b-114">Database names must contain from 1 through 255 characters, and they cannot contain /, \\, #, ?, or a trailing space.</span></span>
    <span data-ttu-id="d323b-115">Samlings-id</span><span class="sxs-lookup"><span data-stu-id="d323b-115">Collection id</span></span>    | <span data-ttu-id="d323b-116">WebCustomers</span><span class="sxs-lookup"><span data-stu-id="d323b-116">WebCustomers</span></span>  | <span data-ttu-id="d323b-117">Ange *WebCustomers* som namn på din nya samling.</span><span class="sxs-lookup"><span data-stu-id="d323b-117">Enter *WebCustomers* as the name for your new collection.</span></span> <span data-ttu-id="d323b-118">Samma teckenkrav gäller för samlings-ID:n som databasnamn.</span><span class="sxs-lookup"><span data-stu-id="d323b-118">Collection ids have the same character requirements as database names.</span></span>
    <span data-ttu-id="d323b-119">Lagringskapacitet</span><span class="sxs-lookup"><span data-stu-id="d323b-119">Storage capacity</span></span> | <span data-ttu-id="d323b-120">Obegränsat</span><span class="sxs-lookup"><span data-stu-id="d323b-120">Unlimited</span></span>     | <span data-ttu-id="d323b-121">Använd standardvärdet **Obegränsat**.</span><span class="sxs-lookup"><span data-stu-id="d323b-121">Use the default value of **Unlimited**.</span></span> <span data-ttu-id="d323b-122">Det här värdet är databasens lagringskapacitet som gör det möjligt att skala upp din databas vid behov.</span><span class="sxs-lookup"><span data-stu-id="d323b-122">This value is the storage capacity of the database, and it enables your database to scale out as needed.</span></span>
    <span data-ttu-id="d323b-123">Partitionsnyckeln</span><span class="sxs-lookup"><span data-stu-id="d323b-123">Partition key</span></span>    | <span data-ttu-id="d323b-124">UserID</span><span class="sxs-lookup"><span data-stu-id="d323b-124">UserId</span></span>        | <span data-ttu-id="d323b-125">UserID är en bra partitionsnyckel i ett scenario med nätbutiker, eftersom så många frågor baseras på kund-ID:n.</span><span class="sxs-lookup"><span data-stu-id="d323b-125">UserID is a good partition key for an online retail scenario, as so many queries are based around the customer ID.</span></span>
    <span data-ttu-id="d323b-126">Dataflöde</span><span class="sxs-lookup"><span data-stu-id="d323b-126">Throughput</span></span>       |<span data-ttu-id="d323b-127">1 000 RU</span><span class="sxs-lookup"><span data-stu-id="d323b-127">1000 RU</span></span>        | <span data-ttu-id="d323b-128">Ändra genomflödet till 1 000 enheter för programbegäran per sekund (RU/s).</span><span class="sxs-lookup"><span data-stu-id="d323b-128">Change the throughput to 1000 request units per second (RU/s).</span></span> <span data-ttu-id="d323b-129">1 000 är det minsta RU/s-värde som du kan ange för att aktivera automatisk skalning.</span><span class="sxs-lookup"><span data-stu-id="d323b-129">1000 is the minimum RU/s value you can set to enable automatic scaling.</span></span>
    
    <span data-ttu-id="d323b-130">För tillfället markerar vi inte alternativet **Etablera databasens dataflöde** och vi lägger inte heller till några unika nycklar i samlingen.</span><span class="sxs-lookup"><span data-stu-id="d323b-130">For now, don't check the **Provision database throughput** option, and don't add any unique keys to the collection.</span></span> 
    
3. <span data-ttu-id="d323b-131">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d323b-131">Click **OK**.</span></span> <span data-ttu-id="d323b-132">Datautforskaren visar den nya databasen och samlingen.</span><span class="sxs-lookup"><span data-stu-id="d323b-132">The Data Explorer will display the new database and collection.</span></span>

    ![Datautforskaren i Azure-portalen visar den nya databasen och samlingen](../media/5-create-a-database-and-collection/azure-cosmos-db-new-collection.png)

## <a name="summary"></a><span data-ttu-id="d323b-134">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d323b-134">Summary</span></span>

<span data-ttu-id="d323b-135">I den här enheten använde du dina kunskaper om partitionsnycklar och enheter för programbegäran till att skapa en databas och en samling med ett dataflöde och en skalning som passar dina affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="d323b-135">In this unit, you used your knowledge of partition keys and request units to create a database and collection with throughput and scaling settings appropriate for your business needs.</span></span>