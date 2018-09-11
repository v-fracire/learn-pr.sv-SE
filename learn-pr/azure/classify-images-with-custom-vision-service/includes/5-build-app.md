### <a name="exercise-5-create-a-nodejs-app-that-uses-the-model"></a>Övning 5: Skapa en Node.js-app som använder modellen

Det bästa med Microsoft Custom Vision Service är hur enkelt det är för en utvecklare att bygga in intelligens i appen med hjälp av [Custom Visions Prediction-API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814). I den här övningen får du använda Visual Studio Code för att göra ändringar i appen Artwork och få den att använda den modell som du skapat och tränat i de tidigare övningarna.

1. Om Node.js inte fins installerat på datorn går du till https://nodejs.org och installerar den senaste LTS-versionen för ditt operativsystem.

    > Om du inte vet om Node.js är installerat öppnar du en kommandotolk eller ett terminalfönster och skriver **node -v**. Om du inte ser något versionsnummer för Node.js kan du sluta dig till att Node.js inte är installerat. Om du har en version av Node.js som är äldre än version 6.0 rekommenderar vi starkt att du hämtar och installerar den senaste versionen.

1. Om Visual Studio Code inte finns installerat på din arbetsstation går du till http://code.visualstudio.com och installerar den nu.

1. Starta Visual Studio Code och välj **Open Folder...**  (Öppna mapp ...) i menyn **File** (Arkiv). När det öppnas en dialogruta väljer du mappen ”Client\Artworks” som finns i labbresurserna.

    ![Välja mappen Artworks](../images/fe-select-folder.png)

    _Välja mappen Artworks_ 

1. Använd kommandot **View** > **Integrated Terminal** (Visa > Integrerad terminal) i Visual Studio Code för att öppna den integrerade terminalen. Läs in alla paket som behövs till appen genom att ange följande kommando i den integrerade terminalen:

    ```
    npm install
    ```

1. Gå tillbaka till Artwork-projektet i Custom Vision Service-portalen, klicka på **Performance** (Prestanda) och klicka sedan på **Make default** (Ange som standard) och kontrollera den senaste iterationen av modellen används som standarditeration. 

    ![Ange standarditeration](../images/portal-make-default.png)

    _Ange standarditeration_ 

1. Innan du kan köra appen och använda den för att anropa Custom Vision Service, måste den ändras så att den inkluderar information om slutpunkt och auktorisering. Klicka på **Prediction URL**.

    ![Visa information om Prediction URL](../images/portal-prediction-url.png)

    _Visa information om Prediction URL_ 

1. Dialogrutan som visas innehåller två URL:er: en för att ladda upp bilder med en URL och en för att ladda upp lokala bilder. Kopiera Prediction-API:ts URL till bildfilerna till Urklipp. 

    ![Kopiera Prediction-API:ts URL](../images/copy-prediction-url.png)

    _Kopiera Prediction-API:ts URL_ 

1. Gå tillbaka till Visual Studio Code och klicka på **predict.js** för att öppna den i kodredigeringsprogrammet.

    ![Öppna predict.js](../images/vs-predict-file.png)

    _Öppna predict.js_ 

1. Ersätt ”PREDICTION_ENDPOINT” på rad 3 med URL:en från Urklipp.

    ![Lägga till Prediction-API:ts URL](../images/vs-prediction-endpoint.png)

    _Lägga till Prediction-API:ts URL_ 

1. Gå tillbaka till Custom Vision Service-portalen och kopiera Prediction-API-nyckeln till Urklipp. 

    ![Kopiera Prediction-API-nyckeln](../images/copy-prediction-key.png)

    _Kopiera Prediction-API-nyckeln_ 

1. Gå tillbaka till Visual Studio Code och ersätt ”PREDICTION_KEY” på rad 4 i **predict.js** med API-nyckeln från Urklipp.

    ![Lägga till Prediction-API-nyckeln](../images/vs-prediction-key.png)

    _Lägga till Prediction-API-nyckeln_ 

1. Rulla nedåt i **predict.js** och undersök kodavsnittet som börjar på rad 34. Det här är den här koden som anropar Custom Vision Service med hjälp av AJAX. Med Custom Vision Prediction-API är det lika lätt som att göra en enkel autentiserad POST i REST-slutpunkt.

    ![Göra anrop till Prediction-API:t](../images/vs-code-block.png)

    _Göra anrop till Prediction-API:t_ 

1. Gå tillbaka till den integrerade terminalen i Visual Studio Code och kör följande kommando för att starta appen:

    ```
    npm start
    ```

1. Kontrollera att appen Artwork startas och visar ett fönster som det här:

    ![Appen Artworks](../images/app-startup.png)

    _Appen Artworks_ 

Artworks är en plattformsoberoende app som skrivits med Node.js och [Electron](https://electron.atom.io/). Den kan därför köras på såväl Windows- som macOS- och Linux-datorer. I nästa övning kommer du att använda den till att klassificera bilder efter konstnär.