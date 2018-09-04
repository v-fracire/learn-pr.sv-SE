<span data-ttu-id="49576-101">I den här övningen skapar du en Azure Redis-cache för att lagra och returnera värden som används ofta.</span><span class="sxs-lookup"><span data-stu-id="49576-101">In this exercise, you'll create an Azure Redis cache to store and return commonly used values.</span></span>

## <a name="create-a-cache"></a><span data-ttu-id="49576-102">Skapa en cache</span><span class="sxs-lookup"><span data-stu-id="49576-102">Create a cache</span></span>

1. <span data-ttu-id="49576-103">Logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="49576-103">Sign in to the Azure portal.</span></span> <span data-ttu-id="49576-104">Klicka sedan på **Skapa en resurs**, klicka på **Databaser** och klicka på **Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="49576-104">Then click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    ![Skapa en cache 1](../media-draft/4-create-a-cache-1.png)

## <a name="configure-your-cache"></a><span data-ttu-id="49576-106">Konfigurera din cache</span><span class="sxs-lookup"><span data-stu-id="49576-106">Configure your cache</span></span>

1. <span data-ttu-id="49576-107">**DNS-namn:** skapa ett globalt unikt namn, till exempel **ContosoSportsApp1028**</span><span class="sxs-lookup"><span data-stu-id="49576-107">**DNS Name:** Create a globally unique such as **ContosoSportsApp1028**</span></span>
1. <span data-ttu-id="49576-108">**Prenumeration**: välj din prenumeration</span><span class="sxs-lookup"><span data-stu-id="49576-108">**Subscription:** Select your subscription</span></span>
1. <span data-ttu-id="49576-109">**Resursgrupp**: skapa en ny resursgrupp och ge den ett namn</span><span class="sxs-lookup"><span data-stu-id="49576-109">**Resource group:** Create a new resource group and give it a name</span></span>
1. <span data-ttu-id="49576-110">**Plats:** eftersom de flesta av dina kunder finns i New York du bör välja **USA, östra**</span><span class="sxs-lookup"><span data-stu-id="49576-110">**Location:** Because most of your customers are in the New York area you should select **East US**</span></span>
1. <span data-ttu-id="49576-111">**Prisnivå:** välj **Basic C0**</span><span class="sxs-lookup"><span data-stu-id="49576-111">**Pricing tier:** Select **Basic C0**</span></span>
1. <span data-ttu-id="49576-112">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="49576-112">Click **Create**</span></span>

    ![Skapa en cache 2](../media-draft/4-create-a-cache-2.png)

> [!Important]
> <span data-ttu-id="49576-114">Du måste vänta tills cachen distribueras innan du fortsätter. Den här processen kan ta lite tid.</span><span class="sxs-lookup"><span data-stu-id="49576-114">You will have to wait until the cache is deployed before continuing, this process make take some time.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="49576-115">Hämta åtkomstnycklarna och värdnamnet</span><span class="sxs-lookup"><span data-stu-id="49576-115">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="49576-116">I Azure-portalen bläddrar du till din cache och väljer **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="49576-116">In the Azure portal, browse to your cache and select **Access keys**.</span></span> <span data-ttu-id="49576-117">Anteckna **primärnyckel**.</span><span class="sxs-lookup"><span data-stu-id="49576-117">Write down the **Primary key**.</span></span>
1. <span data-ttu-id="49576-118">Välj **Egenskaper** och anteckna **HOST NAME** (värdnamnet).</span><span class="sxs-lookup"><span data-stu-id="49576-118">Select **Properties** and write down the **HOST NAME**.</span></span>
1. <span data-ttu-id="49576-119">Starta Anteckningar eller något annat textredigeringsprogram på datorn.</span><span class="sxs-lookup"><span data-stu-id="49576-119">Start Notepad, or another text editor on your computer.</span></span>
1. <span data-ttu-id="49576-120">Lägg till följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="49576-120">Add the following contents:</span></span>

    ```xml
    <appSettings>
    <add key="CacheConnection" value="CACHE-NAME.redis.cache.windows.net,abortConnect=false,ssl=true,password=ACCESS-KEY"/>
    </appSettings>
    ```
    <span data-ttu-id="49576-121">Ersätt **CACHE-NAME** med cachens värdnamn.</span><span class="sxs-lookup"><span data-stu-id="49576-121">Replace **CACHE-NAME** with your cache host name.</span></span>
    <span data-ttu-id="49576-122">Ersätt **ACCESS-KEY** med primärnyckeln för cachen.</span><span class="sxs-lookup"><span data-stu-id="49576-122">Replace **ACCESS-KEY** with the primary key for your cache.</span></span>

1. <span data-ttu-id="49576-123">Spara filen på en plats där den inte checkas in med källkoden för exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="49576-123">Save the file in a location where it won't be checked within the source code of your sample application.</span></span> <span data-ttu-id="49576-124">I den här självstudien sparar vi filen som *C:\AppSecrets\CacheSecrets.config*.</span><span class="sxs-lookup"><span data-stu-id="49576-124">For this     tutorial, we will save the file as *C:\AppSecrets\CacheSecrets.config*.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="49576-125">Skapa en konsolapp</span><span class="sxs-lookup"><span data-stu-id="49576-125">Create a console app</span></span>

1. <span data-ttu-id="49576-126">Starta Visual Studio och klicka på **Arkiv**, klicka på **Nytt** och klicka på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="49576-126">Start Visual Studio and click **File**, click **New**, and click **Project**.</span></span>
1. <span data-ttu-id="49576-127">Expandera **Visual C#**, klicka på **Windows Desktop** och klicka sedan på **Konsolapp**.</span><span class="sxs-lookup"><span data-stu-id="49576-127">Expand **Visual C#**, click **Windows Desktop** and then click **Console App**.</span></span>
1. <span data-ttu-id="49576-128">Klicka på **OK** för att skapa ett nytt konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="49576-128">Click **OK** to create a new console application.</span></span>

## <a name="configure-the-cache-client"></a><span data-ttu-id="49576-129">Konfigurera cacheklienten</span><span class="sxs-lookup"><span data-stu-id="49576-129">Configure the cache client</span></span>

<span data-ttu-id="49576-130">I det här avsnittet konfigurerar du konsolprogrammet att använda StackExchange.Redis-klienten för .NET.</span><span class="sxs-lookup"><span data-stu-id="49576-130">In this section, you will configure the console application to use the StackExchange.Redis client for .NET.</span></span>

1. <span data-ttu-id="49576-131">I Visual Studio klickar du på **Verktyg** pekar på **NuGet Package Manager**, klickar på **Package Manager Console** och kör följande kommando från Package Manager Console-fönstret.</span><span class="sxs-lookup"><span data-stu-id="49576-131">In Visual Studio, click **Tools**, point to **NuGet Package Manager**, click **Package Manager Console**, and run the following command from the Package Manager Console window.</span></span>

    ```powershell
    Install-Package StackExchange.Redis
    ```

<span data-ttu-id="49576-132">När installationen är klar är StackExchange.Redis-klienten redo för användning med ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="49576-132">Once the installation is completed, the StackExchange.Redis cache client is available to use with your project.</span></span>

## <a name="connect-to-the-cache"></a><span data-ttu-id="49576-133">Ansluta till cachen</span><span class="sxs-lookup"><span data-stu-id="49576-133">Connect to the cache</span></span>

1. <span data-ttu-id="49576-134">I Visual Studio klickar du på **Solution Explorer**, dubbelklickar på filen **App.config** och uppdaterar den så att den inkluderar ett appSettings-filattribut refererar till filen CacheSecrets.config.</span><span class="sxs-lookup"><span data-stu-id="49576-134">In Visual Studio, click **Solution Explorer**, double-click the **App.config** file and update it to include an appSettings file attribute that references the CacheSecrets.config file.</span></span>

    ![Skapa en cache 4](../media-draft/4-create-a-cache-4.png)

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
        </startup>

        <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>

    </configuration>
    ```

1. <span data-ttu-id="49576-136">I Solution Explorer högerklickar du på **Referenser** och klickar på **Lägg till en referens**.</span><span class="sxs-lookup"><span data-stu-id="49576-136">In Solution Explorer, right-click **References** and click **Add a reference**.</span></span> <span data-ttu-id="49576-137">Välj **System.Configuration** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="49576-137">Select **System.Configuration** and click **OK**.</span></span>

    ![Skapa en cache 5](../media-draft/4-create-a-cache-5.png)

1. <span data-ttu-id="49576-139">Lägg till följande med hjälp av instruktioner till Program.cs:</span><span class="sxs-lookup"><span data-stu-id="49576-139">Add the following using statements to Program.cs:</span></span>

    ```csharp
    using StackExchange.Redis;
    using System.Configuration;
    ```

    ![Skapa en cache 6](../media-draft/4-create-a-cache-6.png)

<span data-ttu-id="49576-141">Anslutningen till Azure Redis Cache hanteras av klassen ConnectionMultiplexer.</span><span class="sxs-lookup"><span data-stu-id="49576-141">The connection to the Azure Redis Cache is managed by the ConnectionMultiplexer class.</span></span> <span data-ttu-id="49576-142">Den här klassen ska delas och återanvändas i hela ditt klientprogram.</span><span class="sxs-lookup"><span data-stu-id="49576-142">This class should be shared and reused throughout your client application.</span></span> <span data-ttu-id="49576-143">Skapa inte en ny anslutning för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="49576-143">Do not create a new connection for each operation.</span></span>

<span data-ttu-id="49576-144">Lagra aldrig autentiseringsuppgifterna i källkoden.</span><span class="sxs-lookup"><span data-stu-id="49576-144">Never store credentials in source code.</span></span> <span data-ttu-id="49576-145">För att hålla det här exemplet enkelt använder jag endast en config-fil för externa hemligheter.</span><span class="sxs-lookup"><span data-stu-id="49576-145">To keep this sample simple, I’m only using an external secrets config file.</span></span> <span data-ttu-id="49576-146">En bättre metod är att använda Azure Key Vault med certifikat.</span><span class="sxs-lookup"><span data-stu-id="49576-146">A better approach would be to use Azure Key Vault with certificates.</span></span>

1. <span data-ttu-id="49576-147">I Program.cs, lägger du till följande medlemmar i klassen Program för ditt konsolprogram:</span><span class="sxs-lookup"><span data-stu-id="49576-147">In Program.cs, add the following members to the Program class of your console application:</span></span>

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

    ![Skapa en cache 7](../media-draft/4-create-a-cache-7.png)

<span data-ttu-id="49576-149">Den här metoden för att dela en ConnectionMultiplexer-instans i ditt program använder en statisk egenskap som returnerar en ansluten instans.</span><span class="sxs-lookup"><span data-stu-id="49576-149">This approach to sharing a ConnectionMultiplexer instance in your application uses a static property that returns a connected instance.</span></span> <span data-ttu-id="49576-150">Koden ger ett trådsäkert sätt att endast initiera en enda ansluten ConnectionMultiplexer-instans.</span><span class="sxs-lookup"><span data-stu-id="49576-150">The code provides a thread-safe way to initialize only a single connected ConnectionMultiplexer instance.</span></span> <span data-ttu-id="49576-151">abortConnect är inställt på falskt, vilket innebär att anropet lyckas även om en anslutning till Azure Redis Cache inte etableras.</span><span class="sxs-lookup"><span data-stu-id="49576-151">abortConnect is set to false, which means that the call succeeds even if a connection to the Azure Redis Cache is not established.</span></span> <span data-ttu-id="49576-152">En viktig egenskap i ConnectionMultiplexer är att anslutningen till cachen återställs automatiskt när nätverksproblemet eller andra fel har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="49576-152">One key feature of ConnectionMultiplexer is that it automatically restores connectivity to the cache once the network issue or other causes are resolved.</span></span>

<span data-ttu-id="49576-153">Värdet för appSetting CacheConnection används för att referera till cache-anslutningssträngen från Azure-portalen som lösenordsparameter.</span><span class="sxs-lookup"><span data-stu-id="49576-153">The value of the CacheConnection appSetting is used to reference the cache connection string from the Azure portal as the password parameter.</span></span>
