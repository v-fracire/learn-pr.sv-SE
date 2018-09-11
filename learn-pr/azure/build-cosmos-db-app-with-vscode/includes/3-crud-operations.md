<!--TODO: explain Etag in knowledge needed-->

<span data-ttu-id="c49a2-101">När anslutningen till Azure Cosmos DB väl har upprättats är nästa steg att skapa, läsa, ersätta och ta bort dokument som lagras i databasen.</span><span class="sxs-lookup"><span data-stu-id="c49a2-101">Once the connection to Azure Cosmos DB has been made, the next step is to create, read, replace, and delete the documents that are stored in the database.</span></span> <span data-ttu-id="c49a2-102">I den här kursdelen ska du skapa användardokument i din WebCustomer-samling och sedan hämta dem med hjälp av ID:n, ersätta dem och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="c49a2-102">In this unit, you will create User documents in your WebCustomer collection, then you'll retrieve them by ID, replace them, and delete them.</span></span>

## <a name="working-with-documents-programmatically"></a><span data-ttu-id="c49a2-103">Arbeta programmatiskt med dokument</span><span class="sxs-lookup"><span data-stu-id="c49a2-103">Working with documents programmatically</span></span>

<span data-ttu-id="c49a2-104">Data lagras i JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c49a2-104">Data is stored in JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="c49a2-105">[Dokument](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) kan skapas, hämtas, ersättas och tas bort i portalen, som du såg i föregående modul. I den här modulen beskrivs hur du gör det programmässigt.</span><span class="sxs-lookup"><span data-stu-id="c49a2-105">[Documents](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) can be created, retrieved, replaced, or deleted in the portal, as shown in the previous module, or programmatically, as described in this module.</span></span> <span data-ttu-id="c49a2-106">Azure Cosmos DB har SDK:er på klientsidan för .NET, .NET Core, Java, Node.js och Python. De har alla stöd för de här åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="c49a2-106">Azure Cosmos DB provides client-side SDKs for .NET, .NET Core, Java, Node.js, and Python, each of which supports these operations.</span></span> <span data-ttu-id="c49a2-107">I den här modulen kommer vi att använda SDK:n för .NET Core till att skapa, hämta, uppdatera och ta bort objekt.</span><span class="sxs-lookup"><span data-stu-id="c49a2-107">In this module we'll be using the .NET Core SDK to perform CRUD (create, retrieve, update, and delete) operations.</span></span> 

<span data-ttu-id="c49a2-108">De viktigaste åtgärderna för Azure Cosmos DB-dokument ingår i klassen [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet):</span><span class="sxs-lookup"><span data-stu-id="c49a2-108">The main operations for Azure Cosmos DB documents are part of the [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) class:</span></span>
* [<span data-ttu-id="c49a2-109">CreateDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="c49a2-109">CreateDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="c49a2-110">ReadDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="c49a2-110">ReadDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="c49a2-111">ReplaceDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="c49a2-111">ReplaceDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* <span data-ttu-id="c49a2-112">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="c49a2-112">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span></span> <span data-ttu-id="c49a2-113">Upsert skapar eller byter ut beroende på om dokumentet redan finns.</span><span class="sxs-lookup"><span data-stu-id="c49a2-113">Upsert performs a create or replace operation depending on whether the document already exists.</span></span>
* [<span data-ttu-id="c49a2-114">DeleteDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="c49a2-114">DeleteDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

<span data-ttu-id="c49a2-115">Om du vill utföra någon av de här åtgärderna måste du skapa en klass som representerar objektet som lagras i databasen.</span><span class="sxs-lookup"><span data-stu-id="c49a2-115">To perform any of these operations, you need to create a class that represents the object stored in the database.</span></span> <span data-ttu-id="c49a2-116">Eftersom vi arbetar med en databas med användare kan du skapa klassen **User** (Användare) för att lagra viktiga data som förnamn, efternamn och användar-ID (obligatoriskt eftersom det är partitionsnyckeln som möjliggör horisontell skalning) och underklasser för leveransinställningar och orderhistorik.</span><span class="sxs-lookup"><span data-stu-id="c49a2-116">Because we're working with a database of users, you'll want to create a **User** class to store primary data such as their first name, last name, and user id (which is required, as that's the partition key to enable horizontal scaling) and subclasses for shipping preferences and order history.</span></span>

<span data-ttu-id="c49a2-117">När du har skapat de här klasserna för dina användare skapar du nya användardokument för varje instans. Sedan ska vi utföra några enkla åtgärder på dokumenten.</span><span class="sxs-lookup"><span data-stu-id="c49a2-117">Once you have those classes created to represent your users, you'll create new user documents for each instance, and then we'll perform some simple CRUD operations on the documents.</span></span>

## <a name="create-documents"></a><span data-ttu-id="c49a2-118">Skapa dokument</span><span class="sxs-lookup"><span data-stu-id="c49a2-118">Create documents</span></span>

1. <span data-ttu-id="c49a2-119">Skapa först en **User**-klass som representerar objekten som ska lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c49a2-119">First, create a **User** class that represents the objects to store in Azure Cosmos DB.</span></span> <span data-ttu-id="c49a2-120">Vi skapar också underklasserna **OrderHistory** och **ShippingPreference** som används i klassen **User**.</span><span class="sxs-lookup"><span data-stu-id="c49a2-120">We will also create **OrderHistory** and **ShippingPreference** subclasses that are used within **User**.</span></span> <span data-ttu-id="c49a2-121">Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON.</span><span class="sxs-lookup"><span data-stu-id="c49a2-121">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span>

    <span data-ttu-id="c49a2-122">Du kan skapa dem genom att kopiera och klistra in koden för **användaren**, **OrderHistory** och **ShippingPreference** under metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="c49a2-122">To create these classes, copy and paste the following **User**, **OrderHistory**, and **ShippingPreference** classes underneath the **BasicOperations** method.</span></span>

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

1. <span data-ttu-id="c49a2-123">Ange följande kommando för att köra programmet i den integrerade terminalen och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="c49a2-123">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="c49a2-124">Kopiera nu och klistra in uppgiften **CreateUserDocumentIfNotExists** under klassen **ShippingPreference**.</span><span class="sxs-lookup"><span data-stu-id="c49a2-124">Now copy and paste the **CreateUserDocumentIfNotExists** task under the **ShippingPreference** class.</span></span>

    ```csharp
    private async Task CreateUserDocumentIfNotExists(string databaseName, string collectionName, User user)
        {
            try
            {
                await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
                this.WriteToConsoleAndPromptToContinue("User {0} already exists in the database", user.Id);
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

1. <span data-ttu-id="c49a2-125">Lägg sedan till följande i metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="c49a2-125">Then add the following to the **BasicOperations** method.</span></span>

    ```csharp
     User yanhe = new User
                {
                    Id = "1",
                    UserId = "yanhe",
                    LastName = "He",
                    FirstName = "Yan",
                    Email = "yanhe@contoso.com",
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
                    Email = "nelapin@contoso.com",
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

1. <span data-ttu-id="c49a2-126">Ange återigen följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="c49a2-126">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="c49a2-127">Du ser följande utdata i terminalen om att båda användarposterna har skapats.</span><span class="sxs-lookup"><span data-stu-id="c49a2-127">The terminal displays the following output, indicating that both user records were successfully created.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a><span data-ttu-id="c49a2-128">Läsa dokument</span><span class="sxs-lookup"><span data-stu-id="c49a2-128">Read documents</span></span>

1. <span data-ttu-id="c49a2-129">Om du vill läsa dokument från databasen kopierar du följande kod och placerar den i slutet av filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="c49a2-129">To read documents from the database, copy in the following code and place it at the end of the Program.cs file.</span></span>
    
    ```csharp
    private async Task ReadUserDocument(string databaseName, string collectionName, User user)
    {
            try
            {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            this.WriteToConsoleAndPromptToContinue("Read user {0}", user.Id);
            }
            catch (DocumentClientException de)
            {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not read", user.Id);
            }
            else
            {
                throw;
            }
            }
    }
    ```

1.  <span data-ttu-id="c49a2-130">Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**, efter raden `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="c49a2-130">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="c49a2-131">Spara filen Program.cs och kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="c49a2-131">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="c49a2-132">Du ser följande utdata i terminalen, och ”Read user 1” innebär att dokumentet hämtades.</span><span class="sxs-lookup"><span data-stu-id="c49a2-132">The terminal displays the following output, where the output "Read user 1" indicates the document was retrieved.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a><span data-ttu-id="c49a2-133">Ersätta dokument</span><span class="sxs-lookup"><span data-stu-id="c49a2-133">Replace documents</span></span>

<span data-ttu-id="c49a2-134">Azure Cosmos DB har stöd för ersättning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="c49a2-134">Azure Cosmos DB supports replacing JSON documents.</span></span> <span data-ttu-id="c49a2-135">I det här fallet ska vi uppdatera en användarpost eftersom efternamnet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="c49a2-135">In this case, we'll update a user record to account for a change to their last name.</span></span>

1. <span data-ttu-id="c49a2-136">Kopiera och klistra in metoden **ReplaceFamilyDocument** i slutet av filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="c49a2-136">Copy and paste the **ReplaceFamilyDocument** method at the end of the Program.cs file.</span></span>

    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, User updatedUser)
    {
            try
            {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, updatedUser.Id), updatedUser, new RequestOptions { PartitionKey = new PartitionKey(updatedUser.UserId) } );
            this.WriteToConsoleAndPromptToContinue("Replaced last name for {0}", updatedUser.LastName);
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

1. <span data-ttu-id="c49a2-137">Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**, efter raden `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="c49a2-137">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="c49a2-138">Spara filen Program.cs och kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="c49a2-138">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="c49a2-139">Du ser följande utdata i terminalen, och ”Replaced last name for Suh” innebär att dokumentet ersattes.</span><span class="sxs-lookup"><span data-stu-id="c49a2-139">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a><span data-ttu-id="c49a2-140">Ta bort dokument</span><span class="sxs-lookup"><span data-stu-id="c49a2-140">Delete documents</span></span>

1. <span data-ttu-id="c49a2-141">Kopiera och klistra in metoden **DeleteUserDocument** under metoden **ReplaceUserDocument**.</span><span class="sxs-lookup"><span data-stu-id="c49a2-141">Copy and paste the **DeleteUserDocument** method underneath your **ReplaceUserDocument** method.</span></span>
    
    ```csharp
    private async Task DeleteUserDocument(string databaseName, string collectionName, User deletedUser)
    {
        try
        {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, deletedUser.Id), new RequestOptions { PartitionKey = new PartitionKey(deletedUser.UserId) });
        Console.WriteLine("Deleted user {0}", deletedUser.Id);
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

1. <span data-ttu-id="c49a2-142">Kopiera och klistra in följande kod i metoden **BasicOperations** under den andra frågekörningen.</span><span class="sxs-lookup"><span data-stu-id="c49a2-142">Copy and paste the following code to your **BasicOperations** method underneath the second query execution.</span></span>

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="c49a2-143">Kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="c49a2-143">In the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```

    <span data-ttu-id="c49a2-144">Du ser följande utdata i terminalen, och ”Deleted user 1” innebär att dokumentet togs bort.</span><span class="sxs-lookup"><span data-stu-id="c49a2-144">The terminal displays the following output, where the output "Deleted user 1" indicates the document was deleted.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    Deleted user 1
    End of demo, press any key to exit.
    ```

## <a name="summary"></a><span data-ttu-id="c49a2-145">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c49a2-145">Summary</span></span>

<span data-ttu-id="c49a2-146">I den här utbildningsenheten har du skapat, ersatt och tagit bort dokument i din Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="c49a2-146">In this unit you created, replaced, and deleted documents in your Azure Cosmos DB database.</span></span>