<span data-ttu-id="f6827-101">I den här enheten ska du hämta åtkomstnyckeln och lägga till den i appkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f6827-101">In this unit you will retrieve the access key and add it to your app configuration.</span></span> <span data-ttu-id="f6827-102">Eftersom vi har skapat ett .NET Core-konsolprogram måste vi också lägga till stöd för läsning från konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="f6827-102">In addition, since we created a .NET Core console application, we need to add support for reading from configuration files to the application.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="f6827-103">Skapa en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="f6827-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="f6827-104">Högerklicka på Visual Studio-projektet som vi skapade i den föregående övningen, välj **Lägg till**och välj sedan **Nytt objekt** (eller håll ned Ctrl+Skift+ A).</span><span class="sxs-lookup"><span data-stu-id="f6827-104">In your Visual Studio project that we created in the previous unit, right click the project, select **Add**, and then select **New Item** (or hold down CTRL-SHIFT-A).</span></span>

1. <span data-ttu-id="f6827-105">En modal dialogruta visas.</span><span class="sxs-lookup"><span data-stu-id="f6827-105">A modal dialog is presented.</span></span> <span data-ttu-id="f6827-106">Välj **JavaScript JSON Configuration File** och skriv **appsettings.json** i textfältet *Namn* och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f6827-106">Select **JavaScript JSON Configuration File** and type **appsettings.json** into the *Name* textbox, and then click **Add**.</span></span>

  ![Appinställningar](..\media-draft\7-appsettings.png)

1. <span data-ttu-id="f6827-108">En fil läggs till i projektet med ett innehåll som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="f6827-108">A file is added to the project with contents similar to the following:</span></span>

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
1. <span data-ttu-id="f6827-109">Ta bort posten **exclude** (undanta) och det associerade innehållet. Ersätt detta med en enda post, **StorageAccountConnectionString**, med ett tomt strängvärde.</span><span class="sxs-lookup"><span data-stu-id="f6827-109">Remove the **exclude** entry and its associated contents and replace it with the single entry **StorageAccountConnectionString**, with an empty string value.</span></span> <span data-ttu-id="f6827-110">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="f6827-110">It should look similar to the following:</span></span>

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. <span data-ttu-id="f6827-111">Välj filen **appsettings.json**, högerklicka på den och välj **Egenskaper** (du kan också trycka på Alt+Retur eller F4).</span><span class="sxs-lookup"><span data-stu-id="f6827-111">Select the **appsettings.json** file, right click it and select **Properties** (alternatively, press ALT+ENTER or F4).</span></span>

1. <span data-ttu-id="f6827-112">Ändra **Copy to Output Directory** (Kopiera till utdatakatalog) till **Copy if newer** (Kopiera om nyare).</span><span class="sxs-lookup"><span data-stu-id="f6827-112">Change **Copy to Output Directory** to **Copy if newer**.</span></span>

  ![Build-åtgärd](..\media-draft\7-build-action.png)

  > <span data-ttu-id="f6827-114">Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.</span><span class="sxs-lookup"><span data-stu-id="f6827-114">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="f6827-115">Lägga till stöd för läsning från en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="f6827-115">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="f6827-116">Ett .NET Core-konsolprogram kräver att vissa bibliotek läggs till för att det ska kunna läsa från en JSON-konfigurationsfil utan problem.</span><span class="sxs-lookup"><span data-stu-id="f6827-116">A .NET Core console application requires certain libraries to be added to allow it to easily read from a JSON configuration file.</span></span>

1. <span data-ttu-id="f6827-117">Högerklicka på projektet och välj **Hantera NuGet-paket...**.</span><span class="sxs-lookup"><span data-stu-id="f6827-117">Right click on the project and select **Manage Nuget Packages…**.</span></span>

![Hantera NuGet-paket](..\media-draft\5-manage-nuget-packages.png)

1. <span data-ttu-id="f6827-119">En sida visas med de NuGet-paket som är installerade.</span><span class="sxs-lookup"><span data-stu-id="f6827-119">A page is displayed, showing currently installed Nuget packages.</span></span> <span data-ttu-id="f6827-120">Klicka på **Bläddra** och skriv **Microsoft.Extensions.Configuration.Json** i sökfältet.</span><span class="sxs-lookup"><span data-stu-id="f6827-120">Click on the **Browse** option and type **Microsoft.Extensions.Configuration.Json** in the search field.</span></span> <span data-ttu-id="f6827-121">Paketet **Microsoft.Extensions.Configuration.Json** visas i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="f6827-121">The **Microsoft.Extensions.Configuration.Json** package is displayed in the search results.</span></span>

1. <span data-ttu-id="f6827-122">Välj paketet **Microsoft.Extensions.Configuration.Json** och välj **Installera** i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="f6827-122">Select the **Microsoft.Extensions.Configuration.Json** package, and in the right pane select **Install**.</span></span>

1. <span data-ttu-id="f6827-123">Dialogrutan **Förhandsgranska ändringar** visas.</span><span class="sxs-lookup"><span data-stu-id="f6827-123">A **Preview Changes** dialog is displayed.</span></span> <span data-ttu-id="f6827-124">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f6827-124">Click **OK**.</span></span>

1. <span data-ttu-id="f6827-125">En dialogruta för godkännande av licens visas.</span><span class="sxs-lookup"><span data-stu-id="f6827-125">A license acceptance dialog is displayed.</span></span> <span data-ttu-id="f6827-126">Klicka på **Jag accepterar**.</span><span class="sxs-lookup"><span data-stu-id="f6827-126">Click **I Accept**.</span></span>


## <a name="read-from-the-configuration-file"></a><span data-ttu-id="f6827-127">Läsa från konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="f6827-127">Read from the configuration file</span></span>

<span data-ttu-id="f6827-128">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="f6827-128">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="f6827-129">Leta upp filen **Program.cs** i projektet och öppna den i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f6827-129">Locate the **Program.cs** file in your project and open it in Visual Studio.</span></span>

1. <span data-ttu-id="f6827-130">Överst i filen visas raden **using System;**.</span><span class="sxs-lookup"><span data-stu-id="f6827-130">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="f6827-131">Lägg till följande rader med kod under den raden:</span><span class="sxs-lookup"><span data-stu-id="f6827-131">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="f6827-132">Ersätt innehållet i **huvudmetoden** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="f6827-132">Replace the contents of the **Main** method with the following code:</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

1. <span data-ttu-id="f6827-133">Filen **Program.cs** bör se ut så här nu:</span><span class="sxs-lookup"><span data-stu-id="f6827-133">Your **Program.cs** file should now look like the following:</span></span>

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

> <span data-ttu-id="f6827-134">Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="f6827-134">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

## <a name="add-your-connection-string-to-the-configuration-file"></a><span data-ttu-id="f6827-135">Lägga till anslutningssträngen i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="f6827-135">Add your connection string to the configuration file</span></span>

<span data-ttu-id="f6827-136">Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.</span><span class="sxs-lookup"><span data-stu-id="f6827-136">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="f6827-137">Gå till Azure-portalen för den prenumeration som innehåller ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f6827-137">Navigate to the Azure portal for your subscription that contains your storage account.</span></span>

1. <span data-ttu-id="f6827-138">Navigera till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f6827-138">Navigate to your storage account.</span></span>

1. <span data-ttu-id="f6827-139">Navigera till bladet Kontonycklar för lagringskontot i portalen.</span><span class="sxs-lookup"><span data-stu-id="f6827-139">Navigate to the Account Keys blade of the storage account in the portal.</span></span>

1. <span data-ttu-id="f6827-140">Kopiera anslutningssträngen **key1**.</span><span class="sxs-lookup"><span data-stu-id="f6827-140">Copy the **key1** connection string.</span></span>

1. <span data-ttu-id="f6827-141">Klistra in innehållet i åtkomstnyckeln som du kopierade från portalen som värde för **StorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="f6827-141">Paste in the contents of the access key you copied from the portal as the value for **StorageAccountConnectionString**.</span></span> <span data-ttu-id="f6827-142">Konfigurationen bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="f6827-142">Your configuration should now look similar to the following:</span></span>

    ```json
    {
      "StorageAccountConnectionString": "your-access-key-connection-string-goes-here"
    }
    ```
