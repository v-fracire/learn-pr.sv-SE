<span data-ttu-id="f4d64-101">Nu när du har utvärderat din databas och åtgärdat eventuella rapporterade fel är du redo att migrera din databas.</span><span class="sxs-lookup"><span data-stu-id="f4d64-101">Now that you have assessed your database and fixed any errors reported, you're ready to migrate your database.</span></span> <span data-ttu-id="f4d64-102">I den här delen kommer du att migrera din SQL-databas till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f4d64-102">In this unit, you will migrate your SQL database to Azure SQL Database.</span></span>

## <a name="setup"></a><span data-ttu-id="f4d64-103">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="f4d64-103">Setup</span></span>

<span data-ttu-id="f4d64-104">Det första steget är att ladda ned exempeldata.</span><span class="sxs-lookup"><span data-stu-id="f4d64-104">The first step is to download the sample data.</span></span> <span data-ttu-id="f4d64-105">Du kan hoppa över den här konfigurationen om du redan har data installerade lokalt.</span><span class="sxs-lookup"><span data-stu-id="f4d64-105">You can skip this setup if you already have the data installed locally.</span></span>

1. <span data-ttu-id="f4d64-106">Öppna en webbläsare och navigera till https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span><span class="sxs-lookup"><span data-stu-id="f4d64-106">Start an internet browser and navigate to https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

1. <span data-ttu-id="f4d64-107">I **OLTP downloads** (OLTP-nedladdningar) klickar du på **AdventureWorks2008R2.bak**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-107">In **OLTP downloads**, click **AdventureWorks2008R2.bak**.</span></span>

1. <span data-ttu-id="f4d64-108">I Management Studio återställer du **AdventureWorks 2008R2** till din standardinstans.</span><span class="sxs-lookup"><span data-stu-id="f4d64-108">In Management Studio, restore **AdventureWorks 2008R2** to your default instance.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="f4d64-109">Skapa en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f4d64-109">Create an Azure SQL Database</span></span>

1. <span data-ttu-id="f4d64-110">Logga in på ditt Azure-portalkonto.</span><span class="sxs-lookup"><span data-stu-id="f4d64-110">Sign in to your Azure portal account.</span></span>

1. <span data-ttu-id="f4d64-111">Klicka på **Skapa en resurs** längst upp till vänster i portalen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-111">Click **Create a resource** in the upper left-hand corner of the portal.</span></span>

1. <span data-ttu-id="f4d64-112">Klicka på **Databaser** > **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-112">Click **Databases** > **SQL Database**.</span></span>

1. <span data-ttu-id="f4d64-113">Ange följande fält i SQL Database-formuläret:</span><span class="sxs-lookup"><span data-stu-id="f4d64-113">Enter the following fields on the SQL Database form:</span></span>

    |<span data-ttu-id="f4d64-114">Fält</span><span class="sxs-lookup"><span data-stu-id="f4d64-114">Field</span></span>|<span data-ttu-id="f4d64-115">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f4d64-115">Description</span></span>|
    |-----|---|
    |<span data-ttu-id="f4d64-116">Databasnamn</span><span class="sxs-lookup"><span data-stu-id="f4d64-116">Database name</span></span>|<span data-ttu-id="f4d64-117">Ange ett namn för din databas.</span><span class="sxs-lookup"><span data-stu-id="f4d64-117">Enter a name for your database.</span></span> <span data-ttu-id="f4d64-118">Det kan vara namnet på den befintliga databas som du migrerar eller valfritt nytt namn</span><span class="sxs-lookup"><span data-stu-id="f4d64-118">This can be the name of your existing database that you're migrating or any new name of your choice</span></span>|
    |<span data-ttu-id="f4d64-119">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f4d64-119">Resource group</span></span>|<span data-ttu-id="f4d64-120">Välj ett resursgruppsnamn eller ange ett nytt resursgruppsnamn</span><span class="sxs-lookup"><span data-stu-id="f4d64-120">Select a resource group or enter a new resource group name</span></span>|
    |<span data-ttu-id="f4d64-121">Välj källa</span><span class="sxs-lookup"><span data-stu-id="f4d64-121">Select source</span></span>|<span data-ttu-id="f4d64-122">Välj *Tom databas*</span><span class="sxs-lookup"><span data-stu-id="f4d64-122">Select *Blank database*</span></span>|

1. <span data-ttu-id="f4d64-123">Under **Server** väljer du antingen en befintlig server eller anger ett globalt unikt **servernamn** en **inloggning för serveradministratör** ett **lösenord** (som du ska bekräfta) samt en **plats**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-123">Under **Server**, either select an existing server or, enter a globally unique **Server name**, a **Server admin login**, a **Password** (which you should confirm), and a **Location**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f4d64-124">Du använder servernamnet, inloggningsnamnet och lösenordet för att ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-124">You will use the server name, login name, and password to connect to your database.</span></span>

1. <span data-ttu-id="f4d64-125">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-125">Click **Select**.</span></span>

1. <span data-ttu-id="f4d64-126">I **Prisnivå** kan du välja en databas med högre prestanda, men för den här självstudien väljer du standardinställningen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-126">In **Pricing tier**, you can select a database with higher performance, but for this tutorial, select default.</span></span>

1. <span data-ttu-id="f4d64-127">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-127">Click **Create**.</span></span> <span data-ttu-id="f4d64-128">Vänta tills databasen har etablerats innan du fortsätter med självstudien.</span><span class="sxs-lookup"><span data-stu-id="f4d64-128">Wait until the database has been provisioned before continuing the tutorial.</span></span>

    <span data-ttu-id="f4d64-129">Följande skärmbild visar en möjlig konfiguration för den nya SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-129">The following screenshot shows a possible configuration for your new SQL database.</span></span>

    ![Skärmbild av ett nytt SQL Database-blad i Azure-portalen med en exempelkonfiguration.](../media-draft/5-create-azure-sql-db.png)

## <a name="create-a-new-migration-project"></a><span data-ttu-id="f4d64-131">Skapa ett nytt migreringsprojekt</span><span class="sxs-lookup"><span data-stu-id="f4d64-131">Create a new migration project</span></span>

1. <span data-ttu-id="f4d64-132">Starta **Data Migration Assistant**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-132">Start **Data Migration Assistant**.</span></span>

1. <span data-ttu-id="f4d64-133">Klicka på ikonen **Nytt** och ange följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="f4d64-133">Click the **New** icon, and specify the following options:</span></span>

    - <span data-ttu-id="f4d64-134">**Projekttyp** – välj alternativet *Migrering*.</span><span class="sxs-lookup"><span data-stu-id="f4d64-134">**Project type** - Select  the *Migration* option.</span></span>
    - <span data-ttu-id="f4d64-135">**Projektnamn** – ange ett namn som är lätt att komma ihåg för projektet.</span><span class="sxs-lookup"><span data-stu-id="f4d64-135">**Project name** - Enter a memorable name for your project.</span></span>
    - <span data-ttu-id="f4d64-136">**Typ av källserver** – välj *SQL Server*.</span><span class="sxs-lookup"><span data-stu-id="f4d64-136">**Source server type** - Select *SQL Server*.</span></span>
    - <span data-ttu-id="f4d64-137">**Typ av målserver** – välj *Azure SQL Database*.</span><span class="sxs-lookup"><span data-stu-id="f4d64-137">**Target server type** - Select *Azure SQL Database*.</span></span>
    - <span data-ttu-id="f4d64-138">**Migreringsomfång** – välj *Schema och data*.</span><span class="sxs-lookup"><span data-stu-id="f4d64-138">**Migration scope** - Select *Schema and data*.</span></span>

1. <span data-ttu-id="f4d64-139">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-139">Click **Create**.</span></span>

## <a name="specify-the-source-server-and-database"></a><span data-ttu-id="f4d64-140">Ange källserver och databas</span><span class="sxs-lookup"><span data-stu-id="f4d64-140">Specify the source server and database</span></span>

1. <span data-ttu-id="f4d64-141">Under **Connect to source server** (Anslut till källserver) går du till fältet **Servernamn** och anger namnet på SQL Server-källinstansen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-141">Under **Connect to source server**, in the **Server name** field, enter the name of the source SQL Server instance.</span></span>

1. <span data-ttu-id="f4d64-142">Välj den **autentiseringstyp** som stöds av SQL Server-källinstansen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-142">Select the **Authentication type** supported by the source SQL Server instance.</span></span>
    > [!TIP]
    > <span data-ttu-id="f4d64-143">Vi rekommenderar att du markerar kryssrutan **Kryptera anslutning** Anslutningsegenskaper för att kryptera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-143">It is recommended that you select the **Encrypt connection** check box under Connection properties to encrypt the connection.</span></span>

1. <span data-ttu-id="f4d64-144">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-144">Select **Connect**.</span></span>

> [!NOTE]
> <span data-ttu-id="f4d64-145">Om autentiseringen till servern misslyckas eftersom du inte har behörighet från din IP-adress öppnar du SQL Server-instansen i **Azure Management Studio**, går till **Brandväggar och virtuella nätverk** och lägger till en regel .</span><span class="sxs-lookup"><span data-stu-id="f4d64-145">If authentication to the server fails because you do not have permission from your IP address, open the SQL Server instance in **Azure Management Studio**, go to **Firewalls and Virtual Networks** and add a rule.</span></span> <span data-ttu-id="f4d64-146">Ge den valfritt namn, till exempel **Tillåt Azure**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-146">Name it whatever you wish, for example, **Allow Azure**.</span></span> <span data-ttu-id="f4d64-147">Konfigurera ett intervall med IP-adresser som har behörighet att komma åt den här instansen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-147">Set up a range of IP addresses that are allowed to access this instance.</span></span> <span data-ttu-id="f4d64-148">Intervallet kan vara 0.0.0.0 255.255.255.255.</span><span class="sxs-lookup"><span data-stu-id="f4d64-148">The range can be 0.0.0.0 to 255.255.255.255.</span></span> <span data-ttu-id="f4d64-149">Spara den nya regeln. Nu bör du kunna komma åt Azure SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-149">Save this new rule, and you should be able to access the Azure SQL Server instance.</span></span> 

1. <span data-ttu-id="f4d64-150">Välj en enskild källdatabas att migrera till Azure SQL Database och klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-150">Select a single source database to migrate to Azure SQL Database and click **Next**.</span></span>

## <a name="specify-the-target-server-and-database"></a><span data-ttu-id="f4d64-151">Ange målserver och databas</span><span class="sxs-lookup"><span data-stu-id="f4d64-151">Specify the target server and database</span></span>

1. <span data-ttu-id="f4d64-152">Under **Connect to target server** (Anslut till målserver) går du till fältet **Servernamn** och anger namnet på Azure SQL Server Database-instansen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-152">Under **Connect to target server**, in the **Server name** field, enter the name of the Azure SQL Database instance.</span></span> <span data-ttu-id="f4d64-153">Lägg till **.database.windows.net** till databasinstansen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-153">Add **.database.windows.net** to the database instance.</span></span>

1. <span data-ttu-id="f4d64-154">Välj den autentiseringstyp som stöds av Azure SQL Database-målinstansen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-154">Select the Authentication type supported by the target Azure SQL Database instance.</span></span> <span data-ttu-id="f4d64-155">För den här självstudien väljer du **SQL Server-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-155">For this tutorial, select **SQL Server Authentication**.</span></span>
    > [!TIP]
    > <span data-ttu-id="f4d64-156">Vi rekommenderar att du markerar kryssrutan **Kryptera anslutning** Anslutningsegenskaper för att kryptera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-156">It is recommended that you select the **Encrypt connection** check box under Connection properties to encrypt the connection.</span></span>

1. <span data-ttu-id="f4d64-157">Ange **användarnamnet** och **lösenordet** som du definierade när du skapade Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-157">Enter the **Username** and **Password** that you defined when you created the Azure SQL Database.</span></span>

1. <span data-ttu-id="f4d64-158">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-158">Click **Connect**.</span></span>

1. <span data-ttu-id="f4d64-159">Välj en enkel måldatabas att migrera till.</span><span class="sxs-lookup"><span data-stu-id="f4d64-159">Select a single target database to which to migrate.</span></span>
    > <span data-ttu-id="f4d64-160">[Obs!] Om du planerar att migrera Windows-användare kontrollerar du att målets externa användares domännamn har angetts korrekt i **fältet för målets externa användares domännamn**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-160">[NOTE] If you intend to migrate Windows users, in the **Target external user domain name** field, make sure that the target external user domain name is specified correctly.</span></span>

1. <span data-ttu-id="f4d64-161">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-161">Click **Next**.</span></span>

## <a name="select-schema-objects"></a><span data-ttu-id="f4d64-162">Välja schemaobjekt</span><span class="sxs-lookup"><span data-stu-id="f4d64-162">Select schema objects</span></span>

1. <span data-ttu-id="f4d64-163">Välj de schemaobjekt från källdatabasen som du vill migrera till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f4d64-163">Select the schema objects from the source database that you want to migrate to Azure SQL Database.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f4d64-164">Några av de objekt som inte kan konverteras som de är erbjuds möjligheter för automatisk korrigering.</span><span class="sxs-lookup"><span data-stu-id="f4d64-164">Some of the objects that cannot be converted as-is are presented with automatic fix opportunities.</span></span> <span data-ttu-id="f4d64-165">Om du klickar på objekten i den vänstra fönsterrutan visas de föreslagna korrigeringarna i den högra fönsterrutan.</span><span class="sxs-lookup"><span data-stu-id="f4d64-165">Clicking these objects on the left pane displays the suggested fixes on the right pane.</span></span> <span data-ttu-id="f4d64-166">Granska korrigeringarna och välj att tillämpa eller ignorera alla ändringar, objekt för objekt.</span><span class="sxs-lookup"><span data-stu-id="f4d64-166">Review the fixes and choose to either apply or ignore all changes, object by object.</span></span> <span data-ttu-id="f4d64-167">Observera att om du tillämpar eller ignorerar alla ändringar för ett objekt så påverkas inte ändringar för andra databasobjekt.</span><span class="sxs-lookup"><span data-stu-id="f4d64-167">Note that applying or ignoring all changes for one object does not affect changes to other database objects.</span></span> <span data-ttu-id="f4d64-168">Instruktioner som inte kan konverteras eller repareras automatiskt reproduceras till måldatabasen och kommenteras.</span><span class="sxs-lookup"><span data-stu-id="f4d64-168">Statements that cannot be converted or automatically fixed are reproduced to the target database and commented.</span></span>

    ![Skärmbild av Data Migration Assistant som visar en lista med migreringsproblem som påverkar AdventureWorks SQL Server-data med felet ”Stored Procedure: dbo.uspSearchCandidateResumes” (Lagrad procedur: dbo.uspSearchCandidateResumes) markerat och probleminformation som visas.](../media-draft/5-suggested-fix.png)

1. <span data-ttu-id="f4d64-170">Klicka på **Generera SQL-skript**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-170">Click **Generate SQL script**.</span></span>

## <a name="deploy-schema"></a><span data-ttu-id="f4d64-171">Distribuera schema</span><span class="sxs-lookup"><span data-stu-id="f4d64-171">Deploy schema</span></span>

1. <span data-ttu-id="f4d64-172">Klicka på **Distribuera schema**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-172">Click **Deploy schema**.</span></span>

1. <span data-ttu-id="f4d64-173">Granska resultatet av schemadistributionen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-173">Review the results of the schema deployment.</span></span>

## <a name="migrate-data"></a><span data-ttu-id="f4d64-174">Migrera data</span><span class="sxs-lookup"><span data-stu-id="f4d64-174">Migrate data</span></span>

1. <span data-ttu-id="f4d64-175">Klicka på **Migrera data** för att datamigreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-175">Click **Migrate data** to start the data migration process.</span></span>

1. <span data-ttu-id="f4d64-176">Välj tabellerna med de data som du vill migrera.</span><span class="sxs-lookup"><span data-stu-id="f4d64-176">Select the tables with the data you want to migrate.</span></span>

1. <span data-ttu-id="f4d64-177">Klicka på **Starta datamigrering**.</span><span class="sxs-lookup"><span data-stu-id="f4d64-177">Click **Start data migration**.</span></span>

<span data-ttu-id="f4d64-178">Den sista skärmen visar övergripande status.</span><span class="sxs-lookup"><span data-stu-id="f4d64-178">The final screen shows the overall status.</span></span>

## <a name="summary"></a><span data-ttu-id="f4d64-179">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f4d64-179">Summary</span></span>

<span data-ttu-id="f4d64-180">I den här delen skapade du en tom Azure SQL Database och migrerade en lokal SQL Server-databas till den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="f4d64-180">In this unit, you created an empty Azure SQL Database and migrated a local SQL Server database to this new database.</span></span>