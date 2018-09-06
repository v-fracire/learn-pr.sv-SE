<span data-ttu-id="a74d3-101">Innan du ansluter databasen till din app måste du kontrollera att du kan ansluta till den, lägga till en enkel tabell och arbeta med exempeldata.</span><span class="sxs-lookup"><span data-stu-id="a74d3-101">Before you connect the database to your app, you want to check that you can connect to it, add a basic table, and work with sample data.</span></span>

<span data-ttu-id="a74d3-102">Vi står för infrastrukturen, uppdateringar och korrigeringar till din Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a74d3-102">We maintain the infrastructure, software updates, and patches for your Azure SQL Database.</span></span> <span data-ttu-id="a74d3-103">Utöver det kan du hantera din Azure SQL Database som vilken SQL Server-installation som helst.</span><span class="sxs-lookup"><span data-stu-id="a74d3-103">Beyond that, you can treat your Azure SQL Database like you would any other SQL Server installation.</span></span> <span data-ttu-id="a74d3-104">Du kan till exempel använda Visual Studio, SQL Server Management Studio eller andra verktyg för att hantera din Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a74d3-104">For example, you can use Visual Studio, SQL Server Management Studio, or other tools to manage your Azure SQL Database.</span></span>

<span data-ttu-id="a74d3-105">Hur du söker åtkomst till din databas och ansluter den till din app är upp till dig.</span><span class="sxs-lookup"><span data-stu-id="a74d3-105">How you access your database and connect it to your app is up to you.</span></span> <span data-ttu-id="a74d3-106">Men för att få lite mer erfarenhet av att arbeta med databasen har du här möjlighet att prova på att ansluta den direkt från portalen, skapa en tabell och köra några grundläggande CRUD-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a74d3-106">But to get some experience working with your database, here you'll connect to it directly from the portal, create a table, and run a few basic CRUD operations.</span></span> <span data-ttu-id="a74d3-107">Du får lära dig detta:</span><span class="sxs-lookup"><span data-stu-id="a74d3-107">You'll learn:</span></span>

* <span data-ttu-id="a74d3-108">Vad Cloud Shell är och hur du kommer åt detta från portalen.</span><span class="sxs-lookup"><span data-stu-id="a74d3-108">What Cloud Shell is and how to access it from the portal.</span></span>
* <span data-ttu-id="a74d3-109">Hur du hittar information om din databas med Azures kommandoradsgränssnitt (CLI), inklusive anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="a74d3-109">How to access information about your database from the Azure CLI, including connection strings.</span></span>
* <span data-ttu-id="a74d3-110">Hur du ansluter till databasen med `sqlcmd`.</span><span class="sxs-lookup"><span data-stu-id="a74d3-110">How to connect to your database using `sqlcmd`.</span></span>
* <span data-ttu-id="a74d3-111">Hur du initierar databasen med en enkel tabell och exempeldata.</span><span class="sxs-lookup"><span data-stu-id="a74d3-111">How to initialize your database with a basic table and some sample data.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="a74d3-112">Vad är Azure Cloud Shell?</span><span class="sxs-lookup"><span data-stu-id="a74d3-112">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="a74d3-113">Azure Cloud Shell är ett webbläsarbaserat gränssnitt som gör det möjligt att hantera och utveckla Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a74d3-113">Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources.</span></span> <span data-ttu-id="a74d3-114">Man kan säga att Cloud Shell är en interaktiv konsol som körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="a74d3-114">Think of Cloud Shell as an interactive console that runs in the cloud.</span></span>

<span data-ttu-id="a74d3-115">Cloud Shell körs egentligen på Linux i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="a74d3-115">Behind the scenes, Cloud Shell runs on Linux.</span></span> <span data-ttu-id="a74d3-116">Beroende på om du föredrar en Linux- eller en Windows-miljö kan du välja mellan två olika versioner: Bash och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a74d3-116">But depending on whether you prefer a Linux or Windows environment, you have two experiences to choose from: Bash and PowerShell.</span></span>

<span data-ttu-id="a74d3-117">Cloud Shell kan användas var du än befinner dig.</span><span class="sxs-lookup"><span data-stu-id="a74d3-117">The Cloud Shell is accessible from anywhere.</span></span> <span data-ttu-id="a74d3-118">Förutom på portalen kan du också använda Cloud Shell via [shell.azure.com](https://shell.azure.com/), Azure-mobilappen eller Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a74d3-118">Besides the portal, you can also access Cloud Shell from [shell.azure.com](https://shell.azure.com/), the Azure mobile app, or from Visual Studio Code.</span></span>

<span data-ttu-id="a74d3-119">Cloud Shell innehåller flera populära verktyg och redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="a74d3-119">Cloud Shell includes popular tools and text editors.</span></span> <span data-ttu-id="a74d3-120">Här följer en snabb titt på `az`, `jq` och `sqlcmd` – tre verktyg som du behöver för den aktuella uppgiften.</span><span class="sxs-lookup"><span data-stu-id="a74d3-120">Here's a brief look at `az`, `jq`, and `sqlcmd`, three tools you'll use for our current task.</span></span>

* <span data-ttu-id="a74d3-121">`az` är även känt som Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a74d3-121">`az` is also known as the Azure CLI.</span></span> <span data-ttu-id="a74d3-122">Det är kommandoradsgränssnittet som används när du arbetar med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a74d3-122">It's the command-line interface for working with Azure resources.</span></span> <span data-ttu-id="a74d3-123">Används för att hämta information om din databas, inklusive anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="a74d3-123">You'll use this to get information about your database, including the connection string.</span></span>
* <span data-ttu-id="a74d3-124">`jq` är en JSON-parser för kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="a74d3-124">`jq` is a command-line JSON parser.</span></span> <span data-ttu-id="a74d3-125">Du kan styra utdata från `az`-kommandon till det här verktyget för att extrahera viktiga fält från JSON-utdata.</span><span class="sxs-lookup"><span data-stu-id="a74d3-125">You'll pipe output from `az` commands to this tool to extract important fields from JSON output.</span></span>
* <span data-ttu-id="a74d3-126">Med `sqlcmd` kan du köra instruktioner på SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a74d3-126">`sqlcmd` enables you to execute statements on SQL Server.</span></span> <span data-ttu-id="a74d3-127">Du kommer att använda `sqlcmd` för att skapa en interaktiv session med din Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a74d3-127">You'll use `sqlcmd` to create an interactive session with your Azure SQL Database.</span></span>

## <a name="get-information-about-your-azure-sql-database"></a><span data-ttu-id="a74d3-128">Hämta information om Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="a74d3-128">Get information about your Azure SQL Database</span></span>

<span data-ttu-id="a74d3-129">Innan du ansluter till databasen är det en bra om du kontrollerar att den verkligen finns och att den är online.</span><span class="sxs-lookup"><span data-stu-id="a74d3-129">Before you connect to your database, it's a good idea to verify it exists and is online.</span></span>

<span data-ttu-id="a74d3-130">Här kan du öppna Cloud Shell och använder `az`-verktyget för att visa en lista över dina databaser och information om databasen **Logistics** (Logistik), inklusive maximal storlek och status.</span><span class="sxs-lookup"><span data-stu-id="a74d3-130">Here, you bring up Cloud Shell and use the `az` utility to list your databases and show some information about the **Logistics** database, including its maximum size and status.</span></span>

1. <span data-ttu-id="a74d3-131">Gå till portalen och klicka på **Cloud Shell** längst upp.</span><span class="sxs-lookup"><span data-stu-id="a74d3-131">From the portal, at the top, click **Cloud Shell**.</span></span> 
    <span data-ttu-id="a74d3-132">![Öppna Cloud Shell](../media-draft/open-cloud-shell.png)</span><span class="sxs-lookup"><span data-stu-id="a74d3-132">![Opening Cloud Shell](../media-draft/open-cloud-shell.png)</span></span>
1. <span data-ttu-id="a74d3-133">`az`-kommandona som du kör behöver tillgång till namnet på resursgruppen och namnet på den logiska Azure SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="a74d3-133">The `az` commands you'll run require the name of your resource group and the name of your Azure SQL logical server.</span></span> <span data-ttu-id="a74d3-134">Om du vill slippa skriva själv kan du köra `azure configure`-kommandot för att ange dem som standardvärden.</span><span class="sxs-lookup"><span data-stu-id="a74d3-134">To save typing, run this `azure configure` command to specify them as default values.</span></span>
    <span data-ttu-id="a74d3-135">Ersätt `contoso-logistics` med namnet på den logiska Azure SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="a74d3-135">Replace `contoso-logistics` with the name of your Azure SQL logical server.</span></span>
    ```azurecli
    az configure --defaults group=logistics-db-rg sql-server=contoso-logistics
    ```
1. <span data-ttu-id="a74d3-136">Kör `az sql db list` för att lista alla databaser på den logiska Azure SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="a74d3-136">Run `az sql db list` to list all databases on your Azure SQL logical server.</span></span>
    ```azurecli
    az sql db list
    ```
    <span data-ttu-id="a74d3-137">Du ser ett stort JSON-block som utdata.</span><span class="sxs-lookup"><span data-stu-id="a74d3-137">You see a large block of JSON as output.</span></span> 
1. <span data-ttu-id="a74d3-138">Eftersom vi bara vill ha databasnamnen kör du kommandot en gång till.</span><span class="sxs-lookup"><span data-stu-id="a74d3-138">Since we want just the database names, run the command a second time.</span></span> <span data-ttu-id="a74d3-139">Den här gången styr du utdata till `jq` för att endast skriva ut namnfälten.</span><span class="sxs-lookup"><span data-stu-id="a74d3-139">This time, pipe the output to `jq` to print out only the name fields.</span></span>
    ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    <span data-ttu-id="a74d3-140">Du ser det här.</span><span class="sxs-lookup"><span data-stu-id="a74d3-140">You see this.</span></span>
    ```console
    [
      {
        "name": "Logistics"
      },
      {
        "name": "master"
      }
    ]
    ```
    <span data-ttu-id="a74d3-141">Din databas heter **Logistics** (Logistik).</span><span class="sxs-lookup"><span data-stu-id="a74d3-141">**Logistics** is your database.</span></span> <span data-ttu-id="a74d3-142">Precis som på SQL Server innehåller **master** metadata för servern som till exempel inloggningskonton och konfigurationsinställningar för systemet.</span><span class="sxs-lookup"><span data-stu-id="a74d3-142">Like SQL Server, **master** includes server metadata such as logon accounts and system configuration settings.</span></span>
1. <span data-ttu-id="a74d3-143">Kör `az sql db show`-kommandot för att få information om databasen **Logistics** (Logistik).</span><span class="sxs-lookup"><span data-stu-id="a74d3-143">Run this `az sql db show` command to get details about the **Logistics** database.</span></span> 
    ```azurecli
    az sql db show --name Logistics
    ```
    <span data-ttu-id="a74d3-144">Precis som tidigare ser du ett stort JSON-block som utdata.</span><span class="sxs-lookup"><span data-stu-id="a74d3-144">As before, you see a large block of JSON as output.</span></span>
1. <span data-ttu-id="a74d3-145">Kör kommandot en gång till.</span><span class="sxs-lookup"><span data-stu-id="a74d3-145">Run the command a second time.</span></span> <span data-ttu-id="a74d3-146">Den här gången styr du utdata till `jq` för att begränsa utdata till endast namn, maximal storlek och status för databasen **Logistics** (Logistik).</span><span class="sxs-lookup"><span data-stu-id="a74d3-146">This time, pipe the output to `jq` to limit output to only the name, maximum size, and status of the **Logistics** database.</span></span>
    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```
    <span data-ttu-id="a74d3-147">Du ser nu att databasen är online och rymmer cirka 2 GB data.</span><span class="sxs-lookup"><span data-stu-id="a74d3-147">You see that the database is online and can hold around 2 GB of data.</span></span>
    ```console
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a><span data-ttu-id="a74d3-148">Ansluta till databasen</span><span class="sxs-lookup"><span data-stu-id="a74d3-148">Connect to your database</span></span>

<span data-ttu-id="a74d3-149">Nu när du vet lite mer om databasen ska vi prova att ansluta till den med hjälp av `sqlcmd`, skapa en tabell med information om chaufförer och utföra några grundläggande CRUD-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a74d3-149">Now that you understand a bit about your database, let's connect to it using `sqlcmd`, create a table that holds information about transportation drivers, and perform a few basic CRUD operations.</span></span>

<span data-ttu-id="a74d3-150">CRUD handlar om att **skapa**, **läsa**, **uppdatera** och **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a74d3-150">Remember that CRUD stands for **create**, **read**, **update**, and **delete**.</span></span> <span data-ttu-id="a74d3-151">De här åtgärderna kan användas tillsammans med tabelldata och utgör de fyra grundläggande funktionerna som du behöver för din app.</span><span class="sxs-lookup"><span data-stu-id="a74d3-151">These refer to operations you perform on table data, and are the four basic operations you need for your app.</span></span> <span data-ttu-id="a74d3-152">Nu är det dags att kontrollera att du kan utföra var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="a74d3-152">Now's a good time to verify you can perform each of them.</span></span>

1. <span data-ttu-id="a74d3-153">Kör `az sql db show-connection-string`-kommandot för att hämta anslutningssträngen till databasen **Logistics** (Logistik) i ett format som `sqlcmd` kan använda.</span><span class="sxs-lookup"><span data-stu-id="a74d3-153">Run this `az sql db show-connection-string` command to get the connection string to the **Logistics** database in a format that `sqlcmd` can use.</span></span>
    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```
    <span data-ttu-id="a74d3-154">Du ser utdata av den här typen.</span><span class="sxs-lookup"><span data-stu-id="a74d3-154">Your output resembles this.</span></span>
    ```console
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```
1. <span data-ttu-id="a74d3-155">Kör `sqlcmd`-uttrycket från föregående steg för att skapa en interaktiv session.</span><span class="sxs-lookup"><span data-stu-id="a74d3-155">Run the `sqlcmd` statement from the previous step to create an interactive session.</span></span>
    <span data-ttu-id="a74d3-156">Ta bort de omgivande citattecknen och ersätt `<username>` och `<password>` med användarnamnet och lösenordet som du angav när du skapade databasen.</span><span class="sxs-lookup"><span data-stu-id="a74d3-156">Remove the surrounding quotes and replace `<username>` and `<password>` with the username and password you specified when you created your database.</span></span> <span data-ttu-id="a74d3-157">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="a74d3-157">Here's an example.</span></span>
    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```
    > [!TIP]
    > <span data-ttu-id="a74d3-158">Placera ditt lösenord inom citattecken så att ”&” och andra specialtecken inte tolkas som bearbetningsinstruktioner.</span><span class="sxs-lookup"><span data-stu-id="a74d3-158">Place your password in quotes so that "&" and other special characters aren't interpreted as processing instructions.</span></span>
1. <span data-ttu-id="a74d3-159">Skapa en tabell med namnet `Drivers` från `sqlcmd`-sessionen.</span><span class="sxs-lookup"><span data-stu-id="a74d3-159">From your `sqlcmd` session, create a table named `Drivers`.</span></span>
    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255) );
    GO
    ```
    <span data-ttu-id="a74d3-160">Tabellen har fyra kolumner: ett unikt ID, chaufförens för- och efternamn samt vilken ort chauffören är stationerad på.</span><span class="sxs-lookup"><span data-stu-id="a74d3-160">The table contains four columns: a unique identifier, the driver's last and first name, and the driver's city of origin.</span></span>
    > [!NOTE]
    > <span data-ttu-id="a74d3-161">Språket som visas här är Transact-SQL eller T-SQL.</span><span class="sxs-lookup"><span data-stu-id="a74d3-161">The language you see here is Transact-SQL, or T-SQL.</span></span>
1. <span data-ttu-id="a74d3-162">Kontrollera att tabellen `Drivers` finns.</span><span class="sxs-lookup"><span data-stu-id="a74d3-162">Verify that the `Drivers` table exists.</span></span>
    ```sql
    SELECT name FROM sys.tables;
    GO
    ```
    <span data-ttu-id="a74d3-163">Du ser det här.</span><span class="sxs-lookup"><span data-stu-id="a74d3-163">You see this.</span></span>
    ```console
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers
    
    (1 rows affected)
    ```
1. <span data-ttu-id="a74d3-164">Kör T-SQL-uttrycket `INSERT` för att lägga till en exempelrad i tabellen.</span><span class="sxs-lookup"><span data-stu-id="a74d3-164">Run this `INSERT` T-SQL statement to add a sample row to the table.</span></span> <span data-ttu-id="a74d3-165">Det här är åtgärden **create** (skapa).</span><span class="sxs-lookup"><span data-stu-id="a74d3-165">This is the **create** operation.</span></span>
    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Orton', 'Erick', 'Springfield');
    GO
    ```
    <span data-ttu-id="a74d3-166">Detta är en indikation på att åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="a74d3-166">You see this to indicate the operation succeeded.</span></span>
    ```console
    (1 rows affected)
    ```
1. <span data-ttu-id="a74d3-167">Kör T-SQL-uttrycket `SELECT` för att visa en lista med `DriverID`-kolumner från alla rader i tabellen.</span><span class="sxs-lookup"><span data-stu-id="a74d3-167">Run this `SELECT` T-SQL statement list the `DriverID` column from all rows in the table.</span></span> <span data-ttu-id="a74d3-168">Det här är åtgärden **read** (läsa).</span><span class="sxs-lookup"><span data-stu-id="a74d3-168">This is the **read** operation.</span></span>
    ```sql
    SELECT DriverID FROM Drivers;
    GO
    ```
    <span data-ttu-id="a74d3-169">Du ser ett resultat, `DriverID` för den rad som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="a74d3-169">You see one result, the `DriverID` for the row you created in the previous step.</span></span>
    ```console
    DriverID
    -----------
            123
    
    (1 rows affected)
    ```
1. <span data-ttu-id="a74d3-170">Kör T-SQL-uttrycket `UPDATE` för att ändra byta stationeringsort från ”Springfield” till ”Springfield, OR” för chauffören med `DriverID` 123.</span><span class="sxs-lookup"><span data-stu-id="a74d3-170">Run this `UPDATE` T-SQL statement to change the city of origin from "Springfield" to "Springfield, OR" for the driver with a `DriverID` of 123.</span></span> <span data-ttu-id="a74d3-171">Det här är åtgärden **update** (uppdatera).</span><span class="sxs-lookup"><span data-stu-id="a74d3-171">This is the **update** operation.</span></span>
    ```sql
    UPDATE Drivers SET OriginCity='Springfield, OR' WHERE DriverID=123;
    GO
    ```
    <span data-ttu-id="a74d3-172">Du ser det här.</span><span class="sxs-lookup"><span data-stu-id="a74d3-172">You see this.</span></span>
    ```console
    (1 rows affected)
    ```
1. <span data-ttu-id="a74d3-173">Kör T-SQL-uttrycket `DELETE` för att ta bort posten.</span><span class="sxs-lookup"><span data-stu-id="a74d3-173">Run this `DELETE` T-SQL statement to delete the record.</span></span> <span data-ttu-id="a74d3-174">Det här är åtgärden **delete** (ta bort).</span><span class="sxs-lookup"><span data-stu-id="a74d3-174">This is the **delete** operation.</span></span>
    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```
    
    ```console
    (1 rows affected)
    ```
1. <span data-ttu-id="a74d3-175">Kör T-SQL-uttrycket `SELECT` för att kontrollera att tabellen `Drivers` är tom.</span><span class="sxs-lookup"><span data-stu-id="a74d3-175">Run this `SELECT` T-SQL statement to verify the `Drivers` table is empty.</span></span>
    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```
    <span data-ttu-id="a74d3-176">Du ser att tabellen inte innehåller några rader.</span><span class="sxs-lookup"><span data-stu-id="a74d3-176">You see that the table contains no rows.</span></span>
    ```console
    -----------
              0
    
    (1 rows affected)
    ```

## <a name="summary"></a><span data-ttu-id="a74d3-177">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a74d3-177">Summary</span></span>

<span data-ttu-id="a74d3-178">Nu när du har fått en introduktion till hur man arbetar med Azure SQL Database från Cloud Shell kan du hämta anslutningssträngen för ditt SQL-hanteringsverktyg – oavsett om den hämtas från SQL Server Management Studio, Visual Studio eller något annat.</span><span class="sxs-lookup"><span data-stu-id="a74d3-178">Now that you have the hang of working with Azure SQL Database from Cloud Shell, you can get the connection string for your favorite SQL management tool &ndash; whether that's from SQL Server Management Studio, Visual Studio, or something else.</span></span>

<span data-ttu-id="a74d3-179">Cloud Shell gör det enkelt att komma åt och arbeta med dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a74d3-179">Cloud Shell makes it easy to access and work with your Azure resources.</span></span> <span data-ttu-id="a74d3-180">Eftersom Cloud Shell är ett webbläsarbaserat gränssnitt kan du komma åt det från såväl Windows som macOS eller Linux – i stort sett vilket system som helst, bara det finns en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a74d3-180">Because Cloud Shell is browser-based, you can access it from Windows, macOS, or Linux &ndash; essentially any system with a web browser.</span></span>

<span data-ttu-id="a74d3-181">Nu har du fått lite praktisk erfarenhet av att köra kommandon via Azures kommandoradsgränssnitt och visa information om din SQL Azure Database.</span><span class="sxs-lookup"><span data-stu-id="a74d3-181">You gained some hands-on experience running Azure CLI commands to get information about your SQL Azure Database.</span></span> <span data-ttu-id="a74d3-182">Som överkurs har du också fått öva på att skriva T-SQL.</span><span class="sxs-lookup"><span data-stu-id="a74d3-182">As a bonus, you practiced your T-SQL skills.</span></span>

<span data-ttu-id="a74d3-183">Vi avslutar med att titta närmare på hur man gör för att plocka ned databasen.</span><span class="sxs-lookup"><span data-stu-id="a74d3-183">Finally, let's wrap up and see how to tear down your database.</span></span>