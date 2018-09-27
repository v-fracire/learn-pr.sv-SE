<span data-ttu-id="fc9bd-101">Du bestämmer dig för att distribuera Azure AD och använda principer för villkorsstyrd åtkomst som gör att Azure kräver multifaktorautentisering när någon använder Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-101">You decide to deploy Azure AD and use conditional access policies that Azure require Multi-Factor Authentication when anyone accesses the Azure portal.</span></span> <span data-ttu-id="fc9bd-102">Du behöver skapa en katalog och använda tillfälliga licenser.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-102">You need to create a directory and get temporary licenses in place.</span></span>

## <a name="launch-lab-and-sign-in-to-the-azure-portal"></a><span data-ttu-id="fc9bd-103">Starta labbuppgiften och logga in i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fc9bd-103">Launch lab and sign in to the Azure portal</span></span>

1. <span data-ttu-id="fc9bd-104">Klicka på länken **Launch Lab** (Starta labbuppgift) ovan, så startas labbuppgiften.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-104">Click the **Launch Lab** link above to launch the lab.</span></span>

> [!NOTE]
> <span data-ttu-id="fc9bd-105">Efter att du har startat labbuppgiften finns användarnamnet och lösenordet som du måste logga in med på fliken **Resurser** bredvid instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-105">After launching the lab, the username and password you need to sign in with is located on the **Resources** tab next to the instructions.</span></span>

## <a name="create-a-directory"></a><span data-ttu-id="fc9bd-106">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="fc9bd-106">Create a directory</span></span>

<span data-ttu-id="fc9bd-107">Vi skapar en ny katalog för First Up Consultants, där vi kan testa utan att det påverkar produktionsanvändarna.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-107">We will create a new directory for First Up Consultants where we can test without fear of impacting production users.</span></span>

1. <span data-ttu-id="fc9bd-108">Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="fc9bd-108">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

2. <span data-ttu-id="fc9bd-109">I det vänstra navigeringsfönstret klickar du på **Skapa en resurs** > **Identitet** > **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-109">In the left navigation pane, click **Create a resource** > **Identity** > **Azure Active Directory**.</span></span>

3. <span data-ttu-id="fc9bd-110">På bladet **Skapa katalog** anger du följande värden för **Organisationsnamn** och **Ursprungligt domännamn**:</span><span class="sxs-lookup"><span data-stu-id="fc9bd-110">In the **Create directory** blade, provide the following values for the **Organization name** and **Initial domain name**:</span></span>

   1. <span data-ttu-id="fc9bd-111">Organisationsnamn: `First Up Consultants`.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-111">Organization Name: `First Up Consultants`.</span></span>
   2. <span data-ttu-id="fc9bd-112">Inledande domännamn: `firstupconsultants<XXXXXXX>` där <XXXXXXX> är antalet efter användarnamnet på fliken **resurser** högst upp i fönstret instruktioner.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-112">Initial Domain Name: `firstupconsultants<XXXXXXX>` where <XXXXXXX> is the number after the username on the **Resources** tab at the top of the instructions window.</span></span>

4. <span data-ttu-id="fc9bd-113">Vänta tills katalogen skapas.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-113">Wait for the directory to be created.</span></span> <span data-ttu-id="fc9bd-114">Klicka på länken för att växla till den nya katalogen eller klicka på **Katalog- och prenumerationsfilter** längst upp i fönstret och välj sedan den nya katalogen.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-114">Click the link to switch to the new directory or click the **Directory and subscription filter** at the top of the window and then choose the newly created directory.</span></span>

## <a name="get-trial-licenses"></a><span data-ttu-id="fc9bd-115">Skaffa utvärderingslicenser</span><span class="sxs-lookup"><span data-stu-id="fc9bd-115">Get trial licenses</span></span>

<span data-ttu-id="fc9bd-116">För att använda funktioner som villkorsstyrd åtkomst och multifaktorautentisering behöver du minst en utvärderingslicens.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-116">To use features like conditional access and Multi-Factor Authentication, you will need at least a trial license.</span></span> <span data-ttu-id="fc9bd-117">Följande steg visar hur du aktiverar en utvärderingslicens:</span><span class="sxs-lookup"><span data-stu-id="fc9bd-117">The following steps walk you through how to enable a trial license:</span></span>

1. <span data-ttu-id="fc9bd-118">I Azure AD-fönsterrutan **Översikt** klickar du på länken **Starta en kostnadsfri utvärdering**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-118">In the Azure AD **Overview** pane, click the **Start a free trial** link.</span></span>

1. <span data-ttu-id="fc9bd-119">Under objektet **Azure AD Premium P2** klickar du på **Kostnadsfri utvärderingsversion** och klickar sedan på **Aktivera**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-119">Under the item **Azure AD Premium P2**, click **Free trial**, and then click **Activate**.</span></span>

## <a name="create-a-test-user"></a><span data-ttu-id="fc9bd-120">Skapa en testanvändare</span><span class="sxs-lookup"><span data-stu-id="fc9bd-120">Create a test user</span></span>

<span data-ttu-id="fc9bd-121">Vi behöver testa det här med en användare.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-121">We're going to need to test this out with a user.</span></span> <span data-ttu-id="fc9bd-122">Isabella Simonsen (en annan medlem i teamet) har ställt upp som frivillig för att hjälpa till. Hon behöver ett konto i katalogen, så vi går igenom stegen för att skapa kontot.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-122">Isabella Simonsen (another member of your team) has volunteered to help you out. She will need an account in the directory, so we will go through the steps to create her account.</span></span>

1. <span data-ttu-id="fc9bd-123">Gå till **Azure Active Directory** > **Användare**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-123">Browse to **Azure Active Directory** > **Users**.</span></span>

1. <span data-ttu-id="fc9bd-124">Klicka på **Ny användare**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-124">Click **New user**.</span></span>

1. <span data-ttu-id="fc9bd-125">Skapa en användare med namnet **Isabella Simonsen** med användarnamnet:</span><span class="sxs-lookup"><span data-stu-id="fc9bd-125">Create a user named **Isabella Simonsen** with a user name of:</span></span>

   `Isabella@firstupconsultants<XXXXXXX>.onmicrosoft.com`

   <span data-ttu-id="fc9bd-126">Matcha domänen efter @ med den domän du skapade i avsnittet *Skapa en katalog* ovan.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-126">Match the domain after the @ with the domain you created in the *Create a directory* section above.</span></span>

1. <span data-ttu-id="fc9bd-127">Markera kryssrutan för att **Visa lösenord** för användaren.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-127">Check the box to **Show Password** for the user.</span></span> <span data-ttu-id="fc9bd-128">Anteckna lösenordet så att du kan använda det senare när du testar.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-128">Make a note of the password so you can use it later when testing.</span></span>

1. <span data-ttu-id="fc9bd-129">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-129">Click **Create**.</span></span>

## <a name="create-a-pilot-group"></a><span data-ttu-id="fc9bd-130">Skapa en pilotgrupp</span><span class="sxs-lookup"><span data-stu-id="fc9bd-130">Create a pilot group</span></span>

<span data-ttu-id="fc9bd-131">Vi kommer att tilldela den princip som vi skapar till en grupp med användare, men vi måste skapa en grupp för den här principen.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-131">We will be assigning the policy that we create to a group of users, but we need to create a group for this policy.</span></span> <span data-ttu-id="fc9bd-132">Följande steg hjälper dig att skapa en säkerhetsgrupp för pilotdistributionen.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-132">The following steps help you create a security group for the pilot deployment.</span></span>

1. <span data-ttu-id="fc9bd-133">Gå till **Azure Active Directory** > **Grupper**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-133">Browse to **Azure Active Directory** > **Groups**.</span></span>

1. <span data-ttu-id="fc9bd-134">Klicka på **Ny grupp**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-134">Click **New group**.</span></span>

1. <span data-ttu-id="fc9bd-135">Grupptyp **Säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-135">Group type **Security**.</span></span>

1. <span data-ttu-id="fc9bd-136">Gruppnamn **CA-MFA-AzurePortal**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-136">Group name **CA-MFA-AzurePortal**.</span></span>

1. <span data-ttu-id="fc9bd-137">Medlemskapstyp **Tilldelad**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-137">Membership type **Assigned**.</span></span>

1. <span data-ttu-id="fc9bd-138">Välj den användare som vi skapade i föregående steg och välj **Välj**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-138">Select the user that we created in the previous step and choose **Select**.</span></span>

1. <span data-ttu-id="fc9bd-139">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-139">Click **Create**.</span></span>

<span data-ttu-id="fc9bd-140">I den här enheten lärde du dig hur du skapar en katalog med utvärderingslicens, en testanvändare och en pilotgrupp i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fc9bd-140">In this unit, you learned how to create a trial licensed directory, a test user, and a pilot group in the Azure portal.</span></span>