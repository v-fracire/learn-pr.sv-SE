<!--TODO: explain Etag in knowledge needed-->

När anslutningen till Azure Cosmos DB väl har upprättats är nästa steg att skapa, läsa, ersätta och ta bort dokument som lagras i databasen. I den här modulen ska du skapa användardokument i din WebCustomer-samling och sedan hämta dem med hjälp av ID:n, ersätta dem och ta bort dem.

## <a name="working-with-documents-programmatically"></a>Arbeta programmatiskt med dokument

Data lagras i JSON-dokument i Azure Cosmos DB. [Dokument](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) kan skapas, hämtas, ersättas och tas bort i portalen, som du såg i föregående modul. I den här modulen beskrivs hur du gör det programmässigt. Azure Cosmos DB har SDK:er på klientsidan för .NET, .NET Core, Java, Node.js och Python. De har alla stöd för de här åtgärderna. I den här modulen kommer vi att använda SDK:n för .NET Core till att skapa, hämta, uppdatera och ta bort objekt. 

De viktigaste åtgärderna för Azure Cosmos DB-dokument ingår i klassen [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet):
* [CreateDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [ReadDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [ReplaceDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* [UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet). Upsert skapar eller byter ut beroende på om dokumentet redan finns.
* [DeleteDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

Om du vill utföra någon av de här åtgärderna måste du skapa en klass som representerar objektet som lagras i databasen. Eftersom vi arbetar med en databas med användare kan du skapa klassen **User** (Användare) för att lagra viktiga data som förnamn, efternamn och användar-ID (obligatoriskt eftersom det är partitionsnyckeln som möjliggör horisontell skalning) och underklasser för leveransinställningar och orderhistorik.

När du har skapat de här klasserna för dina användare skapar du nya användardokument för varje instans. Sedan ska vi utföra några enkla åtgärder på dokumenten.

## <a name="create-documents"></a>Skapa dokument

1. Skapa först en **User**-klass som representerar objekten som ska lagras i Azure Cosmos DB. Vi skapar också underklasserna **OrderHistory** och **ShippingPreference** som används i klassen **User**. Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON.

    Du kan skapa dem genom att kopiera och klistra in koden för **användaren**, **OrderHistory** och **ShippingPreference** under metoden **BasicOperations**.

    ```csharp
    public class User
    {
            [JsonProperty("id")]
            public string Id { get; set; }
            [JsonProperty("userId")]
            public string UserId { get; set; }
            [JsonProperty("lastName")]
            public string LastName { get; set; }
            [JsonProperty("firstName")]
            public string FirstName { get; set; }
            [JsonProperty("email")]
            public string Email { get; set; }
            [JsonProperty("dividend")]
            public string Dividend { get; set; }
            [JsonProperty("OrderHistory")]
            public OrderHistory[] OrderHistory { get; set; }
            [JsonProperty("ShippingPreference")]
            public ShippingPreference[] ShippingPreference { get; set; }
            [JsonProperty("CouponsUsed")]
            public CouponsUsed[] Coupons { get; set; }
            public override string ToString()
            {
            return JsonConvert.SerializeObject(this);
            }
    }
    
    public class OrderHistory
    {
            public string OrderId { get; set; }
            public string DateShipped { get; set; }
            public string Total { get; set; }
    }
    
    public class ShippingPreference
    {
            public int Priority { get; set; }
            public string AddressLine1 { get; set; }
            public string AddressLine2 { get; set; }
            public string City { get; set; }
            public string State { get; set; }
            public string ZipCode { get; set; }
            public string Country { get; set; }
    }
    
    public class CouponsUsed
    {
            public string CouponCode { get; set; }
    
    }
    
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }
    ```

2. Ange följande kommando för att köra programmet i den integrerade terminalen och kontrollera att det körs.

    ```csharp
    dotnet run
    ```

3. Kopiera nu och klistra in uppgiften **CreateUserDocumentIfNotExists** under klassen **ShippingPreference**.

    ```csharp
    private async Task CreateUserDocumentIfNotExists(string databaseName, string collectionName, User user)
    {
            try
            {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey("user.userId") });
            this.WriteToConsoleAndPromptToContinue("Found {0}", user.Id);
            }
            catch (DocumentClientException de)
            {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                    await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), user);
                    this.WriteToConsoleAndPromptToContinue("Created User {0}", user.Id);
            }
            else
            {
                    throw;
            }
            }
    }
    ```

4. Lägg sedan till följande i metoden **BasicOperations**.

    ```csharp
     User yanhe = new User
                {
                    Id = "1",
                    UserId = "yanhe",
                    LastName = "He",
                    FirstName = "Yan",
                    Email = "yanhe@todo.com",
                    OrderHistory = new OrderHistory[]
            {
                new OrderHistory {
                    OrderId = "1000",
                    DateShipped = "08/17/2018",
                    Total = "52.49"
                }
            },
                    ShippingPreference = new ShippingPreference[]
            {
                    new ShippingPreference {
                            Priority = 1,
                            AddressLine1 = "90 W 8th St",
                            City = "New York",
                            State = "NY",
                            ZipCode = "10001",
                            Country = "USA"
                    }
            },
                };
    
                await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", yanhe);
    
                User nelapin = new User
                {
                    Id = "2",
                    UserId = "nelapin",
                    LastName = "Pindakova",
                    FirstName = "Nela",
                    Email = "nelapin@todo.com",
                    Dividend = "8.50",
                    OrderHistory = new OrderHistory[]
            {
                new OrderHistory {
                    OrderId = "1001",
                    DateShipped = "08/17/2018",
                    Total = "105.89"
                }
            },
                    ShippingPreference = new ShippingPreference[]
            {
                    new ShippingPreference {
                            Priority = 1,
                            AddressLine1 = "505 NW 5th St",
                            City = "New York",
                            State = "NY",
                            ZipCode = "10001",
                            Country = "USA"
                    },
                    new ShippingPreference {
                            Priority = 2,
                            AddressLine1 = "505 NW 5th St",
                            City = "New York",
                            State = "NY",
                            ZipCode = "10001",
                            Country = "USA"
                    }
            },
                    Coupons = new CouponsUsed[]
             {
                 new CouponsUsed{
                     CouponCode = "Fall2018"
                 }
             }
                };
    
                await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);
    ```

5. Ange återigen följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.

    ```csharp
    dotnet run
    ```

    Du ser följande utdata i terminalen om att båda användarposterna har skapats.

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a>Läsa dokument

1. Om du vill läsa dokument från databasen kopierar du följande kod och placerar den i slutet av filen Program.cs.
    <!--TODO: This is not finding the user-->

    ```csharp
    private async Task ReadUserDocument(string databaseName, string collectionName, User user)
    {
            try
            {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey("user.userId") });
            this.WriteToConsoleAndPromptToContinue("Found user {0}", user.Id);
            }
            catch (DocumentClientException de)
            {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not retrieved", user.Id);
            }
            else
            {
                throw;
            }
            }
    }
    ```

2.  Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**, efter raden `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

4. Spara filen Program.cs och kör följande kommando i den integrerade terminalen.

    ```
    dotnet run
    ```
    Du ser följande utdata i terminalen, och ”Found user 1” innebär att dokumentet hittades.

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Found user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a>Ersätta dokument
Azure Cosmos DB har stöd för ersättning av JSON-dokument. I det här fallet ska vi uppdatera en användarpost eftersom efternamnet har ändrats.

1. Kopiera och klistra in metoden **ReplaceFamilyDocument** i slutet av filen Program.cs.
    <!--TODO: This is not finding user to replace-->
    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, string LastName, User updatedUser)
    {
        try
        {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, LastName), new RequestOptions { PartitionKey = new PartitionKey("updatedUser.UserId") });
        this.WriteToConsoleAndPromptToContinue("Replaced last name for {0}", updatedUser);
        }
        catch (DocumentClientException de)
        {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            this.WriteToConsoleAndPromptToContinue("User {0} not found for replacement", updatedUser.Id);
        }
        else
        {
            throw;
        }
        }
    }
    ```

2. Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**, efter raden `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe.LastName, yanhe);
    ```

4. Spara filen Program.cs och kör följande kommando i den integrerade terminalen.

    ```
    dotnet run
    ```
    Du ser följande utdata i terminalen, och ”Replaced last name for Suh” innebär att dokumentet ersattes.

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Found user 1
    Press any key to continue ...
    Replaced last name for Suh
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a>Ta bort dokument

1. Kopiera och klistra in metoden **DeleteUserDocument** under metoden **ReplaceUserDocument**.
    
    ```csharp
    private async Task DeleteUserDocument(string databaseName, string collectionName, string id, User deletedUser)
    {
        try
        {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, id), new RequestOptions { PartitionKey = new PartitionKey("User.UserId") });
        Console.WriteLine("Deleted user {0}", id);
        }
        catch (DocumentClientException de)
        {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            this.WriteToConsoleAndPromptToContinue("User {0} not found for deletion", deletedUser.Id);
        }
        else
        {
            throw;
        }
        }
    }
    ```

2. Kopiera och klistra in följande kod i metoden **BasicOperations** under den andra frågekörningen.

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", "1", yanhe);
    ```

3. Spara filen Program.cs och kör följande kommando i den integrerade terminalen.

    ```
    dotnet run
    ```

    Du ser följande utdata i terminalen, och ”Replaced last name for Suh” innebär att dokumentet ersattes.

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Found user 1
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    Deleted user 1
    End of demo, press any key to exit.
    ```

    Grattis! Du har tagit bort ett Azure Cosmos DB-dokument.

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du skapat, ersatt och tagit bort dokument i din Azure Cosmos DB-databas.