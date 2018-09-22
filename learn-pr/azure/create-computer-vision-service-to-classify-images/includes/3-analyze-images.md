Som en chefsutvecklare på Contoso Beverage Distribution är du ansvarig för att bygga och underhålla en verksamhetsspecifik app som låter dina distributörer skanna och överföra bilder av butikshyllor där de fyller på lagren. 

Du vill verifiera att alla bilder som publicerats av användare följer de innehållsregler som ditt företag har tagit fram. Företaget vill inte att olämpligt innehåll publiceras på företagets webbplatser. Med tanke på framstegen i Artificial Intelligence bestämmer du dig för att använda API för visuellt innehåll i din app. 

För att komma igång skapar du ett konto för visuellt innehåll i din Azure-prenumeration och börjar testa **analyserings**-funktionen.

## <a name="calling-the-computer-vision-api-to-analyze-images"></a>Anropa API för visuellt innehåll för att analysera bilder

Åtgärden `analyze` extraherar en omfattande uppsättning visuella funktioner baserat på innehållet i bilden. Fråge-URL har följande format:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=<...>&details=<...>&language=<...>`

De parametrar som skickas till tjänsten är: `visualFeatures`, `details` och `languages`. Ange parametern `details` till `Landmarks` eller `Celebrities` för att identifiera landmärken eller kändisar. `visualFeatures` identifierar vilken slags information du vill att tjänsten ska returnera åt dig. Alternativet `Categories` kategoriserar bildernas innehåll, till exempel träd, byggnader med mera. `Faces` identifierar personernas ansikten och visar deras kön och ålder.

Alla åtgärder i API för visuellt innehåll har följande begränsningar när det gäller de bilder du ber den behandla:

- Bildformat som stöds: JPEG, PNG, GIF, BMP. 
- Bildens filstorlek måste vara mindre än 4 MB.
- Bilddimensionerna måste vara minst 50 x 50.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a>Identifiera landmärken i en bild

Låt oss börja med ett anrop för att hitta landmärken i en bild. Vi använder följande bild för det här exemplet, men du kan testa samma kommando med URL:er till andra bilder. 

![Bild av ett bergområde med blå himmel ovanför de snötäckta bergen.](../media/3-mountains.jpg)

Kör följande kommando i Azure Cloud Shell. Ersätt `<region>` i kommandot med ditt Cognitive Services-kontos region.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

- Det här anropet söker efter landmärken i bilden som anges med bild-URL:en. Den analyserade bilden lagras i en GitHub-lagringsplats för den här övningen. 
- Anropet uppmanar även tjänsten för att returnera information om programvarukategorier och en beskrivning av bilden. Beskrivningen returneras som en hel engelsk mening. 
- Som du vet behöver varje anrop till API:et en åtkomstnyckel. Detta ställs in i rubriken `Ocp-Apim-Subscription-Key` i begäran. 

> [!TIP]
> Resultatet av begäran är JSON-rådatafilen som har en beskrivning av bilden i sitt `url`. Vi har lagt till ` | jq '.'` i slutet av kommandot för att förklara JSON-utdata.

JSON-svaret från det här anropet returnerar följande:

- En `categories`-matris som listar alla kategorier för bilden som har identifierats, tillsammans med en poäng mellan 0 och 1 över hur säker tjänsten är på att bilden tillhör den angivna kategorin.
- En `description`-post som innehåller en matris med taggar eller ord som är relaterade till bilden.
- En `captions`-post med ett textfält som beskriver på engelska vad som är nytt i bilden. Observera att texten också har en säkerhetspoäng. Denna poäng kan hjälpa dig att avgöra vad du ska göra med den här analysen.


## <a name="check-for-inappropriate-content-in-an-image"></a>Sök efter olämpligt innehåll i en bild

I det här exemplet ska vi analysera en bild för vuxet innehåll. Förtroendepoängen bedömer sannolikheten att bilden innehåller vuxet eller olämpligt innehåll. 

Vi använder följande bild för det här exemplet, men du kan testa samma kommando med URL:er till andra bilder. 

![Bild av en leende familj.](../media/3-people.png)

1. Kör följande kommando i Azure Cloud Shell och ersätt `<region>` i URL:en.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

I det här exemplet anger vi `visualFeatures` till `Adult,Description`. 

Svaret ger oss två förtroendepoäng, ett för olämpligt innehåll och ett för vuxet innehåll. Med dessa poäng samt bildbeskrivningen och andra visuella funktioner kan du börja flagga bilder som ska publiceras till servern.

De exempel som vi provat i den här övningen ger dig en uppfattning om vilken typ av analys som du kan göra med åtgärden `analyze`. Prova analyser med olika bilder och prova olika kombinationer av visuella funktioner, information och språk.

Mer information om åtgärden `analyze` finns i referensdokumentationen [Analysera bild](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).