<span data-ttu-id="e0b57-101">Du bestämmer dig för att distribuera Azure AD och använda principer för villkorsstyrd åtkomst som gör att Azure kräver multifaktorautentisering när någon använder Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0b57-101">You decide to deploy Azure AD and use conditional access policies that Azure require Multi-Factor Authentication when anyone accesses the Azure portal.</span></span> <span data-ttu-id="e0b57-102">Du behöver skapa en katalog och använda tillfälliga licenser.</span><span class="sxs-lookup"><span data-stu-id="e0b57-102">You need to create a directory and get temporary licenses in place.</span></span>

## <a name="create-a-directory"></a><span data-ttu-id="e0b57-103">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="e0b57-103">Create a directory</span></span>
<span data-ttu-id="e0b57-104">Vi skapar en ny katalog för First Up Consultants, där vi kan testa utan att det påverkar produktionsanvändarna.</span><span class="sxs-lookup"><span data-stu-id="e0b57-104">We will create a new directory for First Up Consultants where we can test without fear of impacting production users.</span></span>

1. <span data-ttu-id="e0b57-105">Logga in på [Azure-portalen](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="e0b57-105">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="e0b57-106">I det vänstra navigeringsfönstret klickar du på **Skapa en resurs** > **Identitet** > **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-106">In the left navigation pane, click **Create a resource** > **Identity** > **Azure Active Directory**.</span></span>

1. <span data-ttu-id="e0b57-107">På bladet **Skapa katalog** anger du följande värden för **Organisationsnamn** och **Ursprungligt domännamn**:</span><span class="sxs-lookup"><span data-stu-id="e0b57-107">In the **Create directory** blade, provide the following values for the **Organization name** and **Initial domain name**:</span></span>

   1. <span data-ttu-id="e0b57-108">ORGANISATIONSNAMN: `First Up Consultants`.</span><span class="sxs-lookup"><span data-stu-id="e0b57-108">ORGANIZATION NAME: `First Up Consultants`.</span></span>
   1. <span data-ttu-id="e0b57-109">URSPRUNGLIGT DOMÄNNAMN: `firstupconsultants<XXXX>.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="e0b57-109">INITIAL DOMAIN NAME: `firstupconsultants<XXXX>.onmicrosoft.com`.</span></span>

1. <span data-ttu-id="e0b57-110">Vänta tills katalogen skapas.</span><span class="sxs-lookup"><span data-stu-id="e0b57-110">Wait for the directory to be created.</span></span> <span data-ttu-id="e0b57-111">Klicka på länken för att växla till den nya katalogen eller klicka på **Katalog- och prenumerationsfilter** längst upp i fönstret och välj sedan den nya katalogen.</span><span class="sxs-lookup"><span data-stu-id="e0b57-111">Click the link to switch to the new directory, or click the **Directory and subscription filter** at the top of the window and then choose the newly created directory.</span></span>

## <a name="get-trial-licenses"></a><span data-ttu-id="e0b57-112">Skaffa utvärderingslicenser</span><span class="sxs-lookup"><span data-stu-id="e0b57-112">Get trial licenses</span></span>

<span data-ttu-id="e0b57-113">För att kunna använda funktioner som villkorsstyrd åtkomst och multifaktorautentisering behöver du minst en utvärderingslicens.</span><span class="sxs-lookup"><span data-stu-id="e0b57-113">In order for you to use features like conditional access and Multi-Factor Authentication, you will need at least a trial license.</span></span> <span data-ttu-id="e0b57-114">Följande steg visar hur du aktiverar en utvärderingslicens:</span><span class="sxs-lookup"><span data-stu-id="e0b57-114">The following steps walk you through how to enable a trial license:</span></span>

1. <span data-ttu-id="e0b57-115">I Azure AD-fönsterrutan **Översikt** klickar du på länken **Starta en kostnadsfri utvärdering**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-115">In the Azure AD **Overview** pane, click the **Start a free trial** link.</span></span>

1. <span data-ttu-id="e0b57-116">Under objektet **Azure AD Premium P2** klickar du på **Kostnadsfri utvärderingsversion** och klickar sedan på **Aktivera**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-116">Under the item **Azure AD Premium P2**, click **Free trial**, and then click **Activate**.</span></span>

## <a name="create-a-test-user"></a><span data-ttu-id="e0b57-117">Skapa en testanvändare</span><span class="sxs-lookup"><span data-stu-id="e0b57-117">Create a test user</span></span>

<span data-ttu-id="e0b57-118">Vi behöver testa det här med en användare.</span><span class="sxs-lookup"><span data-stu-id="e0b57-118">We're going to need to test this out with a user.</span></span> <span data-ttu-id="e0b57-119">Isabella Simonsen (en annan medlem i teamet) har ställt upp som frivillig för att hjälpa till. Hon behöver ett konto i katalogen, så vi går igenom stegen för att skapa kontot.</span><span class="sxs-lookup"><span data-stu-id="e0b57-119">Isabella Simonsen (another member of your team) has volunteered to help you out. She will need an account in the directory, so we will go through the steps to create her account.</span></span>

1. <span data-ttu-id="e0b57-120">Gå till **Azure Active Directory** > **Användare** > **Alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-120">Browse to **Azure Active Directory** > **Users** > **All users**.</span></span>

1. <span data-ttu-id="e0b57-121">Klicka på **Ny användare**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-121">Click **New user**.</span></span>

1. <span data-ttu-id="e0b57-122">Skapa en användare med namnet **Isabella Simonsen** med användarnamnet:</span><span class="sxs-lookup"><span data-stu-id="e0b57-122">Create a user named **Isabella Simonsen** with a user name of:</span></span>

   * `Isabella@firstupconsultants<XXXX>.onmicrosoft.com`

1. <span data-ttu-id="e0b57-123">Markera kryssrutan för att **Visa lösenord** för användaren.</span><span class="sxs-lookup"><span data-stu-id="e0b57-123">Check the box to **Show Password** for the user.</span></span> <span data-ttu-id="e0b57-124">Anteckna lösenordet så att du kan använda det senare när du testar.</span><span class="sxs-lookup"><span data-stu-id="e0b57-124">Make a note of the password so you can use it later when testing.</span></span>

1. <span data-ttu-id="e0b57-125">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-125">Click **Create**.</span></span>

## <a name="create-a-pilot-group"></a><span data-ttu-id="e0b57-126">Skapa en pilotgrupp</span><span class="sxs-lookup"><span data-stu-id="e0b57-126">Create a pilot group</span></span>

<span data-ttu-id="e0b57-127">Vi kommer att tilldela den princip som vi skapar till en grupp med användare, men vi måste skapa en grupp för den här principen.</span><span class="sxs-lookup"><span data-stu-id="e0b57-127">We will be assigning the policy that we create to a group of users, but we need to create a group for this policy.</span></span> <span data-ttu-id="e0b57-128">Följande steg hjälper dig att skapa en säkerhetsgrupp för pilotdistributionen.</span><span class="sxs-lookup"><span data-stu-id="e0b57-128">The following steps help you create a security group for the pilot deployment.</span></span>

1. <span data-ttu-id="e0b57-129">Gå till **Azure Active Directory** > **Grupper** > **Alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-129">Browse to **Azure Active Directory** > **Groups** > **All groups**.</span></span>

1. <span data-ttu-id="e0b57-130">Klicka på **Ny grupp**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-130">Click **New group**.</span></span>

1. <span data-ttu-id="e0b57-131">Grupptyp **Säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-131">Group type **Security**.</span></span>

1. <span data-ttu-id="e0b57-132">Gruppnamn **CA-MFA-AzurePortal**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-132">Group name **CA-MFA-AzurePortal**.</span></span>

1. <span data-ttu-id="e0b57-133">Medlemskapstyp **Tilldelad**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-133">Membership type **Assigned**.</span></span>

1. <span data-ttu-id="e0b57-134">Välj den användare som vi skapade i föregående steg och välj **Välj**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-134">Select the user that we created in the previous step, and choose **Select**.</span></span>

1. <span data-ttu-id="e0b57-135">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e0b57-135">Click **Create**.</span></span>

## <a name="summary"></a><span data-ttu-id="e0b57-136">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e0b57-136">Summary</span></span>

<span data-ttu-id="e0b57-137">I den här enheten lärde du dig hur du skapar en katalog med utvärderingslicens, en testanvändare och en pilotgrupp i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0b57-137">In this unit, you learned how to create a trial licensed directory, a test user, and a pilot group in the Azure portal.</span></span>
