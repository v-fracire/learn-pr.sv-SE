Ditt utvecklingsteam för sportstatistik har kommit fram till att cachelagring avsevärt skulle kunna förbättra prestanda för nyligen begärda data. Nästa steg är att skapa och konfigurera en Azure Redis Cache-instans.

## <a name="create-and-configure-the-azure-redis-cache-instance"></a>Skapa och konfigurera Azure Redis Cache-instansen

Du kan skapa en Redis-cache med hjälp av Azure-portalen, Azure CLI eller Azure PowerShell. Det finns flera parametrar måste du bestämma om du vill konfigurera cachen korrekt efter dina egna behov.

### <a name="name"></a>Namn

Redis-cache måste ett globalt unikt namn. Namnet måste vara unikt i Azure eftersom den används för att skapa en offentlig URL för att ansluta och kommunicera med tjänsten.

Namnet måste vara mellan 1 och 63 tecken, består av siffror, bokstäver, och '-' tecken. Cachenamnet får inte inledas eller avslutas med tecknet ”-” eller ha flera ”-” i följd.

### <a name="resource-group"></a>Resursgrupp

Azure Redis Cache är en hanterad resurs och behov en _resursgrupp_ ägare. Du kan skapa en ny resursgrupp eller använda ett befintligt namn i en susbcription som du tillhör.

### <a name="location"></a>Plats

Du måste bestämma där Redis-cache ska finnas fysiskt genom att välja en Azure-region. Du bör alltid placera din cache-instans och ditt program i samma region. Ansluta till en cache i en annan region kan avsevärt öka svarstiden och minska tillförlitlighet. Om du ansluter till cachen utanför Azure, väljer du en plats i närheten av där programmet förbrukar data körs.

> [!IMPORTANT]
> Placera Redis-cache så nära data _konsument_ som möjligt.

### <a name="pricing-tier"></a>Prisnivå

Som vi nämnde i den senaste enheten, finns det tre olika prisnivåer för Azure Redis Cache.

- **Grundläggande**: grundläggande cachelagring som är perfekt för utveckling/testning. Är begränsad till en enda server, 53 GB minne och 20 000 anslutningar. Det finns inget serviceavtal för den här tjänstnivån.
- **Standard**: produktion cachelagring som har stöd för replikering och innehåller ett serviceavtal på 99,99%. Den stöder två servrar (master/slave) och har samma minne/anslutningsbegränsningar som Basic-nivån.
- **Premium**: Enterprise-nivå som bygger på nivån Standard och har stöd för persistence, klustring och skalbar cache. Det här är den högsta nivån som utför med upp till 530 GB minne och 40 000 samtidiga anslutningar.

Du kan styra mängden cacheminne som är tillgängliga på varje nivå – det här alternativet väljs genom att välja en cachenivå från C0 C6 för Basic/Standard och P0-P4 för Premium. Kontrollera den [prissättningssidan](https://azure.microsoft.com/en-us/pricing/details/cache/) fullständig information.

> [!TIP]
> Microsoft rekommenderar att du alltid använda Standard eller Premium-nivån för produktionssystem. Basic-nivån är en enskild nod system med ingen replikering av data och inget serviceavtal. Kan också använda minst en C1 cache. C0 cacheminnen är verkligen avsedda för enkel utveckling och testning eftersom de har en delad CPU-kärna och mycket lite minne.

Premium-nivån kan du bevara data på två sätt att tillhandahålla katastrofåterställning:

1. RDB-persistens tar en periodisk ögonblicksbild och kan återskapa cachen med hjälp av ögonblicksbilden om fel uppstår.

    ![Skärmbild av Azure Portal med alternativen för RDB-persistens på en ny Redis Cache-instans.](../media/3-redis-persistence-1.png)

2. AOF-persistens sparar varje skrivåtgärd till en logg som sparas minst en gång per sekund. Detta skapar större filer än RDB men har mindre dataförlust.

    ![Skärmbild av Azure-portalen med alternativen för AOF-persistens på en ny Redis Cache-instans.](../media/3-redis-persistence-2.png)

Det finns flera andra inställningar som bara är tillgängliga för den **Premium** nivå.

### <a name="virtual-network-support"></a>Stöd för Virtual Network

Om du skapar en premium-nivån Redis-cache kan distribuera du det till ett virtuellt nätverk i molnet. Din cache blir bara tillgänglig för andra virtuella datorer och program i samma virtuella nätverk. Detta ger en högre säkerhetsnivå när din tjänst och cache finns både i Azure och är anslutna via ett Azure-nätverk VPN.

### <a name="clustering-support"></a>Redundanskluster

Med en Redis Cache på Premium-nivå kan du implementera klustring för att automatiskt fördela din datamängd mellan flera noder. För att implementera klustring anger du antalet shards till högst 10. Kostnaden är kostnaden för den ursprungliga noden, multiplicerat med antalet shards.

## <a name="accessing-the-redis-instance"></a>Åtkomst till Redis-instans

Redis stöder en uppsättning [kända kommandon](https://redis.io/commands). Ett normalt utfärdas som `COMMAND parameter1 parameter2 parameter3`.

Här följer några vanliga kommandon som du kan använda:

| Kommando | Beskrivning |
|---------|-------------|
| `ping` | Pinga servern. Returnerar ”PONG”. |
| `set [key] [value]` | Anger ett nyckel/värde i cacheminnet. Returnerar ”OK” på ska lyckas. |
| `get [key]` | Hämtar ett värde från cachen. |
| `exists [key]` | Returnerar '1' om den **nyckel** finns i cacheminnet, '0' om inte. |
| `type [key]` | Returnerar typen som är kopplad till värdet för den angivna **nyckeln**. |
| `incr [key]` | Öka det angivna värdet som är associerade med **nyckel** av '1'. Värdet måste vara ett heltal eller ett double-värde. Det här returnerar det nya värdet. |
| `incrby [key] [amount]` | Öka det angivna värdet som är associerade med **nyckel** av angiven mängd. Värdet måste vara ett heltal eller ett double-värde. Det här returnerar det nya värdet. |
| `del [key]` | Tar bort värdet som är associerade med den **nyckeln**. |
| `flushdb` | Ta bort _alla_ nycklar och värden i databasen. |

Redis har ett kommandoradsverktyg (**redis-cli**) du kan använda för att experimentera direkt med dessa kommandon. Här följer några exempel.

```output
> set somekey somevalue
OK
> get somekey
"somevalue"
> exists somekey
(string) 1
> del somekey
(string) 1
> exists somekey
(string) 0
```

Här är ett exempel på hur du arbetar med den `INCR` kommandon. Här är praktisk eftersom de ger atomiska steg _över flera program_ som använder cachen.

```output
> set counter 100
OK
> incr counter
(integer) 101
> incrby counter 50
(integer) 151
> type counter
(integer)
```

### <a name="adding-an-expiration-time-to-values"></a>Att lägga till en förfallotid till värden

Cachelagring är viktig eftersom den ger oss till lagrar vanliga värden i minnet. Men behöver vi också ett sätt att gå ut värden när de är inaktuella. I Redis detta görs genom att tillämpa en _livslängd_ (TTL) till en nyckel.

När TTL-Perioden går ut raderas automatiskt nyckeln, precis som när kommandot del. det har utfärdats. Här följer några anteckningar på TTL förfallodatum.

- Kontolösenordet kan ställas in med sekunder eller millisekunder precision.
- Upphör att gälla tid lösning är alltid 1 millisekund.
- Information om upphör att gälla replikeras och sparas på disk, på i stort sett tiden när din Redis server förblir Stoppad (Detta innebär att Redis sparar det datum då en nyckel upphör att gälla).

Här är ett exempel på ett förfallodatum:

```output
> set counter 100
OK
> expire counter 5
(integer) 1
> get counter
100
... wait ...
> get counter
(nil)
```

## <a name="accessing-a-redis-cache-from-a-client"></a>Åtkomst till en Redis-cache från en klient

Om du vill ansluta till en Azure Redis Cache-instans, måste flera olika typer av information. Klienter måste värdnamn, port och en åtkomstnyckel för cachen. Du kan hämta den här informationen i Azure-portalen via den **Inställningar > åtkomstnycklar** sidan. 

- Värdnamnet är den offentliga Internet-adressen för din cache, som har skapats med hjälp av namnet på cachen. Till exempel `sportsresults.redis.cache.windows.net`.

- Åtkomstnyckeln fungerar som ett lösenord för din cache. Det finns två nycklar som har skapats: primära och sekundära. Du kan använda någon av nycklarna måste anges två om du behöver ändra den primära nyckeln. Du kan växla alla klienter till den sekundära nyckeln och återskapa den primära nyckeln. Detta skulle blockera de program som använder den ursprungliga primära nyckeln. Microsoft rekommenderar att du regelbundet återskapar nycklar – nästan samma sätt som du skulle göra dina personliga lösenord.

> [!WARNING]
> Åtkomstnycklarna ska betraktas som konfidentiell information, behandla dem som du skulle ett lösenord. Alla som har en åtkomstnyckel kan utföra alla åtgärder för cacheminnet!
