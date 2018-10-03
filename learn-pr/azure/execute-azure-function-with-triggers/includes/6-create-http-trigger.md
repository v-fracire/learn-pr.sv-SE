<span data-ttu-id="5f4e7-101">I den här övningen ska vi skapa en Azure-funktion som tar emot en HTTP-begäran med en enda sträng.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-101">In this unit, we're going to create an Azure function that accepts an HTTP request with a single string.</span></span> <span data-ttu-id="5f4e7-102">Funktionen returnerar en sträng till anroparen som anger om åtgärden lyckades eller misslyckades.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-102">The function returns a string back to the caller to represent success or failure.</span></span> <span data-ttu-id="5f4e7-103">Vi fortsätter arbeta med funktionen från den föregående övningen.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-103">We'll continue working on the function from the previous exercise.</span></span>

## <a name="create-an-http-trigger"></a><span data-ttu-id="5f4e7-104">Skapa en HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="5f4e7-104">Create an HTTP trigger</span></span>

<span data-ttu-id="5f4e7-105">Vi ska fortsätta att använda vår befintliga Azure Functions-app och lägga till en HTTP-utlösare.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-105">Let's continue using our existing Azure Functions application and add an HTTP trigger.</span></span>

1. <span data-ttu-id="5f4e7-106">Kontrollera att du är inloggad på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-106">Make sure you are signed into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="5f4e7-107">Navigera till skärmen **Alla resurser** och välj din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-107">Navigate to the **All resources** screen and select your function app.</span></span>

1. <span data-ttu-id="5f4e7-108">Peka på **Funktioner** och välj plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="5f4e7-108">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="5f4e7-109">Välj **HTTP-utlösare**.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-109">Select **HTTP trigger**.</span></span>

1. <span data-ttu-id="5f4e7-110">Lämna standardvärdet för **Namn**.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-110">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="5f4e7-111">Välj **Anonym** för **Auktorisationsnivå**.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-111">Set the **Authorization level** to **Anonymous**.</span></span>

1. <span data-ttu-id="5f4e7-112">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-112">Select **Create**.</span></span>

1. <span data-ttu-id="5f4e7-113">Ta en titt på den automatiskt genererade koden så att du får en uppfattning om vad som sker.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-113">Take a quick look at the auto-generated code to get an idea about what's going on.</span></span> <span data-ttu-id="5f4e7-114">Parametern *req* representerar den inkommande begäran och innehåller parametern *name*.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-114">The *req* parameter represents the incoming request and contains a *name* parameter.</span></span> <span data-ttu-id="5f4e7-115">Vi kontrollerar om *name* har ett värde.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-115">We check to see if *name* has a value.</span></span> <span data-ttu-id="5f4e7-116">I så fall returnerar vi en hälsning.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-116">If it does, we return a greeting.</span></span> <span data-ttu-id="5f4e7-117">Annars returnerar vi ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-117">Otherwise, we return an error message.</span></span>

## <a name="get-your-function-url"></a><span data-ttu-id="5f4e7-118">Hämta funktionens webbadress</span><span class="sxs-lookup"><span data-stu-id="5f4e7-118">Get your function URL</span></span>

<span data-ttu-id="5f4e7-119">Nu när vi har skapat HTTP-utlösaren ska vi hämta funktionens webbadress så att vi kan börja skapa en begäran.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-119">Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.</span></span>

1. <span data-ttu-id="5f4e7-120">Välj HTTP-utlösaren för att öppna kodskärmen.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-120">Select your HTTP trigger to open the code screen.</span></span>

1. <span data-ttu-id="5f4e7-121">Välj **Hämta funktionswebbadress** till höger om **Kör**.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-121">To the right of **Run**, select **Get function URL**.</span></span>

1. <span data-ttu-id="5f4e7-122">Välj **Kopiera** och stäng sedan funktionens popupfönster.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-122">Select **Copy**, then close the function URL popup.</span></span>

## <a name="issue-a-get-request-to-your-http-trigger"></a><span data-ttu-id="5f4e7-123">Skicka en GET-begäran till HTTP-utlösaren</span><span class="sxs-lookup"><span data-stu-id="5f4e7-123">Issue a GET request to your HTTP trigger</span></span>

<span data-ttu-id="5f4e7-124">Nu har vi kopierat funktionens webbadress till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-124">We now have our function URL copied to our clipboard.</span></span> <span data-ttu-id="5f4e7-125">Vi ska skicka en GET-begäran och se om vi får något svar.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-125">Let's issue a GET request to see if we get a response.</span></span>

1. <span data-ttu-id="5f4e7-126">Öppna en ny flik i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-126">Open a new tab in your web browser.</span></span>

1. <span data-ttu-id="5f4e7-127">Klistra in webbadressen i adressfältet.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-127">Paste the URL into the address bar.</span></span>

1. <span data-ttu-id="5f4e7-128">Lägg till en frågesträngsparameter med namnet *name* med ditt eget namn, till exempel `.../api/HttpTriggerCSharp1?name=Jesse`</span><span class="sxs-lookup"><span data-stu-id="5f4e7-128">Add a query string parameter called *name* with your name for example `.../api/HttpTriggerCSharp1?name=Jesse`</span></span>

1. <span data-ttu-id="5f4e7-129">Tryck på <kbd>ENTER</kbd> att skicka begäran.</span><span class="sxs-lookup"><span data-stu-id="5f4e7-129">Press <kbd>ENTER</kbd> to submit the request.</span></span>
