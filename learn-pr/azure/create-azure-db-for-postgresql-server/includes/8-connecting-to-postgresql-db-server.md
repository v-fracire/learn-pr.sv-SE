Anta att du använder en lokal PostgreSQL-databas. Du hanterar alla säkerhetsaspekter och har låst all åtkomst till dina servrar med brandväggsregler på standardservernivå i PostgreSQL. Nu har du goda kunskaper om hur du konfigurerar brandväggsregler på samma servernivå i Azure.

Vi ska ansluta till en av de Azure Database for PostgreSQL-servrar som du skapade.

## <a name="allow-azure-service-access"></a>Tillåta åtkomst till Azure-tjänster

Innan vi börjar måste du tillåta åtkomst till Azure-tjänster, för att kunna använda PowerShell och `psql` för att kunna ansluta till din server. Kom ihåg att du kan tillåta åtkomst på två sätt.

Ditt första alternativ är att aktivera **Allow access to Azure services** (Tillåt åtkomst till Azure-tjänster). Om du tillåter åtkomst skapas en brandväggsregel även om regeln inte finns med på listan över anpassade regler som du skapar.

Ditt andra alternativ är att skapa en brandväggsregel som tillåter åtkomst till alla IP-adresser. Regeln innehåller IP-adressen till klienten för det PowerShell som du använder till att köra `psql` från.

Du måste också inaktivera alternativet **Framtvinga SSL-anslutning**.

### <a name="create-a-firewall-rule"></a>Skapa en brandväggsregel

1. Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du aktiverade sandbox-miljön.

1. Gå till serverresursen du vill skapa en brandväggsregel för.

1. Välj alternativet **Anslutningssäkerhet** för att öppna bladet Anslutningssäkerhet till höger.

    ![Skärmbild av Azure Portal som visar avsnittet Anslutningssäkerhet på resursbladet för PostgreSQL-databasen](../media/7-db-security-settings.png)

Kom ihåg att du vill tillåta åtkomst till PowerShell-klienter som kör `psql`.

Du kan välja att antingen:

- Ställ in **Allow access to Azure services** (Tillåt åtkomst till Azure-tjänster) på **PÅ**
- Ställ in **Framtvinga SSL-anslutning** på **INAKTIVERAD**
- Klicka på knappen **Spara** för att spara ändringarna

Eller så kan du lägga till en brandväggsregel för att tillåta åtkomst till alla IP-adresser genom att lägga till en brandväggsregel. Ange följande värden:

- Regelnamn: `AllowAll`
- Start-IP: `0.0.0.0`
- Slut-IP: `255.255.255.255`
- Ställ in **Framtvinga SSL-anslutning** på **INAKTIVERAD**
- Klicka på knappen **Spara** för att spara ändringarna

> [!Warning]
> Om du skapar den här brandväggsregeln får alla IP-adresser på Internet tillåtelse att försöka ansluta till din server. I produktionsmiljöer vill du begränsa åtkomsten till specifika IP-adresser.

### <a name="connect-to-the-database-with-psql"></a>Ansluta till databasen med psql

1. Anslut PSQL till servern med följande kommando i Azure Cloud Shell till höger. Byt ut servernamnet och administratörsnamnet.

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
    ```

    Använd de värden som du valde för `server-name` och `admin-user`.

1. **postgres** är standarddatabasen för hantering som varje PostgreSQL-server skapas med. Du blir uppmanad att ange lösenordet som du angav när du skapade servern.

1. När du är ansluten kör du kommandot <kbd>\l</kbd> för att visa en lista med alla databaser. Det här kommandot leder till att två eller fler standarddatabaser returneras.

1. Tryck på <kbd>q</kbd> för att avsluta listan.

1. Du kan prova andra PSQL-kommandon.
    - <kbd>\?</kbd> Få hjälp.
    - <kbd>\dt</kbd> för att göra en lista med tabellerna.

1. När du är klar med att köra PSQL-åtgärder på servern, kör du kommandot <kbd>\q</kbd> för att avsluta PSQL.
