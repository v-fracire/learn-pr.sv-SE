<!--TODO: explain Etag in knowledge needed-->
<!--TODO: Update to weave in the online retailer story-->


<span data-ttu-id="ff8b3-101">Data lagras i JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-101">Data is stored in JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="ff8b3-102">[Dokument](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) kan skapas, hämtas, ersättas och tas bort i portalen, som du såg i föregående modul. I den här modulen beskrivs hur du gör det programmässigt.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-102">[Documents](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) can be created, retrieved, replaced, or deleted in the portal, as shown in the previous module, or programmatically, as described in this module.</span></span> <span data-ttu-id="ff8b3-103">Azure Cosmos DB har SDK:er på klientsidan för .NET, .NET Core, Java, Node.js och Python. De har alla stöd för de här åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-103">Azure Cosmos DB provides client-side SDKs for .NET, .NET Core, Java, Node.js, and Python, each of which supports these operations.</span></span> <span data-ttu-id="ff8b3-104">I den här modulen kommer vi att använda SDK:n för .NET Core till att skapa, hämta, uppdatera och ta bort objekt.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-104">In this module we'll be using the .NET Core SDK to perform CRUD (create, retrieve, update, and delete) operations.</span></span> 

<span data-ttu-id="ff8b3-105">De viktigaste åtgärderna för Azure Cosmos DB-dokument ingår i klassen [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet):</span><span class="sxs-lookup"><span data-stu-id="ff8b3-105">The main operations for Azure Cosmos DB documents are part of the [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) class:</span></span>
* [<span data-ttu-id="ff8b3-106">CreateDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="ff8b3-106">CreateDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="ff8b3-107">ReadDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="ff8b3-107">ReadDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="ff8b3-108">ReplaceDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="ff8b3-108">ReplaceDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* <span data-ttu-id="ff8b3-109">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="ff8b3-109">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span></span> <span data-ttu-id="ff8b3-110">Upsert skapar eller byter ut beroende på om dokumentet redan finns.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-110">Upsert performs a create or replace operation depending on whether the document already exists.</span></span>
* [<span data-ttu-id="ff8b3-111">DeleteDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="ff8b3-111">DeleteDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

<span data-ttu-id="ff8b3-112">Om du vill utföra någon av de här åtgärderna måste du skapa en klass som representerar objektet som lagras i databasen.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-112">To perform any of these operations, you need to create a class that represents the object stored in the database.</span></span> <span data-ttu-id="ff8b3-113">Eftersom vi arbetar med en databas med användare kan du skapa klassen **User** för att lagra viktiga data som namn och användar-ID (som krävs eftersom det är partitionsnyckeln som möjliggör horisontell skalning) och underklasser för leveransinställningar och orderhistorik.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-113">Because we're working with a database of users, you'll want to create a **User** class to store primary data such as their name and UserId (which is required, as that's the partition key to enable horizontal scaling) and subclasses for shipping preferences and order history.</span></span>

<span data-ttu-id="ff8b3-114">När du har skapat de här klasserna för dina användare skapar du nya användardokument för varje instans. Sedan ska vi utföra några enkla åtgärder på dokumenten.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-114">Once you have those classes created to represent your users, you'll create new user documents for each instance, and then we'll perform some simple CRUD operations on the documents.</span></span>

## <a name="create-documents"></a><span data-ttu-id="ff8b3-115">Skapa dokument</span><span class="sxs-lookup"><span data-stu-id="ff8b3-115">Create documents</span></span>

1. <span data-ttu-id="ff8b3-116">Skapa först en **User**-klass som representerar objekten som ska lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-116">First, create a **User** class that represents the objects to store in Azure Cosmos DB.</span></span> <span data-ttu-id="ff8b3-117">Vi skapar också underklasserna **OrderHistory** och **ShippingPreference** som används i klassen **User**.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-117">We will also create **OrderHistory** and **ShippingPreference** subclasses that are used within **User**.</span></span> <span data-ttu-id="ff8b3-118">Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-118">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> 

    <span data-ttu-id="ff8b3-119">Du kan skapa dem genom att kopiera och klistra in koden för **användaren**, **OrderHistory** och **ShippingPreference** under metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-119">To create these classes, copy and paste the following **User**, **OrderHistory**, **ShippingPreference** classes underneath the **BasicOperations** method.</span></span>

    <!--TODO: specify JSON for all of these? -->
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
            public OrderHistory[] OrderHistory { get; set; }
            public ShippingPreference[] ShippingPreference { get; set; }
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

2. <span data-ttu-id="ff8b3-120">Ange följande kommando för att köra programmet i den integrerade terminalen och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-120">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

3. <span data-ttu-id="ff8b3-121">Kopiera nu och klistra in uppgiften **CreateUserDocumentIfNotExists** under klassen **ShippingPreference**.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-121">Now copy and paste the **CreateUserDocumentIfNotExists** task under the **ShippingPreference** class.</span></span>

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

4. <span data-ttu-id="ff8b3-122">Lägg sedan till följande i metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-122">Then add the following to the **BasicOperations** method.</span></span>

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

5. <span data-ttu-id="ff8b3-123">Ange återigen följande kommando i den integrerade terminalen för att köra programmet och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-123">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="ff8b3-124">Du ser följande utdata i terminalen om att båda användarposterna har skapats.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-124">The terminal displays the following output, indicating that both user records were successfully created.</span></span> 

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a><span data-ttu-id="ff8b3-125">Läsa dokument</span><span class="sxs-lookup"><span data-stu-id="ff8b3-125">Read documents</span></span>

1. <span data-ttu-id="ff8b3-126">Om du vill läsa dokument från databasen kopierar du följande kod och placerar den i slutet av filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-126">To read documents from the database, copy in the following code and place it at the end of the Program.cs file.</span></span>

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
                this.WriteToConsoleAndPromptToContinue("User {0} not found", user.Id);
            }
            else
            {
                throw;
            }
            }
    }
    ```

2.  <span data-ttu-id="ff8b3-127">Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**, efter raden `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-127">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

4. <span data-ttu-id="ff8b3-128">Spara filen Program.cs och kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-128">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="ff8b3-129">Du ser följande utdata i terminalen, och ”Found user 1” innebär att dokumentet hittades.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-129">The terminal displays the following output, where the output "Found user 1" indicates the document was found.</span></span>

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

## <a name="replace-documents"></a><span data-ttu-id="ff8b3-130">Ersätta dokument</span><span class="sxs-lookup"><span data-stu-id="ff8b3-130">Replace documents</span></span>
<span data-ttu-id="ff8b3-131">Azure Cosmos DB har stöd för ersättning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-131">Azure Cosmos DB supports replacing JSON documents.</span></span> <span data-ttu-id="ff8b3-132">I det här fallet ska vi uppdatera en användarpost eftersom efternamnet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-132">In this case, we'll update a user record to account for a change to their last name.</span></span>

1. <span data-ttu-id="ff8b3-133">Kopiera och klistra in metoden **ReplaceFamilyDocument** under metoden **CreateUserDocumentIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-133">Copy and paste the **ReplaceFamilyDocument** method underneath your **CreateUserDocumentIfNotExists** method.</span></span>

    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, string LastName, User updatedUser)
    {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, LastName), updatedUser);
        this.WriteToConsoleAndPromptToContinue("Replaced last name for {0}", LastName);
    }
    ```

2. <span data-ttu-id="ff8b3-134">Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**, efter raden `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-134">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe.LastName, yanhe);
    ```
    <!--TODO: need to fix this as it's giving an error-->

4. <span data-ttu-id="ff8b3-135">Spara filen Program.cs och kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-135">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="ff8b3-136">Du ser följande utdata i terminalen, och ”Replaced last name for Suh” innebär att dokumentet ersattes.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-136">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

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

## <a name="delete-documents"></a><span data-ttu-id="ff8b3-137">Ta bort dokument</span><span class="sxs-lookup"><span data-stu-id="ff8b3-137">Delete documents</span></span>

1. <span data-ttu-id="ff8b3-138">Kopiera och klistra in metoden **DeleteUserDocument** under metoden **ReplaceUserDocument**.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-138">Copy and paste the **DeleteUserDocument** method underneath your **ReplaceUserDocument** method.</span></span>
    
    ```csharp
    private async Task DeleteUserDocument(string databaseName, string collectionName, string documentName)
    {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName), new RequestOptions { PartitionKey = new PartitionKey("User.UserId") });
        Console.WriteLine("Deleted User {0}", documentName);
    }
    ```

2. <span data-ttu-id="ff8b3-139">Kopiera och klistra in följande kod i metoden **BasicOperations** under den andra frågekörningen.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-139">Copy and paste the following code to your **BasicOperations** method underneath the second query execution.</span></span>

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", "1");
    ```

3. <span data-ttu-id="ff8b3-140">Spara filen Program.cs och kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-140">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```

    <span data-ttu-id="ff8b3-141">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ff8b3-141">Congratulations!</span></span> <span data-ttu-id="ff8b3-142">Du har tagit bort ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-142">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <a name="summary"></a><span data-ttu-id="ff8b3-143">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ff8b3-143">Summary</span></span>

<span data-ttu-id="ff8b3-144">I den här utbildningsenheten har du skapat, ersatt och tagit bort dokument i din Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="ff8b3-144">In this unit you created, replaced, and deleted documents in your Azure Cosmos DB database.</span></span>