Tänk dig att nyheterna nyss har publicerat en artikel som handlar om din organisations banbrytande cancerbehandling. Det här är en fantastisk milstolpe, och det kommer säkerligen att leda en stor mängd trafik till din webbplats. Kommer webbplatsen att kunna hantera den här trafikökningen, eller kommer belastningen att leda till att webbplatsen blir långsam eller slutar svara?

Här tittar vi på några grundläggande principer som säkerställer att programmens prestanda hålls på topp med hjälp av principer för skalning och optimering.

## <a name="what-is-scaling-and-performance-optimization"></a>Vad är skalning och prestandaoptimering?

Skalning och prestandaoptimering handlar om att matcha de resurser som är tillgängliga för ett program med efterfrågan på programmet. I prestandaoptimering ingår skalningsresurser, identifiering och optimering av eventuella flaskhalsar samt optimering av programkoden för topprestanda.

### <a name="scaling"></a>Skalning

Beräkningsresurserna kan skalas i två olika riktningar:

* När man skalar *upp* lägger man till fler resurser till en enda instans.
* När man skalar *ut* lägger man till fler instanser.

![Skala upp och skala ut](../media-draft/scale-up-scale-out.png)

Man skalar upp när man vill lägga till fler resurser, till exempel CPU eller minne, till en enda instans. Instansen kan vara en virtuell dator eller en PaaS-tjänst. När mer kapacitet läggs till instansen ökas de resurser som finns tillgängliga för programmet, men det finns en begränsning. Virtuella datorer är begränsade till kapaciteten hos den värd de körs på, och de här värdarna har fysiska begränsningar. När du skalar upp en instans kan du till slut stöta på de här begränsningarna, och därmed hindras du från att lägga till fler resurser till instansen.

Man skalar ut när man vill lägga till fler instanser till en tjänst. Dessa kan vara virtuella datorer eller PaaS-tjänster, men i stället för att lägga till mer kapacitet genom att göra en instans kraftfullare lägger vi till kapacitet genom att öka det totala antalet instanser. Fördelen med att skala ut är att det är möjligt för dig att skala ut i all oändlighet så länge som du har fler maskiner att lägga till arkitekturen. När man skalar ut krävs det någon form av belastningsutjämning. Det kan uppnås med en lastbalanserare som fördelar begäranden över alla servrar eller med en tjänsteidentifieringsmetod som identifierar aktiva servrar som begäranden kan skickas till.

I båda fallen kan resurser minskas, och kostnadsoptimering kommer in i bilden.

### <a name="performance-optimization"></a>Prestandaoptimering

Optimera prestandan genom att titta på nätverk och lagring och kontrollera att prestandan är godtagbar. Båda kan påverka svarstiden för ditt program. Genom att välja den rätta nätverks- och lagringstekniken för arkitekturen säkerställer du att dina kunder får en så bra användarupplevelse som möjligt.

I prestandaoptimering ingår även att du får en förståelse för vilken prestanda själva programmen har. Fel, dåligt fungerande kod och flaskhalsar i beroende system kan upptäckas med ett verktyg för hantering av programprestanda. De här problemen kan ofta vara dolda för slutanvändare, utvecklare och administratörer, men de kan ha en negativ inverkan på programmets totala prestanda.

## <a name="scalability-and-performance-best-practices"></a>Metodtips för skalbarhet och prestanda

Se över alla lager i programmet och identifiera och åtgärda flaskhalsar för programmets prestanda. De här flaskhalsarna kan till exempel vara dålig minneshantering i programmet eller till och med processen för att lägga till index i databasen. Det här kan vara en iterativ process då du kanske åtgärdar en flaskhals och sedan upptäcker en till som du inte vetat om.

Du kan förbättra prestandan genom att använda cachelagring i arkitekturen. Cachelagring är en metod som används för att lagra data eller tillgångar (webbsidor, bilder) som används ofta för snabbare åtkomst. Cachelagring kan användas i olika lager i ditt program. Du kan använda cachelagring mellan programservrarna och en databas om du vill minska tiderna för datahämtning. Du kan också använda cachelagring mellan slutanvändarna och webbservrarna. Det statiska innehållet placeras då närmare användaren och minskar tiden det tar att returnera webbsidor till slutanvändaren. Det här leder dessutom till att begäranden avlastas från databasen eller webbservrarna, vilket ökar prestandan för andra begäranden.

Det är inte ovanligt att se äldre program som är byggda som ett stort program som utför flera uppgifter. De här arkitekturerna kan oftast bara skalas upp och kan inte skalas ut. Med sådana här arkitekturer kan du försöka modernisera ditt program och koppla tjänsterna ifrån varandra. Frånkoppling betyder att man delar ett programs huvudsakliga funktioner i flera olika program. När de har separerats kan varje tjänst sedan skalas ut och in enligt behov, oberoende av varandra.

Utöver frånkoppling kan prestandan också förbättras genom att lägga till ett meddelandelager mellan tjänsterna. Genom att lägga till ett meddelandelager skapas en buffert för begäranden mellan tjänsterna. Då kan begäranden fortsätta rulla in utan fel fastän programmet inte hänger med. När programmet arbetar sig igenom begärandena svarar programmet på dem i den ordning de togs emot.

Det finns många platser, beroende på arkitekturen, där du kan försöka optimera prestandan för ditt program. Utvärdera programmets prestanda och leta efter områden som inte presterar optimalt.
