<span data-ttu-id="3e227-101">Du använder en kö för att hantera toppar i efterfrågan på ditt program med bidrag av nyhetsartiklar.</span><span class="sxs-lookup"><span data-stu-id="3e227-101">You are using a queue to deal with peaks in demand for your news article submission application.</span></span> <span data-ttu-id="3e227-102">Du har redan skrivit den kod som skapar en kö och lägger till meddelanden i den.</span><span class="sxs-lookup"><span data-stu-id="3e227-102">You have already written the code that creates a queue and adds messages to it.</span></span>

<span data-ttu-id="3e227-103">Nu ska du slutföra programmet genom att skriva kod i den hämtande komponenten som läser nästa meddelande i kön, bearbetar det och sedan tar bort det från kön.</span><span class="sxs-lookup"><span data-stu-id="3e227-103">Now you want to complete the application by writing code in the retrieving component that reads the next message in the queue, processes it, and then deletes it from the queue.</span></span>

## <a name="dequeue-a-message"></a><span data-ttu-id="3e227-104">Ta bort ett meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="3e227-104">Dequeue a message</span></span>

<span data-ttu-id="3e227-105">Lägga till kod som tar bort nästa meddelande från kön genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="3e227-105">To add code that dequeues the next message from the queue, following these steps:</span></span>

1. <span data-ttu-id="3e227-106">I **Visual Studio Code** öppnar du filen **receivearticle/Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="3e227-106">In **Visual Studio Code**, open the **receivearticle/Program.cs** file.</span></span>

1. <span data-ttu-id="3e227-107">Leta upp metoden `RetrieveAnArticleAsync()` och ta bort platshållarinstruktionen `throw`.</span><span class="sxs-lookup"><span data-stu-id="3e227-107">Locate the `RetrieveAnArticleAsync()` method and remove the placeholder `throw` statement.</span></span> <span data-ttu-id="3e227-108">Allt arbete utförs i den här metoden.</span><span class="sxs-lookup"><span data-stu-id="3e227-108">All your work will be done inside this method.</span></span>

1. <span data-ttu-id="3e227-109">Använd den statiska metoden `CloudStorageAccount.Parse` så får anslutningssträngen en lagringskontoinstans.</span><span class="sxs-lookup"><span data-stu-id="3e227-109">Use the `CloudStorageAccount.Parse` static method and your connection string to get a storage account instance.</span></span>

1. <span data-ttu-id="3e227-110">Anropa metoden `CreateCloudQueueClient` på lagringskontot för att hämta ett klientobjekt.</span><span class="sxs-lookup"><span data-stu-id="3e227-110">Call the `CreateCloudQueueClient` method on the storage account to get a client object.</span></span>

1. <span data-ttu-id="3e227-111">Anropa metoden `GetQueueReference` på klientobjektet och skicka **newsqueue** för könamnet.</span><span class="sxs-lookup"><span data-stu-id="3e227-111">Call `GetQueueReference` method on the client object and pass in **newsqueue** for the queue name.</span></span> <span data-ttu-id="3e227-112">Då får du ett `CloudQueue`-objekt.</span><span class="sxs-lookup"><span data-stu-id="3e227-112">This gets you a `CloudQueue` object.</span></span>

1. <span data-ttu-id="3e227-113">Anropa `GetMessageAsync` på objektet `CloudQueue` för att hämta nästa `CloudQueueMessage` från kön.</span><span class="sxs-lookup"><span data-stu-id="3e227-113">Call `GetMessageAsync` on the `CloudQueue` object to get the next `CloudQueueMessage` from the queue.</span></span> <span data-ttu-id="3e227-114">Det returnerade värdet blir `null` om kön är tom.</span><span class="sxs-lookup"><span data-stu-id="3e227-114">The return value will be `null` if the queue is empty.</span></span>

1. <span data-ttu-id="3e227-115">Använd egenskapen `AsString` på `CloudQueueMessage`-objektet för att hämta innehållet i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="3e227-115">Use the `AsString` property on the `CloudQueueMessage` object to get the contents of the message.</span></span> <span data-ttu-id="3e227-116">Skriv ut strängen till konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="3e227-116">Print the string to the console window.</span></span>

1. <span data-ttu-id="3e227-117">Anropa `DeleteMessageAsync` på `CloudQueue`-objektet för att ta bort meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="3e227-117">Call `DeleteMessageAsync` on the `CloudQueue` object to delete the message from the queue.</span></span>

## <a name="execute-the-application"></a><span data-ttu-id="3e227-118">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="3e227-118">Execute the application</span></span>

<span data-ttu-id="3e227-119">Programmet kan nu skicka och hämta meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3e227-119">The application can now send and retrieve messages.</span></span> <span data-ttu-id="3e227-120">Testa det genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="3e227-120">To test it, follow these steps:</span></span>

1. <span data-ttu-id="3e227-121">I **Visual Studio Code** går du till menyn **Visa** och klickar på **Felsöka**.</span><span class="sxs-lookup"><span data-stu-id="3e227-121">In **Visual Studio Code**, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="3e227-122">Längst upp på fönsterrutan **FELSÖKA** går du till listrutan **FELSÖKA** och väljer **.NET Core Launch Receive Article** (.NET Core, starta mottagande av artikel).</span><span class="sxs-lookup"><span data-stu-id="3e227-122">At the top of the **DEBUG** pane, in the **DEBUG** drop-down list, select **.NET Core Launch Receive Article**.</span></span>

1. <span data-ttu-id="3e227-123">På menyn **Felsöka** klickar du på **Starta felsökning**.</span><span class="sxs-lookup"><span data-stu-id="3e227-123">On the **Debug** menu, click **Start Debugging**.</span></span> <span data-ttu-id="3e227-124">**Visual Studio Code** bygger och kör programmet.</span><span class="sxs-lookup"><span data-stu-id="3e227-124">**Visual Studio Code** builds and executes the application.</span></span>

1. <span data-ttu-id="3e227-125">Växla till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3e227-125">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="3e227-126">I det vänstra navigeringsfönstret klickar du på **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="3e227-126">In the navigation on the left, click **All Resources**.</span></span>

1. <span data-ttu-id="3e227-127">I listan över resurser klickar du på det lagringskonto som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="3e227-127">In the list of resources, click the storage account you created earlier.</span></span>

1. <span data-ttu-id="3e227-128">Klicka på **Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="3e227-128">Click **Storage Explorer**.</span></span>

1. <span data-ttu-id="3e227-129">Expandera listan över **KÖER** och klicka sedan på **newsqueue**.</span><span class="sxs-lookup"><span data-stu-id="3e227-129">Expand the list of **QUEUES** and then click **newsqueue**.</span></span> <span data-ttu-id="3e227-130">Kön är tom eftersom ditt program har tagit bort meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="3e227-130">The queue is empty because your application has dequeued the message.</span></span>

## <a name="summary"></a><span data-ttu-id="3e227-131">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3e227-131">Summary</span></span>

<span data-ttu-id="3e227-132">Din kod nu skapar en kö, lägger till meddelanden i den, läser meddelandet längst fram i kön, bearbetar det och tar slutligen bort det bearbetade meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="3e227-132">Your code now creates a queue, adds messages to it, reads the message at the front of the queue, processes it, and finally deletes the processed message from the queue.</span></span> <span data-ttu-id="3e227-133">Koden för lagringskön är nu klar.</span><span class="sxs-lookup"><span data-stu-id="3e227-133">Your storage queue code is now complete.</span></span>