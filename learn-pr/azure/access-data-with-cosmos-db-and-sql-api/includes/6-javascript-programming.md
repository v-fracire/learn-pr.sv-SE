<span data-ttu-id="b60fe-101">Du måste ofta uppdatera flera dokument i dina databaser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="b60fe-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> 

<span data-ttu-id="b60fe-102">När en användare gör en beställning i ditt onlinebutiksprogram och vill använda en kupongkod, kredit eller en återbäring (eller alla tre samtidigt) behöver du fråga på användarens konto efter de alternativen, göra uppdateringar av kontot som visar att de har använts, uppdatera beställningens summa och behandla beställningen.</span><span class="sxs-lookup"><span data-stu-id="b60fe-102">For your online retail application, when a user places an order and wants to use a coupon code, a credit, or a dividend (or all three at once), you need to query their account for those options, make updates to their account indicating they used them, update the order total, and process the order.</span></span>

<span data-ttu-id="b60fe-103">Alla dessa åtgärder måste ske samtidigt, inom en enda transaktion.</span><span class="sxs-lookup"><span data-stu-id="b60fe-103">All of these actions need to happen at the same time, within a single transaction.</span></span> <span data-ttu-id="b60fe-104">Om användaren väljer att annullera beställningen så måste du återställa ändringarna och inte ändra kontoinformationen – så att kupongkoder, krediter och återbäringar är tillgängliga för nästa köp.</span><span class="sxs-lookup"><span data-stu-id="b60fe-104">If the user chooses to cancel the order, you want to roll back the changes and not modify their account information, so that their coupon codes, credits, and dividends are available for their next purchase.</span></span>

<span data-ttu-id="b60fe-105">Sättet att utföra dessa transaktioner på i Azure Cosmos DB är genom att använda lagrade procedurer och användardefinierade funktioner (UDF:er).</span><span class="sxs-lookup"><span data-stu-id="b60fe-105">The way to perform these transactions in Azure Cosmos DB is by using stored procedures and user-defined functions (UDFs).</span></span> <span data-ttu-id="b60fe-106">Lagrade procedurer är det enda sättet att säkerställa ACID-transaktioner (atomicitet, konsekvens, isolering, varaktighet) eftersom de körs på servern och därför refereras till som programmering på serversidan.</span><span class="sxs-lookup"><span data-stu-id="b60fe-106">Stored procedures are the only way to ensure ACID (Atomicity, Consistency, Isolation, Durability) transactions because they are run on the server, and are thus referred to as server-side programming.</span></span> <span data-ttu-id="b60fe-107">UDF:er lagras också på servern och används vid frågor för att utföra databaserad logik på värden eller dokument i frågan.</span><span class="sxs-lookup"><span data-stu-id="b60fe-107">UDFs are also stored on the server and are used during queries to perform computational logic on values or documents within the query.</span></span> 

<span data-ttu-id="b60fe-108">I den här modulen får du lära dig om lagrade procedurer och UDF:er och får sedan köra några i portalen.</span><span class="sxs-lookup"><span data-stu-id="b60fe-108">In this module, you'll learn about stored procedures and UDFs, and then run some in the portal.</span></span>

## <a name="stored-procedure-basics"></a><span data-ttu-id="b60fe-109">Grunderna om lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="b60fe-109">Stored procedure basics</span></span>

<span data-ttu-id="b60fe-110">Lagrade procedurer utför komplexa transaktioner på dokument och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b60fe-110">Stored procedures perform complex transactions on documents and properties.</span></span> <span data-ttu-id="b60fe-111">Lagrade procedurer skrivs i JavaScript och lagras i en samling i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b60fe-111">Stored procedures are written in JavaScript and are stored in a collection on Azure Cosmos DB.</span></span> <span data-ttu-id="b60fe-112">Genom att utföra de lagrade procedurerna på databasmotorn och nära data kan du förbättra prestanda över klientprogrammering.</span><span class="sxs-lookup"><span data-stu-id="b60fe-112">By performing the stored procedures on the database engine and close to the data, you can improve performance over client-side programming.</span></span>

<span data-ttu-id="b60fe-113">Lagrade procedurer är det enda sättet att åstadkomma atomiska transaktioner i Azure Cosmos DB, SDK:er på klientsidan stöder inte transaktioner.</span><span class="sxs-lookup"><span data-stu-id="b60fe-113">Stored procedures are the only way to achieve atomic transactions within Azure Cosmos DB; the client-side SDKs do not support transactions.</span></span>

<span data-ttu-id="b60fe-114">Att utföra batchåtgärder i lagrade procedurer rekommenderas också på grund av det minskade behovet att skapa separata transaktioner.</span><span class="sxs-lookup"><span data-stu-id="b60fe-114">Performing batch operations in stored procedures is also recommended because of the reduced need to create separate transactions.</span></span>

<!--TODO: Ideally I'd like to list some cases where a stored procedure is not the best option.-->

## <a name="stored-procedure-example"></a><span data-ttu-id="b60fe-115">Exempel på lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="b60fe-115">Stored procedure example</span></span>

<span data-ttu-id="b60fe-116">Följande exempel är en enkel lagrad HelloWorld-procedur som hämtar den aktuella kontexten och skickar ett svar som visar ”Hello, World”.</span><span class="sxs-lookup"><span data-stu-id="b60fe-116">The following sample is a simple HelloWorld stored procedure that gets the current context and sends a response that displays "Hello, World".</span></span> <span data-ttu-id="b60fe-117">Observera att den lagrade proceduren har ett ID-värde, precis som Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="b60fe-117">Note that the stored procedure has an ID value, just like Azure Cosmos DB documents.</span></span>

```javascript
function helloWorld() {
    var context = getContext();
    var response = context.getResponse();

    response.setBody("Hello, World");
}
```

## <a name="user-defined-function-basics"></a><span data-ttu-id="b60fe-118">Grunderna om användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="b60fe-118">User-defined function basics</span></span>

<span data-ttu-id="b60fe-119">UDF:er används till att utöka Azure Cosmos DB SQL-frågegrammatiken och implementera anpassad affärslogik som beräkningar i egenskaper och dokument.</span><span class="sxs-lookup"><span data-stu-id="b60fe-119">UDFs are used to extend the Azure Cosmos DB SQL query language grammar and implement custom business logic, such as calculations on properties and documents.</span></span> <span data-ttu-id="b60fe-120">UDF:er kan bara anropas inifrån frågor och till skillnad från lagrade procedurer så har de inte åtkomst till kontextobjektet, så de kan inte läsa eller skriva dokument.</span><span class="sxs-lookup"><span data-stu-id="b60fe-120">UDFs can be called only from inside queries and, unlike stored procedures, they do not have access to the context object, so they cannot read or write documents.</span></span>

<span data-ttu-id="b60fe-121">I ett scenario med onlinehandel kan en UDF användas för att fastställa om momsen ska tillämpas på beställningens totalsumma eller om en procentuell rabatt ska tillämpas på produkter eller beställningar.</span><span class="sxs-lookup"><span data-stu-id="b60fe-121">In an online commerce scenario, a UDF could be used to determine the sales tax to apply to an order total or a percentage discount to apply to products or orders.</span></span>

## <a name="user-defined-function-example"></a><span data-ttu-id="b60fe-122">Exempel på en användardefinierad funktion</span><span class="sxs-lookup"><span data-stu-id="b60fe-122">User-defined function example</span></span>

<span data-ttu-id="b60fe-123">Följande exempel skapar en UDF för att beräkna moms på en produkt i det fiktiva företaget baserat på produktkostnaden:</span><span class="sxs-lookup"><span data-stu-id="b60fe-123">The following sample creates a UDF to calculate tax on a product in a fictitious company based the product cost:</span></span>

```javascript
function producttax(price) {
    if (price == undefined) 
        throw 'no input';

    var amount = parseFloat(price);

    if (amount < 1000) 
        return amount * 0.1;
    else if (amount < 10000) 
        return amount * 0.2;
    else
        return amount * 0.4;
}
```

## <a name="create-a-stored-procedure-in-the-portal"></a><span data-ttu-id="b60fe-124">Skapa en lagrad procedur i portalen</span><span class="sxs-lookup"><span data-stu-id="b60fe-124">Create a stored procedure in the portal</span></span>

<span data-ttu-id="b60fe-125">Nu ska vi skapa en ny lagrad procedur i portalen.</span><span class="sxs-lookup"><span data-stu-id="b60fe-125">Let's create a new stored procedure in the portal.</span></span> <span data-ttu-id="b60fe-126">Portalen fyller automatiskt i en enkel lagrad procedur som hämtar det första objektet i samlingen, så vi kör den här lagrade proceduren först.</span><span class="sxs-lookup"><span data-stu-id="b60fe-126">The portal automatically populates a simple stored procedure that retrieves the first item in the collection, so we'll run this stored procedure first.</span></span>

1. <span data-ttu-id="b60fe-127">I Datautforskaren klickar du på **Ny lagrad procedur**.</span><span class="sxs-lookup"><span data-stu-id="b60fe-127">In the Data Explorer, click **New Stored Procedure**.</span></span>

    <span data-ttu-id="b60fe-128">Datautforskaren visar en ny flik med ett exempel på lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="b60fe-128">Data Explorer displays a new tab with a sample stored procedure.</span></span>

2. <span data-ttu-id="b60fe-129">I rutan **ID för lagrad procedur** anger du namnet *sample*, klickar på **Spara** och klickar sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="b60fe-129">In the **Stored Procedure Id** box, enter the name *sample*, click **Save**, and then click **Execute**.</span></span>


3. <span data-ttu-id="b60fe-130">I rutan **Indataparametrar** skriver du namnet på partitionsnyckeln, *33218896* och klickar sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="b60fe-130">In the **Input parameters** box, type the name of a partition key, *33218896*, and then click **Execute**.</span></span> <span data-ttu-id="b60fe-131">Observera att lagrade procedurer fungerar i en enda partition.</span><span class="sxs-lookup"><span data-stu-id="b60fe-131">Note that stored procedures work within a single partition.</span></span>

    ![Kör en lagrad procedur i portalen](../media/6-stored-procedure.gif)

    <span data-ttu-id="b60fe-133">**Resultat**fönstret visar flödet från det första dokumentet i samlingen.</span><span class="sxs-lookup"><span data-stu-id="b60fe-133">The **Result** pane displays the feed from the first document in the collection.</span></span>

## <a name="create-a-stored-procedure-that-creates-documents"></a><span data-ttu-id="b60fe-134">Skapa en lagrad procedur som skapar dokument</span><span class="sxs-lookup"><span data-stu-id="b60fe-134">Create a stored procedure that creates documents</span></span>

<span data-ttu-id="b60fe-135">Nu ska vi skapa en lagrad procedur som skapar dokument.</span><span class="sxs-lookup"><span data-stu-id="b60fe-135">Now, let's create a stored procedure that creates documents.</span></span>

1. <span data-ttu-id="b60fe-136">I Datautforskaren klickar du på **Ny lagrad procedur**.</span><span class="sxs-lookup"><span data-stu-id="b60fe-136">In the Data Explorer, click **New Stored Procedure**.</span></span> <span data-ttu-id="b60fe-137">Ge den lagrade proceduren namnet *createMyDocuments*, kopiera och klistra in följande kod i rutan **Brödtext för lagrad procedur**, klicka på **Spara** och sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="b60fe-137">Name this stored procedure *createMyDocument*, copy and paste the following code into the **Stored Procedure Body** box, click **Save**, and then click **Execute**.</span></span>

    ```javascript
    function createMyDocument() {
        var context = getContext();
        var collection = context.getCollection();

        var doc = {
            "id": "3",
            "productId": "33218898",
            "description": "Contoso microfleece zip-up jacket",
            "price": "44.99"
        };

        var accepted = collection.createDocument(collection.getSelfLink(),
            doc,
            function (err, documentCreated) {
                if (err) throw new Error('Error' + err.message);
                context.getResponse().setBody(documentCreated)
            });
        if (!accepted) return;
    }
    ```

2. <span data-ttu-id="b60fe-138">I rutan Indataparametrar anger du partitionsnyckelvärdet *33218898* och klickar sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="b60fe-138">In the Input parameters box, enter a Partition Key Value of *33218898*, and then click **Execute**.</span></span>

    <span data-ttu-id="b60fe-139">Datautforskaren visar det nya dokumentet i resultatområdet.</span><span class="sxs-lookup"><span data-stu-id="b60fe-139">Data Explorer displays the newly created document in the Result area.</span></span>

    <span data-ttu-id="b60fe-140">Du kan gå tillbaka till fliken Dokument, klicka på knappen **Uppdatera** och visa det nya dokumentet.</span><span class="sxs-lookup"><span data-stu-id="b60fe-140">You can go back to the Documents tab, click the **Refresh** button and see the new document.</span></span> 

## <a name="create-a-user-defined-function"></a><span data-ttu-id="b60fe-141">Skapa en användardefinierad funktion</span><span class="sxs-lookup"><span data-stu-id="b60fe-141">Create a user-defined function</span></span>

<span data-ttu-id="b60fe-142">Nu ska vi skapa en UDF i Datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="b60fe-142">Now, let's create a UDF in Data Explorer.</span></span>

<span data-ttu-id="b60fe-143">Klicka på **Ny UDF** i Datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="b60fe-143">In the Data Explorer, click **New UDF**.</span></span> <span data-ttu-id="b60fe-144">Du kan behöva klicka på nedåtpilen bredvid **Ny lagrad procedur** för att **Ny UDF** ska visas.</span><span class="sxs-lookup"><span data-stu-id="b60fe-144">You may need to click the down arrow next to **New Stored Prodedure** to see **New UDF**.</span></span> <span data-ttu-id="b60fe-145">Kopiera följande kod i fönstret, ge UDF:en namnet *producttax* och klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b60fe-145">Copy the following code into the window, name the UDF *producttax*, and then click **Save**.</span></span>

```javascript
function producttax(price) {
    if (price == undefined) 
        throw 'no input';

    var amount = parseFloat(price);

    if (amount < 1000) 
        return amount * 0.1;
    else if (amount < 10000) 
        return amount * 0.2;
    else
        return amount * 0.4;
}
```

<span data-ttu-id="b60fe-146">När du har definierat UDF:en går du till fliken **Fråga 1** och kopierar och klistrar in följande fråga i frågeområdet för att köra UDF:en.</span><span class="sxs-lookup"><span data-stu-id="b60fe-146">Once you have defined the UDF, go to the **Query 1** tab and copy and paste the following query into the query area to run the UDF.</span></span>

```sql
SELECT c.id, c.productId, c.price, udf.producttax(c.price) AS producttax FROM c
```
