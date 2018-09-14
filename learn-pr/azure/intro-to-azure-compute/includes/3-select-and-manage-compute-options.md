Nu när du har introducerats till Azure compute services som är tillgängliga, ska vi titta på var och en bättre för att hjälpa dig att avgöra när du ska använda varje tjänst.

## <a name="azure-virtual-machines"></a>Virtuella Azure-datorer

När du behöver fullständig kontroll över operativsystemet och miljö, är virtuella datorer ett perfekt val. Du kan anpassa alla program som körs på den virtuella datorn, precis som i en fysisk dator. Detta är särskilt perfekt när du kör anpassad programvara eller anpassade som är värd för konfigurationer.

Virtuella datorer är också ett utmärkt val när du flyttar från en fysisk server till molnet. Det går ofta att skapa en avbildning av den fysiska servern och ha den som värd i en virtuell dator. Detta ger dig fullständig frihet att välja operativsystem och programkörningsmiljöer, vilket innebär att du kan utveckla på nästan alla språk som använder det verktyg du väljer.

Men du kommer att behöva underhålla den virtuella datorn. Det innebär att du måste uppdatera operativsystemet och den programvara som den kör. 

Det kräver också fler överväganden vid skalning. Du kan skala upp den virtuella datorn, vilket innebär att du kan lägga till fler resurser för beräkning och minne. Men om du vill köra flera instanser i parallella eller skala ut kan du behöva lägga till ytterligare tjänster, till exempel belastningsutjämnare.

Anta att du kör en webbplats som gör det möjligt för forskare att överföra astronomi avbildningar som måste bearbetas. Om du duplicerade den virtuella datorn behöver du en ytterligare tjänst för att dirigera begäranden mellan flera instanser av webbplatsen VM.

Det finns även avancerade tjänster för virtuella datorer i Azure:

- Använd den **Virtual Machine Scale Sets** alternativet när du behöver köra konsekvent tillgängliga instanser av samma program eller uppsättningar med appar, på på samma sätt att konfigurera virtuella datorer. Det genererar automatiskt tusentals identiska virtuella datorer som lästs in med ditt program på några minuter, så användarna behöver aldrig vänta. Och eftersom du inte behöver etablera virtuella datorer i förväg räcker det med att använda de dataresurser som programmet behöver vid ett givet tillfälle.

- Använd den **Batch** när du behöver schemaläggning av jobb i molnskala och beräkningshantering möjlighet att skala till tiotals, hundratals eller tusentals virtuella datorer. Det kan finnas situationer där du behöver råa datorkraft eller superdator nivå beräkningskraft &mdash; Azure tillhandahåller dessa funktioner.

## <a name="azure-containers"></a>Azure-behållare

Om du vill köra flera instanser av ett program på en enskild virtuell dator är behållare ett utmärkt val. Behållarens initierare kan starta, stoppa och skala ut instanser av programmet vid behov.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

Det kanske inte är uppenbara först vad skillnaden är mellan en virtuell dator och en behållare.  Följande videoklipp försöker beskriver skillnaderna.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuaq]

Behållare används dock ofta till att skapa lösningar med hjälp av en mikrotjänstarkitektur. Behållare används ofta för att skapa lösningar med hjälp av en mikrotjänstbaserad arkitektur, eftersom de används ofta att bryta lösningar i mindre delar. Du kan till exempel dela upp en webbplats i en behållare som är värd för din klientdel, en annan som är värd för din serverdel och en tredje för lagring. Detta innebär att du kan avgränsa delar av din app i logiska delar som kan underhållas, skalas eller uppdateras oberoende av varandra.

Följande videoklipp försöker förklara vad en mikrotjänst är.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yual]

Föreställ dig serverdelen webbplatsen har nått kapaciteten men klientdelen och lagring är inte att nog betona. Du kan skala backend-servern om du vill förbättra prestanda eller du kan välja att använda en annan tjänst. Eller du kan även ersätta lagringsbehållaren utan att påverka resten av programmet.

Om ditt team föredrar att använda Kubernetes behållarorkestrering, kan du använda alternativet **Azure Kubernetes Service (AKS)**. Det förenklar och automatiserar hantering, distribution och drift av Kubernetes-orkestreringen.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuX]

## <a name="azure-functions"></a>Azure-funktioner

Azure Functions är idealiska när du är orolig över bara den kod som körs på din tjänst, och inte den underliggande plattformen eller infrastruktur. De används ofta när du behöver utföra arbete som svar på en händelse, ofta via en REST-begäran, timer eller ett meddelande från en annan Azure-tjänst, och när arbetet kan slutföras snabbt (inom några sekunder eller ännu mindre).

Azure Functions skalas om automatiskt, så att de blir en heldragen val när begäran variabel, och du debiteras endast när en funktion utlöses. Du kan till exempel ta emot meddelanden från en IoT-lösning som används för att övervaka företagets budbilar. Förmodligen inkommer fler data under kontorstid.

Azure Functions är dessutom tillståndslösa; de beter sig som om de har startats om varje gång de svarar på en händelse. Det är perfekt vid bearbetning av inkommande data. Och om tillstånd krävs kan de anslutas till en Azure-lagringstjänst.
