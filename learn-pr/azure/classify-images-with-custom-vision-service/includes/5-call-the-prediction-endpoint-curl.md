I den förra övningen testade vi vår träningsmodell med funktionen **Quick Test** (Snabbtest) i Custom Vision Service-portalen. Det här är ett bra sätt att snabbt kontrollera modellens korrekthet med vissa testbilder. Nu går vi steget längre och gör anrop till förutsägelseslutpunkten för vår modell via HTTP.

[!include[](../../../includes/azure-sandbox-activate.md)]

1. Väl tillbaka i *Artworks-projektet** i Custom Vision Service-portalen väljer du fliken **Prestanda**.

    ![Välja fliken Prestanda](../media/5-performance-tab.png)

1. Välj **Ange som standard**  och kontrollera att den senaste iterationen av modellen är standarditerationen.

1. Välj **Förutsägelse-URL**. Då visas en dialogruta med den information som vi behöver för att göra anrop. 

    ![Visa information om Förutsägelse-URL](../media/5-portal-prediction-url.png)

    Så som visas i dialogrutan kan vi anropa förutsägelseslutpunkten och skicka en bild-URL till den. Vi kan även skicka en raw-bild till slutpunkten i brödtexten i begäran.

    Anteckna tre typer av information från den här dialogrutan.
     - **Förutsägelsenyckel**: den här nyckeln måste anges som en rubrik i alla begäranden. Det är detta som ger oss tillgång till slutpunkten.
    - **Begärande-URL**: dialogrutan visar två olika URL:er. Om vi publicerar en bild-URL använder du den första URL som slutar med `/url`. Om vi vill publicera en raw-bild i meddelandetexten i begäran kan vi använda den andra URL:en, som slutar med `/image`.
    - **Innehållstyp**: om vi publicerar en raw-bild anger vi meddelandetexten i begäran till den binära representationen av bilden och innehållstypen till `application/octet-stream`. Om vi publicerar en bild-URL lägger vi den som JSON i meddelandetexten och anger innehållstypen till `application/json`.
    

3. Kopiera och spara den första URL:en och värdet `Prediction-Key` från dialogrutan **Så här använder du Prediction API**. 

> [!TIP]
> **cURL** är ett kommandoradsverktyg som kan användas för att skicka och ta emot filer. Det ingår i Linux, macOS och Windows 10 och kan laddas ned för de flesta andra operativsystem. cURL har stöd för en rad protokoll, till exempel HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP och POP3. Du kan läsa mer genom att följa länkarna nedan:
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/> 
> 
> cURL är redan installerat i Azure Cloud Shell i sandbox-miljön. Så i den här övningen gör vi HTTP-anrop till vår slutpunkt genom att använda cURL.

2. Kör följande kommando i Cloud Shell. Ersätt **[slutpunkts-URL]** med den URL som du sparade från det förra steget. Ersätt **[förutsägelsenyckel]** med värdet för `Prediction-Key` som du sparade från det förra steget. 

    ```azurecli
    curl [endpoint-URL] \
    -H "Prediction-Key: [Prediction-Key]" \
    -H "Content-Type: application/json" \
    -d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/VanGoghTest_02.jpg'}" \
    | jq '.'
    ```

    När kommandot har slutförts visas ett JSON-svar som liknar följande skärmbild. API:et returnerar en sannolikhet för varje tagg i modellen. Som du kan se finns det en sannolikhet nära 1 för `"painting"` **tagName**-värdet, vilket innebär att den här avbildningen definitivt är en tavla. Det är dock inte en tavla som målats av någon av de konstnärer som vi tränat vår modell med. 

    ![Miniatyr av testbild med Picasso](../media/5-prediction-json.png) 

3. Prova fler förutsägelser genom att ersätta URL-adressen i begärandetexten ovan med URL-adresserna i följande tabell. 

    |Bild  | URL  |
    |---------|---------|
    |![Miniatyr av testbild med Picasso](../media/picasso-test-02-thumb.jpg)     | `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PicassoTest_02.jpg`        |
    |![Miniatyr av testbild med Rembrandt](../media/rembrandt-test-01-thumb.jpg)     |  `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/RembrandtTest_01.jpg`       |
    |![Miniatyr av testbild med Pollock](../media/pollock-test-01-thumb.jpg)  |   `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PollockTest_01.jpg`     |
   

Vår förutsägelseslutpunkt fungerar som förväntat. Att anropa API:et är så enkelt som att göra en HTTP-begäran till slutpunkten med en förutsägelsenyckel och en bild-URL.