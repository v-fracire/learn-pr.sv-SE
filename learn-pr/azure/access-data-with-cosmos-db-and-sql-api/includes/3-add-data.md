<span data-ttu-id="40d43-101">Att lägga till data i din Azure Cosmos DB-databas ska vara enkelt.</span><span class="sxs-lookup"><span data-stu-id="40d43-101">Adding data to your Azure Cosmos DB database should be simple.</span></span> <span data-ttu-id="40d43-102">Öppna Azure Portal, navigera till databasen och använd Datautforskaren till att lägga till JSON-dokument i databasen.</span><span class="sxs-lookup"><span data-stu-id="40d43-102">You open the Azure portal, navigate to your database, and use the Data Explorer to add JSON documents to the database.</span></span> <span data-ttu-id="40d43-103">Det finns mer avancerade sätt att lägga till data, men vi börjar här eftersom Datautforskaren är ett bra verktyg för att bekanta sig med de egenskaper och funktioner som tillhandahålls av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="40d43-103">There are more advanced ways to add data, but we'll start here because the Data Explorer is a great tool to get you acquainted with the inner workings and functionality provided by Azure Cosmos DB.</span></span>

## <a name="what-is-the-data-explorer"></a><span data-ttu-id="40d43-104">Vad är Datautforskaren?</span><span class="sxs-lookup"><span data-stu-id="40d43-104">What is the Data Explorer?</span></span>
<span data-ttu-id="40d43-105">Datautforskaren i Azure Cosmos DB är ett verktyg som ingår i Azure Portal som används för att hantera data som lagras i en Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="40d43-105">The Azure Cosmos DB Data Explorer is a tool included in the Azure portal that is used to manage data stored in an Azure Cosmos DB.</span></span> <span data-ttu-id="40d43-106">Det har ett användargränssnitt för visning och navigering av datasamlingar samt för redigering av dokument i databasen.</span><span class="sxs-lookup"><span data-stu-id="40d43-106">It provides a UI for viewing and navigating data collections, as well as for editing documents within the database.</span></span>

## <a name="add-data-using-the-data-explorer"></a><span data-ttu-id="40d43-107">Lägga till data med Datautforskaren</span><span class="sxs-lookup"><span data-stu-id="40d43-107">Add data using the Data Explorer</span></span>

1. <span data-ttu-id="40d43-108">Om du fortsätter från föregående modul klicka du på **Datautforskaren** I Azure Portal-fönstret och klicka sedan på **Öppna i helskärmsläge**.</span><span class="sxs-lookup"><span data-stu-id="40d43-108">If you're continuing from the previous module, click **Data Explorer** in the Azure portal window, and then click **Open Full Screen**.</span></span>

    <span data-ttu-id="40d43-109">Logga annars in på [Azure Portal](https://portal.azure.com/?azure-portal=true), klicka på **Alla tjänster** > **Databaser** > **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="40d43-109">Otherwise, sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true), click **All services** > **Databases** > **Azure Cosmos DB**.</span></span> <span data-ttu-id="40d43-110">Välj sedan ditt konto, klicka på **Datautforskaren** och klicka sedan på **Öppna i helskärmsläge**</span><span class="sxs-lookup"><span data-stu-id="40d43-110">Then select your account, click **Data Explorer**, and then click **Open Full Screen**</span></span>
 
   ![Skapa nya dokument i Datautforskaren i Azure Portal](../media-draft/3-azure-cosmosdb-data-explorer-full-screen.png)

2. <span data-ttu-id="40d43-112">I rutan **Öppna i helskärmsläge** klickar du på **Öppna**.</span><span class="sxs-lookup"><span data-stu-id="40d43-112">In the **Open Full Screen** box, click **Open**.</span></span>

    <span data-ttu-id="40d43-113">Webbläsaren visar den nya Datautforskaren i helskärm, vilket ger dig mer utrymme och en dedikerad miljö för att arbeta med databasen.</span><span class="sxs-lookup"><span data-stu-id="40d43-113">The web browser displays the new full-screen Data Explorer, which gives you more space and a dedicated environment for working with your database.</span></span>

3. <span data-ttu-id="40d43-114">Om du vill skapa ett nytt JSON-dokument klickar du på **Nytt dokument**.</span><span class="sxs-lookup"><span data-stu-id="40d43-114">To create a new JSON document, click **New Document**.</span></span>

   ![Skapa nya dokument i Datautforskaren i Azure Portal](../media-draft/3-azure-cosmosdb-data-explorer-new-document.png)

4. <span data-ttu-id="40d43-116">Lägg nu till ett dokument i samlingen med följande struktur.</span><span class="sxs-lookup"><span data-stu-id="40d43-116">Now, add a document to the collection with the following structure.</span></span> <span data-ttu-id="40d43-117">Kopiera och klistra in följande kod på fliken **Dokument**:</span><span class="sxs-lookup"><span data-stu-id="40d43-117">Just copy and paste the following code into the **Documents** tab:</span></span>

     ```
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
        }
     ```

5. <span data-ttu-id="40d43-118">När du har lagt till JSON på fliken **Dokument** klickar du på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="40d43-118">Once you've added the JSON to the **Documents** tab, click **Save**.</span></span>

    ![Kopiera in JSON-data och klicka på Spara i Datautforskaren i Azure Portal](../media-draft/3-azure-cosmosdb-data-explorer-save-document.png)

6. <span data-ttu-id="40d43-120">Skapa och spara ett dokument till genom att kopiera följande JSON-objekt i Datautforskaren och klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="40d43-120">Create and save one more document by copying the following JSON object into Data Explorer and clicking **Save**.</span></span>

     ```
    {
        "id": "2",
        "productId": "33218897",
        "category": "Women's Outerwear",
        "manufacturer": "Contoso",
        "description": "Black wool pea-coat",
        "shipping": {
            "weight": 2,
            "dimensions": {
            "width": 8,
            "height": 11,
            "depth": 3
           }
        }
    }
     ```

7. <span data-ttu-id="40d43-121">Bekräfta att dokumenten har sparats genom att klicka på **Dokument** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="40d43-121">Confirm the documents have been saved by clicking **Documents** on the left-hand menu.</span></span> 

    <span data-ttu-id="40d43-122">Datautforskaren de två dokumenten på fliken **Dokument**.</span><span class="sxs-lookup"><span data-stu-id="40d43-122">Data Explorer displays the two documents in the **Documents** tab.</span></span>

## <a name="summary"></a><span data-ttu-id="40d43-123">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="40d43-123">Summary</span></span>

<span data-ttu-id="40d43-124">I den här modulen har du lagt till två dokument, som var och en representerar en produkt i din produktkatalog, i databasen med hjälp av Datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="40d43-124">In this module, you added two documents, each representing a product in your product catalog, to your database by using the Data Explorer.</span></span> <span data-ttu-id="40d43-125">Datautforskaren är ett bra sätt att skapa dokument, ändra dokument och komma igång med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="40d43-125">The Data Explorer is a good way to create documents, modify documents, and get started with Azure Cosmos DB.</span></span>  
