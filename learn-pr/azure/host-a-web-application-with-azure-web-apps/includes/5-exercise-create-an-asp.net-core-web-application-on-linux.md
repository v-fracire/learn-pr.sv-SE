<span data-ttu-id="0fd54-101">I den här övningen skapar du en ASP.NET Core-webbapp med hjälp av .NET CLI på en Ubuntu-dator.</span><span class="sxs-lookup"><span data-stu-id="0fd54-101">In this exercise, you will create an ASP.NET Core Web App, using the .NET CLI , on an Ubuntu machine.</span></span>

## <a name="aspnet-core-installation-on-linux-environment"></a><span data-ttu-id="0fd54-102">ASP.NET Core-installation i Linux-miljö</span><span class="sxs-lookup"><span data-stu-id="0fd54-102">ASP.NET Core Installation on Linux Environment</span></span>

<span data-ttu-id="0fd54-103">Besök Microsofts [nedladdningssida för .NET](https://www.microsoft.com/net/download) och följ samma steg som nämns på .NET Core SDK-sidan.</span><span class="sxs-lookup"><span data-stu-id="0fd54-103">Visit the Microsoft [.NET Downloads page](https://www.microsoft.com/net/download), and follow the same steps mentioned on the .NET Core SDK page.</span></span> <span data-ttu-id="0fd54-104">De finns nedan.</span><span class="sxs-lookup"><span data-stu-id="0fd54-104">They are below.</span></span> <span data-ttu-id="0fd54-105">För att köra kommandona måste du öppna en ny **Terminal**instans på ett sätt som liknar det här.</span><span class="sxs-lookup"><span data-stu-id="0fd54-105">In order to execute the commands, you need to open a new **Terminal** instance, something similar to this.</span></span>

![Terminalinstans](../media-draft/5-terminal-instance.PNG)

### <a name="register-microsoft-key-and-feed"></a><span data-ttu-id="0fd54-107">Registrera Microsoft-nyckel och feed</span><span class="sxs-lookup"><span data-stu-id="0fd54-107">Register Microsoft key and feed</span></span>

<span data-ttu-id="0fd54-108">Innan du installerar .NET behöver du registrera Microsoft-nyckeln, registrera produktlagringsplatsen och installera de nödvändiga beroendena.</span><span class="sxs-lookup"><span data-stu-id="0fd54-108">Before installing .NET, you'll need to register the Microsoft key, register the product repository and install the required dependencies.</span></span> <span data-ttu-id="0fd54-109">Detta behöver bara göras en gång per dator.</span><span class="sxs-lookup"><span data-stu-id="0fd54-109">This only needs to be done once per machine.</span></span>

```console
~$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
~$ sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-net-sdk"></a><span data-ttu-id="0fd54-110">Installera .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0fd54-110">Install .NET SDK</span></span>

<span data-ttu-id="0fd54-111">Uppdatera de produkter som är tillgängliga för installation och sedan installera .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="0fd54-111">Update the products available for installation, then install the .NET SDK.</span></span>

<span data-ttu-id="0fd54-112">I kommandotolken kör du följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0fd54-112">In your command prompt, run the following commands:</span></span>

```console
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.1
```

<span data-ttu-id="0fd54-113">Under installationen kan du bli ombedd att ange lösenordet för kontot.</span><span class="sxs-lookup"><span data-stu-id="0fd54-113">During the installation process, you might be asked to enter your account password.</span></span> <span data-ttu-id="0fd54-114">Ange lösenordet och tryck på Retur för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="0fd54-114">Simply, provide your password and hit Enter to continue.</span></span>

<span data-ttu-id="0fd54-115">För att verifiera installationen skriver du följande:</span><span class="sxs-lookup"><span data-stu-id="0fd54-115">To verify the installation, type the following:</span></span>

```console
dotnet --version
```

<span data-ttu-id="0fd54-116">Och de utdata som visas är:</span><span class="sxs-lookup"><span data-stu-id="0fd54-116">And the output displayed is:</span></span>

```console
2.1.302
```

<span data-ttu-id="0fd54-117">Nu när du har installerat .NET Core SDK har du även ASP.NET Core installerat.</span><span class="sxs-lookup"><span data-stu-id="0fd54-117">Now that you have the .NET Core SDK installed, you also have the ASP.NET Core installed.</span></span> <span data-ttu-id="0fd54-118">Nu går vi igenom hur du kan använda .NET CLI för att skapa ett nytt ASP.NET Core-projekt med hjälp av de kommandon vi just har lärt oss.</span><span class="sxs-lookup"><span data-stu-id="0fd54-118">Let's see together how you can make use of the .NET CLI to create a new ASP.NET Core project using the commands we just learned.</span></span>

## <a name="open-a-terminal-window"></a><span data-ttu-id="0fd54-119">Öppna ett terminalfönster</span><span class="sxs-lookup"><span data-stu-id="0fd54-119">Open a Terminal window</span></span>

<span data-ttu-id="0fd54-120">Först behöver du hitta och öppna terminalfönstret.</span><span class="sxs-lookup"><span data-stu-id="0fd54-120">First, you need to locate and open the Terminal window.</span></span> <span data-ttu-id="0fd54-121">I terminalfönstret kan du köra kommandon, och det har en roll som liknar Windows kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="0fd54-121">The Terminal window allows you to execute commands and has a similar role to the Windows Command Prompt.</span></span>

## <a name="create-a-new-web-project"></a><span data-ttu-id="0fd54-122">Skapa ett nytt webbprojekt</span><span class="sxs-lookup"><span data-stu-id="0fd54-122">Create a new Web project</span></span>

<span data-ttu-id="0fd54-123">I hjärtat av .NET CLI-verktygen är *dotnet*-drivrutinsverktyget.</span><span class="sxs-lookup"><span data-stu-id="0fd54-123">In the heart of the .NET CLI tools is the *dotnet* driver tool.</span></span> <span data-ttu-id="0fd54-124">Med hjälp av det här kommandot skapar du ett nytt ASP.NET Core-webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="0fd54-124">Using this command you will create a new ASP.NET Core Web project.</span></span>

<span data-ttu-id="0fd54-125">För att skapa ett nytt ASP.NET Core MVC-program behöver du bara ange följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0fd54-125">To create a new ASP.NET Core MVC application, you only need to type the following commands:</span></span>

```console
cd ~/Documents
mkdir BestBikeApp   # Hit Enter
cd BestBikeApp      # Hit Enter
dotnet new mvc      # Hit Enter
```

<span data-ttu-id="0fd54-126">Kommandona ovan navigerar till rotmappen *Dokument* och skapar sedan skapar en ny katalog/mapp. Sedan går du in i den katalogen/mappen, och slutligen anger du .NET CLI-kommandona för att generera ett nytt ASP.NET MVC-program med alla de paket som har återställts:</span><span class="sxs-lookup"><span data-stu-id="0fd54-126">The above commands navigate to the *Documents* root folder, then creates a new directory / folder, then you go inside that directory / folder and finally you issue the .NET CLI command to generate a new ASP.NET MVC application with all the packages restored:</span></span>

```console
The template "ASP.NET Core Web App (Model-View-Controller)" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore-template-3pn-210 for details.

Processing post-creation actions...
Running 'dotnet restore' on /home/your-user/Documents/BestBikeApp/BestBikeApp.csproj...
...
```

<span data-ttu-id="0fd54-127">Allt du behöver göra nu är att köra appen genom att ange det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="0fd54-127">All you have to do now is run the application by issuing this command:</span></span>

```console
dotnet run
```

<span data-ttu-id="0fd54-128">På *terminalen* visar *dotnet*-kommandot användbar information för den app som körs:</span><span class="sxs-lookup"><span data-stu-id="0fd54-128">On the *Terminal*, the *dotnet* command displays some useful information for the running app:</span></span>

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Development
Content root path: /home/your-user/Documents/BestBikeApp
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="0fd54-129">Utdata beskriver situationen efter att du har startat appen; programmet körs och lyssnar på port 5001 och 5002 (via HTTPS).</span><span class="sxs-lookup"><span data-stu-id="0fd54-129">The output describes the situation after starting your app; the application is running and listening at port 5001 and 5002 (via HTTPS).</span></span>

<span data-ttu-id="0fd54-130">För att kunna köra HTTPS lokalt på datorn, i utvecklingsläge, behöver du skapa och lita på ett **självsignerat certifikat**.</span><span class="sxs-lookup"><span data-stu-id="0fd54-130">In order to run HTTPs locally on the machine, development mode, you need to create and trust a **Self-Signed Certificate**.</span></span>

## <a name="create-a-self-signed-certificate"></a><span data-ttu-id="0fd54-131">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="0fd54-131">Create a self-signed certificate</span></span>

<span data-ttu-id="0fd54-132">Kör kommandot nedan för att generera ett självsignerat certifikat för utveckling.</span><span class="sxs-lookup"><span data-stu-id="0fd54-132">Run the command below to generate a development self-signed certificate.</span></span>

```console
dotnet dev-certs https -v
```

<span data-ttu-id="0fd54-133">När kommandot körs returneras följande:</span><span class="sxs-lookup"><span data-stu-id="0fd54-133">Running the command returns the following:</span></span>

```console
The HTTPS developer certificate was generated successfully.
```

## <a name="run-the-application"></a><span data-ttu-id="0fd54-134">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="0fd54-134">Run the application</span></span>

<span data-ttu-id="0fd54-135">För den här demonstrationen använder jag webbläsaren Firefox.</span><span class="sxs-lookup"><span data-stu-id="0fd54-135">For this demonstration, I am using Firefox browser.</span></span>

<span data-ttu-id="0fd54-136">Öppna webbläsaren och ange adressen: `http://localhost:5001` för att gå till programmet.</span><span class="sxs-lookup"><span data-stu-id="0fd54-136">Open the browser and type in the address: `http://localhost:5001` to go to the application.</span></span>

![Standardmall för ASP.NET Core MVC](../media-draft/5-asp-core-mvc-default-template.PNG)

> <span data-ttu-id="0fd54-138">Du behöver **Lägga till ett undantag** för programmets URL eftersom det självsignerade certifikatet för utveckling inte kunde verifieras av Firefox.</span><span class="sxs-lookup"><span data-stu-id="0fd54-138">You need to **Add Exception** for the application's URL because the development self-signed certificate couldn't be verified by Firefox.</span></span>

<span data-ttu-id="0fd54-139">Webbappen är igång!</span><span class="sxs-lookup"><span data-stu-id="0fd54-139">The Web App is up and running successfully!</span></span>
