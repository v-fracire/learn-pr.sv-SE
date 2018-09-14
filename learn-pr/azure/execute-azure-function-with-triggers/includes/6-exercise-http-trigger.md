<span data-ttu-id="9786c-101">I den här övningen ska vi skapa en Azure-funktion som tar emot en HTTP-begäran med en enda sträng.</span><span class="sxs-lookup"><span data-stu-id="9786c-101">In this exercise, we're going to create an Azure function that accepts an HTTP request with a single string.</span></span> <span data-ttu-id="9786c-102">Funktionen returnerar en sträng till anroparen som representerar att åtgärden har lyckats eller misslyckats.</span><span class="sxs-lookup"><span data-stu-id="9786c-102">The function returns a string back to the caller to represent success or failure.</span></span>

> [!NOTE]
> <span data-ttu-id="9786c-103">För att kunna slutföra den här övningen måste du vara inloggad på [Azure Portal](https://portal.azure.com/) med ett giltigt konto.</span><span class="sxs-lookup"><span data-stu-id="9786c-103">To complete this exercise, make sure you're signed in to the [Azure portal](https://portal.azure.com/) with a valid account.</span></span>

## <a name="create-an-http-trigger"></a><span data-ttu-id="9786c-104">Skapa en HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="9786c-104">Create an HTTP trigger</span></span>

<span data-ttu-id="9786c-105">Vi ska fortsätta att använda vår befintliga Azure Functions-app och lägger till en HTTP-utlösare.</span><span class="sxs-lookup"><span data-stu-id="9786c-105">Let's continue using our existing Azure Functions application and add an HTTP trigger.</span></span>

1. <span data-ttu-id="9786c-106">Peka på **Funktioner** och välj plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="9786c-106">Point to **Functions** and select the plus (+) icon.</span></span>

    ![Peka på Funktioner och välj plustecknet](../media-drafts/4-hover-function.png)

1. <span data-ttu-id="9786c-108">Välj **HTTP-utlösare**.</span><span class="sxs-lookup"><span data-stu-id="9786c-108">Select **HTTP trigger**.</span></span>

1. <span data-ttu-id="9786c-109">Välj **C#** som språk.</span><span class="sxs-lookup"><span data-stu-id="9786c-109">Select **C#** as the language.</span></span> 

1. <span data-ttu-id="9786c-110">Lämna standardvärdet för **Namn**.</span><span class="sxs-lookup"><span data-stu-id="9786c-110">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="9786c-111">Välj **Anonym** för **Auktorisationsnivå**.</span><span class="sxs-lookup"><span data-stu-id="9786c-111">Set the **Authorization level** to **Anonymous**.</span></span>

1. <span data-ttu-id="9786c-112">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9786c-112">Select **Create**.</span></span>

1. <span data-ttu-id="9786c-113">Ta en titt på den automatiskt genererade koden så att du får en uppfattning om vad som sker.</span><span class="sxs-lookup"><span data-stu-id="9786c-113">Take a quick look at the auto-generated code to get an idea about what's going on.</span></span> <span data-ttu-id="9786c-114">Parametern *req* representerar den inkommande begäran och innehåller parametern *name*.</span><span class="sxs-lookup"><span data-stu-id="9786c-114">The *req* parameter represents the incoming request and contains a *name* parameter.</span></span> <span data-ttu-id="9786c-115">Vi kontrollerar om *name* har ett värde.</span><span class="sxs-lookup"><span data-stu-id="9786c-115">We check to see if *name* has a value.</span></span> <span data-ttu-id="9786c-116">I så fall returnerar vi en hälsning.</span><span class="sxs-lookup"><span data-stu-id="9786c-116">If it does, we return a greeting.</span></span> <span data-ttu-id="9786c-117">Annars returnerar vi ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="9786c-117">Otherwise, we return an error message.</span></span>

## <a name="get-your-function-url"></a><span data-ttu-id="9786c-118">Hämta funktionens webbadress</span><span class="sxs-lookup"><span data-stu-id="9786c-118">Get your function URL</span></span>

<span data-ttu-id="9786c-119">Nu när vi har skapat HTTP-utlösaren ska vi hämta funktionens webbadress så att vi kan börja skapa en begäran.</span><span class="sxs-lookup"><span data-stu-id="9786c-119">Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.</span></span>

1. <span data-ttu-id="9786c-120">Välj HTTP-utlösaren för att öppna kodskärmen.</span><span class="sxs-lookup"><span data-stu-id="9786c-120">Select your HTTP trigger to open the code screen.</span></span>

1. <span data-ttu-id="9786c-121">Välj **Hämta funktionswebbadress** till höger om **Kör**.</span><span class="sxs-lookup"><span data-stu-id="9786c-121">To the right of **Run**, select **Get function URL**.</span></span>

1. <span data-ttu-id="9786c-122">Välj **Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="9786c-122">Select **Copy**.</span></span>

1. <span data-ttu-id="9786c-123">Välj **Kör** för att starta funktionen.</span><span class="sxs-lookup"><span data-stu-id="9786c-123">Select **Run** to start your function.</span></span>

## <a name="issue-a-get-request-to-your-http-trigger"></a><span data-ttu-id="9786c-124">Skicka en GET-begäran till HTTP-utlösaren</span><span class="sxs-lookup"><span data-stu-id="9786c-124">Issue a GET request to your HTTP trigger</span></span>

<span data-ttu-id="9786c-125">Nu har vi kopierat funktionens webbadress till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="9786c-125">We now have our function URL copied to our clipboard.</span></span> <span data-ttu-id="9786c-126">Vi ska skicka en GET-begäran och se om vi får något svar.</span><span class="sxs-lookup"><span data-stu-id="9786c-126">Let's issue a GET request to see if we get a response.</span></span>

1. <span data-ttu-id="9786c-127">Öppna en ny flik i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="9786c-127">Open a new tab in your web browser.</span></span>

1. <span data-ttu-id="9786c-128">Klistra in webbadressen i adressfältet.</span><span class="sxs-lookup"><span data-stu-id="9786c-128">Paste the URL into the address bar.</span></span>

1. <span data-ttu-id="9786c-129">Lägg till en frågesträngsparameter med namnet *name* med till exempel ditt eget namn:</span><span class="sxs-lookup"><span data-stu-id="9786c-129">Add a query string parameter called *name* with your name for example:</span></span>

    ```
    .../api/HttpTriggerCSharp1?name=Jesse
    ```

1. <span data-ttu-id="9786c-130">Klicka på Enter för att skicka förfrågningen.</span><span class="sxs-lookup"><span data-stu-id="9786c-130">Select ENTER to submit the request.</span></span>

## <a name="clean-up"></a><span data-ttu-id="9786c-131">Rensa</span><span class="sxs-lookup"><span data-stu-id="9786c-131">Clean up</span></span>

<span data-ttu-id="9786c-132">För att undvika att debiteras för den här funktionen väljer du **Pausa** ovanför loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="9786c-132">To ensure that you aren't charged for this function, select **Pause** above the log window.</span></span>

![Pausa](../media-drafts/4-pause-timer.png)


