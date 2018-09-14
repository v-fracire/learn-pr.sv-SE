Anta att du kör ett onlineföretag och du publicerar din databas till Azure. Som standard blir en Azure SQL Server-databas som är tillgängliga för alla Azure-program eller tjänster som körs i Azure. Även om du tillåter bara tjänster inom Azure för att ansluta, ska du fortfarande begränsa anslutningar till databasen till godkända program och tjänster.

I den här enheten, ska vi titta på hur du begränsar åtkomsten till Azure SQL-databas via IP-adressintervall.

## <a name="overview"></a>Översikt

En ny Azure SQL-databas, som standard kan Azure-tjänster att ansluta till den. Den här konfigurationen innebär inte en anslutande tjänst kan få tillgång till innehållet i databasen som som den kan försöka att autentisera mot databasen. Kräver en tjänst för att autentisera är säkrare än obegränsad tillgång till internet. Du bör fortfarande begränsa åtkomsten till databasen för att endast program och tjänster som behöver den.

Du kan till exempel ha ett ASP.NET Core-program som pratar med en Azure SQL database. Det är det ASP.NET Core-program som ska få åtkomst till din Azure SQL Server-databas. Nu ska vi titta på hur vi skulle begränsa nätverksåtkomst till databasen från webbprogrammet.

## <a name="restricting-network-access-to-the-database"></a>Begränsa nätverksåtkomst till databasen

Du kan begränsa nätverksåtkomst till din databas genom att bara tillåta åtkomst från ett specifikt IP-adressintervall.

![Skärmbild av Azure-portalen som visar servern skapande av brandväggsregeln med en beskrivs IP-begränsning konfiguration som har lagts till.](../media-draft/2-setting-ip-address-ranges-on-firewall.png)

1. Om du vill skapa en brandväggsregel så anger du en **REGELNAMN**, **första IP-** adress, och **slut-IP** adress.
1. Klicka sedan på **spara** innehåller ändringarna.

Endast de IP-adresser som anges i regler som du skapar har åtkomst till databasen.

## <a name="locking-down-access-at-the-database-level"></a>Låsa åtkomsten på databasnivå

Anta att du kör en redundansgrupp för Azure SQL Server. Med en redundansgrupp finns de sekundära servrarna normalt på olika regioner. Om huvudservern kopplas från, gäller inte längre brandväggsregler för servern. Brandväggsregler för servern anges per server som värd för databasen. Konfigurera en brandväggsregel på databasnivå säkerställer att dina regler replikera för säkerhetskopiering av databas.

Anslut till databasen med SQL Server Management Studio eller SQL Operations Studio för att skapa en brandväggsregel för databasen, och skapa en ny fråga. Du ska skapa en regel för databasen med hjälp av enligt följande konvention där du skickar in Regelnamnet, den första IP-adressen och den sista IP-adressen.

```sql
EXECUTE sp_set_database_firewall_rule N'<Rule Name>', '<From IP Address>', '<To IP Address>'
```

Om du vill begränsa åtkomsten till databasen från IP-adressintervallet 10.21.2.33 – 10.21.2.54, kommer du till exempel använda en regel som liknar följande SQL:

```sql
EXECUTE sp_set_database_firewall_rule N'Web Apps Firewall Rule', '10.21.2.33', '10.21.2.54'
```

## <a name="enabling-transparent-data-encryption-tde"></a>Aktivera Transparent datakryptering (TDE)

### <a name="what-is-transparent-data-encryption"></a>Vad är Transparent datakryptering?

Transparent datakryptering (TDE) utför i realtid kryptering och dekryptering av databasen, säkerhetskopior och loggfiler.

När nya Azure SQL-databaser skapas, har de TDE aktiverat som standard.

Det är viktigt att kontrollera att datakryptering inte har stängts av och äldre Azure SQL Server-databaser kanske inte har TDE aktiverat.

Kontrollera och aktivera transparent Datakryptering:

1. Välj databasen i portalen.
1. Välj alternativet för Transparent datakryptering.
1. Välj 'På' i alternativet för kryptering av data.
1. Klicka på ”Spara”.

## <a name="create-a-secure-connection-to-the-server"></a>Skapa en säker anslutning till servern

Dina program ska ansluta till dina databaser på ett säkert sätt. Du kan använda en anslutningssträng med rätt nivå av säkerhet för att skapa en säker anslutning. Dessa anslutningar ska krypteras för att minska sannolikheten för en man-in-the-middle-attack.

Nu ska vi titta på hur du kan hämta anslutningssträngen för en databas.

Med hjälp av portalen, navigera till din SQL-Server. Välj den databas du vill få åtkomst till.

Välj den *visa Databasanslutningssträngar* alternativet.

Nu bland de tillgängliga alternativen väljer du fliken som matchar programmeringsspråk och kopiera anslutningssträngen visas. Du måste slutföra lösenord, som den förblir hemlig och inte visas här.

![Skärmbild av Azure-portalen som visar avsnittet databas anslutningen strängar med ADO.NET valt och det tillhandahållna värdet-fältet markerat.](../media-draft/2-viewing-connection-strings.png)

Det är viktigt att skydda anslutningssträngen från utanför ögon. Anslutningssträngar ska lagras i Azure Key Vault, inte i ditt projekt, versionskontroll eller system för kontinuerlig integration.

### <a name="what-is-the-azure-key-vault"></a>Vad är Azure Key Vault?

Azure Key Vault är ett verktyg som används för att på ett säkert sätt lagra autentiseringsuppgifter och andra nycklar och hemligheter. Dessa hemligheter kan skyddas genom programvara eller maskinvara.

## <a name="open-the-correct-ports-for-server-access"></a>Öppna rätt portar för serveråtkomst

Din Azure-databas tillåter utgående kommunikation via port 1433. Om du inte har åtkomst till den här porten kan prata med nätverksadministratörerna att tillåta nätverkstrafik.

## <a name="restrict-server-access-with-azure-virtual-networks"></a>Begränsa åtkomst med Azure-nätverk

### <a name="what-is-a-virtual-network"></a>Vad är ett virtuellt nätverk?

Ett virtuellt nätverk är ett logiskt isolerat nätverk som skapats i Azure-nätverket. Du kan använda ett virtuellt nätverk för att styra vad Azure-resurser kan ansluta till andra resurser.

Anta att du kör ett program som ansluter till en databas. Du använder undernät för att isolera olika delar av nätverket. Ett undernät är en del av ett nätverk baserat på ett intervall med IP-adresser.

Om du vill konfigurera dessa undernät måste du skapa ett virtuellt nätverk och sedan dela upp nätverket i undernät. Webbprogrammet ska användas i ett undernät och databasen på ett annat undernät. Varje undernät har sina egna regler för kommunikation till och från andra nätverk. De här reglerna ger dig möjlighet att begränsa åtkomst från databasen till webbprogrammet.

### <a name="what-is-a-network-security-group"></a>Vad är en nätverkssäkerhetsgrupp?

En nätverkssäkerhetsgrupp definierar regler som tillåter eller nekar nätverkstrafik till och från käll-och mål. Varje undernät har en nätverkssäkerhetsgrupp som är tilldelade till den.

Diagrammet nedan visar ett exempel på grupperingar som har skapats. Web-undernät tillåter åtkomst till internet, men endast för HTTP-anslutningar. Databas-undernät tillåter endast åtkomst från undernätet på webben. Hur du konfigurerar det virtuella nätverket lägger till begränsningar om hur tjänster kan nås och fungerar som en brandvägg runt värdbaserade tjänster.

![Virtuellt nätverk för ett webbprogram med anslutna databasen](../media-draft/2-virtualnetwork-overview.png)

I nästa exempel förutsätter att du använder virtuella datorer som ska fungera som webbvärdar för dina- och ansluta till databasen. Nu ska vi titta på hur du skapar ett virtuellt nätverk för att ställa in den här planerade infrastrukturen.

1. Azure-portalen väljer du den **skapa en resurs** länk.
1. Azure Marketplace, Välj **nätverk** > **virtuellt nätverk**. Om det begär Välj en distributionsmodell, Välj **Resource Manager**
1. Klicka på knappen **Skapa**.
1. Ange den **namn** för det virtuella nätverket.
1. Ange en **adressutrymme** som kan användas. Ett adressutrymme är ett sätt att beskriver ett intervall med IP-adresser. I vårt exempel `172.16.0.0/16` refererar till ett adressintervall från 172.16.0.0 till 172.16.255.255.

   Ett adressutrymme som inte redan rekommenderas. Om en IP-adress har identifierats från det här intervallet, kommer att anges som i det här undernätet.

1. Välj Azure **prenumeration**.
1. Välj eller skapa en ny **resursgrupp**.
1. Ange den **namn** för det undernät som du skapar.
1. Ange en **adressintervall**.
1. Klicka på den **skapa** för att skapa det virtuella nätverket.

![Skärmbild av Azure-portalen som visar skapa ett virtuellt nätverk-blad med konfigurationen som beskrivs.](../media-draft/2-create-virtual-network-settings.png)

Du får ett meddelande när det virtuella nätverket har skapats. Klicka på den **gå till resurs** knappen i meddelandet.

Du kommer nu till bladet virtuellt nätverk. Välj den **inställningar** > **undernät** konfigurationsavsnittet. Nu har du det undernät du skapade när du ställer in det inledande nätverk som kallas web_subnet.

Du måste skapa ett nytt undernät som ska representera IP-adresser för databasservrar.

1. Klicka på den __+ undernät__ för att starta processen med att lägga till ett nytt undernät för databasen.

1. Ange en **namn** för undernätet. I exemplet ovan kallas vi den database_subnet.

1. Granska den **adressintervall (CIDR-block)**. Du ser att adressintervallet fylls återigen med ett intervall som inte är i konflikt med andra undernät i systemet.

1. **Nätverkssäkerhetsgrupp** är en core-inställning som du behöver tillämpa till undernätet. Lämna inställningen tom för tillfället. Du kommer senare skapa nätverkssäkerhetsgruppen och gå tillbaka och ange det här värdet.

1. Klicka på den **OK** för att spara undernätet.

![Skärmbild av Azure-portalen som visar bladet Lägg till undernät med beskrivs konfigurationen.](../media-draft/2-create-database-subnet.png)

## <a name="creating-a-network-security-group"></a>Skapa en nätverkssäkerhetsgrupp

En uppgift för nätverkssäkerhetsgruppen är att fungera som en brandvägg och den styr flödet av trafik in och ut ur ett undernät. Du ska skapa två nätverkssäkerhetsgrupper. En är web application undernätet och den andra är för databas-undernätet.

1. Välj **skapa en resurs** och välj **nätverk** > **nätverkssäkerhetsgrupp**. Om du uppmanas att välja en distributionsmodell, Välj Resource Manager.
1. Ange en **namn** för nätverkssäkerhetsgruppen.
1. Välj din Azure-prenumeration, resursgrupp och önskad plats.
1. Klicka på knappen **Skapa**.

![Skärmbild av Azure-portalen som visar säkerhetsgruppen web valt för web-undernätet.](../media-draft/2-define-nsg-for-web-subnet.png)

När nätverkssäkerhetsgruppen har skapats, men det är sedan dags att konfigurera regler för inkommande och utgående trafik för säkerhetsgruppen.

1. Välj den nya nätverkssäkerhetsgruppen.
1. På den **översikt** visning, visas listan över inkommande och utgående regler som har skapats. Det finns standard regler som redan har skapat och som används för intern åtkomst i Azure.

    > [!NOTE]
    > Även om dessa regler inte kan tas bort, kan du skapa ytterligare regler med högre prioritet som har högre prioritet än reglerna.

![Skärmbild av Azure-portalen som visar förkonfigurerade inkommande och utgående säkerhetsregler för en grupp.](../media-draft/2-view-web-apps-security-group-rules.png)

1. Skapa nya regler för att välja den **ingående säkerhetsregler** för nätverkssäkerhetsgruppen.
1. Klicka på knappen **Lägg till**. Härifrån kan konfigurera du informationen för nätverkets säkerhetsregler.

    > [!NOTE]
    > Som standard visas den avancerade vyn för att konfigurera regeln, men genom att klicka på den **grundläggande** knappen, kommer du att kunna välja det protokoll som du vill tillåta.

1. Du vill tillåta åtkomst till HTTP-tjänster från internet. Om du vill filtrera efter HTTP-protokollet, väljer du **Tjänsttagg** som den **källa** värde.
1. Kontrollera att den **Källtjänsttagg** är inställd på **Internet**.
1. Ange porten-intervall för värden som ska `80,443` som representerar de portar som används för att få åtkomst till den här tjänsten. (Används port 80 för HTTP och port 443 används för HTTPS-åtkomst.)
1. Välj **TCP** i **Protokoll**.
1. Ange **åtgärd** till **Tillåt**.
1. Ge regeln ett **prioritet** värdet för `100`.

    > [!NOTE]
    > Ju lägre nummer, de viktigaste regeln är.

![Skärmbild av Azure-portalen som visar bladet Lägg till inkommande regel med den angivna konfigurationen och källtjänsttagg (Internet) och målport intervall (80,443) fält som är markerat.](../media-draft/2-add-inbound-security-rule-for-web-nsg.png)

Nu är det dags att konfigurera inställningarna för nätverkssäkerhetsgrupper för databasen. För databasen är ska ställa in en inkommande regel som tillåter för SQL-förfrågningar från IP-adressintervall för undernätet för web-program och neka andra inkommande och utgående trafik. Detta kommer att du kan styra åtkomsten till databasen och se till att endast databasförfrågningar får i systemet från webbplatsen och att inga andra åtkomst till databasen tillåts.

1. Klicka på den **skapa en resurs** igen för att skapa en ny **nätverk** > **nätverkssäkerhetsgrupp** som använder Resource Manager som distributionsmodell.

    Nu ska du skapa en för databasen.

1. Ange en **namn** för nätverkssäkerhetsgruppen.
1. Välj din Azure-prenumeration, resursgrupp och önskad plats.
1. Klicka på den **skapa** för att skapa nya nätverkssäkerhetsgruppen.

    ![Skärmbild av Azure-portalen som visar skapa network security group-bladet med en exempelkonfiguration.](../media-draft/2-create-database-network-security-group.png)

    Du måste vänta tills nätverkssäkerhetsgruppen har skapats.

1. När klar, väljer den **gå till resurs** alternativet i meddelandet för att börja konfigurera regler för nätverkssäkerhetsgruppen som du skapade.

Huvudfokus är att Databasbegäranden från web application undernätet endast för den nya nätverkssäkerhetsgruppen. Du måste konfigurera inkommande TCP-begäranden på port 1433 från web-undernätets IP-adressintervall.

1. I den **ingående säkerhetsregler** inställningar, Välj den **Lägg till** för att skapa en ny regel. I det här fallet vill tillåta åtkomst från undernätet på web-program.
1. Skapar du en inkommande regel som anger den **källa** till **IP-adresser**.
1. När den **käll-IP-adresser/CIDR-intervall** är visas, ange CIDR-intervall från web-undernätet som du skapade tidigare.
1. För den **målportsintervall**, ange `1433` att ange endast Azure SQL Server-åtkomst.
1. Ange den **protokollet** till **TCP** att ytterligare begränsa inkommande anslutningar.
1. Vill du **Tillåt** komma åt för **åtgärd**.
1. Ge den en **prioritet** av `100`.
1. **Namn på** säkerhetsregeln på rätt sätt.
1. Klicka på **Lägg till** att lägga till regeln.

![Skärmbild av Azure-portalen som visar bladet Lägg till inkommande regel med konfigurationen som beskrivs.](../media-draft/2-create-inbound-rule-for-db-security-group.png)

Grundläggande säkerhetsregler finns nu att begränsa åtkomsten till databassystem. Det som återstår är att tilldela nätverket säkerhetsgrupper till undernät.

1. Öppna det virtuella nätverket som du skapade tidigare. Välj den **inställningar** > **undernät** avsnittet.
1. Välj webb-undernät som skapades tidigare.
1. Välj den **nätverkssäkerhetsgrupp** alternativet och nätverkssäkerhetsgruppen som skapats specifikt för webben.
1. Klicka på **spara** och nätverkssäkerhetsgruppen används mot undernätet.

    ![Skärmbild av Azure-portalen som visar en tillämpad nätverkssäkerhetsgruppen för undernätet web](../media-draft/2-define-nsg-for-web-subnet.png)

Du vill upprepa samma steg för databas-undernätet.

1. Gå tillbaka till undernät.
1. Välj databas-undernät
1. Ställ in dess **nätverkssäkerhetsgrupp** till nätverkssäkerhetsgruppen för databasen.
1. Klicka på **Spara** för att spara ändringarna.

    ![Skärmbild av Azure-portalen som visar en tillämpad nätverkssäkerhetsgrupp som valts för databas-undernätet.](../media-draft/2-define-nsg-for-db-subnet.png)

Nu när du har konfigurerat åtkomst, är det bara några av de virtuella nätverk och undernät mot databasservern och webbservrar.

Vi börjar med databasservern.

1. Välj databasen.
1. Från databasen, Välj den **brandväggar och virtuella nätverk** konfigurationsinställning.

Till vänster visas information om konfigurationen. Du har ett antal inställningar på play här.

1. Först aktiverar **Tillåt åtkomst till Azure-tjänster** till **OFF**. Detta är att säkerställa att endast de tjänster som du vill använda är aktiverade.

    Du ser också att den visar en klient-IP-adress. Klient-IP-adressen är IP-adressen för datorn som ansluter till Azure SQL Server-databasen. Du skulle kunna lägga en klientens IP-adress i namnlistan för regeln. Detta är användbart om du vill ansluta SQL Server Management Studio eller SQL Operations Studio till servern. Den lägger till en regel som anger vilka IP-adresser kan ansluta. Du kommer inte att göra som i det här fallet, men du kan ställa in det virtuella nätverket.

1. Klicka på **Lägg till befintligt virtuellt nätverk**, och du kommer att visas en skärm för alternativ för att ange information för den nya regeln.

1. Ange den **namn** i regeln som du vill använda.
1. Välj den prenumeration som du använde.
1. Viktigast av allt, Välj det virtuella nätverket som du använder.
1. Välj undernätet med de lämpliga reglerna för nätverkssäkerhetsgrupper för databasåtkomst.
1. När du har konfigurerat alla inställningar, Välj den **aktivera** knappen. Den gäller sedan databasen till undernät inom ditt virtuella nätverk.

När databasen undernätet har konfigurerats kan utföra du liknande steg med webbservern. Oavsett hur du har konfigurerat ditt program kan handlar det för att säkerställa att webbprogram använder undernätet för webbåtkomst.

Om du har en virtuell dator eller en belastningsutjämnare med virtuella datorer i en skalningsuppsättning kan du kontrollera att de använder webb-undernät så att de har åtkomst till databasen. När du skapar resurser, till exempel virtuella datorer kan du se till att deras virtuellt nätverk och undernät är konfigurerade för att styra vilken information som går till och från dessa tjänster.

![Skärmbild av Azure-portalen som visar steg på bladet inställningar för att skapa en ny virtuell dator där de virtuella nätverk och undernät värdena har angetts.](../media-draft/2-configure-virtual-machine-with-subnet.png)

När undernäten kan användas i både databasen och de virtuella datorerna som kör web apps kan vara vilken konfiguration på plats. Sedan är närmare åtkomst mellan appar och databas.

Nätverkssäkerhet är den första punkten core skydd. Se till att endast program och tjänster som ska ansluta till databasen ansluter till databasen gör systemet säkrare.
