Nu när du har introducerats till de Azure-beräkningstjänster som finns tillgängliga, ska vi titta på var och en i tur och ordning för att avgöra när du ska använda varje tjänst.

## <a name="azure-virtual-machines"></a>Virtuella Azure-datorer

När fullständig kontroll över operativsystemet och miljö krävs, är virtuella datorer ett perfekt val. Du kan anpassa alla program som körs på den virtuella datorn precis som en fysisk dator. Detta är perfekt när du kör anpassad programvara eller anpassade värdkonfigurationer.

Virtuella datorer är också ett utmärkt val när du flyttar från en fysisk server till molnet. Du kan ofta skapa en avbildning av den fysiska servern och lägga upp den på en virtuell dator. Detta ger dig fullständig frihet att välja operativsystem och programkörningsmiljöer, vilket innebär att du kan utveckla på nästan alla språk som använder det verktyg du väljer.

Men du kommer att behöva underhålla den virtuella datorn. Det innebär att uppdatera operativsystemet och den programvara som den kör. 

Det kräver också mer överväganden vid skalning. Du kan skala upp den virtuella datorn, vilket innebär att du kan lägga till fler resurser för beräkning och minne. Men om du vill köra flera instanser samtidigt kan du behöva lägga till ytterligare tjänster, exempelvis belastningsutjämnare.

Anta att du kör en webbplats som är värd för en återförsäljningswebbplats. Om du duplicerade den virtuella datorn behöver du en ytterligare tjänst för att dirigera begäranden mellan flera instanser av webbplatsens virtuella dator.

Det finns även avancerade tjänster för virtuella datorer i Azure:

* För att konfigurera virtuella datorer som kör konsekvent tillgängliga instanser av samma app eller uppsättningar av appar på virtuella datorer som är likartat konfigurerade, använder du alternativet **Skalningsuppsättningar för virtuella datorer**. Den genererar automatiskt tusentals identiska virtuella datorer som lästs in med ditt program på några minuter, så användarna behöver aldrig vänta. Och eftersom du inte behöver etablera virtuella datorer i förväg kan du använda endast de dataresurser som programmet behöver vid ett givet tillfälle.

* Det kan finnas situationer där du behöver rå datorkraft eller beräkningskraft på superdatornivå. I alternativet **Batch** finns jobbschemaläggning i molnet och beräkningshantering med möjlighet att skala till tiotals, hundratals eller tusentals virtuella datorer. Du kan till och med ange virtuella datorer med superdatorfunktioner.

## <a name="azure-containers"></a>Azure-behållare

Behållare är ett utmärkt val om du vill köra flera instanser av ett program på en enda virtuell dator. Behållarinitieraren kan starta, stoppa och skala upp instanser av programmet efter behov.

Behållare används dock ofta till att skapa lösningar med hjälp av en mikrotjänstarkitektur. Behållare används ofta för att bryta ned lösningar i mindre delar. Du kan till exempel dela upp en webbplats i en behållare som är värd för din klientdel, en annan som är värd för din serverdel och en tredje för lagring. Detta gör att du kan avgränsa delar av din app i logiska delar som kan finnas kvar, skalas eller uppdateras oberoende av varandra.

Föreställ dig att serverdelen för webbplatsen har nått full kapacitet, men klientdelen och lagringen är inte fullt utnyttjade. Du kan skala serverdelen separat om du vill förbättra prestandan. Eller du kan välja att använda en annan lagringstjänst. Du kan också ersätta lagringsbehållaren utan att påverka resten av programmet.

 Om ditt team föredrar att använda Kubernetes behållardirigering kan du använda alternativet **Azure Kubernetes Service (AKS)**. Det förenklar och automatiserar hantering, distribution och drift av Kubernetes-orkestrering.

## <a name="azure-functions"></a>Azure-funktioner

Azure-funktioner är perfekta när du endast är orolig över den kod som kör din tjänst, och inte den underliggande plattformen eller infrastrukturen. De används ofta när du behöver utföra arbete som svar på en händelse, ofta via en REST-begäran, timer eller ett meddelande från en annan Azure-tjänst, och när arbetet kan slutföras snabbt (inom några sekunder eller ännu mindre).

Azure-funktioner skalas automatiskt, så de är ett utmärkt val när begäran varierar. Du debiteras endast när en funktion utlöses. Du kan till exempel ta emot meddelanden från en IoT-lösning som används för att övervaka en flotta med leveransfordon. Förmodligen har du mer data som inkommer under kontorstid.

Azure-funktioner är tillståndslösa; de beter sig som om de har startats om varje gång de svarar på en händelse. Det är perfekt vid bearbetning av inkommande data. Och om tillstånd krävs, kan de anslutas till en Azure-lagringstjänst.
