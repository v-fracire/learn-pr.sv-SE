Du har utvärderat din lokala databas för att kontrollera databasens kompatibilitet och funktionsparitet med Azure SQL-databasen. Eftersom kundbasen för din webbaserade cykelbutik är begränsad till en geografisk plats har du råd att ta databasen offline under tider med låg belastning, till exempel under de tidiga morgontimmarna.

Den bästa databasmigreringsmetoden i det här scenariot är att använda Data Migration Assistant, som innehåller en guide som vägleder dig genom migreringsstegen. I den här kursdelen ska vi titta närmare på hur Data Migration Assistant utför själva migreringen.

## <a name="migrate-the-database-using-data-migration-assistant"></a>Migrera databasen med hjälp av Data Migration Assistant

När du använder Data Migration Assistant för att migrera en databas väljer du en tidsperiod då det inte finns någon eller endast minimal transaktionsaktivitet i databasen.

Åtgärda eventuella fel som identifierades under utvärderingen innan du påbörjar migreringen. Utvärderingsrapporterna innehåller information om de korrigeringar som krävs innan du går vidare med själva migreringen.

För att minska den totala migreringstiden kan du ändra prestandanivån för Azure SQL-måldatabasen under migreringsperioden. Du kan öka prestanda till en högre nivå, till exempel P15, för att minska avbrottstiden som orsakas av migreringen. Glöm i så fall inte att återställa prestandanivån till den tidigare nivån efter migreringen för att undvika onödiga kostnader.

Själva migreringsprocessen omfattar följande steg:

1. Skapa en tom Azure SQL Database.

1. Skapa ett nytt migreringsprojekt.

1. Definiera käll- och målservrarna och databaserna.

1. Välja de objekt som ska migreras. Du behöver inte migrera alla objekt, men se till att du inte utelämnar några beroende objekt. Om en vy exempelvis använder en tabell och du migrerar vyn, är det viktigt att du även migrerar tabellen.

1. Distribuera schemat. När du gör det migreras databasens struktur, men inte dess data.

1. Migrera data. När du gör det migreras innehållet i tabellerna i databasen. Det här är det mest tidskrävande steget.
