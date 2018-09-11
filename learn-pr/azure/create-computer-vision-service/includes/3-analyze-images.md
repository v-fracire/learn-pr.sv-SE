<span data-ttu-id="2c65a-101">I den här kursdelen analyserar du bilder med tjänsten API för visuellt innehåll som vi skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="2c65a-101">In this unit, you will analyze images with the Computer Vision API service that we created in the previous step.</span></span>

# <a name="analyzing-an-image-with-computer-vision-api"></a><span data-ttu-id="2c65a-102">Analysera en bild med API för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="2c65a-102">Analyzing an image with Computer Vision API</span></span>

<span data-ttu-id="2c65a-103">Hämta en nyckel som används för att autentisera mot API genom att köra kommandot `az cognitiveservices account keys list`.</span><span class="sxs-lookup"><span data-stu-id="2c65a-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="2c65a-104">Lagra utdata från kommandot inom variabeln `key`.</span><span class="sxs-lookup"><span data-stu-id="2c65a-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="2c65a-105">Kör kommandot `curl`, så görs en HTTP-begäran mot API för visuellt innehåll. Återanvänd den nyss deklarerade variabeln `key`.</span><span class="sxs-lookup"><span data-stu-id="2c65a-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reusing the previously declared variable `key`.</span></span>

<span data-ttu-id="2c65a-106">De tre parametrarna som skickas till tjänsten är: `visualFeatures`, `details` och `languages`.</span><span class="sxs-lookup"><span data-stu-id="2c65a-106">The parameters that are sent to the service are `visualFeatures`, `details`, and `languages`.</span></span> <span data-ttu-id="2c65a-107">Tjänsten kan fortfarande fungera utan dem, men de hjälper tjänsten att förstå bildernas innehåll.</span><span class="sxs-lookup"><span data-stu-id="2c65a-107">While without them, the service will still work, it provides hints to the service on the content of the images.</span></span> <span data-ttu-id="2c65a-108">Till exempel kan `details` anges för `Landmarks` eller `Celebrities` och hjälpa dig att identifiera sevärdheter eller kändisar.</span><span class="sxs-lookup"><span data-stu-id="2c65a-108">As an example, `details` can be set to `Landmarks` or `Celebrities` to help you identify landmark or celebrities.</span></span> <span data-ttu-id="2c65a-109">`visualFeatures` identifierar vilken slags information du vill att tjänsten ska returnera åt dig.</span><span class="sxs-lookup"><span data-stu-id="2c65a-109">`visualFeatures` identify what kind of information you want the service to return you.</span></span> <span data-ttu-id="2c65a-110">`Categories`-alternativet kategoriserar bildernas innehåll, till exempel träd, byggnader med mera.</span><span class="sxs-lookup"><span data-stu-id="2c65a-110">`Categories` option will categorize the content of the images like trees, building, and more.</span></span> <span data-ttu-id="2c65a-111">`Faces` identifierar människors ansikten och ger dig deras kön och ålder.</span><span class="sxs-lookup"><span data-stu-id="2c65a-111">`Faces` will identify people's faces and give you their gender and age.</span></span>

<span data-ttu-id="2c65a-112">Mer information finns i [dokumentationen](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa).</span><span class="sxs-lookup"><span data-stu-id="2c65a-112">More details can be found in [the documentation](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa).</span></span>

<span data-ttu-id="2c65a-113">För tillfället ska vi identifiera sevärdheter och låta tjänsten returnera `Categories` och `Description`.</span><span class="sxs-lookup"><span data-stu-id="2c65a-113">For now, let's identify landmarks and have the service return us `Categories` and `Description`.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}"
```

<span data-ttu-id="2c65a-114">Resultatet av begäran är JSON-rådatafilen som har en beskrivning av bilden i sin `url`.</span><span class="sxs-lookup"><span data-stu-id="2c65a-114">The result of the request is the raw JSON describing the picture in the `url`.</span></span> <span data-ttu-id="2c65a-115">Vi kan också lägga till ` | jq '.'` på slutet om vi vill göra JSON-utdatan vackrare.</span><span class="sxs-lookup"><span data-stu-id="2c65a-115">We can also add ` | jq '.'` at the end to prettify the JSON output.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" | jq '.'
```

<span data-ttu-id="2c65a-116">Kopiera länken till valfri bild online och försök använda den med tjänsten genom att ersätta URL-adressen i kommandona ovan.</span><span class="sxs-lookup"><span data-stu-id="2c65a-116">Copy the link to any other images found online and try it with the service by replacing the URL in the above commands.</span></span>