<span data-ttu-id="dac66-101">Du arbetar på ett företag som samlar in sportstatistik och som tillhandahåller ett API för appar som returnerar frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="dac66-101">You work at a company that tracks professional sports statistics and provides apps an API to query results.</span></span> <span data-ttu-id="dac66-102">Det hjälper fans att följa och analysera matcher och resultat, både live och bakåt i tiden.</span><span class="sxs-lookup"><span data-stu-id="dac66-102">It helps fans track and review games and scores, both live and historical.</span></span> <span data-ttu-id="dac66-103">Användarna kan också begära lagstatistik genom att söka på naturligt språk, till exempel ”Hur många gånger har John Smith gjort en home run mot en vänsterhänt pitcher?”.</span><span class="sxs-lookup"><span data-stu-id="dac66-103">Users can also request team statistics using a natural language search, such as “How many times has John Smith hit a home run against a left-handed pitcher?”</span></span>

<span data-ttu-id="dac66-104">När efterfrågan är extra hög, till exempel under slutspel, ökar svarstiden för din tjänst eftersom din serverdelstjänst inte har tillräcklig kapacitet för att möta efterfrågan.</span><span class="sxs-lookup"><span data-stu-id="dac66-104">During times of peak demand, such as during playoffs, your response time of your service slows down because your back-end service doesn't have the capacity to meet demand.</span></span> <span data-ttu-id="dac66-105">Du vill förbättra prestanda för dina användare och minska arbetsbelastningen på dina serverdels- och datalagringstjänster.</span><span class="sxs-lookup"><span data-stu-id="dac66-105">You want to improve performance for your users and reduce the workload on your back-end and data storage services.</span></span> <span data-ttu-id="dac66-106">Dina mätvärden visar att mellan 50 % och 80 % av de data som returneras är för skrivskyddade eller nyligen begärda värden.</span><span class="sxs-lookup"><span data-stu-id="dac66-106">Your metrics show that 50% to 80% of the data returned is for read-only or recently requested values.</span></span> <span data-ttu-id="dac66-107">Genom att implementera ett cacheminne för data som används ofta kan du förbättra prestanda och minska svarstiden.</span><span class="sxs-lookup"><span data-stu-id="dac66-107">Implementing a cache of commonly used data could improve performance and reduce latency.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="dac66-108">Utbildningsmål</span><span class="sxs-lookup"><span data-stu-id="dac66-108">Learning objectives</span></span>

<span data-ttu-id="dac66-109">I den här modulen kommer du att:</span><span class="sxs-lookup"><span data-stu-id="dac66-109">In this module, you will:</span></span>

- <span data-ttu-id="dac66-110">Lära dig vad en Redis-cache är och vad du kan använda den för.</span><span class="sxs-lookup"><span data-stu-id="dac66-110">Describe what a Redis cache is, and what you can use it for.</span></span>
- <span data-ttu-id="dac66-111">Skapa en design och planera att använda en Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="dac66-111">Create a design and plan to use a Redis cache.</span></span>
- <span data-ttu-id="dac66-112">Etablera en Redis-cache i Azure.</span><span class="sxs-lookup"><span data-stu-id="dac66-112">Provision a Redis cache in Azure.</span></span>
- <span data-ttu-id="dac66-113">Ansluta en webbapp till cachen.</span><span class="sxs-lookup"><span data-stu-id="dac66-113">Connect a web app to the cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dac66-114">Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="dac66-114">Prerequisites</span></span>

- <span data-ttu-id="dac66-115">Erfarenhet av apputveckling</span><span class="sxs-lookup"><span data-stu-id="dac66-115">Experience with app development</span></span>
- <span data-ttu-id="dac66-116">Erfarenhet av att använda data i appar</span><span class="sxs-lookup"><span data-stu-id="dac66-116">Experience using data in apps</span></span>