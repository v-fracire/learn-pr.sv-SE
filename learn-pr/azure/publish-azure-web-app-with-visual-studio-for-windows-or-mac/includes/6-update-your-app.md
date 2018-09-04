<span data-ttu-id="ffa7f-101">Du har skapat din webbapp och publicerat den på Azure, men vad händer när du vill göra ändringar?</span><span class="sxs-lookup"><span data-stu-id="ffa7f-101">You've successfully created your web app and published it to Azure, but what happens when you want to make changes?</span></span> <span data-ttu-id="ffa7f-102">Visual Studio kommer ihåg där appen publicerades, vilket gör att det bara krävs två klick för att uppdatera och ändra appen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-102">Visual Studio will remember where the app is published which makes updating and changing your app a two click process.</span></span>

## <a name="explore-the-project-structure"></a><span data-ttu-id="ffa7f-103">Utforska projektstrukturen</span><span class="sxs-lookup"><span data-stu-id="ffa7f-103">Explore the Project Structure</span></span>

<span data-ttu-id="ffa7f-104">Vi har skapat en ASP.NET-webbapp i Visual Studio, och nu behöver du redigera och anpassa din webbplats.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-104">We've created an ASP.NET web app in Visual Studio and now you will need to edit and customize your website.</span></span> <span data-ttu-id="ffa7f-105">Vi utforskar projektstrukturen för att se vad Visual Studio har skapat åt oss.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-105">Let's explore the project structure to see what Visual Studio has created for us.</span></span>

### <a name="dependencies"></a><span data-ttu-id="ffa7f-106">Beroenden</span><span class="sxs-lookup"><span data-stu-id="ffa7f-106">Dependencies</span></span>

<span data-ttu-id="ffa7f-107">Beroenden inkluderar ASP.NET-systemarkitekturen för att köra igång din app.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-107">Dependencies include the ASP.NET internals to get your app up and running.</span></span> <span data-ttu-id="ffa7f-108">Såvida du inte lägger till specifika tredjepartspaket behöver du inte tillbringa särskilt mycket tid i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-108">Unless you are adding specific third-party packages you won't need to spend much time in this folder.</span></span>

### <a name="properties"></a><span data-ttu-id="ffa7f-109">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="ffa7f-109">Properties</span></span>

<span data-ttu-id="ffa7f-110">Egenskapsmappen innehåller konfigurationsdata för det ställe som är värd för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-110">The properties folder contains configuration data for where you are hosting your web app.</span></span> <span data-ttu-id="ffa7f-111">Om du expanderar mappen **PublishProfiles** nu bör du se URL:en för Alpine Ski Hill-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-111">If you expand that the **PublishProfiles** folder now, you should see the url for the Alpine Ski Hill site.</span></span> <span data-ttu-id="ffa7f-112">Varje publiceringsprofil är en XML-fil som innehåller information om publiceringskonfiguration, till exempel den Azure-adress som Visual Studio använder för att ladda upp dina filer.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-112">Each publishing profile is an .xml file containing publishing configuration information such as the Azure address that Visual Studio uses to upload your files.</span></span>

### <a name="wwwroot"></a><span data-ttu-id="ffa7f-113">wwwroot</span><span class="sxs-lookup"><span data-stu-id="ffa7f-113">wwwroot</span></span>

<span data-ttu-id="ffa7f-114">Filen wwwroot innehåller alla dina statiska resurser för din webbplats, till exempel css, js, bilder och lib-filer.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-114">The wwwroot file contains all of your static assets for your site, such as the css, js, images and lib files.</span></span> <span data-ttu-id="ffa7f-115">När du är redo att välj stil och lägga till fler funktioner för webbplatsen är det här du arbetar.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-115">When you are ready to style and add more functionality to your site, you will work in here.</span></span>

### <a name="pages"></a><span data-ttu-id="ffa7f-116">Sidor</span><span class="sxs-lookup"><span data-stu-id="ffa7f-116">Pages</span></span>

<span data-ttu-id="ffa7f-117">Mappen **Pages** (Sidor) innehåller _**Razor**_-mallar för sidorna på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-117">The **Pages** folder includes _**Razor**_ templates for the pages of your site.</span></span>
<span data-ttu-id="ffa7f-118">Razor är en syntax som är uppbyggd kring HTML, med särskilda regler för att visa data och köra logik på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-118">Razor is a syntax that is built up around HTML which has special conventions to display data and execute logic on your site.</span></span>

<span data-ttu-id="ffa7f-119">Varje sida på webbplatsen representeras av två kodfiler:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-119">Each page in your site is represented with two code files:</span></span>

- <span data-ttu-id="ffa7f-120">Den första är en `.cshtml`-fil, som är i Razor-kodfilen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-120">The first is a `.cshtml` file which is the Razor markup file.</span></span> <span data-ttu-id="ffa7f-121">Den här filen innehåller HTML för visning och en del C#-logik.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-121">This file includes your display HTML and some C# logic.</span></span>

- <span data-ttu-id="ffa7f-122">Den andra filen är en `.cs`-fil, som är C#-code-behind som innehåller klassen `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-122">The second file is is a `.cs` file which is the C# code-behind which includes `PageModel` class.</span></span> <span data-ttu-id="ffa7f-123">Den här filen gör att du kan fånga upp HTTP-begäranden och utföra viss bearbetning i begärandena innan data skickas till Razor-filen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-123">This file allows you to intercept HTTP requests and do some processing on that request before passing off any data to the Razor file.</span></span>

### <a name="appsettingjson"></a><span data-ttu-id="ffa7f-124">appsetting.json</span><span class="sxs-lookup"><span data-stu-id="ffa7f-124">appsetting.json</span></span>

<span data-ttu-id="ffa7f-125">Det här är en konfigurationsfil för ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-125">This is a configuration file for ASP.NET.</span></span>

### <a name="bundleconfigjson"></a><span data-ttu-id="ffa7f-126">bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="ffa7f-126">bundleconfig.json</span></span>

<span data-ttu-id="ffa7f-127">Bundleconfig.json är förbearbetningskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-127">The bundleconfig.json is preprocessing configuration.</span></span> <span data-ttu-id="ffa7f-128">Den här filen miniatyriserar CSS och JS så att de blir mindre när du publicerar.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-128">This file is minifying your CSS and JS to be smaller when you publish.</span></span>

### <a name="programcs-and-startupcs"></a><span data-ttu-id="ffa7f-129">Program.cs och Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ffa7f-129">Program.cs and Startup.cs</span></span>

<span data-ttu-id="ffa7f-130">Program.cs och Startup.cs konfigurerar och startar webbvärden för din webbplats.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-130">Program.cs and Startup.cs configure and launch your web host for you site.</span></span>

## <a name="updating-your-website-using-razor"></a><span data-ttu-id="ffa7f-131">Uppdatera din webbplats med hjälp av Razor</span><span class="sxs-lookup"><span data-stu-id="ffa7f-131">Updating your website using Razor</span></span>

<span data-ttu-id="ffa7f-132">Vi vill göra några grundläggande ändringar på vår webbplats.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-132">We will want to make some basic changes to our website.</span></span> <span data-ttu-id="ffa7f-133">För att kunna göra det behöver du ha en grundläggande förståelse för hur du kan använda Razor-mallarna för att anpassa din webbapp.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-133">In order to do this, you will need to have a basic understanding of how leverage the Razor templates to customize your web app.</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="ffa7f-134">Vad är Razor?</span><span class="sxs-lookup"><span data-stu-id="ffa7f-134">What is Razor?</span></span>

<span data-ttu-id="ffa7f-135">Razor är en ASP.NET-syntax som används för att skapa dynamiska webbplatser med C#.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-135">Razor is an ASP.NET syntax used to create dynamic web pages with C#.</span></span> <span data-ttu-id="ffa7f-136">När en server läser en Razor-sida körs C#-koden innan den renderar HTML.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-136">When a server reads a Razor page the C# code is run before it renders the HTML.</span></span> <span data-ttu-id="ffa7f-137">På så sätt kan du snabbt skapa dynamiskt innehåll.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-137">This allows you to generate dynamic content quickly.</span></span>

<span data-ttu-id="ffa7f-138">Razor använder `@`-direktiv som talar om för ASP.NET hur det ska bearbeta en sida.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-138">Razor uses `@` directives to tell ASP.NET how to process a page.</span></span>

<span data-ttu-id="ffa7f-139">Titta till exempel på koden på `Contact.cshtml`-sidan.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-139">For example, take a look at the code in the `Contact.cshtml` page.</span></span>

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

<span data-ttu-id="ffa7f-140">Exempel: `@page`-direktivet instruerar ASP.NET att bearbeta den här filen som en Razor-sida.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-140">For example: The `@page` directive is telling the ASP.NET to process this file as a Razor page.</span></span>
<span data-ttu-id="ffa7f-141">`@model`-direktivet instruerar ASP.NET att koppla den här Razor-sidan till en C#-klass som heter `ContactModel`.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-141">The `@model` directive is telling ASP.NET to tie this Razor page with a C# class called `ContactModel`.</span></span>

<span data-ttu-id="ffa7f-142">Razor använder även symbolen `@` för att övergå mellan koden och HTML. Om du till exempel läser kodavsnittet ovan ser du `@{ ... }`.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-142">Razor also uses the `@` symbol to transition between code and HTML For example, if you look at the snippet above you'll notice `@{ ... }`.</span></span> <span data-ttu-id="ffa7f-143">Det här är ett **Razor-kodblock**, det vill säga den kod som _körs men inte renderas_.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-143">This is a **Razor code block**, it's code which is _executed but not rendered_.</span></span>

<span data-ttu-id="ffa7f-144">För att rendera utdata för en kodinstruktion använder vi `@` framför ett C#-uttryck.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-144">To render the output of a code statement we use the `@` in front of a C# expression.</span></span> <span data-ttu-id="ffa7f-145">Det finns två exempel på det i kodblocket ovan i taggarna `<h2>` och `<h3>`.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-145">We have two examples of that in the code block above in the `<h2>` and `<h3>` tags.</span></span>

## <a name="publish-your-updates"></a><span data-ttu-id="ffa7f-146">Publicera dina uppdateringar</span><span class="sxs-lookup"><span data-stu-id="ffa7f-146">Publish your updates</span></span>

<span data-ttu-id="ffa7f-147">När du har gjort ändringarna på webbplatsen är det dags att publicera dem till Azure.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-147">Once you've made the changes to your website you will want to publish them to Azure.</span></span> <span data-ttu-id="ffa7f-148">Den här processen liknar det sätt som vi först publicerade på.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-148">This process is similar to how we initially published.</span></span>

1. <span data-ttu-id="ffa7f-149">Högerklicka på projektet i Solution Explorer</span><span class="sxs-lookup"><span data-stu-id="ffa7f-149">Right click on the project in the Solution Explorer</span></span>
1. <span data-ttu-id="ffa7f-150">Nu bör du se namnet på din webbplats följt av Web Deploy. Om du till exempel gav webbplatsen namnet AlpineSkiHouse42 ser du **AlpineSkiHouse42 - Web Deploy** i de tillgängliga alternativen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-150">Now, you should see the name of your website followed by Web Deploy, for example if you named your website AlpineSkiHouse42, you would see **AlpineSkiHouse42 - Web Deploy** in the available options.</span></span> <span data-ttu-id="ffa7f-151">Välj det så uppdateras webbplatsen i Azure.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-151">Select that and your site will update in Azure.</span></span>

## <a name="summary"></a><span data-ttu-id="ffa7f-152">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ffa7f-152">Summary</span></span>

<span data-ttu-id="ffa7f-153">Att skapa och publicera en webbplats är bara de första stegen för att skapa en bra webbplats.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-153">Creating a publishing a website are just the first steps in creating a good website.</span></span> <span data-ttu-id="ffa7f-154">När du börjar lägga till innehåll kommer du behöva uppdatera webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-154">Once you start to add content you'll need to update your site.</span></span> <span data-ttu-id="ffa7f-155">När du väl har publicerat webbplatsen till Azure kan du uppdatera när som helst från Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-155">Once you've initially published your site to Azure, you can update at any time from the Solution Explorer.</span></span>
