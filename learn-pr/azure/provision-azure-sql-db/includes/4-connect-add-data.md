<span data-ttu-id="b7105-101">Innan du ansluter databasen till din app måste du kontrollera att du kan ansluta till den, lägga till en enkel tabell och arbeta med exempeldata.</span><span class="sxs-lookup"><span data-stu-id="b7105-101">Before you connect the database to your app, you want to check that you can connect to it, add a basic table, and work with sample data.</span></span>

<span data-ttu-id="b7105-102">Vi står för infrastrukturen, uppdateringar och korrigeringar till din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b7105-102">We maintain the infrastructure, software updates, and patches for your Azure SQL database.</span></span> <span data-ttu-id="b7105-103">Utöver det kan du hantera din Azure SQL-databas som vilken SQL Server-installation som helst.</span><span class="sxs-lookup"><span data-stu-id="b7105-103">Beyond that, you can treat your Azure SQL database like you would any other SQL Server installation.</span></span> <span data-ttu-id="b7105-104">Du kan till exempel använda Visual Studio, SQL Server Management Studio, SQL Server Operations Studio eller andra verktyg för att hantera din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b7105-104">For example, you can use Visual Studio, SQL Server Management Studio, SQL Server Operations Studio, or other tools to manage your Azure SQL database.</span></span>

<span data-ttu-id="b7105-105">Hur du söker åtkomst till din databas och ansluter den till din app är upp till dig.</span><span class="sxs-lookup"><span data-stu-id="b7105-105">How you access your database and connect it to your app is up to you.</span></span> <span data-ttu-id="b7105-106">Men för att få lite mer erfarenhet av att arbeta med databasen har du här möjlighet att prova på att ansluta den direkt från portalen, skapa en tabell och köra några grundläggande CRUD-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b7105-106">But to get some experience working with your database, here you'll connect to it directly from the portal, create a table, and run a few basic CRUD operations.</span></span> <span data-ttu-id="b7105-107">Du får lära dig detta:</span><span class="sxs-lookup"><span data-stu-id="b7105-107">You'll learn:</span></span>

- <span data-ttu-id="b7105-108">Vad Cloud Shell är och hur du kommer åt detta från portalen.</span><span class="sxs-lookup"><span data-stu-id="b7105-108">What Cloud Shell is and how to access it from the portal.</span></span>
- <span data-ttu-id="b7105-109">Hur du hittar information om din databas med Azures kommandoradsgränssnitt (CLI), inklusive anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="b7105-109">How to access information about your database from the Azure CLI, including connection strings.</span></span>
- <span data-ttu-id="b7105-110">Hur du ansluter till databasen med `sqlcmd`.</span><span class="sxs-lookup"><span data-stu-id="b7105-110">How to connect to your database using `sqlcmd`.</span></span>
- <span data-ttu-id="b7105-111">Hur du initierar databasen med en enkel tabell och exempeldata.</span><span class="sxs-lookup"><span data-stu-id="b7105-111">How to initialize your database with a basic table and some sample data.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="b7105-112">Vad är Azure Cloud Shell?</span><span class="sxs-lookup"><span data-stu-id="b7105-112">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="b7105-113">Azure Cloud Shell är ett webbläsarbaserat gränssnitt som gör det möjligt att hantera och utveckla Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="b7105-113">Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources.</span></span> <span data-ttu-id="b7105-114">Man kan säga att Cloud Shell är en interaktiv konsol som körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="b7105-114">Think of Cloud Shell as an interactive console that runs in the cloud.</span></span>

<span data-ttu-id="b7105-115">Cloud Shell körs egentligen på Linux i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="b7105-115">Behind the scenes, Cloud Shell runs on Linux.</span></span> <span data-ttu-id="b7105-116">Beroende på om du föredrar en Linux- eller en Windows-miljö kan du välja mellan två olika versioner: Bash och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b7105-116">But depending on whether you prefer a Linux or Windows environment, you have two experiences to choose from: Bash and PowerShell.</span></span>

<span data-ttu-id="b7105-117">Cloud Shell kan användas var du än befinner dig.</span><span class="sxs-lookup"><span data-stu-id="b7105-117">Cloud Shell is accessible from anywhere.</span></span> <span data-ttu-id="b7105-118">Förutom på portalen kan du också använda Cloud Shell via [shell.azure.com](https://shell.azure.com/), Azure-mobilappen eller Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b7105-118">Besides the portal, you can also access Cloud Shell from [shell.azure.com](https://shell.azure.com/), the Azure mobile app, or from Visual Studio Code.</span></span> <span data-ttu-id="b7105-119">Panelen till höger är en Cloud Shell-terminal som du kan använda under den här övningen.</span><span class="sxs-lookup"><span data-stu-id="b7105-119">The panel on the right is a Cloud Shell terminal for you to use during this exercise.</span></span>

<span data-ttu-id="b7105-120">Cloud Shell innehåller flera populära verktyg och redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="b7105-120">Cloud Shell includes popular tools and text editors.</span></span> <span data-ttu-id="b7105-121">Här följer en snabb titt på `az`, `jq` och `sqlcmd` – tre verktyg som du behöver för den aktuella uppgiften.</span><span class="sxs-lookup"><span data-stu-id="b7105-121">Here's a brief look at `az`, `jq`, and `sqlcmd`, three tools you'll use for our current task.</span></span>

- <span data-ttu-id="b7105-122">`az` är även känt som Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b7105-122">`az` is also known as the Azure CLI.</span></span> <span data-ttu-id="b7105-123">Det är kommandoradsgränssnittet som används när du arbetar med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="b7105-123">It's the command-line interface for working with Azure resources.</span></span> <span data-ttu-id="b7105-124">Används för att hämta information om din databas, inklusive anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="b7105-124">You'll use this to get information about your database, including the connection string.</span></span>
- <span data-ttu-id="b7105-125">`jq` är en JSON-parser för kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b7105-125">`jq` is a command-line JSON parser.</span></span> <span data-ttu-id="b7105-126">Du kan styra utdata från `az`-kommandon till det här verktyget för att extrahera viktiga fält från JSON-utdata.</span><span class="sxs-lookup"><span data-stu-id="b7105-126">You'll pipe output from `az` commands to this tool to extract important fields from JSON output.</span></span>
- <span data-ttu-id="b7105-127">Med `sqlcmd` kan du köra instruktioner på SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b7105-127">`sqlcmd` enables you to execute statements on SQL Server.</span></span> <span data-ttu-id="b7105-128">Du kommer att använda `sqlcmd` för att skapa en interaktiv session med din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b7105-128">You'll use `sqlcmd` to create an interactive session with your Azure SQL database.</span></span>

## <a name="get-information-about-your-azure-sql-database"></a><span data-ttu-id="b7105-129">Hämta information om din Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="b7105-129">Get information about your Azure SQL database</span></span>

<span data-ttu-id="b7105-130">Innan du ansluter till databasen är det en bra om du kontrollerar att den verkligen finns och att den är online.</span><span class="sxs-lookup"><span data-stu-id="b7105-130">Before you connect to your database, it's a good idea to verify it exists and is online.</span></span>

<span data-ttu-id="b7105-131">Här kan du använda `az`-verktyget för att visa en lista över dina databaser och information om databasen **Logistics** (Logistik), inklusive maximal storlek och status.</span><span class="sxs-lookup"><span data-stu-id="b7105-131">Here, you use the `az` utility to list your databases and show some information about the **Logistics** database, including its maximum size and status.</span></span>

1. <span data-ttu-id="b7105-132">`az`-kommandona som du kör behöver tillgång till namnet på resursgruppen och namnet på den logiska Azure SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b7105-132">The `az` commands you'll run require the name of your resource group and the name of your Azure SQL logical server.</span></span> <span data-ttu-id="b7105-133">Om du vill slippa skriva själv kan du köra `azure configure`-kommandot för att ange dem som standardvärden.</span><span class="sxs-lookup"><span data-stu-id="b7105-133">To save typing, run this `azure configure` command to specify them as default values.</span></span>
    <span data-ttu-id="b7105-134">Ersätt `<server-name>` med namnet på den logiska Azure SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b7105-134">Replace `<server-name>` with the name of your Azure SQL logical server.</span></span> <span data-ttu-id="b7105-135">Observera att beroende på det blad du är på i portalen så kan det här visas som ett fullständigt domännamn (servername.database.windows.net), men du behöver bara det logiska namnet utan suffixet .database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="b7105-135">Note that depending on the blade you are on in the portal this may show as a FQDN (servername.database.windows.net), but you only need the logical name without the .database.windows.net suffix.</span></span>

    ```azurecli
    az configure --defaults group=<rgn>[sandbox resource group name]</rgn> sql-server=<server-name>
    ```

1. <span data-ttu-id="b7105-136">Kör `az sql db list` för att lista alla databaser på den logiska Azure SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b7105-136">Run `az sql db list` to list all databases on your Azure SQL logical server.</span></span>

    ```azurecli
    az sql db list
    ```
    <span data-ttu-id="b7105-137">Du ser ett stort JSON-block som utdata.</span><span class="sxs-lookup"><span data-stu-id="b7105-137">You see a large block of JSON as output.</span></span>

1. <span data-ttu-id="b7105-138">Eftersom vi bara vill ha databasnamnen kör du kommandot en gång till.</span><span class="sxs-lookup"><span data-stu-id="b7105-138">Since we want just the database names, run the command a second time.</span></span> <span data-ttu-id="b7105-139">Den här gången styr du utdata till `jq` för att endast skriva ut namnfälten.</span><span class="sxs-lookup"><span data-stu-id="b7105-139">This time, pipe the output to `jq` to print out only the name fields.</span></span>
   
     ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    
    <span data-ttu-id="b7105-140">Du bör se dessa utdata.</span><span class="sxs-lookup"><span data-stu-id="b7105-140">You should see this output.</span></span>
    
    ```json
    [
      {
        "name": "Logistics"
      },
      {
        "name": "master"
      }
    ]
    ```

    <span data-ttu-id="b7105-141">Din databas heter **Logistics**.</span><span class="sxs-lookup"><span data-stu-id="b7105-141">**Logistics** is your database.</span></span> <span data-ttu-id="b7105-142">Precis som på SQL Server innehåller **master** metadata för servern som till exempel inloggningskonton och konfigurationsinställningar för systemet.</span><span class="sxs-lookup"><span data-stu-id="b7105-142">Like SQL Server, **master** includes server metadata, such as sign-in accounts and system configuration settings.</span></span>

1. <span data-ttu-id="b7105-143">Kör `az sql db show`-kommandot för att få information om databasen **Logistics** (Logistik).</span><span class="sxs-lookup"><span data-stu-id="b7105-143">Run this `az sql db show` command to get details about the **Logistics** database.</span></span>

    ```azurecli
    az sql db show --name Logistics
    ```

    <span data-ttu-id="b7105-144">Precis som tidigare ser du ett stort JSON-block som utdata.</span><span class="sxs-lookup"><span data-stu-id="b7105-144">As before, you see a large block of JSON as output.</span></span>

1. <span data-ttu-id="b7105-145">Kör kommandot en gång till.</span><span class="sxs-lookup"><span data-stu-id="b7105-145">Run the command a second time.</span></span> <span data-ttu-id="b7105-146">Den här gången styr du utdata till `jq` för att begränsa utdata till endast namn, maximal storlek och status för databasen **Logistics** (Logistik).</span><span class="sxs-lookup"><span data-stu-id="b7105-146">This time, pipe the output to `jq` to limit output to only the name, maximum size, and status of the **Logistics** database.</span></span>

    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```

    <span data-ttu-id="b7105-147">Du ser nu att databasen är online och rymmer cirka 2 GB data.</span><span class="sxs-lookup"><span data-stu-id="b7105-147">You see that the database is online and can hold around 2 GB of data.</span></span>

    ```json
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a><span data-ttu-id="b7105-148">Ansluta till databasen</span><span class="sxs-lookup"><span data-stu-id="b7105-148">Connect to your database</span></span>

<span data-ttu-id="b7105-149">Nu när du vet lite mer om databasen ska vi prova att ansluta till den med hjälp av `sqlcmd`, skapa en tabell med information om chaufförer och utföra några grundläggande CRUD-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b7105-149">Now that you understand a bit about your database, let's connect to it using `sqlcmd`, create a table that holds information about transportation drivers, and perform a few basic CRUD operations.</span></span>

<span data-ttu-id="b7105-150">CRUD handlar om att **skapa**, **läsa**, **uppdatera** och **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="b7105-150">Remember that CRUD stands for **create**, **read**, **update**, and **delete**.</span></span> <span data-ttu-id="b7105-151">De här villkoren avser åtgärder som du kan använda tillsammans med tabelldata och som utgör de fyra grundläggande funktioner som du behöver för din app.</span><span class="sxs-lookup"><span data-stu-id="b7105-151">These terms refer to operations you perform on table data and are the four basic operations you need for your app.</span></span> <span data-ttu-id="b7105-152">Nu är det dags att kontrollera att du kan utföra var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="b7105-152">Now's a good time to verify you can perform each of them.</span></span>

1. <span data-ttu-id="b7105-153">Kör `az sql db show-connection-string`-kommandot för att hämta anslutningssträngen till databasen **Logistics** (Logistik) i ett format som `sqlcmd` kan använda.</span><span class="sxs-lookup"><span data-stu-id="b7105-153">Run this `az sql db show-connection-string` command to get the connection string to the **Logistics** database in a format that `sqlcmd` can use.</span></span>

    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```

    <span data-ttu-id="b7105-154">Du ser utdata av den här typen.</span><span class="sxs-lookup"><span data-stu-id="b7105-154">Your output resembles this.</span></span>

    ```output
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```

1. <span data-ttu-id="b7105-155">Kör `sqlcmd`-uttrycket från föregående stegs utdata för att skapa en interaktiv session.</span><span class="sxs-lookup"><span data-stu-id="b7105-155">Run the `sqlcmd` statement from the output of the previous step to create an interactive session.</span></span> <span data-ttu-id="b7105-156">Ta bort de omgivande citattecknen och ersätt `<username>` och `<password>` med användarnamnet och lösenordet som du angav när du skapade databasen.</span><span class="sxs-lookup"><span data-stu-id="b7105-156">Remove the surrounding quotes and replace `<username>` and `<password>` with the username and password you specified when you created your database.</span></span> <span data-ttu-id="b7105-157">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="b7105-157">Here's an example.</span></span>

    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```

    > [!TIP]
    > <span data-ttu-id="b7105-158">Placera ditt lösenord inom citattecken så att ”&” och andra specialtecken inte tolkas som bearbetningsinstruktioner.</span><span class="sxs-lookup"><span data-stu-id="b7105-158">Place your password in quotes so that "&" and other special characters aren't interpreted as processing instructions.</span></span>

1. <span data-ttu-id="b7105-159">Skapa en tabell med namnet `Drivers` från `sqlcmd`-sessionen.</span><span class="sxs-lookup"><span data-stu-id="b7105-159">From your `sqlcmd` session, create a table named `Drivers`.</span></span>

    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255));
    GO
    ```

    <span data-ttu-id="b7105-160">Tabellen har fyra kolumner: ett unikt ID, chaufförens för- och efternamn samt vilken ort chauffören är stationerad på.</span><span class="sxs-lookup"><span data-stu-id="b7105-160">The table contains four columns: a unique identifier, the driver's last and first name, and the driver's city of origin.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b7105-161">Språket som visas här är Transact-SQL eller T-SQL.</span><span class="sxs-lookup"><span data-stu-id="b7105-161">The language you see here is Transact-SQL, or T-SQL.</span></span>

1. <span data-ttu-id="b7105-162">Kontrollera att tabellen `Drivers` finns.</span><span class="sxs-lookup"><span data-stu-id="b7105-162">Verify that the `Drivers` table exists.</span></span>

    ```sql
    SELECT name FROM sys.tables;
    GO
    ```

    <span data-ttu-id="b7105-163">Du bör se dessa utdata.</span><span class="sxs-lookup"><span data-stu-id="b7105-163">You should see this output.</span></span>

    ```output
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers

    (1 rows affected)
    ```

1. <span data-ttu-id="b7105-164">Kör T-SQL-uttrycket `INSERT` för att lägga till en exempelrad i tabellen.</span><span class="sxs-lookup"><span data-stu-id="b7105-164">Run this `INSERT` T-SQL statement to add a sample row to the table.</span></span> <span data-ttu-id="b7105-165">Det här är åtgärden **create** (skapa).</span><span class="sxs-lookup"><span data-stu-id="b7105-165">This is the **create** operation.</span></span>

    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Zirne', 'Laura', 'Springfield');
    GO
    ```

    <span data-ttu-id="b7105-166">Detta är en indikation på att åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="b7105-166">You see this to indicate the operation succeeded.</span></span>

    ```output
    (1 rows affected)
    ```

1. <span data-ttu-id="b7105-167">Kör det här T-SQL-uttrycket `SELECT` för att lista `DriverID`- och `OriginCity`-kolumnerna från alla rader i tabellen.</span><span class="sxs-lookup"><span data-stu-id="b7105-167">Run this `SELECT` T-SQL statement to list the `DriverID` and `OriginCity` columns from all rows in the table.</span></span> <span data-ttu-id="b7105-168">Det här är åtgärden **read** (läsa).</span><span class="sxs-lookup"><span data-stu-id="b7105-168">This is the **read** operation.</span></span>

    ```sql
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    <span data-ttu-id="b7105-169">Du ser ett resultat med `DriverID` och `OriginCity` för den rad som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="b7105-169">You see one result with the `DriverID` and `OriginCity` for the row you created in the previous step.</span></span>

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Springfield

    (1 rows affected)
    ```

1. <span data-ttu-id="b7105-170">Kör det här T-SQL-uttrycket `UPDATE` för att ändra ursprungsstad från Springfield till Boston för chauffören med `DriverID` 123.</span><span class="sxs-lookup"><span data-stu-id="b7105-170">Run this `UPDATE` T-SQL statement to change the city of origin from "Springfield" to "Boston" for the driver with a `DriverID` of 123.</span></span> <span data-ttu-id="b7105-171">Det här är **update**-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b7105-171">This is the **update** operation.</span></span>

    ```sql
    UPDATE Drivers SET OriginCity='Boston' WHERE DriverID=123;
    GO
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    <span data-ttu-id="b7105-172">Du bör se följande utdata.</span><span class="sxs-lookup"><span data-stu-id="b7105-172">You should see the following output.</span></span> <span data-ttu-id="b7105-173">Observera hur `OriginCity` återspeglar Boston-uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="b7105-173">Notice how the `OriginCity` reflects the update to Boston.</span></span>

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Boston

    (1 rows affected)
    ```

1. <span data-ttu-id="b7105-174">Kör T-SQL-uttrycket `DELETE` för att ta bort posten.</span><span class="sxs-lookup"><span data-stu-id="b7105-174">Run this `DELETE` T-SQL statement to delete the record.</span></span> <span data-ttu-id="b7105-175">Det här är åtgärden **delete** (ta bort).</span><span class="sxs-lookup"><span data-stu-id="b7105-175">This is the **delete** operation.</span></span>

    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```

    ```output
    (1 rows affected)
    ```

1. <span data-ttu-id="b7105-176">Kör T-SQL-uttrycket `SELECT` för att kontrollera att tabellen `Drivers` är tom.</span><span class="sxs-lookup"><span data-stu-id="b7105-176">Run this `SELECT` T-SQL statement to verify the `Drivers` table is empty.</span></span>

    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```

    <span data-ttu-id="b7105-177">Du ser att tabellen inte innehåller några rader.</span><span class="sxs-lookup"><span data-stu-id="b7105-177">You see that the table contains no rows.</span></span>

    ```output
    -----------
              0

    (1 rows affected)
    ```

<span data-ttu-id="b7105-178">Nu när du har fått en introduktion till hur man arbetar med Azure SQL Database från Cloud Shell kan du hämta anslutningssträngen för ditt SQL-hanteringsverktyg &ndash; oavsett om den hämtas från SQL Server Management Studio, Visual Studio eller något annat.</span><span class="sxs-lookup"><span data-stu-id="b7105-178">Now that you have the hang of working with Azure SQL Database from Cloud Shell, you can get the connection string for your favorite SQL management tool &ndash; whether that's from SQL Server Management Studio, Visual Studio, or something else.</span></span>

<span data-ttu-id="b7105-179">Cloud Shell gör det enkelt att komma åt och arbeta med dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="b7105-179">Cloud Shell makes it easy to access and work with your Azure resources.</span></span> <span data-ttu-id="b7105-180">Eftersom Cloud Shell är ett webbläsarbaserat gränssnitt kan du komma åt det från såväl Windows som macOS eller Linux – i stort sett vilket system som helst, bara det finns en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b7105-180">Because Cloud Shell is browser-based, you can access it from Windows, macOS, or Linux &ndash; essentially any system with a web browser.</span></span>

<span data-ttu-id="b7105-181">Nu har du fått lite praktisk erfarenhet av att köra kommandon via Azures kommandoradsgränssnitt och visa information om din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b7105-181">You gained some hands-on experience running Azure CLI commands to get information about your Azure SQL database.</span></span> <span data-ttu-id="b7105-182">Som överkurs har du också fått öva på att skriva T-SQL.</span><span class="sxs-lookup"><span data-stu-id="b7105-182">As a bonus, you practiced your T-SQL skills.</span></span>

<span data-ttu-id="b7105-183">Vi avslutar med att titta närmare på hur man gör för att plocka ned databasen.</span><span class="sxs-lookup"><span data-stu-id="b7105-183">Finally, let's wrap up and see how to tear down your database.</span></span>
