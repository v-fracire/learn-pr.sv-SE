### <a name="exercise-5-create-a-nodejs-app-that-uses-the-model"></a><span data-ttu-id="79b57-101">Övning 5: Skapa en Node.js-app som använder modellen</span><span class="sxs-lookup"><span data-stu-id="79b57-101">Exercise 5: Create a Node.js app that uses the model</span></span>

<span data-ttu-id="79b57-102">Det bästa med Microsoft Custom Vision Service är hur enkelt det är för en utvecklare att bygga in intelligens i appen med hjälp av [Custom Visions Prediction-API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span><span class="sxs-lookup"><span data-stu-id="79b57-102">The true power of the Microsoft Custom Vision Service is the ease with which developers can incorporate its intelligence into their own applications using the [Custom Vision Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span></span> <span data-ttu-id="79b57-103">I den här övningen får du använda Visual Studio Code för att göra ändringar i appen Artwork och få den att använda den modell som du skapat och tränat i de tidigare övningarna.</span><span class="sxs-lookup"><span data-stu-id="79b57-103">In this exercise, you will use Visual Studio Code to modify an app named Artwork to use the model you built and trained in previous exercises.</span></span>

1. <span data-ttu-id="79b57-104">Om Node.js inte fins installerat på datorn går du till https://nodejs.org och installerar den senaste LTS-versionen för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="79b57-104">If Node.js isn't installed on your system, go to https://nodejs.org and install the latest LTS version for your operating system.</span></span>

    > <span data-ttu-id="79b57-105">Om du inte vet om Node.js är installerat öppnar du en kommandotolk eller ett terminalfönster och skriver **node -v**.</span><span class="sxs-lookup"><span data-stu-id="79b57-105">If you aren't sure whether Node.js is installed, open a Command Prompt or terminal window and type **node -v**.</span></span> <span data-ttu-id="79b57-106">Om du inte ser något versionsnummer för Node.js kan du sluta dig till att Node.js inte är installerat.</span><span class="sxs-lookup"><span data-stu-id="79b57-106">If you don't see a Node.js version number, then Node.js isn't installed.</span></span> <span data-ttu-id="79b57-107">Om du har en version av Node.js som är äldre än version 6.0 rekommenderar vi starkt att du hämtar och installerar den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="79b57-107">If a version of Node.js older than 6.0 is installed, it is highly recommend that you download and install the latest version.</span></span>

1. <span data-ttu-id="79b57-108">Om Visual Studio Code inte finns installerat på din arbetsstation går du till http://code.visualstudio.com och installerar den nu.</span><span class="sxs-lookup"><span data-stu-id="79b57-108">If Visual Studio Code isn't installed on your workstation, go to http://code.visualstudio.com and install it now.</span></span>

1. <span data-ttu-id="79b57-109">Starta Visual Studio Code och välj **Open Folder...**  (Öppna mapp ...) i menyn **File** (Arkiv).</span><span class="sxs-lookup"><span data-stu-id="79b57-109">Start Visual Studio Code and select **Open Folder...** from the **File** menu.</span></span> <span data-ttu-id="79b57-110">När det öppnas en dialogruta väljer du mappen ”Client\Artworks” som finns i labbresurserna.</span><span class="sxs-lookup"><span data-stu-id="79b57-110">In the ensuing dialog, select the "Client\Artworks" folder included in the lab resources.</span></span>

    ![Välja mappen Artworks](../images/fe-select-folder.png)

    <span data-ttu-id="79b57-112">_Välja mappen Artworks_</span><span class="sxs-lookup"><span data-stu-id="79b57-112">_Selecting the Artworks folder_</span></span> 

1. <span data-ttu-id="79b57-113">Använd kommandot **View** > **Integrated Terminal** (Visa > Integrerad terminal) i Visual Studio Code för att öppna den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="79b57-113">Use the **View** > **Integrated Terminal** command to open an integrated terminal window in Visual Studio Code.</span></span> <span data-ttu-id="79b57-114">Läs in alla paket som behövs till appen genom att ange följande kommando i den integrerade terminalen:</span><span class="sxs-lookup"><span data-stu-id="79b57-114">Then execute the following command in the integrated terminal to load the packages required by the app:</span></span>

    ```
    npm install
    ```

1. <span data-ttu-id="79b57-115">Gå tillbaka till Artwork-projektet i Custom Vision Service-portalen, klicka på **Performance** (Prestanda) och klicka sedan på **Make default** (Ange som standard) och kontrollera den senaste iterationen av modellen används som standarditeration.</span><span class="sxs-lookup"><span data-stu-id="79b57-115">Return to the Artwork project in the Custom Vision Service portal, click **Performance**, and then click **Make default** to make sure the latest iteration of the model is the default iteration.</span></span> 

    ![Ange standarditeration](../images/portal-make-default.png)

    <span data-ttu-id="79b57-117">_Ange standarditeration_</span><span class="sxs-lookup"><span data-stu-id="79b57-117">_Specifying the default iteration_</span></span> 

1. <span data-ttu-id="79b57-118">Innan du kan köra appen och använda den för att anropa Custom Vision Service, måste den ändras så att den inkluderar information om slutpunkt och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="79b57-118">Before you can run the app and use it to call the Custom Vision Service, it must be modified to include endpoint and authorization information.</span></span> <span data-ttu-id="79b57-119">Klicka på **Prediction URL**.</span><span class="sxs-lookup"><span data-stu-id="79b57-119">To that end, click **Prediction URL**.</span></span>

    ![Visa information om Prediction URL](../images/portal-prediction-url.png)

    <span data-ttu-id="79b57-121">_Visa information om Prediction URL_</span><span class="sxs-lookup"><span data-stu-id="79b57-121">_Viewing Prediction URL information_</span></span> 

1. <span data-ttu-id="79b57-122">Dialogrutan som visas innehåller två URL:er: en för att ladda upp bilder med en URL och en för att ladda upp lokala bilder.</span><span class="sxs-lookup"><span data-stu-id="79b57-122">The ensuing dialog lists two URLs: one for uploading images via URL, and another for uploading local images.</span></span> <span data-ttu-id="79b57-123">Kopiera Prediction-API:ts URL till bildfilerna till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="79b57-123">Copy the Prediction API URL for image files to the clipboard.</span></span> 

    ![Kopiera Prediction-API:ts URL](../images/copy-prediction-url.png)

    <span data-ttu-id="79b57-125">_Kopiera Prediction-API:ts URL_</span><span class="sxs-lookup"><span data-stu-id="79b57-125">_Copying the Prediction API URL_</span></span> 

1. <span data-ttu-id="79b57-126">Gå tillbaka till Visual Studio Code och klicka på **predict.js** för att öppna den i kodredigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="79b57-126">Return to Visual Studio Code and click **predict.js** to open it in the code editor.</span></span>

    ![Öppna predict.js](../images/vs-predict-file.png)

    <span data-ttu-id="79b57-128">_Öppna predict.js_</span><span class="sxs-lookup"><span data-stu-id="79b57-128">_Opening predict.js_</span></span> 

1. <span data-ttu-id="79b57-129">Ersätt ”PREDICTION_ENDPOINT” på rad 3 med URL:en från Urklipp.</span><span class="sxs-lookup"><span data-stu-id="79b57-129">Replace "PREDICTION_ENDPOINT" in line 3 with the URL on the clipboard.</span></span>

    ![Lägga till Prediction-API:ts URL](../images/vs-prediction-endpoint.png)

    <span data-ttu-id="79b57-131">_Lägga till Prediction-API:ts URL_</span><span class="sxs-lookup"><span data-stu-id="79b57-131">_Adding the Prediction API URL_</span></span> 

1. <span data-ttu-id="79b57-132">Gå tillbaka till Custom Vision Service-portalen och kopiera Prediction-API-nyckeln till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="79b57-132">Return to the Custom Vision Service portal and copy the Prediction API key to the clipboard.</span></span> 

    ![Kopiera Prediction-API-nyckeln](../images/copy-prediction-key.png)

    <span data-ttu-id="79b57-134">_Kopiera Prediction-API-nyckeln_</span><span class="sxs-lookup"><span data-stu-id="79b57-134">_Copying the Prediction API key_</span></span> 

1. <span data-ttu-id="79b57-135">Gå tillbaka till Visual Studio Code och ersätt ”PREDICTION_KEY” på rad 4 i **predict.js** med API-nyckeln från Urklipp.</span><span class="sxs-lookup"><span data-stu-id="79b57-135">Return to Visual Studio Code and replace "PREDICTION_KEY" in line 4 of **predict.js** with the API key on the clipboard.</span></span>

    ![Lägga till Prediction-API-nyckeln](../images/vs-prediction-key.png)

    <span data-ttu-id="79b57-137">_Lägga till Prediction-API-nyckeln_</span><span class="sxs-lookup"><span data-stu-id="79b57-137">_Adding the Prediction API key_</span></span> 

1. <span data-ttu-id="79b57-138">Rulla nedåt i **predict.js** och undersök kodavsnittet som börjar på rad 34.</span><span class="sxs-lookup"><span data-stu-id="79b57-138">Scroll down in **predict.js** and examine the block of code that begins on line 34.</span></span> <span data-ttu-id="79b57-139">Det här är den här koden som anropar Custom Vision Service med hjälp av AJAX.</span><span class="sxs-lookup"><span data-stu-id="79b57-139">This is the code that calls out to the Custom Vision Service using AJAX.</span></span> <span data-ttu-id="79b57-140">Med Custom Vision Prediction-API är det lika lätt som att göra en enkel autentiserad POST i REST-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="79b57-140">Using the Custom Vision Prediction API is as easy as making a simple, authenticated POST to a REST endpoint.</span></span>

    ![Göra anrop till Prediction-API:t](../images/vs-code-block.png)

    <span data-ttu-id="79b57-142">_Göra anrop till Prediction-API:t_</span><span class="sxs-lookup"><span data-stu-id="79b57-142">_Making a call to the Prediction API_</span></span> 

1. <span data-ttu-id="79b57-143">Gå tillbaka till den integrerade terminalen i Visual Studio Code och kör följande kommando för att starta appen:</span><span class="sxs-lookup"><span data-stu-id="79b57-143">Return to the integrated terminal in Visual Studio Code and execute the following command to start the app:</span></span>

    ```
    npm start
    ```

1. <span data-ttu-id="79b57-144">Kontrollera att appen Artwork startas och visar ett fönster som det här:</span><span class="sxs-lookup"><span data-stu-id="79b57-144">Confirm that the Artworks app starts and displays a window like this one:</span></span>

    ![Appen Artworks](../images/app-startup.png)

    <span data-ttu-id="79b57-146">_Appen Artworks_</span><span class="sxs-lookup"><span data-stu-id="79b57-146">_The Artworks app_</span></span> 

<span data-ttu-id="79b57-147">Artworks är en plattformsoberoende app som skrivits med Node.js och [Electron](https://electron.atom.io/).</span><span class="sxs-lookup"><span data-stu-id="79b57-147">Artworks is a cross-platform app written with Node.js and [Electron](https://electron.atom.io/).</span></span> <span data-ttu-id="79b57-148">Den kan därför köras på såväl Windows- som macOS- och Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="79b57-148">As such, it is equally capable of running on Windows, macOS, and Linux.</span></span> <span data-ttu-id="79b57-149">I nästa övning kommer du att använda den till att klassificera bilder efter konstnär.</span><span class="sxs-lookup"><span data-stu-id="79b57-149">In the next exercise, you will use it to classify images by the artists who painted them.</span></span>