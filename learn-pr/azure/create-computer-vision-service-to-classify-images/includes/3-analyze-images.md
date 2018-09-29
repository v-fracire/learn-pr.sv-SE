<span data-ttu-id="2c959-101">Som en chefsutvecklare på Contoso Beverage Distribution är du ansvarig för att bygga och underhålla en verksamhetsspecifik app som låter dina distributörer skanna och överföra bilder av butikshyllor där de fyller på lagren.</span><span class="sxs-lookup"><span data-stu-id="2c959-101">As a lead developer at Contoso Beverage Distribution, you're responsible for building and maintaining a line-of-business app that lets your frontline distributors scan and upload images of store shelves where they are restocking.</span></span> 

<span data-ttu-id="2c959-102">Du vill verifiera att alla bilder som publicerats av användare följer de innehållsregler som ditt företag har tagit fram.</span><span class="sxs-lookup"><span data-stu-id="2c959-102">You want to validate that any images posted by users respect the content rules set by your company.</span></span> <span data-ttu-id="2c959-103">Företaget vill inte att olämpligt innehåll publiceras på företagets webbplatser.</span><span class="sxs-lookup"><span data-stu-id="2c959-103">The company doesn't want inappropriate content posted to company sites.</span></span> <span data-ttu-id="2c959-104">Med tanke på framstegen i Artificial Intelligence bestämmer du dig för att använda API för visuellt innehåll i din app.</span><span class="sxs-lookup"><span data-stu-id="2c959-104">Given the advancements in Artificial Intelligence, you decide to use the Computer Vision API in your app.</span></span> 

<span data-ttu-id="2c959-105">För att komma igång skapar du ett konto för visuellt innehåll i din Azure-prenumeration och börjar testa **analyserings**-funktionen.</span><span class="sxs-lookup"><span data-stu-id="2c959-105">To get started, you create a Computer Vision account in your Azure subscription and start testing the **analyze** feature.</span></span>

## <a name="calling-the-computer-vision-api-to-analyze-images"></a><span data-ttu-id="2c959-106">Anropa API för visuellt innehåll för att analysera bilder</span><span class="sxs-lookup"><span data-stu-id="2c959-106">Calling the Computer Vision API to analyze images</span></span>

<span data-ttu-id="2c959-107">Åtgärden `analyze` extraherar en omfattande uppsättning visuella funktioner baserat på innehållet i bilden.</span><span class="sxs-lookup"><span data-stu-id="2c959-107">The `analyze` operation extracts a rich set of visual features based on the image content.</span></span> <span data-ttu-id="2c959-108">Fråge-URL har följande format:</span><span class="sxs-lookup"><span data-stu-id="2c959-108">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=<...>&details=<...>&language=<...>`

<span data-ttu-id="2c959-109">De parametrar som skickas till tjänsten är: `visualFeatures`, `details` och `languages`.</span><span class="sxs-lookup"><span data-stu-id="2c959-109">The parameters that are sent to the service are `visualFeatures`, `details`, and `languages`.</span></span> <span data-ttu-id="2c959-110">Ange parametern `details` till `Landmarks` eller `Celebrities` för att identifiera landmärken eller kändisar.</span><span class="sxs-lookup"><span data-stu-id="2c959-110">Set the `details` parameter to `Landmarks` or `Celebrities` to help you identify landmarks or celebrities.</span></span> <span data-ttu-id="2c959-111">`visualFeatures` identifierar vilken slags information du vill att tjänsten ska returnera åt dig.</span><span class="sxs-lookup"><span data-stu-id="2c959-111">`visualFeatures` identify what kind of information you want the service to return you.</span></span> <span data-ttu-id="2c959-112">Alternativet `Categories` kategoriserar bildernas innehåll, till exempel träd, byggnader med mera.</span><span class="sxs-lookup"><span data-stu-id="2c959-112">The `Categories` option will categorize the content of the images like trees, buildings, and more.</span></span> <span data-ttu-id="2c959-113">`Faces` identifierar personernas ansikten och visar deras kön och ålder.</span><span class="sxs-lookup"><span data-stu-id="2c959-113">`Faces` will identify people's faces and give you their gender and age.</span></span>

<span data-ttu-id="2c959-114">Alla åtgärder i API för visuellt innehåll har följande begränsningar när det gäller de bilder du ber den behandla:</span><span class="sxs-lookup"><span data-stu-id="2c959-114">All operations on the Computer Vision API have the following restrictions when it comes to the images you ask it to process:</span></span>

- <span data-ttu-id="2c959-115">Bildformat som stöds: JPEG, PNG, GIF, BMP.</span><span class="sxs-lookup"><span data-stu-id="2c959-115">Supported image formats: JPEG, PNG, GIF, BMP.</span></span> 
- <span data-ttu-id="2c959-116">Bildens filstorlek måste vara mindre än 4 MB.</span><span class="sxs-lookup"><span data-stu-id="2c959-116">Image file size must be less than  4 MB.</span></span>
- <span data-ttu-id="2c959-117">Bilddimensionerna måste vara minst 50 x 50.</span><span class="sxs-lookup"><span data-stu-id="2c959-117">Image dimensions must be at least 50 x 50.</span></span>

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a><span data-ttu-id="2c959-118">Identifiera landmärken i en bild</span><span class="sxs-lookup"><span data-stu-id="2c959-118">Identify landmarks in an image</span></span>

<span data-ttu-id="2c959-119">Låt oss börja med ett anrop för att hitta landmärken i en bild.</span><span class="sxs-lookup"><span data-stu-id="2c959-119">Let's start with a call to find landmarks in an image.</span></span> <span data-ttu-id="2c959-120">Vi använder följande bild för det här exemplet, men du kan testa samma kommando med URL:er till andra bilder.</span><span class="sxs-lookup"><span data-stu-id="2c959-120">We'll use the following image for this example, but you're free to try the same command with URLs to other images.</span></span> 

![Bild av ett bergområde med blå himmel ovanför de snötäckta bergen.](../media/3-mountains.jpg)

<span data-ttu-id="2c959-122">Kör följande kommando i Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="2c959-122">Execute the following command in Azure Cloud Shell.</span></span> <span data-ttu-id="2c959-123">Ersätt `<region>` i kommandot med ditt Cognitive Services-kontos region.</span><span class="sxs-lookup"><span data-stu-id="2c959-123">Replace `<region>` in the command with the region of your cognitive services account.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

- <span data-ttu-id="2c959-124">Det här anropet söker efter landmärken i bilden som anges med bild-URL:en.</span><span class="sxs-lookup"><span data-stu-id="2c959-124">This call looks for landmarks in the image specified by the image URL.</span></span> <span data-ttu-id="2c959-125">Den analyserade bilden lagras i en GitHub-lagringsplats för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="2c959-125">The image we are analyzing is stored in a GitHub repo for this exercise.</span></span> 
- <span data-ttu-id="2c959-126">Anropet uppmanar även tjänsten för att returnera information om programvarukategorier och en beskrivning av bilden.</span><span class="sxs-lookup"><span data-stu-id="2c959-126">The call also asks the service to return category information and a description of the image.</span></span> <span data-ttu-id="2c959-127">Beskrivningen returneras som en hel engelsk mening.</span><span class="sxs-lookup"><span data-stu-id="2c959-127">The description is returned as a complete English sentence.</span></span> 
- <span data-ttu-id="2c959-128">Som du vet behöver varje anrop till API:et en åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="2c959-128">As you know, every call to the API needs an access key.</span></span> <span data-ttu-id="2c959-129">Detta ställs in i rubriken `Ocp-Apim-Subscription-Key` i begäran.</span><span class="sxs-lookup"><span data-stu-id="2c959-129">This is set in the `Ocp-Apim-Subscription-Key` header of the request.</span></span> 

> [!TIP]
> <span data-ttu-id="2c959-130">Resultatet av begäran är JSON-rådatafilen som har en beskrivning av bilden i sitt `url`.</span><span class="sxs-lookup"><span data-stu-id="2c959-130">The result of the request is the raw JSON describing the picture in the `url`.</span></span> <span data-ttu-id="2c959-131">Vi har lagt till ` | jq '.'` i slutet av kommandot för att förklara JSON-utdata.</span><span class="sxs-lookup"><span data-stu-id="2c959-131">We added ` | jq '.'` at the end of the command to prettify the JSON output.</span></span>

<span data-ttu-id="2c959-132">JSON-svaret från det här anropet returnerar följande:</span><span class="sxs-lookup"><span data-stu-id="2c959-132">The JSON response from this call returns the following:</span></span>

- <span data-ttu-id="2c959-133">En `categories`-matris som listar alla kategorier för bilden som har identifierats, tillsammans med en poäng mellan 0 och 1 över hur säker tjänsten är på att bilden tillhör den angivna kategorin.</span><span class="sxs-lookup"><span data-stu-id="2c959-133">A `categories` array listing all image categories that were detected, along with a score between 0 and 1 of how confident the service is that the image belongs in the specified category.</span></span>
- <span data-ttu-id="2c959-134">En `description`-post som innehåller en matris med taggar eller ord som är relaterade till bilden.</span><span class="sxs-lookup"><span data-stu-id="2c959-134">A `description` entry containing an array of tags or words that are related to the image.</span></span>
- <span data-ttu-id="2c959-135">En `captions`-post med ett textfält som beskriver på engelska vad som är nytt i bilden.</span><span class="sxs-lookup"><span data-stu-id="2c959-135">A `captions` entry with a text field that describes in English what is in the image.</span></span> <span data-ttu-id="2c959-136">Observera att texten också har en säkerhetspoäng.</span><span class="sxs-lookup"><span data-stu-id="2c959-136">Observe that the text also has a certainty score.</span></span> <span data-ttu-id="2c959-137">Denna poäng kan hjälpa dig att avgöra vad du ska göra med den här analysen.</span><span class="sxs-lookup"><span data-stu-id="2c959-137">This score can help you decide what to do next with this analysis.</span></span>


## <a name="check-for-inappropriate-content-in-an-image"></a><span data-ttu-id="2c959-138">Sök efter olämpligt innehåll i en bild</span><span class="sxs-lookup"><span data-stu-id="2c959-138">Check for inappropriate content in an image</span></span>

<span data-ttu-id="2c959-139">I det här exemplet ska vi analysera en bild för vuxet innehåll.</span><span class="sxs-lookup"><span data-stu-id="2c959-139">In this example, we'll analyze an image for adult content.</span></span> <span data-ttu-id="2c959-140">Förtroendepoängen bedömer sannolikheten att bilden innehåller vuxet eller olämpligt innehåll.</span><span class="sxs-lookup"><span data-stu-id="2c959-140">The confidence score rates the likelihood that the image contains either adult or racy content.</span></span> 

<span data-ttu-id="2c959-141">Vi använder följande bild för det här exemplet, men du kan testa samma kommando med URL:er till andra bilder.</span><span class="sxs-lookup"><span data-stu-id="2c959-141">We'll use the following image for this example, but you're free to try the same command with URLs to other images.</span></span> 

![Bild av en leende familj.](../media/3-people.png)

1. <span data-ttu-id="2c959-143">Kör följande kommando i Azure Cloud Shell och ersätt `<region>` i URL:en.</span><span class="sxs-lookup"><span data-stu-id="2c959-143">Execute the following command in Azure Cloud Shell, replacing `<region>` in the URL.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

<span data-ttu-id="2c959-144">I det här exemplet anger vi `visualFeatures` till `Adult,Description`.</span><span class="sxs-lookup"><span data-stu-id="2c959-144">In this example, we set the `visualFeatures` to `Adult,Description`.</span></span> 

<span data-ttu-id="2c959-145">Svaret ger oss två förtroendepoäng, ett för olämpligt innehåll och ett för vuxet innehåll.</span><span class="sxs-lookup"><span data-stu-id="2c959-145">The response gives us two confidence scores, one for racy content and one for adult content.</span></span> <span data-ttu-id="2c959-146">Med dessa poäng samt bildbeskrivningen och andra visuella funktioner kan du börja flagga bilder som ska publiceras till servern.</span><span class="sxs-lookup"><span data-stu-id="2c959-146">Using these scores plus the image description and other visual features, you can start to flag images posted to your server.</span></span>

<span data-ttu-id="2c959-147">De exempel som vi provat i den här övningen ger dig en uppfattning om vilken typ av analys som du kan göra med åtgärden `analyze`.</span><span class="sxs-lookup"><span data-stu-id="2c959-147">The examples we tried in this exercise give you an idea of the type of analysis that you can do with the `analyze` operation.</span></span> <span data-ttu-id="2c959-148">Prova analyser med olika bilder och prova olika kombinationer av visuella funktioner, information och språk.</span><span class="sxs-lookup"><span data-stu-id="2c959-148">Try out the analysis with different images and try different combinations of visualFeatures, details, and languages parameters.</span></span>

<span data-ttu-id="2c959-149">Mer information om åtgärden `analyze` finns i referensdokumentationen [Analysera bild](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).</span><span class="sxs-lookup"><span data-stu-id="2c959-149">For more information about the `analyze` operation, see the [Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) reference documentation.</span></span>