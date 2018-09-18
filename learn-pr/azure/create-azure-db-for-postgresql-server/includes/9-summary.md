I den här modulen har du sett hur Azure Database for PostgreSQL-erbjudandet ser ut. Du såg hur man skapar en Azure Database for PostgreSQL med både Azure Portal och Azure CLI.

Du har tittat på de konfigurationsalternativ som är tillgängliga för att konfigurera en PostgreSQL-brandväggsregel på servernivå och framtvinga SSL-anslutningar. Slutligen såg du hur man ansluter till en Azure Database for PostgreSQL-server med _psql_ i Azure Cloud Shell

## <a name="clean-up"></a>Rensa
<!---TODO: Update for sandbox?--->

Allt som återstår nu är att rensa de resurser som du skapade i labbuppgifterna. Kom ihåg att servern genererar en månatlig avgift även om du inte använder den. Om du skapar resurser i testsyfte kan du enkelt ta bort allihop genom att ta bort den resursgrupp som de ingår i.

> [!NOTE]
> En resursgrupp är ju en samling resurser som delar samma livscykel, behörigheter och principer. Dessa åtgärder tar bort alla resurser som är allokerade i den här resursgruppen, inklusive alla data som du har lagt till i databaserna på den server du skapade.

- Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).

- I den vänstra menyn väljer du **Resursgrupper** för att se alla resursgrupper som för närvarande används på ditt konto.

- Klicka på namnet på den resursgrupp som du skapade och klicka på knappen **Ta bort resursgrupp** i verktygsfältet längst upp.

- Dialogrutan **Är du säker på att du vill ta bort `<resource_group_name>`** visas, där du uppmanas att ange namnet på resursgruppen för att bekräfta borttagningen. Bekräfta resursgruppens namn och klicka på knappen Ta bort längst ned.
