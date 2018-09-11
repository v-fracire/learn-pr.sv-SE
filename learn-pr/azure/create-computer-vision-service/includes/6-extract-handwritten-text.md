I den här kursdelen kommer du att extrahera handskrift från en bild med tjänsten API för visuellt innehåll som vi skapade tidigare.

# <a name="extracting-the-hand-writing--from-an-image"></a>Extrahera handskriften från en bild

Hämta en nyckel som används för att autentisera mot API genom att köra kommandot `az cognitiveservices account keys list`. Lagra utdata från kommandot inom variabeln `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Kör kommandot `curl`, så görs en HTTP-begäran mot API för visuellt innehåll. Återanvänd den nyss deklarerade variabeln `key`.

Vi använder en bild av den här anslagstavlan som bild för igenkänningen av handskrift.

![Handskrift](../images/handwriting.jpg)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/handwriting.jpg\"}" -D -
```

Kommandot ovan genererar huvudena. Kopiera huvudvärdet `Operation-Location` till kommandot nedan.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>"
```

Du får nu en JSON-fil som innehåller resultatet av begäran om handskriftsigenkänning.

Experimentera med andra bilder och få andra resultat genom att ändra URL-adressen som användes i exemplet ovan.