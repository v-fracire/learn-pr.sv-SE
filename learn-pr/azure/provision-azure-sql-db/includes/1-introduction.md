<span data-ttu-id="efb57-101">Datahanteringen är en extremt viktig del av affärsverksamheten idag.</span><span class="sxs-lookup"><span data-stu-id="efb57-101">Managing data is a critical component of any business.</span></span> <span data-ttu-id="efb57-102">Relationsdatabaser, och mer specifikt Microsoft SQL Server, har i flera årtionden varit de kanske vanligaste verktygen för datahantering.</span><span class="sxs-lookup"><span data-stu-id="efb57-102">Relational databases, and specifically Microsoft SQL Server, have been among the most common tools for handling that data for decades.</span></span> 

<span data-ttu-id="efb57-103">Om man vill lägga datahanteringen i molnet _räcker det_ med att använda virtuella datorer i Azure som värdar för de egna Microsoft SQL Server-instanserna.</span><span class="sxs-lookup"><span data-stu-id="efb57-103">If we want to manage our data using the cloud, we _can_ just use Azure virtual machines to host our own Microsoft SQL Server instances.</span></span> <span data-ttu-id="efb57-104">För många är detta den enda rätta lösningen, men Azure erbjuder även en annan metod som ofta är mycket enklare och mer effektiv.</span><span class="sxs-lookup"><span data-stu-id="efb57-104">Sometimes that's the right solution, but Azure offers another way that is often much easier and more cost effective.</span></span> <span data-ttu-id="efb57-105">Azure SQL Database är ett PaaS-erbjudande (Platform-as-a-Service), vilket betyder att du slipper sköta infrastrukturen och underhållet själv.</span><span class="sxs-lookup"><span data-stu-id="efb57-105">Azure SQL Databases are a Platform-as-a-Service (PaaS) offering, meaning much less infrastructure and maintenance to manage yourself.</span></span>

<span data-ttu-id="efb57-106">Vi kan försöka göra det hela tydligare med ett exempel. Tänk dig att du är chefsutvecklare på logistikföretaget Contoso Transport.</span><span class="sxs-lookup"><span data-stu-id="efb57-106">To understand better, let's consider a scenario: You're a software development lead at a transportation logistics company, Contoso Transport.</span></span>

<span data-ttu-id="efb57-107">I transportbranschen är det en hel del som måste samordnas och synkas: administration, orderavdelning, chaufförer och kunder.</span><span class="sxs-lookup"><span data-stu-id="efb57-107">The transportation industry requires tight coordination among everyone involved: schedulers, dispatchers, drivers, and even customers.</span></span>

<span data-ttu-id="efb57-108">Den nuvarande processen innefattar mängder av pappersformulär och många timmar i telefon för att försöka samordna alla leveranser.</span><span class="sxs-lookup"><span data-stu-id="efb57-108">Your current process involves piles of paper forms and hours on the phone to coordinate shipments.</span></span> <span data-ttu-id="efb57-109">Ofta upptäcker du att dokumenten saknar signaturer och att det är svårt att få tag på någon på orderavdelningen.</span><span class="sxs-lookup"><span data-stu-id="efb57-109">You find that paperwork is often missing signatures and dispatchers are frequently unavailable.</span></span> <span data-ttu-id="efb57-110">Det är den här typen av flaskhalsar som leder till att chaufförerna sitter och rullar tummarna medan viktiga leveranser blir kraftigt försenade.</span><span class="sxs-lookup"><span data-stu-id="efb57-110">These holdups leave drivers sitting idle, which cause important shipments to arrive late.</span></span> <span data-ttu-id="efb57-111">Kundnöjdhet och återkommande kunder är avgörande för lönsamheten i ett företag.</span><span class="sxs-lookup"><span data-stu-id="efb57-111">Customer satisfaction and repeat business are crucial to your bottom line.</span></span>

<span data-ttu-id="efb57-112">Ledningen beslutar sig därför för att övergå från pappersformulär och telefonsamtal till digitala dokument och onlinekommunikation.</span><span class="sxs-lookup"><span data-stu-id="efb57-112">Your team decides to move from paper forms and phone calls to digital documents and online communication.</span></span> <span data-ttu-id="efb57-113">Tack vare digitaliseringen kan alla samordna och spåra leveranser via sina webbläsare eller mobilappar.</span><span class="sxs-lookup"><span data-stu-id="efb57-113">Going digital will enable everyone to coordinate and track shipment times through their web browser or mobile app.</span></span>

<span data-ttu-id="efb57-114">Nu vill du skapa en snabb prototyp och visa upp den för ditt team.</span><span class="sxs-lookup"><span data-stu-id="efb57-114">You want to quickly prototype something to share with your team.</span></span> <span data-ttu-id="efb57-115">Prototypen innehåller en databas som lagrar information om chaufförer, kunder och beställningar.</span><span class="sxs-lookup"><span data-stu-id="efb57-115">Your prototype will include a database to hold driver, customer, and order information.</span></span> <span data-ttu-id="efb57-116">Prototypen utgör grunden för din produktionsapp.</span><span class="sxs-lookup"><span data-stu-id="efb57-116">Your prototype will be the basis for your production app.</span></span> <span data-ttu-id="efb57-117">Den teknik du väljer nu bör därför ha relevans för er verksamhet.</span><span class="sxs-lookup"><span data-stu-id="efb57-117">So the technology choices you make now should carry to what your team delivers.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="efb57-118">Utbildningsmål</span><span class="sxs-lookup"><span data-stu-id="efb57-118">Learning objectives</span></span>

<span data-ttu-id="efb57-119">Detta får du får lära dig:</span><span class="sxs-lookup"><span data-stu-id="efb57-119">Here, you'll learn:</span></span>

- <span data-ttu-id="efb57-120">Varför Azure SQL Database är ett fullgott alternativ till en relationsdatabas</span><span class="sxs-lookup"><span data-stu-id="efb57-120">Why Azure SQL Database is a good choice for running your relational database</span></span>
- <span data-ttu-id="efb57-121">Vilka konfigurationer och priser som finns för din Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="efb57-121">What configuration and pricing options are available for your Azure SQL Database</span></span>
- <span data-ttu-id="efb57-122">Hur du skapar en Azure SQL Database med portalen</span><span class="sxs-lookup"><span data-stu-id="efb57-122">How to create a SQL Azure Database from the portal</span></span>
- <span data-ttu-id="efb57-123">Hur du använder Cloud Shell för att ansluta till din Azure SQL Database, lägga till en tabell och arbeta med data</span><span class="sxs-lookup"><span data-stu-id="efb57-123">How to use Cloud Shell to connect to your SQL Azure Database, add a table, and work with data</span></span>