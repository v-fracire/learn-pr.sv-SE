<span data-ttu-id="d0169-101">I den här övningen skapar du en .NET Core-konsolapp och ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d0169-101">In this unit, you will create a .NET Core console application and an Azure storage account.</span></span>

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="d0169-102">Skapa en .NET Core-konsolapp</span><span class="sxs-lookup"><span data-stu-id="d0169-102">Create a .NET Core console application</span></span>

<span data-ttu-id="d0169-103">Vi kommer att skapa appen med Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d0169-103">We will be using Visual Studio 2017 to create our app.</span></span>

1. <span data-ttu-id="d0169-104">Öppna Visual Studio 2017 och välj menyalternativet **Arkiv** > **Nytt** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d0169-104">In Visual Studio 2017, select the **File** > **New** > **Project** menu option.</span></span>

1. <span data-ttu-id="d0169-105">I kategorin **Visual C# - .NET Core** väljer du **Konsolapp (.NET Core)**, anger ett namn för appen och väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0169-105">Within the **Visual C# - .NET Core** category, select **Console App (.NET Core)**, enter a name for your app and select **OK**.</span></span>
  <span data-ttu-id="d0169-106">![Ny App](..\media-draft\3-new-console-app.png)</span><span class="sxs-lookup"><span data-stu-id="d0169-106">![New App](..\media-draft\3-new-console-app.png)</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="d0169-107">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="d0169-107">Create an Azure storage account</span></span>

<span data-ttu-id="d0169-108">Innan vi kan ansluta till ett Azure Storage-konto måste vi först skapa lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="d0169-108">In order to connect to an Azure storage account, we must first create a storage account to connect to.</span></span>

1. <span data-ttu-id="d0169-109">Välj Skapa en resurs uppe till vänster i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d0169-109">In the top left of the Azure Portal, select 'Create a resource'.</span></span>

1. <span data-ttu-id="d0169-110">Välj ”Storage” i urvalsfönstret som visas.</span><span class="sxs-lookup"><span data-stu-id="d0169-110">In the selection pane that appears, select 'Storage'.</span></span>

1. <span data-ttu-id="d0169-111">Välj ”Lagringskonto – blob, fil, tabell, kö” till höger i fönstret.</span><span class="sxs-lookup"><span data-stu-id="d0169-111">On the right side of that pane, select 'Storage account - blob, file, table, queue'.</span></span>
  <span data-ttu-id="d0169-112">![val av portal](..\media-draft\3-portal-storage-select.png)</span><span class="sxs-lookup"><span data-stu-id="d0169-112">![portal select](..\media-draft\3-portal-storage-select.png)</span></span>

1. <span data-ttu-id="d0169-113">Du måste ange information om lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="d0169-113">The details of the storage account need to be entered.</span></span> <span data-ttu-id="d0169-114">Ange ett namn för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="d0169-114">Enter a name for the storage account.</span></span>

1. <span data-ttu-id="d0169-115">En resursgrupp är en logisk container för resurser.</span><span class="sxs-lookup"><span data-stu-id="d0169-115">A resource group is a logical container for resources.</span></span> <span data-ttu-id="d0169-116">Välj **Skapa ny** och ange ett namn på din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0169-116">For the resource group selection, select **Create New** and enter a name for your resource group.</span></span>

1. <span data-ttu-id="d0169-117">Lämna standardvärdena för övriga alternativ och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d0169-117">Leave all other options at their default values and click **Create**.</span></span>
  <span data-ttu-id="d0169-118">![Portalinformation](..\media-draft\3-portal-storage-details.png).</span><span class="sxs-lookup"><span data-stu-id="d0169-118">![Portal details](..\media-draft\3-portal-storage-details.png).</span></span>

<span data-ttu-id="d0169-119">Din resursgrupp etableras.</span><span class="sxs-lookup"><span data-stu-id="d0169-119">Your resource group is now being provisioned.</span></span> <span data-ttu-id="d0169-120">Sedan skapas ditt lagringskonto i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d0169-120">After that, your storage account is created within the resource group.</span></span>
<span data-ttu-id="d0169-121">Dessutom har du skapat en app som nu kan konfigureras och anslutas till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d0169-121">Additionally, you have created your application and are ready to configure and connect it to your storage account.</span></span>
