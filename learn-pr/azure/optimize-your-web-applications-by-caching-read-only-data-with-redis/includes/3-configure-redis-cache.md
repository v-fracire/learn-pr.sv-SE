Ditt utvecklingsteam för sportstatistik har kommit fram till att cachelagring avsevärt skulle kunna förbättra prestanda för nyligen begärda data. Nästa steg är att skapa och konfigurera en Azure Redis Cache.

### <a name="create-and-configure-the-redis-cache"></a>Skapa och konfigurera Redis Cache

1. Du kan lägga till en Redis Cache från databasresurser i Azure-portalen.

1. Ange ett globalt unikt namn. Namnet måste vara en sträng mellan 1 och 63 tecken och får endast innehålla siffror, bokstäver och tecknet ”-”. Cachenamnet får inte inledas eller avslutas med tecknet ”-” eller ha flera ”-” i följd.

1. Ange prenumerationen där den här nya Azure Redis Cache-instansen skapas.

1. Namnet på den resursgrupp där du vill skapa din cache. Genom att lägga alla resurser för en app i en grupp kan du hantera dem tillsammans.

1. Välj en plats som ligger nära datakonsumenten.

    > [!Note]
    > Den är mycket viktigare att cachen är nära datakonsumenten än datalagret.

1. Välj prisnivå. Kom ihåg att dra nytta av den datapersistens som du behöver för att välja en premiumnivå.

    - Med Premium-nivån kan du bevara data på ett av två sätt för att tillhandahålla katastrofåterställning:
        - RDB-persistens tar en periodisk ögonblicksbild och kan återskapa cachen med hjälp av ögonblicksbilden om fel uppstår.

            ![RDB-persistens](../media-draft/3-redis-persistence-1.png)

        - AOF-persistens sparar varje skrivåtgärd till en logg som sparas minst en gång per sekund. Detta skapar större filer än RDB men har mindre dataförlust.

            ![AOF-persistens](../media-draft/3-redis-persistence-2.png)

    - Skydda din cache med ett virtuellt nätverk Om du har en Redis Cache på Premium-nivå kan du distribuera den till ett virtuellt nätverk i molnet. Cachen blir bara tillgänglig för andra virtuella datorer och program i samma virtuella nätverk.

    - Distribuera din cache med klustring Med en Redis Cache på Premium-nivå kan du implementera klustring för att automatiskt dela din datamängd bland flera noder. För att implementera klustring anger du antalet shards till högst 10. Kostnaden är kostnaden för den ursprungliga noden multiplicerat med antalet shards.

    - Migrera från Managed Cache Service Beroende på de Managed Cache Service-funktioner du använder går det att migrera program som använder Azure Managed Cache Service till Azure Redis Cache med minimala ändringar i ditt program. API:erna inte är exakt samma, men de liknar varandra. Mycket av din befintliga kod som kommer åt cachen med Managed Cache Service kan återanvändas med minimala ändringar.

### <a name="configure-your-client"></a>Konfigurera din klient

Klientprogrammet behöver konfigureras för att använda Redis Cache. **StackExchange.Redis** är en populär Redis-klient för höga prestanda för språket .NET. Du lägger till Redis-klienten med hjälp av **NuGet Package Manager** i Visual Studio via kommandot **Install-Package**.

Du behöver sedan konfigurera klienten att ansluta till din cache. Skapa en textfil med filnamnstillägget .config som innehåller inställningarna för värdnamnet och primär eller sekundär nyckel. Filen ska ha följande struktur:

```XML
    <appSettings>
    <add key="CacheConnection" value="HOST-NAME.redis.cache.windows.net,abortConnect=false,ssl=true,password=PRIMARY-KEY"/>
    </appSettings>
```

Du redigerar **App.config**-filen och uppdaterar den med ett appSettings-filattribut som refererar till den config-fil som du nyss skapade.

### <a name="what-are-access-keys"></a>Vad är åtkomstnycklar?

När du ansluter till en Azure Redis Cache-instans behöver cacheklienter värdnamnet, portarna och nycklarna för cachen. Du kan hämta den här informationen i Azure-portalen.

### <a name="connection-settings"></a>Inställningar för anslutning

För att ansluta till din Azure Redis Cache behöver du värdnamnet och åtkomstnyckeln. Värdnamnet är Internetadressen för din cache, till exempel *sportsresults.redis.cache.windows.net*. Åtkomstnyckeln fungerar som ett lösenord för cachen. Det finns primära och sekundära åtkomstnycklar. Du kan använda endera av nycklarna, men det tillhandahålls två ifall du behöver ändra den primära nyckeln. Du kan växla alla dina klienter till den sekundära nyckeln och sedan ändra till den primära nyckeln, vilket kan förhindra förlust av tjänsten.
