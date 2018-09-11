I den här övningen fortsätter vi vårt drevexempel och lägger till logik för temperaturtjänsten. Mer specifikt tar vi emot data från en HTTP-förfrågning.

## <a name="function-requirements"></a>Funktionskrav
Nu ska vi definiera en del krav för vår logik:
- temperaturer mellan 0–25 ska flaggas som **OK**
- temperaturer mellan 26–50 ska flaggas som **CAUTION**
- temperaturer över 50 ska flaggas som **DANGER**

## <a name="add-a-function-to-an-azure-function-app"></a>Lägg till en funktion i en Azure-funktionsapp

Som vi tog upp i föregående enhet innehåller Azure mallar som hjälper dig att komma igång med att skapa funktioner. I den här övningen använder vi mallen HttpTrigger till att implementera temperaturtjänsten.

1. Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true) med ditt Azure-konto.
1. Välj resursgruppen du skapade i den första övningen genom att välja **Alla resurser** på menyn till vänster och sedan välja **escalator-functions-group**.
1. Gruppens resurser visas. Öppna funktionsappen du skapade i föregående övning genom att välja objektet **escalator-functions-xxxxxxx** (identifieras med blixtikonen).
  ![Skärmbild av Alla resurser på portalen med fokus på escalator-functions-group och funktionsappen vi skapade.](../images/6-access-function-app.png)
1. På menyn till vänster visas funktionsappens namn och en undermeny med tre objekt – *Funktioner*, *Proxyservrar* och *Platser*.  Du börjar skapa den första funktionen genom att flytta musen till **Funktioner** i navigeringsträdet och klicka på knappen **+** som visas.
  ![Skärmbild med plustecknet när du hovrar över menykommandot Funktioner som, när du klickar på det, startar proceduren för att skapa funktioner.](../images/5-function-add-button.png)
1. På skärmen Snabbstart väljer du länken **Anpassad funktion** i avsnittet **Kom igång på egen hand** enligt följande skärmbild.
  ![Skärmbild av skärmen Snabbstart med knappen Anpassad funktion markerad.](../images/6-custom-function.png)
1. I listan över mallar som visas på skärmen väljer du JavaScript-implementeringen av HTTP-utlösarmallen enligt följande skärmbild.
  ![Skärmbild av listan över mallar, med HTTP-utlösaren och JavaScript-alternativet markerade.](../images/6-httptrigger-template.png)
1.  Ange **DriveGearTemperatureService** i namnfältet i dialogrutan **Ny funktion** som visas. När du har namngett funktionen trycker du på knappen **Skapa**, enligt följande skärmbild.
  ![Skärmbild av formuläret Ny funktion, med namnfältet markerat och ifyllt med värdet DriveGearTemperatureService](../images/6-create-httptrigger-form.png)
1. När funktionen har skapats öppnas kodredigeraren med innehållet i kodfilen *index.js*. Standardkoden som mallen har genererat åt oss visas i följande kodavsnitt.

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

Funktionen förväntar sig att ett namn skickas via HTTP-begärans frågesträng eller som en del av begärandetexten. Funktionen svarar genom att returnera meddelandet **Hej {name}**, med det namn som skickades i begäran.

1. Till höger i källvyn ser du två flikar. I **vyfilen visas koden och konfigurationsfilen för funktionen.  Välj **function.json** om du vill se funktionens konfiguration. I följande kodavsnitt visas innehållet i **function.json**-filen. Den här konfigurationen deklarerar att funktionen körs när den får en HTTP-begäran. Utdatabindningen deklarerar att svaret skickas som ett HTTP-svar.

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
> [!TIP]
> Observera att utlösaren i föregående JSON-fil har definierats i en matris med bindningar. En utlösare är faktiskt en särskild typ av bindning.

## <a name="run-our-function"></a>Kör funktionen

> [!TIP]
> **cURL** är ett kommandoradsverktyg som kan användas för att skicka och ta emot filer. cURL.exe har stöd för en rad protokoll, till exempel HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP och POP3. Du kan läsa mer genom att följa länkarna nedan:
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

Om du vill testa funktionen kan du skicka en HTTP-begäran till funktions-URL:en med hjälp av cURL på kommandoraden. Du hittar funktionens slutpunktsadress genom att gå tillbaka till funktionskoden och välja länken **Hämta funktionswebbadress**, enligt följande skärmbild. Kopiera den här länken till Urklipp.  

 ![Skärmbild av kodredigeraren i avsnittet Funktionsappar på portalen. Kommandot Hämta funktionswebbadress är markerat längst upp till höger.](../images/6-get-function-url.png)

Med hjälp av den här funktionswebbadressen kör du följande cURL-kommando från kommandoraden och ersätter URL:en med din egen.

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

På följande skärmbild visas ett exempel på svaret du får från standardimplementeringen på kommandoraden.

![Skärmbild av ett exempel på cUrl-kommando och svar på en kommandorad.](../images/6-premadefunction-curl.png)

## <a name="add-business-logic-to-the-function"></a>Lägg till affärslogik i funktionen

Det är dags att lägga till logiken i funktionen som kontrollerar temperaturavläsningar som den tar emot och anger en status för varje.

Funktionen förväntar sig en matris med temperaturavläsningar. Följande JSON-kodavsnitt är ett exempel på begärandetexten som vi skickar till funktionen. Varje `reading`-post har ett ID, en tidsstämpel och en temperatur.

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

Nu ersätter vi standardkoden i funktionen med följande kod som implementerar vår affärslogik. 

1. Öppna filen **index.js** och ersätt den med följande kod.

```javascript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        req.body.readings.forEach(function(reading) {
            
            if(reading.temperature<=25) {
                reading.status = 'OK';
            } else if (reading.temperature<=50) {
                reading.status = 'CAUTION';
            } else {
                reading.status = 'DANGER'
            }
            context.log('Reading is ' + reading.status);
        });

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
Logiken vi har lagt till är enkel. Vi itererar över matrisen med avläsningar och kontrollerar temperaturfältet. Beroende på fältets värde anger vi status **OK**, **CAUTION** eller **DANGER**. Sedan skickar vi tillbaka matrisen med avläsningar med ett tillagt statusfält i varje post.

Notera logginstruktionerna. När funktionen körs lägger instruktionerna till meddelanden i loggfönstret.

## <a name="test-our-business-logic"></a>Testa vår affärslogik

I det här fallet testar vi funktionen med hjälp av **Test**fönstret på portalen.

1. Öppna **Test**fönstret från den utfällbara menyn till höger.
1. Klistra in exempelbegäran ovan i textrutan för begärandetexten. 
1. Välj **Kör** och visa svaret i utdatafönstret. Om du vill se loggmeddelanden öppnar du fliken **Loggar** på den nedre utfällbara menyn på sidan. På följande skärmbild visas ett exempelsvar i utdatafönstret och meddelanden i fönstret **Loggar**.

![Skärmbild av flikarna Test och Loggar i funktionsgränssnittet på portalen. Ett exempelsvar från funktionen visas i utdatafönstret på fliken Test.](../images/6-portal-testing.png)

Du kan se i utdatafönstret att statusfältet har lagts till i alla avläsningar.

Om du navigerar till instrumentpanelen **Övervaka** ser du att begäran har loggats i Application Insights.

![Skärmbild av en del av instrumentpanelen Övervaka med ett meddelande om slutförande från funktionen.](../images/6-app-insights.png)

