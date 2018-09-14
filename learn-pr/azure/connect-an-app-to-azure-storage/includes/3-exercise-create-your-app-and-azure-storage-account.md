<span data-ttu-id="f57f5-101">I den här övningen skapar du en .NET Core-konsolapp och ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f57f5-101">In this unit, you will create a .NET Core console application and an Azure storage account.</span></span>

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="f57f5-102">Skapa en .NET Core-konsolapp</span><span class="sxs-lookup"><span data-stu-id="f57f5-102">Create a .NET Core console application</span></span>

<span data-ttu-id="f57f5-103">Vi kommer att skapa appen med Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="f57f5-103">We will be using Visual Studio 2017 to create our app.</span></span>

1. <span data-ttu-id="f57f5-104">Öppna Visual Studio 2017 och välj menyalternativet **Arkiv** > **Nytt** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f57f5-104">In Visual Studio 2017, select the **File** > **New** > **Project** menu option.</span></span>

1. <span data-ttu-id="f57f5-105">I kategorin **Visual C# - .NET Core** väljer du **Konsolapp (.NET Core)**, anger ett namn för appen och väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="f57f5-105">Within the **Visual C# - .NET Core** category, select **Console App (.NET Core)**, enter a name for your app and select **OK**.</span></span>

  ![Skärmbild av Visual Studio 2017 som visar dialogrutan för nytt projekt med .NET Core, Konsolapp och namnfält markerat.](..\media-draft\3-new-console-app.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="f57f5-107">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="f57f5-107">Create an Azure storage account</span></span>

<span data-ttu-id="f57f5-108">För att ansluta vår app till en Azure storage-resurs, måste vi först skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f57f5-108">In order to connect our app to an Azure storage resource, we must first create a storage account.</span></span>

1. <span data-ttu-id="f57f5-109">Längst upp till vänster på Azure portal, Välj **skapa en resurs**.</span><span class="sxs-lookup"><span data-stu-id="f57f5-109">In the top left of the Azure portal, select **Create a resource**.</span></span>

1. <span data-ttu-id="f57f5-110">Välj i rutan val av **Storage** > **lagringskonto – blob, fil, tabell, kö**.</span><span class="sxs-lookup"><span data-stu-id="f57f5-110">In the selection pane that appears, select **Storage** > **Storage account - blob, file, table, queue**.</span></span>

  ![Skärmbild av Azure-portalen som visar skapa ett Resursblad med Storage kategori och Lagringskonto visas markerad.](..\media-draft\3-portal-storage-select.png)

1. <span data-ttu-id="f57f5-112">En resursgrupp är en logisk behållare för att organisera relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="f57f5-112">A resource group is a logical container to help organize related resources.</span></span> <span data-ttu-id="f57f5-113">Det val av resursgrupp, Välj **Skapa nytt** och ange ett namn för din nya resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f57f5-113">For the resource group selection, select **Create new** and enter a name for your new resource group.</span></span>

1. <span data-ttu-id="f57f5-114">Ange ett globalt unikt namn för din **lagringskontonamn**.</span><span class="sxs-lookup"><span data-stu-id="f57f5-114">Enter a globally unique name for your **Storage account name**.</span></span>

1. <span data-ttu-id="f57f5-115">Lämna övriga alternativ till sina standardvärden och klicka på **granska + skapa**.</span><span class="sxs-lookup"><span data-stu-id="f57f5-115">Leave all other options at their default values and click **Review + create**.</span></span>

  ![Skärmbild av Azure-portalen som visar bladet skapa storage-konto med exempelinformation i de obligatoriska fälten; beskrivs resursen fält, namnfältet för Storage-konto och granska + skapa knappen är markerade.](..\media-draft\3-portal-storage-details.png)

1. <span data-ttu-id="f57f5-117">Kontrollera informationen på granskningssidan det förväntade och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="f57f5-117">Make sure the details on the review page match what you expected and click **Create**.</span></span>

<span data-ttu-id="f57f5-118">Din resursgrupp etableras.</span><span class="sxs-lookup"><span data-stu-id="f57f5-118">Your resource group is now being provisioned.</span></span> <span data-ttu-id="f57f5-119">Sedan skapas ditt lagringskonto i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f57f5-119">After that, your storage account is created within the resource group.</span></span>
<span data-ttu-id="f57f5-120">Dessutom har du skapat en app som nu kan konfigureras och anslutas till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f57f5-120">Additionally, you have created your application and are ready to configure and connect it to your storage account.</span></span>
