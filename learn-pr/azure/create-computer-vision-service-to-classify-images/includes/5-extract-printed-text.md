<span data-ttu-id="9d77c-101">Anta att du nu vill läsa text från lagerbilderna som distributörerna i frontlinjen lägger upp på servern.</span><span class="sxs-lookup"><span data-stu-id="9d77c-101">Suppose you now want to read text from the stock images that your frontline distributors post to your server.</span></span> <span data-ttu-id="9d77c-102">I synnerhet vill du söka igenom produkten efter kampanjklistermärken som innehåller försäljningspriser.</span><span class="sxs-lookup"><span data-stu-id="9d77c-102">In particular, you want to scan product looking for promotional stickers that containing sale prices.</span></span> <span data-ttu-id="9d77c-103">Det är dags att testa funktionen optisk teckenläsning (OCR) i API för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="9d77c-103">It's time to try the optical character recognition (OCR) feature of the Computer Vision API.</span></span> 

## <a name="calling-the-computer-vision-api-to-extract-printed-text"></a><span data-ttu-id="9d77c-104">Anropar API för visuellt innehåll för att extrahera tryckt text</span><span class="sxs-lookup"><span data-stu-id="9d77c-104">Calling the Computer Vision API to extract printed text</span></span>

<span data-ttu-id="9d77c-105">Åtgärden `ocr` identifierar text i en bild och extraherar de tecken som identifieras i en teckenström som kan användas på en dator.</span><span class="sxs-lookup"><span data-stu-id="9d77c-105">The `ocr` operation detects text in an image and extracts the recognized characters into a machine-usable character stream.</span></span> <span data-ttu-id="9d77c-106">Fråge-URL:en har följande format:</span><span class="sxs-lookup"><span data-stu-id="9d77c-106">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr?language=<...>&detectOrientation=<...>`

<span data-ttu-id="9d77c-107">Alla anrop måste som vanligt göras till den region där kontot skapades.</span><span class="sxs-lookup"><span data-stu-id="9d77c-107">As usual, all calls must be made to the region where the account was created.</span></span> <span data-ttu-id="9d77c-108">Anropet godkänner två valfria parametrar:</span><span class="sxs-lookup"><span data-stu-id="9d77c-108">The call accepts two optional parameters:</span></span>

- <span data-ttu-id="9d77c-109">**språk**: Språkkoden för texten som ska identifieras i bilden.</span><span class="sxs-lookup"><span data-stu-id="9d77c-109">**language**: The language code of the text to be detected in the image.</span></span> <span data-ttu-id="9d77c-110">Standardvärdet är `unk` eller okänd.</span><span class="sxs-lookup"><span data-stu-id="9d77c-110">The default value is `unk`,or unknown.</span></span> <span data-ttu-id="9d77c-111">Det låter tjänsten automatisk identifiera textspråket i bilden.</span><span class="sxs-lookup"><span data-stu-id="9d77c-111">This let's the service auto detect the language of the text in the image.</span></span>
- <span data-ttu-id="9d77c-112">**detectOrientation**: När den är sann försöker tjänsten identifiera bildorienteringen och rätta till den innan ytterligare bearbetning, till exempel om avbildningen är upp och ned eller inte.</span><span class="sxs-lookup"><span data-stu-id="9d77c-112">**detectOrientation**: When true, the service  tries to detect the image orientation and correct it before further processing, for example, whether the image is upside-down.</span></span> 

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="extract-printed-text-from-an-image-using-ocr"></a><span data-ttu-id="9d77c-113">Extrahera tryckt text från en bild med OCR</span><span class="sxs-lookup"><span data-stu-id="9d77c-113">Extract printed text from an image using OCR</span></span>

<span data-ttu-id="9d77c-114">Bilden som vi ska använda för den optiska teckenläsningen (OCR) är ett omslag från boken *.NET Microservices: Architecture for Containerized .NET Applications*.</span><span class="sxs-lookup"><span data-stu-id="9d77c-114">The image we're going to be using for Optical Character Recognition (OCR) is the cover from the book *.NET Microservices: Architecture for Containerized .NET Applications*.</span></span>

![Bild av omslaget från e-boken .NET Microservices: Architecture for Containerized .NET Applications](../media/5-ebook.png)

1. <span data-ttu-id="9d77c-116">Kör följande kommando i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="9d77c-116">Execute the following command in Cloud Shell.</span></span> <span data-ttu-id="9d77c-117">Ersätt `<region>` i kommandot med ditt Cognitive Services-kontos region.</span><span class="sxs-lookup"><span data-stu-id="9d77c-117">Replace `<region>` in the command with the region of your cognitive services account.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

<span data-ttu-id="9d77c-118">Följande JSON är ett exempel på svaret som vi får från det här anropet.</span><span class="sxs-lookup"><span data-stu-id="9d77c-118">The following JSON is an example of the response we get from this call.</span></span> <span data-ttu-id="9d77c-119">Vissa rader av JSON har tagits bort för att kodavsnittet ska passa bättre på sidan.</span><span class="sxs-lookup"><span data-stu-id="9d77c-119">Some lines of JSON have been removed to make the snippet fit better on the page.</span></span>

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": 0,
  "regions" : [
        /*... snipped*/
        {
          "boundingBox": "766,1419,302,33",
          "words": [
            {
              "boundingBox": "766,1419,126,25",
              "text": "Microsoft"
            },
            {
              "boundingBox": "903,1420,165,32",
              "text": "Corporation"
            }
          ]
        }]
}
```

<span data-ttu-id="9d77c-120">Låt oss nu undersöka detta svar i detalj.</span><span class="sxs-lookup"><span data-stu-id="9d77c-120">Let's examine this response in more detail.</span></span> 

- <span data-ttu-id="9d77c-121">Tjänsten har identifierat texten som engelska.</span><span class="sxs-lookup"><span data-stu-id="9d77c-121">The service identified the text as being English.</span></span> <span data-ttu-id="9d77c-122">Värdet för fältet `language` innehåller språkkoden BCP-47 för den text som har identifierats i bilden.</span><span class="sxs-lookup"><span data-stu-id="9d77c-122">The value of the `language` field contains the BCP-47 language code of the text detected in the image.</span></span> <span data-ttu-id="9d77c-123">I det här exemplet är det på **en**, engelska.</span><span class="sxs-lookup"><span data-stu-id="9d77c-123">In this example it is **en**, or English.</span></span> 
- <span data-ttu-id="9d77c-124">`orientation` identifierades som **upp**.</span><span class="sxs-lookup"><span data-stu-id="9d77c-124">The `orientation` was detected as **up**.</span></span> <span data-ttu-id="9d77c-125">Den här egenskapen är riktningen överst av den identifierade texten efter att bilden har roterats runt mitten enligt den definierade textvinkeln.</span><span class="sxs-lookup"><span data-stu-id="9d77c-125">This property is the direction that the top of the recognized text is facing, after the image has been rotated around its center according to the detected text angle.</span></span> 
- <span data-ttu-id="9d77c-126">`textAngle` är vinkeln som texten måste roteras för att bli horisontell eller vertikal.</span><span class="sxs-lookup"><span data-stu-id="9d77c-126">The `textAngle` is the angle by which the text must be rotated to become horizontal or vertical.</span></span> <span data-ttu-id="9d77c-127">I det här exemplet var texten perfekt horisontell, så det värde som returneras är **0**.</span><span class="sxs-lookup"><span data-stu-id="9d77c-127">In this example, the text was perfectly horizontal, so the value returned is **0**.</span></span>  
- <span data-ttu-id="9d77c-128">`regions`-egenskaperna innehåller en lista med värden som visar var texten är, dess läge i bilden och orden som hittas i den delen av bilden.</span><span class="sxs-lookup"><span data-stu-id="9d77c-128">The `regions` property contains a list of values used to show where the text is, its position in the picture, and the word found in that part of the image.</span></span> 
- <span data-ttu-id="9d77c-129">De fyra heltalen av `boundingBox`-värdet är:</span><span class="sxs-lookup"><span data-stu-id="9d77c-129">The four integers of the `boundingBox` value are:</span></span> 
    - <span data-ttu-id="9d77c-130">x-koordinaten för den vänstra kanten</span><span class="sxs-lookup"><span data-stu-id="9d77c-130">the x-coordinate of the left edge</span></span> 
    - <span data-ttu-id="9d77c-131">x-koordinaten för den övre kanten</span><span class="sxs-lookup"><span data-stu-id="9d77c-131">the y-coordinate of the top edge</span></span>
    - <span data-ttu-id="9d77c-132">bredden på avgränsningsfältet</span><span class="sxs-lookup"><span data-stu-id="9d77c-132">the width of the bounding box</span></span>
    - <span data-ttu-id="9d77c-133">höjden på avgränsningsfältet,</span><span class="sxs-lookup"><span data-stu-id="9d77c-133">the height of the bounding box,</span></span> 
   
    <span data-ttu-id="9d77c-134">Du kan använda dessa värden för att rita en ruta runt varje del av texten i bilden.</span><span class="sxs-lookup"><span data-stu-id="9d77c-134">You can use these values to draw boxes around every piece of text found in the image.</span></span>

<span data-ttu-id="9d77c-135">Som du ser i den här övningen ger tjänsten `ocr` detaljerad information om tryckt text i en bild.</span><span class="sxs-lookup"><span data-stu-id="9d77c-135">As you can see in this exercise, the `ocr` service gives detailed information about the printed text in an image.</span></span> 

<span data-ttu-id="9d77c-136">Mer information om åtgärden `ocr` finns i referensdokumentationen [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc).</span><span class="sxs-lookup"><span data-stu-id="9d77c-136">For more information about the `ocr` operation, see the [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) reference documentation.</span></span>