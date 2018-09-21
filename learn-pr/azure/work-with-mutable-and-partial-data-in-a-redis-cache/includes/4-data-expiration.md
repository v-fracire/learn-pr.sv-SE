<span data-ttu-id="4cb0e-101">Data är inte alltid permanenta och ibland vill du ta bort dem vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-101">Data is not always permanent, and there are times you would like to delete it at a specific time.</span></span> <span data-ttu-id="4cb0e-102">I till exempel ett snabbmeddelandeprogram kan användarna ange ett visningsnamn för gruppchatten.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-102">For example, in your instant messaging application, users can set the display name of the group chat.</span></span> <span data-ttu-id="4cb0e-103">Men du vill att namnet ska återställas efter en timme.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-103">However, you want the name to reset after one hour.</span></span> <span data-ttu-id="4cb0e-104">Det kan du åstadkomma genom skriva egen logik på serversidan som ställer in en timer på en timme och tar bort namnet.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-104">You could accomplish this by writing your own server-side logic that set an hour timer and deleted the name.</span></span> <span data-ttu-id="4cb0e-105">Men Azure Redis Cache stöder förfallotid för data, som är en funktion som gör detta automatiskt utan att skriva ytterligare logik.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-105">However, Azure Redis Cache supports data expiration, which is a feature that does this automatically without writing additional logic.</span></span>

<span data-ttu-id="4cb0e-106">Här får du lära dig om vanliga Redis-kommandon för att implementera förfallotid.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-106">Here, you'll learn about the common Redis commands to implement data expiration.</span></span>

## <a name="what-is-data-expiration"></a><span data-ttu-id="4cb0e-107">Vad är förfallotid?</span><span class="sxs-lookup"><span data-stu-id="4cb0e-107">What is data expiration?</span></span>

<span data-ttu-id="4cb0e-108">Förfallotid för data är en funktion som automatiskt kan ta bort en nyckel och ett värde i cacheminnet efter en angiven tid.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-108">Data expiration is a feature that can automatically delete a key and value in the cache after a set amount of time.</span></span>

## <a name="why-use-data-expiration"></a><span data-ttu-id="4cb0e-109">Varför använda förfallotid?</span><span class="sxs-lookup"><span data-stu-id="4cb0e-109">Why use data expiration?</span></span>

<span data-ttu-id="4cb0e-110">Förfallotid för data används ofta i situationer där data du lagrar är tillfälliga.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-110">Data expiration is commonly used in situations where the data you're storing is short-lived.</span></span>  <span data-ttu-id="4cb0e-111">Det här är viktigt eftersom Azure Redis Cache är en minnesintern databas och du inte har så mycket minne tillgängligt att använda som du skulle ha om du lagrar på disk.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-111">This is important because Azure Redis Cache is an in-memory database and you don't have as much memory available to use as you would if you were storing on disk.</span></span> <span data-ttu-id="4cb0e-112">Eftersom du har begränsat lagringsutrymme med Azure Redis Cache vill du försäkra dig om att du bara lagrar viktiga data.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-112">Since you have limited storage with Azure Redis Cache, you want to make sure you're only storing data that is important.</span></span> <span data-ttu-id="4cb0e-113">Om data inte måste finnas på längre sikt bör du ange en förfallotid.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-113">If the data doesn't need to be around for a long time, make sure you set an expiration.</span></span>

## <a name="how-to-use-data-expiration-in-azure-redis-cache"></a><span data-ttu-id="4cb0e-114">Så här använder du förfallotid i Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="4cb0e-114">How to use data expiration in Azure Redis Cache</span></span>

<span data-ttu-id="4cb0e-115">Det finns olika kommandon för att implementera och hantera förfallotid i Azure Redis Cache:</span><span class="sxs-lookup"><span data-stu-id="4cb0e-115">There are different commands to implement and manage data expiration in Azure Redis Cache:</span></span>

- <span data-ttu-id="4cb0e-116">`EXPIRE`: anger timeout för en nyckel i sekunder</span><span class="sxs-lookup"><span data-stu-id="4cb0e-116">`EXPIRE`: Sets the timeout of a key in seconds</span></span>
- <span data-ttu-id="4cb0e-117">`PEXPIRE`: anger timeout för en nyckel i millisekunder</span><span class="sxs-lookup"><span data-stu-id="4cb0e-117">`PEXPIRE`: Sets the timeout of a key in milliseconds</span></span>
- <span data-ttu-id="4cb0e-118">`EXPIREAT`: anger timeout för en nyckel med en absolut Unix-tidsstämpel i sekunder</span><span class="sxs-lookup"><span data-stu-id="4cb0e-118">`EXPIREAT`: Sets the timeout of a key using an absolute Unix timestamp in seconds</span></span>
- <span data-ttu-id="4cb0e-119">`PEXPIREAT`: anger timeout för en nyckel med en absolut Unix-tidsstämpel i millisekunder</span><span class="sxs-lookup"><span data-stu-id="4cb0e-119">`PEXPIREAT`: Sets the timeout of a key using an absolute Unix timestamp in milliseconds</span></span>
- <span data-ttu-id="4cb0e-120">`TTL`: returnerar den återstående tiden som en nyckel ska finnas i sekunder</span><span class="sxs-lookup"><span data-stu-id="4cb0e-120">`TTL`: Returns the remaining time a key has to live in seconds</span></span>
- <span data-ttu-id="4cb0e-121">`PTTL`: returnerar den återstående tiden som en nyckel ska finnas i millisekunder</span><span class="sxs-lookup"><span data-stu-id="4cb0e-121">`PTTL`: Returns the remaining time a key has to live in milliseconds</span></span>
- <span data-ttu-id="4cb0e-122">`PERSIST`: gör så att en nyckel aldrig upphör</span><span class="sxs-lookup"><span data-stu-id="4cb0e-122">`PERSIST`: Makes a key never expire</span></span>

<span data-ttu-id="4cb0e-123">Det vanligaste kommandot är `EXPIRE` och vi använder det genom hela modulen.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-123">The most common command is `EXPIRE`, and we'll use it throughout this module.</span></span>

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a><span data-ttu-id="4cb0e-124">Exempel på förfallotid med C# och ServiceStack.Redis</span><span class="sxs-lookup"><span data-stu-id="4cb0e-124">Example of data expiration using C# and ServiceStack.Redis</span></span>

<span data-ttu-id="4cb0e-125">Kom ihåg att vid användning av ett klientbibliotek som **ServiceStack.Redis** behöver vi inte bekymra oss om att komma ihåg Azure Redis Cache-kommandon på låg nivå.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-125">Remember, when using a client library like **ServiceStack.Redis**, we don't have to worry about remembering the low-level Azure Redis Cache commands.</span></span> <span data-ttu-id="4cb0e-126">Vi kan i stället använda enkla C#-metoder.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-126">Instead, we can use simple C# methods.</span></span>

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

<span data-ttu-id="4cb0e-127">Med Azure Redis Cache kan du ta bort data automatiskt efter en viss tid genom att använda förfallotid.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-127">Azure Redis Cache allows you to delete data automatically after a set amount of time using data expiration.</span></span> <span data-ttu-id="4cb0e-128">Det här är viktigt eftersom Azure Redis Cache är en minnesintern databas och du inte har så mycket minne tillgängligt som du skulle ha med lagring på disk.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-128">This is important because Azure Redis Cache is an in-memory database, and you don't have as much memory available as you would with storing data on disk.</span></span> <span data-ttu-id="4cb0e-129">Om data du lagrar inte måste finnas på längre sikt bör du ange en förfallotid.</span><span class="sxs-lookup"><span data-stu-id="4cb0e-129">If the data you're storing doesn't need to be around for a long time, make sure you set an expiration.</span></span>