Nu när din Azure SQL-databas är igång kan du ansluta den till ett SQL Server-hanteringsverktyg och fylla den med riktiga data.

Inledningsvis valde du mellan att köra databasen lokalt eller i molnet. Med Azure SQL Database kan du konfigurera några grundläggande alternativ och enkelt få en fullt fungerande SQL-databas som du kan koppla till dina appar.

Ingen infrastruktur att underhålla, inga programkorrigeringar att installera. Nu slipper du lägga tid på databasadministration och kan istället fokusera på att bli klar med prototypen till logistikappen. Se till så att prototypen blir mer än en enkel demo. Azure SQL Database tillhandahåller säkerhets- och prestandafunktioner på produktionsnivå.

Kom ihåg att varje logisk Azure SQL-server innehåller en eller flera databaser. Azure SQL Database har två olika prismodeller, DTU och Virtuell kärna, som gör det enkelt att hitta den rätta balansen mellan kostnad och prestanda för alla dina databaser.

Välj DTU om du precis har kommit igång eller bara vill ha ett enkelt och förkonfigurerat alternativ. Välj Virtuell kärna om du vill ha mer kontroll över vilka beräknings- och lagringsresurser du skapar och betalar för.

Azure Cloud Shell gör det enkelt att börja arbeta med databaser. Från Cloud Shell har du har åtkomst till Azure CLI, ett kommandoradsgränssnitt som tillhandahåller information om dina Azure-resurser. Cloud Shell tillhandahåller även flera vanliga verktyg, till exempel `sqlcmd`, för att du lättare ska komma igång och jobba med den nya databasen.

## <a name="cleanup"></a>Rensa

Passa på att experimentera lite med din Azure SQL Database-installation. När du är klar är det enklaste sättet att ta bort databasen att ta bort dess överordnade resursgrupp.

1. Klicka på **Resursgrupper** i portalen.

1. Välj **logistics-db-rg**.

1. Klicka på **Ta bort resursgrupp**.

    ![Ta bort resursgruppen](../media-draft/delete-rg.png)

1. Skriv ”logistics-db-rg” på kommandoraden och klicka på **Ta bort**.

## <a name="additional-resources"></a>Ytterligare resurser

I dokumentationen hittar du mer utförlig information, inklusive självstudier och exempel. Här följer några länkar till vad du lärt dig här:

- [Dokumentation om Azure SQL Database](https://docs.microsoft.com/azure/sql-database/)
- [Prismodeller och resurser för Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)
- [Logiska Azure SQL Database-servrar och hur de hanteras](https://docs.microsoft.com/azure/sql-database/sql-database-logical-servers)
- [Brandväggsregler för SQL Database och SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)

Läs mer om Cloud Shell i [Översikt över Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

Om du vill veta mer om verktyget `sqlcmd`, se [sqlcmd](https://docs.microsoft.com/sql/tools/sqlcmd-utility?view=sql-server-2017).
