<span data-ttu-id="49bca-101">Med hjälp av de två dokument du har lagt till i databasen som målet för dina frågor ska vi nu gå igenom lite grundläggande om frågor.</span><span class="sxs-lookup"><span data-stu-id="49bca-101">Using the two documents you added to the database as the target of our queries, let's walk through some query basics.</span></span> <span data-ttu-id="49bca-102">Azure Cosmos DB använder SQL-frågor, precis som SQL Server, för att utföra frågeåtgärder.</span><span class="sxs-lookup"><span data-stu-id="49bca-102">Azure Cosmos DB uses SQL queries, just like SQL Server, to perform query operations.</span></span> <span data-ttu-id="49bca-103">Alla egenskaper indexeras automatiskt som standard, så alla data i databasen är direkt tillgängliga att fråga.</span><span class="sxs-lookup"><span data-stu-id="49bca-103">All properties are automatically indexed by default, so all data in the database is instantly available to query.</span></span>

## <a name="sql-query-basics"></a><span data-ttu-id="49bca-104">Grundläggande om SQL-frågor</span><span class="sxs-lookup"><span data-stu-id="49bca-104">SQL query basics</span></span>
<span data-ttu-id="49bca-105">Varje SQL-fråga består av en SELECT-sats och valfria FROM- och WHERE-satser.</span><span class="sxs-lookup"><span data-stu-id="49bca-105">Every SQL query consists of a SELECT clause and optional FROM and WHERE clauses.</span></span> <span data-ttu-id="49bca-106">Dessutom kan du lägga till andra satser som ORDER BY och JOIN för att få den information du behöver.</span><span class="sxs-lookup"><span data-stu-id="49bca-106">In addition, you can add other clauses like ORDER BY and JOIN to get the information you need.</span></span>

<span data-ttu-id="49bca-107">En SQL-fråga har följande format:</span><span class="sxs-lookup"><span data-stu-id="49bca-107">A SQL query has the following format:</span></span>

    SELECT <select_list>
    [FROM <optional_from_specification>]
    [WHERE <optional_filter_condition>]
    [ORDER BY <optional_sort_specification>]
    [JOIN <optional_join_specification>]

## <a name="select-clause"></a><span data-ttu-id="49bca-108">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="49bca-108">SELECT clause</span></span>

<span data-ttu-id="49bca-109">SELECT-satsen avgör vilken typ av värden som skapas när frågan körs.</span><span class="sxs-lookup"><span data-stu-id="49bca-109">The SELECT clause determines the type of values that will be produced when the query is executed.</span></span> <span data-ttu-id="49bca-110">Värdet `SELECT *` anger att hela JSON-dokumentet returneras.</span><span class="sxs-lookup"><span data-stu-id="49bca-110">A value of `SELECT *` indicates that the entire JSON document is returned.</span></span>

<span data-ttu-id="49bca-111">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="49bca-111">**Query**</span></span>
```
SELECT *
FROM Products p
WHERE p.id ="1"
```

<span data-ttu-id="49bca-112">**Returnerar**</span><span class="sxs-lookup"><span data-stu-id="49bca-112">**Returns**</span></span>
```
[
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "price": "14.99",
        "shipping": {
            "weight": 1,
            "dimensions": {
                "width": 6,
                "height": 8,
                "depth": 1
            }
        },
        "_rid": "iAEeANrzNAAJAAAAAAAAAA==",
        "_self": "dbs/iAEeAA==/colls/iAEeANrzNAA=/docs/iAEeANrzNAAJAAAAAAAAAA==/",
        "_etag": "\"00003a02-0000-0000-0000-5b9208440000\"",
        "_attachments": "attachments/",
        "_ts": 1536297028
    }
]
```

<span data-ttu-id="49bca-113">Eller så kan du begränsa utdata till att bara innehålla vissa egenskaper genom att inkludera en lista över egenskaper i SELECT-satsen.</span><span class="sxs-lookup"><span data-stu-id="49bca-113">Or, you can limit the output to include only certain properties by including a list of properties in the SELECT clause.</span></span> <span data-ttu-id="49bca-114">I följande fråga returneras bara ID, tillverkare och produktbeskrivning.</span><span class="sxs-lookup"><span data-stu-id="49bca-114">In the following query, only the ID, manufacturer, and product description are returned.</span></span>

<span data-ttu-id="49bca-115">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="49bca-115">**Query**</span></span>
```
SELECT 
    p.id, 
    p.manufacturer, 
    p.description
FROM Products p
WHERE p.id ="1"
```

<span data-ttu-id="49bca-116">**Returnerar**</span><span class="sxs-lookup"><span data-stu-id="49bca-116">**Returns**</span></span>
```
[
    {
        "id": "1",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt"
    }
]
```

## <a name="from-clause"></a><span data-ttu-id="49bca-117">FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="49bca-117">FROM clause</span></span>

<span data-ttu-id="49bca-118">FROM-satsen anger den datakälla som frågan körs på.</span><span class="sxs-lookup"><span data-stu-id="49bca-118">The FROM clause specifies the data source upon which the query operates.</span></span> <span data-ttu-id="49bca-119">Du kan göra hela samlingen till källa för frågan eller så kan du ange en delmängd av frågan.</span><span class="sxs-lookup"><span data-stu-id="49bca-119">You can make the whole collection the source of the query or you can specify a subset of the collection instead.</span></span> <span data-ttu-id="49bca-120">FROM-satsen är valfri om inte källan filtreras eller planeras senare i frågan.</span><span class="sxs-lookup"><span data-stu-id="49bca-120">The FROM clause is optional unless the source is filtered or projected later in the query.</span></span>

<span data-ttu-id="49bca-121">En fråga som `SELECT * FROM Products` indikerar att hela Products-samlingen är källan som frågan ska räknas upp över.</span><span class="sxs-lookup"><span data-stu-id="49bca-121">A query such as `SELECT * FROM Products` indicates that the entire Products collection is the source over which to enumerate the query.</span></span>

<span data-ttu-id="49bca-122">En samling kan ha ett alias, som `SELECT p.id FROM Products AS p` eller bara `SELECT p.id FROM Products p`, där `p` motsvarar `Products`.</span><span class="sxs-lookup"><span data-stu-id="49bca-122">A collection can be aliased, such as `SELECT p.id FROM Products AS p` or simply `SELECT p.id FROM Products p`, where `p` is the equivalent of `Products`.</span></span> <span data-ttu-id="49bca-123">`AS` är ett valfritt nyckelord som ger identifieraren ett alias.</span><span class="sxs-lookup"><span data-stu-id="49bca-123">`AS` is an optional keyword to alias the identifier.</span></span>

<span data-ttu-id="49bca-124">När källan har ett alias kan inte den ursprungliga källan bindas.</span><span class="sxs-lookup"><span data-stu-id="49bca-124">Once aliased, the original source cannot be bound.</span></span> <span data-ttu-id="49bca-125">Till exempel är `SELECT Products.id FROM Products p` syntaktiskt ogiltigt eftersom identifieraren ”Products” inte kan matchas längre.</span><span class="sxs-lookup"><span data-stu-id="49bca-125">For example, `SELECT Products.id FROM Products p` is syntactically invalid because the identifier "Products" cannot be resolved anymore.</span></span>

<span data-ttu-id="49bca-126">Alla egenskaper som måste refereras måste bara helt kvalificerade.</span><span class="sxs-lookup"><span data-stu-id="49bca-126">All properties that need to be referenced must be fully qualified.</span></span> <span data-ttu-id="49bca-127">I frånvaron av strikt schemaanslutning tillämpas detta för att undvika tvetydiga bindningar.</span><span class="sxs-lookup"><span data-stu-id="49bca-127">In the absence of strict schema adherence, this is enforced to avoid any ambiguous bindings.</span></span> <span data-ttu-id="49bca-128">Därför är `SELECT id FROM Products p` syntaktiskt ogiltigt eftersom egenskapen `id` inte är bunden.</span><span class="sxs-lookup"><span data-stu-id="49bca-128">Therefore, `SELECT id FROM Products p` is syntactically invalid because the property `id` is not bound.</span></span>

### <a name="subdocuments-in-a-from-clause"></a><span data-ttu-id="49bca-129">Underdokument i en FROM-sats</span><span class="sxs-lookup"><span data-stu-id="49bca-129">Subdocuments in a FROM clause</span></span>
<span data-ttu-id="49bca-130">Källan kan också reduceras till en mindre delmängd.</span><span class="sxs-lookup"><span data-stu-id="49bca-130">The source can also be reduced to a smaller subset.</span></span> <span data-ttu-id="49bca-131">För att till exempel endast räkna upp ett underträd i varje dokument kan den underordnade roten sedan bli källan, enligt följande exempel:</span><span class="sxs-lookup"><span data-stu-id="49bca-131">For instance, to enumerate only a subtree in each document, the subroot could then become the source, as shown in the following example:</span></span>

<span data-ttu-id="49bca-132">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="49bca-132">**Query**</span></span>
```
SELECT * 
FROM Products.shipping
```

<span data-ttu-id="49bca-133">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="49bca-133">**Results**</span></span>  

```
[
    {
        "weight": 1,
        "dimensions": {
            "width": 6,
            "height": 8,
            "depth": 1
        }
    },
    {
        "weight": 2,
        "dimensions": {
            "width": 8,
            "height": 11,
            "depth": 3
        }
    }
]
```

<span data-ttu-id="49bca-134">I exemplet ovan används en matris som datakälla men ett objekt kan också användas som källa, vilket visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="49bca-134">Although the above example used an array as the source, an object could also be used as the source, which is what's shown in the following example.</span></span> <span data-ttu-id="49bca-135">Alla giltiga JSON-värden (som inte är odefinierad) som kan hittas i källan tas med i beräkningen att inkluderas i frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="49bca-135">Any valid JSON value (that's not undefined) that can be found in the source is considered for inclusion in the result of the query.</span></span> <span data-ttu-id="49bca-136">Om några produkter inte har ett `shipping.weight`-värde exkluderas de i frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="49bca-136">If some products don’t have a `shipping.weight` value, they are excluded in the query result.</span></span>

<span data-ttu-id="49bca-137">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="49bca-137">**Query**</span></span>

    SELECT * 
    FROM Products.shipping.weight

<span data-ttu-id="49bca-138">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="49bca-138">**Results**</span></span>

    [
        1,
        2
    ]

## <a name="where-clause"></a><span data-ttu-id="49bca-139">WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="49bca-139">WHERE clause</span></span>
<span data-ttu-id="49bca-140">WHERE-satsen anger villkoren som JSON-dokument som tillhandahålls av källan måste uppfylla för att kunna inkluderas som en del av resultatet.</span><span class="sxs-lookup"><span data-stu-id="49bca-140">The WHERE clause specifies the conditions that the JSON documents provided by the source must satisfy in order to be included as part of the result.</span></span> <span data-ttu-id="49bca-141">Alla JSON-dokument måste utvärdera de angivna villkoren som **sanna** för att övervägas för resultatet.</span><span class="sxs-lookup"><span data-stu-id="49bca-141">Any JSON document must evaluate the specified conditions to **true** to be considered for the result.</span></span> <span data-ttu-id="49bca-142">WHERE-satsen är valfri.</span><span class="sxs-lookup"><span data-stu-id="49bca-142">The WHERE clause is optional.</span></span>

<span data-ttu-id="49bca-143">Följande fråga begär dokument som innehåller ett ID vars värde är 1:</span><span class="sxs-lookup"><span data-stu-id="49bca-143">The following query requests documents that contain an ID whose value is 1:</span></span>

<span data-ttu-id="49bca-144">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="49bca-144">**Query**</span></span>

    SELECT p.description
    FROM Products p 
    WHERE p.id = "1"

<span data-ttu-id="49bca-145">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="49bca-145">**Results**</span></span>

    [
        {
            "description": "Quick dry crew neck t-shirt"
        }
    ]

## <a name="order-by-clause"></a><span data-ttu-id="49bca-146">ORDER BY-satsen</span><span class="sxs-lookup"><span data-stu-id="49bca-146">ORDER BY clause</span></span>

<span data-ttu-id="49bca-147">ORDER BY-satsen gör det möjligt för dig att ordna resultatet i stigande eller fallande ordning.</span><span class="sxs-lookup"><span data-stu-id="49bca-147">The ORDER BY clause enables you to order the results in ascending or descending order.</span></span>

<span data-ttu-id="49bca-148">Följande ORDER BY-fråga returnerar pris, beskrivning och produkt-ID för alla produkter, ordnade efter pris i stigande ordning:</span><span class="sxs-lookup"><span data-stu-id="49bca-148">The following ORDER BY query returns the price, description, and product ID for all products, ordered by price, in ascending order:</span></span>

<span data-ttu-id="49bca-149">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="49bca-149">**Query**</span></span>

    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC

<span data-ttu-id="49bca-150">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="49bca-150">**Results**</span></span>

```
[
    {
        "price": "14.99",
        "description": "Quick dry crew neck t-shirt",
        "productId": "33218896"
    },
    {
        "price": "49.99",
        "description": "Black wool pea-coat",
        "productId": "33218897"
    }
]
```

## <a name="join-clause"></a><span data-ttu-id="49bca-151">JOIN-satsen</span><span class="sxs-lookup"><span data-stu-id="49bca-151">JOIN clause</span></span>

<span data-ttu-id="49bca-152">JOIN-satsen gör det möjligt för dig att utföra inre kopplingar med dokumentet och dokumentets underordnade rötter.</span><span class="sxs-lookup"><span data-stu-id="49bca-152">The JOIN clause enables you to perform inner joins with the document and the document subroots.</span></span> <span data-ttu-id="49bca-153">Så i produktdatabasen kan du till exempel koppla dokumenten med leveransinformation.</span><span class="sxs-lookup"><span data-stu-id="49bca-153">So in the product database, for example, you can join the documents with the shipping data.</span></span>  

<span data-ttu-id="49bca-154">I följande fråga returneras produkt-ID:n för varje produkt som har en leveransmetod.</span><span class="sxs-lookup"><span data-stu-id="49bca-154">In the following query, the product IDs are returned for each product that has a shipping method.</span></span> <span data-ttu-id="49bca-155">Om du har lagt till en tredje produkt som inte har en leveransegenskap skulle resultatet vara samma som det tredje objektet och skulle exkluderas för att det inte har en leveransegenskap.</span><span class="sxs-lookup"><span data-stu-id="49bca-155">If you added a third product that didn't have a shipping property, the result would be the same because the third item would be excluded for not having a shipping property.</span></span>

<span data-ttu-id="49bca-156">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="49bca-156">**Query**</span></span>

    SELECT p.productId
    FROM Products p
    JOIN p.shipping

<span data-ttu-id="49bca-157">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="49bca-157">**Results**</span></span>

    [
        {
            "productId": "33218896"
        },
        {
            "productId": "33218897"
        }
    ]

## <a name="geospatial-queries"></a><span data-ttu-id="49bca-158">Geospatiala frågor</span><span class="sxs-lookup"><span data-stu-id="49bca-158">Geospatial queries</span></span>

<span data-ttu-id="49bca-159">Med hjälp av geospatiala frågor kan du köra spatiala frågor med GeoJSON-punkter.</span><span class="sxs-lookup"><span data-stu-id="49bca-159">Geospatial queries enable you to perform spatial queries using GeoJSON Points.</span></span> <span data-ttu-id="49bca-160">Med hjälp av koordinaterna i databasen kan du beräkna avståndet mellan två punkter och avgöra om en punkt, polygon eller linjesträng finns inom en annan punkt, polygon eller linjesträng.</span><span class="sxs-lookup"><span data-stu-id="49bca-160">Using the coordinates in the database, you can calculate the distance between two points and determine whether a Point, Polygon, or LineString is within another Point, Polygon, or LineString.</span></span>

<span data-ttu-id="49bca-161">För produktkatalogdata skulle detta göra det möjligt för dina användare att ange platsinformation och fastställa om det finns några butiker inom 80 km avstånd som har den artikel som användaren letar efter.</span><span class="sxs-lookup"><span data-stu-id="49bca-161">For product catalog data, this would enable your users to enter their location information and determine whether there were any stores within a 50-mile radius that have the item they're looking for.</span></span> 

## <a name="summary"></a><span data-ttu-id="49bca-162">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="49bca-162">Summary</span></span>

<span data-ttu-id="49bca-163">Att snabbt kunna fråga alla dina data efter att ha lagt till dem i Azure Cosmos DB och använda välbekanta SQL-frågemetoder kan hjälpa dig och dina kunder för att utforska och få insikter om dina lagrade data.</span><span class="sxs-lookup"><span data-stu-id="49bca-163">Being able to quickly query all your data after adding it to Azure Cosmos DB and using familiar SQL query techniques can help you and your customers to explore and gain insights into your stored data.</span></span>