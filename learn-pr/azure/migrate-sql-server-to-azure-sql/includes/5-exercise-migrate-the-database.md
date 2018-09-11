<span data-ttu-id="99dd1-101">Nu när du har utvärderat din databas och åtgärdat eventuella rapporterade fel är du redo att migrera din databas.</span><span class="sxs-lookup"><span data-stu-id="99dd1-101">Now that you have assessed your database and fixed any errors reported, you're ready to migrate your database.</span></span> <span data-ttu-id="99dd1-102">I den här delen kommer du att migrera din SQL-databas till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="99dd1-102">In this unit, you will migrate your SQL database to Azure SQL Database.</span></span>

## <a name="setup"></a><span data-ttu-id="99dd1-103">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="99dd1-103">Setup</span></span>

<span data-ttu-id="99dd1-104">Ladda ned exempeldata om du inte redan har gjort det.</span><span class="sxs-lookup"><span data-stu-id="99dd1-104">If you haven't already, download the sample data.</span></span>

1. <span data-ttu-id="99dd1-105">Öppna en webbläsare och navigera till https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span><span class="sxs-lookup"><span data-stu-id="99dd1-105">Start an internet browser and navigate to https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

1. <span data-ttu-id="99dd1-106">I **OLTP downloads** (OLTP-nedladdningar) klickar du på **AdventureWorks2008R2.bak**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-106">In **OLTP downloads**, click **AdventureWorks2008R2.bak**.</span></span>

1. <span data-ttu-id="99dd1-107">I Management Studio återställer du **AdventureWorks 2008R2** till din standardinstans.</span><span class="sxs-lookup"><span data-stu-id="99dd1-107">In Management Studio, restore **AdventureWorks 2008R2** to your default instance.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="99dd1-108">Skapa en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="99dd1-108">Create an Azure SQL Database</span></span>

1. <span data-ttu-id="99dd1-109">Logga in på ditt Azure Portal-konto.</span><span class="sxs-lookup"><span data-stu-id="99dd1-109">Sign in to your Azure portal account.</span></span>

1. <span data-ttu-id="99dd1-110">Klicka på **Skapa en resurs** längst upp till vänster i portalen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-110">Click **Create a resource** in the upper left-hand corner of the portal.</span></span>

1. <span data-ttu-id="99dd1-111">Klicka på **Databaser** > **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-111">Click **Databases** > **SQL Database**.</span></span>

1. <span data-ttu-id="99dd1-112">Ange följande fält i SQL Database-formuläret:</span><span class="sxs-lookup"><span data-stu-id="99dd1-112">Enter the following fields on the SQL Database form:</span></span>

    |<span data-ttu-id="99dd1-113">Fält</span><span class="sxs-lookup"><span data-stu-id="99dd1-113">Field</span></span>|<span data-ttu-id="99dd1-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="99dd1-114">Description</span></span>|
    |-----|---|
    |<span data-ttu-id="99dd1-115">Databasnamn</span><span class="sxs-lookup"><span data-stu-id="99dd1-115">Database name</span></span>|<span data-ttu-id="99dd1-116">Ange ett namn för din databas.</span><span class="sxs-lookup"><span data-stu-id="99dd1-116">Enter a name for your database.</span></span> <span data-ttu-id="99dd1-117">Detta kan vara namnet på den befintliga databas som du migrerar eller valfritt nytt namn</span><span class="sxs-lookup"><span data-stu-id="99dd1-117">This can be the name of your existing database that you're migrating, or any new name of your choice</span></span>|
    |<span data-ttu-id="99dd1-118">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="99dd1-118">Resource group</span></span>|<span data-ttu-id="99dd1-119">Välj ett resursgruppsnamn eller ange ett nytt resursgruppsnamn</span><span class="sxs-lookup"><span data-stu-id="99dd1-119">Select a resource group or enter a new resource group name</span></span>|
    |<span data-ttu-id="99dd1-120">Välj källa</span><span class="sxs-lookup"><span data-stu-id="99dd1-120">Select source</span></span>|<span data-ttu-id="99dd1-121">Välj *Tom databas*</span><span class="sxs-lookup"><span data-stu-id="99dd1-121">Select *Blank database*</span></span>|

1. <span data-ttu-id="99dd1-122">Under **Server** väljer du antingen en befintlig server eller anger ett globalt unikt **servernamn** en **inloggning för serveradministratör** ett **lösenord** (som du ska bekräfta) samt en **plats**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-122">Under **Server**, either select an existing server or, enter a globally unique **Server name**, a **Server admin login**, a **Password** (which you should confirm), and a **Location**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="99dd1-123">Du använder servernamnet, inloggningsnamnet och lösenordet för att ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-123">You will use the server name, login name, and password to connect to your database.</span></span>

1. <span data-ttu-id="99dd1-124">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-124">Click **Select**.</span></span>

1. <span data-ttu-id="99dd1-125">I **Prisnivå** kan du välja en databas med högre prestanda, men för den här självstudien väljer du standardinställningen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-125">In **Pricing tier**, you can select a database with higher performance, but for this tutorial, select default.</span></span>

1. <span data-ttu-id="99dd1-126">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-126">Click **Create**.</span></span> <span data-ttu-id="99dd1-127">Vänta tills databasen har etablerats innan du fortsätter med självstudien.</span><span class="sxs-lookup"><span data-stu-id="99dd1-127">Wait until the database has been provisioned before continuing the tutorial.</span></span>

    <span data-ttu-id="99dd1-128">Följande skärmbild visar en potentiell konfiguration för den nya SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-128">The following screenshot shows a potential configuration for your new SQL database.</span></span>

    ![Skärmbild av ett nytt SQL Database-blad i Azure-portalen med en exempelkonfiguration.](../media-draft/5-create-azure-sql-db.png)

## <a name="create-a-new-migration-project"></a><span data-ttu-id="99dd1-130">Skapa ett nytt migreringsprojekt</span><span class="sxs-lookup"><span data-stu-id="99dd1-130">Create a new migration project</span></span>

1. <span data-ttu-id="99dd1-131">Starta **Data Migration Assistant**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-131">Start **Data Migration Assistant**.</span></span>

1. <span data-ttu-id="99dd1-132">Klicka på ikonen **Nytt** och ange följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="99dd1-132">Click the **New** icon, and specify the following options:</span></span>

    - <span data-ttu-id="99dd1-133">**Projekttyp** – Välj alternativet *Migrering*.</span><span class="sxs-lookup"><span data-stu-id="99dd1-133">**Project type** - Select  the *Migration* option.</span></span>
    - <span data-ttu-id="99dd1-134">**Projektnamn** – ange ett namn som är lätt att komma ihåg för projektet.</span><span class="sxs-lookup"><span data-stu-id="99dd1-134">**Project name** - Enter a memorable name for your project.</span></span>
    - <span data-ttu-id="99dd1-135">**Typ av källserver** – välj *SQL Server*.</span><span class="sxs-lookup"><span data-stu-id="99dd1-135">**Source server type** - Select *SQL Server*.</span></span>
    - <span data-ttu-id="99dd1-136">**Typ av målserver** – välj *Azure SQL Database*.</span><span class="sxs-lookup"><span data-stu-id="99dd1-136">**Target server type** - Select *Azure SQL Database*.</span></span>
    - <span data-ttu-id="99dd1-137">**Migreringsomfång** – välj *Schema och data*.</span><span class="sxs-lookup"><span data-stu-id="99dd1-137">**Migration scope** - Select *Schema and data*.</span></span>

1. <span data-ttu-id="99dd1-138">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-138">Click **Create**.</span></span>

## <a name="specify-the-source-server-and-database"></a><span data-ttu-id="99dd1-139">Ange källserver och databas</span><span class="sxs-lookup"><span data-stu-id="99dd1-139">Specify the source server and database</span></span>

1. <span data-ttu-id="99dd1-140">Under **Connect to source server** (Anslut till källserver) går du till fältet **Servernamn** och anger namnet på SQL Server-källinstansen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-140">Under **Connect to source server**, in the **Server name** field, enter the name of the source SQL Server instance.</span></span>

1. <span data-ttu-id="99dd1-141">Välj den **autentiseringstyp** som stöds av SQL Server-källinstansen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-141">Select the **Authentication type** supported by the source SQL Server instance.</span></span>
    > [!TIP]
    > <span data-ttu-id="99dd1-142">Vi rekommenderar att du markerar kryssrutan **Kryptera anslutning** Anslutningsegenskaper för att kryptera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-142">It is recommended that you select the **Encrypt connection** check box under Connection properties to encrypt the connection.</span></span>

1. <span data-ttu-id="99dd1-143">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-143">Select **Connect**.</span></span>

1. <span data-ttu-id="99dd1-144">Välj en enskild källdatabas att migrera till Azure SQL Database och klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-144">Select a single source database to migrate to Azure SQL Database and click **Next**.</span></span>

## <a name="specify-the-target-server-and-database"></a><span data-ttu-id="99dd1-145">Ange målserver och databas</span><span class="sxs-lookup"><span data-stu-id="99dd1-145">Specify the target server and database</span></span>

1. <span data-ttu-id="99dd1-146">Under **Connect to target server** (Anslut till målserver) går du till fältet **Servernamn** och anger namnet på Azure SQL Server Database-instansen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-146">Under **Connect to target server**, in the **Server name** field, enter the name of the Azure SQL Database instance.</span></span> <span data-ttu-id="99dd1-147">Lägg till **database.windows.net** till databasinstansen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-147">Add **database.windows.net** to the database instance.</span></span>

1. <span data-ttu-id="99dd1-148">Välj den autentiseringstyp som stöds av Azure SQL Database-målinstansen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-148">Select the Authentication type supported by the target Azure SQL Database instance.</span></span> <span data-ttu-id="99dd1-149">För den här självstudien väljer du **SQL Server-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-149">For this tutorial, select **SQL Server Authentication**.</span></span>
    > [!TIP]
    > <span data-ttu-id="99dd1-150">Vi rekommenderar att du markerar kryssrutan **Kryptera anslutning** Anslutningsegenskaper för att kryptera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-150">It is recommended that you select the **Encrypt connection** check box under Connection properties to encrypt the connection.</span></span>

1. <span data-ttu-id="99dd1-151">Ange **användarnamnet** och **lösenordet** som du definierade när du skapade Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-151">Enter the **Username** and **Password** that you defined when you created the Azure SQL Database.</span></span>

1. <span data-ttu-id="99dd1-152">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-152">Click **Connect**.</span></span>

1. <span data-ttu-id="99dd1-153">Välj en enkel måldatabas att migrera till.</span><span class="sxs-lookup"><span data-stu-id="99dd1-153">Select a single target database to which to migrate.</span></span>
    > <span data-ttu-id="99dd1-154">[OBS] Om du planerar att migrera Windows-användare ska du i textrutan för målets externa användares domännamn se till att målets externa användares domännamn anges korrekt.</span><span class="sxs-lookup"><span data-stu-id="99dd1-154">[NOTE] If you intend to migrate Windows users, in the Target external user domain name text box, make sure that the target external user domain name is specified correctly.</span></span>

1. <span data-ttu-id="99dd1-155">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-155">Click **Next**.</span></span>

## <a name="select-schema-objects"></a><span data-ttu-id="99dd1-156">Välj schemaobjekt</span><span class="sxs-lookup"><span data-stu-id="99dd1-156">Select schema objects</span></span>

1. <span data-ttu-id="99dd1-157">Välj de schemaobjekt från källdatabasen som du vill migrera till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="99dd1-157">Select the schema objects from the source database that you want to migrate to Azure SQL Database.</span></span>

    > [!NOTE]
    > <span data-ttu-id="99dd1-158">Några av de objekt som inte kan konverteras som de är erbjuds möjligheter för automatisk korrigering.</span><span class="sxs-lookup"><span data-stu-id="99dd1-158">Some of the objects that cannot be converted as-is are presented with automatic fix opportunities.</span></span> <span data-ttu-id="99dd1-159">Om du klickar på objekten i den vänstra fönsterrutan visas de föreslagna korrigeringarna i den högra fönsterrutan.</span><span class="sxs-lookup"><span data-stu-id="99dd1-159">Clicking these objects on the left pane displays the suggested fixes on the right pane.</span></span> <span data-ttu-id="99dd1-160">Granska korrigeringarna och välj att tillämpa eller ignorera alla ändringar, objekt för objekt.</span><span class="sxs-lookup"><span data-stu-id="99dd1-160">Review the fixes and choose to either apply or ignore all changes, object by object.</span></span> <span data-ttu-id="99dd1-161">Observera att om du tillämpar eller ignorerar alla ändringar för ett objekt så påverkas inte ändringar för andra databasobjekt.</span><span class="sxs-lookup"><span data-stu-id="99dd1-161">Note that applying or ignoring all changes for one object does not affect changes to other database objects.</span></span> <span data-ttu-id="99dd1-162">Instruktioner som inte kan konverteras eller repareras automatiskt reproduceras till måldatabasen och kommenteras.</span><span class="sxs-lookup"><span data-stu-id="99dd1-162">Statements that cannot be converted or automatically fixed are reproduced to the target database and commented.</span></span>

    ![Skärmbild av Data Migration Assistant som visar en lista med migreringsproblem som påverkar AdventureWorks SQL Server-data med felet ”Stored Procedure: dbo.uspSearchCandidateResumes” (Lagrad procedur: dbo.uspSearchCandidateResumes) markerat och probleminformation som visas.](../media-draft/5-suggested-fix.png)

1. <span data-ttu-id="99dd1-164">Klicka på **Generera SQL-skript**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-164">Click **Generate SQL script**.</span></span>

## <a name="deploy-schema"></a><span data-ttu-id="99dd1-165">Distribuera schema</span><span class="sxs-lookup"><span data-stu-id="99dd1-165">Deploy schema</span></span>

1. <span data-ttu-id="99dd1-166">Klicka på **Distribuera schema**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-166">Click **Deploy schema**.</span></span>

1. <span data-ttu-id="99dd1-167">Granska resultatet av schemadistributionen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-167">Review the results of the schema deployment.</span></span>

## <a name="migrate-data"></a><span data-ttu-id="99dd1-168">Migrera data</span><span class="sxs-lookup"><span data-stu-id="99dd1-168">Migrate data</span></span>

1. <span data-ttu-id="99dd1-169">Klicka på **Migrera data** för att datamigreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-169">Click **Migrate data** to start the data migration process.</span></span>

1. <span data-ttu-id="99dd1-170">Välj tabellerna med de data som du vill migrera.</span><span class="sxs-lookup"><span data-stu-id="99dd1-170">Select the tables with the data you want to migrate.</span></span>

1. <span data-ttu-id="99dd1-171">Klicka på **Starta datamigrering**.</span><span class="sxs-lookup"><span data-stu-id="99dd1-171">Click **Start data migration**.</span></span>

<span data-ttu-id="99dd1-172">Den sista skärmen visar övergripande status.</span><span class="sxs-lookup"><span data-stu-id="99dd1-172">The final screen shows the overall status.</span></span>

## <a name="summary"></a><span data-ttu-id="99dd1-173">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="99dd1-173">Summary</span></span>

<span data-ttu-id="99dd1-174">I den här delen skapade du en tom Azure SQL Database och migrerade en lokal SQL Server-databas till den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="99dd1-174">In this unit, you created an empty Azure SQL Database and migrated a local SQL Server database to this new database.</span></span>