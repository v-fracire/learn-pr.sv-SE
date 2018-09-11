I den här kursdelen analyserar du bilder med tjänsten API för visuellt innehåll som vi skapade i föregående steg.

# <a name="analyzing-an-image-with-computer-vision-api"></a>Analysera en bild med API för visuellt innehåll

Hämta en nyckel som används för att autentisera mot API genom att köra kommandot `az cognitiveservices account keys list`. Lagra utdata från kommandot inom variabeln `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Kör kommandot `curl`, så görs en HTTP-begäran mot API för visuellt innehåll. Återanvänd den nyss deklarerade variabeln `key`.

De tre parametrarna som skickas till tjänsten är: `visualFeatures`, `details` och `languages`. Tjänsten kan fortfarande fungera utan dem, men de hjälper tjänsten att förstå bildernas innehåll. Till exempel kan `details` anges för `Landmarks` eller `Celebrities` och hjälpa dig att identifiera sevärdheter eller kändisar. `visualFeatures` identifierar vilken slags information du vill att tjänsten ska returnera åt dig. `Categories`-alternativet kategoriserar bildernas innehåll, till exempel träd, byggnader med mera. `Faces` identifierar människors ansikten och ger dig deras kön och ålder.

Mer information finns i [dokumentationen](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa).

För tillfället ska vi identifiera sevärdheter och låta tjänsten returnera `Categories` och `Description`.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}"
```

Resultatet av begäran är JSON-rådatafilen som har en beskrivning av bilden i sin `url`. Vi kan också lägga till ` | jq '.'` på slutet om vi vill göra JSON-utdatan vackrare.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" | jq '.'
```

Kopiera länken till valfri bild online och försök använda den med tjänsten genom att ersätta URL-adressen i kommandona ovan.