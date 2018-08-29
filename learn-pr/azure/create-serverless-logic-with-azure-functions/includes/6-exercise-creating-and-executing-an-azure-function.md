I den här övningen fortsätter vi vårt drevexempel och lägger till logik för temperaturtjänsten. Mer specifikt tar vi emot data från en HTTP-förfrågning.

## <a name="function-requirements"></a>Funktionskrav
Nu ska vi definiera en del krav för vår logik:
- temperaturer mellan 0–25 ska flaggas som **OK**
- temperaturer mellan 26–50 ska flaggas som **CAUTION**
- temperaturer över 50 ska flaggas som **DANGER**.

## <a name="adding-a-function-to-an-azure-function-app"></a>Lägga till en funktion i en Azure Functions-app

Som vi redan har sett så kan du få hjälp när du lär dig att arbeta i Azure Functions. En bra funktion när du ska komma igång med Functions är att generera en funktion med någon av de många mallarna. I den här övningen använder du mallen HttpTrigger till att implementera temperaturtjänsten.

1. Logga in på [Azure Portal](https://portal.azure.com) med ditt Azure-konto.
1. Öppna resursgruppen du skapade i den första övningen genom att välja **Alla resurser** på menyn till vänster och sedan välja **escalator-functions-group**.
1. Gruppens resurser visas. Öppna funktionsappen du skapade i föregående övning genom att välja objektet **escalator-functions-xxxxxxx** (identifieras med blixtikonen).
  ![Öppna funktionsappen](../images/6-access-function-app.png)
1. Du ser en översikt över funktionsappen på bladet. Dessutom visas ett navigeringsträd till vänster med definierade funktioner, proxys och platser. Vi har inte någon funktion ännu, så det här trädet är tomt. Du skapar en funktion genom att flytta musen till **Funktion** i navigeringsträdet och klicka på knappen **+** som visas.
  ![Lägg till funktionsnavigering](../images/5-function-add-button.png)
1. I det fördefinierade funktionsformuläret väljer du länken **Anpassad funktion** i avsnittet **Kom igång på egen hand**.
  ![Fördefinierat funktionsformulär](../images/6-custom-function.png)
1. Nu visas en lista med mallar. Välj JavaScript-implementeringen av HTTP-utlösarmallen.
  ![HTTP-utlösarmall](../images/6-httptrigger-template.png)
1. Du ser ett blad där du kan definiera vad mallen ska generera. I det här fallet vill du generera en JavaScript-funktion med namnet **DriveGearTemperatureService**. När du har namngett funktionen trycker du knappen **Skapa**.
  ![Formulär för att skapa HTTP-utlösare](../images/6-create-httptrigger-form.png)
1. Efter en stund ser du den mallgenererade källkoden för funktionen. Den genererade funktionen väntar sig att du anger ett namn, och därefter genereras **Hello, {namn}**.

    ```javascript
    module.exports = function (context, req) {
        context.log('JavaScript HTTP trigger function processed a request.');

        if (req.query.name || (req.body && req.body.name)) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "Hello " + (req.query.name || req.body.name)
            };
        }
        else {
            context.res = {
                status: 400,
                body: "Please pass a name on the query string or in the request body"
            };
        }
        context.done();
    };
    ```

1. Till höger i källvyn ser du två flikar. Du kan se alla filer som har stöd för funktionen på fliken **Visa filer**. Du kan välja **function.json** om du vill se funktionens konfiguration. Här kan du se den definierade httpTrigger-funktionen och utdatabindningen. I konfigurationen beskrivs att funktionen initieras med en HTTP-förfrågning, och utdatabindningen beskriver att svaret skickas som ett HTTP-svar.

    ```javascript
    {
      "disabled": false,
      "bindings": [
        {
          "authLevel": "function",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req"
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        }
      ]
    }
    ```

## <a name="running-the-premade-azure-function"></a>Köra den fördefinierade Azure-funktionen

När du ska köra funktionen kan du initiera en HTTP-förfrågan med cURL från en kommandorad. Du hittar slutpunktsadressen genom att gå tillbaka till funktionskoden och välja länken **Hämta funktionswebbadress**. Kopiera den här länken till Urklipp.  Webbadressen bör se ut ungefär så här: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![Hämta slutpunktsadress](../images/6-get-function-url.png)

Kör ett cURL-kommando med webbadressen för att initiera din funktion (ersätt webbadressen med din egen):

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

![cURL-svar från fördefinierad funktion](../images/6-premadefunction-curl.png)

## <a name="adding-business-logic-to-the-function"></a>Lägga till affärslogik i funktionen

Vår funktion förväntar sig en matris med temperaturavläsningar från kunden. Här är ett exempel på en förfrågan:

```javascript
{
    "readings": [
        {
            "driveGearId": 1,
            "timestamp": 1534263995,
            "temperature": 23
        },
        {
            "driveGearId": 3,
            "timestamp": 1534264048,
            "temperature": 45
        },
        {
            "driveGearId": 18,
            "timestamp": 1534264050,
            "temperature": 55
        }
    ]
}
```

Ändra koden i den fördefinierade funktionen så att du implementerar den affärslogik som behövs. Öppna filen index.js och ersätt den med följande kod:

```javascript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        for(var i=0; i<req.body.readings.length; i++){
            var reading = req.body.readings[i];
            if(reading.temperature<=25){
                context.log('Reading is OK');
                reading.status = 'OK';
                continue;
            }
            if(reading.temperature<=50){
                context.log('Reading is CAUTION');
                reading.status = 'CAUTION';
                continue;
            }
            context.log('Reading is DANGER');
            reading.status = 'DANGER'
        }
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: {
                "readings": req.body.readings
            }
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please send an array of readings in the request body"
        };
    }
    context.done();
};
```

Notera logginstruktionerna. När funktionen körs skickar de meddelanden till loggfönstret.

## <a name="testing-your-business-logic"></a>Testa din affärslogik

Öppna testfönstret från menyn till höger och klistra in exempelförfrågan ovan i textrutan. Tryck på knappen **Kör** och läs utdata. Du kan också öppna loggfönstret från menyn längst ned om du vill se loggposterna.
![Testa affärslogiken](../images/6-portal-testing.png) JSON-data i utdatafönstret visar att vårt fält för temperaturstatus har lagts till för var och en av avläsningarna. Du kan också se att vår förfrågan har loggats i Application Insights på Monitor-instrumentpanelen.
![Förfrågan i Application Insights](../images/6-app-insights.png)
