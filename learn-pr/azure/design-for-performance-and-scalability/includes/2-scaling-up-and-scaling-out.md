Det är ovanligt att vi kan förutse den exakta belastningen i systemet: användningen av de offentliga programmen kan öka snabbt eller ett internt program kan behöva hantera ett större antal användare när verksamheten växer. Även om vi kan förutse belastningen är den sällan konstant: återförsäljarna har större efterfrågan under helger och sportwebbplatserna har högst belastning under slutspel. Här definierar vi begreppen _skala upp/ned_ och _skala ut/in_, berättar om hur Azure kan förbättra dina skalningsmöjligheter på olika sätt och tittar på hur serverfria tekniker och containertekniker kan förbättra arkitekturens möjligheter till skalning.

## <a name="what-is-scaling"></a>Vad är skalning?

_Skalning_ är en process för att hantera dina resurser och se till att programmen uppfyller en uppsättning prestandakrav.  När vi har för många resurser som nyttjas av användare används de inte effektivt och det kostar pengar. För få tillgängliga resurser innebär att prestanda för programmet kan påverkas. Målet är att uppfylla våra definierade prestandakrav och samtidigt optimera kostnaderna. 

”_Resurser_” kan referera till allt som behövs för att köra programmen. Minne och processor för virtuella datorer är de mest uppenbara resurserna, men vissa Azure-tjänster kräver att du även beaktar bandbredd eller abstrakta element som enheter för programbegäran i Cosmos DB.

I en värld där programefterfrågan är konstant är det enkelt att förutse vilka resurser som krävs. I verkligheten växlar kraven på programmen över tid och den nivå som krävs blir då svårare att förutse. Om du har tur kan förändringarna vara förutsebara eller säsongsberoende, men det är inte det vanligaste för många program. Det mest optimala är att etablera rätt mängd resurser för att möta efterfrågan och göra justeringar då efterfrågan ändras.

Skalning är svårt i ett lokalt scenario då du köper och hanterar dina egna servrar. Det kan bli dyrt att lägga till resurser och det tar ofta för lång tid att etablera dem online, ibland längre tid än det faktiska behovet av ökad kapacitet. Det kan vara lika svårt att sedan minska kapaciteten under perioder med låg belastning på systemet, och då står du där med en ökad kostnad.

Enkel skalning är en viktig fördel med Azure. Med de flesta Azure-resurser kan du enkelt lägga till eller ta bort resurser i takt med en ändrad efterfrågan, och många tjänster har automatiserade alternativ för övervakning av efterfrågan och justeras därefter automatiskt. Med funktionen för automatisk skalning, som ofta kallas autoskalning, kan du ange tröskelvärden för lägsta och högsta nivå av instanser som ska vara tillgängliga, och instanser läggs till eller tas bort baserat på prestandamått (till exempel CPU-användning).

## <a name="what-is-scaling-up-or-down"></a>Vad innebär det att skala upp och ned?

Skala upp är en process där vi ökar kapaciteten för en viss instans. En virtuell dator kan utökas från 1 virtuell processor och 3,5 GB RAM-minne till 2 virtuella processorer och 7 GB RAM-minne för att ge mer bearbetningskapacitet. På motsvarande sätt innebär processen att skala ned att kapaciteten minskas för en viss instans. Om du till exempel minskar kapaciteten för en virtuell dator från 2 virtuella processorer och 7 GB RAM-minne till 1 virtuell processor och 3,5 GB RAM-minne minskar både kapaciteten och kostnaden. Följande bild visar ett exempel på att ändra storlek på en virtuell dator.

![En bild som visar uppskalning och Nedskalning av en virtuell dator för att ändra prestandafunktioner.](../media/2-ScaleUpDown.png)

Låt oss ta en titt på vad skala upp eller skala ned kan innebära för Azure-resurser:

- På virtuella Azure-datorer skalar du baserat på en VM-storlek. Den storleken associeras med en viss mängd virtuella processorer, RAM-minne och lokal lagring. Vi kan exempelvis skala upp från en virtuell dator på Standard_DS1_v2 (1 virtuell processor och 3,5 GB RAM-minne) till en virtuell dator på Standard_DS2_v2 (2 virtuella processorer och 7 GB RAM-minne).
- Azure SQL Database är en plattform som en tjänst-implementering (PaaS) av Microsoft SQL Server.  Du kan skala upp en databas baserat på antalet databastransaktionsenheter (DTU) eller virtuella processorer. DTU:er är en abstraktion av underliggande resurser och är en blandning av CPU, IO och minne. Du kan skala din databasinstans från 500 DTU:er till 250 DTU:er.
- Azure App Service är en värdtjänst för PaaS-webbplatsen i Azure. Webbplatserna körs på en virtuell servergrupp, som även kallas App Service-plan. Du kan skala upp eller ned App Service-planen mellan nivåerna och har kapacitetsalternativ inom nivåerna. En S1-App Service-plan har till exempel 1 virtuell processor och 1,75 GB RAM-minne per instans. Vi kan skala upp till en S2-App Service-plan som har 2 virtuella processorer och 3 GB RAM-minne per instans.

För att få dessa funktioner i en lokal miljö skulle du normalt behöva vänta på inköp av den nödvändiga maskinvaran och installationen innan du kan börja använda den nya skalningsnivån. I Azure har de fysiska resurserna redan distribuerats och är tillgängliga för dig. Du behöver bara välja den alternativa skalningsnivå som du vill använda.

Du kan behöva ta hänsyn till effekterna av att skala upp i din lösning, beroende på vilka molntjänster som du har valt.

Om du till exempel väljer att skala upp i Azure SQL Database hanterar tjänsten uppskalning av enskilda noder och fortsätter driften av tjänsten. När du ändrar servicenivån och/eller prestandanivån för en databas skapas en replik av den ursprungliga databasen på den nya prestandanivån och sedan växlas anslutningar över till repliken. Inga data förloras under den här processen och det blir bara ett kort avbrott (vanligtvis mindre än fyra sekunder) när tjänsten växlar över till repliken.

Om du vill skala upp eller ned en virtuell dator kan du även göra det genom att välja en annan instansstorlek. I de flesta fall kräver detta en omstart av den virtuella datorn, så det är bäst att förvänta sig att en omstart krävs och att du måste ha det i åtanke när du utför den här åtgärden.

Slutligen bör du alltid överväga att skala ned när detta är ett alternativ. Om ditt program ger tillfredsställande prestanda på en lägre prisnivå, kan Azure-fakturan minskas avsevärt.

## <a name="what-is-scaling-out-or-in"></a>Vad innebär det att skala ut och in?

Medan en skalning upp och ned justerar mängden resurser en instans har tillgängliga, innebär skalning ut och in att det totala antalet instanser justeras.

_Skala ut_ är en process där fler instanser läggs till för att ge stöd för belastningen i din lösning. Om klientdelen av webbplatsen till exempel finns på virtuella datorer så kan vi öka antalet virtuella datorer om belastningen ökar.

_Skala in_ är en process som innebär att ta bort instanser som inte längre behövs för att ge stöd för belastningen i din lösning. Om webbplatsens klientdelar har låg belastning kan vi minska antalet instanser för att minska kostnaderna. Följande bild visar ett exempel på hur du ändrar antalet instanser av virtuella datorer.


![En bild som visar att skala ut resurser för att hantera begäran och skalning i resurser för att minska kostnaderna.](../media/2-ScaleInOut.png)

Här följer några exempel på vad skala ut eller skala in innebär i samband med Azure-resurser:

- För infrastrukturlagret skulle du förmodligen använda VM-skalningsuppsättningar för att automatisera tillägg och borttagningar av extra instanser.
  - Med VM-skalningsuppsättningar kan du skapa och hantera grupper med identiska och belastningsutjämnade virtuella datorer.
  - Antal VM-instanser kan automatiskt öka eller minska som svar på efterfrågan eller ett definierat schema.
- I en Azure SQL Database-implementering kan du fördela belastningen över databasinstanser med hjälp av horisontell partitionering. _Horisontell partitionering_ är en teknik som används för att distribuera stora mängder identiskt strukturerade data över ett antal oberoende databaser.
- I Azure App Service är App Service-planen den virtuella webbservergruppen som är värd för ditt innehåll. Skala ut innebär på detta sätt att du ökar antalet virtuella datorer i servergruppen. Precis som med VM-skalningsuppsättningar kan antalet instanser automatiskt ökas eller minskas som svar på vissa mått eller enligt ett schema.

Utskalning utförs vanligtvis enkelt på Microsoft Azure-portalen, med kommandoradsverktyg eller med Resource Manager-mallar, och är i de flesta fall inte märkbar för slutanvändaren.

### <a name="autoscale"></a>Automatisk skalning

Du kan konfigurera vissa av dessa tjänster för användning av en funktion som kallas autoskalning. Med autoskalning behöver du inte längre tänka på att skala tjänsterna manuellt. I stället kan du ange ett lägsta och ett högsta tröskelvärde för instanser och skala baserat på specifika mått (kölängd, CPU-användning) eller scheman (vardagar mellan 17:00 och 19:00). Följande bild visar hur funktion skala automatiskt hanterar instanser för att hantera belastningen.

![En bild som visar hur funktioner för autoskalning övervakar CPU-nivåer av en pool med virtuella datorer och lägger till instanser när CPU-användningen är över tröskeln.](../media/2-autoscale.png)

### <a name="considerations-when-scaling-in-and-out"></a>Att tänka på när du skalar in och ut

Vid utskalning kan starttiden för ditt program påverka hur snabbt ditt program kan skalas. Om webbappen tar två minuter att starta och bli tillgänglig för användarna innebär det att det tar två minuter för var och en av dina instanser innan de blir tillgängliga för användarna. Du bör beakta den här starttiden när du bestämmer hur snabbt du vill skala.

Du måste också tänka på hur programmet hanterar tillstånd. När programmet skalas in är tillstånd som lagrats på datorn inte längre tillgängliga. Om en användare ansluter till en instans som inte har tillgång till tillståndet, kan de bli tvingade att logga in, eller välja data på nytt, vilket ger en sämre användarupplevelse. En vanlig metod är att lagra tillstånd externt i en annan tjänst som exempelvis Redis Cache eller SQL Database, vilket gör webbservrarna tillståndslösa. Nu när klientdelarna av webben är tillståndslösa behöver vi inte bekymra oss om vilka enskilda instanser som är tillgängliga. De gör alla samma jobb och distribueras på samma sätt.

## <a name="throttling"></a>Begränsning

Vi har konstaterat att belastningen på ett program varierar över tid. Detta kan bero på antalet aktiva eller samtidiga användare och de aktiviteter som utförs. Vi kan använda autoskalning för att lägga till kapacitet och vi kan också använda en mekanism för bandbreddsbegränsning som begränsar antalet begäranden från en källa. Vi kan skydda prestandabegränsningarna genom att ange kända begränsningar på programnivå, vilket hindrar programmet från att avbrytas. Bandbreddsbegränsning används oftast i program som exponerar API-slutpunkter.

När programmet har identifierat att det inte överträder en gräns kan begränsning påbörjas och säkerställa att det övergripande systemserviceavtalet inte överträds. Om vi till exempel har exponerat en API för kunderna för att hämta data kan vi begränsa antalet begäranden till 100 per minut. Om en enda kund överskrider denna gräns kan vi svara med statuskoden HTTP 429, inklusive väntetiden innan en annan begäran kan skickas.

## <a name="serverless"></a>Utan server

Serverfri databehandling ger en molnbaserad körningsmiljö som kör dina appar men där den underliggande miljön är helt abstrakt. Du skapar en instans av tjänsten och lägger till din kod, men du behöver inte, och kan inte, hantera eller underhålla någon infrastruktur.

Du konfigurerar dina serverfria appar för att svara på händelser. Det kan gälla en REST-slutpunkt, en timer eller ett meddelande som tas emot från en annan Azure-tjänst. Den serverfria appen körs bara när den utlöses av en händelse.

Du ansvarar inte för infrastrukturen. Skalning och prestanda hanteras automatiskt och du debiteras endast för de resurser du använder. Du behöver inte ens reservera kapacitet. Azure Functions, Azure Container Instances och Logic Apps är exempel på serverlös databehandling som är tillgänglig på Azure.

Nu ska vi gå tillbaka till exemplet med Lamna Healthcare. Det kan finnas en viss potential för att spara kostnader och förenkla hanteringen. Överväg en API-slutpunkt. I stället för att ha Azure App Service som värd för API:et, där de måste betala för reserverad kapacitet, kan de använda en Azure Funktionsapp som utlöses av en HTTP-begäran. Azure Functions skulle göra det möjligt för teamet att endast betala för de resurser man behöver för att bearbeta respektive transaktion. Kostnaden och skalningen skulle vara i direkt linje med antalet transaktioner i systemet.

## <a name="containers"></a>Containrar

En container är en metod som kör program i en virtualiserad miljö. En virtuell dator är virtualiserad på maskinvarunivå, där ett hypervisor-program gör det möjligt att köra flera virtualiserade operativsystem på en enda fysisk server. Containrar tar virtualiseringen till nästa nivå. Virtualiseringen utförs på operativsystemsnivå, vilket gör det möjligt att köra flera identiska programinstanser inom samma OS.

Containrar passar mycket bra för utskalningsscenarier. De är menade att vara enkla och utformade för att kunna skapas, skalas ut och stoppas dynamiskt när miljön och behoven ändras.

En fördel med att använda containrar är att du kan köra flera isolerade program på en virtuell dator. Eftersom containrar är skyddade och isolerade i sig själva på kernelnivå behöver du inte nödvändigtvis separata virtuella datorer för olika arbetsbelastningar.

Du kan köra containrar på virtuella datorer. Det finns ett par Azure-tjänster som fokuserar på att underlätta hantering och skalning av containrar:

- **Azure Kubernetes Service (AKS)**

  Med Azure Kubernetes Service kan du konfigurera virtuella datorer så att de fungerar som noder. Azure är värd för Kubernetes hanteringsplan och fakturerar enbart för de arbetsnoder som körs som värdar för dina containrar.

  För att öka antalet arbetsnoder i Azure kan du använda Azure CLI och öka antalet manuellt. När detta skrivs finns det en förhandsversion av Cluster Autoscaler på AKS tillgänglig som möjliggör autoskalning av arbetsnoder. I Kubernetes-klustret kan du använda Horizontal Pod Autoscaler för att skala ut antalet instanser av containern som ska distribueras.

  AKS kan även skalas med Virtual Kubelet som beskrivs nedan.

- **Azure Container Instances (ACI)**
  
  Azure Container Instances är en serverfri metod som gör att du kan skapa och köra containrar på begäran. Du debiteras endast för körningstid per sekund.

  Du kan använda Virtual Kubelet för att ansluta Azure Container Instances till Kubernetes-miljön, inklusive AKS. När Kubernetes-klustret kräver ytterligare containerinstanser kan du med Virtual Kubelet uppfylla dessa krav från ACI. Eftersom ACI är serverfri finns det inget behov av att ha reserverad kapacitet. Du kan därför dra nytta av kontrollen och flexibiliteten i Kubernetes-skalning med fakturering per sekund utan server. När detta skrivs betecknas Virtual Kubelet som experimentell programvara och ska inte användas i produktionsscenarier.

## <a name="scaling-at-lamna-healthcare"></a>Skalning på Lamna Healthcare

Lamna Healthcare har ett system för hantering och tidsbokning av patienter. Hanteringssystemet sköter tidsbokningar och patientjournaler för dussintals sjukhus och medicinska kliniker. Den lokala sjukvårdstjänsten körs med full kapacitet och ingen tillväxt förväntas för närvarande. Systemet körs på en PHP-webbplats i Azure App Service.

Belastningsmönstret för programmet är förutsägbart, eftersom det främst är igång måndag till fredag mellan 9 och 17.  Från tisdag till fredag hanteras i genomsnitt 1 200 transaktioner per timme i hela systemet. Under helgerna hanteras 500 transaktioner per timme. Efter helgens lugn ökar trafiken på måndagar med ett genomsnitt på 2 000 transaktioner per timme.

Programmet finns på en S1-App Service-plan, men driftteamet har märkt av en hög nivå av CPU-användning (mer än 95 %) på alla instanser. Den höga användningen påverkar bearbetnings- och inläsningstiderna i programmet. I en molnmiljö är det inte nödvändigtvis något dåligt att resurser används i hög grad. Det innebär att de får valuta för sina pengar, eftersom resurserna som har distribuerats utnyttjas väl. 

Teamet bestämmer sig för att _skala upp_ App Service-plannivån för de distribuerade instanserna från S1 (1 virtuell processor och 1,75 GB RAM-minne) till S2 (2 virtuella processorer och 3 GB RAM-minne). Detta åstadkoms enkelt med hjälp av Microsoft Azure-portalen, men de kan även uppnå samma sak med hjälp av ett enda kommando i Azure CLI, Azure PowerShell eller med hjälp av Resource Manager-mallar.

Teamet beslutar att de vill automatisera antalet distribuerade instanser baserat på ett schema, eftersom deras belastningsprofil är förutsägbar. De konfigurerar schemat för autoskalning av App Service-planen. Anta att två instanser är tillräckligt för att hantera 500 transaktioner per timme. Teamet kan sedan skala till sex instanser för tisdag till fredag och åtta instanser för måndagen för att uppfylla kraven (baserat på övervakning och insikter av belastningstester).

Automatisk skalning ger även den extra fördelen att ha beredskap inför oförutsedda scenarier. Webbplatsen kan plötsligt få högre belastning under en helg (fler avtalade tider under vintersäsongen på grund av förkylningar och influensa). Teamet kan ställa in automatisk skalning för att öka med en instans när CPU-användningen ligger över 90 % och minska med en instans när användningen går under 15 %.

Teamet har använt mönstret för bandbreddsbegränsning för den patientboknings-API som exponerats bakom en Azure API Management-instans. Detta förhindrar att systemet får sämre prestanda genom att bara tillåta en viss mängd dataflöde genom systemet.

## <a name="summary"></a>Sammanfattning

Vi har pratat om att skala upp och ned och skala in och ut och hur du kan utnyttja de här alternativen i din arkitektur. Vi har även tittat på hur serverfria tekniker och containrar kan utveckla dina skalningsmöjligheter. Nu ska vi ta en titt på hur nätverksprestanda kan påverka ditt program, och olika sätt att optimera nätverket.