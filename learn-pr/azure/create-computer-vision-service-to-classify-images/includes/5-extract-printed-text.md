Anta att du nu vill läsa text från lagerbilderna som distributörerna i frontlinjen lägger upp på servern. I synnerhet vill du söka igenom produkten efter kampanjklistermärken som innehåller försäljningspriser. Det är dags att testa funktionen optisk teckenläsning (OCR) i API för visuellt innehåll. 

## <a name="calling-the-computer-vision-api-to-extract-printed-text"></a>Anropar API för visuellt innehåll för att extrahera tryckt text

Åtgärden `ocr` identifierar text i en bild och extraherar de tecken som identifieras i en teckenström som kan användas på en dator. Fråge-URL:en har följande format:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr?language=<...>&detectOrientation=<...>`

Alla anrop måste som vanligt göras till den region där kontot skapades. Anropet godkänner två valfria parametrar:

- **språk**: Språkkoden för texten som ska identifieras i bilden. Standardvärdet är `unk` eller okänd. Det låter tjänsten automatisk identifiera textspråket i bilden.
- **detectOrientation**: När den är sann försöker tjänsten identifiera bildorienteringen och rätta till den innan ytterligare bearbetning, till exempel om avbildningen är upp och ned eller inte. 

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="extract-printed-text-from-an-image-using-ocr"></a>Extrahera tryckt text från en bild med OCR

Bilden som vi ska använda för den optiska teckenläsningen (OCR) är ett omslag från boken *.NET Microservices: Architecture for Containerized .NET Applications*.

![Bild av omslaget från e-boken .NET Microservices: Architecture for Containerized .NET Applications](../media/5-ebook.png)

1. Kör följande kommando i Cloud Shell. Ersätt `<region>` i kommandot med ditt Cognitive Services-kontos region.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

Följande JSON är ett exempel på svaret som vi får från det här anropet. Vissa rader av JSON har tagits bort för att kodavsnittet ska passa bättre på sidan.

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

- Tjänsten har identifierat texten som engelska. Värdet för fältet `language` innehåller språkkoden BCP-47 för den text som har identifierats i bilden. I det här exemplet är det på **en**, engelska. 
- `orientation` identifierades som **upp**. Den här egenskapen är riktningen överst av den identifierade texten efter att bilden har roterats runt mitten enligt den definierade textvinkeln. 
- `textAngle` är vinkeln som texten måste roteras för att bli horisontell eller vertikal. I det här exemplet var texten perfekt horisontell, så det värde som returneras är **0**.  
- `regions`-egenskaperna innehåller en lista med värden som visar var texten är, dess läge i bilden och orden som hittas i den delen av bilden. 
- De fyra heltalen av `boundingBox`-värdet är: 
    - x-koordinaten för den vänstra kanten 
    - x-koordinaten för den övre kanten
    - bredden på avgränsningsfältet
    - höjden på avgränsningsfältet, 
   
    Du kan använda dessa värden för att rita en ruta runt varje del av texten i bilden.

Som du ser i den här övningen ger tjänsten `ocr` detaljerad information om tryckt text i en bild. 

Mer information om åtgärden `ocr` finns i referensdokumentationen [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc).