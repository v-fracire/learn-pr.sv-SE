Att utföra en migrering av lokala servrar till Azure kräver planering och noggrannhet. Du kan flytta dem alla samtidigt eller (mer sannolikt) i små batchar eller rentav en och en. Innan du skapar en enskild virtuell dator bör du skapa en skiss av den aktuella infrastrukturmodellen och se hur den skulle kunna mappas till molnet.

#### <a name="required-resources-for-iaas-virtual-machines"></a>Resurser som krävs för virtuella IaaS-datorer

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWjVUg]

Vi går igenom en checklista med saker att tänka på.

- Börja med nätverket
- Namnge den virtuella datorn
- Välja plats för den virtuella datorn
- Välja storlek på den virtuella datorn
- Förstå prismodellen
- Lagring för den virtuella datorn
- Välja ett operativsystem

## <a name="start-with-the-network"></a>Börja med nätverket

Det första du bör tänka på är inte den virtuella datorn – det är nätverket.

Virtuella nätverk (VNet) används i Azure för privata anslutningar mellan virtuella Azure-datorer och andra Azure-tjänster. Virtuella datorer och tjänster som ingår i samma virtuella nätverk har åtkomst till varandra. Som standard kan tjänster utanför det virtuella nätverket inte ansluta till tjänster i det virtuella nätverket. Du kan dock konfigurera nätverket för att tillåta åtkomst till den externa tjänsten, inklusive dina lokala servrar.

Den senare aspekten är anledningen till att du noggrant bör överväga nätverkskonfigurationen. Nätverksadresser och undernät är inte helt enkla att ändra när du väl har konfigurerat dem, och om du planerar att ansluta företagets privata nätverk till Azure-tjänster bör du ta hänsyn till topologin innan du etablerar några virtuella datorer.

När du installerar ett virtuellt nätverk anger du tillgängliga adressutrymmen, undernät och säkerhet. Om det virtuella nätverket ska anslutas till andra virtuella nätverk måste du välja adressintervall som inte överlappar varandra. Detta är det intervall med privata adresser som de virtuella datorerna och tjänsterna i nätverket kan använda. Du kan använda ej rountningsbara IP-adresser såsom 10.0.0.0/8, 172.16.0.0/12 eller 192.168.0.0/16 eller definiera egna intervall. Azure behandlar alla adressintervall som en del av det privata VNet:ets IP-adressutrymme om det endast kan nås inom VNet, inom sammankopplade VNets och från din lokala plats. Om någon annan ansvarar för de interna nätverken bör du samarbeta med den personen innan du väljer adressutrymme för att kontrollera att det inte finns någon överlappning och meddela om vilket utrymme du vill använda, så att den personen inte försöker använda samma IP-adressintervall.

### <a name="segregate-your-network"></a>Särskilja nätverket

När du har valt adressutrymmen för det virtuella nätverket kan du skapa ett eller flera undernät för det virtuella nätverket. Du gör detta för att dela in nätverket i mer hanterbara avsnitt. Du kan till exempel tilldela 10.1.0.0 till virtuella datorer, 10.2.0.0 till serverdelstjänster och 10.3.0.0 till virtuella SQL Server-datorer.

> [!NOTE]
> Azure reserverar de fyra första adresserna och den sista adressen i varje undernät för dess användning.

### <a name="secure-the-network"></a>Skydda nätverket

Som standard finns ingen säkerhetsgräns mellan undernät, så tjänster i vart och ett av dessa undernät kan kommunicera med varandra. Du kan dock konfigurera nätverkssäkerhetsgrupper (NSGs), så att du kan styra trafikflödet till och från undernät och till och från virtuella datorer. NSG:er fungerar som programvarubrandväggar. De tillämpar anpassade regler för varje inkommande eller utgående begäran på nätverksgränssnitts- och undernätsnivå. På så sätt kan du styra varje nätverksbegäran som kommer in eller skickas ut från den virtuella datorn.

## <a name="plan-each-vm-deployment"></a>Planera distribution av varje virtuell dator

När du har klargjort dina kommunikations och nätverkskrav kan börja du tänka på de virtuella datorer som du vill skapa. En bra plan är att välja en server och undersöka följande punkter:

- Vad kommunicerar servern med?
- Vilka portar är öppna?
- Vilket operativsystem används?
- Hur mycket diskutrymme används?
- Vilken typ av data använder detta? Finns det begränsningar (juridiska eller andra) förknippade med att den inte är lokal?
- Vilken typ av processor, minne och I/O-diskbelastning har servern? Finns det burst-trafik att ta hänsyn till?

Vi kan sedan börja besvara några frågor som Azure har för en ny virtuell dator.

### <a name="name-the-vm"></a>Namnge den virtuella datorn

En sak som det ofta inte läggs så stor vikt vid är **namnet** för den virtuella datorn. Den virtuella datorns namn används som datornamn, vilket konfigureras som en del av operativsystemet. Du kan ange ett namn på upp till 15 tecken för en virtuell Windows-dator och 64 tecken för en virtuell Linux-dator.

Det här namnet definierar även en hanterbar **Azure-resurs**, och det är inte helt enkelt att ändra senare. Det betyder att du bör välja namn som är meningsfulla och konsekventa så att du lätt kan identifiera vad den virtuella datorn gör. En bra regel är att inkludera följande information i namnet:

| Element | Exempel | Anteckningar |
| --- | --- | --- |
| Miljö |dev, prod, QA |Identifierar miljön för resursen |
| Plats |uw (USA, västra), ue (USA, östra) |Identifierar den region som resursen distribueras till |
| Instans |01, 02 |För resurser som har mer än en namngiven instans (webbservrar osv.) |
| Produkt eller tjänst |tjänst |Identifierar den produkt, det program eller den tjänst som resursen har stöd för |
| Roll |sql, webb, meddelanden |Identifierar rollen för den associerade resursen | 

Till exempel kan `devusc-webvm01` representera den första utvecklingswebbservern som finns på platsen USA, södra centrala. 

#### <a name="what-is-an-azure-resource"></a>Vad är en Azure-resurs?

En **Azure-resurs** är ett hanterbart objekt i Azure. Precis som en fysisk dator i datacentret har virtuella datorer flera element som behövs för att de ska kunna utföra sitt arbete:

- Själva den virtuella datorn
- Lagringskonto för diskarna
- Virtuellt nätverk (delat med andra virtuella datorer och tjänster)
- Nätverksgränssnitt för att kommunicera på nätverket
- Nätverkssäkerhetsgrupper för att skydda nätverkstrafiken
- Offentlig Internet-adress (valfritt)

Azure skapar alla dessa resurser vid behov, eller så kan du ange befintliga resurser som en del av distributionsprocessen. Varje resurs måste ha ett namn som används för att identifiera den. Om Azure skapar resursen används den virtuella datorns namn för att generera ett resursnamn – ytterligare en orsak att vara mycket konsekvent med dina VM-namn!

### <a name="decide-the-location-for-the-vm"></a>Välja plats för den virtuella datorn.

Azure har datacenter över hela världen fyllda med servrar och diskar. Dessa datacenter är grupperade i geografiska _regioner_ (”USA, västra”, ”Europa, norra”, ”Sydostasien” osv.) för att ge redundans och tillgänglighet.

När du skapar och distribuerar en virtuell dator måste du välja en region där du vill att resurserna (CPU, lagring osv.) ska allokeras. På så sätt kan du placera dina virtuella datorer så nära användarna som möjligt för att förbättra prestanda och för att uppfylla juridiska krav, efterlevnadskrav eller skattemässiga krav.

Två andra saker att tänka på angående platsvalet. Först och främst kan platsen begränsa dina tillgängliga alternativ. Varje region har olika tillgänglig maskinvara och vissa konfigurationer är inte tillgängliga i alla regioner. För det andra finns det prisskillnader mellan platser. Om din arbetsbelastning inte är knuten till en viss plats, kan det vara mycket kostnadseffektivt att kontrollera konfigurationen i flera regioner för att hitta det lägsta priset.

### <a name="determine-the-size-of-the-vm"></a>Välja storlek på den virtuella datorn

När du har valt namn och plats måste du bestämma storleken på den virtuella datorn. I stället för att välja beräkningskapacitet, minne och lagringskapacitet oberoende av varandra tillhandahåller Azure olika _VM-storlekar_ som erbjuder varianter av dessa element i olika storlekar. Azure tillhandahåller ett brett utbud av VM-storleksalternativ så att du kan välja den lämpligaste kombinationen av beräkning, minne och lagring för det du vill utföra.

Det bästa sättet att fastställa rätt VM-storlek är att tänka på vilken typ av arbetsbelastning den virtuella datorn ska köra. Baserat på arbetsbelastningen kan du välja bland en delmängd av tillgängliga VM-storlekar. Alternativ för arbetsbelastningar klassificeras på följande sätt i Azure:

| Alternativ              | Beskrivning |
|---------------------|-------------|
| **Generell användning** | Virtuella datorer för generell användning är avsedda att ha ett balanserat förhållande CPU-till-minne. Utmärkt för testning och utveckling, små till medelstora databaser och webbservrar med låg till medelhög trafik. |
| **Beräkningsoptimerad** | Beräkningsoptimerade virtuella datorer är avsedda att ha ett högt förhållande CPU-till-minne. Lämpliga för webbservrar med medelhög trafik, nätverkstillämpningar, batchprocesser och programservrar. |
| **Minnesoptimerad** | Minnesoptimerade virtuella datorer är avsedda att ha ett högt förhållande minne-till-CPU. Utmärkt för relationsdatabasservrar, mellanstora till stora cacheminnen och minnesinterna analyser. |
| **Lagringsoptimerad** | Lagringsoptimerade virtuella datorer är utformade för att ha högt diskgenomflöde och I/O. Perfekt för virtuella datorer som kör databaser. |
| **GPU** | Virtuella GPU-datorer är specialiserade virtuella datorer som är avsedda för krävande grafikrendering och videoredigering. Dessa virtuella datorer är ett perfekt alternativ för modellträning och inferensjobb med djupinlärning. |
| **Databehandling med höga prestanda** | Databehandling med höga prestanda är virtuella datorer med snabbaste och mest kraftfulla processorerna med valfria nätverksgränssnitt för högt genomflöde. |

Du kan filtrera på arbetsbelastningstyp när du konfigurerar VM-storlek i Azure. Den storlek du väljer påverkar direkt kostnaden för tjänsten. Ju mer processorkraft, minne och GPU du behöver, desto högre blir prisnivån.

### <a name="what-if-my-size-needs-change"></a>Vad händer om min storlek måste ändras?

Azure låter dig ändra virtuell datorstorlek när den befintliga storleken inte längre uppfyller dina behov. Du kan uppgradera eller nedgradera den virtuella datorn – så länge din aktuella maskinvarukonfiguration tillåts i den nya storleken. Detta ger en smidig och fullständigt elastisk metod för hantering av virtuella datorer.

Virtuell datorstorlek kan ändras medan den virtuella datorn är igång så länge den nya storleken är tillgänglig i det aktuella maskinvaruklustret som den virtuella datorn körs på. Azure-portalen tydliggör det genom att endast visa tillgängliga storleksalternativ. Kommandoradsverktyget rapporterar ett fel om du försöker ändra storlek på en virtuell dator till en storlek som inte är tillgänglig. Om du ändrar storlek för en virtuell dator som körs så startas datorn om automatiskt för att slutföra begäran.

Om du stoppar och frigör den virtuella datorn så kan du sedan välja valfri tillgänglig storlek i din region eftersom den virtuella datorn tas bort från det kluster den kördes på.

> [!WARNING]
> Var försiktig om du ändrar storlek på virtuella datorer i produktion – de kommer att startas om automatiskt vilket kan orsaka ett tillfälligt avbrott och ändra vissa konfigurationsinställningar som IP-adress.

### <a name="understanding-the-pricing-model"></a>Förstå prismodellen

Det finns två separata kostnader som prenumerationen kommer att debiteras för varje virtuell dator: beräkning och lagring. Genom att skilja dessa kostnader åt så skalar du dem oberoende av varandra och betalar bara för det du behöver.

**Compute-kostnader** – Compute-utgifter prissätts per timme men debiteras per minut. Till exempel debiteras du bara för 55 minuters användning om den virtuella datorn har körts i 55 minuter. Du debiteras inte för beräkningskapacitet om du stoppar och frigör den virtuella datorn eftersom detta frigör maskinvaran. Priset per timme varierar beroende på VM-storleken och det operativsystem du väljer. Kostnaden för en virtuell dator innefattar priset för Windows-operativsystemet. Linux-baserade instanser är billigare eftersom de inte medför någon licensavgift för operativsystemet.

> [!TIP]
> Du kan kanske spara pengar genom att återanvända befintliga licenser för Windows med **Azure Hybrid-förmånen**.

**Lagringskostnader** – Du debiteras separat för den lagring som den virtuella datorn använder. Status för den virtuella datorn har ingen relation till de lagringsdebiteringar som sker; även om den virtuella datorn stoppas/frigörs och du inte debiteras för den virtuella dator som körs kommer du att debiteras för det lagringsutrymme som används av diskarna.

Du kan välja mellan två betalningsalternativ för beräkningskostnaderna.

| Alternativ | Beskrivning |
|--------|-------------|
| **Betala per användning** | Med alternativet **betala per användning** kan du betala för beräkningskapaciteten per sekund, utan långsiktiga åtaganden eller förskottsbetalning. Du kan öka eller minska beräkningskapacitet på begäran samt starta eller stoppa när som helst. Överväg det här alternativet om du kör program med kortsiktiga eller oförutsägbara arbetsbelastningar som inte får avbrytas. Om du till exempel gör ett snabbtest eller utvecklar en app på en virtuell dator är detta ett lämpligt alternativ. |
| **Reserverade VM-instanser** | Alternativet Reserverade VM-instanser (RI) är ett förköp av en virtuell dator för ett eller tre år i en specifik region. Åtagandet görs i förväg och i utbyte får du upp till 72 % lägre avgifter jämfört med priser för betala per användning. **RI** är flexibla och kan enkelt bytas ut eller returneras mot en avgift för tidig uppsägning. Överväg det här alternativet om den virtuella datorn måste köras kontinuerligt eller om du behöver förutsägbara avgifter **och** du kan åta dig att använda den virtuella datorn minst ett år. |

### <a name="storage-for-the-vm"></a>Lagring för den virtuella datorn

Alla virtuella Azure-datorer har minst två virtuella hårddiskar (VHD). Den första disken lagrar operativsystemet och andra används som tillfällig lagring. Du kan lägga till ytterligare diskar för att lagra programdata; det maximala antalet bestäms av valet för VM-storlek (vanligtvis två per processor). Det är vanligt att skapa en eller flera datadiskar, särskilt eftersom operativsystemdisken tenderar att vara ganska liten. Genom att dela upp data på flera virtuella hårddiskar kan du hantera säkerheten, tillförlitligheten och prestanda för en disk oberoende av de andra diskarna.

Data för varje VHD lagras i **Azure Storage** som sidblobar, vilket gör att Azure kan tilldela utrymme endast för den lagring du använder. Det är även så din lagringskostnad mäts; du betalar för den lagring som du förbrukar.

#### <a name="what-is-azure-storage"></a>Vad är Azure Storage?

Azure Storage är Microsofts molnbaserade lösning för datalagring. Den har stöd för nästan alla typer av data och ger säkerhet, redundans och skalbar åtkomst till lagrade data. Ett lagringskonto ger åtkomst till objekt i Azure Storage för en viss prenumeration. Virtuella datorer har alltid ett eller flera lagringskonton för att hantera varje ansluten virtuell disk.

Virtuella diskar kan backas av antingen **Standard** eller **Premium** Storage-konton. Azure Premium Storage använder SSD-diskar för att ge höga prestanda och kort svarstid för virtuella datorer som kör I/O-intensiva arbetsbelastningar. Använd Azure Premium Storage för produktionsarbetsbelastningar, särskilt de som är känsliga för variationer eller är I/O-intensiva. För utveckling och testning räcker det med Standard-lagring.

När du skapar diskar har du två alternativ för att hantera relationen mellan lagringskontot och varje VHD. Du kan välja antingen **ohanterade diskar** eller **hanterade diskar**.

| Alternativ | Beskrivning |
|--------|-------------|
| **Ohanterade diskar** | Med ohanterade diskar ansvarar du för de lagringskonton som används för att lagra de virtuella hårddiskar som motsvarar diskarna på din virtuella dator. Du betalar lagringskontoavgifter för den mängd utrymme du använder. Ett lagringskonto har en fast gräns på 20 000 I/O-åtgärder per sekund. Det betyder att ett lagringskonto kan hantera 40 virtuella standardhårddiskar vid maximal användning. Om du behöver skala ut med fler diskar behöver du fler lagringskonton, vilket kan vara komplicerat. |
| **Hanterade diskar** | Hanterade diskar är den **nyare och rekommenderade disklagringsmodellen**. De löser elegant den här komplexiteten genom att lägga ansvaret för hanteringen av lagringskonton på Azure. Du anger storleken på disken, upp till 4 TB, så skapar och hanterar Azure både disken _och_ lagringen. Du behöver inte bekymra dig om begränsningar i lagringskontot, vilket gör hanterade diskar lättare att skala ut. |

### <a name="select-an-operating-system"></a>Välja ett operativsystem

Azure erbjuder en mängd olika OS-avbildningar som du kan installera på den virtuella datorn, inklusive flera versioner av Windows och varianter av Linux. Såsom nämnts tidigare påverkar valet av operativsystem det timbaserade beräkningspriset eftersom Azure tar med kostnaden för OS-licensen i priset.

Om du vill ha mer än bara grundläggande OS-avbildningar kan du söka på Azure Marketplace efter mer avancerade installationsavbildningar som innehåller operativsystemet och populära programvaruverktyg som installeras för specifika scenarier. Om du till exempel behöver en ny WordPress-webbplats skulle standardteknikstacken bestå av en Linux-server, en Apache-webbserver, en MySQL-databas samt PHP. I stället för att installera och konfigurera varje komponent kan du utnyttja en Marketplace-avbildning och installera hela stacken på en gång.

Om du inte hittar en lämplig OS-avbildning kan du skapa diskavbildningen med det du behöver, ladda upp den till Azure Storage och använda den för att skapa en virtuell Azure-dator. Tänk på att Azure endast stöder 64-bitars operativsystem.
