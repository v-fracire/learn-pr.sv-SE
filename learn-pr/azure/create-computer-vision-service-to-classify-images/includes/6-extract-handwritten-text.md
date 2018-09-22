Vi såg hur API för visuellt innehåll kan extrahera tryckt text från bilder. I den här övningen ska vi använda tjänsten för att identifiera handskriven text.

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a>Anropa API för visuellt innehåll för att extrahera handskriven text

Åtgärden `recognizeText` identifierar och extraherar handskriven text från anteckningar, brev, uppsatser, whiteboardtavlor, formulär och andra källor. Fråge-URL:en har följande format:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=<...>`

Alla anrop måste göras till den region där kontot skapades.

Om parametern `mode` finns måste den anges till `Handwritten` eller `Printed` och är skiftlägeskänslig. Om parametern är inställd till `Handwritten` eller inte är angiven, utförs handskriftsigenkänning. Om parametern är inställd till `Printed`, utförs textigenkänning av tryckt text. Den tid det tar att få ett resultat från det här anropet beror på mängden skrift i bilden.

I det här exemplet kommer vi att:

- Skriva ut svarshuvuden till konsolen med hjälp av cUrl:s alternativ `-D`
- Kopiera huvudvärdet `Operation-Location` från de rubriker som vi får i svaret
- Kontrollera efter några sekunder den URL som anges av `Operation-Location` för resultaten

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a>Identifiera och extrahera handskriven text från en bild

Vi använder följande bild i det här exemplet, men du kan testa samma kommando med URL:er till andra bilder.

![Bild av ett handskriftsexempel på en anteckning](../media/6-handwriting.jpg)

1. Kör följande kommandon i Azure Cloud Shell. Ersätt `<region>` i kommandot med ditt Cognitive Services-kontos region.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

Det ovanstående dumpar rubrikerna för den här åtgärden till konsolen. Här är ett exempel:

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

Rubriken `Operation-Location` är där resultaten kommer att publiceras när du är klar.

2. Kopiera huvudvärdet `Operation-Location`.
1. Kör följande kommando i Azure Cloud Shell som ersätter `"<Operation-Location>"` med värdet för rubriken **Åtgärd-Plats** som du kopierade i föregående steg.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

Om åtgärden har slutförts, får du en JSON-fil som innehåller resultatet av begäran av handskriftsigenkänningen.

Mer information om åtgärden `recognizeText` finns i referensdokumentationen [Identifiera handskriven text](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200).