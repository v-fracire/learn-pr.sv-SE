Vi se hur den API för visuellt innehåll extraherar tryckt text från bilder. I den här övningen använder vi tjänsten för att identifiera handskriven text.

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a>Anropa de API för visuellt innehåll för att extrahera handskriven text

Den `recognizeText` åtgärden identifierar och extraherar handskriven text från anteckningar, brev, uppsatser, whiteboardtavlor, formulär och andra källor. Fråge-URL har följande format:

`https://[location].api.cognitive.microsoft.com/vision/v2.0/recognizeText[?mode] `

Som vanligt måste alla anrop göras till den plats där konton som har skapats. Om den `mode` parametern måste anges till `Handwritten` eller `Printed` och är skiftlägeskänsligt. Om parametern är inställd `Handwritten` eller är inte anges handskriftsigenkänning utförs. Annars kan du ange parametern `Printed` utskrivna textigenkänning utförs genom att anropa OCR-åtgärden.

Den tid det tar för att få ett resultat från det här anropet beror på mängden handskrift i avbildningen. Därför kan vi behöva vänta några sekunder. I det här exemplet ska vi därför:

- Dumpa svarshuvuden till konsolen med Curl's `-D` alternativet
- Kopiera den `Operation-Location` huvudvärde från de rubriker som vi får i svaret
- Efter några sekunder att kontrollera den URL som anges av den `Operation-Location` för resultat

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a>Identifiera och extrahera handskriven text från en bild

Vi använder följande bild i det här exemplet, men du är kostnadsfritt att testa samma kommando med URL: er till andra avbildningar.

![Bild av ett handskrift exempel på en anteckning](../media/6-handwriting.jpg)

1. Kör följande kommandon i Azure Cloud Shell:

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

Ovanstående Dumpar rubrikerna för den här åtgärden till konsolen. Här är ett exempel:

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

Den `Operation-Location` rubriken är där resultatet kommer att publiceras när du är klar.

2. Kopiera den `Operation-Location` huvudets värde.
1. Kör följande kommando i Azure Cloud Shell ersätta `"<Operation-Location>"` med värdet för den **åtgärden plats** rubrik som du kopierade i föregående steg.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

Om åtgärden har slutförts, får du en JSON-fil som innehåller resultatet av handskrift igenkänning av begäran.

Mer information om den `recognizeText` åtgärden, finns i den [identifiera handskriven Text](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) referensdokumentation.