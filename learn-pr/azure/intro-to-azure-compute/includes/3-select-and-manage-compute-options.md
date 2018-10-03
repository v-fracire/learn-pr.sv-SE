Nu när du har sett vilka Azure-beräkningstjänster som finns tillgängliga ska vi titta närmare på var och en för att avgöra när du bör använda de olika tjänsterna.

## <a name="azure-virtual-machines"></a>Virtuella Azure-datorer

:::row:::
  :::column:::
    ![Bild som representerar en virtuell Azure-dator](../media/3-azure-vms.png)
  :::column-end:::
  :::column span="3":::
Virtuella datorer är ett perfekt val när du behöver fullständig kontroll över operativsystemet och miljön. Du kan anpassa all programvara som körs på den virtuella datorn, precis som i en fysisk dator. Detta är särskilt användbart när du kör anpassad programvara eller anpassade värdkonfigurationer.
  :::column-end:::
:::row-end:::

Virtuella datorer är även ett utmärkt val när du flyttar från en fysisk server till molnet. Det går ofta att skapa en avbildning av den fysiska servern och ha den som värd i en virtuell dator. Detta ger dig fullständig frihet att välja operativsystem och programkörningsmiljöer, vilket innebär att du kan utveckla på nästan alla språk som använder det verktyg du väljer.

Men du kommer att behöva underhålla den virtuella datorn. Det innebär att du måste uppdatera operativsystemet och den programvara som den kör. 

Det kräver också fler överväganden vid skalning. Du kan skala upp den virtuella datorn, vilket innebär att du kan lägga till fler resurser för beräkning och minne. Men om du vill köra flera instanser parallellt eller skala ut kan du behöva lägga till ytterligare tjänster, exempelvis lastbalanserare.

Anta att du kör en webbplats som gör det möjligt för forskare att ladda upp astronomibilder som måste bearbetas. Om du duplicerade den virtuella datorn behöver du ytterligare en tjänst som dirigerar begäranden mellan flera instanser för webbplatsens virtuella dator.

Det finns även avancerade tjänster för virtuella datorer i Azure:

- Använd alternativet **Virtual Machine Scale Sets** om du behöver köra ständigt tillgängliga instanser av samma app eller uppsättningar av appar på virtuella datorer med liknande konfigurationer. Det genererar automatiskt tusentals identiska virtuella datorer som ditt program har lästs in till på några minuter, så användarna behöver aldrig vänta. Och eftersom du inte behöver etablera virtuella datorer i förväg räcker det med att använda de dataresurser som programmet behöver vid ett givet tillfälle.

- Använd **Batch**-alternativet när du behöver schemaläggning av jobb på molnskala och beräkningshantering med möjlighet att skala till tiotals, hundratals eller tusentals virtuella datorer. Det kan finnas situationer där du behöver rå beräkningskraft eller beräkningskraft på superdatornivå. Dessa funktioner kan du få med Azure.

## <a name="azure-containers"></a>Azure-containrar

:::row:::
  :::column:::
    ![Bild som representerar Azure-containrar](../media/3-azure-containers.png)
  :::column-end:::
  :::column span="3":::
Om du vill köra flera instanser av ett program på en enda virtuell dator är containrar ett utmärkt val. Containerns initierare kan starta, stoppa och skala ut programinstanser vid behov.
  :::column-end:::
:::row-end:::

#### <a name="vms-versus-containers"></a>Virtuella datorer jämfört med containrar

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuaq]

Containrar används ofta för att skapa lösningar med hjälp av en mikrotjänstarkitektur, eftersom de ofta används för att dela in lösningar i mindre delar. Du kan till exempel dela upp en webbplats i en container som är värd för din klientdel, en annan som är värd för din serverdel och en tredje för lagring. Detta innebär att du kan avgränsa delar av din app i logiska delar som kan underhållas, skalas eller uppdateras oberoende av varandra.

#### <a name="what-is-a-microservice"></a>Vad är en mikrotjänst?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yual]

Föreställ dig att serverdelen för webbplatsen har nått full kapacitet, men att klientdelen och lagringen inte är fullt utnyttjade. Du skulle kunna skala serverdelen separat för att förbättra prestanda, eller så kan du välja en annan lagringstjänst. Du kan även ersätta lagringscontainern utan att påverka resten av programmet.

Om ditt team föredrar att använda Kubernetes-containerorkestrering kan du använda alternativet **Azure Kubernetes Service (AKS)**. Det förenklar och automatiserar hantering, distribution och drift av Kubernetes-orkestreringen.

#### <a name="what-is-kubernetes"></a>Vad är Kubernetes?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuX]

## <a name="azure-functions"></a>Azure Functions

:::row:::
  :::column:::
    ![Bild som representerar Azure Functions](../media/3-azure-functions.png)
  :::column-end:::
  :::column span="3":::
Azure Functions är perfekt när du bara bryr dig om koden som kör din tjänst, och inte den underliggande plattformen eller infrastrukturen. De används ofta när du behöver utföra arbete som svar på en händelse, ofta via en REST-begäran, timer eller ett meddelande från en annan Azure-tjänst, och när arbetet kan slutföras snabbt (inom några sekunder eller ännu mindre).
  :::column-end:::
:::row-end:::

Azure-funktionerna skalas automatiskt, så de är ett utmärkt val när efterfrågan varierar. Du debiteras endast när en funktion utlöses. Du kan till exempel ta emot meddelanden från en IoT-lösning som används för att övervaka företagets leveransfordon. Förmodligen inkommer större datamängder under kontorstid.

Azure Functions är tillståndslösa, vilket innebär att de beter sig som om de har startats om varje gång de svarar på en händelse. Detta är perfekt vid bearbetning av inkommande data. Och om tillstånd krävs kan de anslutas till en Azure-lagringstjänst.
