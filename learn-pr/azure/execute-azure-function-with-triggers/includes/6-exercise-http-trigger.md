I den här övningen ska vi skapa en Azure-funktion som tar emot en HTTP-begäran med en enda sträng. Funktionen returnerar en sträng till anroparen som representerar att åtgärden har lyckats eller misslyckats.

> [!NOTE]
> För att kunna slutföra den här övningen måste du vara inloggad på [Azure Portal](https://portal.azure.com/) med ett giltigt konto.

## <a name="create-an-http-trigger"></a>Skapa en HTTP-utlösare

Vi ska fortsätta att använda vår befintliga Azure Functions-app och lägger till en HTTP-utlösare.

1. Peka på **Funktioner** och välj plustecknet (+).

    ![Peka på Funktioner och välj plustecknet](../media-drafts/4-hover-function.png)

1. Välj **HTTP-utlösare**.

1. Välj **C#** som språk. 

1. Lämna standardvärdet för **Namn**.

1. Välj **Anonym** för **Auktorisationsnivå**.

1. Välj **Skapa**.

1. Ta en titt på den automatiskt genererade koden så att du får en uppfattning om vad som sker. Parametern *req* representerar den inkommande begäran och innehåller parametern *name*. Vi kontrollerar om *name* har ett värde. I så fall returnerar vi en hälsning. Annars returnerar vi ett felmeddelande.

## <a name="get-your-function-url"></a>Hämta funktionens webbadress

Nu när vi har skapat HTTP-utlösaren ska vi hämta funktionens webbadress så att vi kan börja skapa en begäran.

1. Välj HTTP-utlösaren för att öppna kodskärmen.

1. Välj **Hämta funktionswebbadress** till höger om **Kör**.

1. Välj **Kopiera**.

1. Välj **Kör** för att starta funktionen.

## <a name="issue-a-get-request-to-your-http-trigger"></a>Skicka en GET-begäran till HTTP-utlösaren

Nu har vi kopierat funktionens webbadress till Urklipp. Vi ska skicka en GET-begäran och se om vi får något svar.

1. Öppna en ny flik i webbläsaren.

1. Klistra in webbadressen i adressfältet.

1. Lägg till en frågesträngsparameter med namnet *name* med till exempel ditt eget namn:

    ```
    .../api/HttpTriggerCSharp1?name=Jesse
    ```

1. Klicka på Enter för att skicka förfrågningen.

## <a name="clean-up"></a>Rensa

För att undvika att debiteras för den här funktionen väljer du **Pausa** ovanför loggfönstret.

![Pausa](../media-drafts/4-pause-timer.png)


