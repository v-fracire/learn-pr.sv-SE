<span data-ttu-id="6d1b4-101">Appen har ett användargränssnitt och en ViewModel.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-101">The app has a UI and a ViewModel.</span></span> <span data-ttu-id="6d1b4-102">I den här enheten lägger du till platssökning till ViewModel med Xamarin.Essentials.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-102">In this unit, you add location lookup to the ViewModel using Xamarin.Essentials.</span></span>

## <a name="enable-location-permissions"></a><span data-ttu-id="6d1b4-103">Aktivera behörigheter för plats</span><span class="sxs-lookup"><span data-stu-id="6d1b4-103">Enable location permissions</span></span>

<span data-ttu-id="6d1b4-104">Alla mobila plattformar har säkerhet kring användarinformation och viss maskinvara, som kameran, fotobiblioteket och användarens plats.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-104">All mobile platforms have security around user information and certain hardware, such as the camera, photo library, and the user's location.</span></span> <span data-ttu-id="6d1b4-105">Innan en app kan få åtkomst till användarens plats måste användaren bevilja behörighet – antingen genom att implicit bevilja behörigheterna vid installationen eller välja att bevilja en behörighet vid körning.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-105">Before an app can access the user's location, the user has to grant permission - either by implicitly granting these permissions at install time or by choosing to grant a permission at runtime.</span></span> <span data-ttu-id="6d1b4-106">När du visar en UWP-app i butiken visar listan vilka behörigheter som appen behöver.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-106">When you view a UWP app on the store, the listing will show the permissions that the app needs.</span></span> <span data-ttu-id="6d1b4-107">Genom att installera appen godkänner behörighet implicit.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-107">By installing the app, you implicitly grant permission.</span></span> <span data-ttu-id="6d1b4-108">Behörigheterna konfigureras i en appmanifestfil.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-108">These permissions are configured in an app manifest file.</span></span>

1. <span data-ttu-id="6d1b4-109">I approjektet `ImHere.UWP` öppnar du filen `Package.appxmanifest`.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-109">In the `ImHere.UWP` app project, open the `Package.appxmanifest` file.</span></span>

1. <span data-ttu-id="6d1b4-110">Gå till fliken **Funktioner** och markera funktionen *Plats*.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-110">Head to the **Capabilities** tab and check the *Location* capability.</span></span>

    ![Fliken med UWP-funktioner](../media/4-uwp-location-capability.png)

## <a name="query-for-the-users-location"></a><span data-ttu-id="6d1b4-112">Fråga efter användarens plats</span><span class="sxs-lookup"><span data-stu-id="6d1b4-112">Query for the user's location</span></span>

<span data-ttu-id="6d1b4-113">Det finns två sätt att hämta användarens plats – den senast kända eller den aktuella.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-113">There are two ways to get the user's location - the last known or the current.</span></span> <span data-ttu-id="6d1b4-114">Det kan ta lite tid att hämta den aktuella platsen eftersom enheten kanske måste upprätta en GPS-länk och vänta på att rätt plats hämtas.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-114">The current location can take some time to get because the device may need to establish a GPS link and wait for the accurate location to be retrieved.</span></span> <span data-ttu-id="6d1b4-115">Det snabbaste sättet är att hämta den senaste kända platsen som enheten har upptäckt.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-115">The fastest way is to get the last known location detected by the device.</span></span> <span data-ttu-id="6d1b4-116">Den senaste kända platsen är potentiellt mindre exakt men är ett mycket snabbare anrop.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-116">The last known location is potentially less accurate but is a much faster call.</span></span> <span data-ttu-id="6d1b4-117">Platser visas i latitud och longitud i [decimalgrader](https://en.wikipedia.org/wiki/Decimal_degrees?azure-portal=true) och enhetens altitud i meter över havet.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-117">Locations come as the latitude and longitude in [decimal degrees](https://en.wikipedia.org/wiki/Decimal_degrees?azure-portal=true) and the altitude of the device in meters above sea level.</span></span>

1. <span data-ttu-id="6d1b4-118">Öppna klassen `MainViewModel` i `ImHere` .NET-standardprojektet.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-118">Open the `MainViewModel` class in the `ImHere` .NET Standard project.</span></span>

1. <span data-ttu-id="6d1b4-119">I metoden `SendLocation` gör du ett anrop till den `GetLastKnownLocationAsync` statiska metoden i klassen `Geolocation` i namnområdet `Xamarin.Essentials`.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-119">In the `SendLocation` method, make a call to the `GetLastKnownLocationAsync` static method on the `Geolocation` class in the `Xamarin.Essentials` namespace.</span></span> <span data-ttu-id="6d1b4-120">Du måste lägga till ett användningsdirektiv för namnområdet `Xamarin.Essentials`.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-120">You will need to add a using directive for the `Xamarin.Essentials` namespace.</span></span>

    ```csharp
    Location location = await Geolocation.GetLastKnownLocationAsync();
    ```

1. <span data-ttu-id="6d1b4-121">Uppdatera egenskapen `Message` med användarens plats om den hittas.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-121">Update the `Message` property with the user's location if one is found.</span></span>

    ```csharp
    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
    ```

    <span data-ttu-id="6d1b4-122">Den fullständiga koden för den här metoden finns nedan.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-122">The full code for this method is below.</span></span>
    
    ```csharp
    async Task SendLocation()
    {
        Location location = await Geolocation.GetLastKnownLocationAsync();
    
        if (location != null)
        {
            Message = $"Location found: {location.Latitude}, {location.Longitude}.";
        }
    }
    ```

1. <span data-ttu-id="6d1b4-123">Kör appen och klicka på knappen **Send Location** (Skicka plats) för att se platsen i användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-123">Run the app and click the **Send Location** button to see the location on the UI.</span></span>

    ![Appen som körs visar användarens plats](../media/4-running-app-showing-location.png)    

> [!NOTE]
> <span data-ttu-id="6d1b4-125">I den här appen används den senaste kända platsen.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-125">This app uses the last known location.</span></span> <span data-ttu-id="6d1b4-126">I en app med produktionskvalitet kan du hämta den aktuella platsen med en tidsgräns, och om ingen hittas i tid återgår du till den senast kända.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-126">In a production-quality app, you would want to get the current accurate location with a time-out, and if one is not found in time, fall back to the last known.</span></span> <span data-ttu-id="6d1b4-127">Du kan läsa mer om det i [Xamarin.Essentials Geolocation.docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="6d1b4-127">You can read more on how to do this in the [Xamarin.Essentials Geolocation docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation?azure-portal=true).</span></span>
> 
> <span data-ttu-id="6d1b4-128">Den här appen har inte någon felhantering.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-128">This app does not have error handling.</span></span> <span data-ttu-id="6d1b4-129">I en app med produktionskvalitet kan du hantera eventuella undantag som sker, exempelvis om platsen inte var tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-129">In a production-quality app, you should handle any exceptions that occur, such as if the location was not available.</span></span>

## <a name="summary"></a><span data-ttu-id="6d1b4-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6d1b4-130">Summary</span></span>

<span data-ttu-id="6d1b4-131">I den här delen har du lärt dig att använda Xamarin.Essentials för att hämta användarens plats.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-131">In this unit, you learned how to use Xamarin.Essentials to get the user's location.</span></span> <span data-ttu-id="6d1b4-132">I nästa del skapar du en Azure-funktion som ska fungera som en serverdel för mobilappen.</span><span class="sxs-lookup"><span data-stu-id="6d1b4-132">In the next unit, you'll create an Azure function to act as a back end for the mobile app.</span></span>