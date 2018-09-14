Främsta distributörer av dina produkter skanna och ladda upp bilder av affärerna som de återinsättning. Eftersom lead developer på ditt företag, kan du skapa miniatyrbilder av avbildningarna. Miniatyrbilderna används i alla online rapporter som du skapar för säljgruppen. Nyligen SA Försäljningschefen att bilder i rapporten är suddig och ofta behöver inte produkten klient- och center, vilket gör det svårt att söka igenom stora rapporten. Det är upp till dig att förbättra situationen.

Du vill prova funktionen miniatyrbilder i API för visuellt innehåll. Du kan kanske göra ett bättre jobb än funktionen storleksändring du skrev.

Visuellt innehåll analyserar objekten i bilden att bestämma region of interest (ROI) först genererar en högkvalitativ miniatyr. Visuellt innehåll beskär bilden för att passa kraven för ”det intressanta området” (ROI). Den genererade miniatyrbilden kan vid behov anges med proportioner som skiljer sig från proportionerna på den ursprungliga bilden. Nu ska vi se hur det fungerar i praktiken.

## <a name="calling-the-computer-vision-api-to-generate-a-thumbnail"></a>Anropa de API för visuellt innehåll för att generera en miniatyrbild

Den `generateThumbnail` åtgärden skapar en miniatyrbild med användardefinierade bredd och höjd. Som standard tjänsten analyserar avbildningen, identifierar region of interest (ROI) och genererar smart beskärning koordinater baserat på vad du TJÄNAR. Smart beskärning hjälper när du anger proportionerna som skiljer sig från proportionerna på inmatad bild. Fråge-URL har följande format:

`https://[location].api.cognitive.microsoft.com/vision/v2.0/generateThumbnail[?width][&height][&smartCropping]`

Genom att ge olika parametrar till API:t kan du generera den miniatyr du behöver. Den `width` och `height` parametrar krävs. API: et säger vilken storlek som du behöver för en viss avbildning. Den `smartCropping` parametern genererar smartare beskärning genom att analysera region of interest i din avbildning för att det i miniatyrbilden. Till exempel skulle med smart beskärning aktiverad, en beskurna profilbild hindra någons ansikte inom ramen även när avbildningen har en speciell aspekt ransonen.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="generate-a-thumbnail"></a>Skapa en miniatyrbild

Vi använder följande bild i det här exemplet, men du är kostnadsfritt att testa samma kommando med URL: er till andra avbildningar. 

![Bild av en gullig vit hund som sitter i grön gräset.](../media/4-dog.png)

1. Kör följande kommandon i Azure Cloud Shell:

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```

I det här exemplet ber vi tjänsten för att skapa en miniatyrbild som är 100 x 100. Smart beskärning är aktiverat. Ett lyckat svar innehåller på miniatyrbilden binärt, vilket vi skriva till en fil med namnet thumbnail.jpg.  

> [!CAUTION]
> Den `-o` parametern omdirigerar utdata till en fil. Filen skrivs alltid, så om du vill behålla runt mer än en miniatyrbild uppstod det här kommandot ändrar du namnet på filen.

## <a name="downloading-the-thumbnail"></a>Ladda ned miniatyren

Den genererade miniatyrbilden hittas i ditt lagringskonto i Azure Cloud Shell. Vi valde namnet filen **thumbnail.jpg**. 

1. Kör följande kommandon i Azure Cloud Shell för att bekräfta att filen **thumbnail.jg** finns i arbetsmappen.

```azurecli
cd ~
ls -l
```
2. I Cloud Shell-menyn väljer **uppladdning/nedladdning filer**, och välj sedan **hämta** från den nedrullningsbara menyn.

3. I den **ladda ned en fil** dialogrutan som visas, ange **thumbnail.jpg** i det obligatoriska fältet och välj **hämta**. Hämtningen av avbildningen till den lokala datorn startar.

4. Hitta den hämta avbildningen **thumbnail.jpg** i din mapp för nedladdningar. Öppna filen i din favorit image viewer för att se miniatyrbilden som har skapats.

Den `generateThumbnail` åtgärden är en kraftfull miniatyr generator som klarar av att hålla den Region av intresse (ROI) av en avbildning i miniatyrbilden. 

Mer information om den `generateThumbnails` åtgärden, finns i den [hämta miniatyrbilden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) referensdokumentation.