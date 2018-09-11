I den här övningen ska vi skapa en Azure-funktion som tar emot en HTTP-förfrågan med en enda sträng. Funktionen returnerar en sträng till anroparen som representerar att åtgärden har lyckats eller misslyckats.

> [!NOTE]
> För att kunna slutföra den här övningen måste du vara inloggad på [Azure Portal](https://portal.azure.com/) med ett giltigt konto.

## <a name="create-an-http-trigger"></a>Skapa en HTTP-utlösare

Vi fortsätter använda vår befintliga Azure Functions-app och lägger nu till en HTTP-utlösare.

1. Peka på **Functions** och välj plustecknet (+).

    ![Peka på Functions och välj plustecknet](../media-drafts/4-hover-function.png)

1. Välj **HTTP-utlösare**.

1. Välj **#C** som språk. 

1. Låt **Namn** vara inställt på standardvärdet.

1. Som **Auktorisationsnivå** väljer du **Anonym**.

1. Välj **Skapa**.

1. Ta en titt på den automatiskt genererade koden så att du får en uppfattning om vad som sker. Parametern *req* representerar den inkommande förfrågningen och innehåller parametern *name*. Vi kontrollerar att *name* har ett värde. I så fall returnerar vi en hälsning. Annars returnerar vi ett felmeddelande.

## <a name="get-your-function-url"></a>Hämta funktionens webbadress

Nu när vi har skapat HTTP-utlösaren kan vi hämta funktionens webbadress så att vi kan skapa en förfrågning.

1. Välj din HTTP-utlösare så att kodskärmen öppnas.

1. Till höger om **Kör** väljer du **Hämta funktionswebbadress**.

1. Välj **Kopiera**.

1. Välj **Kör** för att starta funktionen.

## <a name="issue-a-get-request-to-your-http-trigger"></a>Utfärda en GET-begäran till din HTTP-utlösare

Nu har vi kopierat funktionens webbadress till Urklipp. Vi utfärdar en GET-förfrågning och ser om vi får något svar.

1. Öppna en ny flik i webbläsaren.

1. Klistra in webbadressen i adressfältet.

1. Lägg till en frågesträngsparameter med namnet *name* med till exempel ditt eget namn:

    ```
    .../api/HttpTriggerCSharp1?name=Jesse
    ```

1. Klicka på Enter för att skicka förfrågningen.

## <a name="clean-up"></a>Rensa

För att säkerställa att du inte debiteras för den här funktionen, väljer du **Pausa** ovanför loggfönstret.

![Pausa](../media-drafts/4-pause-timer.png)


