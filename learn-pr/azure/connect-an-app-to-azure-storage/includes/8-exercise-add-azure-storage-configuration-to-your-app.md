<span data-ttu-id="2ff15-101">::: zone pivot="csharp" Nu ska vi lägga till stöd i vår .NET Core-app för att hämta en anslutningssträng från en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="2ff15-101">::: zone pivot="csharp" Let's add support to our .NET core application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="2ff15-102">Vi börjar med att lägga till de delar som behövs för att hantera konfigurationen i en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="2ff15-102">We'll start by adding the necessary plumbing to manage configuration in a JSON file.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="2ff15-103">Skapa en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="2ff15-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="2ff15-104">`cd` till katalogen PhotoSharingApp om du inte redan är där.</span><span class="sxs-lookup"><span data-stu-id="2ff15-104">`cd` to the PhotoSharingApp directory if you aren't already there.</span></span>

1. <span data-ttu-id="2ff15-105">Använd verktyget `touch` på kommandoraden till att skapa en fil med namnet **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="2ff15-105">Use the `touch` tool on the command line to create a file named **appsettings.json**.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="2ff15-106">Öppna projektet i en redigerare.</span><span class="sxs-lookup"><span data-stu-id="2ff15-106">Open the project in an editor.</span></span> <span data-ttu-id="2ff15-107">Om du arbetar lokalt kan du använda valfri redigerare.</span><span class="sxs-lookup"><span data-stu-id="2ff15-107">If you are working locally, use your editor of choice.</span></span> <span data-ttu-id="2ff15-108">Vi rekommenderar **Visual Studio Code**, som är en utökningsbar plattformsoberoende IDE.</span><span class="sxs-lookup"><span data-stu-id="2ff15-108">We recommend **Visual Studio Code**, which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="2ff15-109">Om du arbetar i Cloud Shell rekommenderar vi Cloud Shell-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="2ff15-109">If you are working in the Cloud Shell, we recommend the Cloud Shell editor.</span></span> <span data-ttu-id="2ff15-110">Följande kommando fungerar för båda.</span><span class="sxs-lookup"><span data-stu-id="2ff15-110">The following command works for both.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="2ff15-111">Välj filen **appsettings.json** i redigeraren och lägg till följande text.</span><span class="sxs-lookup"><span data-stu-id="2ff15-111">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="2ff15-112">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="2ff15-112">Save the file.</span></span> <span data-ttu-id="2ff15-113">I Cloud Shell-redigeraren finns det en meny i det övre högra hörnet med vanliga filåtgärder.</span><span class="sxs-lookup"><span data-stu-id="2ff15-113">In the Cloud Shell editor, there is a menu in the top right corner that has common file operations.</span></span>

    ```json
    {
      "StorageAccountConnectionString": "<value>"
    }
    ```

1. <span data-ttu-id="2ff15-114">Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.</span><span class="sxs-lookup"><span data-stu-id="2ff15-114">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span> <span data-ttu-id="2ff15-115">Kör följande kommando i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="2ff15-115">In Cloud Shell run the following command.</span></span>

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. <span data-ttu-id="2ff15-116">Kopiera anslutningssträngen som returneras från det kommandot och ersätt `"<value>"` i filen **appsettings.json** med den här anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="2ff15-116">Copy the connection string that is returned from that command, and replace `"<value>"` in the **appsettings.json** file with this connection string.</span></span> <span data-ttu-id="2ff15-117">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="2ff15-117">Save the file.</span></span>

1. <span data-ttu-id="2ff15-118">Öppna sedan projektfilen (**PhotoSharingApp.csproj**) i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="2ff15-118">Next, open the project file (**PhotoSharingApp.csproj**) in the editor.</span></span>

1. <span data-ttu-id="2ff15-119">Lägg till följande konfigurationsblock för att inkludera den nya filen i projektet och kopiera den till utdatamappen.</span><span class="sxs-lookup"><span data-stu-id="2ff15-119">Add the following configuration block to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="2ff15-120">Detta säkerställer att appens konfigurationsfil placeras i utdatakatalogen när appen kompileras/skapas.</span><span class="sxs-lookup"><span data-stu-id="2ff15-120">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

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

1. <span data-ttu-id="2ff15-121">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="2ff15-121">Save the file.</span></span> <span data-ttu-id="2ff15-122">(Var noga med att göra det här, annars förlorar du dina ändringar när du lägger till paketet nedan!)</span><span class="sxs-lookup"><span data-stu-id="2ff15-122">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="2ff15-123">Lägga till stöd för läsning från en JSON-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="2ff15-123">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="2ff15-124">I .NET Core-program behövs fler NuGet-paket för att kunna läsa en JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="2ff15-124">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="2ff15-125">Lägg till en referens till NuGet-paketet **Microsoft.Extensions.Configuration.Json** i kommandotolken i fönstret.</span><span class="sxs-lookup"><span data-stu-id="2ff15-125">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="2ff15-126">Lägga till kod för att läsa konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="2ff15-126">Add code to read the configuration file</span></span>

<span data-ttu-id="2ff15-127">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="2ff15-127">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="2ff15-128">Välj **Program.cs** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="2ff15-128">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="2ff15-129">Längst upp i filen ser du raden **using System;**.</span><span class="sxs-lookup"><span data-stu-id="2ff15-129">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="2ff15-130">Lägg till följande kodrader under den här raden:</span><span class="sxs-lookup"><span data-stu-id="2ff15-130">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="2ff15-131">Ersätt innehållet i metoden **Main** med följande kod.</span><span class="sxs-lookup"><span data-stu-id="2ff15-131">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="2ff15-132">Den här koden initierar konfigurationssystemet och får det att läsa från filen **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="2ff15-132">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

<span data-ttu-id="2ff15-133">Filen **Program.cs** bör se ut så här nu:</span><span class="sxs-lookup"><span data-stu-id="2ff15-133">Your **Program.cs** file should now look like the following:</span></span>

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

<span data-ttu-id="2ff15-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2ff15-134">::: zone-end</span></span>

<span data-ttu-id="2ff15-135">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="2ff15-135">::: zone pivot="javascript"</span></span>

<span data-ttu-id="2ff15-136">Nu ska vi lägga till stöd i vår Node.js-app för att hämta en anslutningssträng från en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="2ff15-136">Let's add support to our Node.js application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="2ff15-137">Vi börjar med att lägga till de delar som behövs för att hantera konfigurationen från vår JavaScript-fil.</span><span class="sxs-lookup"><span data-stu-id="2ff15-137">We'll start by adding the necessary plumbing to manage configuration from our JavaScript file.</span></span>

## <a name="create-a-env-configuration-file"></a><span data-ttu-id="2ff15-138">Skapa en .env-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="2ff15-138">Create a .env configuration file</span></span>

1. <span data-ttu-id="2ff15-139">Se till att du befinner dig i rätt arbetskatalog för projektet.</span><span class="sxs-lookup"><span data-stu-id="2ff15-139">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="2ff15-140">Använd verktyget `touch` på kommandoraden till att skapa en fil med namnet **.env**.</span><span class="sxs-lookup"><span data-stu-id="2ff15-140">Use the `touch` tool on the command line to create a file named **.env**.</span></span>

    ```bash
    touch .env
    ```

1. <span data-ttu-id="2ff15-141">Öppna projektet i Cloud Shell-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="2ff15-141">Open the project in the Cloud Shell editor.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="2ff15-142">Välj filen **.env** i redigeraren och lägg till följande text.</span><span class="sxs-lookup"><span data-stu-id="2ff15-142">Select the **.env** file in the editor and add the following text.</span></span> <span data-ttu-id="2ff15-143">Du kan behöva klicka på knappen Uppdatera i koden för att se de nya filerna.</span><span class="sxs-lookup"><span data-stu-id="2ff15-143">You may need to click the refresh button in code to see the new files.</span></span> <span data-ttu-id="2ff15-144">Spara filen. I onlineredigeraren finns det en meny i det övre högra hörnet med vanliga filåtgärder.</span><span class="sxs-lookup"><span data-stu-id="2ff15-144">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > <span data-ttu-id="2ff15-145">Värdet **AZURE_STORAGE_CONNECTION_STRING** är en hårdkodad miljövariabel som API:erna i Storage använder till att slå upp åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="2ff15-145">The **AZURE_STORAGE_CONNECTION_STRING** value is a hard-coded environment variable used for Storage APIs to look up access keys.</span></span> <span data-ttu-id="2ff15-146">Du kan använda ett eget namn om du vill, men du måste ange namnet när du skapar `BlobService`-objektet i Node.js-appen.</span><span class="sxs-lookup"><span data-stu-id="2ff15-146">You can use your own name if you prefer, but you must supply the name to the when you create the `BlobService` object in your Node.js app.</span></span>

1. <span data-ttu-id="2ff15-147">Nu måste vi hämta anslutningssträngen för lagringskontot och placera den i konfigurationen för vår app.</span><span class="sxs-lookup"><span data-stu-id="2ff15-147">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span> <span data-ttu-id="2ff15-148">Kör följande kommando i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="2ff15-148">In Cloud Shell run the following command.</span></span>

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. <span data-ttu-id="2ff15-149">Kopiera anslutningssträngen som returneras från kommandot, minus citattecknen, och ersätt `<value>` i filen **.env** med den här anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="2ff15-149">Copy the connection string that is returned from that command, minus the quotes, and replace `<value>` in the **.env** file with this connection string.</span></span>

1. <span data-ttu-id="2ff15-150">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="2ff15-150">Save the file.</span></span>

## <a name="add-support-to-read-an-environment-configuration-file"></a><span data-ttu-id="2ff15-151">Lägga till stöd för läsning från en miljökonfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="2ff15-151">Add support to read an environment configuration file</span></span>

<span data-ttu-id="2ff15-152">Node.js-appar kan innehålla stöd för att läsa från filen **.env** om du lägger till paketet **dotenv**.</span><span class="sxs-lookup"><span data-stu-id="2ff15-152">Node.js apps can include support to read from the **.env** file by adding the **dotenv** package.</span></span>

1. <span data-ttu-id="2ff15-153">Lägg till ett beroende för paketet *dotenv*\* i kommandotolken i fönstret med hjälp av `npm`.</span><span class="sxs-lookup"><span data-stu-id="2ff15-153">In the command prompt section of the window, add a dependency to the  *dotenv*\* package using `npm`.</span></span>

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="2ff15-154">Lägga till kod för att läsa konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="2ff15-154">Add code to read the configuration file</span></span>

<span data-ttu-id="2ff15-155">Nu när vi har lagt till de bibliotek som krävs för att möjliggöra inläsning av konfigurationen måste vi aktivera funktionerna i vårt program.</span><span class="sxs-lookup"><span data-stu-id="2ff15-155">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our application.</span></span>

1. <span data-ttu-id="2ff15-156">Välj *index.js*\* i redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="2ff15-156">Select *index.js*\* in the editor.</span></span>

1. <span data-ttu-id="2ff15-157">Längst upp i filen ser du raden **#!/usr/bin/env node**.</span><span class="sxs-lookup"><span data-stu-id="2ff15-157">At the top of the file, a **#!/usr/bin/env node** line is present.</span></span> <span data-ttu-id="2ff15-158">Under den här raden lägger du till ett `require`-uttryck som läser in paketet **dotenv**.</span><span class="sxs-lookup"><span data-stu-id="2ff15-158">Underneath that line, add a `require` statement to load the **dotenv** package.</span></span> <span data-ttu-id="2ff15-159">Det här gör miljövariablerna som definieras i vår **.env**-fil tillgängliga i programmet.</span><span class="sxs-lookup"><span data-stu-id="2ff15-159">This will make environment variables defined in our **.env** file available to the program.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
<span data-ttu-id="2ff15-160">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2ff15-160">::: zone-end</span></span>

<span data-ttu-id="2ff15-161">Nu när vi har ordnat med allt det här kan vi börja lägga till kod för att använda vårt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2ff15-161">Now that we have that all wired up, we can start adding code to use our storage account.</span></span>