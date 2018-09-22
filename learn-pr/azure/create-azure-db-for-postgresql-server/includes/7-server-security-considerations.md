Vi antar att du använder en lokal PostgreSQL-databas. Du hanterar alla säkerhetsaspekter och har låst all åtkomst till dina servrar med brandväggsregler på standardservernivå i PostgreSQL. Nu vill du se till att du kan konfigurera samma brandväggsregler på servernivå i Azure.

## <a name="server-security-considerations-and-connection-methods"></a>Säkerhetsöverväganden för server och anslutningsmetoder

Det finns flera olika alternativ för att begränsa åtkomsten till Azure Database for PostgreSQL:s server och databaser. Nätverksåtkomst kan begränsas på nätverks-, server- eller databasnivå. Du kan välja något av följande alternativ:

- Användarkonton för att begränsa databasåtkomst
- Virtuella nätverk för att begränsa nätverksåtkomsten
- Brandväggsregler för att begränsa serveråtkomst

### <a name="authentication-and-authorization"></a>Autentisering och auktorisering

Azure Database for PostgreSQL-servern stöder ursprunglig PostgreSQL-autentisering. Du kan ansluta och autentisera till servern med serverns administratörsinloggning. Du kommer också att skapa användare som kan ansluta till specifika databaser för att begränsa åtkomsten.

### <a name="what-is-a-virtual-network"></a>Vad är ett virtuellt nätverk?

Ett virtuellt nätverk är ett logiskt isolerat nätverk som har skapats i Azure-nätverket. Du kan använda ett virtuellt nätverk till att styra vilka Azure-resurser som kan ansluta till andra resurser.

Anta att du kör en webbapp som ansluter till en databas. Du använder undernät till att isolera olika delar av nätverket. Ett undernät är en del av ett nätverk som baseras på ett intervall med IP-adresser.

När du ska konfigurera undernäten skapar du ett virtuellt nätverk och delar sedan in nätverket i undernät. Webbappen körs i ett undernät och databasen i ett annat. Varje undernät har sina egna regler för kommunikation till och från det andra nätverket. De här reglerna gör att du kan begränsa åtkomsten från databasen till webbappen.

Att skapa ett virtuellt nätverk ligger utanför den här modulens omfattning. Om du behöver mer information kan du utforska andra inlärningsmoduler som relaterar till virtuella nätverk.

### <a name="what-is-a-firewall"></a>Vad är en brandvägg?

En brandvägg är en tjänst som ger serveråtkomst baserat på vilken IP-adress som varje begäran kommer från. Du skapar brandväggsregler som anger intervall med IP-adresser. Det är bara klienter från dessa beviljade IP-adresser som har åtkomst till servern. Brandväggsregler inkluderar även i allmänhet specifika nätverksprotokoll och portinformation. En PostgreSQL-server lyssnar till exempel som standard till TCP-begäranden på port 5432.

### <a name="azure-database-for-postgresql-server-firewall"></a>Azure Database for PostgreSQL-serverbrandvägg

Azure Database for PostgreSQL-serverbrandvägg en förhindrar brandväggar all åtkomst till din databasserver tills du anger vilka datorer som har behörighet. I brandväggskonfigurationen kan du ange ett intervall med IP-adresser som ska kunna ansluta till servern. Servern använder alltid standardinställningen för PostgreSQL-anslutningens information.

![En bild som visar hur Azure Database for PostgreSQL-serverbrandväggen genomsöker IP-adressen för alla inkommande förfrågningar. Endast förfrågningar som kommer från ett antal fördefinierade giltiga IP-adresser vidarebefordras till databasen](../media/7-firewall-diagram.png)

### <a name="azure-database-for-postgresql-server-ssl-connections"></a>SSL-anslutningar till Azure Database for PostgreSQL-server

Azure Database for PostgreSQL föredrar att dina klientprogram ansluts till PostgreSQL-tjänsten med Secure Sockets Layer (SSL). Användning av SSL-anslutningar mellan databasservern och klientprogrammen skyddar mot ”man in the middle”-attacker och liknande attacker genom att kryptera datan mellan servern och klienten. För att aktivera SSL krävs utbyte av nycklar och strikt autentisering mellan klienten och servern för att anslutningen ska fungera. Information om att använda SSL ligger utanför den här modulens omfattning. Om du behöver mer information kan du utforska andra inlärningsmoduler som relaterar till SSL.

## <a name="configure-connection-security"></a>Konfigurera anslutningssäkerhet

Låt oss ta en titt på de beslut och steg som du tar för att konfigurera en Azure Database for PostgreSQL-serverbrandvägg. Du kommer också se hur du ansluter till servern som du skapade tidigare.

Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du aktiverade sandbox-miljön. Gå till serverresursen som du vill skapa en brandväggsregel för.

Välj sedan alternativet **Anslutningssäkerhet** för att öppna bladet Anslutningssäkerhet till höger.

![Skärmbild av Azure Portal som visar avsnittet Anslutningssäkerhet på resursbladet för PostgreSQL-databasen](../media/7-db-security-settings.png)

På den här skärmen finns det flera alternativ. Du kan:

- Lägga till de IP-adresser som du använder för att få åtkomst till portalen som en brandväggspost genom att klicka på knappen **Lägg till klient-IP**.
- Tillåta åtkomst till Azure-tjänster. Som standard har **inte** alla Azure-tjänster åtkomst till PostgreSQL-servern.
- Lägga till brandväggsregler genom att ange IP-adressintervall.
- Framtvinga SSL-anslutningar. Med det här alternativet tvingas klienten ansluta till servern med ett SSL-certifikat.

Kom alltid ihåg att klicka på ikonen **Spara** ovanför inmatningsfältet för att spara din uppdaterade konfiguration när du har gjort ändringar.

### <a name="allow-access-to-azure-services"></a>Tillåta åtkomst till Azure-tjänster

För att använda Azure Cloud Shell för att få åtkomst till eller konfigurera din server ska du se till att aktivera **Tillåt åtkomst till Azure-tjänster**. Det här steget lägger till en brandväggsregel till serverkonfigurationen för att tillåta åtkomst från Cloud Shell. Regeln visas inte som en av de anpassade regler du lägger till.

Du måste också inaktivera **Framtvinga SSL-anslutning**. PowerShell kan inte ansluta till servern om SSL krävs för klientanslutningar.

Båda alternativen leder till ett felmeddelande som visas på kommandoraden om konfigurationen är fel.

Om exempelvis åtkomst inte tillåts för Azure-tjänster och framtvingande av SSL-anslutningar är aktiverat, visas något som liknar det här felet när brandväggen blockerar åtkomsten:

```output
psql: FATAL: no pg_hba.conf entry for host "123.45.67.89", user "adminuser", database "postgres", SSL on FATAL:  SSL connection is required. Please specify SSL options and retry.
```

### <a name="create-a-firewall-rule-using-the-portal"></a>Skapa en brandväggsregel med portalen

Vi antar att du vill skapa en brandväggsregel som ger åtkomst från alla IP-adresser.

> [!WARNING]
> Om du skapar den här brandväggsregeln får alla IP-adresser på Internet tillåtelse att försöka ansluta till din server. Trots att klienterna inte får åtkomst till servern utan användarnamn och lösenord, bör du aktivera den här regeln med försiktighet och se till att du förstår säkerhetsriskerna.

Du kan skapa en ny brandväggsregel genom att ange följande information i fälten med etiketter:

- Regelnamn: `AllowAll`
- Start-IP: `0.0.0.0`
- Slut-IP: `255.255.255.255`

För att ta bort en brandväggsregel klickar du på de tre punkterna (...) i slutet av regeln du vill ta bort. Klicka på knappen **Ta bort** för att ta bort regeln.

Klicka på ikonen **Spara** ovanför inmatningsfälten för att genomföra borttagningen av regeln.

### <a name="create-a-firewall-rule-using-the-azure-cli"></a>Skapa en brandväggsregel med Azure CLI

Du kan använda Azure CLI för att lägga till brandväggsregler till servern med kommandot `az postgres server firewall-rule create`. Här är ett exempel som skapar regeln ovan.

```azurecli
az postgres server firewall-rule create \
  --resource-group <rgn>[Sandbox resource group name]</rgn> \
  --server <server-name> \
  --name AllowAll \
  --start-ip-address 0.0.0.0 \
  --end-ip-address 255.255.255.255
```

Du tar bort brandväggsregler från servern med kommandot `az postgres server firewall-rule delete`. Här är ett exempel:

```azurecli
az postgres server firewall-rule delete \
  --name AllowAll \
  --resource-group <rgn>[Sandbox resource group name]</rgn> \
  --server-name <server-name>
```

## <a name="connecting-to-your-server"></a>Ansluta till servern

Precis som vilken modern databas som helst kräver PostgreSQL regelbunden serveradministration för att uppnå bästa möjliga prestanda. Det finns ett antal alternativ för att ansluta och hantera din Azure Database for PostgreSQL-server. Vi använder `psql` för att ansluta till servern.

### <a name="what-is-psql"></a>Vad är psql?

Kommandoradsverktyget som kallas för `psql` är en distribuerad interaktiv PostgreSQL-terminal som samverkar med PostgreSQL:s server och databaser. `psql` fungerar på samma sätt med Azure Database for PostgreSQL som andra PostgreSQL-implementeringar och medföljer Azure Cloud Shell. Med verktyget `psql` kan du hantera databaser och köra strukturfrågor mot databaserna.

Med hjälp av `psql` kräver en anslutning till en PostgreSQL-server. Det finns ett antal tillgängliga kommandoradsparametrar som du kan använda när du arbetar med `psql`.

- `--host` – Värden som du vill ansluta till.
- `--username` – Användarens namn/ID som du ska ansluta till.
- `--dbname` – Namnet på databasen som du ska ansluta till.

> [!TIP]
> Du ansluter vanligen till hanteringsdatabasen `postgres` när du hanterar din serveråtkomst och databasernas konfiguration.

Här är det fullständiga kommandot:

```bash
psql --host=<server-name>.postgres.database.azure.com
      --username=<admin-user>@<server-name> 
      --dbname=<database>
```

När du är ansluten ser du en kommandotolk där du kan köra kommandon till servern och databaserna.

Nu har du sett vilka åtgärder du vidtar för att konfigurera Azure Database for PostgreSQL:s säkerhetsinställningar. I nästa del ska du konfigurera säkerhetsinställningarna för Azure Database for PostgreSQL. Du ska även ansluta till servern med Cloud Shell.
