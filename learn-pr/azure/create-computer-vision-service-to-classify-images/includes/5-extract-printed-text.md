Anta att du nu vill läsa text från bilder som din främsta distributörer skicka data till servern. I synnerhet du vill genomsöka produkten som söker efter priset klistermärken som den innehåller försäljning priser. Är det dags att testa funktionen optisk teckenläsning (OCR) i den API för visuellt innehåll. 

## <a name="calling-the-computer-vision-api-to-extract-printed-text"></a>Anropa de API för visuellt innehåll för att extrahera tryckt text

Den `ocr` åtgärden identifierar text i en bild och extraherar lästa tecken till en dator teckenström. Fråge-URL har följande format:

`https://[location].api.cognitive.microsoft.com/vision/v2.0/ocr[?language][&detectOrientation ] `

Som vanligt måste alla anrop göras till den plats där konton som har skapats. Anropet godkänner två valfria parametrar:

- **språk**: språkkod för texten som ska identifieras i avbildningen. Standardvärdet är `unk`, eller okänd. Den här vi tjänsten automatisk identifiering av språket i texten i bilden.
- **detectOrientation**: Anger om tjänsten försöker identifiera bildorientering och rätta till det innan du ytterligare bearbetning, till exempel om avbildningen är upp och ned. 

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="extract-printed-text-from-an-image-using-ocr"></a>Extrahera utskrivna text från en avbildning med OCR

Bilden som vi ska använda för den optiska teckenläsningen (OCR) är ett omslag från boken *.NET Microservices: Architecture for Containerized .NET Applications*.

![Bild av omslaget till e-boken .NET Microservices: arkitekturen för program i behållare .NET](../media/5-ebook.png)

1. Kör följande kommandon i Azure Cloud Shell:

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

Följande JSON är ett exempel på svaret som vi får från det här anropet. Vissa rader med JSON har tagits bort för att göra kodavsnitt som passar bättre på sidan.

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

Låt oss nu undersöka detta svar i detalj. 

- Tjänsten har identifierat texten som engelska. Värdet för den `language` fältet innehåller BCP-47 språkkoden för den text som har identifierats i avbildningen. I det här exemplet är det **en**, eller så är engelska. 
- Den `orientation` identifierades som **upp**. Den här egenskapen är den riktning som upp i den tolkade texten har råkat när avbildningen har roterats runt mitten enligt den identifierade textvinkel. 
- Den `textAngle` är den vinkel som måste texten roteras om du vill bli vågrätt eller lodrätt. I det här exemplet texten var perfekt horisontell, så det värde som returneras är **0**.  
- Den `regions` egenskapen innehåller en lista med värden som används för att visa där texten är, dess position i bilden och ordet hittades i den del av avbildningen. 
- Fyra heltal av det `boundingBox` värde är: 
    - x-koordinaten för den vänstra kanten 
    - y-koordinaten för den övre kanten
    - bredden på rutan
    - höjden på rutan, 
   
    Du kan använda dessa värden för att rita en ruta runt varje del av texten i bilden.

Som du ser i den här övningen den `ocr` service ger detaljerad information om tryckt text i en bild. 

Mer information om den `ocr` åtgärden, finns i den [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) referensdokumentation.