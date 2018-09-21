
## <a name="what-is-microsoft-cognitive-services"></a>Vad är Microsoft Cognitive Services?

Microsoft Cognitive Services är en uppsättning maskininlärningsalgoritmer som är tillgänglig som en tjänst för allmän användning. Vi kan använda dessa tjänster för visuellt innehåll, tal, språk, kunskap och sökning i stället för att skapa den här intelligensen för våra appar från grunden. Du kan prova varje tjänst utan kostnad. När du vill integrera en tjänst i din app kan du registrera dig för en betald prenumeration. Vi kommer att fokusera vår uppmärksamhet på API för visuellt innehåll i den här modellen.

> [!TIP]
> Om du vill se en lista med alla Cognitive Services kan du ta en titt i [Cognitive Services-katalogen](https://azure.microsoft.com/services/cognitive-services/directory/). 

## <a name="what-is-the-computer-vision-api"></a>Vad är API för visuellt innehåll?

API för visuellt innehåll ger algoritmer för att bearbeta bilder och returnera insikter. Du kan till exempel ta reda på om en bild har innehåll för vuxna, eller använda den för att hitta alla ansikten i en bild. Den har även andra funktioner som att beräkna dominerande och accentfärger, kategorisera och dela upp innehållet i bilder och beskriva en bild med fullständiga engelska meningar. Den kan dessutom också smart generera miniatyrer av bilder för att visa stora bilder på ett effektivt sätt.

> [!TIP]
> API för visuellt innehåll är tillgängligt i många regioner över hela världen. Du hittar regionen närmast dig i [Produkttillgänglighet per region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

Du kan använda API för visuellt innehåll för att:

- Analysera bilder för att få information
- Extrahera utskriven text från bilder med optisk teckenigenkänning (OCR).
- Känna igen utskriven och handskriven text från bilder
- Identifiera kända personer och landmärken
- Analysera video 
- Skapa en miniatyrbild av en bild 

## <a name="how-to-call-the-computer-vision-api"></a>Hur anropar man API för visuellt innehåll

Du kan anropa visuellt innehåll i ditt program med klientbibliotek eller REST API direkt. Vi anropar REST API i den här modulen. Så här gör man ett anrop:

1. Hämta en API-åtkomstnyckel

    Åtkomstnycklar tilldelas när du registrerar dig för ett tjänstkonto för visuellt innehåll. En nyckel måste skickas i rubriken för **varje** begäran. 

1. Gör ett POST-anrop till API:et

    Formatera URL:en på följande sätt: **region**.api.cognitive.microsoft.com/vision/v2.0/**resource**/**[parametrar]** 

    - **region** – regionen där du har skapat kontot, till exempel *westus*.
    - **resurs** – den resurs för virtuellt innehåll som du anropar, såsom `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.

    Du kan ange bilden som ska bearbetas antingen som en binär raw-bild eller en bild-URL.

    Begärandehuvudet måste innehålla prenumerationsnyckeln som ger åtkomst till detta API.

1. Parsa svaret

    Svaret innehåller den information som API för visuellt innehåll hade om din bild som en JSON-nyttolast.

Anta att du skapade min prenumeration för visuellt innehåll i regionen `westus` och fick en prenumerationsnyckel för åtkomst till API:et. Nu ska vi ge nyckeln det fiktiva värdet `xxxx-xxxxx-xxxx-xxxx`. Nu vill du generera en miniatyrbild för en bild som finns på `http://example.com/images/test.jpg`. Med hjälp av `generateThumbnail`, skulle begäran se ut så här.

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

I den här modulen kommer vi att köra alla övningarna i Azure CLI med hjälp av integrerad Cloud Shell. Låt oss ta reda på lite mer om den här installationen.

## <a name="what-is-the-azure-cli"></a>Vad är Azure CLI

Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser. Det finns för macOS, Linux och Windows eller i webbläsaren med [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> I dag finns det två tillgängliga versioner av verktyget Azure CLI: Azure CLI 1.0 och Azure CLI 2.0. Vi använder Azure CLI 2.0, som är den senaste versionen och rekommenderas om du inte kör äldre skript. Azure CLI 1.0 startas med kommandot `azure`, och Azure CLI 2.0 startas med kommandot `az`.

## <a name="az-cognitiveservices-commands"></a>az cognitiveservices-kommandon

Azure CLI innehåller kommandot `cognitiveservices` för att hantera Cognitive Services-konton i Azure. Det finns flera underkommandon för att utföra specifika aktiviteter. De vanligaste är:

| Underkommando | Beskrivning |
|-------------|-------------|
| `list` | Skapa en lista över tillgängliga Azure Cognitive Services-konton. |
| `account show` | Hämta information om ett Azure Cognitive Services-konto. |
| `account create` | Skapa ett Azure Cognitive Services-konto. |
| `account delete` | Ta bort ett Azure Cognitive Services-konto. |
| `keys list` | Skapa en lista över nycklar för ett Azure Cognitive Services-konto. |

Nu ska vi skapa ett Cognitive Services-konto med Azure CLI.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a>Skapa ett Cognitive Services-konto

Vi behöver en API-åtkomstnyckel för att göra anrop till API för visuellt innehåll. För att få åtkomstnycklar, behöver vi ett Cognitive Services-konto för API för visuellt innehåll. Vi använder `az cognitiveservices create` för att skapa kontot i vår prenumeration.

 Kommandot `az cognitiveservices create` används för att skapa ett Cognitive Services-konto i en resursgrupp.  Följande fem parametrar måste anges när du anropar det här kommandot.

> [!Tip]
> De flesta flaggor för Azure CLI-parametrar kan förkortas till ett enskilt tecken. Anta exempelvis att vi kan säga `-l` i stället för `--location`. Den långa formen används för tydlighetens skull.

| Parameter | Beskrivning |
|-----------|-------------|
| `resource-group` | Den resursgrupp som ska äga Cognitive Services-kontot. I den här interaktiva sandbox-miljön använder du <rgn>[Sandbox-resursgruppens namn]</rgn> |
| `kind` | API-namnet på Cognitive Services-kontot. |
| `name` | Cognitive service-kontonamnet. |
| `sku` | SKU för Cognitive Services-kontot.|
| `location` | Platsen eller regionen, från vilken du vill göra det här API-anropet. Välj någon av platserna i listan nedan. |

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

Kör följande kommando i Azure Cloud Shell. Ersätt `[location]` med en plats nära dig.

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--location [location]
```

> [!NOTE]
> Kom ihåg den plats du har valt. Du ska göra alla anrop till API:et från den regionen.

Vi har skapat ett Cognitive Services-konto för API för **ComputerVision**. Vi har valt *S1* SKU och har döpt vår konto till **ComputerVisionService**. Vårt konto ägs av resursgruppen **<rgn>[Sandbox-resursgruppens namn]</rgn>** och vi anropar API:et från den plats som vi angav i parametern `--location`. 

När kommandot har slutfört skapandet av Cognitive Services-kontot, får du ett JSON-svar som innehåller egenskapen **provisioningState** inställd till **Lyckades**.

## <a name="get-an-access-key"></a>Hämta en åtkomstnyckel

När kontot har skapats kan vi hämta prenumerationsnycklar eller åtkomstnycklar för det här kontot.

1. Kör följande kommando i Azure Cloud Shell:

    ```azurecli
    az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
    ```
    
    Ovanstående kommando returnerar de nycklar som är associerade med Cognitive Services-kontot som kallas **ComputerVisionService** och ägs av den givna resursgruppen. Den returnerar två nycklar – en är en reservnyckel. Nycklarna är svåra att komma ihåg, så vi ska spara den första nyckeln i en variabel som vi använder för alla anrop till API:et.

2.  Kör följande kommando i Azure Cloud Shell:

    ```azurecli
    key=$(az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query key1 -o tsv)
    ```
    
    Azure CLI 2.0 använder argumentet `--query` för att utföra en JMESPath-fråga på resultaten från kommandon. JMESPath är ett frågespråk för JSON som ger dig möjlighet att välja och visa data från CLI-utdata. De här frågorna körs på JSON-utdata före visningsformatering.
    Argumentet `--query` stöds av alla kommandon i Azure CLI. 
    
    I vårt exempel frågar vi listan med nycklar efter en post med namnet ”key1” och matar ut resultatet till **tsv**-format. Det här formatet tar bort citat runt strängvärdet. Vi tilldelar resultatet till en variabel **nyckel**.
    
    > [!IMPORTANT]
    > Vi ska använda den här nyckeln i modulen, så det är en bra idé att spara den i en variabel. Om du förlorar värdet eller variabeln upphävs, kör du kommandot igen för att ställa in det.  

3. Visa värdet för vår nyckeln genom att köra följande kommando i Azure Cloud Shell:

    ```azurecli
    echo $key
    ```

Nu när vi har ett konto och en nyckel, är det dags att göra några anrop till API:et.