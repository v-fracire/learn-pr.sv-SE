<span data-ttu-id="5bcd8-101">I den här kursdelen kommer du att extrahera handskrift från en bild med tjänsten API för visuellt innehåll som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-101">In this unit, you will extract handwriting from an image with the Computer Vision API service that we created previously.</span></span>

# <a name="extracting-the-handwriting-from-an-image"></a><span data-ttu-id="5bcd8-102">Extrahera handskrift från en bild</span><span class="sxs-lookup"><span data-stu-id="5bcd8-102">Extracting the handwriting from an image</span></span>

<span data-ttu-id="5bcd8-103">Genom att köra kommandot `az cognitiveservices account keys list` hämtas en nyckel som används för att autentisera mot API:n.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="5bcd8-104">Lagra utdata från kommandot inom variabeln `key`.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="5bcd8-105">När du kör kommandot `curl` görs en HTTP-begäran till API för visuellt innehåll. Återanvänd därefter den nyss deklarerade variabeln `key`.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.</span></span>

<span data-ttu-id="5bcd8-106">Vi använder en bild av den här anslagstavlan för igenkänningen av handskrift.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-106">The image we're going to be using for handwriting recognition is a picture of this note board.</span></span>

![Handskrift](../images/handwriting.jpg)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/handwriting.jpg\"}" -D -
```

<span data-ttu-id="5bcd8-108">Kommandot ovan genererar huvudena.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-108">The command above will output the headers.</span></span> <span data-ttu-id="5bcd8-109">Kopiera huvudvärdet `Operation-Location` till kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-109">Copy the `Operation-Location` header value into the command below.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>"
```

<span data-ttu-id="5bcd8-110">Du får nu en JSON-fil som innehåller resultatet av din begäran om handskriftsigenkänning.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-110">You will now receive a JSON file containing the result of the handwriting recognition request.</span></span>

<span data-ttu-id="5bcd8-111">Experimentera med andra bilder och få andra resultat genom att ändra URL-adressen som användes i exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="5bcd8-111">Experiment with other images to get different results by changing the URL used in the sample above.</span></span>