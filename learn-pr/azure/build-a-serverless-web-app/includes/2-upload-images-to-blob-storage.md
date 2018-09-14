Programmet du skapar är en Fotogalleriet. Programmet använder JavaScript på klientsidan för att anropa API:er för uppladdning och visning av bilder. I den här enheten skapar du ett API med en serverfri funktion som genererar en tidsbegränsad URL för att ladda upp en bild. Webbprogrammet använder den här URL: en för att ladda upp en bild till Blob storage med hjälp av den [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

## <a name="create-a-blob-storage-container-for-images"></a>Skapa en Blob Storage-container för bilder

Programmet kräver en separat lagringscontainer för att ladda upp och lagra bilder.

1. Kontrollera att du fortfarande är inloggad i Azure Cloud Shell (Bash). Om inte väljer du **Enter focus mode** (Växla till fokusläge) för att öppna ett Cloud Shell-fönster.

1.  Skapa en ny behållare i ditt Storage-konto med namnet **avbildningar** i ditt lagringskonto med offentlig åtkomst till alla blobbar.

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-a-function-app"></a>Skapa en funktionsapp

Med tjänsten Azure Functions kan du köra serverlösa funktioner. En serverlös funktion kan utlösas (anropas) av händelser som till exempel en HTTP-begäran eller när en blobb skapas i en lagringscontainer.

En funktionsapp är en behållare för en eller flera funktioner utan server.

- Skapa en ny funktionsapp med ett unikt namn i den **första serverlösa app** resursgrupp som du skapade tidigare. Funktionsappar kräver ett lagringskonto. I den här enheten, ska du använda det befintliga lagringskontot som du skapade i den senaste enheten.

    ```azurecli
    az functionapp create -n <function app name> -g <rgn>[Sandbox resource group name]</rgn> -s <storage account name>
    ```

## <a name="create-an-http-triggered-serverless-function"></a>Skapa en HTTP-utlöst serverlös funktion

Webbappen för fotogalleriet skickar en HTTP-begäran till den serverlösa funktionen för att generera en tidsbegränsad URL för säker uppladdning av en bild till Blob Storage. Funktionen utlöses av en HTTP-begäran och använder Azure Storage SDK för att generera och returnera den säkra URL:en.

1. När funktionsappen har skapats kan du söka efter den i den [Azure-portalen](https://portal.azure.com/?azure-portal=true) med hjälp av den **Search** box. Klicka på appen för att öppna den.

    ![Öppna funktionsappen](../media/2-search-function-app.png)

1. I navigeringen till vänster i fönstret för Functions-app, peka på **Functions** och klicka på plustecknet (+) för att skapa en ny funktion.

    ![Skapa en ny funktion](../media/2-new-function.png)

1. Klicka på **Anpassad funktion** för att visa en lista med funktionsmallar.

1. Hitta den **HttpTrigger** mallen och klicka på C# eller JavaScript.

1. Använd dessa värden för att skapa en funktion som genererar en blobbuppladdnings-URL:

    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | --- | --- | ---|
    | **Språk** | C# eller JavaScript | Välj det språk som du vill använda. |
    | **Namnge din funktion** | GetUploadUrl | Ange det här namnet exakt så som det visas så att programmet kan identifiera funktionen. |
    | **Auktorisationsnivå** | Anonym | Kan funktionen nås offentligt. |

    ![Ange inställningarna för en ny HTTP-utlöst funktion](../media/2-new-function-httptrigger.png)

1. Skapa funktionen genom att klicka på **Skapa**.

::: zone pivot="csharp"
1. (C#) När funktionens källkoden visas Ersätt hela innehållet i den **run.csx** fil med innehållet i den [ **csharp/GetUploadUrl/run.csx** ](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) fil.

::: zone-end

::: zone pivot="javascript"
1. (JavaScript) Den här funktionen kräver `azure-storage`-paketet från npm. Paketet genererar den SAS-token (signatur för delad åtkomst) som krävs för att skapa den säkra URL:en. Installera npm-paketet genom att klicka på Functions-appen i det vänstra navigeringsfönstret och klicka på **Plattformsfunktioner**.

1. (JavaScript) Öppna ett konsolfönster genom att klicka på **Konsol**.

    ![Öppna ett konsolfönster](../media/2-open-console.jpg)

1. (JavaScript) Kontrollera att den aktuella katalogen är **d:\home\site\wwwroot** genom att köra kommandot `cd d:\home\site\wwwroot`.

1. (JavaScript) Kör kommandot `npm init -y` för att skapa en tom **package.json**-fil.

1. (JavaScript) Kör kommandot `npm install --save azure-storage` i konsolen för att installera paketet. Spara paketet som **package.json**. Det kan ta några minuter att slutföra åtgärden.

1. (JavaScript) Klicka på funktionen (**GetUploadUrl**) i det vänstra navigeringsfönstret för att visa funktionen. Byt ut hela innehållet i filen **index.js** med innehållet i filen [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).

    ![Innehållet i index.js efter uppdatering](../media/2-paste-js.jpg)

::: zone-end

1. Klicka på **Loggar** under kodfönstret för att expandera loggpanelen.

1. Klicka på **Spara**. Granska loggpanelen för att bekräfta att funktionen har kompilerats.

Funktionen genererar en URL för delad åtkomst (signatur) som används för att ladda upp en fil till Blob storage. SAS-Webbadressen är giltig för en kort tid och tillåter endast en enskild fil som ska laddas upp. Läs dokumentationen för Blob-lagring för mer information på [hur du använder signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a>Lägg till en miljövariabel för anslutningssträngen för lagring

Funktionen som du har skapat behöver en anslutningssträng för lagringskontot för att kunna generera SAS-URL:en. I stället för att hårdkoda anslutningssträngen i funktionens kod kan du lagra den som en programinställning. Inställningar för program är tillgängliga som miljövariabler med alla funktioner i funktionsappen.

1. Hämta anslutningssträngen för lagringskontot från Cloud Shell och spara den i en bash-variabel med namnet **STORAGE_CONNECTION_STRING**.

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    Bekräfta att variabeln är korrekt angiven.

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. Skapa en ny programinställning med namnet **AZURE_STORAGE_CONNECTION_STRING** genom att använda värdet som sparades i föregående steg.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    Bekräfta att kommandots utdata innehåller den nya programinställningen med rätt värde.


## <a name="test-the-serverless-function"></a>Testa den serverlösa funktionen

Förutom att skapa och redigera funktioner innehåller Azure Portal även ett inbyggt verktyg för att testa funktioner.

1. Testa den serverlösa HTTP-funktionen genom att klicka på fliken **Testa** till höger om kodfönstret för att expandera testpanelen.

1. Ändra **Http-metoden** till **GET**.

1. Under **Fråga** klickar du på **Lägg till parameter** och lägger till följande parameter:

    | Namn      |  Värde   | 
    | --- | --- |
    | **filename** | image1.jpg |

1. Klicka på **Kör** på testpanelen för att skicka en HTTP-begäran till funktionen.

1. Funktionen returnerar en uppladdnings-URL. Funktionskörningen visas på loggpanelen.

    ![Loggar som visar att funktionen körts utan problem](../media/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a>Konfigurera CORS i funktionsappen

Eftersom funktionen klientdelen finns i Blob storage, har ett annat domännamn än funktionsappen. Funktionsappen måste konfigureras för cross-origin resource sharing (CORS) för klientsidan-JavaScript ska lyckas anropa funktionen som du skapade.

1. I det vänstra navigeringsfönstret i funktionen app-fönstret klickar du på namnet på din funktionsapp.

1. Visa en lista med avancerade funktioner genom att klicka på **Plattformsfunktioner**.

1. Klicka på **CORS** under **API**.

    ![Välj CORS](../media/2-open-cors.jpg)

1. Lägg till ett tillåtna ursprung för URL: en för din webbplats som du skapade i föregående enhet och utelämna avslutande snedstreck (/). Till exempel: `https://firstserverlessweb.z4.web.core.windows.net`.

    ![CORS-inställningar som visar att den serverlösa webbappens URL har lagts till](../media/2-add-cors.png)

1. Klicka på **Spara**.

::: zone pivot="csharp"
1. (C#) Gå tillbaka till funktionen `GetUploadUrl` och välj fliken **Integrera**.

1. (C#) Välj **OPTIONS** under **Utvalda HTTP-metoder**.

    **GET**, **POST** och **OPTIONS** bör vara markerade. CORS använder **OPTIONS**-metoden, som inte är markerad som standard för C#-funktioner.  

1. (C#) Klicka på **Spara**.

::: zone-end

1. Gå till funktionsappen fortfarande i Azure-portalen. Välj fliken **Översikt**. Klicka på **Starta om** för att se till att ändringarna för CORS har tillämpats.

## <a name="configure-cors-in-the-storage-account"></a>Konfigurera CORS i lagringskontot

Eftersom funktionsappen gör även klientens JavaScript-anrop till Blob storage att ladda upp filer, måste du konfigurera Storage-konto för CORS.

- Kör följande kommando för att tillåta att alla ursprung laddar upp filer till lagringskontot:

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a>Ändra webbappen för att ladda upp bilder

Webbappen hämtar inställningar från en fil med namnet **settings.js**. Skapa filen med hjälp av Cloud Shell i de följande stegen. Du ställer in `window.apiBaseUrl` till URL: en för funktionsappen och `window.blobBaseUrl` till Webbadressen för Azure Blob storage-slutpunkten.

1. Kontrollera i Cloud Shell att mappen **www/dist** är den aktuella mappen.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. Öppna Cloud Shell redigeraren genom att skriva kommandot `code`.

    ```azurecli
    code
    ```

1. Fråga function-appens URL i Cloud Shell-fönstret nedanför redigeraren.

    ```azurecli
    echo "https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

1. Lägg till följande rad i fönstret redigeraren med hjälp av funktionen app-URL som du hämtade i föregående steg.

    ```
    window.apiBaseUrl = '<function app url>'
    ```

1. Fråga Azure Blob Storage slutpunkts-URL i Cloud Shell-fönstret nedanför redigeraren.

    ```azurecli
    echo $(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

1. Lägg till en andra raden i fönstret redigeraren med Storage slutpunkts-URL som du hämtade i föregående steg.

    ```
    window.blobBaseUrl = '<blob storage endpoint url>'
    ```

1. Spara filen som **settings.js** och Stäng redigeraren.

1. Bekräfta att filen skrivits korrekt och att den nu innehåller två rader.

    ```azurecli
    cat settings.js
    ```

1. Ladda upp filen till Blob Storage.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```

## <a name="test-the-web-application"></a>Testa webbprogrammet

Nu kan galleriprogrammet ladda upp en bild till Blob Storage, men den kan inte visa bilder än. Den kommer att försöka anropa en `GetImages`-funktion som inte finns ännu eftersom du skapar den i en senare modul. Anropet misslyckas och sidan verkar ha fastnat på ”Analyserar...”, men bilden du valt kommer att laddas upp korrekt.

Du kan kontrollera att en bild har laddats upp genom att kontrollera innehållet i containern **images** på Azure Portal.

1. Gå till programmet i ett webbläsarfönster. Välj en bildfil och ladda upp den. Uppladdningen slutförs, men eftersom vi inte har lagt till möjligheten att visa bilder ännu så visar inte appen det uppladdade fotot. (Sidan verkar ha fastnat på ”Analyserar bild...”.) Du åtgärdar detta senare.)

1. Kontrollera i Cloud Shell att bilden har laddats upp till containern **images**.

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. Ta bort alla filer i containern **images** innan du går vidare till nästa självstudie.

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```

## <a name="summary"></a>Sammanfattning

I den här enheten har du skapat en Azure Functions-app och lärt dig hur du använder en serverlös funktion för att ladda upp bilder till Blob Storage från ett webbprogram. Nu ska du lära dig hur du skapar miniatyrer för uppladdade bilder med hjälp av en blob-utlöst funktion.