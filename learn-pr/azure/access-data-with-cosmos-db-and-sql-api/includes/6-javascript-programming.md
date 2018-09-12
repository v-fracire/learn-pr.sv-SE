<span data-ttu-id="578be-101">Du måste ofta uppdatera flera dokument i dina databaser samtidigt.</span><span class="sxs-lookup"><span data-stu-id="578be-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> 

<span data-ttu-id="578be-102">När en användare gör en beställning i ditt onlinebutiksprogram och vill använda en kupongkod, kredit eller en återbäring (eller alla tre samtidigt) behöver du fråga på användarens konto efter de alternativen, göra uppdateringar av kontot som visar att de har använts, uppdatera beställningens summa och behandla beställningen.</span><span class="sxs-lookup"><span data-stu-id="578be-102">For your online retail application, when a user places an order and wants to use a coupon code, a credit, or a dividend (or all three at once), you need to query their account for those options, make updates to their account indicating they used them, update the order total, and process the order.</span></span>

<span data-ttu-id="578be-103">Alla dessa åtgärder måste ske samtidigt, inom en enda transaktion.</span><span class="sxs-lookup"><span data-stu-id="578be-103">All of these actions need to happen at the same time, within a single transaction.</span></span> <span data-ttu-id="578be-104">Om användaren väljer att annullera beställningen så måste du återställa ändringarna och inte ändra kontoinformationen – så att kupongkoder, krediter och återbäringar är tillgängliga för nästa köp.</span><span class="sxs-lookup"><span data-stu-id="578be-104">If the user chooses to cancel the order, you want to roll back the changes and not modify their account information, so that their coupon codes, credits, and dividends are available for their next purchase.</span></span>

<span data-ttu-id="578be-105">Sättet att utföra dessa transaktioner på i Azure Cosmos DB är genom att använda lagrade procedurer och användardefinierade funktioner (UDF:er).</span><span class="sxs-lookup"><span data-stu-id="578be-105">The way to perform these transactions in Azure Cosmos DB is by using stored procedures and user-defined functions (UDFs).</span></span> <span data-ttu-id="578be-106">Lagrade procedurer är det enda sättet att säkerställa ACID-transaktioner (atomicitet, konsekvens, isolering, varaktighet) eftersom de körs på servern och därför refereras till som programmering på serversidan.</span><span class="sxs-lookup"><span data-stu-id="578be-106">Stored procedures are the only way to ensure ACID (Atomicity, Consistency, Isolation, Durability) transactions because they are run on the server, and are thus referred to as server-side programming.</span></span> <span data-ttu-id="578be-107">UDF:er lagras också på servern och används vid frågor för att utföra databaserad logik på värden eller dokument i frågan.</span><span class="sxs-lookup"><span data-stu-id="578be-107">UDFs are also stored on the server and are used during queries to perform computational logic on values or documents within the query.</span></span> 

<span data-ttu-id="578be-108">I den här modulen får du lära dig om lagrade procedurer och UDF:er och får sedan köra några i portalen.</span><span class="sxs-lookup"><span data-stu-id="578be-108">In this module, you'll learn about stored procedures and UDFs, and then run some in the portal.</span></span>

## <a name="stored-procedure-basics"></a><span data-ttu-id="578be-109">Grunderna om lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="578be-109">Stored procedure basics</span></span>

<span data-ttu-id="578be-110">Lagrade procedurer utför komplexa transaktioner på dokument och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="578be-110">Stored procedures perform complex transactions on documents and properties.</span></span> <span data-ttu-id="578be-111">Lagrade procedurer skrivs i JavaScript och lagras i en samling i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="578be-111">Stored procedures are written in JavaScript and are stored in a collection on Azure Cosmos DB.</span></span> <span data-ttu-id="578be-112">Genom att utföra de lagrade procedurerna på databasmotorn och nära data kan du förbättra prestanda över klientprogrammering.</span><span class="sxs-lookup"><span data-stu-id="578be-112">By performing the stored procedures on the database engine and close to the data, you can improve performance over client-side programming.</span></span>

<span data-ttu-id="578be-113">Lagrade procedurer är det enda sättet att åstadkomma atomiska transaktioner i Azure Cosmos DB, SDK:er på klientsidan stöder inte transaktioner.</span><span class="sxs-lookup"><span data-stu-id="578be-113">Stored procedures are the only way to achieve atomic transactions within Azure Cosmos DB; the client-side SDKs do not support transactions.</span></span>

<span data-ttu-id="578be-114">Att utföra batchåtgärder i lagrade procedurer rekommenderas också på grund av det minskade behovet att skapa separata transaktioner.</span><span class="sxs-lookup"><span data-stu-id="578be-114">Performing batch operations in stored procedures is also recommended because of the reduced need to create separate transactions.</span></span>

<!--TODO: Ideally I'd like to list some cases where a stored procedure is not the best option.-->

## <a name="stored-procedure-example"></a><span data-ttu-id="578be-115">Exempel på lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="578be-115">Stored procedure example</span></span>

<span data-ttu-id="578be-116">Följande exempel är en enkel lagrad HelloWorld-procedur som hämtar den aktuella kontexten och skickar ett svar som visar ”Hello, World”.</span><span class="sxs-lookup"><span data-stu-id="578be-116">The following sample is a simple HelloWorld stored procedure that gets the current context and sends a response that displays "Hello, World".</span></span> <span data-ttu-id="578be-117">Observera att den lagrade proceduren har ett ID-värde, precis som Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="578be-117">Note that the stored procedure has an ID value, just like Azure Cosmos DB documents.</span></span>

```java
var helloWorldStoredProc = {
    id: "helloWorld",
    serverScript: function () {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
}
```

## <a name="user-defined-function-basics"></a><span data-ttu-id="578be-118">Grunderna om användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="578be-118">User-defined function basics</span></span>

<span data-ttu-id="578be-119">UDF:er används till att utöka Azure Cosmos DB SQL-frågegrammatiken och implementera anpassad affärslogik som beräkningar i egenskaper och dokument.</span><span class="sxs-lookup"><span data-stu-id="578be-119">UDFs are used to extend the Azure Cosmos DB SQL query language grammar and implement custom business logic, such as calculations on properties and documents.</span></span> <span data-ttu-id="578be-120">UDF:er kan bara anropas inifrån frågor och till skillnad från lagrade procedurer så har de inte åtkomst till kontextobjektet, så de kan inte läsa eller skriva dokument.</span><span class="sxs-lookup"><span data-stu-id="578be-120">UDFs can be called only from inside queries and, unlike stored procedures, they do not have access to the context object, so they cannot read or write documents.</span></span>

<span data-ttu-id="578be-121">I ett scenario med onlinehandel kan en UDF användas för att fastställa om momsen ska tillämpas på beställningens totalsumma eller om en procentuell rabatt ska tillämpas på produkter eller beställningar.</span><span class="sxs-lookup"><span data-stu-id="578be-121">In an online commerce scenario, a UDF could be used to determine the sales tax to apply to an order total or a percentage discount to apply to products or orders.</span></span>

## <a name="user-defined-function-example"></a><span data-ttu-id="578be-122">Exempel på en användardefinierad funktion</span><span class="sxs-lookup"><span data-stu-id="578be-122">User-defined function example</span></span>

<span data-ttu-id="578be-123">I följande exempel skapas en UDF för att beräkna rabatter baserat på en beställnings totalsumma och sedan returneras den ändrade beställningssumman baserat på rabatten:</span><span class="sxs-lookup"><span data-stu-id="578be-123">The following sample creates a UDF to calculate discounts based on an order total, and then it returns the modified order total based on the discount:</span></span>

```java
var discountUdf = {
    id: "discount",
    serverScript: function discount(orderTotal) {

        if(orderTotal == undefined) 
            throw 'no input';

        if (orderTotal < 50) 
            return orderTotal * 0.9;
        else if (orderTotal < 100) 
            return orderTotal * 0.8;
        else
            return orderTotal * 0.7;
    }
}
```

## <a name="create-a-stored-procedure-in-the-portal"></a><span data-ttu-id="578be-124">Skapa en lagrad procedur i portalen</span><span class="sxs-lookup"><span data-stu-id="578be-124">Create a stored procedure in the portal</span></span>

<span data-ttu-id="578be-125">Nu ska vi skapa en ny lagrad procedur i portalen.</span><span class="sxs-lookup"><span data-stu-id="578be-125">Let's create a new stored procedure in the portal.</span></span> <span data-ttu-id="578be-126">Portalen fyller automatiskt i en enkel lagrad procedur som hämtar det första objektet i samlingen, så vi kör den här lagrade proceduren först.</span><span class="sxs-lookup"><span data-stu-id="578be-126">The portal automatically populates a simple stored procedure that retrieves the first item in the collection, so we'll run this stored procedure first.</span></span>

1. <span data-ttu-id="578be-127">I Datautforskaren klickar du på **Ny lagrad procedur**.</span><span class="sxs-lookup"><span data-stu-id="578be-127">In the Data Explorer, click **New Stored Procedure**.</span></span>

    <span data-ttu-id="578be-128">Datautforskaren visar en ny flik med ett exempel på lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="578be-128">Data Explorer displays a new tab with a sample stored procedure.</span></span>

  <!--TODO: Insert animated .gif of creating the stored procedure.-->

2. <span data-ttu-id="578be-129">I rutan **ID för lagrad procedur** anger du namnet *sample*, klickar på **Spara** och klickar sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="578be-129">In the **Stored Procedure Id** box, enter the name *sample*, click **Save**, and then click **Execute**.</span></span>


3. <span data-ttu-id="578be-130">I rutan **Indataparametrar** skriver du namnet på partitionsnyckeln, *33218896* och klickar sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="578be-130">In the **Input parameters** box, type the name of a partition key, *33218896*, and then click **Execute**.</span></span> <span data-ttu-id="578be-131">Observera att lagrade procedurer fungerar i en enda partition.</span><span class="sxs-lookup"><span data-stu-id="578be-131">Note that stored procedures work within a single partition.</span></span>

    ![Kör en lagrad procedur i portalen](../media-draft/6-stored-procedure.gif)

    <span data-ttu-id="578be-133">**Resultat**fönstret visar flödet från det första dokumentet i samlingen.</span><span class="sxs-lookup"><span data-stu-id="578be-133">The **Result** pane displays the feed from the first document in the collection.</span></span>

## <a name="create-a-stored-procedure-that-creates-documents"></a><span data-ttu-id="578be-134">Skapa en lagrad procedur som skapar dokument</span><span class="sxs-lookup"><span data-stu-id="578be-134">Create a stored procedure that creates documents</span></span>

<span data-ttu-id="578be-135">Nu ska vi skapa en lagrad procedur som skapar dokument.</span><span class="sxs-lookup"><span data-stu-id="578be-135">Now, let's create a stored procedure that creates documents.</span></span>

1. <span data-ttu-id="578be-136">I Datautforskaren klickar du på **Ny lagrad procedur**.</span><span class="sxs-lookup"><span data-stu-id="578be-136">In the Data Explorer, click **New Stored Procedure**.</span></span> <span data-ttu-id="578be-137">Ge den här lagrade proceduren namnet *createDocuments*, klicka på **Spara** och klicka sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="578be-137">Name this stored procedure *createDocuments*, click **Save**, and then click **Execute**.</span></span>

    ```java
    var createDocumentStoredProc = {
        id: "createMyDocument",
        productid: "5"
        serverScript: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();
    
            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }
    ```

<!--TODO: Need to fix code above.-->

2. <span data-ttu-id="578be-138">Ange partitionsnyckelvärdet *3* och klicka sedan på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="578be-138">Enter a partition key value of *3*, and then click **Execute**.</span></span>

    <span data-ttu-id="578be-139">Datautforskaren visar det nya dokumentet.</span><span class="sxs-lookup"><span data-stu-id="578be-139">Data Explorer displays the newly created document.</span></span> 

## <a name="create-a-user-defined-function"></a><span data-ttu-id="578be-140">Skapa en användardefinierad funktion</span><span class="sxs-lookup"><span data-stu-id="578be-140">Create a user-defined function</span></span>

<span data-ttu-id="578be-141">Nu ska vi skapa en UDF i Datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="578be-141">Now, let's create a UDF in Data Explorer.</span></span>

<span data-ttu-id="578be-142">Klicka på **Ny UDF** i Datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="578be-142">In the Data Explorer, click **New UDF**.</span></span> <span data-ttu-id="578be-143">Kopiera följande kod i fönstret, namnet UDF:en *tax* och klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="578be-143">Copy the following code into the window, name the UDF *tax*, and then click **Save**.</span></span> <span data-ttu-id="578be-144">Du kan inte köra UDF:en från portalen men vi använder den i en senare modul.</span><span class="sxs-lookup"><span data-stu-id="578be-144">There's no way to run the UDF from the portal, but we'll use it in a later module.</span></span>

```java
function userDefinedFunction(){
    var taxUdf = {
        id: "tax",
        serverScript: function tax(income) {

            if(income == undefined) 
                throw 'no input';

            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }
}
```

