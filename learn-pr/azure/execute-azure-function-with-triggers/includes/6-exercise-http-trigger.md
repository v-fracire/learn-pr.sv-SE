<span data-ttu-id="82a5b-101">I den här övningen ska vi skapa en Azure-funktion som tar emot en HTTP-förfrågan med en enda sträng.</span><span class="sxs-lookup"><span data-stu-id="82a5b-101">In this exercise, we're going to create an Azure function that accepts an HTTP request with a single string.</span></span> <span data-ttu-id="82a5b-102">Funktionen returnerar en sträng till anroparen som representerar att åtgärden har lyckats eller misslyckats.</span><span class="sxs-lookup"><span data-stu-id="82a5b-102">The function returns a string back to the caller to represent success or failure.</span></span>

> [!NOTE]
> <span data-ttu-id="82a5b-103">För att kunna slutföra den här övningen måste du vara inloggad på [Azure Portal](https://portal.azure.com/) med ett giltigt konto.</span><span class="sxs-lookup"><span data-stu-id="82a5b-103">To complete this exercise, make sure you're signed in to the [Azure portal](https://portal.azure.com/) with a valid account.</span></span>

## <a name="create-an-http-trigger"></a><span data-ttu-id="82a5b-104">Skapa en HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="82a5b-104">Create an HTTP trigger</span></span>

<span data-ttu-id="82a5b-105">Vi fortsätter använda vår befintliga Azure Functions-app och lägger nu till en HTTP-utlösare.</span><span class="sxs-lookup"><span data-stu-id="82a5b-105">Let's continue using our existing Azure Functions application and add an HTTP trigger.</span></span>

1. <span data-ttu-id="82a5b-106">Peka på **Functions** och välj plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="82a5b-106">Point to **Functions** and select the plus (+) icon.</span></span>

    ![Peka på Functions och välj plustecknet](../media/4-hover-function.png)

1. <span data-ttu-id="82a5b-108">Välj **HTTP-utlösare**.</span><span class="sxs-lookup"><span data-stu-id="82a5b-108">Select **HTTP trigger**.</span></span>

1. <span data-ttu-id="82a5b-109">Välj **#C** som språk.</span><span class="sxs-lookup"><span data-stu-id="82a5b-109">Select **C#** as the language.</span></span> 

1. <span data-ttu-id="82a5b-110">Låt **Namn** vara inställt på standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="82a5b-110">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="82a5b-111">Som **Auktorisationsnivå** väljer du **Anonym**.</span><span class="sxs-lookup"><span data-stu-id="82a5b-111">Set the **Authorization level** to **Anonymous**.</span></span>

1. <span data-ttu-id="82a5b-112">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="82a5b-112">Select **Create**.</span></span>

1. <span data-ttu-id="82a5b-113">Ta en titt på den automatiskt genererade koden så att du får en uppfattning om vad som sker.</span><span class="sxs-lookup"><span data-stu-id="82a5b-113">Take a quick look at the auto-generated code to get an idea about what's going on.</span></span> <span data-ttu-id="82a5b-114">Parametern *req* representerar den inkommande förfrågningen och innehåller parametern *name*.</span><span class="sxs-lookup"><span data-stu-id="82a5b-114">The *req* parameter represents the incoming request and contains a *name* parameter.</span></span> <span data-ttu-id="82a5b-115">Vi kontrollerar att *name* har ett värde.</span><span class="sxs-lookup"><span data-stu-id="82a5b-115">We check to see if *name* has a value.</span></span> <span data-ttu-id="82a5b-116">I så fall returnerar vi en hälsning.</span><span class="sxs-lookup"><span data-stu-id="82a5b-116">If it does, we return a greeting.</span></span> <span data-ttu-id="82a5b-117">Annars returnerar vi ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="82a5b-117">Otherwise, we return an error message.</span></span>

## <a name="get-your-function-url"></a><span data-ttu-id="82a5b-118">Hämta funktionens webbadress</span><span class="sxs-lookup"><span data-stu-id="82a5b-118">Get your function URL</span></span>

<span data-ttu-id="82a5b-119">Nu när vi har skapat HTTP-utlösaren kan vi hämta funktionens webbadress så att vi kan skapa en förfrågning.</span><span class="sxs-lookup"><span data-stu-id="82a5b-119">Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.</span></span>

1. <span data-ttu-id="82a5b-120">Välj din HTTP-utlösare så att kodskärmen öppnas.</span><span class="sxs-lookup"><span data-stu-id="82a5b-120">Select your HTTP trigger to open the code screen.</span></span>

1. <span data-ttu-id="82a5b-121">Till höger om **Kör** väljer du **Hämta funktionswebbadress**.</span><span class="sxs-lookup"><span data-stu-id="82a5b-121">To the right of **Run**, select **Get function URL**.</span></span>

1. <span data-ttu-id="82a5b-122">Välj **Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="82a5b-122">Select **Copy**.</span></span>

1. <span data-ttu-id="82a5b-123">Välj **Kör** för att starta funktionen.</span><span class="sxs-lookup"><span data-stu-id="82a5b-123">Select **Run** to start your function.</span></span>

## <a name="issue-a-get-request-to-your-http-trigger"></a><span data-ttu-id="82a5b-124">Utfärda en GET-begäran till din HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="82a5b-124">Issue a GET request to your HTTP trigger</span></span>

<span data-ttu-id="82a5b-125">Nu har vi kopierat funktionens webbadress till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="82a5b-125">We now have our function URL copied to our clipboard.</span></span> <span data-ttu-id="82a5b-126">Vi utfärdar en GET-förfrågning och ser om vi får något svar.</span><span class="sxs-lookup"><span data-stu-id="82a5b-126">Let's issue a GET request to see if we get a response.</span></span>

1. <span data-ttu-id="82a5b-127">Öppna en ny flik i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="82a5b-127">Open a new tab in your web browser.</span></span>

1. <span data-ttu-id="82a5b-128">Klistra in webbadressen i adressfältet.</span><span class="sxs-lookup"><span data-stu-id="82a5b-128">Paste the URL into the address bar.</span></span>

1. <span data-ttu-id="82a5b-129">Lägg till en frågesträngsparameter med namnet *name* med till exempel ditt eget namn:</span><span class="sxs-lookup"><span data-stu-id="82a5b-129">Add a query string parameter called *name* with your name for example:</span></span>

    ```
    .../api/HttpTriggerCSharp1?name=Jesse
    ```

1. <span data-ttu-id="82a5b-130">Klicka på Enter för att skicka förfrågningen.</span><span class="sxs-lookup"><span data-stu-id="82a5b-130">Select ENTER to submit the request.</span></span>

## <a name="clean-up"></a><span data-ttu-id="82a5b-131">Rensa</span><span class="sxs-lookup"><span data-stu-id="82a5b-131">Clean up</span></span>

<span data-ttu-id="82a5b-132">För att säkerställa att du inte debiteras för den här funktionen, väljer du **Pausa** ovanför loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="82a5b-132">To ensure that you aren't charged for this function, select **Pause** above the log window.</span></span>

![Pausa](../media/4-pause-timer.png)


