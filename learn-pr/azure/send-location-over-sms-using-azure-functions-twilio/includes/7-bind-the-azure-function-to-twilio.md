<span data-ttu-id="56bae-101">Nu är den mobila appen färdig och kan skicka användarens plats samt listan med telefonnummer till en Azure-funktion som kan avserialisera data.</span><span class="sxs-lookup"><span data-stu-id="56bae-101">At this point, the mobile app is complete and it can send the user's location and list of phone numbers to an Azure function that can deserialize the data.</span></span> <span data-ttu-id="56bae-102">I den här kursdelen ska du binda Azure-funktionen till Twilio för att skicka SMS.</span><span class="sxs-lookup"><span data-stu-id="56bae-102">In this unit, you bind the Azure function to Twilio to send SMS messages.</span></span>

<span data-ttu-id="56bae-103">Du kan ansluta Azure Functions till andra tjänster, antingen tjänster i Azure eller från tredje part.</span><span class="sxs-lookup"><span data-stu-id="56bae-103">Azure Functions can be connected to other services, either services in Azure or third-party services.</span></span> <span data-ttu-id="56bae-104">Sådana anslutningar kallas bindningar och finns i två varianter, in- och utdatabindningar.</span><span class="sxs-lookup"><span data-stu-id="56bae-104">These connections, called bindings, exist in two forms - input and output bindings.</span></span> <span data-ttu-id="56bae-105">Indatabindningar levererar data till funktionen och bindningar hämtar data från funktionen och skickar dem till en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="56bae-105">Input bindings provide data to your function and output bindings take data from your function and send it to another service.</span></span> <span data-ttu-id="56bae-106">Du kan läsa mer om bindningar i [dokumentationen om Azure Functions-bindningar](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="56bae-106">You can read about bindings in the [Azure Functions Binding docs](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings?azure-portal=true).</span></span>

<span data-ttu-id="56bae-107">Bindningar för Azure Functions som skapas i Visual Studio definieras med funktionsparametrar och attribut.</span><span class="sxs-lookup"><span data-stu-id="56bae-107">Bindings for Azure Functions created in Visual Studio are defined using parameters to the function, decorated with attributes.</span></span>

## <a name="bind-the-azure-function-to-twilio"></a><span data-ttu-id="56bae-108">Binda Azure-funktionen till Twilio</span><span class="sxs-lookup"><span data-stu-id="56bae-108">Bind the Azure function to Twilio</span></span>

<span data-ttu-id="56bae-109">När du ska skicka SMS via Twilio behöver du en utdatabindning som är konfigurerad med ID:t för din kontoprenumeration (SID) och en autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="56bae-109">Sending SMS messages via Twilio requires an output binding that is configured with your account subscription ID (SID) and Auth Token.</span></span>

1. <span data-ttu-id="56bae-110">Stoppa den lokala körningen av Azure Functions om det fortfarande körs sedan den föregående enheten.</span><span class="sxs-lookup"><span data-stu-id="56bae-110">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

1. <span data-ttu-id="56bae-111">Lägg till NuGet-paketet v.3.0.0-rc1 Microsoft.Azure.WebJobs.Extensions.Twilio i projektet `ImHere.Functions`.</span><span class="sxs-lookup"><span data-stu-id="56bae-111">Add the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package v3.0.0-rc1 to the `ImHere.Functions` project.</span></span> <span data-ttu-id="56bae-112">**Använd version 3.0.0-rc1, inte 3.0.0 på grund av ett fel i den säkra versionen med Twilio-bindningar**.</span><span class="sxs-lookup"><span data-stu-id="56bae-112">**Use version 3.0.0-rc1, NOT 3.0.0 due to a bug in the stable version with Twilio bindings**.</span></span> <span data-ttu-id="56bae-113">Det här NuGet-paketet innehåller de klasser som behövs för bindningen.</span><span class="sxs-lookup"><span data-stu-id="56bae-113">This NuGet package contains the relevant classes for the binding.</span></span>

1. <span data-ttu-id="56bae-114">Lägg till en ny parameter i den statiska metoden `Run` i den statiska klassen `SendLocation` med namnet `messages`.</span><span class="sxs-lookup"><span data-stu-id="56bae-114">Add a new parameter to the static `Run` method on the `SendLocation` static class called `messages`.</span></span> <span data-ttu-id="56bae-115">Den här parametern ska ha typen `ICollector<CreateMessageOptions>`.</span><span class="sxs-lookup"><span data-stu-id="56bae-115">This parameter will have a type of `ICollector<CreateMessageOptions>`.</span></span> <span data-ttu-id="56bae-116">Du måste lägga till ett `using`-direktiv för `Twilio.Rest.Api.V2010.Account`-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="56bae-116">You'll need to add a `using` directive for the `Twilio.Rest.Api.V2010.Account` namespace.</span></span>

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,"get", "post", Route = null)]HttpRequestMessage req,
                                                ICollector<CreateMessageOptions> messages,
                                                ILogger log)
    ```

1. <span data-ttu-id="56bae-117">Utöka den nya parametern `messages` med attributet `TwilioSms` enligt följande:</span><span class="sxs-lookup"><span data-stu-id="56bae-117">Decorate the new `messages` parameter with the `TwilioSms` attribute as follows:</span></span> 

      ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",AuthTokenSetting = "TwilioAuthToken", From = "+1xxxxxxxxx")]ICollector<CreateMessageOptions> messages,
    ```
    <span data-ttu-id="56bae-118">Det här attributet har tre parametrar som du måste ange:</span><span class="sxs-lookup"><span data-stu-id="56bae-118">This attribute has three parameters you need to set:</span></span>

    * <span data-ttu-id="56bae-119">**AccountSidSetting** – Ställ in på `"TwilioAccountSid"`</span><span class="sxs-lookup"><span data-stu-id="56bae-119">**AccountSidSetting** - set this to `"TwilioAccountSid"`</span></span>
  
        <span data-ttu-id="56bae-120">Det här är SID-värdet för Twilio-kontot som du antecknade tidigare i modulen.</span><span class="sxs-lookup"><span data-stu-id="56bae-120">This is the SID for your Twilio account that you recorded earlier in the module.</span></span> <span data-ttu-id="56bae-121">Snarare än att ställa in SID-värdet direkt så är den här parametern namnet på ett värde i appinställningarna som används till att hämta ditt SID.</span><span class="sxs-lookup"><span data-stu-id="56bae-121">Rather than set the SID directly, this parameter is the name of a value in the function app settings that will be used to retrieve the SID.</span></span>

    * <span data-ttu-id="56bae-122">**AuthTokenSetting** – Ställ in på `"TwilioAuthToken"`</span><span class="sxs-lookup"><span data-stu-id="56bae-122">**AuthTokenSetting** - set this to `"TwilioAuthToken"`</span></span>

       <span data-ttu-id="56bae-123">Det här är Auth Token-värdet för Twilio-kontot som du antecknade tidigare i modulen.</span><span class="sxs-lookup"><span data-stu-id="56bae-123">This is the Auth Token for your Twilio account that you recorded earlier in the module.</span></span> <span data-ttu-id="56bae-124">Snarare än att ställa in autentiseringstoken direkt så är den här parametern namnet på ett värde i appinställningarna som används till att hämta din autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="56bae-124">Rather than set the Auth Token directly, this parameter is the name of a value in the function app settings that will be used to retrieve the Auth Token.</span></span>

    * <span data-ttu-id="56bae-125">**Från** – Ställ in på ditt aktiva Twilio-telefonnummer som du antecknade tidigare i modulen.</span><span class="sxs-lookup"><span data-stu-id="56bae-125">**From** - set this to your Twilio active phone number that you recorded earlier in the module.</span></span>

        <span data-ttu-id="56bae-126">Twilio-telefonnumret som dina SMS skickas från i internationellt format (+\<landskod\>\<telefonnummer\>, till exempel ”+1555123456”).</span><span class="sxs-lookup"><span data-stu-id="56bae-126">The Twilio phone number that the SMS messages will come from in international format (+\<country code\>\<phone number\>, for example "+1555123456").</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="56bae-127">Se till att ta bort alla blanksteg från telefonnumret.</span><span class="sxs-lookup"><span data-stu-id="56bae-127">Make sure to remove all spaces from the phone number.</span></span>


1. <span data-ttu-id="56bae-128">Du kan konfigurera inställningarna för Functions-appen lokalt i filen `local.settings.json`.</span><span class="sxs-lookup"><span data-stu-id="56bae-128">Function app settings can be configured locally inside the `local.settings.json` file.</span></span> <span data-ttu-id="56bae-129">Lägg till SID och autentiseringstoken för ditt Twilio-konto i den här JSON-filen och använd inställningsnamnen som skickades till attributet `TwilioSMS`.</span><span class="sxs-lookup"><span data-stu-id="56bae-129">Add your Twilio account SID and Auth Token to this JSON file using the setting names that were passed to the `TwilioSMS` attribute.</span></span>

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "UseDevelopmentStorage=true",
            "FUNCTIONS_WORKER_RUNTIME": "dotnet",
            "TwilioAccountSid": "<Your SID>",
            "TwilioAuthToken": "<Your Auth Token>"
        }
    }
    ```

    <span data-ttu-id="56bae-130">Ersätt \<Your SID\> och \<Your Auth Token\> med värdena från instrumentpanelen i Twilio.</span><span class="sxs-lookup"><span data-stu-id="56bae-130">Replace \<Your SID\> and \<Your Auth Token\> with the values from your Twilio dashboard.</span></span>

    > [!NOTE]
    > <span data-ttu-id="56bae-131">De här lokala inställningarna används bara vid lokal körning.</span><span class="sxs-lookup"><span data-stu-id="56bae-131">These local settings will be only for running locally.</span></span> <span data-ttu-id="56bae-132">I en produktionsapp skulle dessa värden vara autentiseringsuppgifterna för det lokala utvecklings- eller testkontot.</span><span class="sxs-lookup"><span data-stu-id="56bae-132">In a production app, these values would be your local development or test account credentials.</span></span> <span data-ttu-id="56bae-133">När Azure-funktionen har distribuerats till Azure kan du konfigurera värden för produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="56bae-133">Once the Azure Function has been deployed to Azure, you'll be able to configure the production values.</span></span>

     > [!NOTE]
    > <span data-ttu-id="56bae-134">Om du checkar in koden i källkontrollen checkas de här lokala inställningsvärdena också in, så var försiktig så att du inte checkar in faktiska värden om koden är öppen källkod eller på annat sätt offentlig.</span><span class="sxs-lookup"><span data-stu-id="56bae-134">If you check your code into source control, these local application setting values will be checked in, too, so be careful not to check in any actual values in these files if your code is open source or public in any form.</span></span>
    

## <a name="create-the-sms-messages"></a><span data-ttu-id="56bae-135">Skapa SMS</span><span class="sxs-lookup"><span data-stu-id="56bae-135">Create the SMS messages</span></span>

<span data-ttu-id="56bae-136">Parametern `ICollector<CreateMessageOptions>` är en samling `CreateMessageOptions`-instanser och används till att samla in de SMS du vill skicka.</span><span class="sxs-lookup"><span data-stu-id="56bae-136">The `ICollector<CreateMessageOptions>` parameter is a collection of `CreateMessageOptions` instances and is used to collect the SMS messages you want to send.</span></span> <span data-ttu-id="56bae-137">När funktionen har körts skickas alla instanser av `CreateMessageOptions` som lagts till vidare först till Twilio och används till att skapa meddelanden som sedan skickas till mottagarna.</span><span class="sxs-lookup"><span data-stu-id="56bae-137">After the function has finished running, any instances of `CreateMessageOptions` added to this collection are passed to Twilio and used to create messages to be sent to the relevant recipients.</span></span>

1. <span data-ttu-id="56bae-138">Lägg till kod i funktionen `SendLocation` som itererar över telefonnumren i `PostData` och skapa ett SMS för varje nummer.</span><span class="sxs-lookup"><span data-stu-id="56bae-138">In the `SendLocation` function, add code to loop through the phone numbers in the `PostData` and create an SMS message for each one.</span></span> <span data-ttu-id="56bae-139">Du måste lägga till ett användningsdirektiv för `Twilio.Types`.</span><span class="sxs-lookup"><span data-stu-id="56bae-139">You will need to add a using directive for `Twilio.Types`.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        PhoneNumber number = new PhoneNumber(toNo);
        CreateMessageOptions message = new CreateMessageOptions(number)
        {
            Body = $"I'm here! {url}"
        };
    }
    ```

    <span data-ttu-id="56bae-140">I meddelandet behövs mottagarens telefonnummer och brödtext som innehåller webbadressen till Google Maps för användarens plats.</span><span class="sxs-lookup"><span data-stu-id="56bae-140">The message needs the phone number to send to and a body that contains the Google Maps URL created from the user's location.</span></span>

1. <span data-ttu-id="56bae-141">Logga varje meddelande och Lägg till dem i samlingen `messages`.</span><span class="sxs-lookup"><span data-stu-id="56bae-141">Log each message, and then add it to the `messages` collection.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.LogInformation($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

<span data-ttu-id="56bae-142">Hela metoden `SendLocation` visas nedan.</span><span class="sxs-lookup"><span data-stu-id="56bae-142">The complete `SendLocation` method is shown below.</span></span> <span data-ttu-id="56bae-143">Det aktiva telefonnumret ersätter platshållaren i parametern `From`.</span><span class="sxs-lookup"><span data-stu-id="56bae-143">Your active phone number would replace the placeholder in the `From` parameter.</span></span>

```cs
[FunctionName("SendLocation")]
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                         "get", "post",
                                                         Route = null)]HttpRequest req,
                                            [TwilioSms(AccountSidSetting = "TwilioAccountSid",
                                                       AuthTokenSetting = "TwilioAuthToken",
                                                       From = "+1xxxxxxxxx")] ICollector<CreateMessageOptions> messages,
                                            ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.LogInformation($"URL created - {url}");

    foreach (string toNo in data.ToNumbers)
    {
        PhoneNumber number = new PhoneNumber(toNo);
        CreateMessageOptions message = new CreateMessageOptions(number)
        {
            Body = $"I'm here! {url}"
        };
        log.LogInformation($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }

    return new OkResult();
}
```

## <a name="test-it-out"></a><span data-ttu-id="56bae-144">Testa den</span><span class="sxs-lookup"><span data-stu-id="56bae-144">Test it out</span></span>

1. <span data-ttu-id="56bae-145">Ställ in appen `ImHere.Functions` som startprojekt och starta den utan felsökning.</span><span class="sxs-lookup"><span data-stu-id="56bae-145">Set the `ImHere.Functions` app as the startup project and start it without debugging.</span></span>

1. <span data-ttu-id="56bae-146">Ställ in appen `ImHere.UWP` som startapp och kör den.</span><span class="sxs-lookup"><span data-stu-id="56bae-146">Set the `ImHere.UWP` app as the startup project and run it.</span></span>

1. <span data-ttu-id="56bae-147">Ange ditt eget telefonnummer med internationellt format (+\<landskod\>\<telefonnummer\>) i Xamarin.Forms-appen.</span><span class="sxs-lookup"><span data-stu-id="56bae-147">Enter your own phone number in international format (+\<country code\>\<phone number\>) into the Xamarin.Forms app.</span></span> <span data-ttu-id="56bae-148">Provkonton hos Twilio kan skicka bara skicka meddelanden till verifierade telefonnummer, så om du inte uppgraderar till ett betalkonto eller verifierar flera nummer kan du bara skicka meddelanden till dig själv.</span><span class="sxs-lookup"><span data-stu-id="56bae-148">Twilio trial accounts can send  messages only to verified phone numbers, so for now, you'll only be able to message yourself unless you upgrade to a paid account or verify other numbers.</span></span>

1. <span data-ttu-id="56bae-149">Klicka på knappen **Send Location** (Skicka plats).</span><span class="sxs-lookup"><span data-stu-id="56bae-149">Click the **Send Location** button.</span></span> <span data-ttu-id="56bae-150">Om meddelandet skickades visas meddelandet ”Location sent successfully” (Platsen har skickats) i Xamarin.Forms-appen.</span><span class="sxs-lookup"><span data-stu-id="56bae-150">If the SMS message was sent successfully, you'll see a message in the Xamarin.Forms app saying, "Location sent successfully".</span></span>

    ![Meddelandet visas som skickat i Xamarin.Forms-appen](../media/7-ui-location-sent.png)

1. <span data-ttu-id="56bae-152">I konsolloggarna för Azure-funktionen ser du att meddelandet har skapats och skickats.</span><span class="sxs-lookup"><span data-stu-id="56bae-152">In the console logs for the Azure function, you'll see the message being created and sent.</span></span> <span data-ttu-id="56bae-153">Om ett fel inträffar (som att telefonnumret har fel format) loggas det hit.</span><span class="sxs-lookup"><span data-stu-id="56bae-153">If any errors occur (such as, the number is in the wrong format), they will be logged out here.</span></span>

    ![Azure-funktionskonsolen visar att meddelandet har skickats](../media/7-function-message-sent.png)

1. <span data-ttu-id="56bae-155">Kontrollera om du har fått meddelandet i telefonen.</span><span class="sxs-lookup"><span data-stu-id="56bae-155">Check your phone for a message.</span></span> <span data-ttu-id="56bae-156">Följ länken i meddelandet för att se din plats.</span><span class="sxs-lookup"><span data-stu-id="56bae-156">Follow the link in the message to see your location.</span></span>

    ![SMS-meddelandet mottaget på en mobiltelefon](../media/7-message-received.png)

    > [!TIP]
    > <span data-ttu-id="56bae-158">Den plats som visas är den plats där programmet körs, så den är i närheten av datacentret som den virtuella datorn körs från.</span><span class="sxs-lookup"><span data-stu-id="56bae-158">The location you'll see is the location where the app is running, so will be near to the data center that the VM is running from.</span></span> <span data-ttu-id="56bae-159">Om den här appen körs på din lokala enhet visas din plats.</span><span class="sxs-lookup"><span data-stu-id="56bae-159">If this app was running on your local device then it would show your location.</span></span>

## <a name="summary"></a><span data-ttu-id="56bae-160">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="56bae-160">Summary</span></span>

<span data-ttu-id="56bae-161">I den här utbildningsenheten har du lärt dig hur du skapar en Twilio-bindning för Azure-funktionen och skickar ett SMS med användarens plats till en funktion som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="56bae-161">In this unit, you learned how to create a Twilio binding for the Azure function and send an SMS message with the user's location to a function that was running locally.</span></span> <span data-ttu-id="56bae-162">I nästa steg får du publicera funktionen i Azure.</span><span class="sxs-lookup"><span data-stu-id="56bae-162">In the next unit, you publish the function to Azure.</span></span>
