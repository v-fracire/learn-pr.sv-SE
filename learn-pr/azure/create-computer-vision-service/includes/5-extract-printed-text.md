<span data-ttu-id="1080f-101">I den här kursdelen extraherar du text från en bild med tjänsten API för visuellt innehåll som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="1080f-101">In this unit, you will extract text from an image with the Computer Vision API service that we created previously.</span></span>

# <a name="extracting-the-text-from-an-image"></a><span data-ttu-id="1080f-102">Extrahera texten från en bild</span><span class="sxs-lookup"><span data-stu-id="1080f-102">Extracting the text from an image</span></span>

<span data-ttu-id="1080f-103">Hämta en nyckel som används för att autentisera mot API genom att köra kommandot `az cognitiveservices account keys list`.</span><span class="sxs-lookup"><span data-stu-id="1080f-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="1080f-104">Lagra utdata från kommandot inom variabeln `key`.</span><span class="sxs-lookup"><span data-stu-id="1080f-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="1080f-105">Kör kommandot `curl`, så görs en HTTP-begäran mot API för visuellt innehåll. Återanvänd den nyss deklarerade variabeln `key`.</span><span class="sxs-lookup"><span data-stu-id="1080f-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reusing the previously declared variable `key`.</span></span>

<span data-ttu-id="1080f-106">Bilden som vi ska använda för den optiska teckenläsningen (OCR) är ett omslag från boken [.NET Microservices: Architecture for Containerized .NET Applications](/dotnet/standard/microservices-architecture/).</span><span class="sxs-lookup"><span data-stu-id="1080f-106">The image we're going to be using for Optical Character Recognition (OCR) is the cover from the book [.NET Microservices: Architecture for Containerized .NET Applications](/dotnet/standard/microservices-architecture/).</span></span>

![E-bokens omslag](../images/ebook.png)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/ocr" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/ebook.png\"}" | jq '.'
```

<span data-ttu-id="1080f-108">De första tre egenskaperna är språk, orientering och textvinkel.</span><span class="sxs-lookup"><span data-stu-id="1080f-108">The first three properties are the language, orientation as well as the text angle.</span></span> <span data-ttu-id="1080f-109">Efter det innehåller `regions`-egenskaperna en lista med värden som visar var texten är, dess läge i bilden och orden den innehåller.</span><span class="sxs-lookup"><span data-stu-id="1080f-109">Then, the `regions` properties will contain a list of values used to show where the text is, it's position in the picture and the actual words it contains.</span></span>

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

<span data-ttu-id="1080f-110">Experimentera med andra bilder och få andra resultat genom att ändra URL-adressen som användes i exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="1080f-110">Experiment with other images to get different result by changing the URL used in the sample above.</span></span>