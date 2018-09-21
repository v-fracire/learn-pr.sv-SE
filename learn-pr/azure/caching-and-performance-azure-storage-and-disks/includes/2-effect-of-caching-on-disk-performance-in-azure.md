Likt dina lokala datorer kan de virtuella datorernas prestanda bero på hur snabbt de kan läsa och skriva data. För att förstå hur vi kan förbättra prestandan, måste vi först förstå hur prestanda mäts och inställningar och alternativ som påverkar den.

Vi tittar närmare på underliggande diskar och lagring som används för virtuella datorer. Du måste även ta programnivån i åtanke när du analyserar prestandan. Om du kör en databas på en virtuell dator vill du kontrollera prestandainställningen för den databasen så att den är optimerad för den virtuella datorn och lagringen som du kör den på.

Låt oss börja med att definiera några begrepp och de garantier som Azure utfäster för dem.

## <a name="io-operations-per-second"></a>I/O-åtgärder per sekund

Den lagringstyp som du väljer (standard eller premium) avgör hur snabba dina diskar är. Vi mäter prestandan i I/O-åtgärder per sekund eller IOPS (uttalas ”aj-åps”).

IOPS är ett tal för antalet begäranden som kan bearbetas av disken på en sekund. En enskild begäran är en läs- eller skrivåtgärd. Detta mått tillämpas direkt på lagringen. Till exempel: Om du har en disk som kan köra **5000 IOPS**, innebär det att den teoretiskt sett kan bearbetning 5 000 läs- och skrivåtgärder per sekund.

IOPS påverkar programmets prestanda direkt. Vissa program, till exempel återförsäljarwebbplatser, behöver hög IOPS för att hantera alla små och slumpmässiga I/O begäranden som måste bearbetas snabbt för att hålla platsen dynamiskt.

### <a name="iops-in-azure"></a>IOPS på Azure

När du kopplar en premiumlagringsdisk till en virtuell dator på hög nivå tilldelar Azure ett garanterat antal IOPS enligt diskspecifikationen. Till exempel en **P50**-disk tilldelar **7500 IOPS**. Varje storlek för virtuella datorer på hög nivå har en specifik IOPS-gräns. Till exempel, en **Standard GS5**-VM har en gräns på **80 000 IOPS**.

IOPS är ett mått på diskar med lagringsutrymme, men det är en _teoretisk_ gräns - två andra faktorer kan påverka den faktiska programprestandan: **dataflöde** och **svarstid**.

### <a name="what-is-throughput"></a>Vad är dataflöde?
Dataflöde (kallas även ”bandbredd”) är mängden data som ditt program skickar till lagringsdiskar inom ett visst intervall (vanligtvis per sekund). Om ditt program utför I/O-åtgärder med stora datablock kräver det ett högt dataflöde.

Azure tillhandahåller genomflödet i premium-lagringsdiskar baserat på diskarnas specifikationer. Till exempel tilldelas en **P50**-disk ett dataflöde på **250 MB per sekund** per disk. Varje storlek för virtuella datorer på hög nivå har en specifik gräns för _genomflödet_. Till exempel, den virtuella datorn **Standard GS5** har en maximal genomströmning på **2 000 MB per sekund**.

#### <a name="iops-vs-throughput"></a>IOPS jämfört med dataflöde

Dataflöde och IOPS har en direkt relation och om du ändrar den ena påverkas den andra. Du kan använda den här formeln för att få en teoretisk gräns för dataflödet: `IOPS x I/O size = throughput`. Det är viktigt att tänka på båda dessa värden när du planerar ditt program.

### <a name="what-is-latency"></a>Vad är svarstid?

Det tar tid att läsa och skriva data. Det är där _svarstid_ kommer in. Svarstiden är den tid det tar att din app för att skicka en begäran till disken och få svar. Svarstiden berättar hur lång tid det tar att _bearbeta_ en enda läs- eller skriv-I/O-åtgärd.

Svarstiden sätter en gräns för IOPS. Till exempel, om vår disk kan hantera 5 000 IOPS men varje åtgärd tar 10 ms att bearbeta kommer vår app inte att kunna bearbeta mer än 100 åtgärder per sekund på grund av bearbetningstiden. Det här är ett enkelt exempel, svarstiden brukar vara mycket lägre. Svarstiden och dataflödet avgör helt enkelt hur snabbt din app kan bearbeta data från lagringen. 

Premium Storage erbjuder genomgående korta svarstider och du kan uppnå ännu bättre svarstid vid behov via _cachelagring_. 

## <a name="testing-your-disk-performance"></a>Testa din diskprestanda

Du kan justera och balansera IOPS, dataflöde och svarstid för dina virtuella diskar genom att välja rätt storlek och lagring för dina virtuella datorer. Vanligtvis har de större och dyrare virtuella datorerna högre garantier för maximalt IOPS och dataflöde. Lägg till detta i ekvationen mellan Standard och Premium Storage-konton, samt HDD och SDD, så har du många parametrar att experimentera med.

Det är viktigt att känna till ditt programs behov för att välja rätt kombination. Program som kräver högt I/O, till exempel databasservrar eller system för transaktioner online, kräver högre IOPS, medan mer databaserade program ofta har lägre krav. Dessutom kan _typen_ av åtgärder som utförs av programmet påverka ditt dataflöde. Åtkomst med slumpmässig I/O är ofta långsammare än långa sekventiella läsåtgärder.

När du har valt din konfiguration kan du använda verktyg som [Iometer](http://iometer.org/) för att testa din diskprestanda på Linux- och Windows-datorer. Detta ger dig en mer verklig uppfattning om vilken typ av prestanda du kan förvänta dig. Det kan också hjälpa dig att identifiera sätt att förbättra appens lagringsanvändning – till exempel ett program som använder enkeltrådade I/O-åtgärder påverkas troligen av sämre I/O-prestanda på grund av svarstiden.

Nu ska vi titta på några saker som vi kan göra för att förbättra vår diskprestanda.

