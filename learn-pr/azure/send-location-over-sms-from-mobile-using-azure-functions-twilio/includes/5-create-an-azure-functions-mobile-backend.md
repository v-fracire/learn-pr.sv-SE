<span data-ttu-id="90498-101">Nu hämtar appen användarens plats och den är redo att skickas till en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="90498-101">At this point, the app is working to get the user's location and is ready to be sent to an Azure function.</span></span> <span data-ttu-id="90498-102">I den här delen får du skapa Azure-funktionen.</span><span class="sxs-lookup"><span data-stu-id="90498-102">In this unit, you build the Azure function.</span></span>

## <a name="create-an-azure-functions-project"></a><span data-ttu-id="90498-103">Skapa ett Azure Functions-projekt</span><span class="sxs-lookup"><span data-stu-id="90498-103">Create an Azure Functions project</span></span>

1. <span data-ttu-id="90498-104">Lägg till ett nytt projekt i `ImHere`-lösningen genom att högerklicka på lösningen och välja *Lägg till -> Nytt projekt …* .</span><span class="sxs-lookup"><span data-stu-id="90498-104">Add a new project to the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

1. <span data-ttu-id="90498-105">Välj *Visual C# -> Molnet* i trädvyn till vänster och välj sedan *Azure Functions* på panelen i mitten.</span><span class="sxs-lookup"><span data-stu-id="90498-105">From the tree on the left-hand side, select *Visual C#->Cloud*, and then select *Azure Functions* from the panel in the center.</span></span>

1. <span data-ttu-id="90498-106">Ange projektnamnet ImHere.Functions och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="90498-106">Name the project "ImHere.Functions", and then click **OK**.</span></span>

    ![Dialogrutan Lägg till nytt projekt](../media-drafts/5-add-new-functions-project.png)

1. <span data-ttu-id="90498-108">I konfigurationsdialogrutan **Nytt projekt** låter du Azure Functions-versionen vara *Azure Functions v1 (.NET Framework)*.</span><span class="sxs-lookup"><span data-stu-id="90498-108">In the **New Project** configuration dialog, leave the Functions version set to *Azure Functions v1 (.NET Framework)*.</span></span> <span data-ttu-id="90498-109">Välj *HTTP-utlösare*, låt lagringskontot vara inställt som *Lagringsemulator* och ange åtkomstbehörigheter för *Anonym*.</span><span class="sxs-lookup"><span data-stu-id="90498-109">Select *Http Trigger*, leave the storage account set to *Storage Emulator*, and set the access rights to *Anonymous*.</span></span> <span data-ttu-id="90498-110">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="90498-110">Then click **OK**.</span></span>

    ![Konfigurationsdialogrutan för Azure Function-projekt](../media-drafts/5-configure-trigger.png)

<span data-ttu-id="90498-112">Det nya projektet skapas och har en standardfunktion som kallas `Function1`.</span><span class="sxs-lookup"><span data-stu-id="90498-112">The new project will be created and have a default function called `Function1`.</span></span>

> <span data-ttu-id="90498-113">Den här funktionen har skapats med anonym åtkomst.</span><span class="sxs-lookup"><span data-stu-id="90498-113">This function was created with anonymous access.</span></span> <span data-ttu-id="90498-114">När den har publicerats till Azure kan alla som känner till webbadressen anropa den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="90498-114">Once published to Azure, anybody who knows the URL will be able to call this function.</span></span> <span data-ttu-id="90498-115">Om det här var ett exempel ur verkligheten skulle du ha skyddat den med någon form av autentisering, till exempel [Azure App Service-autentisering](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) eller [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c).</span><span class="sxs-lookup"><span data-stu-id="90498-115">In a real-world scenario, you would protect this with some form of authentication, such as [Azure App Service authentication](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) or [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c).</span></span>

## <a name="create-the-function"></a><span data-ttu-id="90498-116">Skapa funktionen</span><span class="sxs-lookup"><span data-stu-id="90498-116">Create the function</span></span>

<span data-ttu-id="90498-117">Azure Functions-projektet har skapats med en HTTP-utlösarfunktion som kallas `Function1`.</span><span class="sxs-lookup"><span data-stu-id="90498-117">The Azure Functions project is created with a single HTTP trigger function called `Function1`.</span></span> <span data-ttu-id="90498-118">Själva funktionen implementeras som en statisk `Run`-metod i `Function1`-klassen.</span><span class="sxs-lookup"><span data-stu-id="90498-118">The function itself is implemented as a static `Run` method in the `Function1` class.</span></span>

1. <span data-ttu-id="90498-119">Byt namn på filen i Solution Explorer från Function1.cs till SendLocation.cs.</span><span class="sxs-lookup"><span data-stu-id="90498-119">Rename the file in Solution Explorer from "Function1.cs" to "SendLocation.cs".</span></span> <span data-ttu-id="90498-120">När du uppmanas att byta namn på alla referenser till kodelementet `Function1` klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="90498-120">When prompted to rename all references to the code element `Function1`, click **Yes**.</span></span>

1. <span data-ttu-id="90498-121">Byt namn på funktionen i attributet till SendLocation.</span><span class="sxs-lookup"><span data-stu-id="90498-121">Rename the function name in the attribute to "SendLocation".</span></span>

    ```cs
    [FunctionName("SendLocation")]
    ```

1. <span data-ttu-id="90498-122">Ta bort innehållet i funktionen, förutom den första raden som skriver ett informationsmeddelande till loggaren.</span><span class="sxs-lookup"><span data-stu-id="90498-122">Delete the contents of the function, except the first line that writes an information message to the logger.</span></span>

    ```cs
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                       TraceWriter log)
    {
        log.Info("C# HTTP trigger function processed a request.");
    }
    ```

## <a name="create-a-class-to-share-data-between-the-mobile-app-and-function"></a><span data-ttu-id="90498-123">Skapa en klass för att dela data mellan mobilappen och funktionen</span><span class="sxs-lookup"><span data-stu-id="90498-123">Create a class to share data between the mobile app and function</span></span>

<span data-ttu-id="90498-124">När data skickas till Azure-funktionen sker det i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="90498-124">When data is sent to the Azure function, it will be sent as JSON.</span></span> <span data-ttu-id="90498-125">Mobilappen serialiserar data till JSON och funktionen deserialiserar från JSON.</span><span class="sxs-lookup"><span data-stu-id="90498-125">The mobile app will serialize data into JSON and the function will deserialize from JSON.</span></span> <span data-ttu-id="90498-126">Om du vill hålla dessa data konsekventa mellan mobilappen och funktionen skapar du ett nytt projekt som innehåller en klass för att lagra plats- och telefonnummerdata.</span><span class="sxs-lookup"><span data-stu-id="90498-126">To keep this data consistent between the mobile app and the function, create a new project that contains a class to hold the location and phone number data.</span></span> <span data-ttu-id="90498-127">Det här projektet refereras sedan av appen och funktionen.</span><span class="sxs-lookup"><span data-stu-id="90498-127">This project will then be referenced by the app and function.</span></span>

1. <span data-ttu-id="90498-128">Skapa ett nytt projekt under `ImHere`-lösningen genom att högerklicka på lösningen och välja *Lägg till -> Nytt projekt …*.</span><span class="sxs-lookup"><span data-stu-id="90498-128">Create a new project under the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

1. <span data-ttu-id="90498-129">Välj *Visual C# -> .NET Standard* i trädvyn till vänster och sedan *Klassbibliotek (.NET Standard)* på panelen i mitten.</span><span class="sxs-lookup"><span data-stu-id="90498-129">From the tree on the left-hand side, select *Visual C#->.NET Standard*, and then select *Class Library (.NET Standard)* from the panel in the center.</span></span>

1. <span data-ttu-id="90498-130">Ange projektnamnet ImHere.Data och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="90498-130">Name the project "ImHere.Data", and then click **OK**.</span></span>

    ![Dialogrutan Lägg till nytt projekt](../media-drafts/5-add-new-net-standard-project.png)

1. <span data-ttu-id="90498-132">Ta bort den automatiskt genererade filen Class1.cs.</span><span class="sxs-lookup"><span data-stu-id="90498-132">Delete the auto-generated "Class1.cs" file.</span></span>

1. <span data-ttu-id="90498-133">Skapa en ny klass i `ImHere.Data`-projektet som heter `PostData` genom att högerklicka på projektet och sedan välja *Lägg till -> Klass …*. Ge den nya klassen namnet PostData och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="90498-133">Create a new class in the `ImHere.Data` project called `PostData` by right-clicking on the project and then selecting *Add->Class...*. Name the new class "PostData" and click **OK**.</span></span>

1. <span data-ttu-id="90498-134">Lägg till `double`-egenskaper för latitud och longitud, samt en `string[]`-egenskap för telefonnumren att skicka till.</span><span class="sxs-lookup"><span data-stu-id="90498-134">Add `double` properties for the latitude and longitude, as well as a `string[]` property for the phone numbers to send to.</span></span>

    ```cs
    public class PostData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string[] ToNumbers { get; set; }
    }
    ```

1. <span data-ttu-id="90498-135">Lägg till en referens till det här projektet både i `ImHere.Functions` och `ImHere` genom att högerklicka på projektet och välja *Lägg till -> Referens …* . Välj *Projekt* i trädvyn till vänster och markera sedan kryssrutan bredvid *ImHere.Data*.</span><span class="sxs-lookup"><span data-stu-id="90498-135">Add a reference to this project to both the `ImHere.Functions` and `ImHere` projects by right-clicking on the project and then selecting *Add->Reference...*. Select *Projects* from the tree on the left-hand side, and then check the box next to *ImHere.Data*.</span></span>

    ![Konfigurera projektreferenser](../media-drafts/5-configure-project-references.png)

## <a name="read-the-data-sent-to-the-function"></a><span data-ttu-id="90498-137">Läsa data som skickas till funktionen</span><span class="sxs-lookup"><span data-stu-id="90498-137">Read the data sent to the function</span></span>

<span data-ttu-id="90498-138">Parametern `req` i Azure-funktionen innehåller den HTTP-begäran som skapades, och denna begärans data är ett JSON-serialiserat `PostData`-objekt.</span><span class="sxs-lookup"><span data-stu-id="90498-138">In the Azure function, the `req` parameter contains the HTTP request that was made and the data inside this request will be a JSON serialized `PostData` object.</span></span>

1. <span data-ttu-id="90498-139">Öppna klassen `SendLocation` i `ImHere.Functions`-projektet.</span><span class="sxs-lookup"><span data-stu-id="90498-139">Open the `SendLocation` class in the `ImHere.Functions` project.</span></span>

1. <span data-ttu-id="90498-140">Läs innehållet i HTTP-begäran till ett `PostData`-objekt och lägg till ett using-direktiv för `ImHere.Data`-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="90498-140">Read the contents of the HTTP request into a `PostData` object, adding a using directive for the `ImHere.Data` namespace.</span></span>

    ```cs
    PostData data = await req.Content.ReadAsAsync<PostData>();
    ```

1. <span data-ttu-id="90498-141">Skapa en Google Maps-URL med hjälp av latitud och longitud från `PostData`-objektet.</span><span class="sxs-lookup"><span data-stu-id="90498-141">Construct a Google Maps URL using the latitude and longitude from the `PostData`.</span></span>

   ```cs
   string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
   ```

1. <span data-ttu-id="90498-142">Logga URL:en.</span><span class="sxs-lookup"><span data-stu-id="90498-142">Log the URL.</span></span>

    ```cs
    log.Info($"URL created - {url}");
    ```

1. <span data-ttu-id="90498-143">Returnera en 200-statuskod för att visa att funktionen slutfördes utan fel.</span><span class="sxs-lookup"><span data-stu-id="90498-143">Return a 200 status code to show the function completed without error.</span></span>

    ```cs
    return req.CreateResponse(HttpStatusCode.OK);
    ```

<span data-ttu-id="90498-144">Den fullständiga funktionen visas nedan.</span><span class="sxs-lookup"><span data-stu-id="90498-144">The complete function is shown below.</span></span>

```cs
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                                Route = null)]HttpRequestMessage req,
                                                    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    PostData data = await req.Content.ReadAsAsync<PostData>();
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.Info($"URL created - {url}");
    return req.CreateResponse(HttpStatusCode.OK);
}
```

## <a name="run-the-azure-function-locally"></a><span data-ttu-id="90498-145">Felsöka Azure-funktionen lokalt</span><span class="sxs-lookup"><span data-stu-id="90498-145">Run the Azure function locally</span></span>

<span data-ttu-id="90498-146">Azure Functions kan köras lokalt med ett konto för lokal lagring och lokal Azure Functions-körning.</span><span class="sxs-lookup"><span data-stu-id="90498-146">Functions can be run locally using a local storage account and local Azure Functions runtime.</span></span> <span data-ttu-id="90498-147">Med den här lokala körningen kan du testa funktionen innan du distribuerar den till Azure.</span><span class="sxs-lookup"><span data-stu-id="90498-147">This local runtime allows you to test out your function before deploying it to Azure.</span></span>

1. <span data-ttu-id="90498-148">Högerklicka på projektet `ImHere.Functions` i Solution Explorer och välj *Ställ in som startprojekt*.</span><span class="sxs-lookup"><span data-stu-id="90498-148">Right-click on the `ImHere.Functions` project in the solution explorer, and then select *Set as StartUp project*.</span></span>

1. <span data-ttu-id="90498-149">På menyn *Felsöka* väljer du *Starta utan felsökning*.</span><span class="sxs-lookup"><span data-stu-id="90498-149">From the *Debug* menu, select *Start Without Debugging*.</span></span> <span data-ttu-id="90498-150">Den lokala Azure Functions-körningen startas i ett konsolfönster och startar din funktion, samt lyssnar på en tillgänglig port på `localhost`.</span><span class="sxs-lookup"><span data-stu-id="90498-150">The local Azure Functions runtime will launch inside a console window and start your function, listening on an available port on `localhost`.</span></span>

    ![Azure-funktionen körs lokalt](../media-drafts/5-function-running-locally.png)

1. <span data-ttu-id="90498-152">Anteckna vilken port funktionen lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="90498-152">Take a note of the port that the function is listening on.</span></span> <span data-ttu-id="90498-153">Du behöver den i nästa del för att testa mobilappen.</span><span class="sxs-lookup"><span data-stu-id="90498-153">You'll need this in the next unit to test out the mobile app.</span></span> <span data-ttu-id="90498-154">I bilden ovan lyssnar funktionen på porten **7071**.</span><span class="sxs-lookup"><span data-stu-id="90498-154">In the image above, the function is listening on port **7071**.</span></span>

    ```sh
    Listening on http://localhost:7071/
    ```

1. <span data-ttu-id="90498-155">Låt funktionen köras så att du kan testa mobilappen i nästa del.</span><span class="sxs-lookup"><span data-stu-id="90498-155">Leave the function running so that you can test the mobile app in the next unit.</span></span>

## <a name="summary"></a><span data-ttu-id="90498-156">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="90498-156">Summary</span></span>

<span data-ttu-id="90498-157">Här har du fått lära dig att skapa ett Azure Functions-projekt i Visual Studio, du har lagt till ett delat projekt med ett dataobjekt som ska delas mellan mobilappen och funktionen, samt fått lära dig hur du skapar en grundläggande implementering av funktionen för att deserialisera data som skickats.</span><span class="sxs-lookup"><span data-stu-id="90498-157">In this unit, you learned how to create an Azure Functions project in Visual Studio, added a shared project with a data object to be shared between the mobile app and the function, and learned how to create a basic implementation of the function to deserialize the data passed in.</span></span> <span data-ttu-id="90498-158">Du har också fått lära dig att köra en Azure-funktion lokalt.</span><span class="sxs-lookup"><span data-stu-id="90498-158">You also learned how to run an Azure function locally.</span></span> <span data-ttu-id="90498-159">Härnäst får du lära dig att anropa Azure-funktionen från mobilappen.</span><span class="sxs-lookup"><span data-stu-id="90498-159">In the next unit, you'll call the Azure function from the mobile app.</span></span>