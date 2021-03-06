<!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Nu när du har skapat dokument i ditt program kan du skicka frågor mot dem från programmet. Azure Cosmos DB använder SQL-frågor och LINQ-frågor. Den här kursdelen handlar om att köra SQL-frågor och LINQ-frågor från ditt program i stället för från portalen.

Du kan testa de här frågorna med hjälp av användardokumenten som du har skapat för ditt onlineförsäljningsprogram.

## <a name="linq-query-basics"></a>Grundläggande om LINQ-frågor

LINQ är en .NET-programmeringsmodell som uttrycker beräkningar som frågor på objektströmmar. Du kan skapa ett **IQueryable**-objekt som frågar Azure Cosmos DB direkt så att LINQ-frågan översätts till en Cosmos DB-fråga. Frågan skickas sedan till Azure Cosmos DB-servern för att hämta en uppsättning resultat i JSON-format. De returnerade resultaten deserialiseras till en .NET-objektström på klientsidan. Många utvecklare föredrar LINQ-frågor eftersom det är en konsekvent programmeringsmodell för hela deras arbete med objekt i programkod och hur de uttrycker frågelogik som körs i databasen.

Hur LINQ-frågor översätts till SQL visas i följande tabell.

| LINQ-uttryck | SQL-översättning |
|---|---|
| `input.Select(family => family.parents[0].familyName);`| `SELECT VALUE f.parents[0].familyName FROM Families f` |
|`input.Select(family => family.children[0].grade + c); // c is an int variable` | `SELECT VALUE f.children[0].grade + c FROM Families f` |
|`input.Select(family => new { name = family.children[0].familyName, grade = family.children[0].grade + 3});`| `SELECT VALUE {"name":f.children[0].familyName, "grade": f.children[0].grade + 3 } FROM Families f`|
|`input.Where(family=> family.parents[0].familyName == "Smith");`|`SELECT * FROM Families f WHERE f.parents[0].familyName = "Smith"`|

## <a name="run-sql-and-linq-queries"></a>Köra SQL- och LINQ-frågor

1. I följande exempel visas hur en fråga kan utföras i SQL, LINQ eller LINQ-lambda från din .NET-kod. Kopiera koden och lägg till den i slutet av filen Program.cs.

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

1. Kopiera och klistra in nedanstående kod i metoden **BasicOperations** före raden `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);`.

    ```csharp
    this.ExecuteSimpleQuery("Users", "WebCustomers");
    ```

1. Kör följande kommando i den integrerade terminalen.

    ```bash
    dotnet run
    ```

    Konsolen visar utdatan för LINQ- och SQL-frågorna.

I den här delen har du fått lära dig om LINQ-frågor, samt lagt till en LINQ- och SQL-fråga i ditt program för att hämta användarposter.
