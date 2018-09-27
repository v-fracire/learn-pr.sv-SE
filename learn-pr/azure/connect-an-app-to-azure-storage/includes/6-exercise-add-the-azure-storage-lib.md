<span data-ttu-id="f6dac-101">::: zone pivot="csharp" Nu ska vi integrera Azure Storage-klientbiblioteket i ditt .NET Core-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="f6dac-101">::: zone pivot="csharp" Let's integrate the Azure Storage client library into your .NET Core console application.</span></span>

<span data-ttu-id="f6dac-102">Azure Storages klientbibliotek för .NET distribueras med NuGet.</span><span class="sxs-lookup"><span data-stu-id="f6dac-102">The Azure storage client library for .NET is distributed with NuGet.</span></span> <span data-ttu-id="f6dac-103">Du kan lägga till **WindowsAzure.Storage**-paketet till dina .NET- eller .NET Core-program.</span><span class="sxs-lookup"><span data-stu-id="f6dac-103">You will want to add the **WindowsAzure.Storage** package to your .NET or .NET Core applications.</span></span>

## <a name="add-the-azure-storage-nuget-package"></a><span data-ttu-id="f6dac-104">Lägga till NuGet-paketet Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f6dac-104">Add the Azure Storage NuGet package</span></span>

1. <span data-ttu-id="f6dac-105">I terminalen, `cd` till katalogen PhotoSharingApp om du inte redan är där.</span><span class="sxs-lookup"><span data-stu-id="f6dac-105">In the terminal, `cd` to the PhotoSharingApp directory if you aren't already there.</span></span>

1. <span data-ttu-id="f6dac-106">Lägg till **WindowsAzure.Storage**-paketet i programmet.</span><span class="sxs-lookup"><span data-stu-id="f6dac-106">Add the **WindowsAzure.Storage** package to the application.</span></span>

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. <span data-ttu-id="f6dac-107">Detta resulterar i viss konsolaktivitet medan klientbiblioteket och alla de nödvändiga beroendena laddas ned.</span><span class="sxs-lookup"><span data-stu-id="f6dac-107">This should result in some console activity while the client library and all the required dependencies are downloaded.</span></span> <span data-ttu-id="f6dac-108">När det är klart går du vidare och skapar och kör appen igen för att kontrollera att allt fungerar.</span><span class="sxs-lookup"><span data-stu-id="f6dac-108">Once it's done, go ahead and build and run the app again to make sure everything is ready to go.</span></span>

    ```bash
    dotnet run
    ```

1. <span data-ttu-id="f6dac-109">Som tidigare bör den visa ”Hello World!”.</span><span class="sxs-lookup"><span data-stu-id="f6dac-109">As before, it should output "Hello World!".</span></span>

<span data-ttu-id="f6dac-110">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="f6dac-110">::: zone-end</span></span>

<span data-ttu-id="f6dac-111">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="f6dac-111">::: zone pivot="javascript"</span></span>

<span data-ttu-id="f6dac-112">Nu ska vi integrera **Microsoft Azure Storage-klientbibliotek för Node.js och JavaScript** i ditt program.</span><span class="sxs-lookup"><span data-stu-id="f6dac-112">Let's integrate the **Microsoft Azure Storage Client Library for Node.js and JavaScript** into your application.</span></span>

<span data-ttu-id="f6dac-113">Klientbiblioteket för Node.js är tillgängligt via NPM (Node Package Manager).</span><span class="sxs-lookup"><span data-stu-id="f6dac-113">The client library for Node.js is available through the Node Package manager (NPM).</span></span> <span data-ttu-id="f6dac-114">Du kan lägga till paketet **azure-storage** i din **packages.json**-fil.</span><span class="sxs-lookup"><span data-stu-id="f6dac-114">You will want to add the **azure-storage** package to your **packages.json** file.</span></span>

> [!NOTE]
> <span data-ttu-id="f6dac-115">**Microsoft Azure Storage-klientbibliotek för Node.js och JavaScript** är avsett att användas i serverprogram.</span><span class="sxs-lookup"><span data-stu-id="f6dac-115">The **Microsoft Azure Storage Client Library for Node.js and JavaScript** is intended for server applications.</span></span> <span data-ttu-id="f6dac-116">Om du använder JavaScript för klientsidan kan du använda **Azure Storage-klientbibliotek för JavaScript** som fungerar på samma sätt, men som är skräddarsytt för att köras i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="f6dac-116">If you are doing client-side JavaScript, you will want to use the **Azure Storage Client Library for JavaScript**, which provides the same functionality but is tailored to running in a browser.</span></span>

## <a name="add-the-azure-storage-package"></a><span data-ttu-id="f6dac-117">Lägga till Azure Storage-paketet</span><span class="sxs-lookup"><span data-stu-id="f6dac-117">Add the Azure Storage package</span></span>

1. <span data-ttu-id="f6dac-118">I Cloud Shell, `cd` till katalogen PhotoSharingApp om du inte redan är där.</span><span class="sxs-lookup"><span data-stu-id="f6dac-118">In Cloud Shell, `cd` to the PhotoSharingApp directory if you aren't already there.</span></span>

1. <span data-ttu-id="f6dac-119">Lägg till paketet **azure-storage** i programmet.</span><span class="sxs-lookup"><span data-stu-id="f6dac-119">Add the **azure-storage** package to the application.</span></span> <span data-ttu-id="f6dac-120">Se till att ange `--save`-alternativet så att det finns kvar i **packages.json**.</span><span class="sxs-lookup"><span data-stu-id="f6dac-120">Make sure to supply the `--save` option so it persists to **packages.json**.</span></span>

    ```bash
    npm install azure-storage --save
    ```

1. <span data-ttu-id="f6dac-121">Detta resulterar i viss konsolaktivitet medan klientbiblioteket och alla de nödvändiga beroendena laddas ned.</span><span class="sxs-lookup"><span data-stu-id="f6dac-121">This should result in some console activity while the client library and all the required dependencies are downloaded.</span></span> <span data-ttu-id="f6dac-122">När det är klart går du vidare och skapar och kör appen igen för att kontrollera att allt fungerar.</span><span class="sxs-lookup"><span data-stu-id="f6dac-122">Once it's done, go ahead and build and run the app again to make sure everything is ready to go.</span></span>

    ```bash
    node index.js
    ```

1. <span data-ttu-id="f6dac-123">Som tidigare bör den visa ”Hello, World!”.</span><span class="sxs-lookup"><span data-stu-id="f6dac-123">As before, it should output "Hello, World!"</span></span>

<span data-ttu-id="f6dac-124">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="f6dac-124">::: zone-end</span></span>

<span data-ttu-id="f6dac-125">Nu när vi har nödvändiga bibliotek på plats kan vi titta på vanliga åtgärder som du kommer att utföra i din kod när du arbetar med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f6dac-125">Now that we have the necessary libraries in place, let's look at the common tasks you'll do in your code to work with Azure storage.</span></span>
