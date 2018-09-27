Nu hämtar appen användarens plats och den är redo att skickas till en Azure-funktion. I den här delen får du skapa Azure-funktionen.

## <a name="create-an-azure-functions-project"></a>Skapa ett Azure Functions-projekt

1. Lägg till ett nytt projekt i `ImHere`-lösningen genom att högerklicka på lösningen och välja *Lägg till -> Nytt projekt …* .

1. Välj *Visual C# -> Molnet* i trädvyn till vänster och välj sedan *Azure Functions* på panelen i mitten.

1. Ange projektnamnet ImHere.Functions och klicka på **OK**.

    ![Dialogrutan Lägg till nytt projekt](../media/5-add-new-functions-project.png)

1. Konfigurationsdialogrutan **Nytt projekt** visas, och visar kanske en rotationsruta nere till vänster medan uppdaterade mallar läses in. Om du ser det här väntar du tills allt har lästs in. Om det sedan finns uppdaterade mallar tillgängliga klickar du på alternativet **Uppdatera** som visas så att du säkert får de senaste Functions-mallarna.

    ![Dialogrutan Nytt projekt som läser in de senaste mallarna](../media/5-loading-templates.png)

1. I konfigurationsdialogrutan **Nytt projekt** kontrollerar du att Functions-versionen är *Azure Functions v2 (.NET Standard)* (**INTE** _v1 (.NET Framework)_). Välj *HTTP-utlösare*, låt lagringskontot vara inställt som *Lagringsemulator* och ange *Anonym* för åtkomstbehörigheter. Klicka sedan på **OK**.

    ![Konfigurationsdialogrutan för Azure Function-projekt](../media/5-configure-trigger.png)

    Det nya projektet skapas och har en standardfunktion som kallas `Function1`.

> [!NOTE]
> Den här funktionen har skapats med anonym åtkomst. När den har publicerats till Azure kan alla som känner till webbadressen anropa den här funktionen. Om det här var ett exempel ur verkligheten skulle du ha skyddat den med någon form av autentisering, till exempel [Azure App Service-autentisering](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview?azure-portal=true) eller [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c?azure-portal=true).

## <a name="create-the-function"></a>Skapa funktionen

Azure Functions-projektet har skapats med en enda HTTP-utlösarfunktion som kallas `Function1`. Med HTTP-utlösare kan du anropa funktionerna med hjälp av HTTP-begäranden. Själva funktionen implementeras som en statisk `Run`-metod i `Function1`-klassen.

1. Byt namn på filen i Solution Explorer från Function1.cs till SendLocation.cs. När du uppmanas att byta namn på alla referenser till kodelementet `Function1` klickar du på **Ja**.

1. Byt namn på funktionen i attributet till SendLocation.

    ```cs
    [FunctionName("SendLocation")]
    ```

1. Ta bort innehållet i funktionen, förutom den första raden som skriver ett informationsmeddelande till loggaren.

    ```cs
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                             "get", "post",
                                                             Route = null)]HttpRequestMessage req,
                                                ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
    }
    ```

## <a name="create-a-class-to-share-data-between-the-mobile-app-and-function"></a>Skapa en klass för att dela data mellan mobilappen och funktionen

När data skickas till Azure-funktionen sker det i JSON-format. Mobilappen serialiserar data till JSON och funktionen deserialiserar från JSON. Om du vill hålla dessa data konsekventa mellan mobilappen och funktionen skapar du ett nytt projekt som innehåller en klass för att lagra plats- och telefonnummerdata. Det här projektet refereras sedan av appen och funktionen.

1. Skapa ett nytt projekt under `ImHere`-lösningen genom att högerklicka på lösningen och välja *Lägg till -> Nytt projekt …*.

1. Välj *Visual C# -> .NET Standard* i trädvyn till vänster och sedan *Klassbibliotek (.NET Standard)* på panelen i mitten.

1. Ange projektnamnet ImHere.Data och klicka på **OK**.

    ![Dialogrutan Lägg till nytt projekt](../media/5-add-new-net-standard-project.png)

1. Ta bort den automatiskt genererade filen Class1.cs.

1. Skapa en ny klass i `ImHere.Data`-projektet som heter `PostData` genom att högerklicka på projektet och sedan välja *Lägg till -> Klass …*. Ge den nya klassen namnet PostData och klicka på **OK**. Markera den här nya klassen som `public`.

1. Lägg till `double`-egenskaper för latitud och longitud, samt en `string[]`-egenskap för telefonnumren att skicka till.

    ```cs
    public class PostData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string[] ToNumbers { get; set; }
    }
    ```

1. Lägg till en referens till det här projektet både i `ImHere.Functions` och `ImHere` genom att högerklicka på projektet och välja *Lägg till -> Referens …* . Välj *Projekt* i trädvyn till vänster och markera sedan kryssrutan bredvid *ImHere.Data*.

    ![Konfigurera projektreferenser](../media/5-configure-project-references.png)

## <a name="read-the-data-sent-to-the-function"></a>Läsa data som skickas till funktionen

Parametern `req` i Azure-funktionen innehåller den HTTP-begäran som skapades, och denna begärans data är ett JSON-serialiserat `PostData`-objekt.

1. Öppna klassen `SendLocation` i `ImHere.Functions`-projektet.

1. Läs in innehållet i HTTP-begäran i en sträng, avserialisera den sedan i ett `PostData`-objekt, där du lägger till ett användningsdirektiv för namnrymden `ImHere.Data`.

    ```cs
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    ```

1. Skapa en Google Maps-URL med hjälp av latitud och longitud från `PostData`-objektet.

   ```cs
   string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
   ```

1. Logga URL:en.

    ```cs
    log.LogInformation($"URL created - {url}");
    ```

1. Returnera en 200-statuskod för att visa att funktionen slutfördes utan fel.

    ```cs
    return new OkResult();
    ```

Den fullständiga funktionen visas nedan.

```cs
[FunctionName("SendLocation")]
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                         Route = null)]HttpRequest req,
                                                    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.LogInformation($"URL created - {url}");
    return new OkResult();
}
```

## <a name="run-the-azure-function-locally"></a>Felsöka Azure-funktionen lokalt

Azure Functions kan köras lokalt med ett konto för lokal lagring och lokal Azure Functions-körning. Med den här lokala körningen kan du testa funktionen innan du distribuerar den till Azure.

1. Högerklicka på projektet `ImHere.Functions` i Solution Explorer och välj *Ställ in som startprojekt*.

1. På menyn *Felsöka* väljer du *Starta utan felsökning*. Den lokala Azure Functions-körningen startas i ett konsolfönster och startar din funktion, samt lyssnar på en tillgänglig port på `localhost`. Om du ser en dialogruta som frågar efter brandväggsåtkomst tillåter du åtkomst till privata nätverk (standardalternativet).

    ![Azure-funktionen körs lokalt](../media/5-function-running-locally.png)

1. Anteckna vilken port funktionen lyssnar på. Du behöver den i nästa del för att testa mobilappen. I bilden ovan lyssnar funktionen på porten **7071**.

    ```sh
    Listening on http://localhost:7071/
    ```

1. Låt funktionen köras så att du kan testa mobilappen i nästa del.

## <a name="summary"></a>Sammanfattning

Här har du fått lära dig att skapa ett Azure Functions-projekt i Visual Studio, du har lagt till ett delat projekt med ett dataobjekt som ska delas mellan mobilappen och funktionen, samt fått lära dig hur du skapar en grundläggande implementering av funktionen för att deserialisera data som skickats. Du har också fått lära dig att köra en Azure-funktion lokalt. Härnäst får du lära dig att anropa Azure-funktionen från mobilappen.