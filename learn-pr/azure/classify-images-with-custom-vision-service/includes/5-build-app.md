<span data-ttu-id="b1eaa-101">Det bästa med Microsoft Custom Vision Service är hur enkelt det är för en utvecklare att bygga in intelligens i appen med hjälp av [Custom Visions Prediction-API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span><span class="sxs-lookup"><span data-stu-id="b1eaa-101">The true power of the Microsoft Custom Vision Service is the ease with which developers can incorporate its intelligence into their own applications using the [Custom Vision Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span></span> <span data-ttu-id="b1eaa-102">I den här enheten ska du använda Visual Studio Code för att göra ändringar i appen Artwork och få den att använda den modell som du skapat och tränat i de tidigare övningarna.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-102">In this unit, you will use Visual Studio Code to modify an app named Artwork to use the model you built and trained in previous exercises.</span></span>

1. <span data-ttu-id="b1eaa-103">Om Node.js inte fins installerat på datorn går du till https://nodejs.org och installerar den senaste LTS-versionen för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-103">If Node.js isn't installed on your system, go to https://nodejs.org and install the latest LTS version for your operating system.</span></span>

   > <span data-ttu-id="b1eaa-104">Om du inte vet om Node.js är installerat öppnar du en kommandotolk eller ett terminalfönster och skriver **node -v**.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-104">If you aren't sure whether Node.js is installed, open a command prompt or terminal window and type **node -v**.</span></span> <span data-ttu-id="b1eaa-105">Om du inte ser något versionsnummer för Node.js kan du sluta dig till att Node.js inte är installerat.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-105">If you don't see a Node.js version number, then Node.js isn't installed.</span></span> <span data-ttu-id="b1eaa-106">Om du har en version av Node.js som är äldre än version 6.0 rekommenderar vi starkt att du hämtar och installerar den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-106">If a version of Node.js older than 6.0 is installed, we highly recommend that you download and install the latest version.</span></span>

1. <span data-ttu-id="b1eaa-107">Om Visual Studio Code inte finns installerat på din arbetsstation går du till http://code.visualstudio.com och installerar den nu.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-107">If Visual Studio Code isn't installed on your workstation, go to http://code.visualstudio.com and install it now.</span></span>

1. <span data-ttu-id="b1eaa-108">Starta Visual Studio Code och välj **Open Folder...** (Öppna mapp...) i menyn **File** (Arkiv).</span><span class="sxs-lookup"><span data-stu-id="b1eaa-108">Start Visual Studio Code and select **Open Folder...** from the **File** menu.</span></span> <span data-ttu-id="b1eaa-109">När det öppnas en dialogruta väljer du mappen ”Client\Artworks” som finns i modulresurserna.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-109">In the ensuing dialog, select the "Client\Artworks" folder included in the module resources.</span></span>

    ![Välja mappen Artworks](../media/5-fe-select-folder.png)

1. <span data-ttu-id="b1eaa-111">Använd kommandot **View** > **Integrated Terminal** (Visa > Integrerad terminal) i Visual Studio Code för att öppna den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-111">Use the **View** > **Integrated Terminal** command to open an integrated terminal window in Visual Studio Code.</span></span> <span data-ttu-id="b1eaa-112">Läs in alla paket som behövs till appen genom att ange följande kommando i den integrerade terminalen:</span><span class="sxs-lookup"><span data-stu-id="b1eaa-112">Then execute the following command in the integrated terminal to load the packages required by the app:</span></span>

    ```console
    npm install
    ```

1. <span data-ttu-id="b1eaa-113">Gå tillbaka till Artwork-projektet i Custom Vision Service-portalen, klicka på **Performance** (Prestanda) och klicka sedan på **Make default** (Ange som standard) och kontrollera den senaste iterationen av modellen används som standarditeration.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-113">Return to the Artwork project in the Custom Vision Service portal, click **Performance**, and then click **Make default** to make sure the latest iteration of the model is the default iteration.</span></span>

    ![Ange standarditeration](../media/5-portal-make-default.png)

1. <span data-ttu-id="b1eaa-115">Innan du kan köra appen och använda den för att anropa Custom Vision Service, måste den ändras så att den inkluderar information om slutpunkt och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-115">Before you can run the app and use it to call the Custom Vision Service, it must be modified to include endpoint and authorization information.</span></span> <span data-ttu-id="b1eaa-116">Klicka på **Prediction URL**.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-116">To that end, click **Prediction URL**.</span></span>

    ![Visa information om Prediction URL](../media/5-portal-prediction-url.png)

1. <span data-ttu-id="b1eaa-118">Dialogrutan som visas innehåller två URL:er: en för att ladda upp bilder med en URL och en för att ladda upp lokala bilder.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-118">The ensuing dialog lists two URLs: one for uploading images via URL, and another for uploading local images.</span></span> <span data-ttu-id="b1eaa-119">Kopiera Prediction-API:ts URL till bildfilerna till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-119">Copy the Prediction API URL for image files to the clipboard.</span></span>

    ![Kopiera Prediction-API:ts URL](../media/5-copy-prediction-url.png)

1. <span data-ttu-id="b1eaa-121">Gå tillbaka till Visual Studio Code och klicka på **predict.js** för att öppna den i kodredigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-121">Return to Visual Studio Code and click **predict.js** to open it in the code editor.</span></span>

    ![Öppna predict.js](../media/5-vs-predict-file.png)

1. <span data-ttu-id="b1eaa-123">Ersätt ”PREDICTION_ENDPOINT” på rad 3 med URL:en från Urklipp.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-123">Replace "PREDICTION_ENDPOINT" in line 3 with the URL on the clipboard.</span></span>

    ![Lägga till Prediction-API:ts URL](../media/5-vs-prediction-endpoint.png)

1. <span data-ttu-id="b1eaa-125">Gå tillbaka till Custom Vision Service-portalen och kopiera Prediction-API-nyckeln till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-125">Return to the Custom Vision Service portal and copy the Prediction API key to the clipboard.</span></span>

    ![Kopiera Prediction-API-nyckeln](../media/5-copy-prediction-key.png)

1. <span data-ttu-id="b1eaa-127">Gå tillbaka till Visual Studio Code och ersätt ”PREDICTION_KEY” på rad 4 i **predict.js** med API-nyckeln från Urklipp.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-127">Return to Visual Studio Code and replace "PREDICTION_KEY" in line 4 of **predict.js** with the API key on the clipboard.</span></span>

    ![Lägga till Prediction-API-nyckeln](../media/5-vs-prediction-key.png)

1. <span data-ttu-id="b1eaa-129">Rulla nedåt i **predict.js** och undersök kodavsnittet som börjar på rad 34.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-129">Scroll down in **predict.js** and examine the block of code that begins on line 34.</span></span> <span data-ttu-id="b1eaa-130">Det här är den här koden som anropar Custom Vision Service med hjälp av AJAX.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-130">This is the code that calls out to the Custom Vision Service using AJAX.</span></span> <span data-ttu-id="b1eaa-131">Med Custom Vision Prediction-API är det lika lätt som att göra en enkel autentiserad POST i REST-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-131">Using the Custom Vision Prediction API is as easy as making a simple, authenticated POST to a REST endpoint.</span></span>

    ![Göra anrop till Prediction-API:t](../media/5-vs-code-block.png)

1. <span data-ttu-id="b1eaa-133">Gå tillbaka till den integrerade terminalen i Visual Studio Code och kör följande kommando för att starta appen:</span><span class="sxs-lookup"><span data-stu-id="b1eaa-133">Return to the integrated terminal in Visual Studio Code and execute the following command to start the app:</span></span>

    ```console
    npm start
    ```

1. <span data-ttu-id="b1eaa-134">Kontrollera att appen Artwork startas och visar ett fönster som det här:</span><span class="sxs-lookup"><span data-stu-id="b1eaa-134">Confirm that the Artworks app starts and displays a window like this one:</span></span>

    ![Appen Artworks](../media/5-app-startup.png)

<span data-ttu-id="b1eaa-136">Artworks är en plattformsoberoende app som skrivits med Node.js och [Electron](https://electron.atom.io/).</span><span class="sxs-lookup"><span data-stu-id="b1eaa-136">Artworks is a cross-platform app written with Node.js and [Electron](https://electron.atom.io/).</span></span> <span data-ttu-id="b1eaa-137">Den kan därför köras på såväl Windows- som macOS- och Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-137">As such, it is equally capable of running on Windows, macOS, and Linux.</span></span> <span data-ttu-id="b1eaa-138">I nästa övning kommer du att använda den till att klassificera bilder efter konstnär.</span><span class="sxs-lookup"><span data-stu-id="b1eaa-138">In the next exercise, you will use it to classify images by the artists who painted them.</span></span>