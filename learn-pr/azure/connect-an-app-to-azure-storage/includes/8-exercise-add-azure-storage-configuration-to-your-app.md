<span data-ttu-id="86f08-101">::: zone pivot="csharp" Nu ska vi lägga till stöd i vår .NET Core-app för att hämta en anslutningssträng från en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="86f08-101">::: zone pivot="csharp" Let's add support to our .NET core application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="86f08-102">Vi börjar med att lägga till de delar som behövs för att hantera konfigurationen i en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="86f08-102">We'll start by adding the necessary plumbing to manage configuration in a JSON file.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="86f08-103">Skapa en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="86f08-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="86f08-104">Se till att du befinner dig i rätt arbetskatalog för projektet.</span><span class="sxs-lookup"><span data-stu-id="86f08-104">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="86f08-105">Använd verktyget `touch` på kommandoraden till att skapa en fil med namnet **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="86f08-105">Use the `touch` tool on the command line to create a file named **appsettings.json**.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="86f08-106">Öppna projektet i den interaktiva redigeraren. Om du arbetar lokalt kan du använda valfritt redigeringsprogram, vi rekommenderar **Visual Studio Code** som är en utökningsbar och plattformsoberoende IDE.</span><span class="sxs-lookup"><span data-stu-id="86f08-106">Open the project with the interactive editor, if you are working locally, use your editor of choice - we recommend **Visual Studio Code** which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="86f08-107">Följande kommandon är för redigeringsprogrammet Cloud Shell, men de är mycket lika kommandona i VS Code.</span><span class="sxs-lookup"><span data-stu-id="86f08-107">The following commands are for the Cloud Shell editor, but are very similar to VS Code.</span></span>
    
    ```bash
    code .
    ```

1. <span data-ttu-id="86f08-108">Välj filen **appsettings.json** i redigeraren och lägg till följande text:</span><span class="sxs-lookup"><span data-stu-id="86f08-108">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="86f08-109">Spara filen. I onlineredigeraren finns det en meny i det övre högra hörnet med vanliga filåtgärder.</span><span class="sxs-lookup"><span data-stu-id="86f08-109">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. <span data-ttu-id="86f08-110">Välj sedan projektfilen (**PhotoSharingApp.csproj**) så att den öppnas i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="86f08-110">Next, select the project file (**PhotoSharingApp.csproj**) to open it in the editor.</span></span>

1. <span data-ttu-id="86f08-111">Lägg till följande konfigurationsblock för att inkludera den nya filen i projektet och kopiera den till utdatamappen.</span><span class="sxs-lookup"><span data-stu-id="86f08-111">Add the following configuration block to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="86f08-112">Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.</span><span class="sxs-lookup"><span data-stu-id="86f08-112">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. <span data-ttu-id="86f08-113">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="86f08-113">Save the file.</span></span> <span data-ttu-id="86f08-114">(Var noga med att göra det här, annars förlorar du dina ändringar när du lägger till paketet nedan!)</span><span class="sxs-lookup"><span data-stu-id="86f08-114">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="86f08-115">Lägga till stöd för läsning från en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="86f08-115">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="86f08-116">I .NET Core-program behövs fler NuGet-paket för att kunna läsa en JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="86f08-116">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="86f08-117">Lägg till en referens till NuGet-paketet **Microsoft.Extensions.Configuration.Json** i kommandotolken i fönstret.</span><span class="sxs-lookup"><span data-stu-id="86f08-117">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="86f08-118">Lägga till kod för att läsa konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="86f08-118">Add code to read the configuration file</span></span>

<span data-ttu-id="86f08-119">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="86f08-119">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="86f08-120">Välj **Program.cs** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="86f08-120">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="86f08-121">Längst upp i filen ser du raden **using System;**.</span><span class="sxs-lookup"><span data-stu-id="86f08-121">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="86f08-122">Lägg till följande kodrader under den här raden:</span><span class="sxs-lookup"><span data-stu-id="86f08-122">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="86f08-123">Ersätt innehållet i metoden **Main** med följande kod.</span><span class="sxs-lookup"><span data-stu-id="86f08-123">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="86f08-124">Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="86f08-124">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

<span data-ttu-id="86f08-125">Filen **Program.cs** bör se ut så här nu:</span><span class="sxs-lookup"><span data-stu-id="86f08-125">Your **Program.cs** file should now look like the following:</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace PhotoSharingApp
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

<span data-ttu-id="86f08-126">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="86f08-126">::: zone-end</span></span>

<span data-ttu-id="86f08-127">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="86f08-127">::: zone-pivot="javascript"</span></span>

<span data-ttu-id="86f08-128">Nu ska vi lägga till stöd i vår Node.js-app för att hämta en anslutningssträng från en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="86f08-128">Let's add support to our Node.js application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="86f08-129">Vi börjar med att lägga till de delar som behövs för att hantera konfigurationen från vår JavaScript-fil.</span><span class="sxs-lookup"><span data-stu-id="86f08-129">We'll start by adding the necessary plumbing to manage configuration from our JavaScript file.</span></span>

## <a name="create-a-env-configuration-file"></a><span data-ttu-id="86f08-130">Skapa en .env-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="86f08-130">Create a .env configuration file</span></span>

1. <span data-ttu-id="86f08-131">Se till att du befinner dig i rätt arbetskatalog för projektet.</span><span class="sxs-lookup"><span data-stu-id="86f08-131">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="86f08-132">Använd verktyget `touch` på kommandoraden till att skapa en fil med namnet **.env**.</span><span class="sxs-lookup"><span data-stu-id="86f08-132">Use the `touch` tool on the command line to create a file named **.env**.</span></span>

    ```bash
    touch .env
    ```

1. <span data-ttu-id="86f08-133">Öppna projektet i den interaktiva redigeraren. Om du arbetar lokalt kan du använda valfritt redigeringsprogram, vi rekommenderar **Visual Studio Code** som är en utökningsbar och plattformsoberoende IDE.</span><span class="sxs-lookup"><span data-stu-id="86f08-133">Open the project with the interactive editor, if you are working locally, use your editor of choice - we recommend **Visual Studio Code** which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="86f08-134">Följande kommandon är för redigeringsprogrammet Cloud Shell, men de är mycket lika kommandona i VS Code.</span><span class="sxs-lookup"><span data-stu-id="86f08-134">The following commands are for the Cloud Shell editor, but are very similar to VS Code.</span></span>
    
    ```bash
    code .
    ```

1. <span data-ttu-id="86f08-135">Välj filen **.env** i redigeraren och lägg till följande text:</span><span class="sxs-lookup"><span data-stu-id="86f08-135">Select the **.env** file in the editor and add the following text.</span></span> <span data-ttu-id="86f08-136">Spara filen. I onlineredigeraren finns det en meny i det övre högra hörnet med vanliga filåtgärder.</span><span class="sxs-lookup"><span data-stu-id="86f08-136">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > <span data-ttu-id="86f08-137">Värdet **AZURE_STORAGE_CONNECTION_STRING** är en hårdkodad miljövariabel som API:erna i Storage använder till att slå upp åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="86f08-137">The **AZURE_STORAGE_CONNECTION_STRING** value is a hard-coded environment variable used for Storage APIs to look up access keys.</span></span> <span data-ttu-id="86f08-138">Du kan använda ett eget namn om du vill, men du måste ange namnet när du skapar `BlobService`-objektet.</span><span class="sxs-lookup"><span data-stu-id="86f08-138">You can use your own name if you prefer - but you must supply the name to the when you create the `BlobService` object.</span></span>

1. <span data-ttu-id="86f08-139">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="86f08-139">Save the file.</span></span>

## <a name="add-support-to-read-an-environment-configuration-file"></a><span data-ttu-id="86f08-140">Lägga till stöd för läsning från en miljökonfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="86f08-140">Add support to read an environment configuration file</span></span>

<span data-ttu-id="86f08-141">Node.js-appar kan innehålla stöd för att läsa från filen **.env** om du lägger till paketet **dotenv**.</span><span class="sxs-lookup"><span data-stu-id="86f08-141">Node.js apps can include support to read from the **.env** file by adding the **dotenv** package.</span></span>

1. <span data-ttu-id="86f08-142">Lägg till ett beroende för paketet *dotenv*\* i kommandotolken i fönstret.</span><span class="sxs-lookup"><span data-stu-id="86f08-142">In the command prompt section of the window, add a dependency to the  *dotenv*\* package.</span></span>

    ```bash
    node install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="86f08-143">Lägga till kod för att läsa konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="86f08-143">Add code to read the configuration file</span></span>

<span data-ttu-id="86f08-144">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt program.</span><span class="sxs-lookup"><span data-stu-id="86f08-144">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our application.</span></span>

1. <span data-ttu-id="86f08-145">Välj *index.js*\* i redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="86f08-145">Select *index.js*\* in the editor.</span></span>

1. <span data-ttu-id="86f08-146">Längst upp i filen ser du raden **#!/usr/bin/env node**.</span><span class="sxs-lookup"><span data-stu-id="86f08-146">At the top of the file, a **#!/usr/bin/env node** line is present.</span></span> <span data-ttu-id="86f08-147">Under den här raden lägger du till ett `require`-uttryck som läser in paketet **dotenv**.</span><span class="sxs-lookup"><span data-stu-id="86f08-147">Underneath that line, add a `require` statement to load the **dotenv** package.</span></span> <span data-ttu-id="86f08-148">Det här gör miljövariablerna som definieras i vår **.env**-fil tillgängliga i programmet.</span><span class="sxs-lookup"><span data-stu-id="86f08-148">This will make environment variables defined in our **.env** file available to the program.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
<span data-ttu-id="86f08-149">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="86f08-149">::: zone-end</span></span>

## <a name="add-the-connection-string-to-the-configuration-file"></a><span data-ttu-id="86f08-150">Lägga till anslutningssträngen i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="86f08-150">Add the connection string to the configuration file</span></span>

<span data-ttu-id="86f08-151">Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.</span><span class="sxs-lookup"><span data-stu-id="86f08-151">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="86f08-152">Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="86f08-152">Sign in to the [Azure Portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="86f08-153">Navigera till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="86f08-153">Navigate to your storage account.</span></span> <span data-ttu-id="86f08-154">Du kan leta rätt på lagringskontot under **Alla resurser** eller söka efter namnet i _sökrutan_ längst upp i portalfönstret.</span><span class="sxs-lookup"><span data-stu-id="86f08-154">You can use the **All Resources** section to find the storage account, or search by name from the _search box_ at the top of the portal window.</span></span> 

1. <span data-ttu-id="86f08-155">Välj bladet **Åtkomstnycklar** för lagringskontot i portalen.</span><span class="sxs-lookup"><span data-stu-id="86f08-155">Select the **Access Keys** blade of the storage account in the portal.</span></span>

1. <span data-ttu-id="86f08-156">Kopiera anslutningssträngen **key1**.</span><span class="sxs-lookup"><span data-stu-id="86f08-156">Copy the **key1** Connection string.</span></span>

1. <span data-ttu-id="86f08-157">Klistra in innehållet i åtkomstnyckeln du kopierade från portalen som värde för konfigurationsvariabeln för anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="86f08-157">Paste in the contents of the access key you copied from the portal as the value for the connection string configuration variable.</span></span>

<span data-ttu-id="86f08-158">Konfigurationen bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="86f08-158">Your configuration should now look similar to the following:</span></span>

<span data-ttu-id="86f08-159">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="86f08-159">::: zone pivot="csharp"</span></span>
    ```json
    {
        "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
    }
    ```
<span data-ttu-id="86f08-160">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="86f08-160">::: zone-end</span></span>

<span data-ttu-id="86f08-161">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="86f08-161">::: zone-pivot="javascript"</span></span>
    ```
    AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
    ```
<span data-ttu-id="86f08-162">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="86f08-162">::: zone-end</span></span>

<span data-ttu-id="86f08-163">Nu när vi har ordnat med allt det här kan vi börja lägga till kod för att använda vårt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="86f08-163">Now that we have that all wired up, we can start adding code to use our storage account.</span></span>