Du har skapat din webbapp och publicerat den på Azure, men vad händer när du vill göra ändringar? Visual Studio kommer ihåg var appen publicerades, vilket gör att det bara krävs två klick för att uppdatera och ändra appen.

## <a name="explore-the-project-structure"></a>Utforska projektstrukturen

Vi har skapat en ASP.NET-webbapp i Visual Studio, och nu behöver du redigera och anpassa din webbplats. Vi utforskar projektstrukturen för att se vad Visual Studio har skapat åt oss.

### <a name="dependencies"></a>Beroenden

Beroenden inkluderar ASP.NET-systemarkitekturen för att köra igång din app. Såvida du inte lägger till specifika tredjepartspaket behöver du inte tillbringa särskilt mycket tid i den här mappen.

### <a name="properties"></a>Egenskaper

Egenskapsmappen innehåller konfigurationsdata för det ställe som är värd för din webbapp. Om du expanderar mappen **PublishProfiles** nu bör du se URL:en för Alpine Ski Hill-webbplatsen. Varje publiceringsprofil är en XML-fil som innehåller information om publiceringskonfiguration, till exempel den Azure-adress som Visual Studio använder för att ladda upp dina filer.

### <a name="wwwroot"></a>wwwroot

Mappen wwwroot innehåller alla dina statiska resurser för webbplatsen, som bilder, CSS-, JS- och LIB-filer. När du är redo att välja stil och lägga till fler funktioner för webbplatsen är det här du arbetar.

### <a name="pages"></a>Sidor

Mappen **Pages** (Sidor) innehåller _**Razor**_-mallar för sidorna på webbplatsen.
Razor är en syntax som är uppbyggd kring HTML, med särskilda regler för att visa data och köra logik på webbplatsen.

Varje sida på webbplatsen representeras av två kodfiler:

- Den första är en `.cshtml`-fil, som är Razor-kodfilen. Den här filen innehåller HTML för visning och en del C#-logik.

- Den andra filen är en `.cs` -fil som är C# bakomliggande kod som innehåller `PageModel` klass. Den här filen gör att du kan fånga upp HTTP-begäranden och utföra viss bearbetning i begärandena innan data skickas till Razor-filen.

### <a name="appsettingjson"></a>appsetting.json

Det här är en konfigurationsfil för ASP.NET.

### <a name="bundleconfigjson"></a>bundleconfig.json

Bundleconfig.json är förbearbetningskonfiguration. Den här filen gör dina CSS- och JS-filer mindre när de publiceras.

### <a name="programcs-and-startupcs"></a>Program.cs och Startup.cs

Program.cs och Startup.cs konfigurerar och startar webbvärden för din webbplats.

## <a name="updating-your-website-using-razor"></a>Uppdatera din webbplats med hjälp av Razor

Några grundläggande ändringar behöver göras på webbplatsen. För att kunna göra det måste du ha grundläggande kunskaper i att använda Razor-mallarna för att anpassa din webbapp.

## <a name="what-is-razor"></a>Vad är Razor?

Razor är en ASP.NET-syntax som används för att skapa dynamiska webbsidor med C#. När en server läser en Razor-sida körs C#-koden innan den renderar HTML. På så sätt kan du snabbt skapa dynamiskt innehåll.

Razor använder `@`-direktiv som talar om för ASP.NET hur det ska bearbeta en sida.

Till exempel ta en titt på koden i den `Contact.cshtml` sidan:

```aspx-csharp
@page
@model ContactModel
@{
    ViewData["Title"] = "Contact";
}
<h2>@ViewData["Title"]</h2>
<h3>@Model.Message</h3>
...
```

- Den `@page` direktiv talar om ASP.NET att bearbeta den här filen som en Razor-sida.
- `@model`-direktivet instruerar ASP.NET att koppla den här Razor-sidan till en C#-klass som heter `ContactModel`.

Razor använder också `@`-symbolen som övergång mellan kod och HTML. Om du tittar på fragmentet ovan, ser du `@{ ... }`. Det här är ett **Razor-kodblock**. Det vill säga kod som _körs men inte renderas_.

För att rendera utdata för en kodinstruktion använder vi `@` framför ett C#-uttryck. Det finns två exempel på det i kodblocket ovan i taggarna `<h2>` och `<h3>`.

## <a name="publish-your-updates"></a>Publicera dina uppdateringar

När du har gjort ändringarna på webbplatsen är det dags att publicera dem till Azure. Den här processen liknar det sätt som vi först publicerade på.

1. Högerklicka på projektet i Solution Explorer.

1. Nu bör du se namnet på din webbplats följt av Web Deploy. Om du har namngett din webbplats AlpineSkiHouse42, ser du **AlpineSkiHouse42 - webbdistribution** i de tillgängliga alternativen. Välj det så uppdateras webbplatsen i Azure.

## <a name="summary"></a>Sammanfattning

Att skapa och publicera en webbplats är bara de första stegen för att skapa en bra webbplats. När du börjar lägga till innehåll kommer du behöva uppdatera webbplatsen. När du väl har publicerat webbplatsen till Azure kan du uppdatera när som helst från Solution Explorer.
