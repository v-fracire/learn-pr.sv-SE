<span data-ttu-id="d167a-101">Om du vill skriva kod för att få åtkomst till en kö behöver du tre uppgifter:</span><span class="sxs-lookup"><span data-stu-id="d167a-101">To write code to access a queue, you need three pieces of information:</span></span>
 - <span data-ttu-id="d167a-102">namn på lagringskonto</span><span class="sxs-lookup"><span data-stu-id="d167a-102">storage account name</span></span>
 - <span data-ttu-id="d167a-103">könamn</span><span class="sxs-lookup"><span data-stu-id="d167a-103">queue name</span></span>
 - <span data-ttu-id="d167a-104">auktoriseringstoken.</span><span class="sxs-lookup"><span data-stu-id="d167a-104">authorization token.</span></span>

<span data-ttu-id="d167a-105">I vårt exempel med delning av foton används den här informationen av båda programmen som pratar med kön (webbklientdelen som lägger till meddelanden och medelnivån som bearbetar dem).</span><span class="sxs-lookup"><span data-stu-id="d167a-105">In our photo-sharing example, this information is used by both applications that talk to the queue (the web front-end that adds messages and the mid-tier that processes them).</span></span>

## <a name="queue-identity"></a><span data-ttu-id="d167a-106">Köidentitet</span><span class="sxs-lookup"><span data-stu-id="d167a-106">Queue identity</span></span>

<span data-ttu-id="d167a-107">Varje kö har ett namn som du tilldelar under skapandet.</span><span class="sxs-lookup"><span data-stu-id="d167a-107">Every queue has a name that you assign during creation.</span></span> <span data-ttu-id="d167a-108">Namnet måste vara unikt inom ditt lagringskonto, men det behöver inte vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="d167a-108">The name must be unique within your storage account but does not need to be globally unique.</span></span>

<span data-ttu-id="d167a-109">Kombinationen av namnet på ditt lagringskonto och könamnet identifierar en kö unikt.</span><span class="sxs-lookup"><span data-stu-id="d167a-109">The combination of your storage account name and your queue name uniquely identify a queue.</span></span>

## <a name="access-authorization"></a><span data-ttu-id="d167a-110">Åtkomstauktorisering</span><span class="sxs-lookup"><span data-stu-id="d167a-110">Access authorization</span></span>

<span data-ttu-id="d167a-111">Varje begäran till en kö måste auktoriseras.</span><span class="sxs-lookup"><span data-stu-id="d167a-111">Every request to a queue must be authorized.</span></span> <span data-ttu-id="d167a-112">Det finns flera alternativ för att auktorisera klientåtkomst till din köer: Azure Active Directory, delad nyckel, signaturer för delad åtkomst och så vidare. Vi använder auktorisering med delad nyckel eftersom det är ett enkelt sätt att börja arbeta med köer.</span><span class="sxs-lookup"><span data-stu-id="d167a-112">There are multiple options for authorizing client access to your queues: Azure Active Directory, Shared Key, Shared access signatures, etc. We will use Shared Key authorization because it is a simple way to get started working with queues.</span></span>

<span data-ttu-id="d167a-113">Din delade nyckel är tillgänglig i avsnittet **Inställningar** på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d167a-113">Your Shared Key is available in the **Settings** section of your storage account.</span></span>

## <a name="access-using-rest"></a><span data-ttu-id="d167a-114">Åtkomst med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="d167a-114">Access using REST</span></span>

<span data-ttu-id="d167a-115">Du kan komma åt en kö med hjälp av ett REST API.</span><span class="sxs-lookup"><span data-stu-id="d167a-115">You can access a queue using a REST API.</span></span> <span data-ttu-id="d167a-116">Om du vill göra detta använder du en adress baserat på köidentiteten.</span><span class="sxs-lookup"><span data-stu-id="d167a-116">To do this, you will use an address based on the queue identity.</span></span> <span data-ttu-id="d167a-117">Formatet för http visas nedan:</span><span class="sxs-lookup"><span data-stu-id="d167a-117">The format for http is shown below:</span></span>

```
http://<storage account>.queue.core.windows.net/<queue name> 
```

<span data-ttu-id="d167a-118">Du placerar den delade nyckeln i auktoriseringsrubriken för varje begäran.</span><span class="sxs-lookup"><span data-stu-id="d167a-118">You put the Shared Key in the authorization header of every request.</span></span>

## <a name="access-using-the-azure-storage-client-library-for-net"></a><span data-ttu-id="d167a-119">Åtkomst med hjälp av Azure Storage-klientbiblioteket för .NET</span><span class="sxs-lookup"><span data-stu-id="d167a-119">Access using the Azure Storage Client Library for .NET</span></span>

<span data-ttu-id="d167a-120">Azure Storage-klientbiblioteket för .NET är ett bibliotek som tillhandahålls av Microsoft som formulerar REST-begäranden och parsar REST-svar åt dig.</span><span class="sxs-lookup"><span data-stu-id="d167a-120">The Azure Storage Client Library for .NET is a library provided by Microsoft that formulates REST requests and parses REST responses for you.</span></span> <span data-ttu-id="d167a-121">Detta minskar avsevärt den mängd kod som du behöver skriva.</span><span class="sxs-lookup"><span data-stu-id="d167a-121">This greatly reduces the amount of code you need to write.</span></span> <span data-ttu-id="d167a-122">Åtkomst med hjälp av klientbiblioteket kräver fortfarande samma uppgifter (lagringskontonamn, könamn och delad nyckel), men de är ordnade på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="d167a-122">Access using the client library still requires the same pieces of information (storage account name, queue name, and Shared Key); however, they are organized differently.</span></span>

<span data-ttu-id="d167a-123">Klientbiblioteket använder en **anslutningssträng** för att upprätta din anslutning.</span><span class="sxs-lookup"><span data-stu-id="d167a-123">The client library uses a **connection string** to establish your connection.</span></span> <span data-ttu-id="d167a-124">En anslutningssträng är en enskild sträng som kombinerar ett lagringskontonamn och en delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="d167a-124">A connection string is a single string that combines a storage account name and Shared Key.</span></span> <span data-ttu-id="d167a-125">Formatet ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="d167a-125">The format looks something like this:</span></span>

```
DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net
```

<span data-ttu-id="d167a-126">Observera att anslutningssträngen inte innehåller könamnet.</span><span class="sxs-lookup"><span data-stu-id="d167a-126">Notice how the connection string does not include the queue name.</span></span> <span data-ttu-id="d167a-127">Könamnet används senare i koden när du upprättar en anslutning till kön.</span><span class="sxs-lookup"><span data-stu-id="d167a-127">The queue name is used later in your code when you establish a connection to the queue.</span></span>

<span data-ttu-id="d167a-128">Din anslutningssträng är tillgänglig i avsnittet **Inställningar** på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d167a-128">Your connection string is available in the **Settings** section of your Storage Account.</span></span>

## <a name="summary"></a><span data-ttu-id="d167a-129">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d167a-129">Summary</span></span>

<span data-ttu-id="d167a-130">Vi använder Azure Storage-klientbiblioteket för att få åtkomst till vår kö.</span><span class="sxs-lookup"><span data-stu-id="d167a-130">We will use the Azure Storage Client Library to access our queue.</span></span> <span data-ttu-id="d167a-131">Det innebär att vi behöver hämta en anslutningssträng för vårt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d167a-131">This means we need to get a connection string for our storage account.</span></span> <span data-ttu-id="d167a-132">Sedan använder vi anslutningssträngen i både avsändarprogrammet och mottagarprogrammet eftersom båda apparna behöver programmatisk åtkomst till den delade kön.</span><span class="sxs-lookup"><span data-stu-id="d167a-132">We'll then use the connection string in both our sender and receiver applications since both apps need programmatic access to the shared queue.</span></span>