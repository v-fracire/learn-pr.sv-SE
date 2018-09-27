I den här övningen ska vi skapa en Azure-funktion som tar emot en HTTP-begäran med en enda sträng. Funktionen returnerar en sträng till anroparen som anger om åtgärden lyckades eller misslyckades. Vi fortsätter arbeta med funktionen från den föregående övningen.

## <a name="create-an-http-trigger"></a>Skapa en HTTP-utlösare

Vi ska fortsätta att använda vår befintliga Azure Functions-app och lägga till en HTTP-utlösare.

1. Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Navigera till skärmen **Alla resurser** och välj din funktionsapp.

1. Peka på **Funktioner** och välj plustecknet (+).

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

1. Välj **Kopiera** och stäng sedan funktionens popupfönster.

## <a name="issue-a-get-request-to-your-http-trigger"></a>Skicka en GET-begäran till HTTP-utlösaren

Nu har vi kopierat funktionens webbadress till Urklipp. Vi ska skicka en GET-begäran och se om vi får något svar.

1. Öppna en ny flik i webbläsaren.

1. Klistra in webbadressen i adressfältet.

1. Lägg till en frågesträngsparameter med namnet *name* med ditt eget namn, till exempel `.../api/HttpTriggerCSharp1?name=Jesse`

1. Skicka begäran genom att trycka på <kbd>ENTER</kbd>.
