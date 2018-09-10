<span data-ttu-id="cdfea-101">I den här övningen utvärderar du en befintlig databas med hjälp av Data Migration Assistant och granskar funktioner som används i den lokala SQL Server-instansen som för närvarande inte stöds i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="cdfea-101">In this exercise, you'll assess an existing database using Data Migration Assistant and review any features used in the local SQL Server instance that aren't currently supported in by Azure SQL Database.</span></span>

## <a name="setup"></a><span data-ttu-id="cdfea-102">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="cdfea-102">Setup</span></span>

1. <span data-ttu-id="cdfea-103">[Installera **Data Migration Assistant**](https://www.microsoft.com/en-us/download/details.aspx?id=53595) om du inte redan har gjort det.</span><span class="sxs-lookup"><span data-stu-id="cdfea-103">[Install the **Data Migration Assistant**](https://www.microsoft.com/en-us/download/details.aspx?id=53595) if you haven't done so already.</span></span>

1. <span data-ttu-id="cdfea-104">Du behöver en SQL Server-instans som körs. Kontrollera att du har anslutningsinformationen tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="cdfea-104">You'll need a SQL Server instance running, ensure you have connection details available.</span></span>
1. <span data-ttu-id="cdfea-105">[\*\*\* ersätt sannolikt med en virtuell LOD-dator \*\*\*] <!-- TODO: --></span><span class="sxs-lookup"><span data-stu-id="cdfea-105">[\*\*\*\* likely replace with an LOD VM \*\*\*\*\*] <!-- TODO: --></span></span>

1. <span data-ttu-id="cdfea-106">Öppna en webbläsare och navigera till https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span><span class="sxs-lookup"><span data-stu-id="cdfea-106">Start an Internet browser and navigate to https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

1. <span data-ttu-id="cdfea-107">I **OLTP downloads** (OLTP-nedladdningar) klickar du på **AdventureWorks2008R2.bak** och sparar den till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="cdfea-107">In **OLTP downloads**, click **AdventureWorks2008R2.bak** and save it to your local machine.</span></span>

1. <span data-ttu-id="cdfea-108">I Management Studio återställer du *AdventureWorks 2008R2* till din standardinstans.</span><span class="sxs-lookup"><span data-stu-id="cdfea-108">In Management Studio, restore *AdventureWorks 2008R2* to your default instance.</span></span>

## <a name="create-an-assessment"></a><span data-ttu-id="cdfea-109">Skapa en utvärdering</span><span class="sxs-lookup"><span data-stu-id="cdfea-109">Create an assessment</span></span>

1. <span data-ttu-id="cdfea-110">Starta **Microsoft Data Migration Assistant**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-110">Start the **Microsoft Data Migration Assistant**.</span></span>

1. <span data-ttu-id="cdfea-111">I appens vänstra navigeringsfält klickar du på __+__ för att skapa ett nytt DMA-projekt.</span><span class="sxs-lookup"><span data-stu-id="cdfea-111">In the app's left-hand navigation, click __+__ to create a new DMA project.</span></span>

1. <span data-ttu-id="cdfea-112">Ange följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="cdfea-112">Specify the following options:</span></span>
    - <span data-ttu-id="cdfea-113">**Projekttyp** – välj *Utvärdering*</span><span class="sxs-lookup"><span data-stu-id="cdfea-113">**Project type** - Select *Assessment*</span></span>
    - <span data-ttu-id="cdfea-114">**Projektnamn** – ange ett namn för ditt projekt, till exempel ”Bicycle DB Assessment”</span><span class="sxs-lookup"><span data-stu-id="cdfea-114">**Project name** - Enter a name for your project - for example "Bicycle DB Assessment"</span></span>
    - <span data-ttu-id="cdfea-115">**Typ av källserver** – välj *SQL Server*</span><span class="sxs-lookup"><span data-stu-id="cdfea-115">**Source server type** - Select *SQL Server*</span></span>
    - <span data-ttu-id="cdfea-116">**Typ av målserver** – välj *Azure SQL Database*</span><span class="sxs-lookup"><span data-stu-id="cdfea-116">**Target server type** - Select *Azure SQL Database*</span></span>

1. <span data-ttu-id="cdfea-117">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-117">Click **Create**.</span></span>
    <span data-ttu-id="cdfea-118">![Skärmbild som visar den konfiguration som beskrivs i Data Migration Assistant för dina AdventureWorks SQL Server-data.](../media-draft/3-create-assessment.png)</span><span class="sxs-lookup"><span data-stu-id="cdfea-118">![Screenshot showing the described configuration in the Data Migration Assistant for your AdventureWorks SQL Server data.](../media-draft/3-create-assessment.png)</span></span>

1. <span data-ttu-id="cdfea-119">Välj typ av utvärderingsrapport – markera båda:</span><span class="sxs-lookup"><span data-stu-id="cdfea-119">Select the assessment report type - check both:</span></span>
    - <span data-ttu-id="cdfea-120">Kontrollera databaskompatibilitet</span><span class="sxs-lookup"><span data-stu-id="cdfea-120">Check database compatibility</span></span>
    - <span data-ttu-id="cdfea-121">Kontrollera funktionsparitet</span><span class="sxs-lookup"><span data-stu-id="cdfea-121">Check feature parity</span></span>

1. <span data-ttu-id="cdfea-122">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-122">Click **Next**.</span></span>

## <a name="add-databases-to-assess"></a><span data-ttu-id="cdfea-123">Lägg till databaser att utvärdera</span><span class="sxs-lookup"><span data-stu-id="cdfea-123">Add databases to assess</span></span>

1. <span data-ttu-id="cdfea-124">Klicka på **Lägg till källor** för att öppna anslutningsmenyn.</span><span class="sxs-lookup"><span data-stu-id="cdfea-124">Click **Add Sources** to open the connection menu.</span></span>
2. <span data-ttu-id="cdfea-125">Ange följande information</span><span class="sxs-lookup"><span data-stu-id="cdfea-125">Enter the following information</span></span>
    - <span data-ttu-id="cdfea-126">Ange ditt befintliga SQL-serverinstansnamn</span><span class="sxs-lookup"><span data-stu-id="cdfea-126">Enter your existing SQL server instance name</span></span>
    - <span data-ttu-id="cdfea-127">Välj **Autentiseringstyp**</span><span class="sxs-lookup"><span data-stu-id="cdfea-127">Select the **Authentication** type</span></span>
    - <span data-ttu-id="cdfea-128">Ange anslutningsegenskaper för servern</span><span class="sxs-lookup"><span data-stu-id="cdfea-128">Specify the connection properties for your server</span></span>
3. <span data-ttu-id="cdfea-129">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-129">CLick **Connect**.</span></span>
4. <span data-ttu-id="cdfea-130">I **Lägg till källor** väljer du de databaser som ska utvärderas.</span><span class="sxs-lookup"><span data-stu-id="cdfea-130">In **Add sources**, select the databases to assess.</span></span> <span data-ttu-id="cdfea-131">För den här övningen väljer du **AdventureWorks2008R2**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-131">For this exercise, select **AdventureWorks2008R2**.</span></span>
5. <span data-ttu-id="cdfea-132">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-132">Click **Add**.</span></span>
    > [!NOTE]
    > <span data-ttu-id="cdfea-133">Om du vill lägga till databaser från flera SQL Server-instanser använder du knappen **Lägg till källor**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-133">To add databases from multiple SQL Server instances, use the **Add Sources** button.</span></span> <span data-ttu-id="cdfea-134">Du kan ta bort flera databaser genom att hålla ned tangenterna SKIFT + CTRL, markera de databaser som ska tas bort och sedan klicka på **Ta bort källor**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-134">To remove multiple databases, hold the SHIFT+CTRL keys and select the databases to remove then click **Remove Sources**.</span></span>

6. <span data-ttu-id="cdfea-135">Klicka på **Starta utvärderingen**.</span><span class="sxs-lookup"><span data-stu-id="cdfea-135">Click **Start Assessment**.</span></span>

## <a name="view-results"></a><span data-ttu-id="cdfea-136">Visa resultat</span><span class="sxs-lookup"><span data-stu-id="cdfea-136">View results</span></span>

<span data-ttu-id="cdfea-137">Om det finns flera databaser visas resultaten för varje databas när de blir tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="cdfea-137">If there are multiple databases, the results for each database will appear as soon as it is available.</span></span> <span data-ttu-id="cdfea-138">Du behöver inte vänta tills alla databasutvärderingar är klara.</span><span class="sxs-lookup"><span data-stu-id="cdfea-138">You don't need to wait for all database assessments to complete.</span></span>

1. <span data-ttu-id="cdfea-139">När utvärderingen för **AdventureWorks** är klar klickar du på **Kompatibilitetsproblem** och **Funktionsrekommendationer** för att visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="cdfea-139">Once the assessment for **AdventureWorks** is complete, click **Compatibility issues** and **Feature recommendations** to view the results.</span></span>
    - <span data-ttu-id="cdfea-140">Kategorin för SQL Server-funktionsparitet visar en lista över funktioner som kanske inte stöds fullständigt samt åtgärder för att lösa dessa problem.</span><span class="sxs-lookup"><span data-stu-id="cdfea-140">The SQL Server feature parity category lists features that might not be fully supported and steps to remedy these issues.</span></span> <span data-ttu-id="cdfea-141">Problem med funktionsparitet stoppar inte en migrering.</span><span class="sxs-lookup"><span data-stu-id="cdfea-141">Feature parity issues will not stop a migration.</span></span>
    - <span data-ttu-id="cdfea-142">Kategorin för kompatibilitetsproblem visar en lista över funktioner som skulle blockera en migrering samt åtgärder för att lösa dessa problem.</span><span class="sxs-lookup"><span data-stu-id="cdfea-142">The Compatibility issues category lists features that would block a migration and steps to remedy these issues.</span></span>

## <a name="summary"></a><span data-ttu-id="cdfea-143">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="cdfea-143">Summary</span></span>

<span data-ttu-id="cdfea-144">I den här övningen utvärderade en lokalt installerad SQL Server-databas för att kontrollera om några funktioner skulle bli tillgängliga när du migrerar databasen till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="cdfea-144">In this exercise, you assessed a locally installed SQL Server database to verify if any features would be unavailable when you migrate the database to Azure SQL Database.</span></span>