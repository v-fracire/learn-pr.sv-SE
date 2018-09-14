Molnet har ändrats på sätt som organisationer lösa sina affärsutmaningar och hur program och system är utformade för. Rollen för Lösningsarkitekt är inte bara att skapa affärsvärden via funktionskrav av programmet, men se till att lösningen är utformad på ett sätt som är skalbara, elastiska, effektiv och säker. Lösningsarkitektur är bekymrad över planering, design, implementering och pågående förbättring av en teknik-system. Ett systems arkitektur måste balansera och justera affärskraven med de tekniska funktioner som behövs för att köra dessa krav. Det inkluderar en utvärdering av risk, kostnad och kapacitet i hela systemet och dess komponenter.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEv2]

Det finns ingen enkel metod för att utforma en arkitektur, finns men det några universal begrepp som gäller oavsett arkitektur, teknik eller molnleverantör. Även om dessa inte är omfattande, fokusera på dessa begrepp hjälper dig att bygga en tillförlitlig, säker och flexibel grund för ditt program.

En bra arkitektur börjar med en stabil grund som bygger på fyra grundpelare:

* Säkerhet
* Prestanda och skalbarhet
* Tillgänglighet och återställning
* Effektivitet och drift

![Grundpelare för en bra arkitektur](../media-draft/pillars.png)

## <a name="security"></a>Säkerhet

Data i olika former är den mest värdefulla teknisk fotavtryck för din organisation. I den här grundpelaren så kommer du att fokuserar på att säkra åtkomst till din arkitektur via autentisering och att skydda ditt program och dina data från nätverketssårbarheter. Integriteten för dina data bör också skyddas med verktyg som kryptering.

Du måste tänka på säkerheten under hela livscykeln för ditt program, från design och implementering till distribution och drift. Molnet ger skydd mot en mängd hot, till exempel intrång och DDoS-attacker, men du måste fortfarande bygger in säkerhet i dina program, processer och organisationens kultur.

![Angreppstyper](../media-draft/security.png)

## <a name="performance-and-scalability"></a>Prestanda och skalbarhet

För att en arkitektur ska fungera bra och vara skalbar, måste den korrekt matcha resurskapacitet efter efterfrågan. Traditionellt gör molnarkitekturer det genom att skala program dynamiskt baserat på aktivitet i programmet. Efterfrågan på tjänster ändras så det är viktigt att din arkitektur kan anpassa sig till efterfrågan. Genom att skapa din arkitektur med prestanda och skalbarhet i åtanke så kan du erbjuda en bra upplevelse för dina kunder samtidigt som du är kostnadseffektiv.

![Bild som visar ett stort inflöde av data eller begäranden](../media-draft/performance-demand.png))

## <a name="availability-and-recoverability"></a>Tillgänglighet och återställning

Varje arkitekts värsta mardröm är att din arkitektur slutar fungera utan att det går att återställa den. En lyckad molnmiljö är skapad på ett sätt som förutser fel på alla nivåer. En del av att förutse de här felen är att designa ett system som kan återställas från fel, inom den tid som krävs av dina intressenter och kunder.

![Systemfel](../media-draft/system-failure.png)

## <a name="efficiency-and-operations"></a>Effektivitet och drift

Du vill utforma din molnmiljö så att det är kostnadseffektivt att driva och utveckla mot. Funktionen och spill i molnet utgifter ska identifieras för att säkerställa att du arbetar särskilt pengar där vi kan göra bästa användning av den. Du måste ha en bra övervakning arkitektur på plats så att du kan identifiera fel och problem innan de inträffar eller, som ett minimum innan Lägg märke till dina kunder. Du måste också måste vissa insyn hur ditt program använder olika tillgängliga resurser via ett robust övervakning ramverk.

![Effektivitet](../media-draft/efficiency.png)

## <a name="shared-responsibility"></a>Delat ansvar

Flytta till molnet introducerar en modell för delat ansvar. I den här modellen hanterar molnleverantör vissa aspekter av ditt program, så att du har det återstående ansvaret. Du är ansvarig för allt innehåll i en lokal miljö. När du migrerar till infrastruktur som en tjänst (IaaS), sedan tar till plattform som en tjänst (PaaS) och programvara som en tjänst (SaaS), molnleverantör på flera av den här ansvar. Den här delat ansvar kommer att ha en roll i din arkitektur beslut, eftersom de kan ha konsekvenser för kostnader, operativa funktioner, säkerhet och de tekniska funktionerna i ditt program. Genom att ändra detta ansvar för din leverantör kan du fokusera på insaltning värde för din verksamhet och överger aktiviteter som inte är en grundläggande funktioner.

![Molntjänstmodeller](../media-draft/cloud-responsibility-model.png)

## <a name="design-choices"></a>Utformningsalternativ

I en perfekt arkitektur skulle vi skapa den mest säkra, högpresterande, högtillgängliga och effektiva miljön som går. Men precis som alltid så måste man göra vissa kompromisser. För att skapa en miljö med den högsta nivån för alla pelare så finns det en kostnad. Kostnaden kan vara i pengar, tid till leverans eller driftsmässig flexibilitet. Varje organisation har olika prioriteter som påverkar de designval som görs för varje pelare. När du utformar din arkitektur, behöver du bestämma vilka kompromisser accepteras och som inte är.

När du skapar en Azure-arkitektur, finns det många saker att tänka på. Du vill att din arkitektur är säker, skalbar, tillgänglig och återställningsbar. Om du vill för att måste du fatta beslut utifrån kostnader, organisationens prioriteringar och risk.