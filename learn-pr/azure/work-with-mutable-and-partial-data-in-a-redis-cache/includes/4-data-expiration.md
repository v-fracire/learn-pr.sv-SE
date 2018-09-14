Data är inte alltid permanent och tider som du vill ta bort den vid en viss tidpunkt. Användare kan till exempel i programmet för snabbmeddelanden ange visningsnamn för gruppen chatten. Men vill du namnet att återställa efter en timme. Du kan åstadkomma detta genom att skriva egen logik för serversidan som ställa in en timme timer och ta bort namnet. Azure Redis Cache har dock stöd för förfallotid är en funktion som gör detta automatiskt utan att skriva ytterligare logik.

Här kan lära du dig om de vanliga Redis-kommandona för att implementera data upphör att gälla.

## <a name="what-is-data-expiration"></a>Vad är data upphör att gälla?

Förfallotid är en funktion som kan automatiskt ta bort en nyckel och värde i cacheminnet efter en angiven tid.

## <a name="why-use-data-expiration"></a>Varför använda data upphör att gälla?

Förfallotid används ofta i situationer där de data du lagrar är tillfällig.  Detta är viktigt eftersom Azure Redis Cache är en databas i minnet och du inte har så mycket minne som är tillgängliga för användning som om du lagring på disk. Eftersom du har begränsad storage med Azure Redis Cache, som du vill kontrollera att du bara lagrar data som är viktiga. Om data inte vill att finnas kvar under en längre tid kan du se till att ange ett förfallodatum.

## <a name="how-to-use-data-expiration-in-azure-redis-cache"></a>Hur du använder data upphör att gälla i Azure Redis Cache

Det finns olika kommandon för att implementera och hantera förfallotid i Azure Redis Cache:

- `EXPIRE`: Anger timeout för en nyckel i sekunder
- `PEXIRE`: Anger timeout för en nyckel i millisekunder
- `EXPIREAT`: Anger timeout för en nyckel med en absolut Unix-tidsstämpel i sekunder
- `PEXPIREAT`: Anger timeout för en nyckel med en absolut Unix-tidsstämpel i millisekunder
- `TTL`: Returnerar den återstående tiden som en nyckel har livstid i sekunder
- `PTTL`: Returnerar den återstående tiden som en nyckel har TTL-värde i millisekunder
- `PERSIST`: Gör en nyckel som upphör aldrig att gälla

Det vanligaste kommandot är `EXPIRE`, och vi ska använda det i hela den här modulen.

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a>Exempel på data upphör att gälla med C# och ServiceStack.Redis

Kom ihåg att när du använder ett klientbibliotek som **ServiceStack.Redis**, vi inte behöver bekymra dig om att komma ihåg på den låga nivån Azure Redis Cache-kommandon. Vi kan i stället använda enkla C#-metoder.

```csharp
public static void SetGroupChatName(string groupChatID, string chatName)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create a key for group chat display names
        string key = groupChatID + "displayName";

        //Set the group chat display name
        redisClient.Set(key, chatName);

        //Set the expiration for one hour
        redisClient.Expire(key, 3600);
    }
}
```

Azure Redis Cache kan du ta bort data automatiskt efter en viss tidsperiod med hjälp av data upphör att gälla. Detta är viktigt eftersom Azure Redis Cache är en databas i minnet och du inte har så mycket minne som du skulle göra med att lagra data på disken. Om de data du lagrar inte vill att finnas kvar under en längre tid kan du se till att ange ett förfallodatum.