I den här modulen ska du distribuera ett enkelt webbprogram som visar ett HTML-baserad användargränssnitt. En serverlös serverdel gör det möjligt för program att kunna överföra bilder och automatiskt skapa beskrivande undertexter.

![Webbapp som körs](../media/0-app-screenshot-finished.png)

I följande diagram visas de Azure-tjänster som används av programmet.

![Diagram över lösningsarkitektur](../media/0-architecture.jpg)

1. Azure Blob Storage hanterar statiskt webbinnehåll (HTML, CSS och JS) och lagrar bilder.
2. Azure Functions hanterar uppladdning, storleksändring och metadatalagring för bilder.
3. Azure Cosmos DB lagrar bildmetadata.
4. Med Azure Logic Apps hämtar bildtexter från Cognitive Services-datorn Vision API.
5. Azure Active Directory hanterar användarautentisering.

Azure Blob Storage är en tjänst med extremt hög skalbarhet för lagring av statiska filer till låg kostnad. I den här modulen använder du Blob storage för att hantera statiskt innehåll (till exempel HTML, JavaScript och CSS) för en webbapp som du skapar.

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure Storage-konto

[!include[](../../../includes/azure-sandbox-activate.md)]

Ett Azure Storage-konto är en Azure-resurs där du kan lagra tabeller, köer, filer, blobbar (objekt) och VM-diskar.

1. Det statiska innehållet (HTML-, CSS- och JavaScript-filer) för den här självstudiekursen finns i Blob Storage. För Blob Storage krävs ett lagringskonto. Skapa en generell användning v2 (GPv2) Storage-konto i resursgruppen. Ersätt `<storage account name>` med ett unikt namn.

    ```azurecli
    az storage account create -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 --https-only true --sku Standard_LRS
    ```
    
1. Använd sökfältet överst i [Azure Portal](https://portal.azure.com/?azure-portal=true) för att leta rätt på det lagringskonto som du just skapade. Öppna kontot.

1. Välj **Statisk webbplats (förhandsversion)** i det vänstra navigeringsfönstret för att konfigurera en container för lagring av statiskt webbplatsinnehåll.
    - Välj **Aktiverad** för att aktivera en statisk webbplats.
    - Ange **index.html** som namn på indexdokumentet. I fältet står det redan *index.html* med grått teckensnitt, men det är bara exempeltext. Du måste ändå ange **index.html** i fältet.
    - Klicka på **Spara**.
    
    ![Ange inställningar för statisk webbplats](../media/1-storage-static-website.png)

1. Spara den **primära slutpunkten** på en plats som du enkelt kan kopiera den från när du går igenom självstudien. Denna slutpunkt är webbadressen för ditt webbprogram.

## <a name="upload-the-web-application"></a>Ladda upp webbprogrammet

1. Källfilerna för programmet som du skapar i den här självstudien finns på en [GitHub-lagringsplats](https://github.com/Azure-Samples/functions-first-serverless-web-application). Gå till din arbetskatalog i Cloud Shell och klona den här databasen.

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    Lagringsplatsen klonas till `/home/<username>/functions-first-serverless-web-application`.

1. Webbprogrammet på klientsidan finns i mappen **www** och skapas med hjälp av JavaScript-ramverket Vue.js. Öppna den **www** mappen och kör **npm** kommandon för att installera programmet beroenden och skapa programmet. Det sista av kommandona kan ta flera minuter att slutföra.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    Programmet genereras i mappen **dist**.

1. Ändra aktuell katalog till mappen **dist** och ladda upp programmet till blobcontainern **$web**.

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. Öppna statisk webbplats primära slutpunkts-URL i en webbläsare om du vill visa programmet.

    ![Startsida för den första serverlösa webbappen](../media/1-app-screenshot-new.png)


## <a name="summary"></a>Sammanfattning

I den här enheten skapade du ett lagringskonto. En blobbehållare med namnet **$web** i storage-konto lagrar det statiska innehållet för ditt webbprogram och gör att innehållet blir allmänt tillgängliga. Nu ska du lära dig hur du använder en funktion för att kunna överföra bilder till Blob-lagring från det här webbprogrammet.