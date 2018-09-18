<span data-ttu-id="5d773-101">Nu ska vi skapa ett klientprogram som ska arbeta med en kö.</span><span class="sxs-lookup"><span data-stu-id="5d773-101">Let's create a client application to work with a queue.</span></span> <span data-ttu-id="5d773-102">Sedan ska vi lägga till vår anslutningssträng i koden.</span><span class="sxs-lookup"><span data-stu-id="5d773-102">Then we'll add our connection string to the code.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="5d773-103">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="5d773-103">Create the application</span></span>

<span data-ttu-id="5d773-104">Vi skapar nu ett .NET Core-program som du kan köra i Linux, macOS eller Windows.</span><span class="sxs-lookup"><span data-stu-id="5d773-104">We'll create a .NET Core application that you can run on Linux, macOS, or Windows.</span></span> <span data-ttu-id="5d773-105">Vi ger det namnet **QueueApp**.</span><span class="sxs-lookup"><span data-stu-id="5d773-105">Let's name it **QueueApp**.</span></span> <span data-ttu-id="5d773-106">Vi gör det enkelt för oss genom att använda en enda app för att både skicka och ta emot meddelanden till och från kön.</span><span class="sxs-lookup"><span data-stu-id="5d773-106">For simplicity, we'll use a single app that will both send and receive messages through our queue.</span></span>

1. <span data-ttu-id="5d773-107">Använd kommandot `dotnet new` till att skapa en ny konsolapp med namnet **QueueApp**.</span><span class="sxs-lookup"><span data-stu-id="5d773-107">Use the `dotnet new` command to create a new console app with the name **QueueApp**.</span></span> <span data-ttu-id="5d773-108">Du kan skriva kommandon i Cloud Shell till höger. Om du arbetar lokalt kan du skriva dem i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="5d773-108">You can type commands into the Cloud Shell on the right, or if you are working locally, in a terminal/console window.</span></span> <span data-ttu-id="5d773-109">Nu har du skapat en enkel app med en enda källfil: `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="5d773-109">This creates a simple app with a single source file: `Program.cs`.</span></span>

    ```azurecli
    dotnet new console -n QueueApp
    ```

1. <span data-ttu-id="5d773-110">Växla till den nya mappen `QueueApp` och kompilera appen så att du ser att allting fungerar.</span><span class="sxs-lookup"><span data-stu-id="5d773-110">Switch to the newly created `QueueApp` folder and build the app to verify that all is well.</span></span>

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a><span data-ttu-id="5d773-111">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="5d773-111">Get your connection string</span></span>

<span data-ttu-id="5d773-112">Du kan se din anslutningssträng i avsnittet **Inställningar -> Åtkomstnycklar** för lagringskontot i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5d773-112">Recall that your connection string is available in the Azure portal **Settings > Access keys** section of your storage account.</span></span>

<span data-ttu-id="5d773-113">Du kan också hämta den via verktyg för Azure CLI eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5d773-113">You can also retrieve it through the Azure CLI or PowerShell tools.</span></span> <span data-ttu-id="5d773-114">Nu ska vi använda Azure CLI-kommandot.</span><span class="sxs-lookup"><span data-stu-id="5d773-114">Let's use the Azure CLI command.</span></span> <span data-ttu-id="5d773-115">Kom ihåg att byta ut `<name>` mot namnet på lagringskontot du skapade.</span><span class="sxs-lookup"><span data-stu-id="5d773-115">Remember to replace the `<name>` with the specific name of the storage account you created.</span></span> <span data-ttu-id="5d773-116">Du kan använda `az storage account list` om du behöver en påminnelse.</span><span class="sxs-lookup"><span data-stu-id="5d773-116">You can use `az storage account list` if you need a reminder.</span></span>

```azurecli
az storage account show-connection-string --name <name> --resource-group ExerciseResources
```

<span data-ttu-id="5d773-117">Det här kommandot returnerar ett JSON-block med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="5d773-117">This command will return a JSON block with your connection string.</span></span> <span data-ttu-id="5d773-118">Du ser namnet på lagringskontot och åtkomstnyckeln:</span><span class="sxs-lookup"><span data-stu-id="5d773-118">It will include the storage account name and the account key:</span></span>

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a><span data-ttu-id="5d773-119">Lägg till anslutningssträngen i programmet</span><span class="sxs-lookup"><span data-stu-id="5d773-119">Add the connection string to the application</span></span>

<span data-ttu-id="5d773-120">Slutligen ska vi lägga till anslutningssträngen i vår app så att den kan komma åt lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="5d773-120">Finally, let's add the connection string into our app so it can access the storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="5d773-121">Vi gör det enkelt för oss och placerar anslutningssträngen i filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="5d773-121">For simplicity, you will place the connection string in the **Program.cs** file.</span></span> <span data-ttu-id="5d773-122">I ett produktionsprogram ska du lagra den på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="5d773-122">In a production application, you should store it in a secure location.</span></span> <span data-ttu-id="5d773-123">För användning på serversidan rekommenderar vi att du använder Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5d773-123">For server side use, we recommend using Azure Key Vault.</span></span>

1. <span data-ttu-id="5d773-124">Skriv `code .` i terminalen för att öppna onlinekodredigeraren.</span><span class="sxs-lookup"><span data-stu-id="5d773-124">Type `code .` in the terminal to open the online code editor.</span></span> <span data-ttu-id="5d773-125">Om du arbetar lokalt kan du använda valfri IDE.</span><span class="sxs-lookup"><span data-stu-id="5d773-125">Alternatively, if you are working on your own you can use the IDE of your choice.</span></span> <span data-ttu-id="5d773-126">Vi rekommenderar Visual Studio Code, som är en utmärkt plattformsoberoende IDE.</span><span class="sxs-lookup"><span data-stu-id="5d773-126">We recommend Visual Studio Code, which is an excellent cross-platform IDE.</span></span>

1. <span data-ttu-id="5d773-127">Öppna källfilen `Program.cs` i projektet.</span><span class="sxs-lookup"><span data-stu-id="5d773-127">Open the `Program.cs` source file in the project.</span></span>

1. <span data-ttu-id="5d773-128">Gå till klassen `Program` och lägg till ett värde av typen `const string` för anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="5d773-128">In the `Program` class, add a `const string` value to hold the connection string.</span></span> <span data-ttu-id="5d773-129">Du behöver bara värdet (det börjar med texten **DefaultEndpointsProtocol**).</span><span class="sxs-lookup"><span data-stu-id="5d773-129">You only need the value (it starts with the text **DefaultEndpointsProtocol**).</span></span>

1. <span data-ttu-id="5d773-130">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="5d773-130">Save the file.</span></span> <span data-ttu-id="5d773-131">Du kan klicka på ellipsen ”...” i molnredigerarens högra hörn, då ser du även kortkommandot.</span><span class="sxs-lookup"><span data-stu-id="5d773-131">You can click the ellipse "..." in the right corner of the cloud editor, it will also show you the keyboard shortcut.</span></span>

<span data-ttu-id="5d773-132">Koden bör se ut ungefär så här (strängvärdet är unikt för ditt konto).</span><span class="sxs-lookup"><span data-stu-id="5d773-132">Your code should look something like this (the string value will be unique to your account).</span></span>

```csharp
...
namespace QueueApp
{
    class Program
    {
        private const string ConnectionString = "DefaultEndpointsProtocol=https; ...";
        
        ...
    }
}
```

<span data-ttu-id="5d773-133">Nu när det här enkla projektet är konfigurerat ska vi titta på hur du arbetar med köer i koden.</span><span class="sxs-lookup"><span data-stu-id="5d773-133">Now that we have this starter project setup, let's look at how to work with a queue in code.</span></span> <span data-ttu-id="5d773-134">Det handlar om _meddelanden_.</span><span class="sxs-lookup"><span data-stu-id="5d773-134">It all starts with _messages_.</span></span>