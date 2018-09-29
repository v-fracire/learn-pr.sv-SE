<span data-ttu-id="e6319-101">Vi såg hur API för visuellt innehåll kan extrahera tryckt text från bilder.</span><span class="sxs-lookup"><span data-stu-id="e6319-101">We saw how the Computer Vision API can extract printed text from images.</span></span> <span data-ttu-id="e6319-102">I den här övningen ska vi använda tjänsten för att identifiera handskriven text.</span><span class="sxs-lookup"><span data-stu-id="e6319-102">In this exercise, we'll use the service to detect handwritten text.</span></span>

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a><span data-ttu-id="e6319-103">Anropa API för visuellt innehåll för att extrahera handskriven text</span><span class="sxs-lookup"><span data-stu-id="e6319-103">Calling the Computer Vision API to extract handwritten text</span></span>

<span data-ttu-id="e6319-104">Åtgärden `recognizeText` identifierar och extraherar handskriven text från anteckningar, brev, uppsatser, whiteboardtavlor, formulär och andra källor.</span><span class="sxs-lookup"><span data-stu-id="e6319-104">The `recognizeText` operation detects and extracts handwritten text from notes, letters, essays, whiteboards, forms, and other sources.</span></span> <span data-ttu-id="e6319-105">Fråge-URL:en har följande format:</span><span class="sxs-lookup"><span data-stu-id="e6319-105">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=<...>`

<span data-ttu-id="e6319-106">Alla anrop måste göras till den region där kontot skapades.</span><span class="sxs-lookup"><span data-stu-id="e6319-106">All calls must be made to the region where the account was created.</span></span>

<span data-ttu-id="e6319-107">Om parametern `mode` finns måste den anges till `Handwritten` eller `Printed` och är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="e6319-107">If present, the `mode` parameter must be set to `Handwritten` or `Printed` and is case-sensitive.</span></span> <span data-ttu-id="e6319-108">Om parametern är inställd till `Handwritten` eller inte är angiven, utförs handskriftsigenkänning.</span><span class="sxs-lookup"><span data-stu-id="e6319-108">If the parameter is set to `Handwritten` or is not specified, handwriting recognition is performed.</span></span> <span data-ttu-id="e6319-109">Om parametern är inställd till `Printed`, utförs textigenkänning av tryckt text.</span><span class="sxs-lookup"><span data-stu-id="e6319-109">If the parameter is set to `Printed`, then printed text recognition is performed.</span></span> <span data-ttu-id="e6319-110">Den tid det tar att få ett resultat från det här anropet beror på mängden skrift i bilden.</span><span class="sxs-lookup"><span data-stu-id="e6319-110">The time it takes to get a result from this call depends on the amount of writing in the image.</span></span>

<span data-ttu-id="e6319-111">I det här exemplet kommer vi att:</span><span class="sxs-lookup"><span data-stu-id="e6319-111">In this example, we'll:</span></span>

- <span data-ttu-id="e6319-112">Skriva ut svarshuvuden till konsolen med hjälp av cUrl:s alternativ `-D`</span><span class="sxs-lookup"><span data-stu-id="e6319-112">Print the response headers to the console using cUrl's `-D` option</span></span>
- <span data-ttu-id="e6319-113">Kopiera huvudvärdet `Operation-Location` från de rubriker som vi får i svaret</span><span class="sxs-lookup"><span data-stu-id="e6319-113">Copy the `Operation-Location` header value from the headers we receive in the response</span></span>
- <span data-ttu-id="e6319-114">Kontrollera efter några sekunder den URL som anges av `Operation-Location` för resultaten</span><span class="sxs-lookup"><span data-stu-id="e6319-114">After a few seconds, check the URL specified by the `Operation-Location` for the results</span></span>

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a><span data-ttu-id="e6319-115">Identifiera och extrahera handskriven text från en bild</span><span class="sxs-lookup"><span data-stu-id="e6319-115">Detect and extract handwritten text from an image</span></span>

<span data-ttu-id="e6319-116">Vi använder följande bild i det här exemplet, men du kan testa samma kommando med URL:er till andra bilder.</span><span class="sxs-lookup"><span data-stu-id="e6319-116">We'll use the following image in this example, but you're free to try the same command with URLs to other images.</span></span>

![Bild av ett handskriftsexempel på en anteckning](../media/6-handwriting.jpg)

1. <span data-ttu-id="e6319-118">Kör följande kommandon i Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="e6319-118">Execute the following commands in Azure Cloud Shell.</span></span> <span data-ttu-id="e6319-119">Ersätt `<region>` i kommandot med ditt Cognitive Services-kontos region.</span><span class="sxs-lookup"><span data-stu-id="e6319-119">Replace `<region>` in the command with the region of your cognitive services account.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

<span data-ttu-id="e6319-120">Det ovanstående dumpar rubrikerna för den här åtgärden till konsolen.</span><span class="sxs-lookup"><span data-stu-id="e6319-120">The above dumps the headers of this operation to the console.</span></span> <span data-ttu-id="e6319-121">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="e6319-121">Here's an example:</span></span>

```azurecli
HTTP/1.1 202 Accepted
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Expires: -1
Operation-Location: https://westus2.api.cognitive.microsoft.com/vision/v2.0/textOperations/d0e9b397-4072-471c-ae61-7490bec8f077
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
apim-request-id: f5663487-03c6-4760-9be7-c9157fac10a1
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Date: Wed, 12 Sep 2018 19:22:00 GMT
```

<span data-ttu-id="e6319-122">Rubriken `Operation-Location` är där resultaten kommer att publiceras när du är klar.</span><span class="sxs-lookup"><span data-stu-id="e6319-122">The `Operation-Location` header is where the results will be posted once complete.</span></span>

2. <span data-ttu-id="e6319-123">Kopiera huvudvärdet `Operation-Location`.</span><span class="sxs-lookup"><span data-stu-id="e6319-123">Copy the `Operation-Location` header value.</span></span>
1. <span data-ttu-id="e6319-124">Kör följande kommando i Azure Cloud Shell som ersätter `"<Operation-Location>"` med värdet för rubriken **Åtgärd-Plats** som du kopierade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e6319-124">Execute the following command in Azure Cloud Shell replacing `"<Operation-Location>"` with the value for the **Operation-Location** header you copied from the preceding step.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

<span data-ttu-id="e6319-125">Om åtgärden har slutförts, får du en JSON-fil som innehåller resultatet av begäran av handskriftsigenkänningen.</span><span class="sxs-lookup"><span data-stu-id="e6319-125">If the operation has completed, you'll receive a JSON file containing the result of the handwriting recognition request.</span></span>

<span data-ttu-id="e6319-126">Mer information om åtgärden `recognizeText` finns i referensdokumentationen [Identifiera handskriven text](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200).</span><span class="sxs-lookup"><span data-stu-id="e6319-126">For more information about the `recognizeText` operation, see the [Recognize Handwritten Text](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) reference documentation.</span></span>