<span data-ttu-id="9f605-101">Du har valt att använda en teknik för öppen källkod för att skapa ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9f605-101">You've decided to use an Open Source technology for building your web application.</span></span> <span data-ttu-id="9f605-102">Du vet att ASP.NET Core är ett plattformsoberoende ramverk med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="9f605-102">You know that ASP.NET Core is a cross-platform and Open Source framework.</span></span> <span data-ttu-id="9f605-103">Du bestämmer dig för att utveckla din webbapp i en Linux-miljö med hjälp av ASP.NET Core!</span><span class="sxs-lookup"><span data-stu-id="9f605-103">You decide to develop your Web app in a Linux environment using ASP.NET Core!</span></span>

<span data-ttu-id="9f605-104">Med Azure Web Apps kan du använda din favoritteknik för webben.</span><span class="sxs-lookup"><span data-stu-id="9f605-104">Azure Web Apps allows you to use your favorite web technology.</span></span> <span data-ttu-id="9f605-105">Oavsett om du känner dig mest bekväm med Node.js, PHP eller .NET Core, så fungerar Azure Web Apps lika bra.</span><span class="sxs-lookup"><span data-stu-id="9f605-105">Whether you're most comfortable with Node.js, PHP or .NET Core, Azure Web Apps will work for you.</span></span>

<span data-ttu-id="9f605-106">I det här avsnittet skapar du ett ASP.NET Core-program med hjälp av .NET-kommandoradsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="9f605-106">Here, you will create an ASP.NET Core application using the .NET Command-line interface.</span></span>

## <a name="what-is-aspnet-core"></a><span data-ttu-id="9f605-107">Vad är ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="9f605-107">What is ASP.NET Core?</span></span>

<span data-ttu-id="9f605-108">ASP.NET Core är den senaste utvecklingen av Microsofts populära ASP.NET-webbramverk som är ett plattformsoberoende ramverk med öppen källkod för utveckling av moderna, molnbaserade och Internetanslutna program.</span><span class="sxs-lookup"><span data-stu-id="9f605-108">ASP.NET Core is the latest evolution of Microsoft's popular ASP.NET web framework that is cross-platform, open-source framework for building modern, cloud-based and Internet connected applications.</span></span>

<span data-ttu-id="9f605-109">ASP.NET Core-program kan skrivas mot .NET Core Framework eller det befintliga, fullständiga .NET Framework-ramverket.</span><span class="sxs-lookup"><span data-stu-id="9f605-109">ASP.NET Core applications can be written to target the .NET Core Framework or the existing full .NET Framework.</span></span>

<span data-ttu-id="9f605-110">Eftersom ASP.NET Core är ett plattformsoberoende ramverk med öppen källkod kan du skapa ASP.NET Core-appar på en rad olika plattformar, inklusive Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="9f605-110">Being a cross-platform and open-source framework, you can build ASP.NET Core apps on a variety of platforms including Windows, macOS and Linux.</span></span> <span data-ttu-id="9f605-111">Visual Studio IDE är tillgängligt för både Windows- och macOS-miljöer.</span><span class="sxs-lookup"><span data-stu-id="9f605-111">Thus far, Microsoft offers the Visual Studio IDE for both Windows and macOS environments.</span></span> <span data-ttu-id="9f605-112">Och Visual Studio Code-redigeraren är plattformsoberoende och kompatibel med dem.</span><span class="sxs-lookup"><span data-stu-id="9f605-112">In addition, the Visual Studio Code editor is cross-platform and compatible with them as well.</span></span>

><span data-ttu-id="9f605-113">För att ge stöd för utveckling av ASP.NET Core-program på olika plattformar introducerade Microsoft .NET Core kommandoradsgränssnittsverktygen (CLI) som hjälper dig att bygga, testa och publicera dina program med hjälp av funktionsrika, enhetliga och plattformsoberoende API:er.</span><span class="sxs-lookup"><span data-stu-id="9f605-113">To support building ASP.NET Core applications on different platforms, Microsoft introduced the .NET Core Command-line Interface (CLI) tools to help you build, test and publish your applications using a rich, consistent and cross-platform set of APIs.</span></span>

<span data-ttu-id="9f605-114">Med ASP.NET Core kan du skapa webbappar och tjänster, IoT-appar och serverdelar för mobilappar.</span><span class="sxs-lookup"><span data-stu-id="9f605-114">With ASP.NET Core you can build Web Apps and services, IoT Apps and mobile back-ends.</span></span> <span data-ttu-id="9f605-115">ASP.NET Core-program kan köras antingen i molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="9f605-115">ASP.NET Core applications can be hosted either on the Cloud or on-premises.</span></span>

<span data-ttu-id="9f605-116">ASP.NET Core består av en inbäddad webbserver och en körningsmiljö som kör programkoden.</span><span class="sxs-lookup"><span data-stu-id="9f605-116">By design, ASP.NET Core consists of an embedded web server and a runtime environment that runs the application code.</span></span> <span data-ttu-id="9f605-117">Programkoden skrivs med hjälp av ett omarbetat ASP.NET MVC-ramverk, som kräver mindre moduler och paket.</span><span class="sxs-lookup"><span data-stu-id="9f605-117">The application code is written using a reworked ASP.NET MVC framework, that relies on smaller modules and packages.</span></span> <span data-ttu-id="9f605-118">Resultatet är en mindre ”skiss”, eller blueprint, av webbprogrammet, som enkelt kan underhållas och hanterar i molnmiljöer.</span><span class="sxs-lookup"><span data-stu-id="9f605-118">The result, is a smaller Web application blueprint, that is easy to maintain and host over cloud environments.</span></span>

![ASP.NET Core Architecture](../media-draft/4-asp-net-core-architecture.PNG)

<span data-ttu-id="9f605-120">ASP.NET Core-program är fristående **konsolprogram** som anropas via **dotnet**-drivrutinsverktyget.</span><span class="sxs-lookup"><span data-stu-id="9f605-120">ASP.NET Core applications are standalone **Console** applications invoked through the **dotnet** driver tool.</span></span> <span data-ttu-id="9f605-121">ASP.NET Core-program läses inte in i IIS-arbetsprocessen, utan via en inbyggd IIS-modul med namnet **AspNetCoreModule** som kör det externa konsolprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9f605-121">ASP.NET Core applications are not loaded into IIS worker process, but rather loaded through a native IIS module called **AspNetCoreModule** that executes the external Console application.</span></span>

## <a name="how-to-create-an-aspnet-core-web-project"></a><span data-ttu-id="9f605-122">Skapa ett ASP.NET Core-webbprojekt</span><span class="sxs-lookup"><span data-stu-id="9f605-122">How to create an ASP.NET Core Web Project</span></span>

<span data-ttu-id="9f605-123">Det kan välja mellan några olika alternativ när du ska skapa ett nytt ASP.NET Core-projekt:</span><span class="sxs-lookup"><span data-stu-id="9f605-123">There are a few options to create a new ASP.NET Core project:</span></span>

- <span data-ttu-id="9f605-124">Du kan använda Visual Studio-mallar (Windows- och Mac OS-versioner) för att generera ett nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="9f605-124">You can use Visual Studio (Windows and macOS versions) templates to generate a new project.</span></span> <span data-ttu-id="9f605-125">En mängd olika mallar är tillgängliga, som du kan använda för att skapa webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="9f605-125">It offers a variety of templates that you can use to create Web projects.</span></span> <span data-ttu-id="9f605-126">Du kan exempelvis använda mallen **Tom** för att skapa ett tomt ASP.NET Core-projekt med baskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="9f605-126">For instance, you can use the **Empty** template to create a bare-bone ASP.NET Core project with the basic setup.</span></span> <span data-ttu-id="9f605-127">Du kan också använda mallen **Webbprogram (Modal-View-Controller)** för att generera ett fullständigt ASP.NET Core MVC-program med **exempelkontroller** och **exempelvyer**som hjälper dig att börja koda ditt program.</span><span class="sxs-lookup"><span data-stu-id="9f605-127">In addition, you can use the **Web Application (Modal-View-Controller)** template to generate a full-fledge ASP.NET Core MVC application with sample **Controllers** and **Views** that can help you start coding your application.</span></span> <span data-ttu-id="9f605-128">Det senaste tillskottet är projektmallen **Webbprogram** som används för att skapa ett ASP.NET Core-projekt baserat på Razor-sidor i stället för den traditionella MVC-projektstrukturen.</span><span class="sxs-lookup"><span data-stu-id="9f605-128">The latest arrival is the **Web Application** project template that is used to create an ASP.NET Core project based on Razor pages and not the traditional MVC project structure.</span></span>

- <span data-ttu-id="9f605-129">Du kan använda kommandoradsgränssnittsverktygen (CLI) för .NET Core för att generera ett nytt ASP.NET Core-projekt.</span><span class="sxs-lookup"><span data-stu-id="9f605-129">You can use the .NET Core Command-line Interface (CLI) tools to generate a new ASP.NET Core project.</span></span> <span data-ttu-id="9f605-130">Uppsättningen ASP.NET Core-projektmallar är i princip densamma för både Visual Studio och CLI-verktygen.</span><span class="sxs-lookup"><span data-stu-id="9f605-130">Microsoft almost maintains a common set of ASP.NET Core project templates for both Visual Studio and the CLI tools.</span></span> <span data-ttu-id="9f605-131">Den enda skillnaden med CLI-verktygen är att du måste ange kommandon för att skapa ett nytt ASP.NET Core-projekt.</span><span class="sxs-lookup"><span data-stu-id="9f605-131">The only difference with the CLI tools is that you need to type commands to create a new ASP.NET Core project.</span></span>
> <span data-ttu-id="9f605-132">.NET CLI-verktygen använder **mallmotorn** för att skapa stöd för olika projektmallar. Gå till den här GitHub-lagringsplatsen om du vill veta mer om hur [mallmotorn](https://github.com/dotnet/templating) används internt av .NET-kommandoradsgränssnittsverktygen.</span><span class="sxs-lookup"><span data-stu-id="9f605-132">The .NET CLI tools make use of the **Templating Engine** to support different project templates.-  Visit this GitHub repository if you want to learn more about [Templating Engine](https://github.com/dotnet/templating) used internally by the .NET CLI tools.</span></span>

<span data-ttu-id="9f605-133">Verktygen ovan är de viktigaste verktygen för att skapa ASP.NET Core-projekt.</span><span class="sxs-lookup"><span data-stu-id="9f605-133">The above are considered the top tools to generate ASP.NET Core projects.</span></span> <span data-ttu-id="9f605-134">Men fler verktyg är tillgängliga, som du kan söka efter och utforska.</span><span class="sxs-lookup"><span data-stu-id="9f605-134">However, there are more tools out there that you can search and explore.</span></span>

<span data-ttu-id="9f605-135">Det är värt att nämna att projekten som genereras av de olika verktygen kan skilja sig åt en aning, men alla skapar giltiga och optimerade ASP.NET Core-projekt.</span><span class="sxs-lookup"><span data-stu-id="9f605-135">It's worth mentioning that the projects generated by the different tools can be slightly different, however, they all generate valid and optimized ASP.NET Core projects.</span></span>

## <a name="net-command-line-interface-cli-tools"></a><span data-ttu-id="9f605-136">.NET-kommandoradsgränssnittsverktyg (CLI)</span><span class="sxs-lookup"><span data-stu-id="9f605-136">.NET Command-line Interface (CLI) tools</span></span>

<span data-ttu-id="9f605-137">Kommandoradsgränssnittsverktyget för .NET, även kallat .NET Core CLI, är ett plattformsoberoende verktyg som tillhandahåller kommandon för att skapa, återställa paket, utveckla, köra och publicera .NET-program från kommandoraden utan behov av en fullständig IDE-miljö.</span><span class="sxs-lookup"><span data-stu-id="9f605-137">The .NET Command-line Interface tools, also known as .NET Core CLI, is a cross-platform tool that provides commands to create, restore packages, building, running and publishing .NET applications from the command line without the need for a full featured IDE.</span></span>

<span data-ttu-id="9f605-138">.NET CLI installeras som en del av .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="9f605-138">The .NET CLI is installed as part of the .NET Core SDK.</span></span> <span data-ttu-id="9f605-139">Flera versioner av CLI kan finnas samtidigt på samma dator och köras parallellt.</span><span class="sxs-lookup"><span data-stu-id="9f605-139">Multiple versions of the CLI can co-exist on the same machine and run side by side.</span></span>

<span data-ttu-id="9f605-140">Innan du kan börja använda .NET CLI måste du installera relevant .NET Core SDK baserat på den miljö som du använder för att utveckla din app: Windows, macOS eller Linux.</span><span class="sxs-lookup"><span data-stu-id="9f605-140">To start using the .NET CLI you need to install the relevant .NET Core SDK with respect to the environment that you are using to develop your app: Windows, macOS and Linux.</span></span> <span data-ttu-id="9f605-141">Gå till sidan för [.NET-nedladdningar](https://www.microsoft.com/net/download?initial-os=linux) och ladda ned .NET Core SDK för Linux.</span><span class="sxs-lookup"><span data-stu-id="9f605-141">Go to [.NET Downloads](https://www.microsoft.com/net/download?initial-os=linux) and grab the .NET Core SDK for Linux.</span></span> <span data-ttu-id="9f605-142">I den här demonstrationen av kommandona i .NET CLI använder jag Ubuntu 18.04 i Windows 10 med VMware Workspace Player 14.</span><span class="sxs-lookup"><span data-stu-id="9f605-142">I am using an Ubuntu 18.04 OS, running on top Windows 10 using VMware Workspace Player 14, to demonstrate the different commands offered by .NET CLI.</span></span>

<span data-ttu-id="9f605-143">Öppna kommandotolken och skriv följande:</span><span class="sxs-lookup"><span data-stu-id="9f605-143">Open the command line and type the following:</span></span>

```console
dotnet --version
```

<span data-ttu-id="9f605-144">Det här kommandot verifierar .NET CLI och visar den version av .NET-CLI som är installerad.</span><span class="sxs-lookup"><span data-stu-id="9f605-144">This command verifies that the .NET CLI and displays the version of the .NET CLI installed.</span></span>

<span data-ttu-id="9f605-145">När jag kör kommandot ovan på min dator returneras följande: `2.1.302`.</span><span class="sxs-lookup"><span data-stu-id="9f605-145">Running the above command on my machine displays this: `2.1.302`.</span></span>

<span data-ttu-id="9f605-146">Låt oss börja med att utforska några av de populära kommandona av .NET-CLI.</span><span class="sxs-lookup"><span data-stu-id="9f605-146">Let us start exploring some of the popular commands of the .NET CLI.</span></span>

<span data-ttu-id="9f605-147">Kommandot *dotnet* har följande allmänna syntax:</span><span class="sxs-lookup"><span data-stu-id="9f605-147">The *dotnet* command has the following general syntax:</span></span>

```console
dotnet [verb] [arguments]
```

<span data-ttu-id="9f605-148">Verbet som representerar åtgärden som ska köra.</span><span class="sxs-lookup"><span data-stu-id="9f605-148">The verb represents the action to execute.</span></span> <span data-ttu-id="9f605-149">Argumenten representerar listan med indataargument som verbet kräver för att kunna köras.</span><span class="sxs-lookup"><span data-stu-id="9f605-149">The arguments represent the list of input arguments that the verb requires in order to execute.</span></span>

<span data-ttu-id="9f605-150">Om du vill ha hjälp med att använda *dotnet* och visa en lista över de tillgängliga *verben* och annan relaterad information, skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9f605-150">To get help on the *dotnet* usage and list all of the *verbs* available and other related information, type the following command:</span></span>

```console
dotnet --help
```

<span data-ttu-id="9f605-151">Det här kommandot visar följande:</span><span class="sxs-lookup"><span data-stu-id="9f605-151">This command displays the following:</span></span>

```console
.NET Command Line Tools (2.1.302)
Usage: dotnet [runtime-options] [path-to-application]
Usage: dotnet [sdk-options] [command] [arguments] [command-options]

path-to-application:
  The path to an application .dll file to execute.

SDK commands:
  new              Initialize .NET projects.
  restore          Restore dependencies specified in the .NET project.
  run              Compiles and immediately executes a .NET project.
  build            Builds a .NET project.
  publish          Publishes a .NET project for deployment (including the runtime).
  test             Runs unit tests using the test runner specified in the project.

...
```

<span data-ttu-id="9f605-152">Under **SDK commands** kan du se hela listan med kommandon som du kan köra mot .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="9f605-152">Under the **SDK commands** you can see the entire list of commands that you can execute against the .NET Core SDK.</span></span>

<span data-ttu-id="9f605-153">De mest användbara kommandona som ofta används är följande:</span><span class="sxs-lookup"><span data-stu-id="9f605-153">The most useful commands at all times are the following:</span></span>

- <span data-ttu-id="9f605-154">**dotnet new**: Det här kommandot används för att bygga/generera ett nytt .NET-program.</span><span class="sxs-lookup"><span data-stu-id="9f605-154">**dotnet new**: This command is used to scaffold / generate a new .NET application.</span></span>

- <span data-ttu-id="9f605-155">**dotnet restore**: Det här kommandot används för att återställa/hämta alla paket som programmet refererar till.</span><span class="sxs-lookup"><span data-stu-id="9f605-155">**dotnet restore**: This command is used to restore / download all packages that are referenced by the application.</span></span>

- <span data-ttu-id="9f605-156">**dotnet run**: Det här kommandot används för att köra .NET-programmet.</span><span class="sxs-lookup"><span data-stu-id="9f605-156">**dotnet run**: This command is used to run your .NET application.</span></span>

<span data-ttu-id="9f605-157">Om du vill ha hjälp med att använda ett visst kommando kan du skriva följande:</span><span class="sxs-lookup"><span data-stu-id="9f605-157">Now to get assistance on how to use a specific command you can type the following:</span></span>

```console
dotnet run --help
```

<span data-ttu-id="9f605-158">Det här kommandot resulterar i:</span><span class="sxs-lookup"><span data-stu-id="9f605-158">This command results in:</span></span>

```console
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.
  -n, --name          The name for the output being created. If no name is specified, the name of the current directory is used.
  -o, --output        Location to place the generated output.
  -i, --install       Installs a source or a template pack.
  -u, --uninstall     Uninstalls a source or a template pack.
  --nuget-source      Specifies a NuGet source to use during install.
  --type              Filters templates based on available types. Predefined values are "project", "item" or "other".
  --force             Forces content to be generated even if it would change existing files.
  -lang, --language   Filters templates based on language and specifies the language of the template to create.


Templates                                         Short Name         Language          Tags
----------------------------------------------------------------------------------------------------------------------------
Console Application                               console            [C#], F#, VB      Common/Console
Class library                                     classlib           [C#], F#, VB      Common/Library
...

Razor Page                                        page               [C#]              Web/ASP.NET
MVC ViewImports                                   viewimports        [C#]              Web/ASP.NET
MVC ViewStart                                     viewstart          [C#]              Web/ASP.NET
ASP.NET Core Empty                                web                [C#], F#          Web/Empty
ASP.NET Core Web App (Model-View-Controller)      mvc                [C#], F#          Web/MVC
ASP.NET Core Web App                              razor              [C#]              Web/MVC/Razor Pages
ASP.NET Core with Angular                         angular            [C#]              Web/MVC/SPA
...

Solution File                                     sln                                  Solution

Examples:
    dotnet new mvc --auth Individual
    dotnet new webapi
    dotnet new --help
```

<span data-ttu-id="9f605-159">Det här kommandot visar de tillgängliga alternativen som du kan använda med kommandot `dotnet new`.</span><span class="sxs-lookup"><span data-stu-id="9f605-159">This command lists all the available options that you can use with the `dotnet new` command.</span></span> <span data-ttu-id="9f605-160">Det visar även alla tillgängliga projektmallar som du kan använda för att skapa ditt .NET-program.</span><span class="sxs-lookup"><span data-stu-id="9f605-160">Also, it lists all the available project templates that you can use to generate your next .NET application.</span></span> <span data-ttu-id="9f605-161">Slutligen finns ett avsnitt som visar exempel på hur du använder kommandot för att skapa ett nytt .NET-program.</span><span class="sxs-lookup"><span data-stu-id="9f605-161">Finally, a section showing examples on how to use the command to generate a new .NET application.</span></span>

<span data-ttu-id="9f605-162">Du kan lära dig mer om resten av kommandona genom att använda argumentet ”--help'' med valfritt kommando som är tillgängligt i .NET CLI.</span><span class="sxs-lookup"><span data-stu-id="9f605-162">You can learn the rest of the commands by using the \`--help\`\` argument for any command available in the .NET CLI.</span></span>

## <a name="aspnet-core-installation-on-linux-environment"></a><span data-ttu-id="9f605-163">ASP.NET Core-installation i Linux-miljö</span><span class="sxs-lookup"><span data-stu-id="9f605-163">ASP.NET Core Installation on Linux Environment</span></span>

<span data-ttu-id="9f605-164">Den senaste och bästa versionen av ASP.NET Core är v2.1.</span><span class="sxs-lookup"><span data-stu-id="9f605-164">The latest and greatest version of ASP.NET Core is the v2.1.</span></span> <span data-ttu-id="9f605-165">ASP.NET Core medföljer vanligtvis .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="9f605-165">In general, ASP.NET Core ships as part of the .NET Core SDK.</span></span> <span data-ttu-id="9f605-166">Genom att installera .NET Core SDK på din dator kan du därför börja utveckla med valfritt .NET Core-program, till exempel ASP.NET Core-webbappar.</span><span class="sxs-lookup"><span data-stu-id="9f605-166">Therefore, by installing the .NET Core SDK on your machine, you can start developing with any .NET Core application, including ASP.NET Core Web apps.</span></span>

<span data-ttu-id="9f605-167">I den här demonstrationen använder jag Ubuntu 18.04 i Windows 10 med VMware Workstation Player.</span><span class="sxs-lookup"><span data-stu-id="9f605-167">For this demonstration, I am using an Ubuntu 18.04 OS running on Windows 10 with the help of VMware Workstation Player.</span></span>

<span data-ttu-id="9f605-168">Gå till sidan för [.NET-nedladdningar](https://www.microsoft.com/net/download) för att ladda ned .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="9f605-168">To download the .NET Core SDK you need to visit the [.NET Downloads](https://www.microsoft.com/net/download) home page.</span></span> <span data-ttu-id="9f605-169">Sidan innehåller länkar för nedladdning av .NET Core SDK på olika plattformar: Windows, Linux eller macOS.</span><span class="sxs-lookup"><span data-stu-id="9f605-169">The page contains links to download the .NET Core SDK on different platforms: Windows, Linux and macOS.</span></span>

<span data-ttu-id="9f605-170">Leta upp och klicka på fliken **Linux**. Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="9f605-170">Locate and click on the **Linux** tab. You have two options:</span></span>

- <span data-ttu-id="9f605-171">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="9f605-171">.NET Core SDK</span></span>
- <span data-ttu-id="9f605-172">.NET Core Runtime</span><span class="sxs-lookup"><span data-stu-id="9f605-172">.NET Core Runtime</span></span>

<span data-ttu-id="9f605-173">.NET Core Runtime ingår redan i .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="9f605-173">The .NET Core Runtime is already contained inside the .NET Core SDK.</span></span> <span data-ttu-id="9f605-174">.NET Core Runtime används för att köra ett program.</span><span class="sxs-lookup"><span data-stu-id="9f605-174">The runtime is used to run an application.</span></span> <span data-ttu-id="9f605-175">Verktygen som du använder för att utveckla, testa, köra och publicera finns i .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="9f605-175">While all the tools to develop, test, run and publish are inside the .NET Core SDK.</span></span> <span data-ttu-id="9f605-176">Klicka därför på *.NET Core SDK*.</span><span class="sxs-lookup"><span data-stu-id="9f605-176">Therefore, you will click on *.NET Core SDK*.</span></span>

<span data-ttu-id="9f605-177">Du omdirigeras till nedladdningssidan.</span><span class="sxs-lookup"><span data-stu-id="9f605-177">The Microsoft website redirects you to the download page.</span></span> <span data-ttu-id="9f605-178">Där väljer du **Linux-distributionen**.</span><span class="sxs-lookup"><span data-stu-id="9f605-178">There, you need to select the **Linux Distribution**.</span></span> <span data-ttu-id="9f605-179">I mitt fall väljer jag **Ubuntu 18.04**.</span><span class="sxs-lookup"><span data-stu-id="9f605-179">In my case, I will select **Ubuntu 18.04**.</span></span> <span data-ttu-id="9f605-180">När du har valt distributionen visas instruktioner för hur du installerar .NET Core SDK i Ubuntu 18.04.</span><span class="sxs-lookup"><span data-stu-id="9f605-180">Automatically, upon selection, the instructions on how to install .NET Core SDK on Ubuntu 18.04 is displayed.</span></span>

## <a name="summary"></a><span data-ttu-id="9f605-181">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9f605-181">Summary</span></span>

<span data-ttu-id="9f605-182">När du har bestämt dig för att skapa ett webbprogram kan du välja mellan flera språk och ramverk.</span><span class="sxs-lookup"><span data-stu-id="9f605-182">When deciding to build a web application you have the choice between many languages and frameworks.</span></span> <span data-ttu-id="9f605-183">I Azure blir valet enklare eftersom du kan använda olika typer av program, till exempel Node.js, PHP eller .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f605-183">Azure tries to make this choice easier by allowing you to host different types of applications like: Node.js, PHP or .NET Core.</span></span> <span data-ttu-id="9f605-184">På så sätt kan du använda de språk och ramverk som du är mest bekväm med i stället för att behöva anpassa dig efter webbvärdens krav.</span><span class="sxs-lookup"><span data-stu-id="9f605-184">This allows you to use the languages and frameworks that you're most comfortable with instead of changing to meet the needs of your web host.</span></span>
