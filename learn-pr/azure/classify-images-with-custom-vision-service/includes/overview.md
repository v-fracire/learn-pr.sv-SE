[Microsoft Cognitive Services](https://azure.microsoft.com/en-us/services/cognitive-services/ "Microsoft Cognitive Services") är en uppsättning tjänster och API:er som drivs med maskininlärning, och gör att utvecklare kan lägga till intelligenta funktioner som ansiktsigenkänning i foton och videor, analysera tonen i en text och bygga in språkförståelse i sina appar. Microsofts [Custom Vision Service](https://azure.microsoft.com/en-us/services/cognitive-services/custom-vision-service/) är en av de senaste medlemmarna i Cognitive Services-familjen. Det används till att skapa modeller för bildklassificering som ”lär sig” från taggade bilder som du tillhandahåller. Vill du veta om ett foto innehåller en bild av en blomma? Träna upp Custom Vision Service med en samling bilder av blommor. Sedan kan tjänsten avgöra om nästa bild du skickar innehåller en blomma, eller till om med vilken sorts blomma det är.

### <a name="what-is-covered-in-this-lab"></a>Vad går vi igenom i den här labbuppgiften?
I den här labbuppgiften kommer du att göra följande:
* skapa ett Custom Vision Service-projekt 
* träna upp en Custom Vision Service-modell med taggade bilder  
* testa en Custom Vision Service-modell 
* skapa appar där Custom Vision Service-modeller används genom anrop till REST-API:er.

För att kunna göra den här labbuppgiften behöver du ett Microsoft-konto. Om du inte har ett sådant kan du [registrera dig utan kostnad](https://account.microsoft.com/account). Du behöver även Visual Studio Code och Node.js.