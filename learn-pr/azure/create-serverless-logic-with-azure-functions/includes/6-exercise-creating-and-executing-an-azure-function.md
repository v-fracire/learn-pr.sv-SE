<span data-ttu-id="2fa6c-101">I den här övningen fortsätter vi vårt drevexempel och lägger till logik för temperaturtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-101">In this exercise, we'll continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="2fa6c-102">Mer specifikt tar vi emot data från en HTTP-förfrågning.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="2fa6c-103">Funktionskrav</span><span class="sxs-lookup"><span data-stu-id="2fa6c-103">Function requirements</span></span>
<span data-ttu-id="2fa6c-104">Nu ska vi definiera en del krav för vår logik:</span><span class="sxs-lookup"><span data-stu-id="2fa6c-104">Let's define some requirements for our logic:</span></span>
- <span data-ttu-id="2fa6c-105">temperaturer mellan 0–25 ska flaggas som **OK**</span><span class="sxs-lookup"><span data-stu-id="2fa6c-105">temperatures between 0-25 should be flagged as **OK**</span></span>
- <span data-ttu-id="2fa6c-106">temperaturer mellan 26–50 ska flaggas som **CAUTION**</span><span class="sxs-lookup"><span data-stu-id="2fa6c-106">temperatures between 26-50 should be flagged as **CAUTION**</span></span>
- <span data-ttu-id="2fa6c-107">temperaturer över 50 ska flaggas som **DANGER**</span><span class="sxs-lookup"><span data-stu-id="2fa6c-107">temperatures above 50 should be flagged as **DANGER**</span></span>

## <a name="add-a-function-to-an-azure-function-app"></a><span data-ttu-id="2fa6c-108">Lägg till en funktion i en Azure-funktionsapp</span><span class="sxs-lookup"><span data-stu-id="2fa6c-108">Add a function to an Azure function app</span></span>

<span data-ttu-id="2fa6c-109">Som vi tog upp i föregående enhet innehåller Azure mallar som hjälper dig att komma igång med att skapa funktioner.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-109">As we discussed in the preceding unit, Azure provides templates that help you get started building functions.</span></span> <span data-ttu-id="2fa6c-110">I den här övningen ska vi använda mallen HttpTrigger för att implementera temperaturtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-110">In this exercise, we'll use the HttpTrigger template to implement the temperature service.</span></span>

1. <span data-ttu-id="2fa6c-111">Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true) med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-111">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true) using your Azure account.</span></span>
1. <span data-ttu-id="2fa6c-112">Välj resursgruppen som du skapade i den första övningen genom att välja **Alla resurser** på menyn till vänster och sedan välja **escalator-functions-group**.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-112">Select the resource group you created in the first exercise by choosing **All resources** in the left-hand menu, and then selecting **escalator-functions-group**.</span></span>
1. <span data-ttu-id="2fa6c-113">Gruppens resurser visas.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-113">The resources for the group will then be displayed.</span></span> <span data-ttu-id="2fa6c-114">Öppna funktionsappen du skapade i föregående övning genom att välja objektet **escalator-functions-xxxxxxx** (identifieras med blixtikonen).</span><span class="sxs-lookup"><span data-stu-id="2fa6c-114">Access the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt icon).</span></span>
  <span data-ttu-id="2fa6c-115">![Skärmbild av Alla resurser på portalen med fokus på escalator-functions-group och funktionsappen vi skapade.](../images/6-access-function-app.png)</span><span class="sxs-lookup"><span data-stu-id="2fa6c-115">![Screenshot of All Resources in the portal highlighting the escalator-functions-group and the function app we created.](../images/6-access-function-app.png)</span></span>
1. <span data-ttu-id="2fa6c-116">På menyn till vänster visas funktionsappens namn och en undermeny med tre objekt – *Funktioner*, *Proxyservrar* och *Platser*.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-116">The left-side menu displays your function app name and a submenu with three items - *Functions*, *Proxies*, and *Slots*.</span></span>  <span data-ttu-id="2fa6c-117">Du börjar skapa den första funktionen genom att flytta musen till **Funktioner** i navigeringsträdet och klicka på knappen **+** som visas.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-117">To start creating our first function, move your mouse over **Functions** in the navigation tree and click  the **+** button that appears.</span></span>
  <span data-ttu-id="2fa6c-118">![Skärmbild med plustecknet när du hovrar över menykommandot Funktioner som, när du klickar på det, startar proceduren för att skapa funktioner.](../images/5-function-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="2fa6c-118">![Screenshot showing plus sign when you hover over the Functions menu item that, when clicked, starts the function creation procedure.](../images/5-function-add-button.png)</span></span>
1. <span data-ttu-id="2fa6c-119">På skärmen Snabbstart väljer du länken **Anpassad funktion** i avsnittet **Kom igång på egen hand** enligt följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-119">In the Quickstart screen, select the **Custom function** link in the **Get started on your own** section as shown in the following screenshot.</span></span>
  <span data-ttu-id="2fa6c-120">![Skärmbild av skärmen Snabbstart med knappen Anpassad funktion markerad.](../images/6-custom-function.png)</span><span class="sxs-lookup"><span data-stu-id="2fa6c-120">![Screenshot of Quickstart screen with the "Custom function" button highlighted.](../images/6-custom-function.png)</span></span>
1. <span data-ttu-id="2fa6c-121">I listan över mallar som visas på skärmen väljer du JavaScript-implementeringen av HTTP-utlösarmallen enligt följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-121">From the list of templates displayed on the screen, select the JavaScript implementation of the HTTP trigger template as shown in the following screenshot.</span></span>
  <span data-ttu-id="2fa6c-122">![Skärmbild av listan över mallar, med HTTP-utlösaren och JavaScript-alternativet markerade.](../images/6-httptrigger-template.png)</span><span class="sxs-lookup"><span data-stu-id="2fa6c-122">![Screenshot of template list, with the HTTP trigger and JavaScript option highlighted.](../images/6-httptrigger-template.png)</span></span>
1.  <span data-ttu-id="2fa6c-123">Ange **DriveGearTemperatureService** i namnfältet i dialogrutan **Ny funktion** som visas.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-123">Enter **DriveGearTemperatureService** in the name field of the **New Function** dialog that appears.</span></span> <span data-ttu-id="2fa6c-124">När du har namngett funktionen trycker du på knappen **Skapa**, enligt följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-124">After you have named your function, press the **Create** button, as illustrated in te following screenshot.</span></span>
  <span data-ttu-id="2fa6c-125">![Skärmbild av formuläret Ny funktion, med namnfältet markerat och ifyllt med värdet DriveGearTemperatureService](../images/6-create-httptrigger-form.png)</span><span class="sxs-lookup"><span data-stu-id="2fa6c-125">![Screenshot of New Function form, showing Name field highlighted and set with the value "DriveGearTemperatureService"](../images/6-create-httptrigger-form.png)</span></span>
1. <span data-ttu-id="2fa6c-126">När funktionen har skapats öppnas kodredigeraren med innehållet i kodfilen *index.js*.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-126">When your function creation completes, the code editor opens with the contents of the *index.js* code file.</span></span> <span data-ttu-id="2fa6c-127">Standardkoden som mallen har genererat åt oss visas i följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-127">The default code that the template generated for us is listed in the following snippet.</span></span>

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

<span data-ttu-id="2fa6c-128">Funktionen förväntar sig att ett namn skickas i frågesträngen för HTTP-begäran eller som en del av själva begäran.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-128">Our function expects a name to be passed in either through the HTTP request query string or as part of the request body.</span></span> <span data-ttu-id="2fa6c-129">Funktionen svarar genom att returnera meddelandet **Hej {name}**, med det namn som skickades i begäran.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-129">The function responds by returning the message  **Hello, {name}**, echoing back the name that was sent in the request.</span></span>

1. <span data-ttu-id="2fa6c-130">Till höger i källvyn ser du två flikar.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-130">On the right-hand side of the source view, you'll see two tabs.</span></span> <span data-ttu-id="2fa6c-131">I \*\*vyfilen visas koden och konfigurationsfilen för funktionen.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-131">The \*\*View file lists the code and config file for your function.</span></span>  <span data-ttu-id="2fa6c-132">Välj **function.json** om du vill se funktionens konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-132">Select **function.json** to view the configuration of the function.</span></span> <span data-ttu-id="2fa6c-133">I följande kodavsnitt visas innehållet i **function.json**-filen.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-133">The following snippet shows the contents of the **function.json** file.</span></span> <span data-ttu-id="2fa6c-134">Den här konfigurationen deklarerar att funktionen körs när den får en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-134">This configuration declares that the function is runs when it receives an HTTP request.</span></span> <span data-ttu-id="2fa6c-135">Utdatabindningen deklarerar att svaret skickas som ett HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-135">The output binding declares that the response will be sent as an HTTP response.</span></span>

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
> <span data-ttu-id="2fa6c-136">Observera att utlösaren i föregående JSON-fil har definierats i en matris med bindningar.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-136">Notice in the preceding JSON file that the trigger is defined in an array of bindings.</span></span> <span data-ttu-id="2fa6c-137">En utlösare är faktiskt en särskild typ av bindning.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-137">So, a trigger is actually a special type of binding.</span></span>

## <a name="run-our-function"></a><span data-ttu-id="2fa6c-138">Kör funktionen</span><span class="sxs-lookup"><span data-stu-id="2fa6c-138">Run our function</span></span>

> [!TIP]
> <span data-ttu-id="2fa6c-139">**cURL** är ett kommandoradsverktyg som kan användas för att skicka och ta emot filer.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-139">**cURL** is a command line tool that can be used to send or receive files.</span></span> <span data-ttu-id="2fa6c-140">cURL.exe har stöd för en rad protokoll, till exempel HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP och POP3. Du kan läsa mer genom att följa länkarna nedan:</span><span class="sxs-lookup"><span data-stu-id="2fa6c-140">cURL.exe supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 etc. For more information please refer the below links:</span></span>
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

<span data-ttu-id="2fa6c-141">Om du vill testa funktionen kan du skicka en HTTP-begäran till funktions-URL:en med hjälp av cURL på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-141">To test our function, you can send an HTTP request to the function URL using cURL on the command line.</span></span> <span data-ttu-id="2fa6c-142">Du hittar funktionens slutpunktsadress genom att gå tillbaka till funktionskoden och välja länken **Hämta funktionswebbadress**, enligt följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-142">To find the endpoint URL of the function, return to your function code and select the **Get function URL** link, as shown in the following screenshot.</span></span> <span data-ttu-id="2fa6c-143">Kopiera den här länken till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-143">Copy this link to your clipboard.</span></span>  

 ![Skärmbild av kodredigeraren i avsnittet Funktionsappar på portalen.](../images/6-get-function-url.png)

<span data-ttu-id="2fa6c-146">Med hjälp av den här funktionswebbadressen kör du följande cURL-kommando från kommandoraden och ersätter URL:en med din egen.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-146">Using this function URL, run the following cURL command from your command line, replacing the URL with your own.</span></span>

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

<span data-ttu-id="2fa6c-147">På följande skärmbild visas ett exempel på svaret du får från standardimplementeringen på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-147">The following screenshot shows an example of the response you receive from the default implementation in the command line.</span></span>

![Skärmbild av ett exempel på cUrl-kommando och svar på en kommandorad.](../images/6-premadefunction-curl.png)

## <a name="add-business-logic-to-the-function"></a><span data-ttu-id="2fa6c-149">Lägg till affärslogik i funktionen</span><span class="sxs-lookup"><span data-stu-id="2fa6c-149">Add business logic to the function</span></span>

<span data-ttu-id="2fa6c-150">Det är dags att lägga till logiken i funktionen som kontrollerar temperaturavläsningar som den tar emot och anger en status för varje.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-150">It's time to add the logic to the function that checks temperature readings that it receives and sets a status for each.</span></span>

<span data-ttu-id="2fa6c-151">Funktionen förväntar sig en matris med temperaturavläsningar.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-151">Our function is expecting an array of temperature readings.</span></span> <span data-ttu-id="2fa6c-152">Följande JSON-kodavsnitt är ett exempel på begäran som vi ska skicka till funktionen.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-152">The following JSON snippet is an example of the request body that we'll send to our function.</span></span> <span data-ttu-id="2fa6c-153">Varje `reading`-post har ett ID, en tidsstämpel och en temperatur.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-153">Each `reading` entry has an ID, timestamp, and temperature.</span></span>

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

<span data-ttu-id="2fa6c-154">Nu ersätter vi standardkoden i funktionen med följande kod som implementerar vår affärslogik.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-154">Next, we'll replace the default code in our function with the following code that implements our business logic.</span></span> 

1. <span data-ttu-id="2fa6c-155">Öppna filen **index.js** och ersätt den med följande kod.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-155">Open the **index.js** file, and replace it with the following code.</span></span>

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
<span data-ttu-id="2fa6c-156">Logiken vi har lagt till är enkel.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-156">The logic we added is straightforward.</span></span> <span data-ttu-id="2fa6c-157">Vi itererar över matrisen med avläsningar och kontrollerar temperaturfältet.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-157">We iterate over the array of readings and check the temperature field.</span></span> <span data-ttu-id="2fa6c-158">Beroende på fältets värde anger vi statusen **OK**, **CAUTION** eller **DANGER**.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-158">Depending on the value of that field, we set a status of **OK**, **CAUTION**, or **DANGER**.</span></span> <span data-ttu-id="2fa6c-159">Sedan skickar vi tillbaka matrisen med avläsningar med ett tillagt statusfält i varje post.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-159">We then send back the array of readings with a status field added to each entry.</span></span>

<span data-ttu-id="2fa6c-160">Notera logginstruktionerna.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-160">Notice the log statements.</span></span> <span data-ttu-id="2fa6c-161">När funktionen körs lägger instruktionerna till meddelanden i loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-161">When the function runs, these statements will add messages in the log window.</span></span>

## <a name="test-our-business-logic"></a><span data-ttu-id="2fa6c-162">Testa affärslogiken</span><span class="sxs-lookup"><span data-stu-id="2fa6c-162">Test our business logic</span></span>

<span data-ttu-id="2fa6c-163">I det här fallet ska vi testa funktionen med hjälp av **testfönstret** på portalen.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-163">In this case, we're going to use the **Test** pane in the portal to test our function.</span></span>

1. <span data-ttu-id="2fa6c-164">Öppna fönstret **Test** från den utfällbara menyn till höger.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-164">Open the **Test** window from the right-hand side flyout menu.</span></span>
1. <span data-ttu-id="2fa6c-165">Klistra in exempelbegäran ovan i textrutan för begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-165">Paste the sample request from above into the request body text box.</span></span> 
1. <span data-ttu-id="2fa6c-166">Välj **Kör** och visa svaret i utdatafönstret.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-166">Select **Run** and view the response in the output pane.</span></span> <span data-ttu-id="2fa6c-167">Om du vill visa loggmeddelanden öppnar du fliken **Loggar** på den nedre utfällbara menyn på sidan.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-167">To see log messages, open the **Logs** tab in the bottom flyout of the page.</span></span> <span data-ttu-id="2fa6c-168">På följande skärmbild visas ett exempelsvar i utdatafönstret och meddelanden i fönstret **Loggar**.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-168">The following screenshot shows an example response in the output pane and messages in the  **Logs** pane.</span></span>

![Skärmbild av flikarna Test och Loggar i funktionsgränssnittet på portalen.](../images/6-portal-testing.png)

<span data-ttu-id="2fa6c-171">Du kan se i utdatafönstret att vårt statusfält har lagts till i alla avläsningar.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-171">You can see in the output pane that our status field has been correctly added to each of the readings.</span></span>

<span data-ttu-id="2fa6c-172">Om du navigerar till instrumentpanelen **Övervaka** ser du att begäran har loggats i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2fa6c-172">If you navigate to the **Monitor** dashboard, you'll see that the request has been logged to Application Insights.</span></span>

![Skärmbild av en del av instrumentpanelen Övervaka med ett meddelande från funktionen om att åtgärden har slutförts.](../images/6-app-insights.png)

