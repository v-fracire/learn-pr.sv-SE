I den här kursdelen kommer du att extrahera handskrift från en bild med tjänsten API för visuellt innehåll som vi skapade tidigare.

# <a name="extracting-the-handwriting-from-an-image"></a>Extrahera handskrift från en bild

Genom att köra kommandot `az cognitiveservices account keys list` hämtas en nyckel som används för att autentisera mot API:n. Lagra utdata från kommandot inom variabeln `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

När du kör kommandot `curl` görs en HTTP-begäran till API för visuellt innehåll. Återanvänd därefter den nyss deklarerade variabeln `key`.

Vi använder en bild av den här anslagstavlan för igenkänningen av handskrift.

![Handskrift](../images/handwriting.jpg)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/handwriting.jpg\"}" -D -
```

Kommandot ovan genererar huvudena. Kopiera huvudvärdet `Operation-Location` till kommandot nedan.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>"
```

Du får nu en JSON-fil som innehåller resultatet av din begäran om handskriftsigenkänning.

Experimentera med andra bilder och få andra resultat genom att ändra URL-adressen som användes i exemplet ovan.