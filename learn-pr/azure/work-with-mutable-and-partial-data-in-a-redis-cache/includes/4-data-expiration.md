Data är inte alltid permanenta och ibland vill du ta bort dem vid en viss tidpunkt. I till exempel ett snabbmeddelandeprogram kan användarna ange ett visningsnamn för gruppchatten. Men du vill att namnet ska återställas efter en timme. Det kan du åstadkomma genom skriva egen logik på serversidan som ställer in en timer på en timme och tar bort namnet. Men Azure Redis Cache stöder förfallotid för data, som är en funktion som gör detta automatiskt utan att skriva ytterligare logik.

Här får du lära dig om vanliga Redis-kommandon för att implementera förfallotid.

## <a name="what-is-data-expiration"></a>Vad är förfallotid?

Förfallotid för data är en funktion som automatiskt kan ta bort en nyckel och ett värde i cacheminnet efter en angiven tid.

## <a name="why-use-data-expiration"></a>Varför använda förfallotid?

Förfallotid för data används ofta i situationer där data du lagrar är tillfälliga.  Det här är viktigt eftersom Azure Redis Cache är en minnesintern databas och du inte har så mycket minne tillgängligt att använda som du skulle ha om du lagrar på disk. Eftersom du har begränsat lagringsutrymme med Azure Redis Cache vill du försäkra dig om att du bara lagrar viktiga data. Om data inte måste finnas på längre sikt bör du ange en förfallotid.

## <a name="how-to-use-data-expiration-in-azure-redis-cache"></a>Så här använder du förfallotid i Azure Redis Cache

Det finns olika kommandon för att implementera och hantera förfallotid i Azure Redis Cache:

- `EXPIRE`: anger timeout för en nyckel i sekunder
- `PEXPIRE`: anger timeout för en nyckel i millisekunder
- `EXPIREAT`: anger timeout för en nyckel med en absolut Unix-tidsstämpel i sekunder
- `PEXPIREAT`: anger timeout för en nyckel med en absolut Unix-tidsstämpel i millisekunder
- `TTL`: returnerar den återstående tiden som en nyckel ska finnas i sekunder
- `PTTL`: returnerar den återstående tiden som en nyckel ska finnas i millisekunder
- `PERSIST`: gör så att en nyckel aldrig upphör

Det vanligaste kommandot är `EXPIRE` och vi använder det genom hela modulen.

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a>Exempel på förfallotid med C# och ServiceStack.Redis

Kom ihåg att vid användning av ett klientbibliotek som **ServiceStack.Redis** behöver vi inte bekymra oss om att komma ihåg Azure Redis Cache-kommandon på låg nivå. Vi kan i stället använda enkla C#-metoder.

```csharp
public static void SetGroupChatName(string groupChatID, string chatName)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create a key for group chat display names
        string key = groupChatID + "displayName";

        //Set the group chat display name
        redisClient.SetValue(key, chatName);

        //Set the expiration for one hour
        redisClient.Expire(key, 3600);
    }
}
```

Med Azure Redis Cache kan du ta bort data automatiskt efter en viss tid genom att använda förfallotid. Det här är viktigt eftersom Azure Redis Cache är en minnesintern databas och du inte har så mycket minne tillgängligt som du skulle ha med lagring på disk. Om data du lagrar inte måste finnas på längre sikt bör du ange en förfallotid.