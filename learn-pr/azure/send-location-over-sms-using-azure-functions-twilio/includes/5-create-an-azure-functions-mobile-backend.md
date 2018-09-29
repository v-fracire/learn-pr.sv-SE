<span data-ttu-id="b6dd8-101">Nu hämtar appen användarens plats och den är redo att skickas till en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-101">At this point, the app is working to get the user's location and is ready to be sent to an Azure function.</span></span> <span data-ttu-id="b6dd8-102">I den här delen får du skapa Azure-funktionen.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-102">In this unit, you build the Azure function.</span></span>

## <a name="create-an-azure-functions-project"></a><span data-ttu-id="b6dd8-103">Skapa ett Azure Functions-projekt</span><span class="sxs-lookup"><span data-stu-id="b6dd8-103">Create an Azure Functions project</span></span>

1. <span data-ttu-id="b6dd8-104">Lägg till ett nytt projekt i `ImHere`-lösningen genom att högerklicka på lösningen och välja *Lägg till -> Nytt projekt …* .</span><span class="sxs-lookup"><span data-stu-id="b6dd8-104">Add a new project to the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

1. <span data-ttu-id="b6dd8-105">Välj *Visual C# -> Molnet* i trädvyn till vänster och välj sedan *Azure Functions* på panelen i mitten.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-105">From the tree on the left-hand side, select *Visual C#->Cloud*, and then select *Azure Functions* from the panel in the center.</span></span>

1. <span data-ttu-id="b6dd8-106">Ange projektnamnet ImHere.Functions och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-106">Name the project "ImHere.Functions", and then click **OK**.</span></span>

    ![Dialogrutan Lägg till nytt projekt](../media/5-add-new-functions-project.png)

1. <span data-ttu-id="b6dd8-108">Konfigurationsdialogrutan **Nytt projekt** visas, och visar kanske en rotationsruta nere till vänster medan uppdaterade mallar läses in.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-108">The **New Project** configuration dialog will appear, and it may show a spinner in the bottom-left whilst loading updated templates.</span></span> <span data-ttu-id="b6dd8-109">Om du ser det här väntar du tills allt har lästs in. Om det sedan finns uppdaterade mallar tillgängliga klickar du på alternativet **Uppdatera** som visas så att du säkert får de senaste Functions-mallarna.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-109">If you see this, wait until this has finished loading, then if updated templates are available, click the **Refresh** option that will appear to ensure you get the latest Function templates.</span></span>

    ![Dialogrutan Nytt projekt som läser in de senaste mallarna](../media/5-loading-templates.png)

1. <span data-ttu-id="b6dd8-111">I konfigurationsdialogrutan **Nytt projekt** kontrollerar du att Functions-versionen är *Azure Functions v2 (.NET Standard)* (**INTE** _v1 (.NET Framework)_).</span><span class="sxs-lookup"><span data-stu-id="b6dd8-111">In the **New Project** configuration dialog, ensure the Functions version is set to *Azure Functions v2 (.NET Standard)* (**NOT** _v1 (.NET Framework)_).</span></span> <span data-ttu-id="b6dd8-112">Välj *HTTP-utlösare*, låt lagringskontot vara inställt som *Lagringsemulator* och ange *Anonym* för åtkomstbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-112">Select *Http Trigger*, leave the storage account set to *Storage Emulator*, and set the access rights to *Anonymous*.</span></span> <span data-ttu-id="b6dd8-113">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-113">Then click **OK**.</span></span>

    ![Konfigurationsdialogrutan för Azure Function-projekt](../media/5-configure-trigger.png)

    <span data-ttu-id="b6dd8-115">Det nya projektet skapas och har en standardfunktion som kallas `Function1`.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-115">The new project will be created and have a default function called `Function1`.</span></span>

> [!NOTE]
> <span data-ttu-id="b6dd8-116">Den här funktionen har skapats med anonym åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-116">This function was created with anonymous access.</span></span> <span data-ttu-id="b6dd8-117">När den har publicerats till Azure kan alla som känner till webbadressen anropa den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-117">Once published to Azure, anybody who knows the URL will be able to call this function.</span></span> <span data-ttu-id="b6dd8-118">Om det här var ett exempel ur verkligheten skulle du ha skyddat den med någon form av autentisering, till exempel [Azure App Service-autentisering](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview?azure-portal=true) eller [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="b6dd8-118">In a real-world scenario, you would protect this with some form of authentication, such as [Azure App Service authentication](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview?azure-portal=true) or [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c?azure-portal=true).</span></span>

## <a name="create-the-function"></a><span data-ttu-id="b6dd8-119">Skapa funktionen</span><span class="sxs-lookup"><span data-stu-id="b6dd8-119">Create the function</span></span>

<span data-ttu-id="b6dd8-120">Azure Functions-projektet har skapats med en enda HTTP-utlösarfunktion som kallas `Function1`.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-120">The Azure Functions project is created with a single HTTP trigger function called `Function1`.</span></span> <span data-ttu-id="b6dd8-121">Med HTTP-utlösare kan du anropa funktionerna med hjälp av HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-121">HTTP triggers allow you to invoke your functions using HTTP requests.</span></span> <span data-ttu-id="b6dd8-122">Själva funktionen implementeras som en statisk `Run`-metod i `Function1`-klassen.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-122">The function itself is implemented as a static `Run` method in the `Function1` class.</span></span>

1. <span data-ttu-id="b6dd8-123">Byt namn på filen i Solution Explorer från Function1.cs till SendLocation.cs.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-123">Rename the file in Solution Explorer from "Function1.cs" to "SendLocation.cs".</span></span> <span data-ttu-id="b6dd8-124">När du uppmanas att byta namn på alla referenser till kodelementet `Function1` klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-124">When prompted to rename all references to the code element `Function1`, click **Yes**.</span></span>

1. <span data-ttu-id="b6dd8-125">Byt namn på funktionen i attributet till SendLocation.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-125">Rename the function name in the attribute to "SendLocation".</span></span>

    ```cs
    [FunctionName("SendLocation")]
    ```

1. <span data-ttu-id="b6dd8-126">Ta bort innehållet i funktionen, förutom den första raden som skriver ett informationsmeddelande till loggaren.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-126">Delete the contents of the function, except the first line that writes an information message to the logger.</span></span>

    ```cs
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                             "get", "post",
                                                             Route = null)]HttpRequestMessage req,
                                                ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
    }
    ```

## <a name="create-a-class-to-share-data-between-the-mobile-app-and-function"></a><span data-ttu-id="b6dd8-127">Skapa en klass för att dela data mellan mobilappen och funktionen</span><span class="sxs-lookup"><span data-stu-id="b6dd8-127">Create a class to share data between the mobile app and function</span></span>

<span data-ttu-id="b6dd8-128">När data skickas till Azure-funktionen sker det i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-128">When data is sent to the Azure function, it will be sent as JSON.</span></span> <span data-ttu-id="b6dd8-129">Mobilappen serialiserar data till JSON och funktionen deserialiserar från JSON.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-129">The mobile app will serialize data into JSON and the function will deserialize from JSON.</span></span> <span data-ttu-id="b6dd8-130">Om du vill hålla dessa data konsekventa mellan mobilappen och funktionen skapar du ett nytt projekt som innehåller en klass för att lagra plats- och telefonnummerdata.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-130">To keep this data consistent between the mobile app and the function, create a new project that contains a class to hold the location and phone number data.</span></span> <span data-ttu-id="b6dd8-131">Det här projektet refereras sedan av appen och funktionen.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-131">This project will then be referenced by the app and function.</span></span>

1. <span data-ttu-id="b6dd8-132">Skapa ett nytt projekt under `ImHere`-lösningen genom att högerklicka på lösningen och välja *Lägg till -> Nytt projekt …*.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-132">Create a new project under the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

1. <span data-ttu-id="b6dd8-133">Välj *Visual C# -> .NET Standard* i trädvyn till vänster och sedan *Klassbibliotek (.NET Standard)* på panelen i mitten.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-133">From the tree on the left-hand side, select *Visual C#->.NET Standard*, and then select *Class Library (.NET Standard)* from the panel in the center.</span></span>

1. <span data-ttu-id="b6dd8-134">Ange projektnamnet ImHere.Data och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-134">Name the project "ImHere.Data", and then click **OK**.</span></span>

    ![Dialogrutan Lägg till nytt projekt](../media/5-add-new-net-standard-project.png)

1. <span data-ttu-id="b6dd8-136">Ta bort den automatiskt genererade filen Class1.cs.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-136">Delete the auto-generated "Class1.cs" file.</span></span>

1. <span data-ttu-id="b6dd8-137">Skapa en ny klass i `ImHere.Data`-projektet som heter `PostData` genom att högerklicka på projektet och sedan välja *Lägg till -> Klass …*. Ge den nya klassen namnet PostData och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-137">Create a new class in the `ImHere.Data` project called `PostData` by right-clicking on the project and then selecting *Add->Class...*. Name the new class "PostData" and click **OK**.</span></span> <span data-ttu-id="b6dd8-138">Markera den här nya klassen som `public`.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-138">Mark this new class as `public`.</span></span>

1. <span data-ttu-id="b6dd8-139">Lägg till `double`-egenskaper för latitud och longitud, samt en `string[]`-egenskap för telefonnumren att skicka till.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-139">Add `double` properties for the latitude and longitude, as well as a `string[]` property for the phone numbers to send to.</span></span>

    ```cs
    public class PostData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string[] ToNumbers { get; set; }
    }
    ```

1. <span data-ttu-id="b6dd8-140">Lägg till en referens till det här projektet både i `ImHere.Functions` och `ImHere` genom att högerklicka på projektet och välja *Lägg till -> Referens …* . Välj *Projekt* i trädvyn till vänster och markera sedan kryssrutan bredvid *ImHere.Data*.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-140">Add a reference to this project to both the `ImHere.Functions` and `ImHere` projects by right-clicking on the project and then selecting *Add->Reference...*. Select *Projects* from the tree on the left-hand side, and then check the box next to *ImHere.Data*.</span></span>

    ![Konfigurera projektreferenser](../media/5-configure-project-references.png)

## <a name="read-the-data-sent-to-the-function"></a><span data-ttu-id="b6dd8-142">Läsa data som skickas till funktionen</span><span class="sxs-lookup"><span data-stu-id="b6dd8-142">Read the data sent to the function</span></span>

<span data-ttu-id="b6dd8-143">Parametern `req` i Azure-funktionen innehåller den HTTP-begäran som skapades, och denna begärans data är ett JSON-serialiserat `PostData`-objekt.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-143">In the Azure function, the `req` parameter contains the HTTP request that was made and the data inside this request will be a JSON serialized `PostData` object.</span></span>

1. <span data-ttu-id="b6dd8-144">Öppna klassen `SendLocation` i `ImHere.Functions`-projektet.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-144">Open the `SendLocation` class in the `ImHere.Functions` project.</span></span>

1. <span data-ttu-id="b6dd8-145">Läs in innehållet i HTTP-begäran i en sträng, avserialisera den sedan i ett `PostData`-objekt, där du lägger till ett användningsdirektiv för namnrymden `ImHere.Data`.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-145">Read the contents of the HTTP request into a string, then deserialize it into a `PostData` object, adding a using directive for the `ImHere.Data` namespace.</span></span>

    ```cs
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    ```

1. <span data-ttu-id="b6dd8-146">Skapa en Google Maps-URL med hjälp av latitud och longitud från `PostData`-objektet.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-146">Construct a Google Maps URL using the latitude and longitude from the `PostData`.</span></span>

   ```cs
   string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
   ```

1. <span data-ttu-id="b6dd8-147">Logga URL:en.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-147">Log the URL.</span></span>

    ```cs
    log.LogInformation($"URL created - {url}");
    ```

1. <span data-ttu-id="b6dd8-148">Returnera en 200-statuskod för att visa att funktionen slutfördes utan fel.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-148">Return a 200 status code to show the function completed without error.</span></span>

    ```cs
    return new OkResult();
    ```

<span data-ttu-id="b6dd8-149">Den fullständiga funktionen visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-149">The complete function is shown below.</span></span>

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

## <a name="run-the-azure-function-locally"></a><span data-ttu-id="b6dd8-150">Felsöka Azure-funktionen lokalt</span><span class="sxs-lookup"><span data-stu-id="b6dd8-150">Run the Azure function locally</span></span>

<span data-ttu-id="b6dd8-151">Azure Functions kan köras lokalt med ett konto för lokal lagring och lokal Azure Functions-körning.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-151">Functions can be run locally using a local storage account and local Azure Functions runtime.</span></span> <span data-ttu-id="b6dd8-152">Med den här lokala körningen kan du testa funktionen innan du distribuerar den till Azure.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-152">This local runtime allows you to test out your function before deploying it to Azure.</span></span>

1. <span data-ttu-id="b6dd8-153">Högerklicka på projektet `ImHere.Functions` i Solution Explorer och välj *Ställ in som startprojekt*.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-153">Right-click on the `ImHere.Functions` project in the solution explorer, and then select *Set as StartUp project*.</span></span>

1. <span data-ttu-id="b6dd8-154">På menyn *Felsöka* väljer du *Starta utan felsökning*.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-154">From the *Debug* menu, select *Start Without Debugging*.</span></span> <span data-ttu-id="b6dd8-155">Den lokala Azure Functions-körningen startas i ett konsolfönster och startar din funktion, samt lyssnar på en tillgänglig port på `localhost`.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-155">The local Azure Functions runtime will launch inside a console window and start your function, listening on an available port on `localhost`.</span></span> <span data-ttu-id="b6dd8-156">Om du ser en dialogruta som frågar efter brandväggsåtkomst tillåter du åtkomst till privata nätverk (standardalternativet).</span><span class="sxs-lookup"><span data-stu-id="b6dd8-156">If you see a dialog asking for firewall access, allow access to private networks (the default option).</span></span>

    ![Azure-funktionen körs lokalt](../media/5-function-running-locally.png)

1. <span data-ttu-id="b6dd8-158">Anteckna vilken port funktionen lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-158">Take a note of the port that the function is listening on.</span></span> <span data-ttu-id="b6dd8-159">Du behöver den i nästa del för att testa mobilappen.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-159">You'll need this in the next unit to test out the mobile app.</span></span> <span data-ttu-id="b6dd8-160">I bilden ovan lyssnar funktionen på porten **7071**.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-160">In the image above, the function is listening on port **7071**.</span></span>

    ```sh
    Listening on http://localhost:7071/
    ```

1. <span data-ttu-id="b6dd8-161">Låt funktionen köras så att du kan testa mobilappen i nästa del.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-161">Leave the function running so that you can test the mobile app in the next unit.</span></span>

## <a name="summary"></a><span data-ttu-id="b6dd8-162">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b6dd8-162">Summary</span></span>

<span data-ttu-id="b6dd8-163">Här har du fått lära dig att skapa ett Azure Functions-projekt i Visual Studio, du har lagt till ett delat projekt med ett dataobjekt som ska delas mellan mobilappen och funktionen, samt fått lära dig hur du skapar en grundläggande implementering av funktionen för att deserialisera data som skickats.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-163">In this unit, you learned how to create an Azure Functions project in Visual Studio, added a shared project with a data object to be shared between the mobile app and the function, and learned how to create a basic implementation of the function to deserialize the data passed in.</span></span> <span data-ttu-id="b6dd8-164">Du har också fått lära dig att köra en Azure-funktion lokalt.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-164">You also learned how to run an Azure function locally.</span></span> <span data-ttu-id="b6dd8-165">Härnäst får du lära dig att anropa Azure-funktionen från mobilappen.</span><span class="sxs-lookup"><span data-stu-id="b6dd8-165">In the next unit, you'll call the Azure function from the mobile app.</span></span>