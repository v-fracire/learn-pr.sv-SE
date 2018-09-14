Azure tillhandahåller PaaS-tjänster som du kan använda för att hantera alla typer av data, från strukturerade relationsdata till ostrukturerade data.

Här kan du läsa mer om varför Azure SQL Database ger dig ett praktiskt, kostnadseffektivt och säkert sätt att agera värd för relationsdatabaser.

## <a name="why-choose-azure-sql-database"></a>Men varför ska man välja Azure SQL Database?

Säg att du har en logistikapp som behöver tillgång till några lagrade CRUD-åtgärder (**skapa**, **läsa**, **uppdatera** och **ta bort**). Du har erfarenhet av att arbeta med SQL Server och andra relationsdatabaser.

Du har två alternativ när det gäller databasen:

1. Du kan vara värd för SQL Server lokalt. IT-avdelningen driver ett litet lokalt datacenter som tillhandahåller tjänster åt ekonomiavdelningen och några andra avdelningar på företaget. Samarbeta med IT-avdelningen och låt datacentret vara värd för en SQL Server-distribution.

1. Du kan vara värd för Azure SQL Database i molnet. Azure SQL Database bygger på SQL Server och tillhandahåller de relationsdatabasfunktioner som du behöver.

Du har bestämt dig för att skapa webb- och appnivåer för din logistikapp hos Azure. Då är det också klokt att låta Azure vara värd för databasen. Det finns även andra anledningar till varför Azure SQL Database är ett smart val och varför det till och med är enklare att använda än virtuella datorer.

- **Praktiskt**

    När du ska konfigurera SQL Server på en virtuell dator eller på någon annan fysisk maskinvara måste du först ta reda på maskin- och programvarukraven. Du måste ha kunskap om de senaste regelverken och rekommendationerna för säkerhet samt kunna hantera operativsystem och SQL Server-korrigeringar som en del av rutinunderhållet. Du måste också kunna sköta säkerhetskopiering och tackla eventuella problem med datakvarhållning själv.

    När du väljer Azure SQL Database tar vi hand om hanteringen av maskinvaran, uppdateringen av programvaran och installationen av korrigeringsfiler för operativsystemet. Allt du behöver göra är att ange namnet på din databas och ett par andra alternativ. På bara några minuter har du en aktiv och fungerande SQL-databas.

    Du kan enkelt skala upp och ned antalet SQL Database-instanser vid behov. Azure SQL Database går snabbt att skala upp och är enkel att konfigurera. Du slipper tänka på konfigurationen och kan istället lägga mer krut på att bygga en helt fantastisk app.

- **Kostnad**

    Eftersom vi sköter allt åt dig behöver du inte köpa, driva eller underhålla några egna system.

    Azure SQL Database har flera olika prisnivåer. Använd de olika prisnivåerna för att hitta rätt balans mellan prestanda och kostnad. Börja med en liten summa varje månad.

- **Skalning**

    Du upptäcker att mängden logistikdata som måste du lagra fördubblas för varje år som går. Hur mycket extra kapacitet ska du planera för när du kör lokalt?

    Om du använder Azure SQL Database kan du enkelt justera prestanda och storleken på databasen i takt med att dina behov skiftar.

- **Säkerhet**

    Azure SQL Database levereras med en brandvägg som konfigureras automatiskt för att begränsa antalet anslutningar från Internet.

    Du kan sätta upp IP-adresser som du har förtroende för på en lista med tillåtna användare. Om du använder en sådan lista blir det lättare att använda Visual Studio, SQL Server Management Studio eller andra verktyg för att hantera din Azure SQL-databas.

## <a name="summary"></a>Sammanfattning

När du väljer Azure SQL Database tar vi hand om hanteringen av maskinvaran, uppdateringen av programvaran och installationen av korrigeringsfiler för operativsystemet. Vi har en rad olika prisnivåer så att du får den prestanda du behöver till en förutsägbar kostnad. Azure SQL Database levereras också med en brandvägg så att du kan styra över åtkomsten till dina data.

Du inte behöver vara någon erfaren databasadministratör för att kunna använda Azure SQL Database, men det finns ändå några begrepp som du bör känna till innan du börjar. Härnäst går vi igenom just dessa begrepp.
