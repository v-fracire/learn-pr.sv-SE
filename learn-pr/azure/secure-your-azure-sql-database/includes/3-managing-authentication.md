Anta att du kör en webbplats och angriparen fick åtkomst till din databas. Angriparen fick åtkomst till ett dbo-konto på attacken och kan utföra alla åtgärder i databasen. Ett dbo-konto kan komma åt och ändra information, till exempel metadata och utföra alla åtkomstnivåer.

Om angriparen fick bara tillgång till ett vanligt användarkonto, sedan de kan bara komma åt tabeller, vyer, lagrade procedurer och andra objekt som definierats för det användarkontot. Till exempel om en webbplats som har åtkomst till en databas har komprometterats på grund av ett SQL-angrepp, skulle sedan angriparen vara begränsad i vad de kunde göra.

Om ett intrång inträffar kan det hjälper dig för att minska effekten av överträdelser. Nu ska vi titta på hur du begränsar åtkomsten till SQL Azure-databas på nivån för användaren.

## <a name="reduce-the-attack-surface-of-the-database"></a>Minska risken för angrepp på databasen

För att minska effekten av någon säkerhetsbrist kan begränsa du ytan på attacken. Om du ansluter till din databas med ett dbo eller administratör-konto och en angripare får tillgång till databasen, angriparen åtkomst för att utföra alla åtgärder i databasen. Åtkomst kan omfatta fråga databasmetadata, avgör vilka data som är tillgängliga och/eller känsliga och utnyttja den här informationen.

Du kan undvika denna säkerhetsrisk genom att skapa databasanvändare som har begränsade behörigheter. Vi använder termen lägsta behörighet här, eftersom användarna bara har åtkomst till tabeller, vyer, lagrade procedurer och andra entiteter som behövs för att utföra sitt arbete.

## <a name="create-a-database-user"></a>Skapa en databasanvändare

Om du vill skapa en användare med lägre behörighet du skapa en databasanvändare och sedan koppla användaren till databasen. Nu ska vi skapa en SQL Server-användare och ge användarbehörighet till databasen.

> [!Note]
> Det finns 13 (13) typer av användare i SQL Server. Om du vill skapa en annan typ av databasanvändare, kan du använda länken för att ta reda på rätt syntax. Se [skapa en databasanvändare](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/create-a-database-user?view=sql-server-2017).

Prioriterade metoden är att skapa användaren på en databasnivå. Det här steget förhindrar användarbehörigheter går utanför gränserna för den databas som du vill skydda.

1. Börja med att välja databasen.
2. Skapa sedan den användare som använder du följande fråga:

   ```sql
   CREATE USER MyWebAppUser WITH PASSWORD = '<YourSuperStrongPassword>';
   ```

   Den föregående frågan skapar användaren *MyWebUser*. Användaren har inte åtkomst till alla tabeller, vyer och lagrade procedurer som standard.

3. Skapa nu rätt behörighet för användaren genom att lägga till dem roller som db_datareader och db_datawriter.

   Rollen db_datareader kan användaråtkomst till alla användartabeller och vyer i databasen. På samma sätt kommer rollen db_datawriter kan användaråtkomst till läsa, skriva, uppdatera och ta bort rader i databasen.

   ```sql
   ALTER ROLE db_datareader ADD MEMBER MyWebAppUser;
   ALTER ROLE db_datawriter ADD MEMBER MyWebAppUser;
   ```

Du kan neka en användares åtkomst till andra element i databasen med NEKA-operator. Du här neka användaren MyWebAppUser möjlighet att välja data från tabellen kunder.

```sql
DENY SELECT ON Customers TO MyWebAppUser;
```

Du kan också använda ge behörighet att uttryckligen ge behörigheter till en användare eller roll.

```sql
GRANT SELECT ON Customers TO MyWebAppUser;
```

Du kommer att fortsätta att förfina åtgärder i databasen för att få användaren att åtkomstnivån som behövs. I stället för en användare, kan du skapa en roll med de lägsta behörigheten som krävs och sedan lägga till användaren till rollen.

Om du har flera användare som har samma behörigheter, kan du skapa de användarna som en del av rollen. Användaren har endast åtkomst till saker som de behöver, när alla åtkomstbehörigheter har beviljats eller nekats.
Om en angripare får åtkomst till databasen via den nyligen skapade användaren, kommer de endast se och köra samma data och åtgärder som användaren kan. Låsa användaråtkomst avsevärt minskar ytan på attack på databasen.
