Med hjälp av de två dokument du har lagt till i databasen som målet för dina frågor ska vi nu gå igenom lite grundläggande om frågor. Azure Cosmos DB använder SQL-frågor, precis som SQL Server, för att utföra frågeåtgärder. Alla egenskaper indexeras automatiskt som standard, så alla data i databasen är direkt tillgängliga att fråga.

## <a name="sql-query-basics"></a>Grundläggande om SQL-frågor
Varje SQL-fråga består av en SELECT-sats och valfria FROM- och WHERE-satser. Dessutom kan du lägga till andra satser som ORDER BY och JOIN för att få den information du behöver.

En SQL-fråga har följande format:

    SELECT <select_list>
    [FROM <optional_from_specification>]
    [WHERE <optional_filter_condition>]
    [ORDER BY <optional_sort_specification>]
    [JOIN <optional_join_specification>]

## <a name="select-clause"></a>SELECT-satsen

SELECT-satsen avgör vilken typ av värden som skapas när frågan körs. Värdet `SELECT *` anger att hela JSON-dokumentet returneras.

**Fråga**
```
SELECT *
FROM Products p
WHERE p.id ="1"
```

**Returnerar**
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

Eller så kan du begränsa utdata till att bara innehålla vissa egenskaper genom att inkludera en lista över egenskaper i SELECT-satsen. I följande fråga returneras bara ID, tillverkare och produktbeskrivning.

**Fråga**
```
SELECT 
    p.id, 
    p.manufacturer, 
    p.description
FROM Products p
WHERE p.id ="1"
```

**Returnerar**
```
[
    {
        "id": "1",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt"
    }
]
```

## <a name="from-clause"></a>FROM-satsen

FROM-satsen anger den datakälla som frågan körs på. Du kan göra hela samlingen till källa för frågan eller så kan du ange en delmängd av frågan. FROM-satsen är valfri om inte källan filtreras eller planeras senare i frågan.

En fråga som `SELECT * FROM Products` indikerar att hela Products-samlingen är källan som frågan ska räknas upp över.

En samling kan ha ett alias, som `SELECT p.id FROM Products AS p` eller bara `SELECT p.id FROM Products p`, där `p` motsvarar `Products`. `AS` är ett valfritt nyckelord som ger identifieraren ett alias.

När källan har ett alias kan inte den ursprungliga källan bindas. Till exempel är `SELECT Products.id FROM Products p` syntaktiskt ogiltigt eftersom identifieraren ”Products” inte kan matchas längre.

Alla egenskaper som måste refereras måste bara helt kvalificerade. I frånvaron av strikt schemaanslutning tillämpas detta för att undvika tvetydiga bindningar. Därför är `SELECT id FROM Products p` syntaktiskt ogiltigt eftersom egenskapen `id` inte är bunden.

### <a name="subdocuments-in-a-from-clause"></a>Underdokument i en FROM-sats
Källan kan också reduceras till en mindre delmängd. För att till exempel endast räkna upp ett underträd i varje dokument kan den underordnade roten sedan bli källan, enligt följande exempel:

**Fråga**
```
SELECT * 
FROM Products.shipping
```

**Resultat**  

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

I exemplet ovan används en matris som datakälla men ett objekt kan också användas som källa, vilket visas i följande exempel. Alla giltiga JSON-värden (som inte är odefinierad) som kan hittas i källan tas med i beräkningen att inkluderas i frågeresultatet. Om några produkter inte har ett `shipping.weight`-värde exkluderas de i frågeresultatet.

**Fråga**

    SELECT * 
    FROM Products.shipping.weight

**Resultat**

    [
        1,
        2
    ]

## <a name="where-clause"></a>WHERE-satsen
WHERE-satsen anger villkoren som JSON-dokument som tillhandahålls av källan måste uppfylla för att kunna inkluderas som en del av resultatet. Alla JSON-dokument måste utvärdera de angivna villkoren som **sanna** för att övervägas för resultatet. WHERE-satsen är valfri.

Följande fråga begär dokument som innehåller ett ID vars värde är 1:

**Fråga**

    SELECT p.description
    FROM Products p 
    WHERE p.id = "1"

**Resultat**

    [
        {
            "description": "Quick dry crew neck t-shirt"
        }
    ]

## <a name="order-by-clause"></a>ORDER BY-satsen

ORDER BY-satsen gör det möjligt för dig att ordna resultatet i stigande eller fallande ordning.

Följande ORDER BY-fråga returnerar pris, beskrivning och produkt-ID för alla produkter, ordnade efter pris i stigande ordning:

**Fråga**

    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC

**Resultat**

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

## <a name="join-clause"></a>JOIN-satsen

JOIN-satsen gör det möjligt för dig att utföra inre kopplingar med dokumentet och dokumentets underordnade rötter. Så i produktdatabasen kan du till exempel koppla dokumenten med leveransinformation.  

I följande fråga returneras produkt-ID:n för varje produkt som har en leveransmetod. Om du har lagt till en tredje produkt som inte har en leveransegenskap skulle resultatet vara samma som det tredje objektet och skulle exkluderas för att det inte har en leveransegenskap.

**Fråga**

    SELECT p.productId
    FROM Products p
    JOIN p.shipping

**Resultat**

    [
        {
            "productId": "33218896"
        },
        {
            "productId": "33218897"
        }
    ]

## <a name="geospatial-queries"></a>Geospatiala frågor

Geospatiala frågor kan du utföra rumsliga förfrågningar med GeoJSON punkter. Med koordinaterna i databasen kan du beräkna avståndet mellan två punkter och avgöra om en punkt eller Polygon LineString är i en annan plats, Polygon eller LineString.

För produktkatalogdata skulle detta göra det möjligt för dina användare att ange platsinformation och fastställa om det finns några butiker inom 80 km avstånd som har artikeln som användaren letar efter. 

## <a name="summary"></a>Sammanfattning

Att snabbt kunna fråga alla dina data efter att ha lagt till dem i Azure Cosmos DB och använda välbekanta SQL-frågemetoder kan hjälpa dig och dina kunder för att utforska och få insikter om dina lagrade data.