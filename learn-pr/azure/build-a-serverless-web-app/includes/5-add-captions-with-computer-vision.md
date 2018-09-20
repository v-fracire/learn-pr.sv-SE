Nu är programmet ett fungerande galleri som du kan ladda upp bilder till och visa bilder i. I den här modulen lär du dig hur du använder API:et för visuellt innehåll från Microsoft Cognitive Services för att generera bildtexter för uppladdade bilder och spara dem med bildmetadata i Azure Cosmos DB.

## <a name="create-a-computer-vision-account"></a>Skapa ett konto för visuellt innehåll

Microsoft Cognitive Services är en samling tjänster som utvecklare kan använda för att göra sina program mer intelligenta. API:et för visuellt innehåll är en serverlös tjänst som bearbetar bilder med hjälp av avancerade algoritmer och som returnerar information om varje bild. Det har en kostnadsfri nivå för upp till 5,000 API-anrop per månad.

Skapa ett nytt Cognitive Services-konto av typen **ComputerVision** med ett unikt namn i resursgruppen. För den kostnadsfria nivån använder du **F0** som SKU. Om du redan har ett befintligt konto för visuellt innehåll kan du behöva skapa ett standardkonto (S1). Detta kan dock medföra vissa kostnader.

```azurecli
az cognitiveservices account create \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <computer vision account name> \
    --kind ComputerVision \
    --sku F0 \
    --location $(az group show -n <rgn>[Sandbox resource group name]</rgn> --query location -o tsv)
```

När du uppmanas att godkänna datameddelandet anger du `y` och trycker på `Enter`. Det kan ta ett par minuter innan kommandot har slutförts.

## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a>Skapa funktionsappinställningar för nyckeln och URL:en för visuellt innehåll

En URL och en nyckel krävs för att anropa API:et för visuellt innehåll.

1. Hämta nyckeln och URL:en för API:et för visuellt innehåll och spara dem i Bash-variabler.

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g <rgn>[Sandbox resource group name]</rgn> -n <computer vision account name> --query key1 --output tsv)
    export COMP_VISION_URL=$(az cognitiveservices account show -g <rgn>[Sandbox resource group name]</rgn> -n <computer vision account name> --query endpoint --output tsv)
    ```

1. Skapa appinställningar med namnen **COMP_VISION_KEY** och **COMP_VISION_URL** i funktionsappen.

    ```azurecli
    az functionapp config appsettings set \
        -n <function app name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```

## <a name="call-the-computer-vision-api-from-the-resizeimage-function"></a>Anropa API:et för visuellt innehåll från funktionen ResizeImage

I följande steg ska du ändra funktionen **ResizeImage** så att den anropar API:et för visuellt innehåll för att beskriva varje uppladdad bild och spara beskrivningen i Azure Cosmos DB.

1. Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du har aktiverat sandbox-miljön med.

1. Öppna din funktionsapp.

::: zone pivot="csharp"

3. Leta upp funktionen **ResizeImage** i det vänstra navigeringsfönstret och öppna funktionens kodfönster.

1. Ersätt koden med innehållet i filen [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx). I den här koden används `HttpClient` till att anropa API:t för visuellt innehåll och spara resultatet i Azure Cosmos DB.

1. Klicka på **Loggar** under kodfönstret för att expandera loggpanelen.

1. Klicka på **Spara**. Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.

::: zone-end

::: zone pivot="javascript"

2. I den här funktionen behövs paketet `axios` från npm för att göra ett HTTP-anrop till API:t för visuellt innehåll. Du installerar npm-paketet genom att klicka på funktionsappens namn i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.

1. Öppna ett konsolfönster genom att klicka på **Konsol**.

1. Kör kommandot `npm install axios` i konsolen. Det kan ta några minuter att slutföra åtgärden.

1. Klicka på funktionsnamnet **ResizeImage** i det vänstra navigeringsfönstret för att visa funktionen. Byt ut hela innehållet i filen **index.js** mot innehållet i [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).

1. Klicka på **Loggar** under kodfönstret för att expandera loggpanelen.

1. Klicka på **Spara**. Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.

::: zone-end

## <a name="test-the-application"></a>Testa programmet

1. Öppna programmet i en webbläsare.

1. Välj en bildfil och ladda upp den.

1. Efter några sekunder bör du se en miniatyrbild för den nya bilden på sidan. Peka på bilden för att visa beskrivningen som genererats av Visuellt innehåll.

## <a name="summary"></a>Sammanfattning

I den här delen har du lärt dig hur du automatiskt skapar bildtexter för uppladdade bilder med hjälp av API:et för visuellt innehåll i Microsoft Cognitive Services. I nästa avsnitt lär du dig hur du lägger till autentisering till programmet med hjälp av Azure App Service-autentisering.