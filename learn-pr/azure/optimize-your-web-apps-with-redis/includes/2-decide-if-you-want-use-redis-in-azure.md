Bakom din sportwebbplats finns en databas som returnerar data genom att köra frågor. Prestandan sänks dock när belastningen är hög, särskilt under stora sportevenemang. I värdbaserade miljöer medför ökad resursanvändning högre kostnader. Genom att cachelagra data ser du till att webbplatsen körs stabilt och kostnadseffektivt.

## <a name="what-is-caching"></a>Vad är cachelagring?

Cachelagring innebär att data som används ofta lagras i ett minne som ligger mycket nära programmet som använder data. Cachelagring används för att öka prestandan och minska belastningen på dina servrar. Vi använder Redis till att skapa en minnesintern cache som ger utmärkta svarstider och kan förbättra prestandan.

## <a name="what-is-a-redis-cache"></a>Vad är en Redis-cache?

Redis-cache (**RE**mote **DI**ctionary **S**erver) är en lagring för nyckel/värde-par i minnet med öppen källkod. Det är populärt eftersom det är snabbt och kan lagra och manipulera vanliga datatyper såsom strängar, hash-värden och mängder. Det anses även vara utvecklarvänligt eftersom det har stöd för flera språk, till exempel Python, C, C++, C#, Java, JavaScript och fler.

## <a name="what-is-azure-redis-cache"></a>Vad är Azure Redis Cache?

Microsoft Azure Redis Cache är baserat på den populära Redis-cachen med öppen källkod. Det ger dig tillgång till en säker och dedikerad Redis-cache som hanteras av Microsoft. Ett cacheminne som har skapats med Azure Redis Cache är tillgängligt från alla program i Microsoft Azure. Azure Redis Cache används vanligtvis för att förbättra prestanda i system som är starkt beroende av serverdelsdatalager.

Dina cachelagrade data finns i minnet på en Azure-server som kör Redis-cachen i stället för att läsas in från en disk av en databas. Din cache är även mycket skalbar. Du kan ändra storlek och prisnivå när som helst.

## <a name="what-type-of-data-can-be-stored-in-the-cache"></a>Vilken typ av data kan lagras i cacheminnet?

Redis stöder en mängd olika datatyper som allihop är inriktade på _binärt säkra_ strängar. Det innebär att du kan använda valfri binär sekvens för ett värde från en sträng som ”jag-gillar-rocky-road” till innehållet i en bildfil. En tom sträng är också ett giltigt värde.

- Binärt säkra strängar (det vanligaste)
- Lista över strängar
- Osorterade uppsättningar med strängar
- Hash
- Sorterade uppsättningar med strängar
- Karta över strängar

Varje datavärde som är kopplat till en _nyckel_ som kan användas för att hämta värdet från cachen. Redis fungerar bäst med mindre värden (100 kB eller mindre), så fundera över att dela upp större data i flera nycklar. Det är möjligt att lagra större värden (upp till 500 MB), men det ökar nätverkets svarstid och kan orsaka problem med cachelagring och otillräckligt med minne om cachen inte är konfigurerad för att upphäva gamla värden.

## <a name="what-is-a-redis-key"></a>Vad är en Redis-nyckel?
Redis-nycklar är också binärt säkra strängar. Här följer några riktlinjer för att välja nycklar:

- Undvik långa nycklar. De förbrukar mer minne och kräver längre sökningstider eftersom de måste jämföras byte för byte. Om du vill använda en binär blob som nyckel ska du generera ett unikt hash-värde och använda det som nyckel. Den maximala storleken för en nyckel är 512 MB, men du bör _aldrig_ använda en nyckel med den storleken.
- Använd nycklar som kan identifiera data. Till exempel skulle ”sport:fotboll;datum:2008-02-02” vara en bättre nyckel än ”fb:8-2-2”. Det förstnämnda är mer lättläst och den extra storleken är försumbar. Hitta balansen mellan storlek och läsbarhet.
- Använd en konvention. Ett bra exempel är ”objekt:id”, som i ”sport:fotboll”. 

## <a name="how-is-data-stored-in-a-redis-cache"></a>Hur lagras data i en Redis-cache?

Data i Redis lagras i _**noder**_ och _**kluster**_.

**Noder** är ett utrymme i Redis där dina data lagras.

**Kluster** är grupper med tre eller fler noder som datamängden delas upp mellan. Kluster är användbara eftersom driften fortsätter om en nod misslyckas eller inte kan kommunicera med resten av klustret.

## <a name="what-are-redis-caching-architectures"></a>Vad är Redis-cachelagringsarkitekturer?

Med Redis-cachelagringsarkitektur distribuerar vi våra data i cacheminnet. Redis distribuerar data på tre huvudsakliga sätt:

1. **En nod**
1. **Flera noder**
1. **Kluster**

Arkitekturerna för Redis-cachelagring i Azure är indelade i nivåer:

### <a name="basic-cache"></a>Basic-cache

En grundläggande cache som ger en Redis-cache med _**en nod**_. Den fullständiga datamängden lagras i en enda nod. Den här nivån är perfekt för utveckling, testning och icke-kritiska arbetsbelastningar.

### <a name="standard-cache"></a>Standard-cache

Med standard-cache får du en arkitektur med _**flera noder**_. Redis replikerar en cache i en primär/sekundär konfiguration med två noder. Azure hanterar replikeringen mellan de två noderna. Det här är en produktionsklar cache med överordnad/underordnad replikering.

### <a name="premium-tier"></a>Premiumnivå

På premiumnivån får du alla funktioner från standardnivån, men dessutom möjlighet att bevara data, ta ögonblicksbilder och säkerhetskopiera data. På den här nivån kan du skapa ett Redis-kluster där data delas mellan flera Redis-noder för att öka mängden tillgängligt minne. Premium-nivån har även stöd för Azure Virtual Network som ger dig fullständig kontroll över dina anslutningar, undernät, IP-adresser och nätverksisolering. Den här nivån har även geo-replikering så att du kan säkerställa att dina data är nära den app som använder dem.

## <a name="summary"></a>Sammanfattning

En databas passar bra till att lagra stora mängder data, när du ska använd dem medför det svarstider. Du skickar en fråga. Servern tolkar frågan, letar upp data och returnerar dem. Servrar har även kapacitetsbegränsningar för hantering av sådana förfrågningar. Om du skickar för många blir datahämtningen troligtvis långsammare. Med cachelagring lagrar du data som efterfrågas ofta i ett minne som kan returnera data snabbare än vid körning av frågor mot en databas, och det här bör ge både kortare svarstider och bättre prestanda. Med Azure Redis Cache får du tillgång till en säker, dedikerad och skalbar Redis-cache som lagras i Azure och hanteras av Microsoft.
