Anta att du använder en lokal PostgreSQL-relationsdatabas med flexibla datatyper och geospatialt stöd. Ditt företag tittar på Expandera, vilket kräver att skala databasen. Som ett alternativ till att investera i ytterligare maskinvara, du är har behörighet att söka efter ett optimala molnbaserade databas-erbjudande. Du har valt att använda en Azure Database for PostgreSQL-server.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Vad är en Azure Database for PostgreSQL-server?

PostgreSQL-server är en central administrationsplats för en eller flera databaser. PostgreSQL-tjänsten i Azure är en hanterad resurs som ger prestanda och ger åtkomst och funktioner på servernivå.

En **Azure Database for PostgreSQL** servern är överordnat _resource_ för en databas. En _resource_ är ett hanterbart objekt som är tillgänglig via Azure. Den här resursen skapades kan du konfigurera din server-instans.

### <a name="what-is-an-azure-database-for-postgresql-server-resource"></a>Vad är en Azure Database for PostgreSQL serverresurs?

En Azure Database for PostgreSQL serverresurs är en behållare med stark livslängd konsekvenser för din server och databaser. Om serverresursen tas bort raderas även alla databaser. Tänk på att alla resurser som tillhör överordnat finns i samma region.

Resursnamnet server används för att definiera namnet på slutpunkten. Exempel: Om resursnamnet är **mypgsqlserver**, och sedan servernamnet blir **mypgsqlserver.postgres.database.azure.com**.

Serverresursen tillhandahåller även den __anslutning omfång__ för hanteringsprinciper som tillämpas på databasen. Till exempel: inloggning, brandvägg, användare, roller och konfiguration.

Precis som öppen källkod-versionen av PostgreSQL, servern är tillgänglig i flera versioner och gör för installation av tillägg. Du väljer vilken serverversion som ska installeras.

> [!NOTE]
> Tillägg kan paketera flera SQL-objekt tillsammans i ett enda paket som kan läsas in eller ta bort det med ett enda kommando. Ett exempel på ett tillägg är `chkpass`, vilket möjliggör en datatyp för automatisk-krypterade lösenord.

## <a name="pricing-tiers"></a>Prisnivåer

Azure Database for PostgreSQL får du möjlighet att välja mellan tre prisnivåer baserat på parametrar som beräkningskraft och lagring.

### <a name="basic-tier"></a>Basic-nivå

Den **grundläggande** nivå är perfekt för arbetsbelastningar som kräver lätt beräkning och i/o-prestanda. I den här nivån kan ha du tillgång till följande maskinvara:

- Compute Gen 4 processorer basera på Intel E5-2673 v3 (Haswell) 2,4 GHz-processorer som är tillgängliga som en 1 eller 2 vCore-konfiguration
- Compute generation 5 processorer basera på Intel E5-2673 v4-processorn (Broadwell) 2.3 GHz-processorer som är tillgängliga som en 1 eller 2 vCore-konfiguration
- Storage upp till 1 TB
- Lokalt redundant säkerhetskopia

Du använder en specifik Nivåinställning för prisnivå för att illustrera stöd för ett specifikt scenario när du skapar en exempelserver senare. Tänk på att för produktionsservrar ska du väljer en prisnivå som matchar din miljö.

### <a name="general-purpose-tier"></a>Nivån för allmän användning

Den **generella** nivå är perfekt för de flesta företags arbetsbelastningar som kräver belastningsutjämnade beräknings- och minnesresurser med skalbart i/o-dataflöde. I den här nivån kan ha du tillgång till följande maskinvara:

- Compute generation 4 processorer som baseras på Intel E5-2673 v3 (Haswell) 2,4 GHz-processorer som är tillgängliga i 2, 4, 8, 18, 32 vCore-konfigurationer
- Compute generation 5 processorer som baseras på Intel E5-2673 v4-processorn (Broadwell) 2.3 GHz-processorer som är tillgängliga i 2, 4, 8, 18, 32 vCore-konfigurationer
- Storage upp till 4 TB
- Lokalt redundant säkerhetskopia
- Geografiskt redundant säkerhetskopia

### <a name="memory-optimized-tier"></a>Optimerad minnesnivå

Den **Minnesoptimerade** nivå är perfekt för högpresterande arbetsbelastningar som kräver minnesprestanda för snabbare bearbetning av transaktioner och högre samtidighet. I den här nivån kan ha du tillgång till följande maskinvara:

- Compute generation 5 processorer som baseras på Intel E5-2673 v4-processorn (Broadwell) 2.3 GHz-processorer som är tillgängliga i 2, 4, 8, 16 vCore-konfigurationer
- Storage upp till 4 TB
- Lokalt redundant säkerhetskopia
- Geografiskt redundant säkerhetskopia

## <a name="steps-to-create-an-azure-database-for-postgresql-server"></a>Steg för att skapa en Azure Database for PostgreSQL-server

Normalt ska du skapa en Azure Database for PostgreSQL-server med Azure-portalen. Nu ska vi titta på de steg som du behöver vidta.

Du ska logga in på Azure Portal först och sedan klickar du på **skapa en resurs**.

Väljer du **databaser** och **Azure Database for PostgreSQL**. Du kan också använda den **Search** funktioner för att hitta den här kategorin.

Portalen visar en PostgreSQL-server configuration skärm, även kallat ett blad och du har gjort dina val.

![Skärmbild av Azure-portalen som visar bladet skapa för en ny databas för PostgreSQL](../media-draft/4-create-blade.png)

Du måste ge ett värde till alla objekt i bladet, så Låt oss ta en titt på var och en.

### <a name="server-name"></a>servernamn

Vi nämnde tidigare ska du skapa en serverresurs. Servernamnet är det objekt som anger den här resursen. Därför kan måste du välja ett unikt namn för servern. Servernamnet får bara innehålla gemener och kan ha siffror och bindestreck.

Anta att du vill att namnge servern _Adventure Works spåra_. Du skulle ställa in namn som `adventure-works-tracking`. Om du försöker skapa en server med ett namn som redan finns, får du ett fel för detta.

### <a name="subscription"></a>Prenumeration

Fältet prenumeration används för fakturering. Du väljer en viss prenumeration om du har mer än en prenumeration som är tillgängliga.

### <a name="resource-group"></a>Resursgrupp

Du ska använda en resursgrupp för att hantera alla resurser som är relaterade till din server. Du kan skapa en ny resursgrupp eller återanvända en befintlig resursgrupp.

### <a name="source"></a>Källa

Du kan också skapa en server från grunden genom att välja den _tom_ standard alternativet eller från en befintlig säkerhetskopia. Den **säkerhetskopiering** alternativet får du möjlighet återställning en geo-säkerhetskopia av en befintlig Azure Database for PostgreSQL-server.

### <a name="server-admin-login-name"></a>Inloggningsnamn för serveradministratör

Du skapar serveradministratörsanvändaren. Välj ett inloggningsnamn som ska användas som en administratör logga in för den nya servern. Inloggningsnamnet för administratören får inte vara azure_superuser, azure_pg_admin, admin, administrator, root, Gäst eller offentlig. Det får inte börja med pg_. Kom ihåg eller Skriv ned det här namnet för framtida användning.

### <a name="password"></a>Lösenord

Du kan välja ett lösenord för att använda med ovanstående inloggningsnamnet för administratören. Lösenordet måste innehålla tecken från tre av följande kategorier:
- Engelska versala bokstäver
- Engelska gemena bokstäver
- Siffror (0 till 9)
- Icke-alfanumeriska tecken (!, $, #, % och så vidare)

Kom ihåg eller Skriv ned det här lösenordet för framtida användning.

### <a name="confirm-password"></a>Bekräfta lösenord

Bekräfta lösenord bekräftelse.

### <a name="location"></a>Plats

Plats-alternativet kan du ange där servern skapas fysiskt. Välj den geografiska platsen som är närmast dig. Platsen måste vara platsen närmast majoriteten av användarna i ett verkligt scenario.

### <a name="version"></a>Version

Du kan ange vilken version av PostgreSQL att använda. Microsoft syftar till att stödja den aktuella versionen och två tidigare versioner av PostgreSQL. Du väljer en version som matchar din produktionsmiljö.

> [!NOTE]
> Mer information finns i följande källor:
> - [PostgreSQL-databas-versioner som stöds](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)
> - [Princip för versionshantering](https://www.postgresql.org/support/versioning/)

### <a name="pricing-tier"></a>Prisnivå

Du måste välja en prisnivå som stöder serverns arbetsbelastning. Kom ihåg att du har tre nivåer att välja bland. Som vi såg tidigare kan var och en av dessa nivåer du konfigurera alternativ för beräkning och lagring. Här är ett exempel med Basic-prisnivån som valts, en Gen 5-beräknings- och en 25 dagars kvarhållningsperiod för säkerhetskopiering.

![Skärmbild av Azure-portalen som visar databasprisnivå för en ny databas för PostgreSQL](../media-draft/4-azure-db-pricing-tier.png)

Allt som återstår nu är att granska de värden som du har angett, göra eventuella kommentarer som du kanske behöver för att senare och klicka på **skapa** du skapade servern.

Det tar några minuter att distribuera servern. Du får ett meddelande när distributionen är klar. Från meddelandet, kan du navigera till den nyligen skapade servern.

Du har nu sett vilka steg du utför för att skapa en Azure Database for PostgreSQL-server. I nästa enhet får du möjlighet att skapa din egen Azure Database for PostgreSQL-server.
