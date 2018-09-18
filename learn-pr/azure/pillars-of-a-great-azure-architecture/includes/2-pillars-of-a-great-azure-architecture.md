Molnet har ändrat hur organisationer löser sina affärsutmaningar och hur program och system utformas. Rollen för en lösningsarkitekt är inte bara att skapa affärsvärde via programmets funktionskrav, utan också att se till att lösningen är utformad på ett sätt som är skalbart, elastiskt, effektivt och säkert. Lösningsarkitektur handlar om planering, design, implementering och pågående förbättringar av ett tekniksystem. Ett systems arkitektur måste balansera och justera affärskraven med de tekniska funktioner som behövs för att köra dessa krav. Det inkluderar en utvärdering av risk, kostnad och kapacitet i hela systemet och dess komponenter.

Även om det inte finns någon enkel metod för att utforma en arkitektur som passar alla, så finns det vissa universella koncept som gäller oavsett arkitektur, teknik och molnprovider. Även om de inte inkluderar allt kan du utgå från dessa koncept för att få hjälp att skapa en tillförlitlig, säker och flexibel grund för ditt program.

En bra arkitektur börjar med en stabil grund som bygger på fyra grundpelare:

* Säkerhet
* Prestanda och skalbarhet
* Tillgänglighet och återställning
* Effektivitet och drift

![Grundpelare för en bra arkitektur](../media-draft/pillars.png)

## <a name="security"></a>Säkerhet

Data i olika former är oftast den värdefullaste delen av din organisations tekniska fotavtryck. I den här grundpelaren kommer du att fokuserar på att säkra åtkomst till din arkitektur via autentisering och att skydda ditt program och dina data från nätverkets sårbarheter. Integriteten för dina data bör också skyddas med verktyg som kryptering.

Du måste tänka på säkerheten i programmets hela livscykel, från design och implementering till distribution och drift. Molnet ger skydd mot en mängd hot, till exempel nätverksintrång och DDoS-attacker, men du måste fortfarande bygga in säkerhet i dina program, processer och i organisationens kultur.

![Angreppstyper](../media-draft/security.png)

## <a name="performance-and-scalability"></a>Prestanda och skalbarhet

För att en arkitektur ska fungera bra och vara skalbar, måste den korrekt matcha resurskapacitet efter efterfrågan. Traditionellt gör molnarkitekturer det genom att skala program dynamiskt baserat på aktivitet i programmet. Efterfrågan på tjänster ändras så det är viktigt att din arkitektur kan anpassa sig till efterfrågan. Genom att skapa din arkitektur med prestanda och skalbarhet i åtanke så kan du erbjuda en bra upplevelse för dina kunder samtidigt som du är kostnadseffektiv.

![Bild som visar ett stort inflöde av data eller begäranden](../media-draft/performance-demand.png))

## <a name="availability-and-recoverability"></a>Tillgänglighet och återställning

Varje arkitekts värsta mardröm är att din arkitektur slutar fungera utan att det går att återställa den. En lyckad molnmiljö är skapad på ett sätt som förutser fel på alla nivåer. En del av att förutse de här felen är att designa ett system som kan återställas från fel, inom den tid som krävs av dina intressenter och kunder.

![Systemfel](../media-draft/system-failure.png)

## <a name="efficiency-and-operations"></a>Effektivitet och drift

Vi vill utforma din molnmiljö så att den är kostnadseffektiv att driva och utveckla mot. Ineffektivitet och slöseri av molnutgifter ska identifieras för att säkerställa att du lägger pengarna där du får mest valuta för dem. Du behöver ha en bra övervakningsarkitektur på plats för att kunna identifiera fel och problem innan de inträffar eller åtminstone innan dina kunder märker av dem. Du måste också ha visibilitet över hur dina program använder de tillgängliga resurserna via ett robust ramverk för övervakning.

![Effektivitet](../media-draft/efficiency.png)

## <a name="shared-responsibility"></a>Delat ansvar

Genom att flytta till molnet introduceras en modell för delat ansvar. I den här modellen hanterar din molnprovider vissa aspekter av ditt program och du har ansvar för återstående delar. Du är ansvarig för allt i en lokal miljö. När du migrerar till infrastruktur som en tjänst (IaaS), sedan till plattform som en tjänst (PaaS) och programvara som en tjänst (SaaS) tar molnprovidern över mer av det ansvaret. Det här delade ansvaret kommer att spela en roll i dina arkitektursbeslut, eftersom de kan ha inverkan på kostnader, operativa funktioner, säkerhet och de tekniska funktionerna i ditt program. Genom att överlåta dessa ansvarsområden till din provider kan du fokusera på att generera värde för din verksamhet och lägga mindre vikt vid aktiviteter som inte är centrala i verksamheten.

![Molntjänstmodeller](../media-draft/cloud-responsibility-model.png)

## <a name="design-choices"></a>Utformningsalternativ

I en perfekt arkitektur skulle vi skapa den mest säkra, högpresterande, högtillgängliga och effektiva miljön som går. Men precis som alltid så måste man göra vissa kompromisser. För att skapa en miljö med den högsta nivån för alla pelare så finns det en kostnad. Kostnaden kan vara i pengar, tid till leverans eller driftsmässig flexibilitet. Varje organisation har olika prioriteter som påverkar de designval som görs för varje pelare. När du utformar din arkitektur behöver du bestämma vilka kompromisser som är godtagbara och vilka som inte är det.

När du skapar en Azure-arkitektur finns det många saker att tänka på. Du vill att din arkitektur är säker, skalbar, tillgänglig och återställningsbar. Om du ska lyckas med det behöver du fatta beslut utifrån kostnad, organisationens prioriteringar och riskerna.