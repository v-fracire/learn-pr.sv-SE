<span data-ttu-id="9543a-101">I den här övningen ska du hämta åtkomstnyckeln och lägga till den i appkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="9543a-101">In this exercise you will retrieve the access key and add it to your app configuration.</span></span> <span data-ttu-id="9543a-102">Eftersom vi har skapat ett .NET Core-konsolprogram måste vi också lägga till stöd för läsning från konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="9543a-102">In addition, since we created a .NET Core console application, we need to add support for reading from configuration files to the application.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="9543a-103">Skapa en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="9543a-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="9543a-104">Högerklicka på Visual Studio-projektet som vi skapade i den föregående övningen, välj **Lägg till**och välj sedan **Nytt objekt** (eller håll ned Ctrl+Skift+ A).</span><span class="sxs-lookup"><span data-stu-id="9543a-104">In your Visual Studio project that we created in the previous unit, right click the project, select **Add**, then select **New Item** (or hold down CTRL-SHIFT-A).</span></span>
1. <span data-ttu-id="9543a-105">En modal dialogruta visas.</span><span class="sxs-lookup"><span data-stu-id="9543a-105">A modal dialog is presented.</span></span> <span data-ttu-id="9543a-106">Välj **JavaScript JSON Configuration File** och skriv **appsettings.json** i textfältet *Namn* och klicka på **Lägg till** 
   ![appsettings](..\media-draft\9-appsettings.png)</span><span class="sxs-lookup"><span data-stu-id="9543a-106">Select **JavaScript JSON Configuration File** and type **appsettings.json** into the *Name* textbox, then click **Add**
![appsettings](..\media-draft\9-appsettings.png)</span></span>
1. <span data-ttu-id="9543a-107">En fil läggs till i projektet med ett innehåll som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="9543a-107">A file is added to the project with contents similar to the following:</span></span>
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
1. <span data-ttu-id="9543a-108">Ta bort posten **exclude** (undanta) och det associerade innehållet. Ersätt detta med en enda post, **StorageAccountConnectionString**, med ett tomt strängvärde.</span><span class="sxs-lookup"><span data-stu-id="9543a-108">Remove the **exclude** entry and its associated contents and replace with a single entry **StorageAccountConnectionString** with an empty string value.</span></span> <span data-ttu-id="9543a-109">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="9543a-109">It should look similar to the following:</span></span>
    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. <span data-ttu-id="9543a-110">Välj filen **appsettings.json**, högerklicka på den och välj **Egenskaper** (du kan också trycka på Alt+Retur eller F4).</span><span class="sxs-lookup"><span data-stu-id="9543a-110">Select the **appsettings.json** file, right click it and select **Properties** (alternatively press ALT+ENTER or F4).</span></span>
1. <span data-ttu-id="9543a-111">Ändra **Copy to Output Directory** (Kopiera till utdatakatalog) till **Copy if newer**
  ![build action](..\media-draft\10-build-action.png) (Kopiera om nyare).</span><span class="sxs-lookup"><span data-stu-id="9543a-111">Change the **Copy to Output Directory** to **Copy if newer**
![build action](..\media-draft\10-build-action.png)</span></span>

  > <span data-ttu-id="9543a-112">Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.</span><span class="sxs-lookup"><span data-stu-id="9543a-112">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="9543a-113">Lägga till stöd för läsning från en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="9543a-113">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="9543a-114">Ett .NET Core-konsolprogram kräver att vissa bibliotek läggs till för att det ska kunna läsa från en JSON-konfigurationsfil utan problem.</span><span class="sxs-lookup"><span data-stu-id="9543a-114">A .NET Core console application requires certain libraries to be added to allow it to easily read from a JSON configuration file.</span></span>

1. <span data-ttu-id="9543a-115">Högerklicka på projektet och välj **Hantera Nuget-paket …**
  ![Hantera NuGet-paket](..\media-draft\11-manage-nuget-packages.png).</span><span class="sxs-lookup"><span data-stu-id="9543a-115">Right click on the project and select **Manage Nuget Packages …**
![Manage NuGet packages](..\media-draft\11-manage-nuget-packages.png)</span></span>
1. <span data-ttu-id="9543a-116">Då visas en sida med de Nuget-paket som är installerade för tillfället.</span><span class="sxs-lookup"><span data-stu-id="9543a-116">A page is displayed showing currently installed Nuget packages.</span></span> <span data-ttu-id="9543a-117">Klicka på **Bläddra** och skriv **Microsoft.Extensions.Configuration.Json** i sökfältet.</span><span class="sxs-lookup"><span data-stu-id="9543a-117">Click on the **Browse** option and type in **Microsoft.Extensions.Configuration.Json** in the search field.</span></span> <span data-ttu-id="9543a-118">Paketet **Microsoft.Extensions.Configuration.Json** visas i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="9543a-118">**Microsoft.Extensions.Configuration.Json** package is displayed in the search results.</span></span>
1. <span data-ttu-id="9543a-119">Välj paketet **Microsoft.Extensions.Configuration.Json** och välj **Installera** i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="9543a-119">Select the **Microsoft.Extensions.Configuration.Json** package and in the right pane select **Install**</span></span>
1. <span data-ttu-id="9543a-120">Dialogrutan **Förhandsgranska ändringar** visas.</span><span class="sxs-lookup"><span data-stu-id="9543a-120">A **Preview Changes** dialog is displayed.</span></span> <span data-ttu-id="9543a-121">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="9543a-121">Click **OK**</span></span>
1. <span data-ttu-id="9543a-122">En dialogruta för godkännande av licens visas.</span><span class="sxs-lookup"><span data-stu-id="9543a-122">A License acceptance dialog is displayed.</span></span> <span data-ttu-id="9543a-123">Klicka på **Jag accepterar**.</span><span class="sxs-lookup"><span data-stu-id="9543a-123">Click **I Accept**</span></span>

## <a name="read-from-the-configuration-file"></a><span data-ttu-id="9543a-124">Läsa från konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="9543a-124">Read from the configuration file</span></span>

<span data-ttu-id="9543a-125">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="9543a-125">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="9543a-126">Leta upp filen **Program.cs** i projektet och öppna den i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9543a-126">Locate the **Program.cs** file in your project and open it in Visual Studio.</span></span>
1. <span data-ttu-id="9543a-127">Överst i filen står det **using System;**.</span><span class="sxs-lookup"><span data-stu-id="9543a-127">At the top of the file, a **using System;** is present.</span></span> <span data-ttu-id="9543a-128">Lägg till följande rader med kod under den raden:</span><span class="sxs-lookup"><span data-stu-id="9543a-128">Underneath that line, add the following lines of code:</span></span>
    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```
1. <span data-ttu-id="9543a-129">Ersätt innehållet i **huvudmetoden** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="9543a-129">Replace the contents of the **Main** method with the following code:</span></span>
    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```
1. <span data-ttu-id="9543a-130">Filen **Program.cs** bör se ut så här nu:</span><span class="sxs-lookup"><span data-stu-id="9543a-130">Your **Program.cs** file should now look like the following:</span></span>
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

> <span data-ttu-id="9543a-131">Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="9543a-131">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

## <a name="add-your-storage-connection-string-to-the-configuration-file"></a><span data-ttu-id="9543a-132">Lägga till anslutningssträngen för lagringskontot i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="9543a-132">Add your Storage connection string to the configuration file</span></span>

<span data-ttu-id="9543a-133">Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.</span><span class="sxs-lookup"><span data-stu-id="9543a-133">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="9543a-134">Gå till Azure-portalen för den prenumeration som innehåller ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9543a-134">Navigate to the Azure portal for your subscription that contains your storage account.</span></span>
1. <span data-ttu-id="9543a-135">Navigera till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9543a-135">Navigate to your storage account.</span></span>
1. <span data-ttu-id="9543a-136">Navigera till bladet Kontonycklar för lagringskontot i portalen.</span><span class="sxs-lookup"><span data-stu-id="9543a-136">Navigate to the Account Keys blade of the storage account in the portal.</span></span>
1. <span data-ttu-id="9543a-137">Kopiera anslutningssträngen **key1**.</span><span class="sxs-lookup"><span data-stu-id="9543a-137">Copy the **key1** connection string.</span></span>
1. <span data-ttu-id="9543a-138">Klistra in innehållet i åtkomstnyckeln som du kopierade från portalen som värde för **StorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="9543a-138">Paste in the contents of the access key you copied from the portal as the value for the **StorageAccountConnectionString**.</span></span> <span data-ttu-id="9543a-139">Konfigurationen bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="9543a-139">Your configuration should now look similar to the following:</span></span>
    ```json
    {
      "StorageAccountConnectionString": "your-access-key-connection-string-goes-here"
    }
    ```


