Appen har ett användargränssnitt och en ViewModel. I den här enheten lägger du till platssökning till ViewModel med Xamarin.Essentials.

## <a name="enable-location-permissions"></a>Aktivera behörigheter för plats

Alla mobila plattformar har säkerhet kring användarinformation och viss maskinvara, som kameran, fotobiblioteket och användarens plats. Innan en app kan få åtkomst till användarens plats måste användaren bevilja behörighet – antingen genom att implicit bevilja behörigheterna vid installationen eller välja att bevilja en behörighet vid körning. När du visar en UWP-app i butiken visar listan vilka behörigheter som appen behöver. Genom att installera appen godkänner behörighet implicit. Behörigheterna konfigureras i en appmanifestfil.

1. I approjektet `ImHere.UWP` öppnar du filen `Package.appxmanifest`.

2. Gå till fliken **Funktioner** och markera funktionen *Plats*.

    ![Fliken med UWP-funktioner](../media/4-uwp-location-capability.png)

> Om du vill stödja Android eller iOS måste behörigheterna konfigureras på olika sätt. Det beskrivs i [Xamarin.Essentials Geolocation docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=android#getting-started).

## <a name="query-for-the-users-location"></a>Fråga efter användarens plats

Det finns två sätt att hämta användarens plats – den senast kända eller den aktuella. Det kan ta lite tid att hämta den aktuella platsen eftersom enheten kanske måste upprätta en GPS-länk och vänta på att rätt plats hämtas. Det snabbaste sättet är att hämta den senaste kända platsen som enheten har upptäckt. Den senaste kända platsen är potentiellt mindre exakt men är ett mycket snabbare anrop. Platser visas i latitud och longitud i [decimalgrader](https://en.wikipedia.org/wiki/Decimal_degrees) och enhetens altitud i meter över havet.

1. Öppna klassen `MainViewModel` i `ImHere` .NET-standardprojektet.

2. I metoden `SendLocation` gör du ett anrop till den `GetLastKnownLocationAsync` statiska metoden i klassen `Geolocation` i namnområdet `Xamarin.Essentials`.

    ```cs
    Location location = await Geolocation.GetLastKnownLocationAsync();
    ```

3. Uppdatera egenskapen `Message` med användarens plats om den hittas.

    ```cs
    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
    ```

Den fullständiga koden för den här metoden finns nedan.

```cs
async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
}
```

Kör appen och klicka på knappen **Send Location** (Skicka plats) för att se platsen i användargränssnittet.

![Appen som körs visar användarens plats](../media/4-running-app-showing-location.png)

> I den här appen används den senaste kända platsen. I en app med produktionskvalitet kan du hämta den aktuella platsen med en tidsgräns, och om ingen hittas i tid återgår du till den senast kända. Du kan läsa mer om det i [Xamarin.Essentials Geolocation.docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation). Den här appen har inte felhantering. I en app med produktionskvalitet kan du hantera eventuella undantag som sker, exempelvis om platsen inte var tillgänglig och ett undantag uppstod.

## <a name="summary"></a>Sammanfattning

I den här delen har du lärt dig att använda Xamarin.Essentials för att hämta användarens plats. I nästa del skapar du en Azure-funktion som ska fungera som en serverdel för mobilappen.