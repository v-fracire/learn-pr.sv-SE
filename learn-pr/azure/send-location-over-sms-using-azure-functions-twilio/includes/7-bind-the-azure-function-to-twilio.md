<span data-ttu-id="9118d-101">Nu är den mobila appen färdig och kan skicka användarens plats samt listan med telefonnummer till en Azure-funktion som kan avserialisera data.</span><span class="sxs-lookup"><span data-stu-id="9118d-101">At this point, the mobile app is complete and it can send the user's location and list of phone numbers to an Azure function that can deserialize the data.</span></span> <span data-ttu-id="9118d-102">I den här kursdelen ska du binda Azure-funktionen till Twilio för att skicka SMS.</span><span class="sxs-lookup"><span data-stu-id="9118d-102">In this unit, you bind the Azure function to Twilio to send SMS messages.</span></span>

<span data-ttu-id="9118d-103">Du kan ansluta Azure Functions till andra tjänster, antingen tjänster i Azure eller från tredje part.</span><span class="sxs-lookup"><span data-stu-id="9118d-103">Azure Functions can be connected to other services, either services in Azure or third-party services.</span></span> <span data-ttu-id="9118d-104">Sådana anslutningar kallas bindningar och finns i två varianter, in- och utdatabindningar.</span><span class="sxs-lookup"><span data-stu-id="9118d-104">These connections, called bindings, exist in two forms - input and output bindings.</span></span> <span data-ttu-id="9118d-105">Indatabindningar levererar data till funktionen och bindningar hämtar data från funktionen och skickar dem till en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="9118d-105">Input bindings provide data to your function and output bindings take data from your function and send it to another service.</span></span> <span data-ttu-id="9118d-106">Du kan läsa mer om bindningar i [dokumentationen om Azure Functions-bindningar](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).</span><span class="sxs-lookup"><span data-stu-id="9118d-106">You can read about bindings in the [Azure Functions Binding docs](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).</span></span>

<span data-ttu-id="9118d-107">Bindningar för Azure Functions som skapas i Visual Studio definieras med funktionsparametrar och attribut.</span><span class="sxs-lookup"><span data-stu-id="9118d-107">Bindings for Azure Functions created in Visual Studio are defined using parameters to the function, decorated with attributes.</span></span>

## <a name="bind-the-azure-function-to-twilio"></a><span data-ttu-id="9118d-108">Binda Azure-funktionen till Twilio</span><span class="sxs-lookup"><span data-stu-id="9118d-108">Bind the Azure function to Twilio</span></span>

<span data-ttu-id="9118d-109">När du ska skicka SMS via Twilio behöver du en utdatabindning som är konfigurerad med ID:t för din kontoprenumeration (SID) och en autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="9118d-109">Sending SMS messages via Twilio requires an output binding that is configured with your account subscription ID (SID) and Auth Token.</span></span>

1. <span data-ttu-id="9118d-110">Stoppa den lokala körningen av Azure Functions om det fortfarande körs sedan den föregående enheten.</span><span class="sxs-lookup"><span data-stu-id="9118d-110">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

1. <span data-ttu-id="9118d-111">Lägg till NuGet-paketet Microsoft.Azure.WebJobs.Extensions.Twilio i projektet `ImHere.Functions`.</span><span class="sxs-lookup"><span data-stu-id="9118d-111">Add the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package to the `ImHere.Functions` project.</span></span> <span data-ttu-id="9118d-112">Det här NuGet-paketet innehåller de klasser som behövs för bindningen.</span><span class="sxs-lookup"><span data-stu-id="9118d-112">This NuGet package contains the relevant classes for the binding.</span></span>

1. <span data-ttu-id="9118d-113">Lägg till en ny parameter i den statiska metoden `Run` i den statiska klassen `SendLocation` med namnet `messages`.</span><span class="sxs-lookup"><span data-stu-id="9118d-113">Add a new parameter to the static `Run` method on the `SendLocation` static class called `messages`.</span></span> <span data-ttu-id="9118d-114">Den här parametern ska ha typen `ICollector<SMSMessage>`.</span><span class="sxs-lookup"><span data-stu-id="9118d-114">This parameter will have a type of `ICollector<SMSMessage>`.</span></span> <span data-ttu-id="9118d-115">Du måste lägga till ett användningsdirektiv för namnområdet `Twilio`.</span><span class="sxs-lookup"><span data-stu-id="9118d-115">You'll need to add a using directive for the `Twilio` namespace.</span></span>

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                      ICollector<SMSMessage> messages,
                                                      TraceWriter log)
    ```

1. <span data-ttu-id="9118d-116">Utöka den nya parametern `messages` med attributet `TwilioSms`.</span><span class="sxs-lookup"><span data-stu-id="9118d-116">Decorate the new `messages` parameter with the `TwilioSms` attribute.</span></span> <span data-ttu-id="9118d-117">Det här attributet har tre parametrar som du måste ange.</span><span class="sxs-lookup"><span data-stu-id="9118d-117">This attribute has three parameters you need to set.</span></span>

    | <span data-ttu-id="9118d-118">Inställning</span><span class="sxs-lookup"><span data-stu-id="9118d-118">Setting</span></span>      |  <span data-ttu-id="9118d-119">Värde</span><span class="sxs-lookup"><span data-stu-id="9118d-119">Value</span></span>   | <span data-ttu-id="9118d-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9118d-120">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="9118d-121">**AccountSidSetting**</span><span class="sxs-lookup"><span data-stu-id="9118d-121">**AccountSidSetting**</span></span> | <span data-ttu-id="9118d-122">”TwilioAccountSid”</span><span class="sxs-lookup"><span data-stu-id="9118d-122">"TwilioAccountSid"</span></span> | <span data-ttu-id="9118d-123">Aktuellt SID för ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="9118d-123">The SID for your Twilio account.</span></span> <span data-ttu-id="9118d-124">Snarare än att ställa in SID-värdet direkt så är den här parametern namnet på ett värde i appinställningarna som används till att hämta ditt SID.</span><span class="sxs-lookup"><span data-stu-id="9118d-124">Rather than set the SID directly, this parameter is the name of a value in the function app settings that will be used to retrieve the SID.</span></span> |
    | <span data-ttu-id="9118d-125">**AuthTokenSetting**</span><span class="sxs-lookup"><span data-stu-id="9118d-125">**AuthTokenSetting**</span></span> | <span data-ttu-id="9118d-126">”TwilioAuthToken”</span><span class="sxs-lookup"><span data-stu-id="9118d-126">"TwilioAuthToken"</span></span> | <span data-ttu-id="9118d-127">Autentiseringstoken för ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="9118d-127">The Auth Token for your Twilio account.</span></span> <span data-ttu-id="9118d-128">Snarare än att ställa in autentiseringstoken direkt så är den här parametern namnet på ett värde i appinställningarna som används till att hämta din autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="9118d-128">Rather than set the Auth Token directly, this parameter is the name of a value in the function app settings that will be used to retrieve the Auth Token.</span></span> |
    | <span data-ttu-id="9118d-129">**From**</span><span class="sxs-lookup"><span data-stu-id="9118d-129">**From**</span></span> | <span data-ttu-id="9118d-130">Ditt Twilio-telefonnummer</span><span class="sxs-lookup"><span data-stu-id="9118d-130">Your Twilio phone number</span></span> | <span data-ttu-id="9118d-131">Twilio-telefonnumret som dina SMS skickas från i internationellt format (+\<landskod\>\<telefonnummer\>, till exempel ”+1555123456”).</span><span class="sxs-lookup"><span data-stu-id="9118d-131">The Twilio phone number that the SMS messages will come from in international format (+\<country code\>\<phone number\>, for example "+1555123456").</span></span> |

    <span data-ttu-id="9118d-132">När du skapar ett Twilio-konto tilldelas du ett telefonnummer som du kan skicka meddelanden från.</span><span class="sxs-lookup"><span data-stu-id="9118d-132">When you create a Twilio account, you are assigned a phone number that you can send messages from.</span></span> <span data-ttu-id="9118d-133">Du hittar det här telefonnumret på Twilio-instrumentpanelen **Phone Numbers** (telefonnummer).</span><span class="sxs-lookup"><span data-stu-id="9118d-133">You can find this phone number on the Twilio **Phone Numbers** dashboard.</span></span> <span data-ttu-id="9118d-134">Välj ellipserna längst ned i den vänstra menyn på Twilio-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="9118d-134">From the Twilio site, select the ellipses at the bottom of the left-hand menu.</span></span> <span data-ttu-id="9118d-135">Välj sedan *SUPER NETWORK -> Phone Numbers*.</span><span class="sxs-lookup"><span data-stu-id="9118d-135">Then, select *SUPER NETWORK->Phone Numbers*.</span></span> <span data-ttu-id="9118d-136">Du kan fästa den här instrumentpanelen vid den vänstra menyn med en knappnålsikon.</span><span class="sxs-lookup"><span data-stu-id="9118d-136">You can pin this dashboard to the left-hand menu using the pin icon.</span></span> <span data-ttu-id="9118d-137">Du ser ditt Twilio-nummer under *Manage Numbers -> Active Numbers* (hantera nummer -> aktiva nummer).</span><span class="sxs-lookup"><span data-stu-id="9118d-137">Your Twilio number will be under *Manage Numbers->Active Numbers*.</span></span> <span data-ttu-id="9118d-138">Du måste ta bort alla blanksteg från numret.</span><span class="sxs-lookup"><span data-stu-id="9118d-138">You'll need to remove any spaces from the number.</span></span>

    ![Hitta ditt Twilio-nummer](../media/7-twilio-find-number.png)

    ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
               AuthTokenSetting = "TwilioAuthToken",
               From = "+1xxxxxxxxx")]ICollector<SMSMessage> messages,
    ```

1. <span data-ttu-id="9118d-140">Du kan konfigurera inställningarna för Functions-appen lokalt i filen `local.settings.json`.</span><span class="sxs-lookup"><span data-stu-id="9118d-140">Function app settings can be configured locally inside the `local.settings.json` file.</span></span> <span data-ttu-id="9118d-141">Lägg till SID och autentiseringstoken för ditt Twilio-konto i den här JSON-filen och använd inställningsnamnen som skickades till attributet `TwilioSMS`.</span><span class="sxs-lookup"><span data-stu-id="9118d-141">Add your Twilio account SID and Auth Token to this JSON file using the setting names that were passed to the `TwilioSMS` attribute.</span></span>

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "UseDevelopmentStorage=true",
            "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
            "TwilioAccountSid": "<Your SID>",
            "TwilioAuthToken": "<Your Auth Token>"
        }
    }
    ```

    <span data-ttu-id="9118d-142">Ersätt \<Your SID\> och \<Your Auth Token\> med värdena från instrumentpanelen i Twilio.</span><span class="sxs-lookup"><span data-stu-id="9118d-142">Replace \<Your SID\> and \<Your Auth Token\> with the values from your Twilio dashboard.</span></span>

    > <span data-ttu-id="9118d-143">De här lokala inställningarna används bara vid lokal körning.</span><span class="sxs-lookup"><span data-stu-id="9118d-143">These local settings will be only for running locally.</span></span> <span data-ttu-id="9118d-144">I en produktionsapp skulle dessa värden vara autentiseringsuppgifterna för det lokala utvecklings- eller testkontot.</span><span class="sxs-lookup"><span data-stu-id="9118d-144">In a production app, these values would be your local development or test account credentials.</span></span> <span data-ttu-id="9118d-145">När Azure-funktionen har distribuerats till Azure kan du konfigurera värden för produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="9118d-145">Once the Azure Function has been deployed to Azure, you'll be able to configure the production values.</span></span>
    > <span data-ttu-id="9118d-146">Om du checkar in koden i källkontrollen checkas de här lokala inställningsvärdena också in, så var försiktig så att du inte checkar in faktiska värden om koden är öppen källkod eller på annat sätt offentlig.</span><span class="sxs-lookup"><span data-stu-id="9118d-146">If you check your code into source control, these local application setting values will be checked in, too, so be careful not to check in any actual values in these files if your code is open source or public in any form.</span></span>

## <a name="create-the-sms-messages"></a><span data-ttu-id="9118d-147">Skapa SMS</span><span class="sxs-lookup"><span data-stu-id="9118d-147">Create the SMS messages</span></span>

<span data-ttu-id="9118d-148">Parametern `ICollector<SMSMessage>` är en samling `SMSMessage`-instanser och används till att samla in de SMS du vill skicka.</span><span class="sxs-lookup"><span data-stu-id="9118d-148">The `ICollector<SMSMessage>` parameter is a collection of `SMSMessage` instances and is used to collect the SMS messages you want to send.</span></span> <span data-ttu-id="9118d-149">När funktionen har körts skickas alla instanser av `SMSMessage` som lagts till vidare först till Twilio och sedan till mottagarna.</span><span class="sxs-lookup"><span data-stu-id="9118d-149">After the function has finished running, any instances of `SMSMessage` added to this collection are passed to Twilio and sent to the relevant recipients.</span></span>

1. <span data-ttu-id="9118d-150">Lägg till kod i funktionen `SendLocation` som itererar över telefonnumren i `PostData` och skapa ett SMS för varje nummer.</span><span class="sxs-lookup"><span data-stu-id="9118d-150">In the `SendLocation` function, add code to loop through the phone numbers in the `PostData` and create an SMS message for each one.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        SMSMessage message = new SMSMessage
        {
            Body = $"I'm here! {url}",
            To = toNo
        };
    }
    ```

    <span data-ttu-id="9118d-151">I meddelandet behövs mottagarens telefonnummer och brödtext som innehåller webbadressen till Google Maps för användarens plats.</span><span class="sxs-lookup"><span data-stu-id="9118d-151">The message needs the phone number to send to and a body that contains the Google Maps URL created from the user's location.</span></span>

1. <span data-ttu-id="9118d-152">Logga varje meddelande och Lägg till dem i samlingen `messages`.</span><span class="sxs-lookup"><span data-stu-id="9118d-152">Log each message, and then add it to the `messages` collection.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

<span data-ttu-id="9118d-153">Hela metoden `SendLocation` visas nedan.</span><span class="sxs-lookup"><span data-stu-id="9118d-153">The complete `SendLocation` method is shown below.</span></span>

```cs
[FunctionName("SendLocation")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                                Route = null)]HttpRequestMessage req,
                                                    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
                                                                AuthTokenSetting = "TwilioAuthToken",
                                                                From = "<your Twilio phone number>")]ICollector<SMSMessage> messages,
                                                    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    PostData data = await req.Content.ReadAsAsync<PostData>();
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.Info($"URL created - {url}");
    foreach (string toNo in data.ToNumbers)
    {
        SMSMessage message = new SMSMessage
        {
            Body = $"I'm here! {url}",
            To = toNo
        };
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

## <a name="test-it-out"></a><span data-ttu-id="9118d-154">Testa den</span><span class="sxs-lookup"><span data-stu-id="9118d-154">Test it out</span></span>

1. <span data-ttu-id="9118d-155">Ställ in appen `ImHere.Functions` som startprojekt och starta den utan felsökning.</span><span class="sxs-lookup"><span data-stu-id="9118d-155">Set the `ImHere.Functions` app as the startup project and start it without debugging.</span></span>

1. <span data-ttu-id="9118d-156">Ställ in appen `ImHere.UWP` som startapp och kör den.</span><span class="sxs-lookup"><span data-stu-id="9118d-156">Set the `ImHere.UWP` app as the startup project and run it.</span></span>

1. <span data-ttu-id="9118d-157">Ange ditt eget telefonnummer med internationellt format (+\<landskod\>\<telefonnummer\>) i Xamarin.Forms-appen.</span><span class="sxs-lookup"><span data-stu-id="9118d-157">Enter your own phone number in international format (+\<country code\>\<phone number\>) into the Xamarin.Forms app.</span></span> <span data-ttu-id="9118d-158">Provkonton hos Twilio kan skicka bara skicka meddelanden till verifierade telefonnummer, så om du inte uppgraderar till ett betalkonto eller verifierar flera nummer kan du bara skicka meddelanden till dig själv.</span><span class="sxs-lookup"><span data-stu-id="9118d-158">Twilio trial accounts can send  messages only to verified phone numbers, so for now, you'll only be able to message yourself unless you upgrade to a paid account or verify other numbers.</span></span>

1. <span data-ttu-id="9118d-159">Klicka på knappen **Send Location** (Skicka plats).</span><span class="sxs-lookup"><span data-stu-id="9118d-159">Click the **Send Location** button.</span></span> <span data-ttu-id="9118d-160">Om meddelandet skickades visas meddelandet ”Location sent successfully” (Platsen har skickats) i Xamarin.Forms-appen.</span><span class="sxs-lookup"><span data-stu-id="9118d-160">If the SMS message was sent successfully, you'll see a message in the Xamarin.Forms app saying, "Location sent successfully".</span></span>

    ![Meddelandet visas som skickat i Xamarin.Forms-appen](../media/7-ui-location-sent.png)

1. <span data-ttu-id="9118d-162">I konsolloggarna för Azure-funktionen ser du att meddelandet har skapats och skickats.</span><span class="sxs-lookup"><span data-stu-id="9118d-162">In the console logs for the Azure function, you'll see the message being created and sent.</span></span> <span data-ttu-id="9118d-163">Om ett fel inträffar (som att telefonnumret har fel format) loggas det hit.</span><span class="sxs-lookup"><span data-stu-id="9118d-163">If any errors occur (such as, the number is in the wrong format), they will be logged out here.</span></span>

    ![Azure-funktionskonsolen visar att meddelandet har skickats](../media/7-function-message-sent.png)

1. <span data-ttu-id="9118d-165">Kontrollera om du har fått meddelandet i telefonen.</span><span class="sxs-lookup"><span data-stu-id="9118d-165">Check your phone for a message.</span></span> <span data-ttu-id="9118d-166">Följ länken i meddelandet för att se din plats.</span><span class="sxs-lookup"><span data-stu-id="9118d-166">Follow the link in the message to see your location.</span></span>

    ![SMS-meddelandet mottaget på en mobiltelefon](../media/7-message-received.png)

## <a name="summary"></a><span data-ttu-id="9118d-168">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9118d-168">Summary</span></span>

<span data-ttu-id="9118d-169">I den här utbildningsenheten har du lärt dig hur du skapar en Twilio-bindning för Azure-funktionen och skickar ett SMS med användarens plats till en funktion som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="9118d-169">In this unit, you learned how to create a Twilio binding for the Azure function and send an SMS message with the user's location to a function that was running locally.</span></span> <span data-ttu-id="9118d-170">I nästa steg får du publicera funktionen i Azure.</span><span class="sxs-lookup"><span data-stu-id="9118d-170">In the next unit, you publish the function to Azure.</span></span>