<span data-ttu-id="54e02-101"><!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Nu när du har skapat dokument i ditt program kan du skicka frågor mot dem från programmet.</span><span class="sxs-lookup"><span data-stu-id="54e02-101"><!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Now that you've created documents in your application, let's query them from your application.</span></span> <span data-ttu-id="54e02-102">Azure Cosmos DB använder SQL-frågor och LINQ-frågor.</span><span class="sxs-lookup"><span data-stu-id="54e02-102">Azure Cosmos DB uses SQL queries and LINQ queries.</span></span> <span data-ttu-id="54e02-103">Den här kursdelen handlar om att köra SQL-frågor och LINQ-frågor från ditt program i stället för från portalen.</span><span class="sxs-lookup"><span data-stu-id="54e02-103">This unit focuses on running SQL queries and LINQ queries from your application, as opposed to the portal.</span></span>

<span data-ttu-id="54e02-104">Du kan testa de här frågorna med hjälp av användardokumenten som du har skapat för ditt onlineförsäljningsprogram.</span><span class="sxs-lookup"><span data-stu-id="54e02-104">We'll use the user documents you've created for your online retailer application to test these queries.</span></span>

## <a name="linq-query-basics"></a><span data-ttu-id="54e02-105">Grundläggande om LINQ-frågor</span><span class="sxs-lookup"><span data-stu-id="54e02-105">LINQ query basics</span></span>

<span data-ttu-id="54e02-106">LINQ är en .NET-programmeringsmodell som uttrycker beräkningar som frågor på objektströmmar.</span><span class="sxs-lookup"><span data-stu-id="54e02-106">LINQ is a .NET programming model that expresses computations as queries on streams of objects.</span></span> <span data-ttu-id="54e02-107">Du kan skapa ett **IQueryable**-objekt som frågar Azure Cosmos DB direkt så att LINQ-frågan översätts till en Cosmos DB-fråga.</span><span class="sxs-lookup"><span data-stu-id="54e02-107">You can create an **IQueryable** object that directly queries Azure Cosmos DB, which translates the LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="54e02-108">Frågan skickas sedan till Azure Cosmos DB-servern för att hämta en uppsättning resultat i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="54e02-108">The query is then passed to the Azure Cosmos DB server to retrieve a set of results in JSON format.</span></span> <span data-ttu-id="54e02-109">De returnerade resultaten deserialiseras till en .NET-objektström på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="54e02-109">The returned results are deserialized into a stream of .NET objects on the client side.</span></span> <span data-ttu-id="54e02-110">Många utvecklare föredrar LINQ-frågor eftersom det är en konsekvent programmeringsmodell för hela deras arbete med objekt i programkod och hur de uttrycker frågelogik som körs i databasen.</span><span class="sxs-lookup"><span data-stu-id="54e02-110">Many developers prefer LINQ queries, as they provide a single consistent programming model across how they work with objects in application code and how they express query logic running in the database.</span></span>

<span data-ttu-id="54e02-111">Hur LINQ-frågor översätts till SQL visas i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="54e02-111">The following table shows how LINQ queries are translated into SQL.</span></span>

| <span data-ttu-id="54e02-112">LINQ-uttryck</span><span class="sxs-lookup"><span data-stu-id="54e02-112">LINQ expression</span></span> | <span data-ttu-id="54e02-113">SQL-översättning</span><span class="sxs-lookup"><span data-stu-id="54e02-113">SQL translation</span></span> |
|---|---|
| `input.Select(family => family.parents[0].familyName);`| `SELECT VALUE f.parents[0].familyName FROM Families f` |
|`input.Select(family => family.children[0].grade + c); // c is an int variable` | `SELECT VALUE f.children[0].grade + c FROM Families f` |
|`input.Select(family => new { name = family.children[0].familyName, grade = family.children[0].grade + 3});`| `SELECT VALUE {"name":f.children[0].familyName, "grade": f.children[0].grade + 3 } FROM Families f`|
|`input.Where(family=> family.parents[0].familyName == "Smith");`|`SELECT * FROM Families f WHERE f.parents[0].familyName = "Smith"`|

## <a name="run-sql-and-linq-queries"></a><span data-ttu-id="54e02-114">Köra SQL- och LINQ-frågor</span><span class="sxs-lookup"><span data-stu-id="54e02-114">Run SQL and LINQ queries</span></span>

1. <span data-ttu-id="54e02-115">I följande exempel visas hur en fråga kan utföras i SQL, LINQ eller LINQ-lambda från din .NET-kod.</span><span class="sxs-lookup"><span data-stu-id="54e02-115">The following sample shows how a query could be performed in SQL, LINQ, or LINQ lambda from your .NET code.</span></span> <span data-ttu-id="54e02-116">Kopiera koden och lägg till den i slutet av filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="54e02-116">Copy the code and add it to the end of the Program.cs file.</span></span>

    ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1, EnableCrossPartitionQuery = true };

        // Here we find nelapin via their LastName
        IQueryable<User> userQuery = this.client.CreateDocumentQuery<User>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(u => u.LastName == "Pindakova");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (User user in userQuery)
        {
            Console.WriteLine("\tRead {0}", user);
        }

        // Now execute the same query via direct SQL
        IQueryable<User> userQueryInSql = this.client.CreateDocumentQuery<User>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM User WHERE User.lastName = 'Pindakova'", queryOptions );

        Console.WriteLine("Running direct SQL query...");
        foreach (User user in userQueryInSql)
        {
                Console.WriteLine("\tRead {0}", user);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }
    ```

1. <span data-ttu-id="54e02-117">Kopiera och klistra in nedanstående kod i metoden **BasicOperations** före raden `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);`.</span><span class="sxs-lookup"><span data-stu-id="54e02-117">Copy and paste the following code to your **BasicOperations** method, before the `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` line.</span></span>

    ```csharp
    this.ExecuteSimpleQuery("Users", "WebCustomers");
    ```

1. <span data-ttu-id="54e02-118">Kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="54e02-118">In the integrated terminal, run the following command.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="54e02-119">Konsolen visar utdatan för LINQ- och SQL-frågorna.</span><span class="sxs-lookup"><span data-stu-id="54e02-119">The console displays the output of the LINQ and SQL queries.</span></span>

<span data-ttu-id="54e02-120">I den här delen har du fått lära dig om LINQ-frågor, samt lagt till en LINQ- och SQL-fråga i ditt program för att hämta användarposter.</span><span class="sxs-lookup"><span data-stu-id="54e02-120">In this unit you learned about LINQ queries, and then added a LINQ and SQL query to your application to retrieve user records.</span></span>
