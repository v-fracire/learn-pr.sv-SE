<span data-ttu-id="43395-101">Vi fortsätter med vårt exempel om kugghjulsdrift och lägger till logiken för temperaturtjänsten.</span><span class="sxs-lookup"><span data-stu-id="43395-101">Let's continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="43395-102">Mer specifikt tar vi emot data från en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="43395-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="43395-103">Funktionskrav</span><span class="sxs-lookup"><span data-stu-id="43395-103">Function requirements</span></span>
<span data-ttu-id="43395-104">Först måste vi definiera vissa krav för vår logik:</span><span class="sxs-lookup"><span data-stu-id="43395-104">First, we need to define some requirements for our logic:</span></span>

- <span data-ttu-id="43395-105">Temperaturer mellan 0–25 ska flaggas som **OK**</span><span class="sxs-lookup"><span data-stu-id="43395-105">Temperatures between 0-25 should be flagged as **OK**</span></span>
- <span data-ttu-id="43395-106">Temperaturer mellan 26–50 ska flaggas som **CAUTION**</span><span class="sxs-lookup"><span data-stu-id="43395-106">Temperatures between 26-50 should be flagged as **CAUTION**</span></span>
- <span data-ttu-id="43395-107">Temperaturer över 50 ska flaggas som **DANGER**</span><span class="sxs-lookup"><span data-stu-id="43395-107">Temperatures above 50 should be flagged as **DANGER**</span></span>

## <a name="add-a-function-to-our-function-app"></a><span data-ttu-id="43395-108">Lägga till en funktion i funktionsappen</span><span class="sxs-lookup"><span data-stu-id="43395-108">Add a function to our function app</span></span>

<span data-ttu-id="43395-109">Som du såg i föregående kursdel innehåller Azure mallar som hjälper dig att komma igång med att skapa funktioner.</span><span class="sxs-lookup"><span data-stu-id="43395-109">As we discussed in the preceding unit, Azure provides templates that help you get started building functions.</span></span> <span data-ttu-id="43395-110">I den här övningen ska vi använda mallen HttpTrigger för att implementera temperaturtjänsten.</span><span class="sxs-lookup"><span data-stu-id="43395-110">In this exercise, we'll use the HttpTrigger template to implement the temperature service.</span></span>

1. <span data-ttu-id="43395-111">Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true) med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="43395-111">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true) using your Azure account.</span></span>

2. <span data-ttu-id="43395-112">Välj resursgruppen som du skapade i den första övningen genom att välja **Alla resurser** på menyn till vänster och sedan välja **escalator-functions-group**.</span><span class="sxs-lookup"><span data-stu-id="43395-112">Select the resource group you created in the first exercise by choosing **All resources** in the left-hand menu, and then selecting **escalator-functions-group**.</span></span>

3. <span data-ttu-id="43395-113">Gruppens resurser visas.</span><span class="sxs-lookup"><span data-stu-id="43395-113">The resources for the group will then be displayed.</span></span> <span data-ttu-id="43395-114">Öppna funktionsappen som du skapade i föregående övning genom att välja objektet **escalator-functions-xxxxxxx** (visas med blixtikonen).</span><span class="sxs-lookup"><span data-stu-id="43395-114">Access the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt icon).</span></span>

  ![Skärmbild av Alla resurser på portalen med fokus på escalator-functions-group och funktionsappen som vi skapade.](../images/6-access-function-app.png)

4. <span data-ttu-id="43395-116">På menyn till vänster visas funktionsappens namn och en undermeny med tre objekt: *Funktioner*, *Proxyservrar* och *Platser*.</span><span class="sxs-lookup"><span data-stu-id="43395-116">The left-side menu displays your function app name and a submenu with three items: *Functions*, *Proxies*, and *Slots*.</span></span>  <span data-ttu-id="43395-117">Du börjar att skapa den första funktionen genom att flytta musen till **Funktioner** i navigeringsträdet och klicka på knappen **+** som visas.</span><span class="sxs-lookup"><span data-stu-id="43395-117">To start creating our first function, move your mouse over **Functions** in the navigation tree and click  the **+** button that appears.</span></span>

  ![Skärmbild som visar plustecknet när du hovrar över menyobjektet Funktioner. När du klickar på objektet startar proceduren för att skapa funktioner.](../images/5-function-add-button.png)

5. <span data-ttu-id="43395-119">På skärmen Snabbstart väljer du länken **Anpassad funktion** i avsnittet **Kom igång på egen hand**, som du ser i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="43395-119">In the Quickstart screen, select the **Custom function** link in the **Get started on your own** section as shown in the following screenshot.</span></span>
 
  ![Skärmbild av skärmen Snabbstart med knappen Anpassad funktion markerad.](../images/6-custom-function.png)

6. <span data-ttu-id="43395-121">I listan över mallar som visas på skärmen väljer du JavaScript-implementeringen av HTTP-utlösarmallen, som du ser i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="43395-121">From the list of templates displayed on the screen, select the JavaScript implementation of the HTTP trigger template as shown in the following screenshot.</span></span>

  ![Skärmbild av listan över mallar, med HTTP-utlösaren och JavaScript-alternativet markerade.](../images/6-httptrigger-template.png)

7.  <span data-ttu-id="43395-123">Ange **DriveGearTemperatureService** i namnfältet i dialogrutan **Ny funktion** som visas.</span><span class="sxs-lookup"><span data-stu-id="43395-123">Enter **DriveGearTemperatureService** in the name field of the **New Function** dialog that appears.</span></span> <span data-ttu-id="43395-124">Lämna Auktoriseringsnivå som ”Funktion” och tryck på knappen **Skapa** för att skapa funktionen.</span><span class="sxs-lookup"><span data-stu-id="43395-124">Leave the Authorization level as "Function" and press the **Create** button to create the function.</span></span>

  ![Skärmbild av formuläret Ny funktion, med namnfältet markerat och ifyllt med värdet ”DriveGearTemperatureService”](../images/6-create-httptrigger-form.png)

8. <span data-ttu-id="43395-126">När funktionen har skapats öppnas kodredigeraren med innehållet i kodfilen *index.js*.</span><span class="sxs-lookup"><span data-stu-id="43395-126">When your function creation completes, the code editor opens with the contents of the *index.js* code file.</span></span> <span data-ttu-id="43395-127">Standardkoden som mallen har genererat åt oss visas i följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="43395-127">The default code that the template generated for us is listed in the following snippet.</span></span>

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

<span data-ttu-id="43395-128">Funktionen förväntar sig att ett namn skickas i frågesträngen för HTTP-begäran eller som en del av själva begäran.</span><span class="sxs-lookup"><span data-stu-id="43395-128">Our function expects a name to be passed in either through the HTTP request query string or as part of the request body.</span></span> <span data-ttu-id="43395-129">Funktionen svarar genom att returnera meddelandet **Hello, {name}**, med namnet som skickades i begäran.</span><span class="sxs-lookup"><span data-stu-id="43395-129">The function responds by returning the message  **Hello, {name}**, echoing back the name that was sent in the request.</span></span>

<span data-ttu-id="43395-130">Till höger i källvyn ser du två flikar.</span><span class="sxs-lookup"><span data-stu-id="43395-130">On the right-hand side of the source view, you'll find two tabs.</span></span> <span data-ttu-id="43395-131">**Visa fil** visar koden och konfigurationsfilen för funktionen.</span><span class="sxs-lookup"><span data-stu-id="43395-131">The **View file** lists the code and config file for your function.</span></span>  <span data-ttu-id="43395-132">Välj **function.json** för att visa funktionens konfiguration, som bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="43395-132">Select **function.json** to view the configuration of the function which should look like the following:</span></span> 

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

<span data-ttu-id="43395-133">Den här konfigurationen deklarerar att funktionen körs när den får en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="43395-133">This configuration declares that the function is runs when it receives an HTTP request.</span></span> <span data-ttu-id="43395-134">Utdatabindningen deklarerar att svaret skickas som ett HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="43395-134">The output binding declares that the response will be sent as an HTTP response.</span></span>

## <a name="test-the-function-using-curl"></a><span data-ttu-id="43395-135">Testa funktionen med cURL</span><span class="sxs-lookup"><span data-stu-id="43395-135">Test the function using cURL</span></span>

> [!TIP]
> <span data-ttu-id="43395-136">**cURL** är ett kommandoradsverktyg som kan användas för att skicka och ta emot filer.</span><span class="sxs-lookup"><span data-stu-id="43395-136">**cURL** is a command line tool that can be used to send or receive files.</span></span> <span data-ttu-id="43395-137">Det ingår i Linux, macOS och Windows 10 och kan laddas ned för de flesta andra operativsystem.</span><span class="sxs-lookup"><span data-stu-id="43395-137">It's included with Linux, macOS, and Windows 10, and can be downloaded for most other operating systems.</span></span> <span data-ttu-id="43395-138">cURL har stöd för en rad protokoll, till exempel HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP och POP3. Du kan läsa mer genom att följa länkarna nedan:</span><span class="sxs-lookup"><span data-stu-id="43395-138">cURL supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 etc. For more information please refer the below links:</span></span>
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

<span data-ttu-id="43395-139">Om du vill testa funktionen kan du skicka en HTTP-begäran till funktions-URL:en med hjälp av cURL på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="43395-139">To test the function, you can send an HTTP request to the function URL using cURL on the command line.</span></span> <span data-ttu-id="43395-140">Du hittar funktionens slutpunktsadress genom att gå tillbaka till funktionskoden och välja länken **Hämta funktionswebbadress**, som du ser i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="43395-140">To find the endpoint URL of the function, return to your function code and select the **Get function URL** link, as shown in the following screenshot.</span></span> <span data-ttu-id="43395-141">Spara den här länken tills vidare.</span><span class="sxs-lookup"><span data-stu-id="43395-141">Save this link off temporarily.</span></span>  

 ![Skärmbild av kodredigeraren i avsnittet Funktionsappar på portalen.](../images/6-get-function-url.png)

### <a name="securing-http-triggers"></a><span data-ttu-id="43395-144">Skydda HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="43395-144">Securing HTTP triggers</span></span>
<span data-ttu-id="43395-145">Med HTTP-utlösare kan du använda API-nycklar för att blockera okända anropare genom att kräva att nyckeln måste finnas i varje begäran.</span><span class="sxs-lookup"><span data-stu-id="43395-145">HTTP triggers let you use API keys to block unknown callers by requiring the key to be present on each request.</span></span> <span data-ttu-id="43395-146">När du skapar en funktion väljer du _auktoriseringsnivån_.</span><span class="sxs-lookup"><span data-stu-id="43395-146">When you create a function, you select the _authorization level_.</span></span> <span data-ttu-id="43395-147">Standardinställningen är ”Funktion”, vilket kräver en funktionsspecifik API-nyckel, men den kan också anges till ”Administratör” för användning av en global ”huvudnyckel” eller till ”Anonym” om ingen nyckel krävs.</span><span class="sxs-lookup"><span data-stu-id="43395-147">By default, it's set to "Function" which requires a function-specific API key, but it can also be set to "Admin" to use a global "master" key, or "Anonymous" to indicate that no key is required.</span></span> <span data-ttu-id="43395-148">Du kan också ändra åtkomstnivån via funktionens egenskaper när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="43395-148">You can also change the authorization level through the function properties after creation.</span></span>

<span data-ttu-id="43395-149">Eftersom vi valde ”Funktion” när vi skapade funktionen måste vi ange nyckeln när vi skickar HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="43395-149">Since we specified "Function" when we created this function, we will need to supply the key when we send the HTTP request.</span></span> <span data-ttu-id="43395-150">Du kan skicka den som en frågesträngsparameter med namnet `code`, eller som ett HTTP-huvud (rekommenderas) med namnet `x-functions-key`.</span><span class="sxs-lookup"><span data-stu-id="43395-150">You can send it as a query string parameter named `code`, or as an HTTP header (preferred) named `x-functions-key`.</span></span>

<span data-ttu-id="43395-151">Funktionen och huvudnycklarna visas i avsnittet **Hantera** när funktionen har expanderats.</span><span class="sxs-lookup"><span data-stu-id="43395-151">The function and master keys are found in the **Manage** section when the function is expanded.</span></span> <span data-ttu-id="43395-152">De är dolda som standard och du måste uttryckligen visa dem.</span><span class="sxs-lookup"><span data-stu-id="43395-152">By default, they are hidden, and you need to display them.</span></span>

1. <span data-ttu-id="43395-153">Expandera funktionen och välj avsnittet **Hantera**. Visa standardfunktionsnyckeln och kopiera den till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="43395-153">Expand your function and select the **Manage** section, show the default Function Key and copy it to the clipboard.</span></span>

![Blad för att hämta funktionsnyckeln från Azure Portal](../images/6-get-function-key.png)

2. <span data-ttu-id="43395-155">Formatera sedan ett cURL-kommando med URL:en för din funktion och funktionsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="43395-155">Next, format a cURL command with the URL for your function, and the Function key.</span></span>
    - <span data-ttu-id="43395-156">Använd en `POST`-begäran.</span><span class="sxs-lookup"><span data-stu-id="43395-156">Use a `POST` request.</span></span>
    - <span data-ttu-id="43395-157">Lägg till ett `Content-Type`-huvudvärde av typen `application/json`.</span><span class="sxs-lookup"><span data-stu-id="43395-157">Add a `Content-Type` header value of type `application/json`.</span></span>
    - <span data-ttu-id="43395-158">Ersätt URL:en nedan med din egen.</span><span class="sxs-lookup"><span data-stu-id="43395-158">Make sure to replace the URL below with your own.</span></span>
    - <span data-ttu-id="43395-159">Skicka funktionsnyckeln som huvudvärdet `x-functions-key`.</span><span class="sxs-lookup"><span data-stu-id="43395-159">Pass the Function Key as the header value `x-functions-key`.</span></span>

```bash
curl --header "Content-Type: application/json" --header "x-functions-key: VCWjWkBTvWBsnvw0TlbIbtsav3P3J80m/PKe8WclH0C3RSmvG4Sy8w==" --request POST --data "{\"name\": \"Azure Function\"}" https://<your-url-here>/api/DriveGearTemperatureService
```

<span data-ttu-id="43395-160">Funktionen svarar med texten `"Hello Azure Function"`.</span><span class="sxs-lookup"><span data-stu-id="43395-160">The function will respond back with the text `"Hello Azure Function"`.</span></span>

## <a name="add-business-logic-to-the-function"></a><span data-ttu-id="43395-161">Lägga till affärslogik i funktionen</span><span class="sxs-lookup"><span data-stu-id="43395-161">Add business logic to the function</span></span>

<span data-ttu-id="43395-162">Nu ska vi lägga till logiken till funktionen som kontrollerar temperaturavläsningar som tas emot och som anger en status för var och en.</span><span class="sxs-lookup"><span data-stu-id="43395-162">Next, let's add the logic to the function that checks temperature readings that it receives and sets a status for each.</span></span>

<span data-ttu-id="43395-163">Funktionen förväntar sig en matris med temperaturavläsningar.</span><span class="sxs-lookup"><span data-stu-id="43395-163">Our function is expecting an array of temperature readings.</span></span> <span data-ttu-id="43395-164">Följande JSON-kodavsnitt är ett exempel på begäran som vi ska skicka till funktionen.</span><span class="sxs-lookup"><span data-stu-id="43395-164">The following JSON snippet is an example of the request body that we'll send to our function.</span></span> <span data-ttu-id="43395-165">Varje `reading`-post har ett ID, en tidsstämpel och en temperatur.</span><span class="sxs-lookup"><span data-stu-id="43395-165">Each `reading` entry has an ID, timestamp, and temperature.</span></span>

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

<span data-ttu-id="43395-166">Nu ska vi ersätta standardkoden i funktionen med följande kod som implementerar vår affärslogik.</span><span class="sxs-lookup"><span data-stu-id="43395-166">Next, we'll replace the default code in our function with the following code that implements our business logic.</span></span> 

1. <span data-ttu-id="43395-167">Öppna filen **index.js** och ersätt den med följande kod.</span><span class="sxs-lookup"><span data-stu-id="43395-167">Open the **index.js** file and replace it with the following code.</span></span>

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

<span data-ttu-id="43395-168">Logiken som vi lade till är enkel.</span><span class="sxs-lookup"><span data-stu-id="43395-168">The logic we added is straightforward.</span></span> <span data-ttu-id="43395-169">Vi itererar över matrisen med avläsningar och kontrollerar temperaturfältet.</span><span class="sxs-lookup"><span data-stu-id="43395-169">We iterate over the array of readings and check the temperature field.</span></span> <span data-ttu-id="43395-170">Beroende på fältets värde anger vi statusen **OK**, **CAUTION** eller **DANGER**.</span><span class="sxs-lookup"><span data-stu-id="43395-170">Depending on the value of that field, we set a status of **OK**, **CAUTION**, or **DANGER**.</span></span> <span data-ttu-id="43395-171">Sedan skickar vi tillbaka matrisen med avläsningar med ett tillagt statusfält för varje post.</span><span class="sxs-lookup"><span data-stu-id="43395-171">We then send back the array of readings with a status field added to each entry.</span></span>

<span data-ttu-id="43395-172">Observera `log`-instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="43395-172">Notice the `log` statements.</span></span> <span data-ttu-id="43395-173">När funktionen körs lägger instruktionerna till meddelanden i loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="43395-173">When the function runs, these statements will add messages in the log window.</span></span>

## <a name="test-our-business-logic"></a><span data-ttu-id="43395-174">Testa affärslogiken</span><span class="sxs-lookup"><span data-stu-id="43395-174">Test our business logic</span></span>

<span data-ttu-id="43395-175">I det här fallet ska vi testa funktionen med hjälp av **testfönstret** på portalen.</span><span class="sxs-lookup"><span data-stu-id="43395-175">In this case, we're going to use the **Test** pane in the portal to test our function.</span></span>

1. <span data-ttu-id="43395-176">Öppna fönstret **Test** från den utfällbara menyn till höger.</span><span class="sxs-lookup"><span data-stu-id="43395-176">Open the **Test** window from the right-hand side flyout menu.</span></span>

2. <span data-ttu-id="43395-177">Klistra in exempelbegäran ovan i textrutan för begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="43395-177">Paste the sample request from above into the request body text box.</span></span> 

3. <span data-ttu-id="43395-178">Välj **Kör** och visa svaret i utdatafönstret.</span><span class="sxs-lookup"><span data-stu-id="43395-178">Select **Run** and view the response in the output pane.</span></span> <span data-ttu-id="43395-179">Om du vill visa loggmeddelanden öppnar du fliken **Loggar** på den nedre utfällbara menyn på sidan.</span><span class="sxs-lookup"><span data-stu-id="43395-179">To see log messages, open the **Logs** tab in the bottom flyout of the page.</span></span> <span data-ttu-id="43395-180">På följande skärmbild visas ett exempelsvar i utdatafönstret och meddelanden i fönstret **Loggar**.</span><span class="sxs-lookup"><span data-stu-id="43395-180">The following screenshot shows an example response in the output pane and messages in the  **Logs** pane.</span></span>

![Skärmbild av flikarna Test och Loggar i funktionsgränssnittet på portalen.](../images/6-portal-testing.png)

<span data-ttu-id="43395-183">Du kan se i utdatafönstret att vårt statusfält har lagts till i alla avläsningar.</span><span class="sxs-lookup"><span data-stu-id="43395-183">You can see in the output pane that our status field has been correctly added to each of the readings.</span></span>

<span data-ttu-id="43395-184">Om du navigerar till instrumentpanelen **Övervaka** ser du att begäran har loggats i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="43395-184">If you navigate to the **Monitor** dashboard, you'll see that the request has been logged to Application Insights.</span></span>

![Skärmbild av en del av instrumentpanelen Övervaka med ett meddelande från funktionen om att åtgärden har slutförts.](../images/6-app-insights.png)

