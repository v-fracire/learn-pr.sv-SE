Säkerhetskopiering är den slutgiltiga och mest kraftfulla försvarslinjen mot permanent dataförlust. En effektiv strategi för säkerhetskopiering kräver mer än bara göra kopior av data. Den behöver beakta dataarkitektur och infrastruktur för dina program. Din app kan hantera många typer av data av varierande betydelse, ofta fördelade över filsystem, databaser och andra lagringstjänster både i molnet och lokalt. Med rätt tjänster och produkter för jobbet förenklas säkerhetskopieringsprocessen och återställningstiden förbättras om en säkerhetskopia behöver återställas.

## <a name="establish-backup-and-restoration-requirements"></a>Etablera krav för säkerhetskopiering och återställning

Precis som med en strategi för haveriberedskap baseras säkerhetskopieringskrav på en kostnadsfördelsanalys. Analys av din Apps data bör följa den relativa prioriteten för olika typer av data som app som hanterar, samt externa förutsättningar, till exempel lagar för kvarhållning av data.

Om du vill etablera säkerhetskopieringskrav för appen går du igenom appens data och gör en analys för att gruppera dem utifrån följande krav:

* Hur mycket av den här typen av data du har råd att förlora, mätt i tid
* Den längsta tiden som en återställning av den här typen av data skulle kräva
* Krav på datalagring för säkerhetskopiering: hur lång tid och med vilken frekvens för säkerhetskopiering behöver fortfarande är tillgängliga

De här koncepten mappa snyggt till begreppen mål för återställningspunkt och återställningstid (RTO och RPO). Varaktigheten för godtagbar dataförlust översätts vanligen direkt till nödvändiga säkerhetskopieringsintervaller och RPO. Den längsta tid en säkerhetskopiering tar motsvarar RTO:n för programmets datakomponent. Båda krav bör utvecklas i förhållande till kostnaden för att uppfylla dem. Varje organisation vill säga att de verkligen inte har råd att förlora *alla* data, men ofta som inte är fallet när kostnaden för att uppfylla detta krav anses.

Säkerhetskopiering spelar absolut roll vid haveriberedskap (DR), men säkerhetskopior, återställningar och deras associerade scenarier sträcker sig längre än haveriberedskapens omfattning. Säkerhetskopieringar kan behöva återställas i icke-katastrofåterställningsscenarier, inklusive de där RTO och RPO inte är viktig för oss. Till exempel om en liten mängd data som är äldre än intervall för säkerhetskopiering har skadats eller tagits bort, men programmet inte drabbas, ditt program kanske aldrig riskerar saknas sitt SERVICENIVÅAVTAL för och en lyckad återställning resulterar i att inga data förlorade. Din haveriberedskapsplan kanske eller kanske inte omfattar vägledning om att utföra återställningar i icke-katastrofsituationer.

> [!TIP]
> Blanda inte ihop *arkivering*, *replikering* och *säkerhetskopiering*. Arkivering är lagring av data för långsiktigt skydd och läsbehörighet. Replikering är nära realtid kopiering av data mellan repliker för hög tillgänglighet och vissa scenarier för haveriberedskap. Vissa förutsättningar, till exempel lagar för kvarhållning av data, kan påverka din strategier för alla tre av dessa problem. Arkivering, replikering och säkerhetskopiering alla kräver separat analys och implementering.

## <a name="azure-backup-and-restore-capabilities"></a>Funktioner för säkerhetskopiering och återställning i Azure

Azure erbjuder flera säkerhetskopieringsrelaterade tjänster och funktioner för olika scenarier, inklusive data i Azure och lokala data. De flesta Azure-tjänster innehåller någon typ av säkerhetskopieringsfunktioner. Här tar vi en titt på några av de populäraste Azure-erbjudandena som relaterar till säkerhetskopiering.

### <a name="azure-backup"></a>Azure Backup

Azure Backup är en familj med säkerhetskopieringsprodukter som säkerhetskopierar data till Azure Recovery Services-valv för lagring och återställning. Recovery Service-valv är lagringsresurser i Azure som är dedikerade för att innehålla data- och konfigurationssäkerhetskopiering för virtuella datorer, servrar och individuella arbetsstationer och arbetsbelastningar.

> [!NOTE]
> I både Azure Backup och Azure Site Recovery används Azure Recovery Service-valv till lagring. Azure Backup är en allmänt lösning för säkerhetskopiering. Azure Site Recovery kan samordna replikering och redundans och stöd för låg RPO och RTO disaster recovery-åtgärder.

Azure Backup fungerar som en allmän lösning för säkerhetskopiering för molnet och lokala arbetsflöden som körs på virtuella datorer eller fysiska servrar. Det är utformat för att vara en lättillgänglig ersättning av traditionella säkerhetskopieringslösningar som lagrar data i Azure istället för arkivband eller andra lokala fysiska medier.

Fyra olika produkter och tjänster kan använda Azure Backup för att skapa säkerhetskopior:

* **Azure Backup-agenten** är ett litet Windows-program som säkerhetskopierar filer, mappar och systemtillstånd från Windows virtuell dator eller server är installerad. Den fungerar på ett sätt som liknar många konsument molnbaserade säkerhetskopieringslösningar, men kräver konfiguration av ett Azure Recovery-valvet. När du laddar ned och installerar den på en Windows-server eller virtuell dator kan du konfigurera den så att den skapar säkerhetskopior upp till tre gånger per dag.
* **System Center Data Protection Manager** är ett robust, fullständigt system för säkerhetskopiering och återställning på företagsnivå. Data Protection Manager är ett Windows Server-program som kan säkerhetskopiera virtuella datorer (Windows och Linux) och filsystem, skapa bare metal-säkerhetskopior av fysiska servrar och utför Programmedveten säkerhetskopiering av många Microsoft-serverprodukter, till exempel SQL Server och Exchange. Data Protection Manager är en del av System Center-familj av produkter och de är licensierad och säljs med System Center, men den betraktas som en del i Azure Backup-familjen eftersom den kan lagra säkerhetskopior i ett Azure Recovery-valv.
* **Azure Backup Server** liknar Data Protection Manager, men dess licensierade som en del av en Azure-prenumeration och kräver inte en System Center-licens. Azure Backup Server stöder samma funktioner som Data Protection Manager förutom lokala bandsäkerhetskopiering och integrering med andra System Center-produkter.
* **Säkerhetskopiering av virtuella IaaS-datorer i Azure** är en nyckelfärdig funktion för säkerhetskopiering och återställning av Azure Virtual Machines. Säkerhetskopiering av virtuella datorer stöder en gång per dag säkerhetskopieringar för Windows och Linux-datorer. Den har stöd för återställning av enskilda filer, fulla diskar och hela virtuella datorer och kan också utföra programkonsekventa säkerhetskopior. Enskilda program kan bli underrättad om säkerhetskopiering och få sina filsystem-resurser i ett konsekvent tillstånd innan ögonblicksbilden tas.

![Azure Backup](../media/azure-backup.png)

Azure Backup kan ge ökat värde och bidra till strategin för säkerhetskopiering och återställning för IaaS- och lokala program av praktiskt taget vilken storlek och form som helst.

### <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Storage innehåller inte en funktionen för automatisk säkerhetskopiering, men används ofta för säkerhetskopiering av alla typer av data från olika källor. Många tjänster som erbjuder säkerhetskopieringsfunktioner använder blobar för att lagra sina data, och blobar är ett vanligt mål för skript och verktyg i alla tänkbara scenarier för säkerhetskopiering.

V2-lagringskonton för generell användning stöder tre olika nivåer för bloblagring med varierande prestanda och kostnad. **Lågfrekvent** storage erbjuder förhållandet bästa kostnaden för prestanda för de flesta säkerhetskopieringar inte **frekvent** lagring, vilket ger lägre åtkomst kostar men högre kostnader för lagring. **Arkivera**-nivån lagring kan vara lämpligt för sekundära säkerhetskopieringar eller säkerhetskopior av data med låg förväntningar för återställningstid. Det är låg kostnad, men kräver upp till 15 timmar ledtider för att få åtkomst till.

Oföränderlig bloblagring kan konfigureras för att vara icke-raderbar och icke-ändringsbar för ett användarangivet intervall. Oföränderlig bloblagring utformades främst för att uppfylla strikta krav för vissa typer av data, till exempel finansiella data. Det är ett bra alternativ för att säkerställa att säkerhetskopior är skyddade mot oavsiktlig borttagning och ändring av.

### <a name="azure-sql-database"></a>Azure SQL Database

Omfattande, automatiska säkerhetskopieringsfunktioner ingår i Azure SQL Database utan extra kostnad. Fullständiga säkerhetskopieringar skapas varje vecka, och differentiella säkerhetskopieringar utförs var 12: e timme och loggsäkerhetskopior görs var femte minut. Säkerhetskopior som har skapats av tjänsten kan användas för att återställa en databas till en specifik tidpunkt, även om de har tagits bort. Återställningar kan göras med hjälp av Azure Portal, PowerShell eller REST-API:et. Säkerhetskopiering för databaser som krypterats med transparent datakryptering, som är aktiverat som standard, krypteras också.

SQL Database-säkerhetskopiering är i företagslass, är produktionsklar och aktiverad som standard. Om du utvärderar olika databasalternativ för en app, bör den vara med i kostnadsfördel analys, eftersom det är en stor fördel med tjänsten. Alla appar som använder Azure SQL Database ska dra nytta av det genom att inkludera det i sin plan för haveriberedskap och i sina procedurer för säkerhetskopiering/återställning.

### <a name="azure-app-service"></a>Azure App Service

Webbprogram som hanteras i Azure App Service Standard och Premium-nivåerna stöd för nyckelfärdig schemalagda säkerhetskopieringar och manuella säkerhetskopieringar. Säkerhetskopieringar innefattar konfiguration och filinnehåll och innehåll i databaser som används i appen. De stöder också enkla filter för att exkludera filer. Återställa åtgärder kan riktas mot olika App Service-instanser, vilket gör Apptjänst Säkerhetskopiera ett enkelt sätt att flytta innehållet från en app till en annan.

App Service-säkerhetskopior är begränsade till 10 GB totalt, inklusive app- och databasinnehåll. De är en bra lösning för nya appar som utvecklas, och småskaliga appar. Mognare program använder inte Allmänt App Service-säkerhetskopiering. De i stället förlitar sig på stabil och rollback procedurer, storage strategier som inte använder programmet disklagring och dedikerad säkerhetskopieringsstrategier för databaser och beständig lagring.

## <a name="verify-backups-and-test-restore-procedures"></a>Kontrollera säkerhetskopieringar och testa återställningsprocedurerna

Inget säkerhetskopieringssystem är komplett utan någon strategi för att verifiera säkerhetskopior och testa återställningsprocedurerna. Även om du använder en dedikerad tjänst som säkerhetskopiering eller en produkt, men du bör fortfarande återställningsproceduren dokumentet och praxis för att säkerställa att de är väl utvärderade och återgå till det förväntade tillståndet.

Strategier för att verifiera säkerhetskopior varierar och beror på typ av infrastruktur. Du kanske vill teknik, till exempel skapa en ny distribution av programmet, återställer säkerhetskopian och jämföra tillståndet för två instanser. I många fall imiterar nära faktiska återställningsprocedurer i den här tekniken. Det räcker med att bara jämföra en delmängd av säkerhetskopierade data med livedata omedelbart efter att en säkerhetskopia har skapats. En komponent som är vanliga i verifiera säkerhetskopieringen försöker återställa gamla säkerhetskopior för att säkerställa att de är fortfarande tillgänglig och fungerar och att säkerhetskopiera systemet inte har ändrats på ett sätt som gör dem är inkompatibla.

Alla strategier är bättre än att ta reda på att dina säkerhetskopior är skadade eller ofullständiga när du försöker återställa efter en katastrof.

En strategi för säkerhetskopiering och återställning är en viktig del av att se till att din arkitektur kan återställas från dataförlust eller skadade data. Granska din arkitektur för att definiera dina krav för säkerhetskopiering och återställning. Azure tillhandahåller flera tjänster och funktioner för att tillhandahålla säkerhetskopierings- och återställningsfunktioner för alla arkitekturer.
