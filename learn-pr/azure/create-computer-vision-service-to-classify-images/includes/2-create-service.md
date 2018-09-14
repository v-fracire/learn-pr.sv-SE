
## <a name="what-is-microsoft-cognitive-services"></a>Vad är Microsoft Cognitive Services?

Microsoft Cognitive Services är en uppsättning machine learning-algoritmer som är tillgängligt som en tjänst för allmän användning. Vi kan använda dessa tjänster för visuellt innehåll, tal, språk, kunskap och sökning i stället för att skapa den här intelligensen för våra appar från grunden. Du kan prova varje tjänst utan kostnad. När du vill integrera en tjänst i din app kan registrera dig för en betald prenumeration. Vi att fokusera våra uppmärksamhet på den API för visuellt innehåll i den här modellen.

> [!TIP]
> Om du vill se en lista med alla kognitiva tjänster kan ta en titt på [Cognitive Services-katalog](https://azure.microsoft.com/services/cognitive-services/directory/). 

## <a name="what-is-the-computer-vision-api"></a>Vad är den API för visuellt innehåll?

API för visuellt innehåll ger algoritmer för att bearbeta bilder och returnera insikter. Du kan till exempel ta reda om en avbildning har mogen innehåll, eller använda den för att hitta alla ansikten i en bild. Det har även andra funktioner som beräknar dominerande ställning och accent färger, kategorisera och dela upp innehållet i bilder och som beskriver en bild med fullständiga engelska meningar. Det kan dessutom också smart generera miniatyrer av bilder för att visa stora bilder på ett effektivt sätt.

> [!TIP]
> API för visuellt innehåll är tillgängligt i många regioner över hela världen. Du hittar regionen närmast dig den [produkttillgänglighet per region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

Du kan använda den API för visuellt innehåll till:

- Analysera bilder för att få information
- Extrahera utskrivna text från bilder med optisk teckenigenkänning (OCR).
- Känner igen utskrivna och handskriven text från bilder
- Identifiera kändisar och landmärken
- Analysera video 
- Skapa miniatyrbilder av en avbildning 

## <a name="how-to-call-the-computer-vision-api"></a>Hur du anropar den API för visuellt innehåll

Du kan anropa visuellt innehåll i ditt program med klientbibliotek eller REST API direkt. Vi ringer REST-API i den här modulen. Så här gör ett anrop:

1. Hämta en API-åtkomstnyckel

    Åtkomstnycklar tilldelas när du registrerar dig för ett tjänstkonto för visuellt innehåll. En nyckel måste skickas i rubriken för **varje** begäran. 

1. Göra ett INLÄGG anrop till API: et

    Formatera URL: en på följande sätt: **region**.api.cognitive.microsoft.com/vision/v2.0/**resource**/**[parametrar]** 

    - **region** -regionen där du har skapat kontot, till exempel *westus*.
    - **resursen** -visuellt resursen som du anropar som `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.

    Du kan ange avbildningen som ska bearbetas antingen som en raw-bild binär fil eller en bild-URL.

    Huvudet för begäran måste innehålla prenumerationsnyckel som ger åtkomst till detta API.

1. Parsa svaret

    Svaret innehåller den information som den API för visuellt innehåll hade om din avbildning som en JSON-nyttolast.

Anta att du har skapat min prenumeration för visuellt innehåll i den `westus` region och fick en prenumerationsnyckel åtkomst till API. Nu ska vi ge nyckeln fiktiva värdet `xxxx-xxxxx-xxxx-xxxx`. Nu vill du generera en miniatyrbild för en avbildning som finns på `http://example.com/images/test.jpg`. Med hjälp av `generateThumbnail`, begäran skulle se ut så här.

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

I den här modulen vi kommer att köra alla övningarna i Azure CLI med integrerad Cloud Shell. Låt oss ta ut lite mer om den här installationen.

## <a name="what-is-the-azure-cli"></a>Vad är Azure CLI

Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser. Det finns för macOS, Linux och Windows eller i webbläsaren med [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> I dag finns det två tillgängliga versioner av verktyget Azure CLI: Azure CLI 1.0 och Azure CLI 2.0. Vi använder Azure CLI 2.0, som är den senaste versionen och rekommenderas om du inte kör äldre skript. Azure CLI 1.0 startas med kommandot `azure`, och Azure CLI 2.0 startas med kommandot `az`.

## <a name="az-cognitiveservices-commands"></a>AZ cognitiveservices-kommandon

Azure CLI innehåller den `cognitiveservices` kommando för att hantera Cognitive Services-konton i Azure. Det finns flera underkommandon för att utföra specifika aktiviteter. De vanligaste är:

| Underkommandot | Beskrivning |
|-------------|-------------|
| `list` | Lista över tillgängliga Azure Cognitive Services-konton. |
| `account show` | Hämta information om ett Azure Cognitive Services-konto. |
| `account create` | Skapa ett Azure Cognitive Services-konto. |
| `account delete` | Ta bort ett Azure Cognitive Services-konto. |
| `keys list` | Lista nycklarna på ett Azure Cognitive Services-konto. |

Nu ska vi skapa ett Cognitive Services med Azure CLI.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a>Skapa ett Cognitive Services-konto

Vi behöver en API-åtkomstnyckel för att göra anrop till API för visuellt innehåll. För att få åtkomst till nycklar, behöver vi ett Cognitive Services-konto för den API för visuellt innehåll. Vi använder `az cognitiveservices create` att skapa kontot i vår prenumeration.

 Kommandot `az cognitiveservices create` används för att skapa ett Cognitive Services-konto i en resursgrupp.  Följande fem parametrar måste anges när du anropar det här kommandot.

> [!Tip]
> De flesta flaggor för Azure CLI-parametrar kan förkortas till ett enskilt tecken. Anta exempelvis att vi kan `-l` i stället för `--location`. Formuläret långa används för tydlighetens skull.

| Parameter | Beskrivning |
|-----------|-------------|
| `resource-group` | Den resursgrupp som ska äga cognitive services-konto. I den här interaktiva sandbox-sessionen använder du <rgn>[Sandbox resursgruppens namn]</rgn> |
| `kind` | API-namnet på cognitive services-konto. |
| `location` | Platsen eller region, från vilken du vill göra det här API-anrop. |
| `name` | Kognitiva tjänstkontonamn. |
| `sku` | SKU: N för cognitive services-konto.|

1. Kör följande kommando i Azure Cloud Shell:

> [!Tip]
> Azure CLI-kommandon i den här modulen skrivs som flerradiga-kommandon för att minska mängden vågrät rullning. Du kan konvertera dessa kommandon till en enradig kommandon om du vill. 

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--location westus2 \
--name ComputerVisionService \
--sku F0 \
--resource-group <rgn>[Sandbox resource group name]</rgn>
```

Här vi har skapat ett cognitive services-konto för den **ComputerVision** API. Vi har valt den kostnadsfria SKU: n *F0* och som har namnet vår konto **ComputerVisionService**. Vår konto ägs av resursgruppen **<rgn>[Sandbox resursgruppens namn]</rgn>** och vi kan anropa API: T från den plats som vi angav i den `--location` parametern. I det här exemplet ska vi kunna göra anrop från västra USA 2. Anrop från en annan region resulterar i ett fel.

När kommandot har slutförts skapar cognitive services-konto, får du en JSON-svar som innehåller **provisioningState** egenskapen **lyckades**. Här är ett exempel på hur ett lyckat svar som ser ut.

<!-- TODO: find out the default location! -->

```json
{
  "endpoint": "https://westus2.api.cognitive.microsoft.com/vision/v2.0",
  "etag": "\"5c0070e5-0000-0000-0000-5b98cafa0000\"",
  "id": "/subscriptions/ae49d8aa-de86-418a-ba8c-4e9f3add2620/resourceGroups/ComputerVisionRG/providers/Microsoft.CognitiveServices/accounts/ComputerVisionService",
  "internalId": "437c9519bead47e6ba4b8e0842c1f76f",
  "kind": "ComputerVision",
  "location": "westus2",
  "name": "ComputerVisionService",
  "provisioningState": "Succeeded",
  "resourceGroup": "ComputerVisionRG",
  "sku": {
    "name": "F0",
    "tier": null
  },
  "tags": null,
  "type": "Microsoft.CognitiveServices/accounts"
}
```

> [!NOTE]
> Om din Azure-prenumeration redan har ett visuellt cognitive services-konto som konfigurerats med den **F0** Sku innan du kör det här kommandot, ser du följande meddelande:
>
> `Only one free account is allowed for account type 'ComputerVision'` 
>
> Du bör använda det kontot för den här modulen eller ändra Sku i den `az cognitiveservices account create` för att *S1* eller en annan Sku.

## <a name="get-an-access-key"></a>Hämta en åtkomstnyckel

När vi har vår konto har skapats kan vi hämta prenumerationsnycklar eller åtkomstnycklar för det här kontot.

1. Kör följande kommando i Azure Cloud Shell:

```azurecli
az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn>
```

Ovanstående kommando returnerar de nycklar som är associerade med cognitive services-kontot som kallas **ComputerVisionService**, som ägs av den givna resursgruppen. Den returnerar två nycklar – en är en ledig nyckel. Nycklarna är svåra att komma ihåg, så vi ska sparas den första nyckeln i en variabel som vi använder för alla anrop till API: et.

2.  Kör följande kommando i Azure Cloud Shell:

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--query key1 -o tsv)
```

Azure CLI 2.0 använder den `--query` argumentet att köra en JMESPath-fråga på resultatet av kommandon. JMESPath är ett frågespråk för JSON som ger dig möjlighet att välja och visa data från CLI-utdata. De här frågorna körs på JSON-utdata innan någon Visa formatering.
Argumentet `--query` stöds av alla kommandon i Azure CLI. 

I vårt exempel har vi frågar listan med nycklar för en post med namnet ”key1” och utdata resultatet ska **tsv** format. Det här formatet tar bort offerter runt strängvärdet. Vi tilldela resultatet till en variabel **nyckeln**.

> [!IMPORTANT]
> Vi ska använda den här nyckeln i modulen, så spara den i en variabel är en bra idé. Om du förlorar värdet eller variabeln blir har angetts, kör du kommandot igen för att ställa in den.  

3. Visa värdet för våra nyckeln genom att köra följande kommando i Azure Cloud Shell:

```azurecli
echo $key
```

Nu när vi har ett konto och en nyckel, är det dags att göra vissa anrop till API: et.