Distributörer av dina produkter i frontlinjen skannar och laddar upp bilder av butikshyllor som de fyller på. Du som chefsutvecklare är ansvarig för att skapa miniatyrbilder av bilderna. Miniatyrbilderna används i alla onlinerapporter som du skapar för säljteamet. Försäljningschefen sa nyligen att bilderna i rapporten är suddiga och ofta inte har produkterna i centrum, vilket gör det svårt att söka igenom stora rapporter. Det är upp till dig att förbättra situationen.

Du vill prova funktionen generera miniatyrbilder i API för visuellt innehåll. Du kan kanske göra ett bättre jobb än storleksändringsfunktionen du skrev.

Visuellt innehåll genererar först en högkvalitativ miniatyrbild och sedan analyseras objekten i bilden för att fastställa det intressanta området (ROI). Visuellt innehåll beskär bilden för att passa kraven för ”det intressanta området” (ROI). Den genererade miniatyrbilden kan vid behov anges med proportioner som skiljer sig från proportionerna på den ursprungliga bilden. Nu ska vi se hur det fungerar i praktiken.

## <a name="calling-the-computer-vision-api-to-generate-a-thumbnail"></a>Anropa API för visuellt innehåll för att skapa en miniatyrbild

Åtgärden `generateThumbnail` skapar en miniatyrbild med den användardefinierade bredden och höjden. Som standard analyserar tjänsten bilden, identifierar det intressanta området (ROI) och skapar smarta beskärningskoordinater baserat på det intressanta området. Smart beskärning hjälper dig när du anger ett bredd–höjd-förhållande som skiljer sig från bredd–höjd-förhållandet på den inmatade bilden. Fråge-URL har följande format:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=<...>&height=<...>&smartCropping=<...>`

Genom att ge olika parametrar till API:t kan du generera den miniatyr du behöver. Parametrarna `width` och `height` krävs. De anger för API vilken storlek du behöver för en viss bild. Parametern `smartCropping` genererar en smartare beskärning genom att analysera det intressantaste området i bilden och behålla det i miniatyren. När smart beskärning är aktiverat behålls till exempel ansiktet i en beskuren profilbild inom bildramen fastän bilden har ett annat bredd–höjd-förhållande.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="generate-a-thumbnail"></a>Skapa en miniatyrbild

Vi använder följande bild i det här exemplet, men du kan testa samma kommando med URL:er till andra bilder. 

![Bild av en gullig vit hund som sitter på grönt gräs.](../media/4-dog.png)

1. Kör följande kommandon i Azure Cloud Shell. Ersätt `<region>` i kommandot med ditt Cognitive Services-kontos region

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```

I det här exemplet ber vi tjänsten att skapa en miniatyrbild som är 100 x 100. Smart beskärning är aktiverat. Ett lyckat svar innehåller den binära miniatyrbilden som vi skriver till en fil med namnet thumbnail.jpg.

> [!CAUTION]
> Parametern `-o` dirigerar om utdata till en fil. Filen skrivs alltid över, så om du vill behålla mer än en miniatyrbild när du prövar detta kommandot måste du ändra filnamnet.

## <a name="view-the-generated-thumbnail"></a>Visa den skapade miniatyrbilden

Du hittar den genererade miniatyren i ditt Azure Cloud Shell-lagringskonto. Vi gav filen namnet **thumbnail.jpg**. 

Cloud Shell i Microsoft Learn har inte möjlighet att hämta filer, men du kan hämta miniatyrbilden via Azure Portal med hjälp av följande anvisningar.

1. Kör följande kommandon i Azure Cloud Shell för att bekräfta att filen **thumbnail.jpg** finns i arbetsmappen.

    ```azurecli
    cd ~
    ls -l
    ```

    

1. Flytta `thumbnail.jpg` till clouddrive-mappen med följande kommando.

    ```azurecli
    mv ~/thumbnail.jpg ~/clouddrive
    ```
1. Logga in på [Azure Portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.
1. Välj det lagringskonto som börjar med `cloudshell` på panelen **Alla resurser** på portalens instrumentpanel. 
1. Välj **Storage Explorer** på Storage-kontopanelen, sedan **FILRESURSER** och därefter filresursen i samlingen med det namn som börjar ned ** cloudshellfiles***.
1. Välj den *thunbnail.jpg* fil och sedan **hämta** på den översta menyn för att se avbildningen.

Åtgärden `generateThumbnail` är en kraftfull miniatyrskapare som klarar av att hålla det intressanta området (ROI) av en bild i miniatyrbilden.

Mer information om åtgärden `generateThumbnails` finns i referensdokumentationen [Hämta miniatyrbild](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb).