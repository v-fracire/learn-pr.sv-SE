Det bästa med Microsoft Custom Vision Service är hur enkelt det är för en utvecklare att bygga in intelligens i appen med hjälp av [Custom Visions Prediction-API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814). I den här enheten ska du använda Visual Studio Code för att göra ändringar i appen Artwork och få den att använda den modell som du skapat och tränat i de tidigare övningarna.

1. Om Node.js inte fins installerat på datorn går du till https://nodejs.org och installerar den senaste LTS-versionen för ditt operativsystem.

   > Om du inte vet om Node.js är installerat öppnar du en kommandotolk eller ett terminalfönster och skriver **node -v**. Om du inte ser något versionsnummer för Node.js kan du sluta dig till att Node.js inte är installerat. Om du har en version av Node.js som är äldre än version 6.0 rekommenderar vi starkt att du hämtar och installerar den senaste versionen.

1. Om Visual Studio Code inte finns installerat på din arbetsstation går du till http://code.visualstudio.com och installerar den nu.

1. Starta Visual Studio Code och välj **Open Folder...** (Öppna mapp...) i menyn **File** (Arkiv). När det öppnas en dialogruta väljer du mappen ”Client\Artworks” som finns i modulresurserna.

    ![Välja mappen Artworks](../media/5-fe-select-folder.png)

1. Använd kommandot **View** > **Integrated Terminal** (Visa > Integrerad terminal) i Visual Studio Code för att öppna den integrerade terminalen. Läs in alla paket som behövs till appen genom att ange följande kommando i den integrerade terminalen:

    ```
    npm install
    ```

1. Gå tillbaka till Artwork-projektet i Custom Vision Service-portalen, klicka på **Performance** (Prestanda) och klicka sedan på **Make default** (Ange som standard) och kontrollera den senaste iterationen av modellen används som standarditeration.

    ![Ange standarditeration](../media/5-portal-make-default.png)

1. Innan du kan köra appen och använda den för att anropa Custom Vision Service, måste den ändras så att den inkluderar information om slutpunkt och auktorisering. Klicka på **Prediction URL**.

    ![Visa information om Prediction URL](../media/5-portal-prediction-url.png)

1. Dialogrutan som visas innehåller två URL:er: en för att ladda upp bilder med en URL och en för att ladda upp lokala bilder. Kopiera Prediction-API:ts URL till bildfilerna till Urklipp.

    ![Kopiera Prediction-API:ts URL](../media/5-copy-prediction-url.png)

1. Gå tillbaka till Visual Studio Code och klicka på **predict.js** för att öppna den i kodredigeringsprogrammet.

    ![Öppna predict.js](../media/5-vs-predict-file.png)

1. Ersätt ”PREDICTION_ENDPOINT” på rad 3 med URL:en från Urklipp.

    ![Lägga till Prediction-API:ts URL](../media/5-vs-prediction-endpoint.png)

1. Gå tillbaka till Custom Vision Service-portalen och kopiera Prediction-API-nyckeln till Urklipp.

    ![Kopiera Prediction-API-nyckeln](../media/5-copy-prediction-key.png)

1. Gå tillbaka till Visual Studio Code och ersätt ”PREDICTION_KEY” på rad 4 i **predict.js** med API-nyckeln från Urklipp.

    ![Lägga till Prediction-API-nyckeln](../media/5-vs-prediction-key.png)

1. Rulla nedåt i **predict.js** och undersök kodavsnittet som börjar på rad 34. Det här är den här koden som anropar Custom Vision Service med hjälp av AJAX. Med Custom Vision Prediction-API är det lika lätt som att göra en enkel autentiserad POST i REST-slutpunkt.

    ![Göra anrop till Prediction-API:t](../media/5-vs-code-block.png)

1. Gå tillbaka till den integrerade terminalen i Visual Studio Code och kör följande kommando för att starta appen:

    ```
    npm start
    ```

1. Kontrollera att appen Artwork startas och visar ett fönster som det här:

    ![Appen Artworks](../media/5-app-startup.png)

Artworks är en plattformsoberoende app som skrivits med Node.js och [Electron](https://electron.atom.io/). Den kan därför köras på såväl Windows- som macOS- och Linux-datorer. I nästa övning kommer du att använda den till att klassificera bilder efter konstnär.