Inom IT, innebär arkitekturen planering, design, implementering och pågående förbättring av ett tekniksystem. Ett systems arkitektur måste balansera och justera affärskraven med de tekniska funktioner som behövs för att köra dessa krav. Det inkluderar en utvärdering av risk, kostnad och kapacitet i hela systemet och dess komponenter.

Även om det inte finns någon enkel metod för att utforma en arkitektur så finns det vissa universella principer som gäller oavsett arkitektur, teknik eller molnprovider. En fokus på dessa principer hjälper dig att skapa en tillförlitlig, säker och flexibel grund för ditt program.

En bra arkitektur börjar med en stabil grund som bygger på fyra grundpelare:

* Säkerhet
* Prestanda och skalbarhet
* Tillgänglighet och återställning
* Effektivitet och drift

![Grundpelare för en bra arkitektur](../media-draft/pillars.png)

## <a name="security"></a>Säkerhet

Data i olika former är oftast den värdefullaste delen av din organisations tekniska fotavtryck. I den här grundpelaren så kommer du att fokuserar på att säkra åtkomst till din arkitektur via autentisering och att skydda ditt program och dina data från nätverketssårbarheter. Integriteten för dina data bör också skyddas med verktyg som kryptering. Det kan finnas regelmässiga föreskrifter som påverkar dina krav. Om dina kunder inte tror att du är säker så kommer de inte lita på dig och kommer inte att använda din produkt.

![Angreppstyper](../media-draft/security.png)

## <a name="performance-and-scalability"></a>Prestanda och skalbarhet

För att en arkitektur ska fungera bra och vara skalbar, måste den korrekt matcha resurskapacitet efter efterfrågan. Traditionellt gör molnarkitekturer det genom att skala program dynamiskt baserat på aktivitet i programmet. Efterfrågan på tjänster ändras så det är viktigt att din arkitektur kan anpassa sig till efterfrågan. Genom att skapa din arkitektur med prestanda och skalbarhet i åtanke så kan du erbjuda en bra upplevelse för dina kunder samtidigt som du är kostnadseffektiv.

![Bild som visar ett stort inflöde av data eller begäranden](../media-draft/performance-demand.png))

## <a name="availability-and-recoverability"></a>Tillgänglighet och återställning

Varje arkitekts värsta mardröm är att din arkitektur slutar fungera utan att det går att återställa den. En lyckad molnmiljö är skapad på ett sätt som förutser fel på alla nivåer. En del av att förutse de här felen är att designa ett system som kan återställas från fel, inom den tid som krävs av dina intressenter och kunder.

![Systemfel](../media-draft/system-failure.png)

## <a name="efficiency-and-operations"></a>Effektivitet och drift

Vi vill utforma vår molnmiljö så att den är kostnadseffektiv att driva och utveckla mot. Ineffektivitet och slöseri av molnutgifter ska identifieras för att säkerställa att vi lägger pengarna där vi får mest utväxling av dem. Vi behöver ha en bra övervakningsarkitektur på plats för att kunna identifiera fel och problem innan de inträffar eller minst innan våra kunder märker av dem. Vi måste också ha visibilitet över hur våra program använder de tillgängliga resurserna via ett robust ramverk för övervakning.

![Effektivitet](../media-draft/efficiency.png)

## <a name="design-choices"></a>Utformningsalternativ

I en perfekt arkitektur skulle vi skapa den mest säkra, högpresterande, högtillgängliga och effektiva miljön som går. Men precis som alltid så måste man göra vissa kompromisser. För att skapa en miljö med den högsta nivån för alla pelare så finns det en kostnad. Kostnaden kan vara i pengar, tid till leverans eller driftsmässig flexibilitet. Varje organisation har olika prioriteter som påverkar de designval som görs för varje pelare.

När du skapar en Azure-arkitektur, finns det många saker att tänka på. Du vill att din arkitektur är säker, skalbar, tillgänglig och återställningsbar. Om du vill åstadkomma det så måste du fatta beslut utifrån kostnad och din organisations prioriteringar.
