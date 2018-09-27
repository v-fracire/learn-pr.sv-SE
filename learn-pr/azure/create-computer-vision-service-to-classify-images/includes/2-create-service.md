
## <a name="what-is-microsoft-cognitive-services"></a><span data-ttu-id="9299f-101">Vad är Microsoft Cognitive Services?</span><span class="sxs-lookup"><span data-stu-id="9299f-101">What is Microsoft Cognitive Services?</span></span>

<span data-ttu-id="9299f-102">Microsoft Cognitive Services är en uppsättning maskininlärningsalgoritmer som är tillgänglig som en tjänst för allmän användning.</span><span class="sxs-lookup"><span data-stu-id="9299f-102">Microsoft Cognitive Services is a set of machine learning algorithms available as a service for anyone to use.</span></span> <span data-ttu-id="9299f-103">Vi kan använda dessa tjänster för visuellt innehåll, tal, språk, kunskap och sökning i stället för att skapa den här intelligensen för våra appar från grunden.</span><span class="sxs-lookup"><span data-stu-id="9299f-103">Instead of building this intelligence for our apps from scratch, we can use these services for vision, speech, language, knowledge, and search.</span></span> <span data-ttu-id="9299f-104">Du kan prova varje tjänst utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="9299f-104">You can try each service for free.</span></span> <span data-ttu-id="9299f-105">När du vill integrera en tjänst i din app kan du registrera dig för en betald prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9299f-105">When you decide to integrate a service into your app, you sign up for a paid subscription.</span></span> <span data-ttu-id="9299f-106">Vi kommer att fokusera vår uppmärksamhet på API för visuellt innehåll i den här modellen.</span><span class="sxs-lookup"><span data-stu-id="9299f-106">We'll focus our attention on the Computer Vision API in this model.</span></span>

> [!TIP]
> <span data-ttu-id="9299f-107">Om du vill se en lista med alla Cognitive Services kan du ta en titt i [Cognitive Services-katalogen](https://azure.microsoft.com/services/cognitive-services/directory/).</span><span class="sxs-lookup"><span data-stu-id="9299f-107">To see a list of all Cognitive Services, check out the [Cognitive Services Directory](https://azure.microsoft.com/services/cognitive-services/directory/).</span></span> 

## <a name="what-is-the-computer-vision-api"></a><span data-ttu-id="9299f-108">Vad är API för visuellt innehåll?</span><span class="sxs-lookup"><span data-stu-id="9299f-108">What is the Computer Vision API?</span></span>

<span data-ttu-id="9299f-109">API för visuellt innehåll ger algoritmer för att bearbeta bilder och returnera insikter.</span><span class="sxs-lookup"><span data-stu-id="9299f-109">The Computer Vision API provides algorithms to process images and return insights.</span></span> <span data-ttu-id="9299f-110">Du kan till exempel ta reda på om en bild har innehåll för vuxna, eller använda den för att hitta alla ansikten i en bild.</span><span class="sxs-lookup"><span data-stu-id="9299f-110">For example, you can find out if an image has mature content, or can use it to find all the faces in an image.</span></span> <span data-ttu-id="9299f-111">Den har även andra funktioner som att beräkna dominerande och accentfärger, kategorisera och dela upp innehållet i bilder och beskriva en bild med fullständiga engelska meningar.</span><span class="sxs-lookup"><span data-stu-id="9299f-111">It also has other features like estimating dominant and accent colors, categorizing the content of images, and describing an image with complete English sentences.</span></span> <span data-ttu-id="9299f-112">Den kan dessutom också smart generera miniatyrer av bilder för att visa stora bilder på ett effektivt sätt.</span><span class="sxs-lookup"><span data-stu-id="9299f-112">Additionally, it can also intelligently generate images thumbnails for displaying large images effectively.</span></span>

> [!TIP]
> <span data-ttu-id="9299f-113">API för visuellt innehåll är tillgängligt i många regioner över hela världen.</span><span class="sxs-lookup"><span data-stu-id="9299f-113">The Computer Vision API is available in many regions across the globe.</span></span> <span data-ttu-id="9299f-114">Du hittar regionen närmast dig i [Produkttillgänglighet per region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).</span><span class="sxs-lookup"><span data-stu-id="9299f-114">To find the region nearest you, see the [Products available by region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).</span></span>

<span data-ttu-id="9299f-115">Du kan använda API för visuellt innehåll för att:</span><span class="sxs-lookup"><span data-stu-id="9299f-115">You can use the Computer Vision API to:</span></span>

- <span data-ttu-id="9299f-116">Analysera bilder för att få information</span><span class="sxs-lookup"><span data-stu-id="9299f-116">Analyze images for insight</span></span>
- <span data-ttu-id="9299f-117">Extrahera utskriven text från bilder med optisk teckenigenkänning (OCR).</span><span class="sxs-lookup"><span data-stu-id="9299f-117">Extract printed text from images using optical character recognition (OCR).</span></span>
- <span data-ttu-id="9299f-118">Känna igen utskriven och handskriven text från bilder</span><span class="sxs-lookup"><span data-stu-id="9299f-118">Recognize printed and handwritten text from images</span></span>
- <span data-ttu-id="9299f-119">Identifiera kända personer och landmärken</span><span class="sxs-lookup"><span data-stu-id="9299f-119">Recognize celebrities and landmarks</span></span>
- <span data-ttu-id="9299f-120">Analysera video</span><span class="sxs-lookup"><span data-stu-id="9299f-120">Analyze video</span></span> 
- <span data-ttu-id="9299f-121">Skapa en miniatyrbild av en bild</span><span class="sxs-lookup"><span data-stu-id="9299f-121">Generate a thumbnail of an image</span></span> 

## <a name="how-to-call-the-computer-vision-api"></a><span data-ttu-id="9299f-122">Hur anropar man API för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="9299f-122">How to call the Computer Vision API</span></span>

<span data-ttu-id="9299f-123">Du kan anropa visuellt innehåll i ditt program med klientbibliotek eller REST API direkt.</span><span class="sxs-lookup"><span data-stu-id="9299f-123">You call Computer Vision in your application using client libraries or the REST API directly.</span></span> <span data-ttu-id="9299f-124">Vi anropar REST API i den här modulen.</span><span class="sxs-lookup"><span data-stu-id="9299f-124">We'll call the REST API in this module.</span></span> <span data-ttu-id="9299f-125">Så här gör man ett anrop:</span><span class="sxs-lookup"><span data-stu-id="9299f-125">To make a call:</span></span>

1. <span data-ttu-id="9299f-126">Hämta en API-åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="9299f-126">Get an API access key</span></span>

    <span data-ttu-id="9299f-127">Åtkomstnycklar tilldelas när du registrerar dig för ett tjänstkonto för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="9299f-127">You are assigned access keys when you sign up for a Computer Vision service account.</span></span> <span data-ttu-id="9299f-128">En nyckel måste skickas i rubriken för **varje** begäran.</span><span class="sxs-lookup"><span data-stu-id="9299f-128">A key must be passed in the header of **every** request.</span></span> 

1. <span data-ttu-id="9299f-129">Gör ett POST-anrop till API:et</span><span class="sxs-lookup"><span data-stu-id="9299f-129">Make a POST call to the API</span></span>

    <span data-ttu-id="9299f-130">Formatera URL:en på följande sätt: **region**.api.cognitive.microsoft.com/vision/v2.0/**resource**/**[parametrar]**</span><span class="sxs-lookup"><span data-stu-id="9299f-130">Format the URL as follows: **region**.api.cognitive.microsoft.com/vision/v2.0/**resource**/**[parameters]**</span></span> 

    - <span data-ttu-id="9299f-131">**region** – regionen där du har skapat kontot, till exempel *westus*.</span><span class="sxs-lookup"><span data-stu-id="9299f-131">**region** - the region where you created the account, for example, *westus*.</span></span>
    - <span data-ttu-id="9299f-132">**resurs** – den resurs för virtuellt innehåll som du anropar, såsom `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.</span><span class="sxs-lookup"><span data-stu-id="9299f-132">**resource** - the Computer Vision resource you are calling such as `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.</span></span>

    <span data-ttu-id="9299f-133">Du kan ange bilden som ska bearbetas antingen som en binär raw-bild eller en bild-URL.</span><span class="sxs-lookup"><span data-stu-id="9299f-133">You can supply the image to be processed either as a raw image binary or an image URL.</span></span>

    <span data-ttu-id="9299f-134">Begärandehuvudet måste innehålla prenumerationsnyckeln som ger åtkomst till detta API.</span><span class="sxs-lookup"><span data-stu-id="9299f-134">The request header must contain the subscription key, which provides access to this API.</span></span>

1. <span data-ttu-id="9299f-135">Parsa svaret</span><span class="sxs-lookup"><span data-stu-id="9299f-135">Parse the response</span></span>

    <span data-ttu-id="9299f-136">Svaret innehåller den information som API för visuellt innehåll hade om din bild som en JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="9299f-136">The response holds the insight the Computer Vision API had about your image as a JSON payload.</span></span>

<span data-ttu-id="9299f-137">Anta att du skapade min prenumeration för visuellt innehåll i regionen `westus` och fick en prenumerationsnyckel för åtkomst till API:et.</span><span class="sxs-lookup"><span data-stu-id="9299f-137">Suppose you created my Computer Vision subscription in the `westus` region and got a subscription key to access the API.</span></span> <span data-ttu-id="9299f-138">Nu ska vi ge nyckeln det fiktiva värdet `xxxx-xxxxx-xxxx-xxxx`.</span><span class="sxs-lookup"><span data-stu-id="9299f-138">Let's give the key a fictitious value of `xxxx-xxxxx-xxxx-xxxx`.</span></span> <span data-ttu-id="9299f-139">Nu vill du generera en miniatyrbild för en bild som finns på `http://example.com/images/test.jpg`.</span><span class="sxs-lookup"><span data-stu-id="9299f-139">You now want to generate a thumbnail for an image located at `http://example.com/images/test.jpg`.</span></span> <span data-ttu-id="9299f-140">Med hjälp av `generateThumbnail`, skulle begäran se ut så här.</span><span class="sxs-lookup"><span data-stu-id="9299f-140">Using `generateThumbnail`, the request would look like the following.</span></span>

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

<span data-ttu-id="9299f-141">I den här modulen kommer vi att köra alla övningarna i Azure CLI med hjälp av integrerad Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="9299f-141">In this module, we'll run all exercises in the Azure CLI using the integrated Cloud Shell.</span></span> <span data-ttu-id="9299f-142">Låt oss ta reda på lite mer om den här installationen.</span><span class="sxs-lookup"><span data-stu-id="9299f-142">Let's find out a little more about this setup.</span></span>

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="9299f-143">Vad är Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9299f-143">What is the Azure CLI</span></span>

<span data-ttu-id="9299f-144">Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="9299f-144">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources.</span></span> <span data-ttu-id="9299f-145">Det finns för macOS, Linux och Windows eller i webbläsaren med [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="9299f-145">It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9299f-146">I dag finns det två tillgängliga versioner av verktyget Azure CLI: Azure CLI 1.0 och Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="9299f-146">There are two versions of the Azure CLI tool available today: Azure CLI 1.0 and Azure CLI 2.0.</span></span> <span data-ttu-id="9299f-147">Vi använder Azure CLI 2.0, som är den senaste versionen och rekommenderas om du inte kör äldre skript.</span><span class="sxs-lookup"><span data-stu-id="9299f-147">We'll be using Azure CLI 2.0, which is the latest version and is recommended unless you're running legacy scripts.</span></span> <span data-ttu-id="9299f-148">Azure CLI 1.0 startas med kommandot `azure`, och Azure CLI 2.0 startas med kommandot `az`.</span><span class="sxs-lookup"><span data-stu-id="9299f-148">Azure CLI 1.0 is started with the `azure` command, and Azure CLI 2.0 is started with the `az` command.</span></span>

## <a name="az-cognitiveservices-commands"></a><span data-ttu-id="9299f-149">az cognitiveservices-kommandon</span><span class="sxs-lookup"><span data-stu-id="9299f-149">az cognitiveservices commands</span></span>

<span data-ttu-id="9299f-150">Azure CLI innehåller kommandot `cognitiveservices` för att hantera Cognitive Services-konton i Azure.</span><span class="sxs-lookup"><span data-stu-id="9299f-150">The Azure CLI includes the `cognitiveservices` command to manage Cognitive Services accounts in Azure.</span></span> <span data-ttu-id="9299f-151">Det finns flera underkommandon för att utföra specifika aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="9299f-151">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="9299f-152">De vanligaste är:</span><span class="sxs-lookup"><span data-stu-id="9299f-152">The most common include:</span></span>

| <span data-ttu-id="9299f-153">Underkommando</span><span class="sxs-lookup"><span data-stu-id="9299f-153">Subcommand</span></span> | <span data-ttu-id="9299f-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9299f-154">Description</span></span> |
|-------------|-------------|
| `list` | <span data-ttu-id="9299f-155">Skapa en lista över tillgängliga Azure Cognitive Services-konton.</span><span class="sxs-lookup"><span data-stu-id="9299f-155">List available Azure Cognitive Services accounts.</span></span> |
| `account show` | <span data-ttu-id="9299f-156">Hämta information om ett Azure Cognitive Services-konto.</span><span class="sxs-lookup"><span data-stu-id="9299f-156">Get the details of an Azure Cognitive Services account.</span></span> |
| `account create` | <span data-ttu-id="9299f-157">Skapa ett Azure Cognitive Services-konto.</span><span class="sxs-lookup"><span data-stu-id="9299f-157">Create an Azure Cognitive Services account.</span></span> |
| `account delete` | <span data-ttu-id="9299f-158">Ta bort ett Azure Cognitive Services-konto.</span><span class="sxs-lookup"><span data-stu-id="9299f-158">Delete an Azure Cognitive Services account.</span></span> |
| `keys list` | <span data-ttu-id="9299f-159">Skapa en lista över nycklar för ett Azure Cognitive Services-konto.</span><span class="sxs-lookup"><span data-stu-id="9299f-159">List the keys of an Azure Cognitive Services account.</span></span> |

<span data-ttu-id="9299f-160">Nu ska vi skapa ett Cognitive Services-konto med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9299f-160">Let's create a Cognitive Services with the Azure CLI.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="9299f-161">Skapa ett Cognitive Services-konto</span><span class="sxs-lookup"><span data-stu-id="9299f-161">Create a Cognitive Services account</span></span>

<span data-ttu-id="9299f-162">Vi behöver en API-åtkomstnyckel för att göra anrop till API för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="9299f-162">We need an API access key to make calls to the Computer Vision API.</span></span> <span data-ttu-id="9299f-163">För att få åtkomstnycklar, behöver vi ett Cognitive Services-konto för API för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="9299f-163">To get access keys, we need a Cognitive Services account for the Computer Vision API.</span></span> <span data-ttu-id="9299f-164">Vi använder `az cognitiveservices create` för att skapa kontot i vår prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9299f-164">We'll use `az cognitiveservices create` to create the account in our subscription.</span></span>

 <span data-ttu-id="9299f-165">Kommandot `az cognitiveservices create` används för att skapa ett Cognitive Services-konto i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9299f-165">The command `az cognitiveservices create` is used to create a Cognitive Services account in a resource group.</span></span>  <span data-ttu-id="9299f-166">Följande fem parametrar måste anges när du anropar det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="9299f-166">The following five parameters must be supplied when calling this command.</span></span>

> [!Tip]
> <span data-ttu-id="9299f-167">De flesta flaggor för Azure CLI-parametrar kan förkortas till ett enskilt tecken.</span><span class="sxs-lookup"><span data-stu-id="9299f-167">Most flags for Azure CLI parameters can be abbreviated to a single character.</span></span> <span data-ttu-id="9299f-168">Anta exempelvis att vi kan säga `-l` i stället för `--location`.</span><span class="sxs-lookup"><span data-stu-id="9299f-168">For example, we could say `-l` instead of `--location`.</span></span> <span data-ttu-id="9299f-169">Den långa formen används för tydlighetens skull.</span><span class="sxs-lookup"><span data-stu-id="9299f-169">The long-form is used for clarity.</span></span>

| <span data-ttu-id="9299f-170">Parameter</span><span class="sxs-lookup"><span data-stu-id="9299f-170">Parameter</span></span> | <span data-ttu-id="9299f-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9299f-171">Description</span></span> |
|-----------|-------------|
| `resource-group` | <span data-ttu-id="9299f-172">Den resursgrupp som ska äga Cognitive Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="9299f-172">The resource group that will own the cognitive services account.</span></span> <span data-ttu-id="9299f-173">I den här interaktiva sandbox-miljön använder du <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn></span><span class="sxs-lookup"><span data-stu-id="9299f-173">In this interactive sandbox session, you'll use <rgn>[sandbox resource group name]</rgn></span></span> |
| `kind` | <span data-ttu-id="9299f-174">API-namnet på Cognitive Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="9299f-174">The API name of cognitive services account.</span></span> |
| `name` | <span data-ttu-id="9299f-175">Cognitive service-kontonamnet.</span><span class="sxs-lookup"><span data-stu-id="9299f-175">Cognitive service account name.</span></span> |
| `sku` | <span data-ttu-id="9299f-176">SKU för Cognitive Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="9299f-176">The Sku of cognitive services account.</span></span>|
| `location` | <span data-ttu-id="9299f-177">Platsen eller regionen, från vilken du vill göra det här API-anropet.</span><span class="sxs-lookup"><span data-stu-id="9299f-177">The location, or region, from which you want to make calls to this API.</span></span> <span data-ttu-id="9299f-178">Välj någon av platserna i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="9299f-178">Select one of the locations from the list below.</span></span> |

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

<span data-ttu-id="9299f-179">Kör följande kommando i Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="9299f-179">Execute the following command in Azure Cloud Shell.</span></span> <span data-ttu-id="9299f-180">Ersätt `[location]` med en plats nära dig.</span><span class="sxs-lookup"><span data-stu-id="9299f-180">Make sure to replace `[location]` with a location near you.</span></span>

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--location [location]
```

> [!NOTE]
> <span data-ttu-id="9299f-181">Kom ihåg den plats du har valt.</span><span class="sxs-lookup"><span data-stu-id="9299f-181">Remember the location you have selected.</span></span> <span data-ttu-id="9299f-182">Du ska göra alla anrop till API:et från den regionen.</span><span class="sxs-lookup"><span data-stu-id="9299f-182">You'll make all calls to the API from that region.</span></span>

<span data-ttu-id="9299f-183">Vi har skapat ett Cognitive Services-konto för API för **ComputerVision**.</span><span class="sxs-lookup"><span data-stu-id="9299f-183">We've created a cognitive services account for the **ComputerVision** API.</span></span> <span data-ttu-id="9299f-184">Vi har valt *S1* SKU och har döpt vårt konto till **ComputerVisionService**.</span><span class="sxs-lookup"><span data-stu-id="9299f-184">We selected the *S1* sku and named our account **ComputerVisionService**.</span></span> <span data-ttu-id="9299f-185">Vårt konto ägs av resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>** och vi anropar API:et från den plats som vi angav i parametern `--location`.</span><span class="sxs-lookup"><span data-stu-id="9299f-185">Our account is owned by the resource group **<rgn>[sandbox resource group name]</rgn>** and we'll call the API from the location we set in the `--location` parameter.</span></span> 

<span data-ttu-id="9299f-186">När kommandot har slutfört skapandet av Cognitive Services-kontot, får du ett JSON-svar som innehåller egenskapen **provisioningState** inställd till **Lyckades**.</span><span class="sxs-lookup"><span data-stu-id="9299f-186">Once the command finishes creating the cognitive services account, you'll get a JSON response, which includes **provisioningState** property set to **Succeeded**.</span></span>

## <a name="get-an-access-key"></a><span data-ttu-id="9299f-187">Hämta en åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="9299f-187">Get an access key</span></span>

<span data-ttu-id="9299f-188">När kontot har skapats kan vi hämta prenumerationsnycklar eller åtkomstnycklar för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="9299f-188">Once we have our account successfully created we can retrieve the subscription keys, or access keys, for this account.</span></span>

1. <span data-ttu-id="9299f-189">Kör följande kommando i Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="9299f-189">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[sandbox resource group name]</rgn>
    ```
    
    <span data-ttu-id="9299f-190">Ovanstående kommando returnerar de nycklar som är associerade med Cognitive Services-kontot som kallas **ComputerVisionService** och ägs av den givna resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9299f-190">The above command returns the keys associated with the cognitive services account called **ComputerVisionService**, which is owned by the given resource group.</span></span> <span data-ttu-id="9299f-191">Den returnerar två nycklar – en är en reservnyckel.</span><span class="sxs-lookup"><span data-stu-id="9299f-191">It returns two keys - one is a spare key.</span></span> <span data-ttu-id="9299f-192">Nycklarna är svåra att komma ihåg, så vi ska spara den första nyckeln i en variabel som vi använder för alla anrop till API:et.</span><span class="sxs-lookup"><span data-stu-id="9299f-192">The keys are difficult to remember, so we'll store the first key in a variable that we'll use for all calls to the API.</span></span>

2.  <span data-ttu-id="9299f-193">Kör följande kommando i Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="9299f-193">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    key=$(az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query key1 -o tsv)
    ```
    
    <span data-ttu-id="9299f-194">Azure CLI 2.0 använder argumentet `--query` för att utföra en JMESPath-fråga på resultaten från kommandon.</span><span class="sxs-lookup"><span data-stu-id="9299f-194">The Azure CLI 2.0 uses the `--query` argument to execute a JMESPath query on the results of commands.</span></span> <span data-ttu-id="9299f-195">JMESPath är ett frågespråk för JSON som ger dig möjlighet att välja och visa data från CLI-utdata.</span><span class="sxs-lookup"><span data-stu-id="9299f-195">JMESPath is a query language for JSON, giving you the ability to select and present data from CLI output.</span></span> <span data-ttu-id="9299f-196">De här frågorna körs på JSON-utdata före visningsformatering.</span><span class="sxs-lookup"><span data-stu-id="9299f-196">These queries are executed on the JSON output before any display formatting.</span></span>
    <span data-ttu-id="9299f-197">Argumentet `--query` stöds av alla kommandon i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9299f-197">The `--query` argument is supported by all commands in the Azure CLI.</span></span> 
    
    <span data-ttu-id="9299f-198">I vårt exempel frågar vi listan med nycklar efter en post med namnet ”key1” och matar ut resultatet till **tsv**-format.</span><span class="sxs-lookup"><span data-stu-id="9299f-198">In our example, we query the list of keys for an entry named "key1" and output the result to **tsv** format.</span></span> <span data-ttu-id="9299f-199">Det här formatet tar bort citat runt strängvärdet.</span><span class="sxs-lookup"><span data-stu-id="9299f-199">This format removes quotations around the string value.</span></span> <span data-ttu-id="9299f-200">Vi tilldelar resultatet till en variabel **nyckel**.</span><span class="sxs-lookup"><span data-stu-id="9299f-200">We assign the result to a variable **key**.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="9299f-201">Vi ska använda den här nyckeln i modulen, så det är en bra idé att spara den i en variabel.</span><span class="sxs-lookup"><span data-stu-id="9299f-201">We're going to use this key throughout the module, so saving it in a variable is a good idea.</span></span> <span data-ttu-id="9299f-202">Om du förlorar värdet eller variabeln upphävs, kör du kommandot igen för att ställa in det.</span><span class="sxs-lookup"><span data-stu-id="9299f-202">If you lose the value or the variable becomes unset, run the command again to set it.</span></span>  

3. <span data-ttu-id="9299f-203">Visa värdet för vår nyckeln genom att köra följande kommando i Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="9299f-203">To see the value of our key, execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    echo $key
    ```

<span data-ttu-id="9299f-204">Nu när vi har ett konto och en nyckel, är det dags att göra några anrop till API:et.</span><span class="sxs-lookup"><span data-stu-id="9299f-204">Now that we have an account and a key, it's time to make some calls to the API.</span></span>