<span data-ttu-id="f8452-101">I den här enheten skapar du en Azure Redis-cache för att lagra och returnera värden som används ofta.</span><span class="sxs-lookup"><span data-stu-id="f8452-101">In this unit, you'll create an Azure Redis Cache instance to store and return commonly used values.</span></span>

## <a name="create-a-cache"></a><span data-ttu-id="f8452-102">Skapa en cache</span><span class="sxs-lookup"><span data-stu-id="f8452-102">Create a cache</span></span>

1. <span data-ttu-id="f8452-103">Logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f8452-103">Sign in to the Azure portal.</span></span> <span data-ttu-id="f8452-104">Klicka sedan på **Skapa en resurs**, klicka på **Databaser** och klicka på **Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="f8452-104">Then click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="f8452-105">Följande skärmbild visar Redis Cache-platsen i de olika alternativen för databasresurser på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f8452-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![Skärmbild som visar Azure-portalens databasalternativ, med Skapa en resurs, Databas och Redis Cache-alternativet markerade.](../media/4-create-a-cache-1.png)

## <a name="configure-your-cache"></a><span data-ttu-id="f8452-107">Konfigurera din cache</span><span class="sxs-lookup"><span data-stu-id="f8452-107">Configure your cache</span></span>

1. <span data-ttu-id="f8452-108">**DNS-namn:** Skapa ett globalt unikt namn, till exempel **ContosoSportsApp1028**.</span><span class="sxs-lookup"><span data-stu-id="f8452-108">**DNS Name:** Create a globally unique name such as **ContosoSportsApp1028**.</span></span>

1. <span data-ttu-id="f8452-109">**Prenumeration:** Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f8452-109">**Subscription:** Select your subscription.</span></span>

1. <span data-ttu-id="f8452-110">**Resursgrupp**: Skapa en ny resursgrupp och ge den ett namn.</span><span class="sxs-lookup"><span data-stu-id="f8452-110">**Resource group:** Create a new resource group and give it a name.</span></span>

1. <span data-ttu-id="f8452-111">**Plats:** Eftersom de flesta av dina kunder finns i New York du bör välja **USA, östra**.</span><span class="sxs-lookup"><span data-stu-id="f8452-111">**Location:** Because most of your customers are in the New York area, you should select **East US**.</span></span>

1. <span data-ttu-id="f8452-112">**Prisnivå:** Välj **Basic C0**.</span><span class="sxs-lookup"><span data-stu-id="f8452-112">**Pricing tier:** Select **Basic C0**.</span></span>

1. <span data-ttu-id="f8452-113">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f8452-113">Click **Create**.</span></span>

    <span data-ttu-id="f8452-114">Följande skärmbild visar en typisk konfiguration som används för att skapa en ny Redis Cache-resurs.</span><span class="sxs-lookup"><span data-stu-id="f8452-114">The following screenshot shows a representative configuration used to create a new Redis Cache resource.</span></span>

    ![Skärmbild som visar bladet för Azure-portalen när du skapar en ny Redis Cache-resurs, med exempel på DNS-namn, prenumeration, ny resursgrupp, plats och prisnivå.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> <span data-ttu-id="f8452-116">Du måste vänta tills cachen distribueras innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="f8452-116">You will have to wait until the cache is deployed before continuing.</span></span> <span data-ttu-id="f8452-117">Den här processen kan ta lite tid.</span><span class="sxs-lookup"><span data-stu-id="f8452-117">This process might take some time.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="f8452-118">Hämta åtkomstnycklarna och värdnamnet</span><span class="sxs-lookup"><span data-stu-id="f8452-118">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="f8452-119">I Azure-portalen bläddrar du till din cache och väljer **Inställningar** > **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="f8452-119">In the Azure portal, browse to your cache and select **Settings** > **Access keys**.</span></span> <span data-ttu-id="f8452-120">Anteckna **Primär anslutningssträng (StackExchange.Redis)**.</span><span class="sxs-lookup"><span data-stu-id="f8452-120">Write down the **Primary connection string (StackExchange.Redis)**.</span></span>

    <span data-ttu-id="f8452-121">Den här nyckeln innehåller din primära nyckel och värdnamnet i en fullständig anslutningssträng, för användning i programinställningarna för StackExchange.Redis-paketet som vi använder.</span><span class="sxs-lookup"><span data-stu-id="f8452-121">This key includes your primary key and host name in a complete connection string for use within your application settings for the StackExchange.Redis package we are using.</span></span>

1. <span data-ttu-id="f8452-122">Starta Anteckningar eller något annat textredigeringsprogram på datorn.</span><span class="sxs-lookup"><span data-stu-id="f8452-122">Start Notepad or another text editor on your computer.</span></span>

1. <span data-ttu-id="f8452-123">Lägg till följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="f8452-123">Add the following contents:</span></span>

    <span data-ttu-id="f8452-124">Ersätt `<connection-string>` med cachens primära anslutningssträng från ovan.</span><span class="sxs-lookup"><span data-stu-id="f8452-124">Replace `<connection-string>` with your cache's primary connection string from above.</span></span>

    ```xml
    <appSettings>
        <add key="CacheConnection" value="<connection-string>"/>
    </appSettings>
    ```

1. <span data-ttu-id="f8452-125">Spara filen på en plats där den inte inkluderas i källkoden till exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f8452-125">Save the file in a location where it won't be included within the source code of your sample application.</span></span> <span data-ttu-id="f8452-126">I den här självstudiekursen kommer vi att spara filen som `C:\AppSecrets\CacheSecrets.config`.</span><span class="sxs-lookup"><span data-stu-id="f8452-126">For this tutorial, we will save the file as `C:\AppSecrets\CacheSecrets.config`.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="f8452-127">Skapa en konsolapp</span><span class="sxs-lookup"><span data-stu-id="f8452-127">Create a console app</span></span>

1. <span data-ttu-id="f8452-128">Starta Visual Studio och klicka på menykommandot **File**  > **New**  > **Project...**.</span><span class="sxs-lookup"><span data-stu-id="f8452-128">Start Visual Studio and click the **File** > **New** > **Project...** menu item.</span></span>

1. <span data-ttu-id="f8452-129">Välj mallen **Visual C#** > **Windows Desktop** > **Console App**.</span><span class="sxs-lookup"><span data-stu-id="f8452-129">Select the **Visual C#** > **Windows Desktop** > **Console App** template.</span></span>

1. <span data-ttu-id="f8452-130">Ange ett namn och klicka på **OK** för att skapa ett nytt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="f8452-130">Give your app a name and click **OK** to create a new console application.</span></span>

## <a name="configure-the-cache-client"></a><span data-ttu-id="f8452-131">Konfigurera cacheklienten</span><span class="sxs-lookup"><span data-stu-id="f8452-131">Configure the cache client</span></span>

<span data-ttu-id="f8452-132">I det här avsnittet konfigurerar du konsolprogrammet att använda **StackExchange.Redis**-klienten för .NET.</span><span class="sxs-lookup"><span data-stu-id="f8452-132">In this section, you will configure the console application to use the **StackExchange.Redis** client for .NET.</span></span>

1. <span data-ttu-id="f8452-133">I Visual Studio klickar du på **Tools** pekar på **NuGet Package Manager**, klickar på **Package Manager Console** och kör följande kommando från Package Manager Console-fönstret:</span><span class="sxs-lookup"><span data-stu-id="f8452-133">In Visual Studio, click **Tools**, point to **NuGet Package Manager**, click **Package Manager Console**, and run the following command from the Package Manager Console window:</span></span>

    ```powershell
    Install-Package StackExchange.Redis
    ```

<span data-ttu-id="f8452-134">När installationen är klar är StackExchange.Redis-klienten redo för användning med ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="f8452-134">Once the installation is completed, the StackExchange.Redis cache client is available to use with your project.</span></span>

## <a name="connect-to-the-cache"></a><span data-ttu-id="f8452-135">Ansluta till cachen</span><span class="sxs-lookup"><span data-stu-id="f8452-135">Connect to the cache</span></span>

1. <span data-ttu-id="f8452-136">I Visual Studio öppnar du filen **App.config** i **Solution Explorer** och uppdaterar den så att den inkluderar ett **appSettings**-filattribut som refererar till filen **CacheSecrets.config**.</span><span class="sxs-lookup"><span data-stu-id="f8452-136">In Visual Studio, open the **App.config** file within the **Solution Explorer** and update it to include an **appSettings** file attribute that references the **CacheSecrets.config** file.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
        </startup>

        <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>

    </configuration>
    ```

1. <span data-ttu-id="f8452-137">I Solution Explorer högerklickar du på **Referenser** och klickar på **Lägg till referens...**. Välj **System.Configuration** från listan **Assemblies** > **Framework** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8452-137">In Solution Explorer, right-click **References** and click **Add Reference...**. Select **System.Configuration** from the **Assemblies** > **Framework** list and click **OK**.</span></span>

1. <span data-ttu-id="f8452-138">Lägg till följande `using`-instruktioner i **Program.cs**:</span><span class="sxs-lookup"><span data-stu-id="f8452-138">Add the following `using` statements to **Program.cs**:</span></span>

    ```csharp
    using StackExchange.Redis;
    using System.Configuration;
    ```

<span data-ttu-id="f8452-139">Anslutningen till Azure Redis Cache hanteras av klassen ConnectionMultiplexer.</span><span class="sxs-lookup"><span data-stu-id="f8452-139">The connection to Azure Redis Cache is managed by the ConnectionMultiplexer class.</span></span> <span data-ttu-id="f8452-140">Den här klassen ska delas och återanvändas i hela ditt klientprogram.</span><span class="sxs-lookup"><span data-stu-id="f8452-140">This class should be shared and reused throughout your client application.</span></span> <span data-ttu-id="f8452-141">Skapa inte en ny anslutning för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f8452-141">Do not create a new connection for each operation.</span></span>

<span data-ttu-id="f8452-142">Lagra aldrig autentiseringsuppgifterna i källkoden.</span><span class="sxs-lookup"><span data-stu-id="f8452-142">Never store credentials in source code.</span></span> <span data-ttu-id="f8452-143">För att hålla det här exemplet enkelt använder vi endast en config-fil för externa hemligheter.</span><span class="sxs-lookup"><span data-stu-id="f8452-143">To keep this sample simple, we're only using an external secrets config file.</span></span> <span data-ttu-id="f8452-144">En bättre metod är att använda Azure Key Vault med certifikat.</span><span class="sxs-lookup"><span data-stu-id="f8452-144">A better approach would be to use Azure Key Vault with certificates.</span></span>

1. <span data-ttu-id="f8452-145">I **Program.cs**, lägger du till följande medlemmar i klassen Program för ditt konsolprogram:</span><span class="sxs-lookup"><span data-stu-id="f8452-145">In **Program.cs**, add the following members to the Program class of your console application:</span></span>

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

<span data-ttu-id="f8452-146">Den här metoden för att dela en ConnectionMultiplexer-instans i ditt program använder en statisk egenskap som returnerar en ansluten instans.</span><span class="sxs-lookup"><span data-stu-id="f8452-146">This approach to sharing a ConnectionMultiplexer instance in your application uses a static property that returns a connected instance.</span></span> <span data-ttu-id="f8452-147">Koden ger ett trådsäkert sätt att endast initiera en enda ansluten ConnectionMultiplexer-instans.</span><span class="sxs-lookup"><span data-stu-id="f8452-147">The code provides a thread-safe way to initialize only a single connected ConnectionMultiplexer instance.</span></span> <span data-ttu-id="f8452-148">**abortConnect** är inställt på falskt, vilket innebär att anropet lyckas även om en anslutning till Azure Redis Cache inte etableras.</span><span class="sxs-lookup"><span data-stu-id="f8452-148">**abortConnect** is set to false, which means that the call succeeds even if a connection to Azure Redis Cache is not established.</span></span> <span data-ttu-id="f8452-149">En viktig egenskap i ConnectionMultiplexer är att anslutningen till cachen återställs automatiskt när nätverksproblemet eller andra fel har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="f8452-149">One key feature of ConnectionMultiplexer is that it automatically restores connectivity to the cache once the network issue or other causes are resolved.</span></span>

<span data-ttu-id="f8452-150">Värdet för appSetting **CacheConnection** används för att referera till cache-anslutningssträngen från Azure-portalen som lösenordsparameter.</span><span class="sxs-lookup"><span data-stu-id="f8452-150">The value of the **CacheConnection** appSetting value is used to reference the cache connection string from the Azure portal as the password parameter.</span></span>
