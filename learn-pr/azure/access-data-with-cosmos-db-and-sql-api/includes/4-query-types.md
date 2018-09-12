<span data-ttu-id="d93c9-101">Med hjälp av de två dokument du har lagt till i databasen som målet för dina frågor ska vi nu gå igenom lite grundläggande om frågor.</span><span class="sxs-lookup"><span data-stu-id="d93c9-101">Using the two documents you added to the database as the target of our queries, let's walk through some query basics.</span></span> <span data-ttu-id="d93c9-102">Azure Cosmos DB använder SQL-frågor, precis som SQL Server, för att utföra frågeåtgärder.</span><span class="sxs-lookup"><span data-stu-id="d93c9-102">Azure Cosmos DB uses SQL queries, just like SQL Server, to perform query operations.</span></span> <span data-ttu-id="d93c9-103">Alla egenskaper indexeras automatiskt som standard, så alla data i databasen är direkt tillgängliga att fråga.</span><span class="sxs-lookup"><span data-stu-id="d93c9-103">All properties are automatically indexed by default, so all data in the database is instantly available to query.</span></span>

## <a name="sql-query-basics"></a><span data-ttu-id="d93c9-104">Grundläggande om SQL-frågor</span><span class="sxs-lookup"><span data-stu-id="d93c9-104">SQL query basics</span></span>
<span data-ttu-id="d93c9-105">Varje SQL-fråga består av en SELECT-sats och valfria FROM- och WHERE-satser.</span><span class="sxs-lookup"><span data-stu-id="d93c9-105">Every SQL query consists of a SELECT clause and optional FROM and WHERE clauses.</span></span> <span data-ttu-id="d93c9-106">Dessutom kan du lägga till andra satser som ORDER BY och JOIN för att få den information du behöver.</span><span class="sxs-lookup"><span data-stu-id="d93c9-106">In addition, you can add other clauses like ORDER BY and JOIN to get the information you need.</span></span>

<span data-ttu-id="d93c9-107">En SQL-fråga har följande format:</span><span class="sxs-lookup"><span data-stu-id="d93c9-107">A SQL query has the following format:</span></span>

    SELECT <select_list>
    [FROM <optional_from_specification>]
    [WHERE <optional_filter_condition>]
    [ORDER BY <optional_sort_specification>]
    [JOIN <optional_join_specification>]

## <a name="select-clause"></a><span data-ttu-id="d93c9-108">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="d93c9-108">SELECT clause</span></span>

<span data-ttu-id="d93c9-109">SELECT-satsen avgör vilken typ av värden som skapas när frågan körs.</span><span class="sxs-lookup"><span data-stu-id="d93c9-109">The SELECT clause determines the type of values that will be produced when the query is executed.</span></span> <span data-ttu-id="d93c9-110">Värdet `SELECT *` anger att hela JSON-dokumentet returneras.</span><span class="sxs-lookup"><span data-stu-id="d93c9-110">A value of `SELECT *` indicates that the entire JSON document is returned.</span></span>

<span data-ttu-id="d93c9-111">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="d93c9-111">**Query**</span></span>
```
SELECT *
FROM Products p
WHERE p.id ="1"
```

<span data-ttu-id="d93c9-112">**Returnerar**</span><span class="sxs-lookup"><span data-stu-id="d93c9-112">**Returns**</span></span>
```
[
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "shipping": {
            "weight": 1,
            "dimensions": {
                "width": 6,
                "height": 8,
                "depth": 1
            }
        },
        "_rid": "glAZAJFm0VsBAAAAAAAAAA==",
        "_self": "dbs/glAZAA==/colls/glAZAJFm0Vs=/docs/glAZAJFm0VsBAAAAAAAAAA==/",
        "_etag": "\"00006000-0000-0000-0000-5b71be760000\"",
        "_attachments": "attachments/",
        "_ts": 1534180982
    }
]
```

<span data-ttu-id="d93c9-113">Eller så kan du begränsa utdata till att bara innehålla vissa egenskaper genom att inkludera en lista över egenskaper i SELECT-satsen.</span><span class="sxs-lookup"><span data-stu-id="d93c9-113">Or, you can limit the output to include only certain properties by including a list of properties in the SELECT clause.</span></span> <span data-ttu-id="d93c9-114">I följande fråga returneras bara ID, tillverkare och produktbeskrivning.</span><span class="sxs-lookup"><span data-stu-id="d93c9-114">In the following query, only the ID, manufacturer, and product description are returned.</span></span>

<span data-ttu-id="d93c9-115">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="d93c9-115">**Query**</span></span>
```
SELECT 
    p.id, 
    p.manufacturer, 
    p.description
FROM Products p
WHERE p.id ="1"
```

<span data-ttu-id="d93c9-116">**Returnerar**</span><span class="sxs-lookup"><span data-stu-id="d93c9-116">**Returns**</span></span>
```
[
    {
        "id": "1",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt"
    }
]
```

## <a name="from-clause"></a><span data-ttu-id="d93c9-117">FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="d93c9-117">FROM clause</span></span>

<span data-ttu-id="d93c9-118">FROM-satsen anger den datakälla som frågan körs på.</span><span class="sxs-lookup"><span data-stu-id="d93c9-118">The FROM clause specifies the data source upon which the query operates.</span></span> <span data-ttu-id="d93c9-119">Du kan göra hela samlingen till källa för frågan eller så kan du ange en delmängd av frågan.</span><span class="sxs-lookup"><span data-stu-id="d93c9-119">You can make the whole collection the source of the query or you can specify a subset of the collection instead.</span></span> <span data-ttu-id="d93c9-120">FROM-satsen är valfri om inte källan filtreras eller planeras senare i frågan.</span><span class="sxs-lookup"><span data-stu-id="d93c9-120">The FROM clause is optional unless the source is filtered or projected later in the query.</span></span>

<span data-ttu-id="d93c9-121">En fråga som `SELECT * FROM Products` indikerar att hela Products-samlingen är källan som frågan ska räknas upp över.</span><span class="sxs-lookup"><span data-stu-id="d93c9-121">A query such as `SELECT * FROM Products` indicates that the entire Products collection is the source over which to enumerate the query.</span></span>

<span data-ttu-id="d93c9-122">En samling kan ha ett alias, som `SELECT p.id FROM Products AS p` eller bara `SELECT p.id FROM Products p`, där `p` motsvarar `Products`.</span><span class="sxs-lookup"><span data-stu-id="d93c9-122">A collection can be aliased, such as `SELECT p.id FROM Products AS p` or simply `SELECT p.id FROM Products p`, where `p` is the equivalent of `Products`.</span></span> <span data-ttu-id="d93c9-123">`AS` är ett valfritt nyckelord som ger identifieraren ett alias.</span><span class="sxs-lookup"><span data-stu-id="d93c9-123">`AS` is an optional keyword to alias the identifier.</span></span>

<span data-ttu-id="d93c9-124">När källan har ett alias kan inte den ursprungliga källan bindas.</span><span class="sxs-lookup"><span data-stu-id="d93c9-124">Once aliased, the original source cannot be bound.</span></span> <span data-ttu-id="d93c9-125">Till exempel är `SELECT Products.id FROM Products p` syntaktiskt ogiltigt eftersom identifieraren ”Products” inte kan matchas längre.</span><span class="sxs-lookup"><span data-stu-id="d93c9-125">For example, `SELECT Products.id FROM Products p` is syntactically invalid because the identifier "Products" cannot be resolved anymore.</span></span>

<span data-ttu-id="d93c9-126">Alla egenskaper som måste refereras måste bara helt kvalificerade.</span><span class="sxs-lookup"><span data-stu-id="d93c9-126">All properties that need to be referenced must be fully qualified.</span></span> <span data-ttu-id="d93c9-127">I frånvaron av strikt schemaanslutning tillämpas detta för att undvika tvetydiga bindningar.</span><span class="sxs-lookup"><span data-stu-id="d93c9-127">In the absence of strict schema adherence, this is enforced to avoid any ambiguous bindings.</span></span> <span data-ttu-id="d93c9-128">Därför är `SELECT id FROM Products p` syntaktiskt ogiltigt eftersom egenskapen `id` inte är bunden.</span><span class="sxs-lookup"><span data-stu-id="d93c9-128">Therefore, `SELECT id FROM Products p` is syntactically invalid because the property `id` is not bound.</span></span>

### <a name="subdocuments-in-a-from-clause"></a><span data-ttu-id="d93c9-129">Underdokument i en FROM-sats</span><span class="sxs-lookup"><span data-stu-id="d93c9-129">Subdocuments in a FROM clause</span></span>
<span data-ttu-id="d93c9-130">Källan kan också reduceras till en mindre delmängd.</span><span class="sxs-lookup"><span data-stu-id="d93c9-130">The source can also be reduced to a smaller subset.</span></span> <span data-ttu-id="d93c9-131">För att till exempel endast räkna upp ett underträd i varje dokument kan den underordnade roten sedan bli källan, enligt följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d93c9-131">For instance, to enumerate only a subtree in each document, the subroot could then become the source, as shown in the following example:</span></span>

<span data-ttu-id="d93c9-132">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="d93c9-132">**Query**</span></span>
```
SELECT * 
FROM Products.shipping
```

<span data-ttu-id="d93c9-133">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="d93c9-133">**Results**</span></span>  

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
    "weight": 1,
    "dimensions": {
        "width": 6,
        "height": 8,
        "depth": 1
    }
}
]
```

<span data-ttu-id="d93c9-134">I exemplet ovan används en matris som datakälla men ett objekt kan också användas som källa, vilket visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="d93c9-134">Although the above example used an array as the source, an object could also be used as the source, which is what's shown in the following example.</span></span> <span data-ttu-id="d93c9-135">Alla giltiga JSON-värden (som inte är odefinierad) som kan hittas i källan tas med i beräkningen att inkluderas i frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="d93c9-135">Any valid JSON value (that's not undefined) that can be found in the source is considered for inclusion in the result of the query.</span></span> <span data-ttu-id="d93c9-136">Om några produkter inte har ett `shipping.weight`-värde exkluderas de i frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="d93c9-136">If some products don’t have a `shipping.weight` value, they are excluded in the query result.</span></span>

<span data-ttu-id="d93c9-137">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="d93c9-137">**Query**</span></span>

    SELECT * 
    FROM Products.shipping.weight

<span data-ttu-id="d93c9-138">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="d93c9-138">**Results**</span></span>

    [
        1,
        2
    ]

## <a name="where-clause"></a><span data-ttu-id="d93c9-139">WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="d93c9-139">WHERE clause</span></span>
<span data-ttu-id="d93c9-140">WHERE-satsen anger villkoren som JSON-dokument som tillhandahålls av källan måste uppfylla för att kunna inkluderas som en del av resultatet.</span><span class="sxs-lookup"><span data-stu-id="d93c9-140">The WHERE clause specifies the conditions that the JSON documents provided by the source must satisfy in order to be included as part of the result.</span></span> <span data-ttu-id="d93c9-141">Alla JSON-dokument måste utvärdera de angivna villkoren som **sanna** för att övervägas för resultatet.</span><span class="sxs-lookup"><span data-stu-id="d93c9-141">Any JSON document must evaluate the specified conditions to **true** to be considered for the result.</span></span> <span data-ttu-id="d93c9-142">WHERE-satsen är valfri.</span><span class="sxs-lookup"><span data-stu-id="d93c9-142">The WHERE clause is optional.</span></span>

<span data-ttu-id="d93c9-143">Följande fråga begär dokument som innehåller ett ID vars värde är 1:</span><span class="sxs-lookup"><span data-stu-id="d93c9-143">The following query requests documents that contain an ID whose value is 1:</span></span>

<span data-ttu-id="d93c9-144">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="d93c9-144">**Query**</span></span>

    SELECT p.description
    FROM Products p 
    WHERE p.id = "1"

<span data-ttu-id="d93c9-145">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="d93c9-145">**Results**</span></span>

    [
        {
            "description": "Quick dry crew neck t-shirt"
        }
    ]

## <a name="order-by-clause"></a><span data-ttu-id="d93c9-146">ORDER BY-satsen</span><span class="sxs-lookup"><span data-stu-id="d93c9-146">ORDER BY clause</span></span>

<span data-ttu-id="d93c9-147">ORDER BY-satsen gör det möjligt för dig att ordna resultatet i stigande eller fallande ordning.</span><span class="sxs-lookup"><span data-stu-id="d93c9-147">The ORDER BY clause enables you to order the results in ascending or descending order.</span></span>

<span data-ttu-id="d93c9-148">Följande ORDER BY-fråga returnerar pris, beskrivning och produkt-ID för alla produkter, ordnade efter pris i stigande ordning:</span><span class="sxs-lookup"><span data-stu-id="d93c9-148">The following ORDER BY query returns the price, description, and product ID for all products, ordered by price, in ascending order:</span></span>

<span data-ttu-id="d93c9-149">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="d93c9-149">**Query**</span></span>

    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC

<span data-ttu-id="d93c9-150">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="d93c9-150">**Results**</span></span>

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

## <a name="join-clause"></a><span data-ttu-id="d93c9-151">JOIN-satsen</span><span class="sxs-lookup"><span data-stu-id="d93c9-151">JOIN clause</span></span>

<span data-ttu-id="d93c9-152">JOIN-satsen gör det möjligt för dig att utföra inre kopplingar med dokumentet och dokumentets underordnade rötter.</span><span class="sxs-lookup"><span data-stu-id="d93c9-152">The JOIN clause enables you to perform inner joins with the document and the document subroots.</span></span> <span data-ttu-id="d93c9-153">Så i produktdatabasen kan du till exempel koppla dokumenten med leveransinformation.</span><span class="sxs-lookup"><span data-stu-id="d93c9-153">So in the product database, for example, you can join the documents with the shipping data.</span></span>  

<span data-ttu-id="d93c9-154">I följande fråga returneras produkt-ID:n för varje produkt som har en leveransmetod.</span><span class="sxs-lookup"><span data-stu-id="d93c9-154">In the following query, the product IDs are returned for each product that has a shipping method.</span></span> <span data-ttu-id="d93c9-155">Om du har lagt till en tredje produkt som inte har en leveransegenskap skulle resultatet vara samma som det tredje objektet och skulle exkluderas för att det inte har en leveransegenskap.</span><span class="sxs-lookup"><span data-stu-id="d93c9-155">If you added a third product that didn't have a shipping property, the result would be the same because the third item would be excluded for not having a shipping property.</span></span>

<span data-ttu-id="d93c9-156">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="d93c9-156">**Query**</span></span>

    SELECT p.productId
    FROM Products p
    JOIN p.shipping

<span data-ttu-id="d93c9-157">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="d93c9-157">**Results**</span></span>

    [
        {
            "productId": "33218896"
        },
        {
            "productId": "33218897"
        }
    ]

## <a name="geospatial-queries"></a><span data-ttu-id="d93c9-158">Geospatiala frågor</span><span class="sxs-lookup"><span data-stu-id="d93c9-158">Geospatial queries</span></span>

<span data-ttu-id="d93c9-159">Med geospatiala frågor kan du utföra spatiala frågor med GeoJSON-punkter.</span><span class="sxs-lookup"><span data-stu-id="d93c9-159">Geospatial queries enable you to perform spatial queries using GeoJSON points.</span></span> <span data-ttu-id="d93c9-160">Med koordinaterna i databasen kan du beräkna avståndet mellan två punkter och avgöra om en punkt, polygon eller linjesträng är inom en annan punkt, polygon eller linjesträng.</span><span class="sxs-lookup"><span data-stu-id="d93c9-160">Using the coordinates in the database, you can calculate the distance between two points and determine whether a point, polygon, or linestring is within another point, polygon, or linestring.</span></span>

<span data-ttu-id="d93c9-161">För produktkatalogdata skulle detta göra det möjligt för dina användare att ange platsinformation och fastställa om det finns några butiker inom 80 km avstånd som har artikeln som användaren letar efter.</span><span class="sxs-lookup"><span data-stu-id="d93c9-161">For product catalog data, this would enable your users to enter their location information and determine whether there were any stores within a 50-mile radius that have the item they're looking for.</span></span> 

## <a name="summary"></a><span data-ttu-id="d93c9-162">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d93c9-162">Summary</span></span>

<span data-ttu-id="d93c9-163">Att snabbt kunna fråga alla dina data efter att ha lagt till dem i Azure Cosmos DB och använda välbekanta SQL-frågemetoder kan hjälpa dig och dina kunder för att utforska och få insikter om dina lagrade data.</span><span class="sxs-lookup"><span data-stu-id="d93c9-163">Being able to quickly query all your data after adding it to Azure Cosmos DB and using familiar SQL query techniques can help you and your customers to explore and gain insights into your stored data.</span></span>