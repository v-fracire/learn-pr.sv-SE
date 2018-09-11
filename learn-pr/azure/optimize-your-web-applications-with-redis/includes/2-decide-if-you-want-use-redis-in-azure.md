Bakom din sportwebbplats finns en databas som returnerar data genom att köra frågor. Men prestandan sänks när belastningen är hög, särskilt under stora sportevenemang. I värdbaserade miljöer medför ökad resursanvändning högre kostnader. Cachelagring av data ser till att din webbplats körs med hög prestanda och kostnadseffektivt.

## <a name="what-is-caching"></a>Vad är cachelagring?

Cachelagring innebär att data som används ofta lagras i minne som ligger mycket nära det program som använder dem. Cachelagring används för att öka prestandan och minska belastningen på dina servrar. Vi använder Redis för att skapa en minnesintern cache som kan ge utmärkt svarstid och potentiellt avsevärt förbättrad prestanda.

## <a name="what-is-a-redis-cache"></a>Vad är en Redis-cache?

Redis (**RE**mote **DI**ctionary **S**erver)-cache är en lagring av nyckel/värde-par i minnet med öppen källkod. Redis är populärt eftersom det är snabbt och kan lagra och manipulera vanliga datatyper som strängar, hash-värden och mängder. Det anses även vara utvecklarvänligt eftersom det har stöd för flera språk, till exempel ASP.NET, Python, C, C++, C#, Java, JavaScript, Node.js och fler.

## <a name="what-is-azure-redis-cache"></a>Vad är Azure Redis Cache?

Microsoft Azure Redis Cache är baserat på den populära Redis-cachen med öppen källkod. Det ger dig tillgång till en säker och dedikerad Redis-cache som hanteras av Microsoft. Ett cacheminne som har skapats med Azure Redis Cache är tillgängligt från alla program i Microsoft Azure. Azure Redis Cache används vanligtvis för att förbättra prestanda i system som är starkt beroende av serverdelsdatalager.

Dina cachelagrade data finns i minnet på en Azure-server som kör Redis-cachen i stället för att läsas in från en disk av en databas. Din cache är även mycket skalbar. Du kan ändra storlek och prisnivå när som helst.

## <a name="how-is-data-stored-in-a-redis-cache"></a>Hur lagras data i en Redis-cache?

Data i Redis lagras i _**noder**_ och _**kluster**_.

**Noder** är ett utrymme i Redis där dina data lagras.

**Kluster** är grupper med tre eller fler noder som datamängden delas upp mellan. Kluster är användbara eftersom driften fortsätter om en nod misslyckas eller inte kan kommunicera med resten av klustret.

## <a name="what-are-redis-caching-architectures"></a>Vad är Redis-cachelagringsarkitekturer?

Redis-cachelagringsarkitektur är det sätt som vi distribuerar våra data i cacheminnet. Redis har tre huvudsakliga sätt att distribuera data:

1. **En nod**
1. **Flera noder**
1. **Klustrad**

Redis-cachelagringsarkitekturer delas upp i Azure i nivåer:

### <a name="basic-cache"></a>Basic-cache

Med en Basic-cache får du en Redis-cache med _**en nod**_. Den fullständiga datamängden lagras i en enda nod. Den här nivån är perfekt för utveckling, testning och icke-kritiska arbetsbelastningar.

### <a name="standard-cache"></a>Standard-cache

Standard-cache skapar arkitekturer med _**flera noder**_. Redis replikerar en cache i en primär/sekundär konfiguration med två noder. Azure hanterar replikeringen mellan de två noderna. Det här är en produktionsklar cache med överordnad/underordnad replikering.

### <a name="premium-tier"></a>Premiumnivå

Premiumnivån innehåller funktionerna i standardnivån, men lägger till möjligheten att bevara data, ta ögonblicksbilder och säkerhetskopiera data. Med den här nivån kan du skapa ett Redis-kluster som gör shards av data över flera Redis-noder för att öka tillgängligt minne. Premium-nivån har även stöd för Azure Virtual Network för att ge dig fullständig kontroll över dina anslutningar, undernät, IP-adresser och nätverksisolering. Den här nivån har även geo-replikering så att du kan säkerställa att dina data är nära den app som använder dem.

## <a name="summary"></a>Sammanfattning

En databas är utmärkt när stora mängder data ska lagras, men det finns en ofrånkomlig svarstid för att söka efter data. Du skickar en fråga. Servern tolkar frågan, letar upp data och returnerar dem. Servrar har även kapacitetsbegränsningar för hantering av begäranden. Om för många begäranden görs blir datahämtningen troligtvis långsammare. Cachelagring lagrar data som begärs ofta i minne och som kan returneras snabbare än vid körning av frågor mot en databas, vilket bör ge kortare svarstid och högre prestanda. Med Azure Redis Cache får du tillgång till en säker, dedikerad och skalbar Redis-cache som lagras i Azure och hanteras av Microsoft.
