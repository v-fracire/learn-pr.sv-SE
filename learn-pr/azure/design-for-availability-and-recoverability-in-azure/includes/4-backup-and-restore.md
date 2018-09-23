Säkerhetskopiering är den slutgiltiga och mest kraftfulla försvarslinjen mot permanent dataförlust. En effektiv strategi för säkerhetskopiering kräver mer än att bara kopiera data. Den behöver ha ditt programs dataarkitektur och infrastruktur i åtanke. Din app kan hantera många typer av data av olika betydelse, ofta fördelade över filsystem, databaser och andra lagringstjänster både i molnet och lokalt. Med rätt tjänster och produkter för jobbet förenklas säkerhetskopieringsprocessen och återställningstiden förbättras om en säkerhetskopia behöver återställas.

## <a name="establish-backup-and-restoration-requirements"></a>Etablera krav för säkerhetskopiering och återställning

Precis som med en strategi för haveriberedskap baseras säkerhetskopieringskrav på en kostnadsfördelsanalys. Analys av din apps data bör vägledas av den relativa vikten av de olika kategorierna av data som appen hanterar samt externa krav som lagar för datakvarhållning.

Om du vill etablera säkerhetskopieringskrav för appen, grupperar du din apps data baserat på följande krav:

* Hur mycket av den här typen av data du har råd att förlora, mätt i tid
* Den längsta tiden som en återställning av den här typen av data skulle kräva
* Krav på kvarhållning av säkerhetskopior, hur länge och ofta säkerhetskopior ska vara tillgängliga

De här begreppen mappas smidigt till begreppen inom mål för återställningspunkt (RPO) och mål för återställningstid (RTO). Varaktigheten för godtagbar dataförlust översätts vanligen direkt till nödvändiga säkerhetskopieringsintervaller och RPO. Den längsta tid en säkerhetskopiering tar motsvarar RTO:n för programmets datakomponent. Bägge kraven bör utvecklas i förhållande till kostnaden för att uppnå dem. Varje organisation skulle vilja säga att de inte har råd att förlora *några* data, men ofta är det inte fallet när kostnaden för att uppnå det kravet övervägs.

Säkerhetskopiering spelar absolut roll vid haveriberedskap (DR), men säkerhetskopior, återställningar och deras associerade scenarier sträcker sig längre än haveriberedskapens omfattning. Säkerhetskopior kan behöva återställas i icke-katastrofsituationer, däribland sådana där RTO och RPO inte är viktiga. Om exempelvis en liten mängd data som är äldre än ditt säkerhetskopieringsintervall har skadats eller tagits bort, men programmet inte drabbas av driftavbrott, kanske aldrig ditt program riskerar att missa sitt serviceavtal, och en lyckad återställning leder inte till någon dataförlust. Din haveriberedskapsplan kanske eller kanske inte omfattar vägledning om att utföra återställningar i icke-katastrofsituationer.

> [!TIP]
> Blanda inte ihop *arkivering*, *replikering* och *säkerhetskopiering*. Arkivering är lagring av data för långsiktigt bevarande och läsbehörighet. Replikering är datakopiering i så gott som realtid mellan repliker för att stödja hög tillgänglighet och vissa haveriberedskapsscenarier. Vissa förutsättningar, till exempel lagar för kvarhållning av data, kan påverka dina strategier för alla tre av dessa problem. Arkivering, replikering och säkerhetskopiering kräver alla separat analys och implementering.

## <a name="azure-backup-and-restore-capabilities"></a>Funktioner för säkerhetskopiering och återställning i Azure

Azure erbjuder flera säkerhetskopieringsrelaterade tjänster och funktioner för olika scenarier, inklusive data i Azure och lokala data. De flesta Azure-tjänster innehåller någon typ av säkerhetskopieringsfunktioner. Här tar vi en titt på några av de populäraste Azure-erbjudandena som relaterar till säkerhetskopiering.

### <a name="azure-backup"></a>Azure Backup

Azure Backup är en familj med säkerhetskopieringsprodukter som säkerhetskopierar data till Azure Recovery Services-valv för lagring och återställning. Recovery Service-valv är lagringsresurser i Azure som är dedikerade för att innehålla data- och konfigurationssäkerhetskopiering för virtuella datorer, servrar och individuella arbetsstationer och arbetsbelastningar.

> [!NOTE]
> I både Azure Backup och Azure Site Recovery används Azure Recovery Service-valv till lagring. Azure Backup är en allmän säkerhetskopieringslösning. Azure Site Recovery kan samordna replikering och redundans och stödja åtgärder för haveriberedskap vid låg RPO och RTO.

Azure Backup fungerar som en allmän lösning för säkerhetskopiering för molnet och lokala arbetsflöden som körs på virtuella datorer eller fysiska servrar. Det är utformat för att vara en lättillgänglig ersättning av traditionella säkerhetskopieringslösningar som lagrar data i Azure istället för arkivband eller andra lokala fysiska medier.

Fyra olika produkter och tjänster kan använda Azure Backup för att skapa säkerhetskopior:

* **Azure Backup-agenten** är ett litet Windows-program som säkerhetskopierar filer, mappar och systemtillstånd från den virtuella Windows-datorn eller servern som den är installerad på. Det fungerar på ett sätt som liknar många konsumentlösningar med molnbaserade säkerhetskopiering, men kräver konfiguration av ett Azure Recovery-valv. När du laddar ned och installerar den på en Windows-server eller virtuell dator kan du konfigurera den så att den skapar säkerhetskopior upp till tre gånger per dag.
* **System Center Data Protection Manager** är ett robust, fullständigt system för säkerhetskopiering och återställning på företagsnivå. Data Protection Manager är ett Windows Server-program som kan säkerhetskopiera filsystem och virtuella datorer (Windows and Linux), skapa Bare Metal-säkerhetskopior av fysiska servrar och utföra programmedveten säkerhetskopiering av många Microsoft-serverprodukter som SQL Server och Exchange. Data Protection Manager är en del av System Center-sortimentet med produkter och är licensierat och säljs med System Center men betraktas som en del av Azure Backup-familjen eftersom det kan lagra säkerhetskopior i ett Azure Recovery-valv.
* **Azure Backup Server** liknar Data Protection Manager, men är licensierat som en del av en Azure-prenumeration och kräver ingen System Center-licens. Azure Backup Server stöder samma funktioner som Data Protection Manager förutom lokal säkerhetskopiering på band och integrering med andra System Center-produkter.
* **Säkerhetskopiering av virtuella IaaS-datorer i Azure** är en nyckelfärdig funktion för säkerhetskopiering och återställning av Azure Virtual Machines. Säkerhetskopiering av virtuell dator stöder säkerhetskopiering av virtuella Windows- och Linux-datorer en gång per dag. Den stöder återställning av enskilda filer, hela diskar och hela virtuella datorer och kan även utföra programkonsekventa säkerhetskopior. Enskilda program kan göras medvetna om säkerhetskopieringsåtgärder och få sina filsystemresurser i ett konsekvent tillstånd innan ögonblicksbilden tas.

![En bild som visar en Azure-säkerhetskopieringsagent som konfigurerats med återställningsvalv och en virtuell Azure-datorsäkerhetskopia. Säkerhetskopieringsagenten och den virtuella Azure-datorsäkerhetskopian sparar alla data i återställningsvalvet.](../media/azure-backup.png)

Azure Backup kan ge ökat värde och bidra till strategin för säkerhetskopiering och återställning av IaaS-program och lokala program av praktiskt taget vilken storlek och form som helst.

### <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Storage inkluderar ingen automatiserad säkerhetskopieringsfunktion men blobar används ofta för att säkerhetskopiera alla typer av data från olika källor. Många tjänster som erbjuder säkerhetskopieringsfunktioner använder blobar för att lagra sina data, och blobar är ett vanligt mål för skript och verktyg i alla tänkbara scenarier för säkerhetskopiering.

V2-lagringskonton för generell användning stöder tre olika nivåer för bloblagring med varierande prestanda och kostnad. **Lågfrekvent** lagring erbjuder det bästa förhållandet mellan kostnad och prestanda för de flesta säkerhetskopieringar, till skillnad från **frekvent** lagring, som erbjuder lägre åtkomstkostnader men högre lagringskostnader. Lagring på **Arkivnivå** kan vara lämpligt för sekundära säkerhetskopieringar eller säkerhetskopior av data med låga förväntningar på återställningstid. Det har en låg kostnad, men kräver upp till 15 timmars ledtid för att få åtkomst till.

Oföränderlig bloblagring kan konfigureras för att vara icke-raderbar och icke-ändringsbar för ett användarangivet intervall. Oföränderlig bloblagring skapades främst för att uppfylla strikta krav för vissa typer av data, till exempel finansiella data. Det är ett bra alternativ för att säkerställa att säkerhetskopior är skyddade mot oavsiktlig borttagning och ändring.

### <a name="azure-sql-database"></a>Azure SQL Database

Omfattande, automatiska säkerhetskopieringsfunktioner ingår i Azure SQL Database utan extra kostnad. Fullständiga säkerhetskopieringar skapas varje vecka, och differentiella säkerhetskopieringar utförs var 12: e timme och loggsäkerhetskopior görs var femte minut. Säkerhetskopior som har skapats av tjänsten kan användas för att återställa en databas till en specifik tidpunkt, även om de har tagits bort. Återställningar kan göras med hjälp av Azure Portal, PowerShell eller REST-API:et. Säkerhetskopiering för databaser som krypterats med transparent datakryptering, som är aktiverat som standard, krypteras också.

SQL Database-säkerhetskopiering är i företagslass, är produktionsklar och aktiverad som standard. Om du utvärderar olika databasalternativ för en app bör den ingå so en del av en kostnadsanalys eftersom det är en stor fördel för tjänsten. Alla appar som använder Azure SQL Database ska dra nytta av det genom att inkludera det i sin plan för haveriberedskap och i sina procedurer för säkerhetskopiering/återställning.

### <a name="azure-app-service"></a>Azure App Service

Webbprogram i Azure App Services Standard- och Premium-nivåer stöder nyckelfärdiga schemalagda och manuella säkerhetskopieringar. Säkerhetskopieringar innefattar konfiguration och filinnehåll och innehåll i databaser som används i appen. De stöder också enkla filter för att exkludera filer. Återställningsåtgärderna kan riktas mot olika App Service-instanser, vilket gör App Service-säkerhetskopiering till ett enkelt sätt att flytta en apps innehåll till en annan app.

App Service-säkerhetskopior är begränsade till 10 GB totalt, inklusive app- och databasinnehåll. De är en bra lösning för nya appar som utvecklas, och småskaliga appar. Mognare program använder generellt sätt inte App Service-säkerhetskopiering. De förlitar sig istället på robusta procedurer för distribution och återställning, strategier som inte använder programdisklagring och dedikerade säkerhetskopieringsstrategier för databaser och beständig lagring.

## <a name="verify-backups-and-test-restore-procedures"></a>Kontrollera säkerhetskopieringar och testa återställningsprocedurerna

Inget säkerhetskopieringssystem är komplett utan någon strategi för att verifiera säkerhetskopior och testa återställningsprocedurerna. Även om du använder en dedikerad tjänst eller produkt för säkerhetskopiering bör du dokumentera och praktisera återställningsprocedurer för att garantera att de förstås väl och att de returnerar systemet till förväntat tillstånd.

Strategier för att verifiera säkerhetskopior varierar och beror på typ av infrastruktur. Du kan vilja överväga teknik som att skapa en ny distribution för programmet, återställa säkerhetskopieringen till den och jämföra två instansers tillstånd. I många fall imiterar den här tekniken faktiska återställningsprocedurer. Det räcker med att bara jämföra en delmängd av säkerhetskopierade data med livedata omedelbart efter att en säkerhetskopia har skapats. En vanlig del i att verifiera säkerhetskopiering är att försöka återställa gamla säkerhetskopior för att se om de fortfarande är tillgängliga och fungerar, och att säkerhetskopieringssystemet inte har ändrats på ett sätt som gör att de blir inkompatibla.

Alla strategier är bättre än att ta reda på att dina säkerhetskopior är skadade eller ofullständiga när du försöker återställa efter en katastrof.

En strategi för säkerhetskopiering och återställning är en viktig del av att se till att din arkitektur kan återställas från dataförlust eller skadade data. Granska din arkitektur för att definiera dina krav för säkerhetskopiering och återställning. Azure tillhandahåller flera tjänster och funktioner för att tillhandahålla säkerhetskopierings- och återställningsfunktioner för alla arkitekturer.
