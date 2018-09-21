Vi antar att du använder en lokal PostgreSQL-relationsdatabas med flexibla datatyper och geospatialt stöd. Ditt företag planerar att expandera, vilket kräver att databasen skalas. Som ett alternativ till att investera i ytterligare maskinvara får du uppgiften att hitta ett optimalt erbjudande för molnbaserade databaser. Du har bestämt dig för att använda en Azure Database for PostgreSQL-server.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Vad är en Azure Database for PostgreSQL-server?

PostgreSQL-servern är en central administrationsplats för en eller flera databaser. PostgreSQL-tjänsten i Azure är en hanterad resurs som ger prestandagarantier och åtkomst och funktioner på servernivå.

En **Azure Database for PostgreSQL**-server är den överordnade _resursen_ för en databas. En _resurs_ är ett hanterbart objekt som är tillgängligt via Azure. När du skapar den här resursen kan du konfigurera din serverinstans.

### <a name="what-is-an-azure-database-for-postgresql-server-resource"></a>Vad är en Azure Database for PostgreSQL-serverresurs?

En Azure Database for PostgreSQL-serverresurs är en container med avsevärda effekter vad gäller livslängden för din server och dina databaser. Om serverresursen tas bort så tas även alla databaser bort. Tänk på att alla resurser som tillhör det överordnade elementet finns i samma region.

Serverresursnamnet används för att definiera namnet på serverslutpunkten. Om resursnamnet till exempel är **mypgsqlserver** blir servernamnet **mypgsqlserver.postgres.database.azure.com**.

Serverresursen tillhandahåller även __anslutningsomfånget__ för de hanteringsprinciper som tillämpas på dess databas. Det gäller till exempel inloggning, brandvägg, användare, roller och konfiguration.

Precis som i versionen av PostgreSQL med öppen källkod, finns servern i flera versioner och medger installation av tillägg. Du väljer vilken serverversion som ska installeras.

> [!NOTE]
> Med tillägg kan du sammanställa flera SQL-objekt i ett enda paket, som kan läsas in eller tas bort med ett enda kommando. Ett exempel på ett tillägg är `chkpass`, som tillhandahåller en datatyp för automatiskt krypterade lösenord.

## <a name="pricing-tiers"></a>Prisnivåer

Med Azure Database for PostgreSQL kan du välja mellan tre prisnivåer utifrån parametrar som exempelvis beräkningskraft och lagring.

### <a name="basic-tier"></a>Basic-nivå

Nivån **Basic** är perfekt för arbetsbelastningar som kräver lätt beräkning och I/O-prestanda. På den här nivån får du tillgång till följande maskinvara:

- Compute Gen 4-processorer baserade på Intel E5-2673 v3 (Haswell) 2,4 GHz-processorer som är tillgängliga som antingen en 1- eller 2 vCore-konfiguration
- Compute Gen 5-processorer baserade på Intel E5-2673 v4 (Broadwell) 2,3 GHz-processorer som är tillgängliga som antingen en 1- eller 2 vCore-konfiguration
- Lagring upp till 1 TB
- Lokalt redundant säkerhetskopiering

Du använder en specifik inställning för prisnivå för att illustrera stöd för ett specifikt scenario när du skapar en exempelserver senare. Tänk på att du för produktionsservrar måste välja en prisnivå som matchar din miljö.

### <a name="general-purpose-tier"></a>Nivån Generell användning

Nivån för **Generell användning** är perfekt för de flesta företags arbetsbelastningar som kräver balans mellan beräkning och minne med skalbart I/O-dataflöde. På den här nivån får du tillgång till följande maskinvara:

- Compute Gen 4-processorer baserade på Intel E5-2673 v3 (Haswell) 2,4 GHz-processorer som är tillgängliga i konfigurationer med 2, 4, 8, 18 eller 32 vCore
- Compute Gen 5-processorer baserade på Intel E5-2673 v4 (Broadwell) 2,3 GHz-processorer som är tillgängliga i konfigurationer med 2, 4, 8, 18 eller 32 vCore
- Lagring upp till 4 TB
- Lokalt redundant säkerhetskopiering
- Geografiskt redundant säkerhetskopiering

### <a name="memory-optimized-tier"></a>Nivån Minnesoptimerad

Nivån **Minnesoptimerad** är perfekt för högpresterande databasarbetsbelastningar som kräver minnesintern prestanda för snabbare TP (transaction processing) och högre samtidighet. På den här nivån får du tillgång till följande maskinvara:

- Compute Gen 5-processorer baserade på Intel E5-2673 v4 (Broadwell) 2,3 GHz-processorer som är tillgängliga i konfigurationer med 2, 4, 8 eller 16 vCore
- Lagring upp till 4 TB
- Lokalt redundant säkerhetskopiering
- Geografiskt redundant säkerhetskopiering

## <a name="steps-to-create-an-azure-database-for-postgresql-server"></a>Steg för att skapa en Azure Database for PostgreSQL-server

Vanligtvis skapar du en Azure Database for PostgreSQL-server med hjälp av Azure-portalen. Vi tittar på de steg som du behöver vidta.

Först loggar du in på Azure Portal och sedan klickar du på **Skapa en resurs**.

Välj **Databaser** och **Azure Database for PostgreSQL**. Du kan även använda funktionen **Sök** för att hitta den här kategorin.

Portalen visar en skärm för PostgreSQL-serverkonfiguration (även kallat ett blad) där du gör ditt val.

![Skärmbild av Azure Portal som visar bladet Skapa för en ny PostgreSQL-databas](../media/4-create-blade.png)

Du måste ange ett värde för alla objekt på bladet, så vi går igenom vart och ett av dem.

### <a name="server-name"></a>Servernamn

Som du kanske minns behöver du skapa en serverresurs. Servernamnet är det objekt som anger resursen. Därför behöver du även välja ett unikt namn för servern. Servernamnet får bara innehålla gemener och får innehålla siffror och bindestreck.

Anta att du vill ge servern namnet _Adventure Works Tracking_. Du skulle då ange namnet som `adventure-works-tracking`. Om du försöker skapa en server med ett namn som redan finns, visas ett felmeddelande.

### <a name="subscription"></a>Prenumeration

Fältet prenumeration används för fakturering. Du väljer en viss prenumeration om du har mer än en prenumeration tillgänglig.

### <a name="resource-group"></a>Resursgrupp

Du använder en resursgrupp för att hantera alla resurser som är relaterade till din server. Du kan skapa en ny resursgrupp eller återanvända en befintlig resursgrupp.

### <a name="source"></a>Källa

Du kan skapa en server från grunden genom att välja standardalternativet _Tom_, eller från en befintlig säkerhetskopia. Alternativet **Säkerhetskopia** ger möjlighet att återställa en geo-säkerhetskopia av en befintlig Azure Database for PostgreSQL-server.

### <a name="server-admin-login-name"></a>Inloggningsnamn för serveradministratör

Du skapar serveradministratörsanvändaren. Välj ett inloggningsnamn som du ska använda som administratörsinloggning för den nya servern. Inloggningsnamnet för administratören får inte vara azure_superuser, azure_pg_admin, admin, administrator, root, guest eller public. Det får inte börja med pg_. Kom ihåg att anteckna namnet för framtida användning.

### <a name="password"></a>Lösenord

Du väljer ett lösenord som ska användas med administratörsinloggningsnamnet ovan. Ditt lösenord måste innehålla tecken från tre av följande kategorier:
- Engelska versala bokstäver
- Engelska gemena bokstäver
- Siffror (0 till 9)
- Icke-alfanumeriska tecken (!, $, #, % osv.)

Kom ihåg att anteckna lösenordet för framtida användning.

### <a name="confirm-password"></a>Bekräfta lösenord

Ange lösenordet igen som bekräftelse.

### <a name="location"></a>Plats

Med platsalternativet kan du ange det ställe där servern skapas fysiskt. Välj den geografiska plats som är närmast dig. I ett verkligt scenario bör platsen vara den plats som är närmast majoriteten av dina användare.

### <a name="version"></a>Version

Du kan ange den version av PostgreSQL som ska användas. Microsoft syftar till att stödja den aktuella versionen och två tidigare versioner av PostgreSQL. Du väljer en version som matchar din produktionsmiljö.

> [!NOTE]
> Mer information finns i följande källor:
> - [PostgreSQL-databasversioner som stöds](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)
> - [Versionspolicy](https://www.postgresql.org/support/versioning/)

### <a name="pricing-tier"></a>Prisnivå

Välj en prisnivå som stöder serverns arbetsbelastning. Kom ihåg att du har tre nivåer att välja mellan. Som vi såg tidigare kan du med var och en av dessa nivåer konfigurera alternativ för beräkning och lagring. Här är ett exempel med prisnivån Basic vald, en Gen 5-beräkning och en 25-dagars kvarhållningsperiod för säkerhetskopior.

![Skärmbild av Azure Portal som visar databasens prisnivå för en ny PostgreSQL-databas](../media/4-azure-db-pricing-tier.png)

Allt som återstår nu är att granska de värden du angav, göra eventuella anteckningar som du kan behöva senare och klicka på **Skapa** för att skapa servern.

Det tar några minuter att distribuera servern. Du får ett meddelande när distributionen är klar. Från meddelandet kan du navigera till den nyligen skapade servern.

Nu har du sett vilka åtgärder du ska vidta för att skapa en Azure Database for PostgreSQL-server. I nästa kursdel får du möjlighet att skapa din egna Azure Database for PostgreSQL-server.
