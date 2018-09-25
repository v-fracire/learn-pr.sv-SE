Innan du ansluter databasen till din app måste du kontrollera att du kan ansluta till den, lägga till en enkel tabell och arbeta med exempeldata.

Vi står för infrastrukturen, uppdateringar och korrigeringar till din Azure SQL-databas. Utöver det kan du hantera din Azure SQL-databas som vilken SQL Server-installation som helst. Du kan till exempel använda Visual Studio, SQL Server Management Studio, SQL Server Operations Studio eller andra verktyg för att hantera din Azure SQL-databas.

Hur du söker åtkomst till din databas och ansluter den till din app är upp till dig. Men för att få lite mer erfarenhet av att arbeta med databasen har du här möjlighet att prova på att ansluta den direkt från portalen, skapa en tabell och köra några grundläggande CRUD-åtgärder. Du får lära dig detta:

- Vad Cloud Shell är och hur du kommer åt detta från portalen.
- Hur du hittar information om din databas med Azures kommandoradsgränssnitt (CLI), inklusive anslutningssträngar.
- Hur du ansluter till databasen med `sqlcmd`.
- Hur du initierar databasen med en enkel tabell och exempeldata.

## <a name="what-is-azure-cloud-shell"></a>Vad är Azure Cloud Shell?

Azure Cloud Shell är ett webbläsarbaserat gränssnitt som gör det möjligt att hantera och utveckla Azure-resurser. Man kan säga att Cloud Shell är en interaktiv konsol som körs i molnet.

Cloud Shell körs egentligen på Linux i bakgrunden. Beroende på om du föredrar en Linux- eller en Windows-miljö kan du välja mellan två olika versioner: Bash och PowerShell.

Cloud Shell kan användas var du än befinner dig. Förutom på portalen kan du också använda Cloud Shell via [shell.azure.com](https://shell.azure.com/), Azure-mobilappen eller Visual Studio Code. Panelen till höger är en Cloud Shell-terminal som du kan använda under den här övningen.

Cloud Shell innehåller flera populära verktyg och redigeringsprogram. Här följer en snabb titt på `az`, `jq` och `sqlcmd` – tre verktyg som du behöver för den aktuella uppgiften.

- `az` är även känt som Azure CLI. Det är kommandoradsgränssnittet som används när du arbetar med Azure-resurser. Används för att hämta information om din databas, inklusive anslutningssträngen.
- `jq` är en JSON-parser för kommandoraden. Du kan styra utdata från `az`-kommandon till det här verktyget för att extrahera viktiga fält från JSON-utdata.
- Med `sqlcmd` kan du köra instruktioner på SQL Server. Du kommer att använda `sqlcmd` för att skapa en interaktiv session med din Azure SQL-databas.

## <a name="get-information-about-your-azure-sql-database"></a>Hämta information om din Azure SQL-databas

Innan du ansluter till databasen är det en bra om du kontrollerar att den verkligen finns och att den är online.

Här kan du använda `az`-verktyget för att visa en lista över dina databaser och information om databasen **Logistics** (Logistik), inklusive maximal storlek och status.

1. `az`-kommandona som du kör behöver tillgång till namnet på resursgruppen och namnet på den logiska Azure SQL-servern. Om du vill slippa skriva själv kan du köra `azure configure`-kommandot för att ange dem som standardvärden.
    Ersätt `<server-name>` med namnet på den logiska Azure SQL-servern. Observera att beroende på det blad du är på i portalen så kan det här visas som ett fullständigt domännamn (servername.database.windows.net), men du behöver bara det logiska namnet utan suffixet .database.windows.net.

    ```azurecli
    az configure --defaults group=<rgn>[sandbox resource group name]</rgn> sql-server=<server-name>
    ```

1. Kör `az sql db list` för att lista alla databaser på den logiska Azure SQL-servern.

    ```azurecli
    az sql db list
    ```
    Du ser ett stort JSON-block som utdata.

1. Eftersom vi bara vill ha databasnamnen kör du kommandot en gång till. Den här gången styr du utdata till `jq` för att endast skriva ut namnfälten.
   
     ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    
    Du bör se dessa utdata.
    
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

    Din databas heter **Logistics**. Precis som på SQL Server innehåller **master** metadata för servern som till exempel inloggningskonton och konfigurationsinställningar för systemet.

1. Kör `az sql db show`-kommandot för att få information om databasen **Logistics** (Logistik).

    ```azurecli
    az sql db show --name Logistics
    ```

    Precis som tidigare ser du ett stort JSON-block som utdata.

1. Kör kommandot en gång till. Den här gången styr du utdata till `jq` för att begränsa utdata till endast namn, maximal storlek och status för databasen **Logistics** (Logistik).

    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```

    Du ser nu att databasen är online och rymmer cirka 2 GB data.

    ```json
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a>Ansluta till databasen

Nu när du vet lite mer om databasen ska vi prova att ansluta till den med hjälp av `sqlcmd`, skapa en tabell med information om chaufförer och utföra några grundläggande CRUD-åtgärder.

CRUD handlar om att **skapa**, **läsa**, **uppdatera** och **ta bort**. De här villkoren avser åtgärder som du kan använda tillsammans med tabelldata och som utgör de fyra grundläggande funktioner som du behöver för din app. Nu är det dags att kontrollera att du kan utföra var och en av dem.

1. Kör `az sql db show-connection-string`-kommandot för att hämta anslutningssträngen till databasen **Logistics** (Logistik) i ett format som `sqlcmd` kan använda.

    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```

    Du ser utdata av den här typen.

    ```output
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```

1. Kör `sqlcmd`-uttrycket från föregående stegs utdata för att skapa en interaktiv session. Ta bort de omgivande citattecknen och ersätt `<username>` och `<password>` med användarnamnet och lösenordet som du angav när du skapade databasen. Här är ett exempel.

    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```

    > [!TIP]
    > Placera ditt lösenord inom citattecken så att ”&” och andra specialtecken inte tolkas som bearbetningsinstruktioner.

1. Skapa en tabell med namnet `Drivers` från `sqlcmd`-sessionen.

    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255));
    GO
    ```

    Tabellen har fyra kolumner: ett unikt ID, chaufförens för- och efternamn samt vilken ort chauffören är stationerad på.

    > [!NOTE]
    > Språket som visas här är Transact-SQL eller T-SQL.

1. Kontrollera att tabellen `Drivers` finns.

    ```sql
    SELECT name FROM sys.tables;
    GO
    ```

    Du bör se dessa utdata.

    ```output
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers

    (1 rows affected)
    ```

1. Kör T-SQL-uttrycket `INSERT` för att lägga till en exempelrad i tabellen. Det här är åtgärden **create** (skapa).

    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Zirne', 'Laura', 'Springfield');
    GO
    ```

    Detta är en indikation på att åtgärden lyckades.

    ```output
    (1 rows affected)
    ```

1. Kör det här T-SQL-uttrycket `SELECT` för att lista `DriverID`- och `OriginCity`-kolumnerna från alla rader i tabellen. Det här är åtgärden **read** (läsa).

    ```sql
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    Du ser ett resultat med `DriverID` och `OriginCity` för den rad som du skapade i föregående steg.

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Springfield

    (1 rows affected)
    ```

1. Kör det här T-SQL-uttrycket `UPDATE` för att ändra ursprungsstad från Springfield till Boston för chauffören med `DriverID` 123. Det här är **update**-åtgärden.

    ```sql
    UPDATE Drivers SET OriginCity='Boston' WHERE DriverID=123;
    GO
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    Du bör se följande utdata. Observera hur `OriginCity` återspeglar Boston-uppdateringen.

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Boston

    (1 rows affected)
    ```

1. Kör T-SQL-uttrycket `DELETE` för att ta bort posten. Det här är åtgärden **delete** (ta bort).

    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```

    ```output
    (1 rows affected)
    ```

1. Kör T-SQL-uttrycket `SELECT` för att kontrollera att tabellen `Drivers` är tom.

    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```

    Du ser att tabellen inte innehåller några rader.

    ```output
    -----------
              0

    (1 rows affected)
    ```

Nu när du har fått en introduktion till hur man arbetar med Azure SQL Database från Cloud Shell kan du hämta anslutningssträngen för ditt SQL-hanteringsverktyg &ndash; oavsett om den hämtas från SQL Server Management Studio, Visual Studio eller något annat.

Cloud Shell gör det enkelt att komma åt och arbeta med dina Azure-resurser. Eftersom Cloud Shell är ett webbläsarbaserat gränssnitt kan du komma åt det från såväl Windows som macOS eller Linux – i stort sett vilket system som helst, bara det finns en webbläsare.

Nu har du fått lite praktisk erfarenhet av att köra kommandon via Azures kommandoradsgränssnitt och visa information om din Azure SQL-databas. Som överkurs har du också fått öva på att skriva T-SQL.

Vi avslutar med att titta närmare på hur man gör för att plocka ned databasen.
