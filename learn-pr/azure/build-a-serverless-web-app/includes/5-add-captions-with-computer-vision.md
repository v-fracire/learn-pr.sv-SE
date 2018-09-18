<span data-ttu-id="86665-101">Nu är programmet ett fungerande galleri som du kan ladda upp bilder till och visa bilder i.</span><span class="sxs-lookup"><span data-stu-id="86665-101">At this point, the application is a functional gallery that allows you to upload and view images.</span></span> <span data-ttu-id="86665-102">I den här modulen lär du dig hur du använder API:et för visuellt innehåll från Microsoft Cognitive Services för att generera bildtexter för uppladdade bilder och spara dem med bildmetadata i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="86665-102">In this module, you learn how to use the Computer Vision API from Microsoft Cognitive Services to generate captions for uploaded images and save the captions with the image metadata in Azure Cosmos DB.</span></span>

## <a name="create-a-computer-vision-account"></a><span data-ttu-id="86665-103">Skapa ett konto för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="86665-103">Create a Computer Vision account</span></span>

<span data-ttu-id="86665-104">Microsoft Cognitive Services är en samling tjänster som utvecklare kan använda för att göra sina program mer intelligenta.</span><span class="sxs-lookup"><span data-stu-id="86665-104">Microsoft Cognitive Services is a collection of services that are available to developers to make their applications more intelligent.</span></span> <span data-ttu-id="86665-105">API:et för visuellt innehåll är en serverlös tjänst som bearbetar bilder med hjälp av avancerade algoritmer och som returnerar information om varje bild.</span><span class="sxs-lookup"><span data-stu-id="86665-105">The Computer Vision API is a serverless service that processes images using advanced algorithms and returns information about each image.</span></span> <span data-ttu-id="86665-106">Det har en kostnadsfri nivå för upp till 5,000 API-anrop per månad.</span><span class="sxs-lookup"><span data-stu-id="86665-106">It has a free tier that provides up to 5,000 API calls per month.</span></span>

1. <span data-ttu-id="86665-107">Kontrollera att du fortfarande är inloggad i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="86665-107">Ensure you're still signed into Cloud Shell.</span></span> <span data-ttu-id="86665-108">Om du inte är det väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.</span><span class="sxs-lookup"><span data-stu-id="86665-108">If you aren't, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="86665-109">Skapa ett nytt Cognitive Services-konto av typen **ComputerVision** med ett unikt namn i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="86665-109">Create a new Cognitive Services account of type **ComputerVision** with a unique name in your resource group.</span></span> <span data-ttu-id="86665-110">För den kostnadsfria nivån använder du **F0** som SKU.</span><span class="sxs-lookup"><span data-stu-id="86665-110">For the free tier, use **F0** as the SKU.</span></span> <span data-ttu-id="86665-111">Om du redan har ett befintligt konto för visuellt innehåll kan du behöva skapa ett standardkonto (S1). Detta kan dock medföra vissa kostnader.</span><span class="sxs-lookup"><span data-stu-id="86665-111">If you already have an existing Computer Vision account, you may need to create a Standard account (S1), which may incur some costs.</span></span>

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a><span data-ttu-id="86665-112">Skapa funktionsappinställningar för nyckeln och URL:en för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="86665-112">Create function app settings for Computer Vision URL and key</span></span>

<span data-ttu-id="86665-113">En URL och en nyckel krävs för att anropa API:et för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="86665-113">To call the Computer Vision API, a URL and key are required.</span></span>

1. <span data-ttu-id="86665-114">Hämta nyckeln och URL:en för API:et för visuellt innehåll och spara dem i Bash-variabler.</span><span class="sxs-lookup"><span data-stu-id="86665-114">Get the Computer Vision API key and URL and save them in Bash variables.</span></span>

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. <span data-ttu-id="86665-115">Skapa appinställningar med namnen **COMP_VISION_KEY** och **COMP_VISION_URL** i funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="86665-115">Create app settings named **COMP_VISION_KEY** and **COMP_VISION_URL**, respectively, in the function app.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```

## <a name="call-the-computer-vision-api-from-the-resizeimage-function"></a><span data-ttu-id="86665-116">Anropa API:et för visuellt innehåll från funktionen ResizeImage</span><span class="sxs-lookup"><span data-stu-id="86665-116">Call the Computer Vision API from the ResizeImage function</span></span>

<span data-ttu-id="86665-117">I följande steg ska du ändra funktionen **ResizeImage** så att den anropar API:et för visuellt innehåll för att beskriva varje uppladdad bild och spara beskrivningen i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="86665-117">In the following steps, you modify the **ResizeImage** function to call the Computer Vision API to describe each uploaded image and save the description in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="86665-118">Öppna funktionsappen i [Azure-portalen](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="86665-118">Open your function app in the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

<span data-ttu-id="86665-119">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="86665-119">::: zone pivot="csharp"</span></span>
1. <span data-ttu-id="86665-120">(C#) Leta upp funktionen **ResizeImage** i det vänstra navigeringsfönstret och öppna funktionens kodfönster.</span><span class="sxs-lookup"><span data-stu-id="86665-120">(C#) Using the left navigation, locate the **ResizeImage** function and open its code window.</span></span>

1. <span data-ttu-id="86665-121">(C#) Ersätt koden med innehållet i filen [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span><span class="sxs-lookup"><span data-stu-id="86665-121">(C#) Replace the code with the contents of the [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) file.</span></span> <span data-ttu-id="86665-122">I den här koden används `HttpClient` till att anropa API:t för visuellt innehåll och spara resultatet i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="86665-122">This code uses `HttpClient` to call the Computer Vision API and save its result in Azure Cosmos DB.</span></span>

<span data-ttu-id="86665-123">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="86665-123">::: zone-end</span></span>

<span data-ttu-id="86665-124">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="86665-124">::: zone pivot="javascript"</span></span>
1. <span data-ttu-id="86665-125">(JavaScript) I den här funktionen behövs paketet `axios` från npm för att göra ett HTTP-anrop till API:t för visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="86665-125">(JavaScript) This function requires the `axios` package from npm to make an HTTP call to the Computer Vision API.</span></span> <span data-ttu-id="86665-126">Du installerar npm-paketet genom att klicka på funktionsappens namn i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="86665-126">To install the npm package, click on the function app name on the left navigation and click **Platform features**.</span></span>

1. <span data-ttu-id="86665-127">(JavaScript) Öppna ett konsolfönster genom att klicka på **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="86665-127">(JavaScript) Click **Console** to reveal a console window.</span></span>

1. <span data-ttu-id="86665-128">(JavaScript) Kör kommandot `npm install axios` i konsolen.</span><span class="sxs-lookup"><span data-stu-id="86665-128">(JavaScript) Run the command `npm install axios` in the console.</span></span> <span data-ttu-id="86665-129">Det kan ta några minuter att slutföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="86665-129">It may take a few minutes to complete the operation.</span></span>

1. <span data-ttu-id="86665-130">(JavaScript) Klicka på funktionsnamnet (**ResizeImage**) i det vänstra navigeringsfönstret för att visa funktionen.</span><span class="sxs-lookup"><span data-stu-id="86665-130">(JavaScript) Click on the function name (**ResizeImage**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="86665-131">Byt ut hela innehållet i filen **index.js** mot innehållet i [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span><span class="sxs-lookup"><span data-stu-id="86665-131">Replace all of the **index.js** file with the contents of the [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) file.</span></span>

<span data-ttu-id="86665-132">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="86665-132">::: zone-end</span></span>

1. <span data-ttu-id="86665-133">Klicka på **Loggar** under kodfönstret för att expandera loggpanelen.</span><span class="sxs-lookup"><span data-stu-id="86665-133">Click **Logs** below the code window to expand the Logs panel.</span></span>

1. <span data-ttu-id="86665-134">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="86665-134">Click **Save**.</span></span> <span data-ttu-id="86665-135">Kontrollera loggpanelen för att bekräfta att funktionen har sparats och att det inte har uppstått några fel.</span><span class="sxs-lookup"><span data-stu-id="86665-135">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="86665-136">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="86665-136">Test the application</span></span>

1. <span data-ttu-id="86665-137">Öppna programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="86665-137">Open the application in a browser.</span></span> 

1. <span data-ttu-id="86665-138">Välj en bildfil och ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="86665-138">Select an image file and upload it.</span></span>

1. <span data-ttu-id="86665-139">Efter några sekunder bör du se en miniatyrbild för den nya bilden på sidan.</span><span class="sxs-lookup"><span data-stu-id="86665-139">After a few seconds, the thumbnail of the new image should appear on the page.</span></span> <span data-ttu-id="86665-140">Peka på bilden för att visa beskrivningen som genererats av Visuellt innehåll.</span><span class="sxs-lookup"><span data-stu-id="86665-140">Point to the image to see the description that's generated by Computer Vision.</span></span>

## <a name="summary"></a><span data-ttu-id="86665-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="86665-141">Summary</span></span>

<span data-ttu-id="86665-142">I den här delen har du lärt dig hur du automatiskt skapar bildtexter för uppladdade bilder med hjälp av API:et för visuellt innehåll i Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="86665-142">In this unit, you learned how to automatically generate captions for each uploaded image using Microsoft Cognitive Services Computer Vision API.</span></span> <span data-ttu-id="86665-143">I nästa avsnitt lär du dig hur du lägger till autentisering till programmet med hjälp av Azure App Service-autentisering.</span><span class="sxs-lookup"><span data-stu-id="86665-143">Next, you learn how to add authentication to the application using Azure App Service authentication.</span></span>