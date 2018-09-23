Molnet har ändrat hur organisationer löser sina affärsutmaningar och hur program och system utformas. Rollen för en lösningsarkitekt är inte bara att skapa affärsvärde via programmets funktionskrav, utan också att se till att lösningen är utformad på ett sätt som är skalbart, elastiskt, effektivt och säkert. Lösningsarkitekturer handlar om planering, design, implementering och pågående förbättringar av ett tekniksystem. Ett systems arkitektur måste balansera och justera affärskraven med de tekniska funktioner som behövs för att köra dessa krav. I det ingår att utvärdera risker, kostnader och kapaciteten för hela systemet och dess komponenter.

#### <a name="design-a-great-azure-architecture"></a>Utforma en fantastisk Azure-arkitektur

<!-- TODO: revisit this video after Ignite -->
<!-- > VIDEO: https://www.microsoft.com/videoplayer/embed/RE2yEv2 -->

Även om det inte finns någon enkel metod för att utforma en arkitektur som passar alla, så finns det vissa generella koncept som gäller oavsett arkitektur, teknik och molnprovider. Även om de inte hanterar allt kan du utgå från de här koncepten om du behöver hjälp med att skapa en tillförlitlig, säker och flexibel grund för ditt program.

En bra arkitektur börjar med en stabil grund som bygger på fyra grundpelare:

* Säkerhet
* Prestanda och skalbarhet
* Tillgänglighet och återställning
* Effektivitet och drift

![En bild som visar grundpelarna i en bra Azure-arkitektur](../media/pillars.png)

## <a name="security"></a>Säkerhet

Data i olika former är oftast den värdefullaste delen av din organisations tekniska fotavtryck. I den här grundpelaren kommer du att fokusera på att skydda åtkomsten till din arkitektur via autentisering, och att skydda ditt program och dina data från sårbarheter i nätverket. Integriteten för dina data bör också skyddas med verktyg som kryptering.

Du måste tänka på säkerheten i programmets hela livscykel, från design och implementering till distribution och drift. Molnet ger skydd mot en mängd hot, till exempel nätverksintrång och DDoS-attacker, men du måste fortfarande bygga in säkerhet i dina program, processer och i organisationens kultur.

![En bild som visar vilka typer av säkerhetshot och attacker som kan påverka dina data i molnet.](../media/security.png)

## <a name="performance-and-scalability"></a>Prestanda och skalbarhet

För att en arkitektur ska fungera bra och vara skalbar, måste den korrekt matcha resurskapacitet efter efterfrågan. Traditionellt gör molnarkitekturer det genom att skala program dynamiskt baserat på aktivitet i programmet. Efterfrågan på tjänster ändras så det är viktigt att din arkitektur kan anpassa sig till efterfrågan. Genom att skapa din arkitektur med prestanda och skalbarhet i åtanke så kan du erbjuda en bra upplevelse för dina kunder samtidigt som du är kostnadseffektiv.

![En bild som visar hur resurser i molnet skalas dynamiskt baserat på efterfrågan, vilket resulterar i mycket effektiv användning. När resurserna däremot implementeras på en fast nivå leder det till ineffektiv användning vid låg efterfrågan och kapacitetsbrist under perioder med hög efterfrågan.](../media/performance-demand.png)

## <a name="availability-and-recoverability"></a>Tillgänglighet och återställning

Varje arkitekts värsta mardröm är att din arkitektur slutar fungera utan att det går att återställa den. En lyckad molnmiljö är skapad på ett sätt som förutser fel på alla nivåer. En del av att förutse de här felen är att designa ett system som kan återställas från fel, inom den tid som krävs av dina intressenter och kunder.

![En bild som visar två virtuella datorer i ett virtuellt nätverk. En av datorerna visas som misslyckad medan den andra arbetar med kundtjänstförfrågningar.](../media/system-failure.png)

## <a name="efficiency-and-operations"></a>Effektivitet och drift

Vi vill utforma din molnmiljö så att den är kostnadseffektiv att driva och utveckla mot. Ineffektivitet och slöseri av molnutgifter ska identifieras för att säkerställa att du lägger pengarna där du får mest valuta för dem. Du behöver ha en bra övervakningsarkitektur på plats för att kunna identifiera fel och problem innan de inträffar, eller åtminstone innan dina kunder märker av dem. Du måste också ha insyn i hur dina program använder de tillgängliga resurserna via ett robust ramverk för övervakning.

![En bild som visar hur kvalitet, hastighet och effektivitet ökar samtidigt som kostnaderna minskar.](../media/efficiency.png)

# <a name="shared-responsibility"></a>Delat ansvar

När du flyttar till molnet uppstår en modell med delat ansvar. I den här modellen hanterar din molnprovider vissa aspekter av ditt program och du har ansvar för återstående delar. Du är ansvarig för allt i en lokal miljö. När du migrerar till IaaS (infrastruktur som en tjänst), till PaaS (plattform som en tjänst) och slutligen till SaaS (programvara som en tjänst) tar molnprovidern över allt mer av ansvaret. Det här delade ansvaret kommer att spela en roll i dina arkitektursbeslut, eftersom de kan påverka kostnader, operativa funktioner, säkerheten och de tekniska funktionerna i ditt program. Genom att överlåta dessa ansvarsområden till din provider kan du fokusera på att generera värde i verksamheten och lägga mindre vikt vid aktiviteter som inte är centrala.

![En bild som visar nivån för delat ansvar i respektive molntjänstmodell](../media/cloud-responsibility-model.png)

# <a name="design-choices"></a>Designalternativ

I en perfekt arkitektur skulle vi skapa den mest säkra, högpresterande, högtillgängliga och effektiva miljön som går. Men precis som alltid så måste man göra vissa kompromisser. För att skapa en miljö med den högsta nivån för alla pelare så finns det en kostnad. Kostnaden kan vara i pengar, tid till leverans eller driftsmässig flexibilitet. Varje organisation har olika prioriteter som påverkar de designval som görs för varje pelare. När du utformar din arkitektur måste du avgöra vilka kompromisser som är godtagbara och vilka som inte är det.

När du skapar en Azure-arkitektur finns det många saker att tänka på. Du vill att din arkitektur ska vara säker, skalbar, tillgänglig och återställningsbar. Om du ska lyckas med det måste du fatta beslut utifrån kostnad, organisationens prioriteringar och riskerna.
