Tänk dig att nyheterna nyss har publicerat en artikel som handlar om din organisations banbrytande cancerbehandling. Det här är en fantastisk milstolpe, och det kommer säkerligen att leda en stor mängd trafik till din webbplats. Kommer webbplatsen att kunna hantera den här trafikökningen, eller kommer belastningen att leda till att webbplatsen blir långsam eller slutar svara?

Här tittar vi på några grundläggande principer som säkerställer att programmens prestanda hålls på topp med hjälp av principer för skalning och optimering.

## <a name="what-is-scaling-and-performance-optimization"></a>Vad är skalning och prestandaoptimering?

Skalning och prestandaoptimering handlar om att matcha de resurser som är tillgängliga för ett program med efterfrågan på programmet. I prestandaoptimering ingår skalningsresurser, identifiering och optimering av eventuella flaskhalsar samt optimering av programkoden för topprestanda.

### <a name="scaling"></a>Skalning

Beräkningsresurserna kan skalas i två olika riktningar:

* När man skalar *upp* lägger man till fler resurser till en enda instans.
* När man skalar *ut* lägger man till fler instanser.

![En bild som visar skala upp och skala ut för en virtuell dator för att ändra prestandan.](../media/scale-up-scale-out.png)

Man skalar upp när man vill lägga till fler resurser, till exempel CPU eller minne, till en enda instans. Instansen kan vara en virtuell dator eller en PaaS-tjänst. När mer kapacitet läggs till instansen ökas de resurser som finns tillgängliga för programmet, men det finns en begränsning. Virtuella datorer är begränsade till kapaciteten hos den värd de körs på, och de här värdarna har fysiska begränsningar. När du skalar upp en instans kan du till slut stöta på de här begränsningarna, och därmed hindras du från att lägga till fler resurser till instansen.

Man skalar ut när man vill lägga till fler instanser till en tjänst. Dessa kan vara virtuella datorer eller PaaS-tjänster, men i stället för att lägga till mer kapacitet genom att göra en instans kraftfullare lägger vi till kapacitet genom att öka det totala antalet instanser. Fördelen med att skala ut är att det är möjligt för dig att skala ut i all oändlighet så länge som du har fler maskiner att lägga till arkitekturen. När man skalar ut krävs det någon form av belastningsutjämning. Det kan uppnås med en lastbalanserare som fördelar begäranden över alla servrar eller med en tjänsteidentifieringsmetod som identifierar aktiva servrar som begäranden kan skickas till.

I båda fallen kan resurser minskas, och kostnadsoptimering kommer in i bilden.

### <a name="performance-optimization"></a>Prestandaoptimering

Optimera prestandan genom att titta på nätverk och lagring och kontrollera att prestandan är godtagbar. Båda kan påverka svarstiden för ditt program. Genom att välja den rätta nätverks- och lagringstekniken för arkitekturen säkerställer du att dina kunder får en så bra användarupplevelse som möjligt.

I prestandaoptimering ingår även att du får en förståelse för vilken prestanda själva programmen har. Fel, dåligt fungerande kod och flaskhalsar i beroende system kan upptäckas med ett verktyg för hantering av programprestanda. De här problemen kan ofta vara dolda för slutanvändare, utvecklare och administratörer, men de kan ha en negativ inverkan på programmets totala prestanda.

## <a name="scalability-and-performance-patterns-and-practices"></a>Metodtips och mönster för skalbarhet och prestanda

Låt oss ta en titt på några mönster och metodtips som kan utnyttjas för att förbättra skalbarheten och prestandan i ditt program.

### <a name="data-partitioning"></a>Datapartitionering

I många storskaliga lösningar delas data in i partitioner som kan hanteras och kommas åt separat. Det är viktigt att välja partitioneringsstrategi noga för att maximera fördelarna och minimera de negativa effekterna. Partitionering kan förbättra skalbarheten, minska konkurrensen och ge bästa möjliga prestanda.

### <a name="caching"></a>Cachelagring

Du kan förbättra prestandan genom att använda cachelagring i arkitekturen. Cachelagring är en metod som används för att lagra data eller tillgångar (webbsidor, bilder) som används ofta för snabbare åtkomst. Cachelagring kan användas i olika lager i ditt program. Du kan använda cachelagring mellan programservrarna och en databas om du vill minska tiderna för datahämtning. Du kan också använda cachelagring mellan slutanvändarna och webbservrarna. Det statiska innehållet placeras då närmare användaren och minskar tiden det tar att returnera webbsidor till slutanvändaren. Det här leder dessutom till att begäranden avlastas från databasen eller webbservrarna, vilket ökar prestandan för andra begäranden.

### <a name="autoscaling"></a>Automatisk skalning

Automatisk skalning är processen för att dynamiskt tilldela resurser för att matcha prestandakrav. När mängden arbete växer kan ett program behöver ytterligare resurser för att underhålla de önskade prestandanivåerna och uppfylla servicenivåavtal (SLA). När behovet sjunker och de ytterligare resurserna inte längre behövs kan de frigöras för att minimera kostnader.

Automatisk skalning använder elasticiteten hos molndrivna miljöer och kräver lägre arbetsinsats för hantering. Det minskar behovet av att en operatör regelbundet övervakar systemets prestanda och tar beslut om att lägga till eller ta bort resurser.

### <a name="decouple-resource-intensive-tasks-as-background-jobs"></a>Frikoppla resurskrävande uppgifter som bakgrundsjobb

Många typer av program kräver bakgrundsaktiviteter som körs oberoende av användargränssnittet (UI). Det kan vara batchjobb, beräkningsintensiva bearbetningar och tidskrävande processer, till exempel arbetsflöden. Bakgrundsjobb kan köras utan användaråtgärder – programmet kan starta jobbet och sedan fortsätta att bearbeta interaktiva begäranden från användarna. Det kan hjälpa dig för att minimera belastningen på programmets användargränssnitt, vilket kan förbättra tillgängligheten och minska interaktiva svarstider.

### <a name="use-a-messaging-layer-between-services"></a>Använda ett meddelandelager mellan tjänster

Att lägga till ett meddelandelager mellan tjänsterna kan ha en gynnsam inverkan på prestanda och skalbarhet. Genom att lägga till ett meddelandelager skapas en buffert för begäranden mellan tjänsterna. Då kan begäranden fortsätta rulla in utan fel fastän programmet inte hänger med. När programmet arbetar sig igenom begärandena svarar programmet på dem i den ordning de togs emot.

### <a name="implement-scale-units"></a>Implementera skalningsenheter

Skala som en enhet. För varje resurs ska du fastställa vilken effekt som en skalningsaktivitet kan ha på beroende system. Det gör det enklare att tillämpa skalningsåtgärder, med mindre negativ inverkan på programmet. Att till exempel lägga till x antal webb- och arbetsroller kan kräva y antal ytterligare köer och z antal lagringskonton för att hantera den extra arbetsbelastning som genererats av rollerna. En skalningsenhet kan därför bestå av x webb- och arbetsroller, y köer och z lagringskonton. Designa programmet så att det skalas enkelt genom att lägga till en eller flera skalningsenheter.

### <a name="performance-monitoring"></a>Prestandaövervakning

Distribuerade program och tjänster som körs i molnet är till sin natur komplexa delar av programvara som utgör många rörliga delar. I en produktionsmiljö är det viktigt att kunna spåra hur användarna använder systemet, spåra resursutnyttjande och i allmänhet övervaka systemets hälsotillstånd och prestanda. Du kan använda den här informationen som diagnostiskt stöd och identifiera och åtgärda problem, och för att upptäcka potentiella problem och förebygga att de uppstår.

Se över alla lager i programmet och identifiera och åtgärda flaskhalsar för programmets prestanda. De här flaskhalsarna kan till exempel vara dålig minneshantering i programmet eller till och med processen för att lägga till index i databasen. Det kan vara en iterativ process då du åtgärdar en flaskhals och sedan upptäcker en annan som du inte vetat om.

Med en omfattande metod för prestandaövervakning kommer du att kunna avgöra vilka typer av mönster och metodtips som din arkitektur kan dra nytta av.