<span data-ttu-id="afaa6-101">Du kommer kanske ihåg att vi arbetar med ett bilddelningsprogram som använder Azure Storage till att hantera bilder och andra data som vi lagrar åt våra användare.</span><span class="sxs-lookup"><span data-stu-id="afaa6-101">Recall that we are working on a photo-sharing application that will use Azure Storage to manage pictures and other bits of data we store on behalf of our users.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="afaa6-102">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="afaa6-102">::: zone pivot="csharp"</span></span>

<span data-ttu-id="afaa6-103">För att förenkla vårt scenario så att vi kan fokusera på Storage-API:erna, skapar vi ett nytt .NET Core-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="afaa6-103">To simplify our scenario so that we can focus on the Storage APIs, we will create a new .NET Core Console application.</span></span> <span data-ttu-id="afaa6-104">Vi förutsätter också det alltid är anslutet till nätverket.</span><span class="sxs-lookup"><span data-stu-id="afaa6-104">We will also assume it always has network connectivity.</span></span> <span data-ttu-id="afaa6-105">Du bör dock alltid förstärka din app för att säkerställa att nätverksfel inte påverkar användarupplevelsen, eller leder till fel i programmet.</span><span class="sxs-lookup"><span data-stu-id="afaa6-105">However, you should always harden your app to ensure network failures will not impact the user experience or result in a failure of the application itself.</span></span>

## <a name="create-a-net-core-application"></a><span data-ttu-id="afaa6-106">Skapa ett .NET Core-program</span><span class="sxs-lookup"><span data-stu-id="afaa6-106">Create a .NET Core application</span></span>

<span data-ttu-id="afaa6-107">.NET Core är en plattformsoberoende version av .NET som körs på macOS, Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="afaa6-107">.NET Core is a cross-platform version of .NET that runs on macOS, Windows, and Linux.</span></span> <span data-ttu-id="afaa6-108">Du kan installera verktyg lokalt eller använda Cloud Shell på höger sida i fönstret för att köra stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="afaa6-108">You can install the tools locally or use the Cloud Shell on the right side of the window to execute the below steps below.</span></span>

1. <span data-ttu-id="afaa6-109">Logga in på Cloud Shell eller öppna en kommandoradssession och skapa ett nytt .NET Core-konsolprogram med namnet ”PhotoSharingApp”.</span><span class="sxs-lookup"><span data-stu-id="afaa6-109">Sign into the Cloud Shell or open a command-line session and create a new .NET Core Console application with the name "PhotoSharingApp".</span></span> <span data-ttu-id="afaa6-110">Du kan lägga till `-o`- eller `--output`-flaggan för att skapa appen i en viss mapp.</span><span class="sxs-lookup"><span data-stu-id="afaa6-110">You can add the `-o` or `--output` flag to create the app in a specific folder.</span></span>

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. <span data-ttu-id="afaa6-111">Kör appen för att kontrollera att den skapas och körs korrekt.</span><span class="sxs-lookup"><span data-stu-id="afaa6-111">Run the app to make sure it builds and executes correctly.</span></span> <span data-ttu-id="afaa6-112">Den bör visa ”Hello World!”</span><span class="sxs-lookup"><span data-stu-id="afaa6-112">It should display "Hello World!"</span></span> <span data-ttu-id="afaa6-113">i konsolen.</span><span class="sxs-lookup"><span data-stu-id="afaa6-113">to the console.</span></span>

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
<span data-ttu-id="afaa6-114">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="afaa6-114">::: zone-end</span></span>

<span data-ttu-id="afaa6-115">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="afaa6-115">::: zone pivot="javascript"</span></span>

<span data-ttu-id="afaa6-116">För att förenkla vårt scenario så att vi kan fokusera på Storage-API:erna, skapar vi ett nytt Node.js-program som kan köras från konsolen.</span><span class="sxs-lookup"><span data-stu-id="afaa6-116">To simplify our scenario so that we can focus on the Storage APIs, we will create a new Node.js application that can run from the console.</span></span> <span data-ttu-id="afaa6-117">Vi förutsätter också det alltid är anslutet till nätverket.</span><span class="sxs-lookup"><span data-stu-id="afaa6-117">We will also assume it always has network connectivity.</span></span> <span data-ttu-id="afaa6-118">Du bör dock alltid förstärka din app för att säkerställa att nätverksfel inte påverkar användarupplevelsen, eller leder till fel i programmet.</span><span class="sxs-lookup"><span data-stu-id="afaa6-118">However, you should always harden your app to ensure network failures will not impact the user experience, or result in a failure of the application itself.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="afaa6-119">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="afaa6-119">Create a Node.js application</span></span>

<span data-ttu-id="afaa6-120">Node.js är ett populärt ramverk för att köra JavaScript-appar.</span><span class="sxs-lookup"><span data-stu-id="afaa6-120">Node.js is a popular framework for running JavaScript apps.</span></span> <span data-ttu-id="afaa6-121">Det används oftast för webbappar, men du kan använda det till att köra logik från kommandoraden också.</span><span class="sxs-lookup"><span data-stu-id="afaa6-121">It is most commonly used for web apps, but you can use it to run logic from the command line as well.</span></span> <span data-ttu-id="afaa6-122">Om du har de verktyg som installerats lokalt kan du köra följande steg från en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="afaa6-122">If you have the tools installed locally, you can run the following steps from a command line.</span></span> <span data-ttu-id="afaa6-123">Du kan också använda Cloud Shell på höger sida i fönstret för att köra stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="afaa6-123">Alternatively, you can use the Cloud Shell on the right side of the window to execute the below steps.</span></span>

1. <span data-ttu-id="afaa6-124">Logga in på Cloud Shell eller öppna en kommandoradssession och skapa en ny mapp med namnet ”PhotoSharingApp”.</span><span class="sxs-lookup"><span data-stu-id="afaa6-124">Sign into the Cloud Shell or open a command-line session and create a new folder named "PhotoSharingApp".</span></span>

    ```bash
    mkdir PhotoSharingApp
    ```

1. <span data-ttu-id="afaa6-125">Ändra till den nya mappen och använd `npm` för att initiera en ny Node.js-app.</span><span class="sxs-lookup"><span data-stu-id="afaa6-125">Change into the new folder and use `npm` to initialize a new Node.js app.</span></span> <span data-ttu-id="afaa6-126">Detta skapar en **package.json**-fil som innehåller metadata som beskriver appen.</span><span class="sxs-lookup"><span data-stu-id="afaa6-126">This will create a **package.json** file containing metadata that describes the app.</span></span>

    ```bash
    cd PhotoSharingApp
    npm init -y
    ```

1. <span data-ttu-id="afaa6-127">Skapa en ny källfil med namnet **index.js** som kommer att vara den plats där vår kod hamnar.</span><span class="sxs-lookup"><span data-stu-id="afaa6-127">Create a new source file **index.js**, which is where our code will go.</span></span>

    ```bash
    touch index.js
    ```

1. <span data-ttu-id="afaa6-128">Öppna filen **index.js** med ett redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="afaa6-128">Open the **index.js** file with an editor.</span></span> <span data-ttu-id="afaa6-129">Om du använder Cloud Shell kan du skriva `code .` för att öppna ett redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="afaa6-129">If you are using the Cloud Shell, you can type `code .` to open an editor.</span></span>

1. <span data-ttu-id="afaa6-130">Klistra in följande program i filen **index.js**.</span><span class="sxs-lookup"><span data-stu-id="afaa6-130">Paste the following program into the **index.js** file.</span></span> <span data-ttu-id="afaa6-131">Du kan använda kortkommandot **Ctrl + V** eller högerklicka för att klistra in.</span><span class="sxs-lookup"><span data-stu-id="afaa6-131">You can use the **Ctrl+V** keyboard shortcut or right-click to paste.</span></span>

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. <span data-ttu-id="afaa6-132">Spara filen&mdash;. Du kan använda ”...”-menyn i det övre högra hörnet av Cloud Shell-redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="afaa6-132">Save the file&mdash;you can use the "..." menu on the top right corner of the Cloud Shell editor.</span></span>

1. <span data-ttu-id="afaa6-133">Kör appen för att kontrollera att den körs korrekt.</span><span class="sxs-lookup"><span data-stu-id="afaa6-133">Run the app to make sure it executes correctly.</span></span> <span data-ttu-id="afaa6-134">Den bör visa ”Hello, World!”</span><span class="sxs-lookup"><span data-stu-id="afaa6-134">It should display "Hello, World!"</span></span> <span data-ttu-id="afaa6-135">i konsolen.</span><span class="sxs-lookup"><span data-stu-id="afaa6-135">to the console.</span></span>

    ```bash
    node index.js
    ```

<span data-ttu-id="afaa6-136">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="afaa6-136">::: zone-end</span></span>