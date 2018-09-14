Anta att en hackare som försöker få åtkomst till din databas. Program som ansluter till databasen är sårbara punkter för angrepp. Dessa program kan inte ansluta till databasen med säkra metoder.

Databaser behöver sina egna säkerhet, men hur databasen används spelar en viktig roll i datasäkerhet. Databasen överträdelser är vanligtvis resultatet av SQL-inmatningsattacker. SQL-inmatningsattacker är resultatet av program som inte använder prioriterade metoder för att komma åt en databas.

Låt oss titta på tekniker för att skydda din databas på programnivå.

## <a name="sql-injection-attacks"></a>SQL-inmatningsattacker

Den [OWASP foundation](https://owasp.org) är en icke-vinstdrivande organisation som har utformats för att skapa standarder för program som ska vara betrodd. Den publicerar regelbundet en lista över de översta 10 säkerhetsproblem.

De vanligaste sårbarheten enligt OWASP är inmatningsattacker som normalt tar form av SQL-inmatningsattacker. Information som skickas till en SQL-instruktion har ändrats i en SQL-angrepp. Frågorna ändrade antingen returnera känslig information eller utföra skadliga åtgärder på databasen.

Låt oss titta på följande kod för ASP.NET Core med ADO.NET för att hämta samverkan med en viss kund.

```csharp
public List<CustomerInteraction> GetCustomerInteractions(string customerId)
{
    var result = new List<CustomerInteraction>();

    using (var conn = new SqlConnection(ConnectionString))
    {
        var sql = "select Id, CustomerId, InteractionDate, Details " +
            "from CustomerInteractions where CustomerId = '" + customerId + "'";

        using (var command = conn.CreateCommand())
        {
            command.CommandText = sql;
            conn.Open();

            using (var reader = command.ExecuteReader())
            {
                if (reader.HasRows)
                {
                    while (reader.Read())
                        result.Add(GetInteractionsFromReader(reader));
                }
            }
        }
        return result.OrderByDescending(i => i.InteractionDate).ToList();
    }
}
```

Själva frågan är förbereder sig för en SQL-angrepp, som parametern customerId inte är språkoberoende innan du fortsätter i frågan och så kan ändras. Webbplatsen som anropar frågan är skicka parametern customerId på URL-frågesträngen på följande sätt:

.../Home/ViewInteractions?customerId=8c69a607-3c09-45ac-9beb-c59ca2de2385

Problem med den här strategin är att parametern customerId kan ändras. Du kan till exempel ändra parametern customerId för att placera ytterligare information i frågan och välj data från en annan tabell.

```sql
select Id, CustomerId, InteractionDate, Details from CustomerInteractions where CustomerId = '8c69a607-3c09-45ac-9beb-c59ca2de2385'
```

Du kan nu ändra för att lägga till ytterligare information via en UNION-uttrycket för att lägga till ytterligare information, genom att lägga till följande frågan:

```sql
union select Id, Id as CustomerId, getdate() as InteractionDate, CreditCardNumber + '/' + STR(CreditCardExpiryMonth, 2) + '/' + STR(CreditCardExpiryYear, 4) + ' cvv ' + STR(CreditCardCVV, 3) as Details from Customers --
```

SQL-frågan är URL-kodade så att värdet accepteras som en giltig URL-webbdel. URL: en skickas till webbplatsen och ytterligare SQL-frågan köras på databasen.

```sql
%27+union+select+Id%2C+Id+as+CustomerId%2C+getdate%28%29+as+InteractionDate%2C+CreditCardNumber+%2B+%27%2F%27+%2B+STR%28CreditCardExpiryMonth%2C+2%29+%2B+%27%2F%27+%2B+STR%28CreditCardExpiryYear%2C+4%29+%2B+%27+cvv+%27+%2B+STR%28CreditCardCVV%2C+3%29+as+Details+from+Customers+--
```

Det här exemplet visar hur inte sanitizing information från platsen kan leda till problem med säkerhet.

![Skärmbild av en web browser adressfältet som visar ett exempel på en SQL-inmatning angreppsförsök via en webbapp.](../media-draft/4-view-web-page-after-sql-injection.png)

> [!Note]
> Om du vill veta mer om inmatningsattacker Besök den [OWASP Foundation](https://www.owasp.org/).

## <a name="avoiding-sql-injection-attacks"></a>Undvika SQL-inmatningsattacker.

Undvik att SQL-inmatningsattacker, bör du alltid kontrollera att alla användarens indata är språkoberoende. Användarindata som används som parametrar för frågor bör inte skapats via strängsammanfogning, men som faktiska Frågeparametrar.

I följande exempel ser du hur CustomerId nu skickas som en parameter med hjälp av den @-tecknet. Parametrar definieras uttryckligen och värden har skickats i frågan.

```csharp
using (var command = conn.CreateCommand())
{
    var sql = "select Id, CustomerId, InteractionDate, Details " +
        "from CustomerInteractions where CustomerId = @CustomerId order by InteractionDate";

    command.CommandText = sql;
    var prmCustomerId = command.Parameters.Add("@CustomerId", SqlDbType.UniqueIdentifier);
    prmCustomerId.Value = Guid.Parse(customerId);

    conn.Open();

    using (var reader = command.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
                result.Add(GetInteractionsFromReader(reader));
        }
    }
}
```

Core-rader med kod är:

```csharp
    var prmCustomerId = command.Parameters.Add("@CustomerId", SqlDbType.UniqueIdentifier);
    prmCustomerId.Value = Guid.Parse(customerId);
```

Med språkoberoende datainmatning och parameterfrågor minskar risken för SQL-inmatningsattacker i databasen.

Vårt exempel används ASP.NET Core för att illustrera begreppen. Tänk dock på att alla programmeringsspråk system som har stöd för åtkomst till SQL Server har mekanismer för att skicka parametrar till värden för frågor.

## <a name="dynamic-data-masking"></a>Dynamisk datamaskning

Du kanske har märkt att del av informationen i databasen är särskilt känsliga; kanske kreditkortsinformation. I ett verkligt system, skulle du aldrig lagra kreditkortsinformation okrypterade. Tyvärr kreditkortsinformation är inte krypterad och du behöver hitta ett annat sätt att dölja data.

Anta att du skapar en perioder webbplats och under beställningsprocessen platsen måste Visa kreditkortsinformation. Du vill visa de första 12 siffrorna spärrad från användare, visas t.ex. xxxx-xxxx-xxxx-1234.

Den här metoden kallas dynamisk datamaskning. Dynamisk datamaskning kan du lägga till masker mot kolumner i din databas.

1. Med portalen kan du välja den databas du vill tillämpa masker till och välj sedan den **dynamisk Datamaskning** alternativet från den **Security** inställningen.

    Maskning regler skärmen visar en lista över befintliga dynamiska data masker och rekommendationer för kolumner som ska ha en mask för dynamiska data som tillämpas.

    ![Skärmbild av Azure-portalen som visar en lista över rekommenderade masker för de olika databasen kolumner i en exempeldatabas.](../media-draft/4-view-recommended-masked-columns.png)

1. Om du vill lägga till en mask till en kolumn klickar du på den **Lägg till mask** för att lägga till rekommenderade masken till kolumnen.

    ![Skärmbild av Azure-portalen med rekommenderade maskerade kolumner tillämpas och mask funktioner som används.](../media-draft/4-recommended-masks-applied.png)

1. Varje ny mask läggs till i Maskning regellistan. Klicka på den **spara** för att tillämpa masker.

När du frågar efter kolumnerna databasadministratörer fortfarande finns i de ursprungliga värdena, men icke-administratörer ser maskerade värdena.

Du kan låta andra användare att se versionerna som inte är maskeras genom att lägga till dem i SQL-användare uteslutna från maskering av listan.

Du kan se hur maskerade data ser ut när efterfrågas av en icke-administratör.

![Skärmbild av en databasfråga som visar e-post, telefonnummer, SocialSecurityNumber och CreditCardNumber resultera kolumner som är dolda när de skulle ses av en icke-administratör.](../media-draft/4-sql-query-showing-masks.png)

Masker på visas är de som har lagts till baserat på rekommendationer, men du kan lägga till en mask manuellt för. Välj den + Lägg till mask knappen och sedan pop över gör det möjligt att välja schemat, tabellen och den kolumn som ska användas. Sedan definierar du maskeringen som används. Det finns standard masker som kan användas som:

- Standardvärdet, vilket visar standardvärdet för den datatypen i stället;
- Kreditkortvärde, som visar bara de sista fyra siffrorna i numret, konverterar alla andra tal till gemener kryss;
- E-post, som döljer domänen namnet och alla utom det första tecknet i detta e-kontonamnet.
- Nummer, som anger ett slumptal mellan ett intervall med värden. Till exempel på kreditkort förfallodatum månaden och året, kan du markera slumpmässiga månader från 1 till 12 och ange årsintervallet från 2018 till 3000; eller
- Anpassad sträng. På så sätt kan du ange antalet tecken som visas från början av data, hur många tecken som visas från slutet av data och tecknen ska upprepas under resten av data.
