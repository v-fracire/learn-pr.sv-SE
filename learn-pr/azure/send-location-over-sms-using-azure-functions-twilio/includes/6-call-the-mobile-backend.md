<span data-ttu-id="6e4a1-101">Mobilappen körs och den första versionen av Azure-funktionen har skapats.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-101">The mobile app runs and the initial version of the Azure function has been created.</span></span> <span data-ttu-id="6e4a1-102">I den här enheten anropar du Azure-funktionen från mobilappen, och skickar som indata användarens plats och en lista med telefonnummer som ska få SMS.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-102">In this unit, you call the Azure function from the mobile app, passing in the user's location and the list of phone numbers the user wants to send SMS messages to.</span></span>

## <a name="calling-the-azure-function-from-the-mobile-app"></a><span data-ttu-id="6e4a1-103">Anropa Azure-funktionen från mobilappen</span><span class="sxs-lookup"><span data-stu-id="6e4a1-103">Calling the Azure function from the mobile app</span></span>

1. <span data-ttu-id="6e4a1-104">Öppna `MainViewModel`.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-104">Open the `MainViewModel`.</span></span>

1. <span data-ttu-id="6e4a1-105">Lägg till ett privat `HttpClient`-fält med namnet `client` i klassen.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-105">In this class, add a private `HttpClient` field called `client`.</span></span> <span data-ttu-id="6e4a1-106">Du måste lägga till en referens till namnområdet `System.Net.Http`.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-106">You'll need to add a reference to the `System.Net.Http` namespace.</span></span>

    ```cs
    HttpClient client = new HttpClient();
    ```

1. <span data-ttu-id="6e4a1-107">Lägg till ett konstant fält för funktionens basadress.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-107">Add a constant field for the base URL for the function.</span></span> <span data-ttu-id="6e4a1-108">Använd adressen som den lokala körningsmiljön för Azure Functions lyssnar vid.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-108">Set this to the address that the local Azure Functions runtime is listening on.</span></span> <span data-ttu-id="6e4a1-109">När funktionen distribueras till Azure kan du ändra den här konstanten till webbadressen för Azure.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-109">Once the function is deployed to Azure, this constant can be changed to be the Azure URL.</span></span>

    ```cs
    const string baseUrl = "http://localhost:7071";
    ```

1. <span data-ttu-id="6e4a1-110">När du har hittat platsen ska du i metoden `SendLocation` skapa en ny instans av `PostData` med hjälp av platsen och en lista med telefonnummer som användaren anger.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-110">Inside the `SendLocation` method, after the location has been found, create a new instance of `PostData` using the location and the list of phone numbers entered by the user.</span></span> <span data-ttu-id="6e4a1-111">Du måste lägga till ett användningsdirektiv för namnområdet `ImHere.Data`.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-111">You'll need to add a using directive for the `ImHere.Data` namespace.</span></span>

    ```cs
    PostData postData = new PostData
    {
        Latitude = location.Latitude,
        Longitude = location.Longitude,
        ToNumbers = PhoneNumbers.Split('\n')
    };
    ```

    > <span data-ttu-id="6e4a1-112">Det här förutsätter att telefonnumren har angetts med rätt format, ett per rad i `Editor`-kontrollen.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-112">This assumes that the phone numbers have been entered in the correct format, one per line in the `Editor` control.</span></span> <span data-ttu-id="6e4a1-113">I en produktionsklar app skulle det kontrolleras att minst ett telefonnummer har angetts med rätt format.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-113">In a production-quality app, there would be validation around this to ensure one or more phone numbers were entered and were in the correct format.</span></span>

1. <span data-ttu-id="6e4a1-114">Om du vill serialisera `PostData` som JSON är det enklast att använda NuGet-paketet Newtonsoft.JSON.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-114">To serialize the `PostData` as JSON, the easiest way is to use the Newtonsoft.JSON NuGet package.</span></span> <span data-ttu-id="6e4a1-115">Lägg till det här NuGet-paketet i projektet `ImHere` på samma sätt som du lade till Xamarin.Essentials i en tidigare utbildningsenhet.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-115">Add this NuGet package to the `ImHere` project in the same way that you added Xamarin.Essentials in an earlier unit.</span></span>

1. <span data-ttu-id="6e4a1-116">Serialisera `PostData` till en `string` med hjälp av den statiska klassen `JsonConvert`.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-116">Serialize the `PostData` to a `string` using the `JsonConvert` static class.</span></span> <span data-ttu-id="6e4a1-117">Du måste lägga till ett användningsdirektiv för namnområdet `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-117">You'll need to add a using directive for the `Newtonsoft.Json` namespace.</span></span> <span data-ttu-id="6e4a1-118">Koda om den här strängen till en `StringContent`-klass så att den kan skickas till Azure-funktionen som JSON.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-118">Encode this string into a `StringContent` class so that it can be passed to the Azure function as JSON.</span></span>

    ```cs
    string data = JsonConvert.SerializeObject(postData);
    StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
    ```

1. <span data-ttu-id="6e4a1-119">Publicera dessa data till funktionen och hämta resultatet.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-119">Post this data to the function and get the result back.</span></span>

   ```cs
    HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                        content);
   ```

   <span data-ttu-id="6e4a1-120">Du kommer åt Azure-funktioner med `/api/<function name>`, så om den port som väljs i den lokala körningsmiljön för Functions är 7071 så kan funktionen `SendLocation` nås via `http://localhost:7071/api/SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-120">Azure functions are accessed using `/api/<function name>`, so assuming the port chosen by the local Functions runtime is 7071, the `SendLocation` function will be accessible at `http://localhost:7071/api/SendLocation`.</span></span>

1. <span data-ttu-id="6e4a1-121">Visa ett meddelande i gränssnittet beroende på resultatet.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-121">Depending on the result, show a message on the UI.</span></span>

    ```cs
    if (result.IsSuccessStatusCode)
        Message = "Location sent successfully";
    else
        Message = $"Error - {result.ReasonPhrase}";
    ```

<span data-ttu-id="6e4a1-122">Nedan visas den fullständiga koden för de nya fälten och metoden `SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-122">The full code for the new fields and the `SendLocation` method is below.</span></span>

```cs
HttpClient client = new HttpClient();
const string baseUrl = "http://localhost:7071";

async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";

        PostData postData = new PostData
        {
            Latitude = location.Latitude,
            Longitude = location.Longitude,
            ToNumbers = PhoneNumbers.Split('\n')
        };

        string data = JsonConvert.SerializeObject(postData);
        StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
        HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                            content);

        if (result.IsSuccessStatusCode)
            Message = "Location sent successfully";
        else
            Message = $"Error - {result.ReasonPhrase}";
    }
}
```

## <a name="testing-it-out"></a><span data-ttu-id="6e4a1-123">Testa den</span><span class="sxs-lookup"><span data-stu-id="6e4a1-123">Testing it out</span></span>

1. <span data-ttu-id="6e4a1-124">Se till att Azure-funktionen fortfarande körs lokalt och att porten matchar metoden `SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-124">Make sure the Azure function is still running locally and the port matches the `SendLocation` method.</span></span>

1. <span data-ttu-id="6e4a1-125">Ange appen UWP som startapp och kör den.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-125">Set the UWP app as the startup app and run it.</span></span> <span data-ttu-id="6e4a1-126">Klicka på knappen **Send Location**.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-126">Click the **Send Location** button.</span></span> <span data-ttu-id="6e4a1-127">Du ser utdata från funktionen som anropas i konsolfönstret för Functions och loggar med den genererade webbadressen.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-127">You'll see output in the Functions runtime console window showing the function being called, and the logging showing the generated URL.</span></span>

    ![Utdata från funktionen som anropas](../media-drafts/6-function-called.png)

1. <span data-ttu-id="6e4a1-129">Du kan testa adressgenereringen genom att klistra in adressen från konsolen i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-129">To test the URL generation, paste it from the console into a browser.</span></span> <span data-ttu-id="6e4a1-130">Du bör se din aktuella plats.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-130">It should show your current location.</span></span>

## <a name="summary"></a><span data-ttu-id="6e4a1-131">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6e4a1-131">Summary</span></span>

<span data-ttu-id="6e4a1-132">I den här utbildningsenheten har du fått lära dig att anropa en Azure-funktion från mobilappen.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-132">In this unit, you learned how to call an Azure function from the mobile app.</span></span> <span data-ttu-id="6e4a1-133">I anropet skickades användarens plats och angivna telefonnummer som JSON.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-133">This call passed the user's location and the phone numbers they entered as JSON.</span></span> <span data-ttu-id="6e4a1-134">I nästa utbildningsenhet kommer du att binda Azure-funktionen till Twilio och skicka platsen som ett SMS.</span><span class="sxs-lookup"><span data-stu-id="6e4a1-134">In the next unit, you'll bind the Azure function to Twilio to send this location as an SMS message.</span></span>