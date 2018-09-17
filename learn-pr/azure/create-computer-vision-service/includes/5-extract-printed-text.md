I den här kursdelen extraherar du text från en bild med tjänsten API för visuellt innehåll som vi skapade tidigare.

# <a name="extracting-the-text-from-an-image"></a>Extrahera texten från en bild

Hämta en nyckel som används för att autentisera mot API genom att köra kommandot `az cognitiveservices account keys list`. Lagra utdata från kommandot inom variabeln `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Kör kommandot `curl`, så görs en HTTP-begäran mot API för visuellt innehåll. Återanvänd den nyss deklarerade variabeln `key`.

Bilden som vi ska använda för den optiska teckenläsningen (OCR) är ett omslag från boken [.NET Microservices: Architecture for Containerized .NET Applications](/dotnet/standard/microservices-architecture/).

![E-bokens omslag](../images/ebook.png)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/ocr" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/ebook.png\"}" | jq '.'
```

De första tre egenskaperna är språk, orientering och textvinkel. Efter det innehåller `regions`-egenskaperna en lista med värden som visar var texten är, dess läge i bilden och orden den innehåller.

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

Experimentera med andra bilder och få andra resultat genom att ändra URL-adressen som användes i exemplet ovan.