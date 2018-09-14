<span data-ttu-id="c7bd6-101">I den här enheten ska du hämta åtkomstnyckeln och lägga till den i appkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-101">In this unit you will retrieve the access key and add it to your app configuration.</span></span> <span data-ttu-id="c7bd6-102">Eftersom vi har skapat ett .NET Core-konsolprogram måste vi också lägga till stöd för läsning från konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-102">In addition, since we created a .NET Core console application, we need to add support for reading from configuration files to the application.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="c7bd6-103">Skapa en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="c7bd6-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="c7bd6-104">I Visual Studio-projektet, högerklicka på projektet i den **Solution Explorer** och klicka på **Lägg till** > **nytt objekt...** .</span><span class="sxs-lookup"><span data-stu-id="c7bd6-104">In your Visual Studio project, right click the project in the **Solution Explorer** and click **Add** > **New Item...**.</span></span>

1. <span data-ttu-id="c7bd6-105">I den **Lägg till nytt objekt** dialogrutan den **Web** > **JavaScript JSON-konfigurationsfil** mall.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-105">In the **Add New Item** dialog, select the **Web** > **JavaScript JSON Configuration File** template.</span></span>
1. <span data-ttu-id="c7bd6-106">Ge den nya filen en **namn** av `appsettings.json` och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-106">Give the new file a **Name** of `appsettings.json` and click **Add**.</span></span>

1. <span data-ttu-id="c7bd6-107">En fil läggs till i projektet med ett innehåll som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="c7bd6-107">A file is added to the project with contents similar to the following:</span></span>

    ```json
    {
      "exclude": [
        "**/bin",
        "**/bower_components",
        "**/jspm_packages",
        "**/node_modules",
        "**/obj",
        "**/platforms"
      ]
    }
    ```

1. <span data-ttu-id="c7bd6-108">Ta bort posten **exclude** (undanta) och det associerade innehållet. Ersätt detta med en enda post, **StorageAccountConnectionString**, med ett tomt strängvärde.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-108">Remove the **exclude** entry and its associated contents and replace it with the single entry **StorageAccountConnectionString**, with an empty string value.</span></span> <span data-ttu-id="c7bd6-109">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c7bd6-109">It should look similar to the following:</span></span>

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. <span data-ttu-id="c7bd6-110">Högerklicka på den **appsettings.json** fil i den **Solution Explorer** och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-110">Right click the **appsettings.json** file in the **Solution Explorer** and select **Properties**.</span></span>

1. <span data-ttu-id="c7bd6-111">Ändra **Copy to Output Directory** (Kopiera till utdatakatalog) till **Copy if newer** (Kopiera om nyare).</span><span class="sxs-lookup"><span data-stu-id="c7bd6-111">Change **Copy to Output Directory** to **Copy if newer**.</span></span>

  ![Skärmbild av Visual Studio som visar dialogrutan appsettings.json egenskapssidor med avsnittet Kopiera till utdatakatalog markerat och ange att kopiera om nyare.](..\media-draft\7-build-action.png)

  > <span data-ttu-id="c7bd6-113">Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-113">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="c7bd6-114">Lägga till stöd för läsning från en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="c7bd6-114">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="c7bd6-115">Ett .NET Core-konsolprogram kräver att vissa bibliotek läggs till för att det ska kunna läsa från en JSON-konfigurationsfil utan problem.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-115">A .NET Core console application requires certain libraries to be added to allow it to easily read from a JSON configuration file.</span></span>

1. <span data-ttu-id="c7bd6-116">Högerklicka på projektet och välj **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="c7bd6-116">Right click on the project and select **Manage NuGet Packages…**.</span></span>

1. <span data-ttu-id="c7bd6-117">Klicka på den **Bläddra** och ange **Microsoft.Extensions.Configuration.Json** i sökfältet.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-117">Click on the **Browse** tab and type **Microsoft.Extensions.Configuration.Json** in the search field.</span></span>

1. <span data-ttu-id="c7bd6-118">Välj den **Microsoft.Extensions.Configuration.Json** paketet från sökresultaten och, i den högra rutan, väljer **installera**.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-118">Select the **Microsoft.Extensions.Configuration.Json** package from the search results and, in the right pane, select **Install**.</span></span>

1. <span data-ttu-id="c7bd6-119">Dialogrutan **Förhandsgranska ändringar** visas.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-119">A **Preview Changes** dialog is displayed.</span></span> <span data-ttu-id="c7bd6-120">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-120">Click **OK**.</span></span>

1. <span data-ttu-id="c7bd6-121">En **godkännande av licensen** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-121">A **License Acceptance** dialog is displayed.</span></span> <span data-ttu-id="c7bd6-122">Klicka på **jag accepterar** när du har granskat licenser.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-122">Click **I Accept** after reviewing any licenses.</span></span>

## <a name="read-from-the-configuration-file"></a><span data-ttu-id="c7bd6-123">Läsa från konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="c7bd6-123">Read from the configuration file</span></span>

<span data-ttu-id="c7bd6-124">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-124">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="c7bd6-125">Leta upp filen **Program.cs** i projektet och öppna den i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-125">Locate the **Program.cs** file in your project and open it in Visual Studio.</span></span>

1. <span data-ttu-id="c7bd6-126">Högst upp i filen, lägger du till följande ytterligare `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="c7bd6-126">At the top of the file, add the following additional `using` statements:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="c7bd6-127">Ersätt innehållet i **huvudmetoden** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="c7bd6-127">Replace the contents of the **Main** method with the following code:</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

1. <span data-ttu-id="c7bd6-128">Filen **Program.cs** bör se ut så här nu:</span><span class="sxs-lookup"><span data-stu-id="c7bd6-128">Your **Program.cs** file should now look like the following:</span></span>

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;

    namespace DemoConsoleApp
    {
        class Program
        {
            static void Main(string[] args)
            {
                var builder = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile("appsettings.json");

                var configuration = builder.Build();
            }
        }
    }
    ```

    > <span data-ttu-id="c7bd6-129">Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-129">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

## <a name="add-your-connection-string-to-the-configuration-file"></a><span data-ttu-id="c7bd6-130">Lägga till anslutningssträngen i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="c7bd6-130">Add your connection string to the configuration file</span></span>

<span data-ttu-id="c7bd6-131">Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-131">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="c7bd6-132">Gå till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-132">Navigate to the Azure portal.</span></span>

1. <span data-ttu-id="c7bd6-133">Under den **alla resurser** avsnittet, leta reda på nyligen skapade lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-133">Under the **All resources** section, find your newly created storage account.</span></span>

1. <span data-ttu-id="c7bd6-134">Navigera till den **inställningar** > **Kontonycklar** avsnittet i bladet storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-134">Navigate to the **Settings** > **Account keys** section in the storage account blade.</span></span>

1. <span data-ttu-id="c7bd6-135">Kopiera den **anslutningssträngen** värdet från den **key1** fält.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-135">Copy the **Connection string** value from the **key1** fields.</span></span>

1. <span data-ttu-id="c7bd6-136">Klistra in innehållet i åtkomstnyckeln som du kopierade från portalen som värde för **StorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="c7bd6-136">Paste in the contents of the access key you copied from the portal as the value for **StorageAccountConnectionString**.</span></span> <span data-ttu-id="c7bd6-137">Konfigurationen bör nu se ut ungefär så här med din anslutning sträng ersätter den `<your-access-key-connection-string-goes-here>` platshållaren (utan vinkelparenteser):</span><span class="sxs-lookup"><span data-stu-id="c7bd6-137">Your configuration should now look similar to the following, with your connection string replacing the `<your-access-key-connection-string-goes-here>` placeholder (without angle brackets):</span></span>

    ```json
    {
      "StorageAccountConnectionString": "<your-access-key-connection-string-goes-here>"
    }
    ```
