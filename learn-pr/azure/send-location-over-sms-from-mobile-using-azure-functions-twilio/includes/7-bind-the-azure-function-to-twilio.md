Nu är den mobila appen färdig och skickar användarens plats samt listan med telefonnummer till en Azure-funktion som kan avserialisera data. I den här utbildningsenheten får du binda Azure-funktionen till Twilio för att skicka SMS.

Du kan ansluta Azure Functions till andra tjänster, antingen tjänster i Azure eller från tredje part. Sådana anslutningar kallas bindningar och finns i två varianter, in- och utdatabindningar. Indatabindningar levererar data till funktionen och bindningar hämtar data från funktionen och skickar dem till en annan tjänst. Du kan läsa mer om bindningar i [dokumentationen om Azure Functions-bindningar](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).

Bindningar för Azure Functions som skapas i Visual Studio definieras med funktionsparametrar och attribut.

## <a name="bind-the-azure-function-to-twilio"></a>Binda Azure-funktionen till Twilio

När du ska skicka SMS via Twilio behöver du en utdatabindning som är konfigurerad med ID:t för din kontoprenumeration (SID) och en autentiseringstoken.

1. Stoppa den lokala körningen av Azure Functions om det fortfarande körs sedan den föregående enheten.

2. Lägg till NuGet-paketet Microsoft.Azure.WebJobs.Extensions.Twilio i projektet `ImHere.Functions`. Det här NuGet-paketet innehåller de klasser som behövs för bindningen.

3. Lägg till en ny parameter i den statiska metoden `Run` i den statiska klassen `SendLocation` med namnet `messages`. Den här parametern ska ha typen `ICollector<SMSMessage>`. Du måste lägga till ett användningsdirektiv för namnområdet `Twilio`.

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                      ICollector<SMSMessage> messages,
                                                      TraceWriter log)
    ```

4. Utöka den nya parametern `messages` med attributet `TwilioSms`. Det här attributet har tre parametrar som du måste ange.

    | Inställning      |  Värde   | Beskrivning                                        |
    | --- | --- | ---|
    | **AccountSidSetting** | ”TwilioAccountSid” | Aktuellt SID för ditt Twilio-konto. Snarare än att ställa in SID-värdet direkt så är den här parametern namnet på ett värde i appinställningarna som används till att hämta ditt SID. |
    | **AuthTokenSetting** | ”TwilioAuthToken” | Autentiseringstoken för ditt Twilio-konto. Snarare än att ställa in autentiseringstoken direkt så är den här parametern namnet på ett värde i appinställningarna som används till att hämta din autentiseringstoken. |
    | **From** | Ditt Twilio-telefonnummer | Twilio-telefonnumret som dina SMS skickas från i internationellt format (+\<landskod\>\<telefonnummer\>, till exempel ”+1555123456”). |

    När du skapar ett Twilio-konto tilldelas du ett telefonnummer som du kan skicka meddelanden från. Du hittar det här telefonnumret på Twilio-instrumentpanelen **Phone Numbers** (telefonnummer). Välj ellipserna längst ned i den vänstra menyn på Twilio-webbplatsen. Välj sedan *SUPER NETWORK -> Phone Numbers*. Du kan fästa den här instrumentpanelen vid den vänstra menyn med en knappnålsikon. Du ser ditt Twilio-nummer under *Manage Numbers -> Active Numbers* (hantera nummer -> aktiva nummer). Du måste ta bort alla blanksteg från numret.

    ![Hitta ditt Twilio-nummer](../media/7-twilio-find-number.png)

    ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
               AuthTokenSetting = "TwilioAuthToken",
               From = "+1xxxxxxxxx")]ICollector<SMSMessage> messages,
    ```

5. Du kan konfigurera inställningarna för Functions-appen lokalt i filen `local.settings.json`. Lägg till SID och autentiseringstoken för ditt Twilio-konto i den här JSON-filen och använd inställningsnamnen som skickades till attributet `TwilioSMS`.

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

    Ersätt \<Your SID\> och \<Your Auth Token\> med värdena från instrumentpanelen i Twilio.

    > De här lokala inställningarna används bara vid lokal körning. I en produktionsapp skulle värdena vara autentiseringsuppgifterna för det lokala utvecklings- eller testkontot. När Azure-funktionen har distribuerats till Azure kan du konfigurera värden för produktionsmiljön.
    > Om du checkar in koden i källkontrollen checkas de här lokala inställningsvärdena också in, så var försiktig så att du inte checkar in faktiska värden om koden är öppen källkod eller på annat sätt offentlig.

## <a name="create-the-sms-messages"></a>Skapa SMS

Parametern `ICollector<SMSMessage>` är en samling `SMSMessage`-instanser och används till att samla in de SMS du vill skicka. När funktionen har körts skickas alla instanser av `SMSMessage` som lagts till vidare först till Twilio och sedan till mottagarna.

1. Lägg till kod i funktionen `SendLocation` som itererar över telefonnumren i `PostData` och skapa ett SMS för varje nummer.

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

    I meddelandet behövs mottagarens telefonnummer och brödtext som innehåller webbadressen till Google Maps för användarens plats.

2. Logga varje meddelande och Lägg till dem i samlingen `messages`.

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

Hela metoden `SendLocation` visas nedan.

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

## <a name="test-it-out"></a>Testa den

1. Ställ in appen `ImHere.Functions` som startprojekt och starta den utan felsökning.

2. Ställ in appen `ImHere.UWP` som startapp och kör den.

3. Ange ditt eget telefonnummer med internationellt format (+\<landskod\>\<telefonnummer\>) i Xamarin.Forms-appen. Provkonton hos Twilio kan skicka bara skicka meddelanden till verifierade telefonnummer, så om du inte uppgraderar till ett betalkonto eller verifierar flera nummer kan du bara skicka meddelanden till dig själv.

4. Klicka på knappen **Send Location**. Om meddelandet gick att skicka visas meddelandet ”Location sent successfully” i Xamarin.Forms-appen.

    ![Meddelandet visas som skickat i Xamarin.Forms-appen](../media/7-ui-location-sent.png)

5. I konsolloggarna för Azure-funktionen ser du att meddelandet har skapats och skickats. Om ett fel inträffar (som att telefonnumret har fel format) loggas det hit.

    ![Azure-funktionskonsolen visar att meddelandet har skickats](../media/7-function-message-sent.png)

6. Kontrollera om du har fått meddelandet i telefonen. Följ länken i meddelandet för att se din plats.

    ![SMS-meddelandet mottaget på en mobiltelefon](../media/7-message-received.png)

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du lärt dig hur du skapar en Twilio-bindning för Azure-funktionen och skickar ett SMS med användarens plats till en funktion som körs lokalt. I nästa steg får du publicera funktionen i Azure.