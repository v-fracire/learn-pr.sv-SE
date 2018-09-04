<span data-ttu-id="b2ef8-101">Du har valt att använda en kö för att hantera toppar i efterfrågan på ditt program med bidrag av nyhetsartiklar.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-101">You have decided to use a queue to deal with peaks in demand for your news article submission application.</span></span>

<span data-ttu-id="b2ef8-102">I den här enheten skriver du C#-kod som skapar en ny kö i ditt Azure-lagringskonto och lägger till ett meddelande i kön.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-102">In this unit, you will write C# code that creates a new queue in your Azure Storage account and adds a message to the queue.</span></span> <span data-ttu-id="b2ef8-103">Den här enheten är en fortsättning på den föregående övningen.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-103">This unit is a continuation of the previous exercise.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="b2ef8-104">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="b2ef8-104">Create a queue</span></span>

<span data-ttu-id="b2ef8-105">Nu när alla krav är uppfyllda kan du skriva kod som skapar en ny lagringskö och lägger till ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-105">Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message.</span></span>

1. <span data-ttu-id="b2ef8-106">I **Visual Studio Code** öppnar du filen **sendarticle/Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-106">In **Visual Studio Code**, open the **sendarticle/Program.cs** file.</span></span>

1. <span data-ttu-id="b2ef8-107">Leta upp metoden `SendAnArticleAsync()` och ta bort platshållarinstruktionen `throw`.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-107">Locate the `SendAnArticleAsync()` method and remove the placeholder `throw` statement.</span></span> <span data-ttu-id="b2ef8-108">Allt arbete utförs i den här metoden.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-108">All your work will be done inside this method.</span></span>

1. <span data-ttu-id="b2ef8-109">Använd den statiska metoden `CloudStorageAccount.Parse` så får anslutningssträngen en lagringskontoinstans.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-109">Use the `CloudStorageAccount.Parse` static method and your connection string to get a storage account instance.</span></span>

1. <span data-ttu-id="b2ef8-110">Anropa metoden `CreateCloudQueueClient` på lagringskontot för att hämta ett klientobjekt.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-110">Call the `CreateCloudQueueClient` method on the storage account to get a client object.</span></span>

1. <span data-ttu-id="b2ef8-111">Anropa metoden `GetQueueReference` på klientobjektet och skicka **newsqueue** för könamnet.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-111">Call `GetQueueReference` method on the client object and pass in **newsqueue** fpr the queue name.</span></span> <span data-ttu-id="b2ef8-112">Då får du ett `CloudQueue`-objekt (det är OK om kön inte finns ännu; det är nästa steg).</span><span class="sxs-lookup"><span data-stu-id="b2ef8-112">This gets you a `CloudQueue` object (it is OK if the queue does not exist yet, that's the next step).</span></span>

1. <span data-ttu-id="b2ef8-113">Anropa `CreateIfNotExistsAsync` på `CloudQueue`-objektet för att se till att kön är redo att användas.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-113">Call `CreateIfNotExistsAsync` on the `CloudQueue` object to ensure the queue is ready for use.</span></span>

1. <span data-ttu-id="b2ef8-114">Använd `new` för att skapa en instans av `CloudQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-114">Use `new` to create an instance of `CloudQueueMessage`.</span></span> <span data-ttu-id="b2ef8-115">Skicka en meddelandesträng till konstruktorn.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-115">Pass a message string to the constructor.</span></span>

1. <span data-ttu-id="b2ef8-116">Anropa `AddMessageAsync` på `CloudQueue`-objektet för att lägga till meddelandet i kön.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-116">Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue.</span></span>

## <a name="execute-the-application"></a><span data-ttu-id="b2ef8-117">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="b2ef8-117">Execute the application</span></span>

<span data-ttu-id="b2ef8-118">Programmet kan nu skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-118">The application can now send messages.</span></span> <span data-ttu-id="b2ef8-119">Testa det genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="b2ef8-119">To test it, follow these steps:</span></span>

1. <span data-ttu-id="b2ef8-120">I **Visual Studio Code** går du till menyn **Visa** och klickar på **Felsöka**.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-120">In **Visual Studio Code**, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="b2ef8-121">Längst upp på fönsterrutan **FELSÖKA** går du till listrutan **FELSÖKA** och väljer **.NET Core Launch Send Article** (.NET Core, starta skickande av artikel).</span><span class="sxs-lookup"><span data-stu-id="b2ef8-121">At the top of the **DEBUG** pane, in the **DEBUG** drop-down list, select **.NET Core Launch Send Article**.</span></span>

1. <span data-ttu-id="b2ef8-122">På menyn **Felsöka** klickar du på **Starta felsökning**.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-122">On the **Debug** menu, click **Start Debugging**.</span></span> <span data-ttu-id="b2ef8-123">**Visual Studio Code** bygger och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-123">**Visual Studio Code** builds and executes the application.</span></span>

1. <span data-ttu-id="b2ef8-124">Växla till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-124">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="b2ef8-125">I det vänstra navigeringsfönstret klickar du på **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-125">In the navigation on the left, click **All Resources**.</span></span>

1. <span data-ttu-id="b2ef8-126">I listan över resurser klickar du på det lagringskonto som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-126">In the list of resources, click the storage account you created earlier.</span></span>

1. <span data-ttu-id="b2ef8-127">Klicka på **Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-127">Click **Storage Explorer**.</span></span>

1. <span data-ttu-id="b2ef8-128">Expandera listan över **KÖER** och klicka sedan på **newsqueue**.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-128">Expand the list of **QUEUES** and then click **newsqueue**.</span></span> <span data-ttu-id="b2ef8-129">Portalen visar det meddelande som lades till av ditt program.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-129">The portal displays the message added by your application.</span></span>

## <a name="summary"></a><span data-ttu-id="b2ef8-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b2ef8-130">Summary</span></span>

<span data-ttu-id="b2ef8-131">Den kod som skapar en meddelandekö och lägger till meddelanden i den är nu klar.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-131">The code that creates a message queue and adds messages to it is now complete.</span></span> <span data-ttu-id="b2ef8-132">Därefter behöver du skriva kod som hämtar meddelanden från kön.</span><span class="sxs-lookup"><span data-stu-id="b2ef8-132">Next, you need to write code that retrieves messages from the queue.</span></span>
