Som en chefsutvecklare på Contoso dryck Distribution du ansvarar för att bygga och underhålla en line-of-business-app som gör att din främsta distributörer skanna och ladda upp bilder av affärerna där de återinsättning. 

Du vill verifiera att alla avbildningar som publicerats av användare följer med innehåll regler från ditt företag. Företaget vill inte olämpligt innehåll som publiceras på företagets webbplatser. Angivet förbättringarna i artificiell intelligens, som du vill använda den API för visuellt innehåll i din app. 

Börja skapa ett visuellt-konto i din Azure-prenumeration och börja testa den **analysera** funktionen.

## <a name="calling-the-computer-vision-api-to-analyze-images"></a>Anropa de API för visuellt innehåll för att analysera bilder

Den `analyze` åtgärden extraherar en omfattande uppsättning visuella funktioner baserat på innehållet i.  Fråge-URL har följande format:

`https://[location].api.cognitive.microsoft.com/vision/v2.0/analyze[?visualFeatures][&details][&language] `

De tre parametrarna som skickas till tjänsten är: `visualFeatures`, `details` och `languages`. Ange den `details` parameter `Landmarks` eller `Celebrities` för att identifiera landmärken eller kändisar. `visualFeatures` identifierar vilken slags information du vill att tjänsten ska returnera åt dig. Den `Categories` alternativet klassificerar innehållet i bilder som träd, byggnader och mer. `Faces` identifierar människors ansikten och ger dig deras kön och ålder.

Alla åtgärder på den API för visuellt innehåll har följande begränsningar när det gäller de avbildningar du be den kan behandla:

- Bildformat som stöds: JPEG, PNG, GIF, BMP. 
- Bildens filstorlek måste vara mindre än 4 MB.
- Bilddimensioner måste vara minst 50 x 50.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a>Identifiera landmärken i en bild

Låt oss börja med ett anrop till hitta landmärken i en bild. Vi använder följande bild för det här exemplet, men du är kostnadsfritt att testa samma kommando med URL: er till andra avbildningar. 

![Bild av flera mountain med blå skies ovan snow-capped bergen.](../media/3-mountains.jpg)

1. Kör följande kommando i Azure Cloud Shell:

<!-- TODO Replace image URL with one that points to an image in the sample repo -->
```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

Det här anropet söker efter landmärken i avbildningen som anges med bild-URL. Anropet uppmanas även tjänsten för att returnera information om programvarukategorier och en beskrivning av avbildningen. Beskrivningen returneras som en hel engelska mening. Som du vet behöver en åtkomstnyckel för varje anrop till API: et. Detta är inställt i varje anrop med den `Ocp-Apim-Subscription-Key` rubrik. 

> [!TIP]
> Resultatet av begäran är JSON-rådatafilen som har en beskrivning av bilden i sin `url`. Vi har lagt till ` | jq '.'` i slutet av kommandot för att prettify JSON-utdata.

JSON-svar i det här exemplet returnerar:

- En `categories` matris som listar alla kategorier för avbildningen som har identifierats, tillsammans med en poäng mellan 0 och 1 av hur säker tjänsten är att avbildningen tillhör den angivna kategorin.
- En `description` post som innehåller en matris med taggar eller ord som är relaterade till avbildningen.
- En `captions` post med ett textfält som beskriver på engelska Nyheter i avbildningen. Observera att texten också har en poäng för säkerhet. Det här resultatet kan hjälpa dig att avgöra vad du ska göra med den här analysen.


## <a name="check-for-inappropriate-content-in-an-image"></a>Sök efter olämpligt innehåll i en bild

I det här exemplet ska vi analysera en bild för vuxet innehåll. Förtroendepoäng bedömer sannolikheten att bilden innehåller vuxet eller olämpligt innehåll. 

Vi använder följande bild för det här exemplet, men du är kostnadsfritt att testa samma kommando med URL: er till andra avbildningar. 

![Bild av en Leende familj.](../media/3-people.png)

1. Kör följande kommando i Azure Cloud Shell:

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

I det här exemplet anger vi den `visualFeatures` till `Adult,Description`. Svaret ger oss två förtroende poäng, en för olämpligt innehåll och en för vuxet innehåll. Med dessa poäng plus avbildningsbeskrivningen och andra visuella funktioner kan börja du flaggan bilder som ska publiceras till servern.

De exempel som vi försökte i den här övningen ger dig en uppfattning om vilken typ av analys som du kan göra med den `analyze` igen. Prova analys med olika bilder och prova olika kombinationer av visualFeatures, information och språk.

Mer information om den `analyze` åtgärden, finns i den [analysera bild](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) referensdokumentation.