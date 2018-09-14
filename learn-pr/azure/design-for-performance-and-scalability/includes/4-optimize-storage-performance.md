Det är viktigt att du överväger lagringsprestandan i din arkitektur. Dålig prestanda på lagringsnivå kan påverka slutanvändarna precis som långa svarstider i nätverket. Hur kan du optimera din datalagring? Vad behöver du tänka på så att du inte inför flaskhalsar för lagringen i arkitekturen? Nu ska vi gå igenom hur du kan optimera lagringsprestandan i din arkitektur.

## <a name="optimize-virtual-machine-storage-performance"></a>Optimera lagringsprestandan för virtuella datorer

Först tittar vi på hur du kan optimera lagringen för virtuella datorer. Disklagringen spelar stor roll för prestandan i dina virtuella datorer, så det är viktigt att välja rätt typ av diskar för appen.

Olika appar har olika krav på lagringen. Appen kan vara känslig för svarstider vid läs- och skrivåtgärder, eller så kan den behöva hantera stora IOPS-volymer eller helt enkelt stora dataflöden.

Vilken typ av disk ska du använda när du skapar en IaaS-arbetsbelastning? Det finns fyra alternativ:

- **Lokal SSD-lagring** – varje virtuell dator har en tillfällig disk som backas upp av lokal SSD-lagring. Storleken på disken varierar beroende på den virtuella datorns storlek. Eftersom den här disken är en lokal SSD-disk har den bra prestanda, men data kan förloras vid underhåll eller om den virtuella datorn distribueras om. Den här typen av disk passar endast för tillfällig lagring av data som du inte behöver permanent. Den här disken är utmärkt för sidor eller växlingsfiler och för exempelvis tempdb i SQL Server. Den här lagringen är kostnadsfri. Den ingår i kostnaden för den virtuella datorn.

- **Standard Storage HDD** – Det här är axeldisklagring som kan passa bra när programmet inte är känsligt för varierande svarstider eller lägre dataflöden. Ett bra exempel är en arbetsbelastning för utveckling/testning där du inte behöver garanterade prestanda.

- **Standard Storage SSD** – Det här är SSD-lagring med korta SSD-svarstider, men lägre nivåer för dataflöden. Den här disktypen kan passa bra för webbservrar utanför produktion.

- **Premium Storage SSD** – Den här SSD-lagringen passar bra till arbetsbelastningar som används i produktion där du behöver bästa möjliga tillförlitlighet, korta svarstider samt stora dataflöden och IOPS-volymer. Eftersom de här diskarnas prestanda och tillförlitlighet är bättre, rekommenderas de för alla arbetsbelastningar i produktionen.

Premiumlagring kan bara kopplas till virtuella datorer av vissa storlekar. De virtuella datorer du kan ansluta till har ett s i namnet, exempelvis D2s_v3 eller Standard_F2s_v2. Du kan koppla HDD- eller SSD-diskar av standardtyp till valfri virtuell dator (med eller utan s i namnet).

Du kan koppla diskar med en stripeteknik (som Storage Spaces Direct i Windows eller mdadm i Linux) om du behöver hantera större dataflöden och IOPS-volymer, genom att diskaktiviteten sprids ut över flera diskar. När du stripekopplar diskarna kan du verkligen få ut mesta möjliga av diskarna, och det här är vanligt i avancerade databassystem och andra system med stora lagringskrav.

När du använder arbetsbelastningar i virtuella datorer måste du utvärdera appens prestandakrav så att du vet hur mycket underliggande lagring du måste etablera för de virtuella datorerna.

## <a name="optimize-storage-performance-for-your-application"></a>Optimera lagringsprestanda för appen

Även om du kan använda olika lagringstekniker för att förbättra diskens prestanda kan du även påverka prestandan för dataåtkomsten på programnivå. Nu ska vi gå igenom hur du kan göra det.

### <a name="caching"></a>Cachelagring

Ett vanligt sätt att förbättra appars prestanda är att integrera ett cachelager mellan appen och ditt datalager. Vid cachelagring lagras data normalt i minnet så att de snabbt kan hämtas. Det här kan vara data som används ofta, data du anger från en databas eller tillfälliga data som exempelvis användartillstånd. Du har kontroll över vilken typ av data som lagras, hur ofta den uppdateras och när den upphör att gälla. Genom att placera den här cachelagringen i samma region som programmet och databasen minskar du svarstiderna mellan dem. Det är nästan alltid snabbare att hämta data från cachelagringen än att hämta samma data från en databas, så med ett cachelager kan du förbättra programmets prestanda avsevärt. Följande bild visar hur ett program hämtar data från en databas, lagrar den i ett cacheminne och använder det cachelagrade värdet vid behov.

![En bild som visar att det är snabbare att hämta data från cachen än att hämta från en databas.](../media/4-cache.png)

Azure Redis Cache är en cachelagringstjänst i Azure. Den baseras på Redis Cache (öppen källkod). Azure Redis Cache är en helt hanterad tjänst från Microsoft. Du väljer vilken prestandanivå du behöver och konfigurerar appen för användning av tjänsten.

### <a name="polyglot-persistence"></a>Flerspråkig persistens

Flerspråkig persistens är användningen av olika datalagringstekniker vid hanteringen av olika lagringskrav.

Tänk dig t.ex. en onlinebutik. Du kan lagra programresurser i Blob Store, produktrecensioner och rekommendationer i ett NoSQL-lager, samt användarprofiler eller kontouppgifter i en SQL-databas. Följande bild visar hur ett program kan använda flera tekniker för datalagring till att lagra olika typer av data.

![En bild som visar användningen av olika datalagringsmetoder inom samma program för att öka prestandan och minska kostnaderna.](../media/4-polyglotpersistence.png)

Det här är viktigt eftersom olika datalager är utformade för olika användningsfall, och kan vara mer tillgängliga på grund av kostnaderna. Att lagra blobbar i en SQL-databas kan vara både dyrare och leda till längre svarstider än att använda dem direkt från Blob Store.

Om du använder många lagringsenheter ökar lösningens komplexitet. Fundera på hur du uppfyller de icke-funktionella kraven med de olika datalagren och hur försämringar i tjänsten påverkar appen som helhet. Tänk även på hur data hålls konsekventa mellan de olika datalagren. 

**Eventuell konsekvens** ger ofta en bra balans, men det finns flera olika konsekvensmodeller beroende på tjänsten.

Eventuell konsekvens innebär att replikdatalagren så småningom konvergeras om inga ytterligare skrivningar görs. Om en skrivning görs till ett av datalagren kan läsningar från ett annat lager visa inaktuella data. Du kan använda eventuell konsekvens i större skala eftersom läsningar och skrivningar har korta svarstider, på grund av att informationens konsekvens inte behöver kontrolleras i alla datalager.

## <a name="lamna-healthcare-example"></a>Lamna Healthcare-exempel

Bokningssystemet för Lamna Healthcares patienter körs i två Azure-regioner, Europa, västra och Australien, östra. De använder virtuella datorer som klientnoder när webbplatsen distribueras och de har en Azure SQL-databas distribuerad i regionen Europa, västra som primär lagring samt regionen Australien, östra som ett sekundärt läsbart lager. Klientnoderna behöver inte hantera några stora dataflöden, men de behöver korta svarstider och hög tillförlitlighet, så därför används Premium SSD-lagring.

De kör Azure Redis Cache lokalt i respektive Azure-region för att lagra vanliga användarförfrågningar och läkarnas tillgänglighet. Cachelagringen är implementerad för att optimera prestandan för de vanligaste dataläsningsåtgärderna i programmet.

## <a name="summary"></a>Sammanfattning

Vi har gått igenom några exempel på hur du kan förbättra lagringsprestandan på infrastrukturnivån genom att välja rätt diskarkitektur, samt på programnivå med hjälp av cachelagring och att välja rätt dataplattform för dina data. När du har rätt arkitektur i din lösning får du bästa möjliga prestanda för dina data. Nu ska vi gå igenom hur du kan identifiera prestandaproblem i en arkitektur.
