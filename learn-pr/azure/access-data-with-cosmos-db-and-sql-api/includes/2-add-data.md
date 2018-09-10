<span data-ttu-id="0651d-101">Att lägga till data i din Azure Cosmos DB-databas ska vara enkelt.</span><span class="sxs-lookup"><span data-stu-id="0651d-101">Adding data to your Azure Cosmos DB database should be simple.</span></span> <span data-ttu-id="0651d-102">Öppna Azure Portal, navigera till databasen och använd **Datautforskaren** till att lägga till json-dokument i databasen.</span><span class="sxs-lookup"><span data-stu-id="0651d-102">You open the Azure portal, navigate to your database, and use the **Data Explorer** to add json documents to the database.</span></span> <span data-ttu-id="0651d-103">Det finns mer avancerade sätt att lägga till data, men vi börjar här eftersom Datautforskaren är ett bra verktyg för att bekanta sig med de egenskaper och funktioner som tillhandahålls av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0651d-103">There are more advanced ways to add data, but we'll start here as the Data Explorer is a great tool to get you aquainted with the inner workings and functionality provided by Azure Cosmos DB.</span></span>

## <a name="what-is-the-data-explorer"></a><span data-ttu-id="0651d-104">Vad är Datautforskaren?</span><span class="sxs-lookup"><span data-stu-id="0651d-104">What is the Data Explorer?</span></span>
<span data-ttu-id="0651d-105">Datautforskaren i Azure Cosmos DB är en verktyg som ingår i Azure Portal som används för att hantera data som lagras i en Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0651d-105">The Azure Cosmos DB Data Explorer is a tool built included in the Azure Portal that is used to manage data stored in an Azure Cosmos DB.</span></span> <span data-ttu-id="0651d-106">Den har ett användargränssnitt för att visa och navigera bland datasamlingar samt redigera dokument i databasen.</span><span class="sxs-lookup"><span data-stu-id="0651d-106">It provides UI to view and navigate data collections as well as edit documents within the db.</span></span>

## <a name="add-data-using-the-data-explorer"></a><span data-ttu-id="0651d-107">Lägga till data med Datautforskaren</span><span class="sxs-lookup"><span data-stu-id="0651d-107">Add data using the Data Explorer</span></span>

1. <span data-ttu-id="0651d-108">Om du fortsätter från föregående modul klicka du på **Datautforskaren** I Azure Portal-fönstret och klicka sedan på **Öppna i helskärmsläge**.</span><span class="sxs-lookup"><span data-stu-id="0651d-108">If you're continuing from the previous module, click **Data Explorer** in the Azure portal window, and then click **Open Full Screen**.</span></span>

    <span data-ttu-id="0651d-109">Logga annars in på [Azure Portal](https://portal.azure.com/), klicka på **Alla tjänster** > **Databaser** > **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="0651d-109">Otherwise, sign in to the [Azure portal](https://portal.azure.com/), click **All services** > **Databases** > **Azure Cosmos DB**.</span></span> <span data-ttu-id="0651d-110">Välj sedan ditt konto, klicka på **Datautforskaren** och klicka sedan på **Öppna i helskärmsläge**</span><span class="sxs-lookup"><span data-stu-id="0651d-110">Then select your account, click **Data Explorer**, and then click **Open Full Screen**</span></span>
 
   ![Skapa nya dokument i Datautforskaren i Azure Portal](../media-draft/2-add-data/azure-cosmosdb-data-explorer-full-screen.png)

2. <span data-ttu-id="0651d-112">I rutan **Öppna i helskärmsläge** klickar du på **Öppna**.</span><span class="sxs-lookup"><span data-stu-id="0651d-112">In the **Open Full Screen** box, click **Open**.</span></span>

    <span data-ttu-id="0651d-113">Webbläsaren visar den nya Datautforskaren i helskärm, vilket ger dig mer utrymme och en dedikerad miljö för att arbeta med databasen.</span><span class="sxs-lookup"><span data-stu-id="0651d-113">The web browser displays the new full screen Data Explorer, which gives you more space and a dedicated environment for working with your database.</span></span>

3. <span data-ttu-id="0651d-114">Om du vill skapa ett nytt json-dokument klickar du på **Nytt dokument**.</span><span class="sxs-lookup"><span data-stu-id="0651d-114">To create a new json document, click **New Document**.</span></span>

   ![Skapa nya dokument i Datautforskaren i Azure Portal](../media-draft/2-add-data/azure-cosmosdb-data-explorer-new-document.png)

4. <span data-ttu-id="0651d-116">Lägg nu till ett dokument i samlingen med följande struktur, kopiera bara och klistra in följande kod på fliken **Dokument**.</span><span class="sxs-lookup"><span data-stu-id="0651d-116">Now add a document to the collection with the following structure, just copy and paste the following code into the **Documents** tab.</span></span>

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

5. <span data-ttu-id="0651d-117">När du har lagt till json på fliken **Dokument** klickar du på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0651d-117">Once you've added the json to the **Documents** tab, click **Save**.</span></span>

    ![Kopiera in json-data och klicka på Spara i Datautforskaren i Azure Portal](../media-draft/2-add-data/azure-cosmosdb-data-explorer-save-document.png)

6. <span data-ttu-id="0651d-119">Skapa och spara ett dokument till genom att kopiera följande json-objekt i Datautforskaren och klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0651d-119">Create and save one more document by copying the following json object into Data Explorer and clicking **Save**.</span></span>

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

7. <span data-ttu-id="0651d-120">Bekräfta att dokumenten har sparats genom att klicka på **Dokument** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="0651d-120">Confirm the documents have been saved by clicking **Documents** on the left-hand menu.</span></span> 

    <span data-ttu-id="0651d-121">Datautforskaren de två dokumenten på fliken **Dokument**.</span><span class="sxs-lookup"><span data-stu-id="0651d-121">Data Explorer displays the two documents in the **Documents** tab.</span></span>

## <a name="summary"></a><span data-ttu-id="0651d-122">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0651d-122">Summary</span></span>

<span data-ttu-id="0651d-123">I den här modulen har du lagt till två dokument, som var och en representerar en produkt i din produktkatalog, i databasen med hjälp av Datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="0651d-123">In this module you added two documents, each representing a product in your product catalog, to your database by using the Data Explorer.</span></span> <span data-ttu-id="0651d-124">Datautforskaren är ett bra sätt att skapa dokument, ändra dokument och komma igång med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0651d-124">The Data Explorer is a good way to create documents, modify documents, and get started with Azure Cosmos DB.</span></span>  
