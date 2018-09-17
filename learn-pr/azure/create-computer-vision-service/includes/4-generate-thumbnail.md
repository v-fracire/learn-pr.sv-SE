I den här kursdelen genererar du miniatyrer från en källbild med hjälp av tjänsten API för visuellt innehåll som vi skapade tidigare.

# <a name="generate-a-thumbnail-from-an-image-with-computer-vision-api"></a>Generera en miniatyr från en bild med hjälp av API för visuellt innehåll

Hämta en nyckel som används för att autentisera mot API genom att köra kommandot `az cognitiveservices account keys list`. Lagra utdata från kommandot inom variabeln `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Kör kommandot `curl`, så görs en HTTP-begäran mot API för visuellt innehåll. Återanvänd den nyss deklarerade variabeln `key`.

Genom att ge olika parametrar till API:t kan du generera den miniatyr du behöver. `width` och `height` krävs och anger för API:t vilken storlek du behöver för en viss bild. Avslutningsvis genererar parametern `smartCropping` en smartare beskärning genom att analysera det intressantaste området i bilden och behålla det i miniatyren. När smart beskärning är aktiverat behålls till exempel ansiktet i en beskuren profilbild inom bildramen fastän bilden inte har samma proportioner som den vi efterfrågade.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" -o clouddrive/thumbnail.jpg
```

# <a name="downloading-the-thumbnail"></a>Ladda ned miniatyren

Du hittar den genererade miniatyren i ditt Azure Cloud Shell-lagringskonto i en resursgrupp med namnet `cloud-shell-storage-<region>`.

1. Öppna det automatiskt genererade lagringskontot.

![image](../images/storage-account.png)

2. Klicka på avsnittet för filer.

![image](../images/storage-account-click-on-files.png)

3. Du hittar miniatyren i roten för containern.

![image](../images/storage-account-thumbnail.png)

4. Klicka på filen och ladda sedan ned den.

I mappen med nedladdade filer kan du öppna bilden med `100x100` bildpunkter med valfritt bildvisningsprogram.