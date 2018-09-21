<!--TODO: explain Etag in knowledge needed-->

<span data-ttu-id="ee700-101">När anslutningen till Azure Cosmos DB väl har upprättats är nästa steg att skapa, läsa, ersätta och ta bort dokument som lagras i databasen.</span><span class="sxs-lookup"><span data-stu-id="ee700-101">Once the connection to Azure Cosmos DB has been made, the next step is to create, read, replace, and delete the documents that are stored in the database.</span></span> <span data-ttu-id="ee700-102">I det här momentet skapar du användardokument i WebCustomer-samlingen.</span><span class="sxs-lookup"><span data-stu-id="ee700-102">In this unit, you will create User documents in your WebCustomer collection.</span></span> <span data-ttu-id="ee700-103">Sedan kan du hämta dem via ID, ersätta dem och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="ee700-103">Then, you'll retrieve them by ID, replace them, and delete them.</span></span>

## <a name="working-with-documents-programmatically"></a><span data-ttu-id="ee700-104">Arbeta programmatiskt med dokument</span><span class="sxs-lookup"><span data-stu-id="ee700-104">Working with documents programmatically</span></span>

<span data-ttu-id="ee700-105">Data lagras i JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ee700-105">Data is stored in JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="ee700-106">[Dokument](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) kan skapas, hämtas, ersättas och tas bort i portalen, som du såg i föregående modul. I den här modulen beskrivs hur du gör det programmässigt.</span><span class="sxs-lookup"><span data-stu-id="ee700-106">[Documents](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) can be created, retrieved, replaced, or deleted in the portal, as shown in the previous module, or programmatically, as described in this module.</span></span> <span data-ttu-id="ee700-107">Azure Cosmos DB har SDK:er på klientsidan för .NET, .NET Core, Java, Node.js och Python. De har alla stöd för de här åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="ee700-107">Azure Cosmos DB provides client-side SDKs for .NET, .NET Core, Java, Node.js, and Python, each of which supports these operations.</span></span> <span data-ttu-id="ee700-108">I den här modulen kommer vi att använda SDK:n för .NET Core till att skapa, hämta, uppdatera och ta bort NoSQL-data som lagras på Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ee700-108">In this module we'll be using the .NET Core SDK to perform CRUD (create, retrieve, update, and delete) operations on the NoSQL data stored in Azure Cosmos DB.</span></span>

<span data-ttu-id="ee700-109">De viktigaste åtgärderna för Azure Cosmos DB-dokument ingår i klassen [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet):</span><span class="sxs-lookup"><span data-stu-id="ee700-109">The main operations for Azure Cosmos DB documents are part of the [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) class:</span></span>
* [<span data-ttu-id="ee700-110">CreateDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="ee700-110">CreateDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="ee700-111">ReadDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="ee700-111">ReadDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="ee700-112">ReplaceDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="ee700-112">ReplaceDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* <span data-ttu-id="ee700-113">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="ee700-113">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span></span> <span data-ttu-id="ee700-114">Upsert skapar eller byter ut beroende på om dokumentet redan finns.</span><span class="sxs-lookup"><span data-stu-id="ee700-114">Upsert performs a create or replace operation depending on whether the document already exists.</span></span>
* [<span data-ttu-id="ee700-115">DeleteDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="ee700-115">DeleteDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

<span data-ttu-id="ee700-116">Om du vill utföra någon av de här åtgärderna måste du skapa en klass som representerar objektet som lagras i databasen.</span><span class="sxs-lookup"><span data-stu-id="ee700-116">To perform any of these operations, you need to create a class that represents the object stored in the database.</span></span> <span data-ttu-id="ee700-117">Eftersom vi arbetar med en databas med användare kan du skapa klassen **User** (Användare) för att lagra viktiga data som förnamn, efternamn och användar-ID (obligatoriskt eftersom det är partitionsnyckeln som möjliggör horisontell skalning) och underklasser för leveransinställningar och orderhistorik.</span><span class="sxs-lookup"><span data-stu-id="ee700-117">Because we're working with a database of users, you'll want to create a **User** class to store primary data such as their first name, last name, and user id (which is required, as that's the partition key to enable horizontal scaling) and subclasses for shipping preferences and order history.</span></span>

<span data-ttu-id="ee700-118">När du har skapat de här klasserna för dina användare skapar du nya användardokument för varje instans. Sedan ska vi utföra några enkla åtgärder på dokumenten.</span><span class="sxs-lookup"><span data-stu-id="ee700-118">Once you have those classes created to represent your users, you'll create new user documents for each instance, and then we'll perform some simple CRUD operations on the documents.</span></span>

## <a name="create-documents"></a><span data-ttu-id="ee700-119">Skapa dokument</span><span class="sxs-lookup"><span data-stu-id="ee700-119">Create documents</span></span>

1. <span data-ttu-id="ee700-120">Skapa först en **User**-klass som representerar objekten som ska lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ee700-120">First, create a **User** class that represents the objects to store in Azure Cosmos DB.</span></span> <span data-ttu-id="ee700-121">Vi skapar också underklasserna **OrderHistory** och **ShippingPreference** som används i klassen **User**.</span><span class="sxs-lookup"><span data-stu-id="ee700-121">We will also create **OrderHistory** and **ShippingPreference** subclasses that are used within **User**.</span></span> <span data-ttu-id="ee700-122">Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON.</span><span class="sxs-lookup"><span data-stu-id="ee700-122">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span>

    <span data-ttu-id="ee700-123">Du kan skapa dem genom att kopiera och klistra in koden för **användaren**, **OrderHistory** och **ShippingPreference** under metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="ee700-123">To create these classes, copy and paste the following **User**, **OrderHistory**, and **ShippingPreference** classes underneath the **BasicOperations** method.</span></span>

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

1. <span data-ttu-id="ee700-124">Ange följande kommando för att köra programmet i den integrerade terminalen och kontrollera att det körs.</span><span class="sxs-lookup"><span data-stu-id="ee700-124">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="ee700-125">Kopiera och klistra in uppgiften **CreateUserDocumentIfNotExists** under funktionen **WriteToConsoleAndPromptToContinue** i slutet av filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="ee700-125">Now copy and paste the **CreateUserDocumentIfNotExists** task under the **WriteToConsoleAndPromptToContinue** function at the end of the Program.cs file.</span></span>

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

1. <span data-ttu-id="ee700-126">Gå sedan tillbaka till metoden **BasicOperations** och lägg till följande i slutet av metoden.</span><span class="sxs-lookup"><span data-stu-id="ee700-126">Then, return to the **BasicOperations** method and add the following to the end of that method.</span></span>

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

1. <span data-ttu-id="ee700-127">Ange återigen följande kommando i den integrerade terminalen för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="ee700-127">In the integrated terminal, again, type the following command to run the program.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="ee700-128">Terminalen visar utdata efter hand som programmet skapar varje nytt användardokument.</span><span class="sxs-lookup"><span data-stu-id="ee700-128">The terminal will display output as the application creates each new user document.</span></span> <span data-ttu-id="ee700-129">Tryck på valfri tangent för att slutföra programmet.</span><span class="sxs-lookup"><span data-stu-id="ee700-129">Press any key to complete the program.</span></span>

    ```output
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a><span data-ttu-id="ee700-130">Läsa dokument</span><span class="sxs-lookup"><span data-stu-id="ee700-130">Read documents</span></span>

1. <span data-ttu-id="ee700-131">Om du vill läsa dokument från databasen kopierar du följande kod och placerar den efter metoden WriteToConsoleAndPromptToContinue i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="ee700-131">To read documents from the database, copy in the following code and place after the WriteToConsoleAndPromptToContinue method in the Program.cs file.</span></span>

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

1. <span data-ttu-id="ee700-132">Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**, efter raden `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="ee700-132">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="ee700-133">Ange följande kommando i den integrerade terminalen för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="ee700-133">In the integrated terminal, type the following command to run the program.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="ee700-134">Du ser följande utdata i terminalen, och ”Read user 1” innebär att dokumentet hämtades.</span><span class="sxs-lookup"><span data-stu-id="ee700-134">The terminal displays the following output, where the output "Read user 1" indicates the document was retrieved.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a><span data-ttu-id="ee700-135">Ersätta dokument</span><span class="sxs-lookup"><span data-stu-id="ee700-135">Replace documents</span></span>

<span data-ttu-id="ee700-136">Azure Cosmos DB har stöd för ersättning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ee700-136">Azure Cosmos DB supports replacing JSON documents.</span></span> <span data-ttu-id="ee700-137">I det här fallet ska vi uppdatera en användarpost eftersom efternamnet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="ee700-137">In this case, we'll update a user record to account for a change to their last name.</span></span>

1. <span data-ttu-id="ee700-138">Kopiera och klistra in metoden **ReplaceFamilyDocument** efter metoden ReadUserDocument i slutet av filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="ee700-138">Copy and paste the **ReplaceFamilyDocument** method after the ReadUserDocument method in the Program.cs file.</span></span>

    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, User updatedUser)
    {
        try
        {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, updatedUser.Id), updatedUser, new RequestOptions { PartitionKey = new PartitionKey(updatedUser.UserId) });
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

1. <span data-ttu-id="ee700-139">Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**, efter raden `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="ee700-139">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="ee700-140">Kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="ee700-140">In the integrated terminal, run the following command.</span></span>

    ```bash
    dotnet run
    ```
    <span data-ttu-id="ee700-141">Du ser följande utdata i terminalen, och ”Replaced last name for Suh” innebär att dokumentet ersattes.</span><span class="sxs-lookup"><span data-stu-id="ee700-141">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a><span data-ttu-id="ee700-142">Ta bort dokument</span><span class="sxs-lookup"><span data-stu-id="ee700-142">Delete documents</span></span>

1. <span data-ttu-id="ee700-143">Kopiera och klistra in metoden **DeleteUserDocument** under metoden **ReplaceUserDocument**.</span><span class="sxs-lookup"><span data-stu-id="ee700-143">Copy and paste the **DeleteUserDocument** method underneath your **ReplaceUserDocument** method.</span></span>

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

1. <span data-ttu-id="ee700-144">Kopiera och klistra in följande kod i slutet av metoden **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="ee700-144">Copy and paste the following code in the end of the **BasicOperations** method.</span></span>

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="ee700-145">Kör följande kommando i den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="ee700-145">In the integrated terminal, run the following command.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="ee700-146">Du ser följande utdata i terminalen, och ”Deleted user 1” innebär att dokumentet togs bort.</span><span class="sxs-lookup"><span data-stu-id="ee700-146">The terminal displays the following output, where the output "Deleted user 1" indicates the document was deleted.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    Deleted user 1
    End of demo, press any key to exit.
    ```

<span data-ttu-id="ee700-147">I den här utbildningsenheten har du skapat, ersatt och tagit bort dokument i din Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="ee700-147">In this unit you created, replaced, and deleted documents in your Azure Cosmos DB database.</span></span>
