<span data-ttu-id="f9807-101">I den här övningen fortsätter vi vårt drevexempel och lägger till logik för temperaturtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f9807-101">In this exercise, we'll continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="f9807-102">Mer specifikt tar vi emot data från en HTTP-förfrågning.</span><span class="sxs-lookup"><span data-stu-id="f9807-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="f9807-103">Funktionskrav</span><span class="sxs-lookup"><span data-stu-id="f9807-103">Function requirements</span></span>
<span data-ttu-id="f9807-104">Nu ska vi definiera en del krav för vår logik:</span><span class="sxs-lookup"><span data-stu-id="f9807-104">Let's define some requirements for our logic:</span></span>
- <span data-ttu-id="f9807-105">temperaturer mellan 0–25 ska flaggas som **OK**</span><span class="sxs-lookup"><span data-stu-id="f9807-105">temperatures between 0-25 should be flagged as **OK**</span></span>
- <span data-ttu-id="f9807-106">temperaturer mellan 26–50 ska flaggas som **CAUTION**</span><span class="sxs-lookup"><span data-stu-id="f9807-106">temperatures between 26-50 should be flagged as **CAUTION**</span></span>
- <span data-ttu-id="f9807-107">temperaturer över 50 ska flaggas som **DANGER**.</span><span class="sxs-lookup"><span data-stu-id="f9807-107">temperatures above 50 should be flagged as **DANGER**</span></span>

## <a name="adding-a-function-to-an-azure-function-app"></a><span data-ttu-id="f9807-108">Lägga till en funktion i en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="f9807-108">Adding a function to an Azure function app</span></span>

<span data-ttu-id="f9807-109">Som vi redan har sett så kan du få hjälp när du lär dig att arbeta i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="f9807-109">As we have already learned, Azure provides a helping hand when you're learning how to work with Azure Functions.</span></span> <span data-ttu-id="f9807-110">En bra funktion när du ska komma igång med Functions är att generera en funktion med någon av de många mallarna.</span><span class="sxs-lookup"><span data-stu-id="f9807-110">One great feature to get your feet wet with Functions is to generate one using one of many templates.</span></span> <span data-ttu-id="f9807-111">I den här övningen använder du mallen HttpTrigger till att implementera temperaturtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f9807-111">In this exercise, you will be using the HttpTrigger template to implement the temperature service.</span></span>

1. <span data-ttu-id="f9807-112">Logga in på [Azure Portal](https://portal.azure.com) med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f9807-112">Sign in to the [Azure portal](https://portal.azure.com) using your Azure account.</span></span>
1. <span data-ttu-id="f9807-113">Öppna resursgruppen du skapade i den första övningen genom att välja **Alla resurser** på menyn till vänster och sedan välja **escalator-functions-group**.</span><span class="sxs-lookup"><span data-stu-id="f9807-113">Access the resource group you created in the first exercise by choosing **All resources** in the left-hand menu, and then selecting **escalator-functions-group**.</span></span>
1. <span data-ttu-id="f9807-114">Gruppens resurser visas.</span><span class="sxs-lookup"><span data-stu-id="f9807-114">The resources for the group will then be displayed.</span></span> <span data-ttu-id="f9807-115">Öppna funktionsappen du skapade i föregående övning genom att välja objektet **escalator-functions-xxxxxxx** (identifieras med blixtikonen).</span><span class="sxs-lookup"><span data-stu-id="f9807-115">Access the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt icon).</span></span>
  <span data-ttu-id="f9807-116">![Öppna funktionsappen](../images/6-access-function-app.png)</span><span class="sxs-lookup"><span data-stu-id="f9807-116">![Access the function app](../images/6-access-function-app.png)</span></span>
1. <span data-ttu-id="f9807-117">Du ser en översikt över funktionsappen på bladet.</span><span class="sxs-lookup"><span data-stu-id="f9807-117">The blade will show an overview of your function app.</span></span> <span data-ttu-id="f9807-118">Dessutom visas ett navigeringsträd till vänster med definierade funktioner, proxys och platser.</span><span class="sxs-lookup"><span data-stu-id="f9807-118">There is also a navigation tree on the left that shows any functions, proxies or slots that are defined.</span></span> <span data-ttu-id="f9807-119">Vi har inte någon funktion ännu, så det här trädet är tomt.</span><span class="sxs-lookup"><span data-stu-id="f9807-119">We don't have a function yet, so this  will be empty.</span></span> <span data-ttu-id="f9807-120">Du skapar en funktion genom att flytta musen till **Funktion** i navigeringsträdet och klicka på knappen **+** som visas.</span><span class="sxs-lookup"><span data-stu-id="f9807-120">To create a function, move your mouse over **Function** in the navigation tree and click  the **+** button that appears.</span></span>
  <span data-ttu-id="f9807-121">![Lägg till funktionsnavigering](../images/5-function-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="f9807-121">![Add function navigation](../images/5-function-add-button.png)</span></span>
1. <span data-ttu-id="f9807-122">I det fördefinierade funktionsformuläret väljer du länken **Anpassad funktion** i avsnittet **Kom igång på egen hand**.</span><span class="sxs-lookup"><span data-stu-id="f9807-122">In the quickstart premade function form, select the **Custom function** link in the **Get started on your own** section.</span></span>
  <span data-ttu-id="f9807-123">![Fördefinierat funktionsformulär](../images/6-custom-function.png)</span><span class="sxs-lookup"><span data-stu-id="f9807-123">![Premade function form](../images/6-custom-function.png)</span></span>
1. <span data-ttu-id="f9807-124">Nu visas en lista med mallar.</span><span class="sxs-lookup"><span data-stu-id="f9807-124">You are now presented with a list of templates.</span></span> <span data-ttu-id="f9807-125">Välj JavaScript-implementeringen av HTTP-utlösarmallen.</span><span class="sxs-lookup"><span data-stu-id="f9807-125">Select the JavaScript implementation of the HTTP trigger template.</span></span>
  <span data-ttu-id="f9807-126">![HTTP-utlösarmall](../images/6-httptrigger-template.png)</span><span class="sxs-lookup"><span data-stu-id="f9807-126">![HTTP trigger template](../images/6-httptrigger-template.png)</span></span>
1. <span data-ttu-id="f9807-127">Du ser ett blad där du kan definiera vad mallen ska generera.</span><span class="sxs-lookup"><span data-stu-id="f9807-127">A blade will appear, allowing you to define what the template will generate.</span></span> <span data-ttu-id="f9807-128">I det här fallet vill du generera en JavaScript-funktion med namnet **DriveGearTemperatureService**.</span><span class="sxs-lookup"><span data-stu-id="f9807-128">In this case, you are interested in generating a JavaScript function named **DriveGearTemperatureService**.</span></span> <span data-ttu-id="f9807-129">När du har namngett funktionen trycker du knappen **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f9807-129">After you have named your function, press the **Create** button.</span></span>
  <span data-ttu-id="f9807-130">![Formulär för att skapa HTTP-utlösare](../images/6-create-httptrigger-form.png)</span><span class="sxs-lookup"><span data-stu-id="f9807-130">![Create HTTP trigger form](../images/6-create-httptrigger-form.png)</span></span>
1. <span data-ttu-id="f9807-131">Efter en stund ser du den mallgenererade källkoden för funktionen.</span><span class="sxs-lookup"><span data-stu-id="f9807-131">After a few moments, you will be presented with the templated source code for your function.</span></span> <span data-ttu-id="f9807-132">Den genererade funktionen väntar sig att du anger ett namn, och därefter genereras **Hello, {namn}**.</span><span class="sxs-lookup"><span data-stu-id="f9807-132">The premade function expects a name to be passed in, and it will return **Hello, {name}**.</span></span>

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

1. <span data-ttu-id="f9807-133">Till höger i källvyn ser du två flikar.</span><span class="sxs-lookup"><span data-stu-id="f9807-133">On the right-hand side of the source view, you will see two tabs.</span></span> <span data-ttu-id="f9807-134">Du kan se alla filer som har stöd för funktionen på fliken **Visa filer**. Du kan välja **function.json** om du vill se funktionens konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f9807-134">You are able to view all the files supporting the function in the **View files** tab. You can select **function.json** to view the configuration of the function.</span></span> <span data-ttu-id="f9807-135">Här kan du se den definierade httpTrigger-funktionen och utdatabindningen.</span><span class="sxs-lookup"><span data-stu-id="f9807-135">Here you can see the httpTrigger defined, as well as the output binding.</span></span> <span data-ttu-id="f9807-136">I konfigurationen beskrivs att funktionen initieras med en HTTP-förfrågning, och utdatabindningen beskriver att svaret skickas som ett HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="f9807-136">This configuration describes that the function is initiated by an HTTP request, and the output binding describes that the response will be sent as an HTTP response.</span></span>

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

## <a name="running-the-premade-azure-function"></a><span data-ttu-id="f9807-137">Köra den fördefinierade Azure-funktionen</span><span class="sxs-lookup"><span data-stu-id="f9807-137">Running the premade Azure function</span></span>

<span data-ttu-id="f9807-138">När du ska köra funktionen kan du initiera en HTTP-förfrågan med cURL från en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="f9807-138">To run the function, you can initiate an HTTP request from cURL from a command prompt.</span></span> <span data-ttu-id="f9807-139">Du hittar slutpunktsadressen genom att gå tillbaka till funktionskoden och välja länken **Hämta funktionswebbadress**.</span><span class="sxs-lookup"><span data-stu-id="f9807-139">To find the endpoint URL, return to your function code and select the **Get function URL** link.</span></span> <span data-ttu-id="f9807-140">Kopiera den här länken till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="f9807-140">Copy this link to your clipboard.</span></span>  <span data-ttu-id="f9807-141">Webbadressen bör se ut ungefär så här: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![Hämta slutpunktsadress](../images/6-get-function-url.png)</span><span class="sxs-lookup"><span data-stu-id="f9807-141">Your URL should look something similar to the following: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![Get endpoint URL](../images/6-get-function-url.png)</span></span>

<span data-ttu-id="f9807-142">Kör ett cURL-kommando med webbadressen för att initiera din funktion (ersätt webbadressen med din egen):</span><span class="sxs-lookup"><span data-stu-id="f9807-142">With that URL, run a cURL command to initiate your function (replacing the URL with your own):</span></span>

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

![cURL-svar från fördefinierad funktion](../images/6-premadefunction-curl.png)

## <a name="adding-business-logic-to-the-function"></a><span data-ttu-id="f9807-144">Lägga till affärslogik i funktionen</span><span class="sxs-lookup"><span data-stu-id="f9807-144">Adding business logic to the function</span></span>

<span data-ttu-id="f9807-145">Vår funktion förväntar sig en matris med temperaturavläsningar från kunden.</span><span class="sxs-lookup"><span data-stu-id="f9807-145">Our function is expecting an array of temperature readings from the customer.</span></span> <span data-ttu-id="f9807-146">Här är ett exempel på en förfrågan:</span><span class="sxs-lookup"><span data-stu-id="f9807-146">This is an example of the request body:</span></span>

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

<span data-ttu-id="f9807-147">Ändra koden i den fördefinierade funktionen så att du implementerar den affärslogik som behövs.</span><span class="sxs-lookup"><span data-stu-id="f9807-147">Modify the premade function code to implement the required business logic.</span></span> <span data-ttu-id="f9807-148">Öppna filen index.js och ersätt den med följande kod:</span><span class="sxs-lookup"><span data-stu-id="f9807-148">Open the index.js file, and replace it with the following listing:</span></span>

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

<span data-ttu-id="f9807-149">Notera logginstruktionerna.</span><span class="sxs-lookup"><span data-stu-id="f9807-149">Notice the log statements.</span></span> <span data-ttu-id="f9807-150">När funktionen körs skickar de meddelanden till loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="f9807-150">When the function runs, they will add messages in the log window.</span></span>

## <a name="testing-your-business-logic"></a><span data-ttu-id="f9807-151">Testa din affärslogik</span><span class="sxs-lookup"><span data-stu-id="f9807-151">Testing your business logic</span></span>

<span data-ttu-id="f9807-152">Öppna testfönstret från menyn till höger och klistra in exempelförfrågan ovan i textrutan.</span><span class="sxs-lookup"><span data-stu-id="f9807-152">Open the test window from the right-hand side flyout menu, and paste the sample request from above into the request body text box.</span></span> <span data-ttu-id="f9807-153">Tryck på knappen **Kör** och läs utdata.</span><span class="sxs-lookup"><span data-stu-id="f9807-153">Press the **Run** button and view the output.</span></span> <span data-ttu-id="f9807-154">Du kan också öppna loggfönstret från menyn längst ned om du vill se loggposterna.</span><span class="sxs-lookup"><span data-stu-id="f9807-154">You can also open the log window from the bottom flyout menu to see the logging statements in the log.</span></span>
<span data-ttu-id="f9807-155">![Testa affärslogiken](../images/6-portal-testing.png) JSON-data i utdatafönstret visar att vårt fält för temperaturstatus har lagts till för var och en av avläsningarna.</span><span class="sxs-lookup"><span data-stu-id="f9807-155">![Testing the business Logic](../images/6-portal-testing.png) You can see from the JSON data in the output window that our temperature status field has been added to each of the readings correctly.</span></span> <span data-ttu-id="f9807-156">Du kan också se att vår förfrågan har loggats i Application Insights på Monitor-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f9807-156">You may also visit the Monitor dashboard to see that the request has been logged to Application Insights.</span></span>
<span data-ttu-id="f9807-157">![Förfrågan i Application Insights](../images/6-app-insights.png)</span><span class="sxs-lookup"><span data-stu-id="f9807-157">![Request in Application Insights](../images/6-app-insights.png)</span></span>
