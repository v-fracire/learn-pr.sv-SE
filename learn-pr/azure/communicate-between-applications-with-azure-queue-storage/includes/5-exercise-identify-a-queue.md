<span data-ttu-id="37fff-101">I den här övningen letar du upp och kopierar anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="37fff-101">In this exercise, you will locate and copy your connection string.</span></span> <span data-ttu-id="37fff-102">Sedan lägger du till den i det tillhandahållna startkodsprojektet.</span><span class="sxs-lookup"><span data-stu-id="37fff-102">You will then add it to the provided stater code project.</span></span>

## <a name="get-your-connection-string"></a><span data-ttu-id="37fff-103">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="37fff-103">Get your connection string</span></span>

<span data-ttu-id="37fff-104">Din anslutningssträng är tillgänglig i avsnittet Inställningar på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="37fff-104">Your connection string is available in the settings section of your storage account.</span></span>

1. <span data-ttu-id="37fff-105">I en webbläsare navigerar du till [Azure-portalen](https://portal.azure.com?azure-portal=true) och loggar in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="37fff-105">In a web browser, navigate to the [Azure portal](https://portal.azure.com?azure-portal=true) and log in to your account.</span></span>

1. <span data-ttu-id="37fff-106">På startsidan klickar du på **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="37fff-106">On the home page, click **All Resources**.</span></span>

1. <span data-ttu-id="37fff-107">Välj det lagringskonto som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="37fff-107">Select the storage account you created earlier.</span></span>

1. <span data-ttu-id="37fff-108">Leta upp avsnittet **Inställningar** och väljer **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="37fff-108">Locate the **Settings** section and select **Access keys**.</span></span>

1. <span data-ttu-id="37fff-109">Under **key1** och till höger om textrutan **Anslutningssträng** klickar du på knappen **Klicka för att kopiera**.</span><span class="sxs-lookup"><span data-stu-id="37fff-109">Under **key1**, to the right of the **Connection string** textbox, click the **Click to copy** button.</span></span>

1. <span data-ttu-id="37fff-110">Spara anslutningssträngen för användning senare i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="37fff-110">Save the connection string for use later in this exercise.</span></span>

## <a name="clone-the-starter-application"></a><span data-ttu-id="37fff-111">Stäng startprogrammet</span><span class="sxs-lookup"><span data-stu-id="37fff-111">Clone the starter application</span></span>

<span data-ttu-id="37fff-112">Du kommer att slutföra två konsolprogram i Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="37fff-112">You will complete two console applications in Visual Studio Code.</span></span> <span data-ttu-id="37fff-113">Det första placerar meddelanden i en kö och det andra hämtar dem.</span><span class="sxs-lookup"><span data-stu-id="37fff-113">The first places messages into a queue and the second retrieves them.</span></span> <span data-ttu-id="37fff-114">Programmen är en del av en enda .NET Core-lösning som har tillhandahållits.</span><span class="sxs-lookup"><span data-stu-id="37fff-114">The applications are part of a single .NET Core solution that has been provided.</span></span> <span data-ttu-id="37fff-115">Börja genom att klona lösningen:</span><span class="sxs-lookup"><span data-stu-id="37fff-115">Start by cloning the solution:</span></span>

1. <span data-ttu-id="37fff-116">Starta en kommandotolk och ändra till den katalog där du vill hantera källkoden för programmet.</span><span class="sxs-lookup"><span data-stu-id="37fff-116">Start a command prompt and change to the directory where you want to host the source code for the application.</span></span>

1. <span data-ttu-id="37fff-117">Ange följande kommando och tryck på **Enter**:</span><span class="sxs-lookup"><span data-stu-id="37fff-117">Type the following command and then press **Enter**:</span></span>

```
git clone https:\\ TODO (add git URL)
```

## <a name="add-your-connection-string-to-the-project"></a><span data-ttu-id="37fff-118">Lägga till anslutningssträngen i projektet</span><span class="sxs-lookup"><span data-stu-id="37fff-118">Add your connection string to the project</span></span>

<span data-ttu-id="37fff-119">Slutligen lägger du till anslutningssträngen till både sändningsappen och mottagningsappen.</span><span class="sxs-lookup"><span data-stu-id="37fff-119">Finally, you will add your connection string to both the sender and receiver apps.</span></span>

> [!NOTE]
> <span data-ttu-id="37fff-120">För enkelhetens skull placerar du anslutningssträngen i filen **Program.cs** för båda konsolprogrammen.</span><span class="sxs-lookup"><span data-stu-id="37fff-120">For simplicity, you will place the connection string in the **Program.cs** file of both console applications.</span></span> <span data-ttu-id="37fff-121">I ett produktionsprogram ska du lagra den på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="37fff-121">In a production application, you should store it in a secure location.</span></span>

1. <span data-ttu-id="37fff-122">Öppna lösningen i **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="37fff-122">Open the solution in **Visual Studio Code**.</span></span>

1. <span data-ttu-id="37fff-123">I **Explorer**-fönsterrutan i mappen **sendarticle** klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="37fff-123">In the **Explorer** pane, in the **sendarticle** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="37fff-124">Leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="37fff-124">Locate the following line of code:</span></span>

    ```C#
    static string connectionString = "";
    ```

1. <span data-ttu-id="37fff-125">Kopiera anslutningssträngen till variabeln `connectionString`.</span><span class="sxs-lookup"><span data-stu-id="37fff-125">Copy your connection string into the `connectionString` variable.</span></span>

1. <span data-ttu-id="37fff-126">I **Explorer**-fönsterrutan i mappen **receivearticle** klickar du på filen **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="37fff-126">In the **Explorer** pane, in the **receivearticle** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="37fff-127">Leta upp följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="37fff-127">Locate the following line of code:</span></span>

    ```C#
    static string connectionString = "";
    ```

1. <span data-ttu-id="37fff-128">Kopiera anslutningssträngen till variabeln `connectionString`.</span><span class="sxs-lookup"><span data-stu-id="37fff-128">Copy your connection string into the `connectionString` variable.</span></span>

1. <span data-ttu-id="37fff-129">Klicka på **Arkiv** och sedan på **Spara alla**.</span><span class="sxs-lookup"><span data-stu-id="37fff-129">Click **File** and then click **Save All**.</span></span>

## <a name="summary"></a><span data-ttu-id="37fff-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="37fff-130">Summary</span></span>

<span data-ttu-id="37fff-131">Den kod som konfigurerar en anslutningssträng för ett lagringskonto är nu slutförd.</span><span class="sxs-lookup"><span data-stu-id="37fff-131">The code that configures a connection string to a storage account is now complete.</span></span>