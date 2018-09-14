Anta att du använder en lokal PostgreSQL-databas. Du hanterar alla säkerhetsaspekter och du har låst all åtkomst till dina servrar med hjälp av standardregler för PostgreSQL-brandväggen på servernivå. Nu vill du se till att du kan konfigurera samma servernivå brandväggsregler i Azure.

## <a name="server-security-considerations-and-connection-methods"></a>Säkerhetsöverväganden för Server och anslutningsmetoder

Du har ett antal alternativ för att begränsa åtkomsten till din Azure Database for PostgreSQL-server och databaser. Åtkomst till nätverket kan vara begränsad till ett nätverk, servern eller databasnivå. Du kan använda någon av följande alternativ:

- Användarkonton för att begränsa databasåtkomst
- Virtuella nätverk för att begränsa nätverksåtkomsten till
- Brandväggsregler för att begränsa åtkomst

### <a name="authentication-and-authorization"></a>Autentisering och auktorisering

Azure Database for PostgreSQL-server stöder interna PostgreSQL-autentisering. Du kan ansluta och autentisera till servern med den serveradministratör. Du måste också skapa användarna att ansluta till vissa databaser att begränsa åtkomsten.

### <a name="what-is-a-virtual-network"></a>Vad är ett virtuellt nätverk?

Ett virtuellt nätverk är ett logiskt isolerat nätverk som har skapats i Azure-nätverket. Du kan använda ett virtuellt nätverk för att styra vad Azure-resurser kan ansluta till andra resurser.

Anta att du kör ett program som ansluter till en databas. Du använder undernät för att isolera olika delar av nätverket. Ett undernät är en del av ett nätverk som baseras på ett intervall med IP-adresser.

Om du vill konfigurera dessa undernät måste du skapa ett virtuellt nätverk och sedan dela upp nätverket i undernät. Webbprogrammet ska användas i ett undernät och databasen på ett annat undernät. Varje undernät har sina egna regler för kommunikation till och från andra nätverk. De här reglerna ger dig möjlighet att begränsa åtkomst från databasen till webbprogrammet.

Skapa ett virtuellt nätverk är utanför omfattningen för den här modulen. Om du behöver mer information kan du utforska andra learning-moduler som är relaterade till virtuella nätverk.

### <a name="what-is-a-firewall"></a>Vad är en brandvägg?

En brandvägg är en tjänst som ger åtkomst baserat på den ursprungliga IP-adressen för varje begäran. Du skapar brandväggsregler som anger intervall med IP-adresser. Endast klienter från dessa beviljas IP-adresser kommer att kunna ansluta till servern. Brandväggsregler, generellt sett innehålla också information om specifika nätverk protokoll och port. Till exempel lyssnar en PostgreSQL-server som standard på TCP-begäranden på port 5432.

### <a name="azure-database-for-postgresql-server-firewall"></a>Azure Database för PostgreSQL-server-brandväggen

Azure Database for PostgreSQL serverbrandvägg förhindrar all åtkomst till din databasserver tills du anger vilka datorer som har behörighet. Brandväggskonfigurationen kan du ange ett intervall med IP-adresser som ska kunna ansluta till servern. Servern använder alltid informationen som standard PostgreSQL.

![Funktionsdiagram för Azure-brandväggen](../media-draft/7-firewall-diagram.png)

### <a name="azure-database-for-postgresql-server-ssl-connections"></a>Azure Database for PostgreSQL server SSL-anslutningar

Azure Database för PostgreSQL föredrar att ditt klientprogram ansluta till PostgreSQL-tjänsten med hjälp av Secure Sockets Layer (SSL). Att framtvinga SSL-anslutningar mellan databasservern och klientprogrammen hjälper till att skydda mot ”man in the middle” och liknande attacker genom att kryptera data mellan servern och klienten. Om du aktiverar SSL kräver ett utbyte av nycklar och strikt autentisering mellan klienten och servern för anslutningen ska fungera. Information om hur du använder SSL ligger utanför omfånget för den här lärmodulen. Om du behöver mer information kan du utforska andra inlärningsmoduler som rör SSL.

## <a name="configure-connection-security"></a>Konfigurera anslutningssäkerhet

Nu ska vi titta på beslut och steg som du gör för att konfigurera en Azure Database for PostgreSQL-server-brandväggen. Du lär dig också att ansluta till servern som du skapade tidigare.

Först ska du öppna den [Azure-portalen](https://portal.azure.com?azure-portal=true) och navigera till serverresursen som du vill skapa en brandväggsregel.

Sedan väljer du den **anslutningssäkerhet** möjligheten att öppna anslutningsbladet till höger.

![Skärmbild av Azure-portalen som visar avsnittet anslutning säkerhet på resursbladet för PostgreSQL-databas](../media-draft/7-db-security-settings.png)

På den här skärmen har du flera alternativ. Du kan:

- Lägg till IP-adressen som används för att få åtkomst till portalen som en brandvägg post genom att klicka på den **klientens IP-adress** knappen.
- Tillåt åtkomst till Azure-tjänster. Som standard alla Azure-tjänster **inte** har åtkomst till PostgreSQL-servern.
- Lägga till brandväggsregler genom att ange IP-adressintervall.
- Framtvinga SSL-anslutningar. Det här alternativet tvingas klienten att ansluta till servern med ett SSL-certifikat.

Kom ihåg att klicka på alltid den **spara** -ikonen ovanför fälten posten för att spara den uppdaterade konfigurationen när du har gjort ändringar.

### <a name="allow-access-to-azure-services"></a>Tillåt åtkomst till Azure-tjänster

Om du vill använda Azure Cloud Shell för att komma åt eller konfigurera servern, se till att aktivera **Tillåt åtkomst till Azure-tjänster**. Det här steget kommer att lägga till en brandväggsregel i serverkonfigurationen som ger åtkomst från Cloud Shell. Den här regeln visas inte som en av de anpassa regler som du lägger till.

Du måste också inaktivera **framtvinga SSL-anslutning**. PowerShell kan inte ansluta till servern om SSL krävs för klientanslutningar.

Båda dessa alternativ leder till ett felmeddelande som har visas på kommandoraden om de inte konfigurerats korrekt.

Till exempel om åtkomst tillåts inte till Azure-tjänster och framtvinga SSL-anslutningar är aktiverad, därefter visas något som liknar det här felet när brandväggen blockerar åtkomst:

> psql: oåterkalleligt fel: ingen pg_hba.conf post för värd ”123.45.67.89” användare ”adminuser” databas ”postgres”, SSL på oåterkalleligt fel: SSL-anslutning krävs. Please specify SSL options and retry.

### <a name="create-a-firewall-rule-using-the-portal"></a>Skapa en brandväggsregel med hjälp av portalen

Vi antar att du vill skapa en brandväggsregel som ger åtkomst från alla IP-adresser.

> [!WARNING]
> Skapa den här brandväggsregeln tillåter alla IP-adresser på internet att ansluta till servern. Eventhough klienter inte kan få åtkomst till servern utan användarnamn och lösenord, aktivera den här regeln med försiktighet och kontrollera att du förstår säkerhetsriskerna.

Du kan skapa en ny brandväggsregel genom att ange följande information i fälten med etiketter:

- Regelnamn: `AllowAll`
- Start-IP: `0.0.0.0`
- Slut-IP: `255.255.255.255`

Om du vill ta bort en brandväggsregel, klickar du på ellipsen (...) i slutet av den regel som du vill ta bort. Klicka på den **ta bort** för att ta bort regeln.

Klicka på den **spara** -ikonen ovanför inmatningsfälten att genomföra borttagningen av regeln.

### <a name="create-a-firewall-rule-using-the-azure-cli"></a>Skapa en brandväggsregel med hjälp av Azure CLI

Du använder Azure CLI för att lägga till brandväggsregler till servern med den `az postgres server firewall-rule create` kommandot använder Azure CloudShell.

Vi antar att du vill skapa samma regler som ovan. Du kan använda följande kommando:

  ```azurecli
  az postgres server firewall-rule create --resource-group <resource_group_name> --server <server-name> --name AllowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
  ```

Du tar bort brandväggsregler från servern med kommandot `az postgres server firewall-rule delete`.

Vi antar att du vill ta bort brandväggen som du skapade. Sedan kan du använda följande kommando:

  ```azurecli
  az postgres server firewall-rule delete --name AllowAll --resource-group <resource_group_name> --server-name <server-name>
  ```

## <a name="connecting-to-your-server"></a>Ansluter till servern

Som alla moderna database kräver PostgreSQL regelbundna serveradministration att uppnå bästa prestanda. Du har ett antal alternativ för att ansluta och hantera din Azure Database for PostgreSQL-server. Vi använder `psql` att ansluta till servern.

### <a name="what-is-psql"></a>Vad är psql?

Kommandoradsverktyget kallas `psql` är PostgreSQL distribuerade interaktiv terminal för att arbeta med PostgreSQL-servrar och databaser. `psql` fungerar med Azure Database för PostgreSQL på samma sätt som med andra PostgreSQL-implementering och ingår i Azure Cloud Shell. Den `psql` verktyget kan du hantera databaser samt köra struktur frågor mot dessa databaser.

Med hjälp av `psql` kräver en anslutning till en PostgreSQL-server. Det finns ett antal kommandoradsparametrar tillgängliga för användning när du arbetar med `psql`.

- `--host` -Värden som du vill ansluta.
- `--username` -Användaren namn/ID som ska anslutas.
- `--dbname` – Namnet på databasen som ska ansluta till.

> [!Note]
> Ansluter du normalt till de `postgres` hanteringsdatabasen när du hanterar din serverkonfigurationer för åtkomst och databasen.

Här är det fullständiga kommandot:

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=<database>
  ```

När du är ansluten, visas en kommandotolk och kan köra kommandon till servern och databaserna.

Du har nu sett instruktioner om hur du konfigurerar Azure Database för PostgreSQL-säkerhetsinställningar. I nästa enhet konfigurerar du Azure Database för PostgreSQL-säkerhetsinställningar. Du kan också ansluta till servern med Cloud Shell.
