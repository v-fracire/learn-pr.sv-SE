Ditt utvecklingsteam för sportstatistik har kommit fram till att cachelagring avsevärt skulle kunna förbättra prestanda för nyligen begärda data. Nästa steg är att skapa och konfigurera en Azure Redis Cache-instans.

## <a name="create-and-configure-the-azure-redis-cache-instance"></a>Skapa och konfigurera Azure Redis Cache-instansen

Du kan skapa en Redis-cache med Azure-portalen, Azure CLI eller Azure PowerShell. Det finns flera parametrar som du måste bestämma för att konfigurera cachen korrekt för dina syften.

### <a name="name"></a>Namn

Redis-cachen behöver ett globalt unikt namn. Namnet måste vara unikt i Azure eftersom den används för att skapa en offentlig URL för att ansluta och kommunicera med tjänsten.

Namnet måste vara mellan 1 och 63 tecken, innehålla siffror, bokstäver, och bindestreck. Cachenamnet får inte inledas eller avslutas med tecknet - eller ha flera - i följd.

### <a name="resource-group"></a>Resursgrupp

Azure Redis Cache är en hanterad resurs och behöver en _resursgrupp_-ägare. Du kan antingen skapa en ny resursgrupp eller använda en befintlig en på en prenumeration du är del av.

### <a name="location"></a>Plats

Du måste bestämma var din Redis-cache kommer att finnas fysiskt genom att välja en Azure-region. Du ska alltid placera din cacheinstans och ditt program i samma region. Om du ansluter till en cache i en annan region kan latensen öka och tillförlitligheten minska avsevärt. Om du ansluter till cachen utanför Azure ska du välja en plats som ligger nära platsen där programmet som förbrukar data körs.

> [!IMPORTANT]
> Placera din Redis-cache så nära data_konsumenten_ som möjligt.

### <a name="pricing-tier"></a>Prisnivå

Som vi nämnde i den senaste enheten, finns det tre olika prisnivåer för Azure Redis Cache.

- **Grundläggande**: grundläggande cachelagring är perfekt för utveckling/testning. Är begränsad till en enda server, 53 GB minne och 20 000 anslutningar. Det finns inget serviceavtal för den här tjänstnivån.
- **Standard**: cachelagring för produktion som stöder replikering och innehåller ett serviceavtal på 99,99 %. Den stöder två servrar (över-/underordnad) och har samma minnes-/anslutningsbegränsningar som Basic-nivån.
- **Premium**: Enterprise-nivå som bygger på nivån Standard och har stöd för persistence, klustring och skala ut cache. Det här är den högsta prestandanivån med upp till 530 GB minne och 40 000 samtidiga anslutningar.

Du kan styra mängden tillgängligt cacheminne på varje nivå – det väljs genom att välja en cachenivå från C0-C6 för Basic/Standard och P0-P4 för Premium. Mer information finns på [prissättningssidan](https://azure.microsoft.com/pricing/details/cache/).

> [!TIP]
> Microsoft rekommenderar alltid att du använder Standard- eller Premium-nivån för produktionssystem. Basic-nivån är ett system med en nod utan datareplikering och serviceavtal. Använd också minst ett C1-cacheminne. C0-cacheminnen är avsedda för enkla scenarier för utveckling/testning eftersom de har en delad processorkärna och mycket lite minne.

Med Premium-nivån kan du bevara data på två sätt för att tillhandahålla katastrofåterställning:

1. RDB-persistens tar en periodisk ögonblicksbild och kan återskapa cachen med hjälp av ögonblicksbilden om fel uppstår.

    ![Skärmbild av Azure Portal med alternativen för RDB-persistens på en ny Redis Cache-instans.](../media/3-redis-persistence-1.png)

2. AOF-persistens sparar varje skrivåtgärd till en logg som sparas minst en gång per sekund. Detta skapar större filer än RDB men har mindre dataförlust.

    ![Skärmbild av Azure-portalen med alternativen för AOF-persistens på en ny Redis Cache-instans.](../media/3-redis-persistence-2.png)

Det finns flera andra inställningar som bara är tillgängliga för **Premium**-nivån.

### <a name="virtual-network-support"></a>Stöd för virtuellt nätverk

Om du skapar en Redis Cache på Premium-nivå kan du distribuera den till ett virtuellt nätverk i molnet. Din cache blir bara tillgänglig för andra virtuella datorer och program i samma virtuella nätverk. Det ger en högre säkerhetsnivå när din tjänst och cache finns i både Azure och är anslutna via ett virtuellt privat Azure-nätverk.

### <a name="clustering-support"></a>Klusterstöd

Med en Redis Cache på Premium-nivå kan du implementera klustring för att automatiskt fördela din datamängd mellan flera noder. Implementera klustring genom att ange antalet shards till högst 10. Kostnaden som uppstår är kostnaden för den ursprungliga noden multiplicerat med antalet shards.

## <a name="accessing-the-redis-instance"></a>Åtkomst till Redis-instans

Redis stöder en uppsättning av [kända kommandon](https://redis.io/commands). Ett kommando utfärdas vanligen som `COMMAND parameter1 parameter2 parameter3`.

Här följer några vanliga kommandon som du kan använda:

| Kommando | Beskrivning |
|---------|-------------|
| `ping` | Pinga servern. Returnerar ”PONG”. |
| `set [key] [value]` | Ställer in en nyckel/värde i cacheminnet. Returnerar ”OK” om åtgärden lyckades. |
| `get [key]` | Hämtar ett värde från cachen. |
| `exists [key]` | Returnerar ”1” om **nyckeln** finns i cacheminnet och ”0” om det inte gör det. |
| `type [key]` | Returnerar typen som är associerad till värdet för den angivna **nyckeln**. |
| `incr [key]` | Öka det angivna värdet som är associerat med **nyckeln** med 1. Värdet måste vara ett heltal eller ett double-värde. Det här returnerar det nya värdet. |
| `incrby [key] [amount]` | Öka det angivna värdet som är associerat med **nyckeln** med den angivna mängden. Värdet måste vara ett heltal eller ett double-värde. Det här returnerar det nya värdet. |
| `del [key]` | Tar bort värdet som är associerat med **nyckeln**. |
| `flushdb` | Ta bort _alla_ nycklar och värden i databasen. |

Redis har ett kommandoradsverktyg (**redis-cli**) som du kan använda för att experimentera direkt med dessa kommandon. Här följer några exempel.

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

Här är ett exempel på hur du arbetar med kommandot `INCR`. De är praktiska eftersom de erbjuder atomiska steg _över flera program_ som använder cacheminnet.

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

### <a name="adding-an-expiration-time-to-values"></a>Lägga till en förfallotid för värden

Cachelagring är viktig eftersom den gör att vi kan lagra vanliga värden i minnet. Men vi behöver också ett sätt att se till att värden förfaller när de är inaktuella. I Redis gör du det genom att tillämpa TTL (_time to live_) på en nyckel.

När TTL går ut tas nyckeln bort automatiskt, precis som om kommandot DEL skulle användas. Här är några anteckningar om TTL-förfallotider.

- Förfallotider kan ställas in med sekund- eller millisekundprecision.
- Upplösningen för förfallotid är alltid 1 millisekund.
- Information om förfallotid replikeras och sparas på disk, och tiden passerar praktiskt taget när Redis-servern förblir stoppad (det innebär att Redis sparar det datum då en nyckel upphör att gälla).

Här är ett exempel på en förfallotid:

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

## <a name="accessing-a-redis-cache-from-a-client"></a>Få åtkomst till Redis Cache från en klient

Om du vill ansluta till en Azure Redis Cache-instans så behöver du flera olika typer av information. Klienter behöver värdnamn, port och en åtkomstnyckel för cachen. Du kan hämta den här informationen i Azure-portalen via sidan **Inställningar > Åtkomstnycklar**. 

- Värdnamnet är den offentliga Internet-adressen för cacheminnet, som skapades med cachenamnet. Till exempel `sportsresults.redis.cache.windows.net`.

- Åtkomstnyckeln fungerar som lösenord för cachen. Två nycklar har skapats: primär och sekundär. Du kan använda endera av nycklarna, men det tillhandahålls två ifall du behöver ändra den primära nyckeln. Du kan växla alla dina klienter till den sekundära nyckeln och återskapa primärnyckeln. Det skulle blockera de program som använder den ursprungliga primärnyckeln. Microsoft rekommenderar att du regelbundet återskapar nycklar – precis som du bör göra med dina personliga lösenord.

> [!WARNING]
> Dina åtkomstnycklar ska betraktas som konfidentiell information, så behandla dem som du behandlar lösenord. Vem som helst som har en åtkomstnyckel kan utföra vilka åtgärder som helst i cacheminnet!
