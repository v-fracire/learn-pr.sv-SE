<span data-ttu-id="ff150-101">Du arbetar på First Up Consultants och har beviljats åtkomst till Azure-prenumerationen för marknadsföringsteamet.</span><span class="sxs-lookup"><span data-stu-id="ff150-101">At First Up Consultants, you've been granted access to the Azure subscription for the marketing team.</span></span> <span data-ttu-id="ff150-102">Du vill bekanta dig med Azure Portal och se vilka roller som är tilldelade för närvarande.</span><span class="sxs-lookup"><span data-stu-id="ff150-102">You want to familiarize yourself with the Azure portal and see what roles are currently assigned.</span></span>

## <a name="list-role-assignments-for-yourself"></a><span data-ttu-id="ff150-103">Visa en lista med egna rolltilldelningar</span><span class="sxs-lookup"><span data-stu-id="ff150-103">List role assignments for yourself</span></span>

<span data-ttu-id="ff150-104">Följ stegen nedan om du vill se vilka roller som du har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="ff150-104">Follow these steps to see what roles are currently assigned to you.</span></span>

1. <span data-ttu-id="ff150-105">Klicka på **Azure Active Directory** i navigeringslistan.</span><span class="sxs-lookup"><span data-stu-id="ff150-105">In the navigation list, click **Azure Active Directory**.</span></span>

1. <span data-ttu-id="ff150-106">Öppna **Alla användare** genom att klicka på **Användare**.</span><span class="sxs-lookup"><span data-stu-id="ff150-106">Click **Users** to open **All users**.</span></span>

    ![Azure Active Directory-användare](../media-draft/4-aad-all-users.png)

1. <span data-ttu-id="ff150-108">Leta upp och klicka på användarnamnet **LabAdmin-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="ff150-108">Find and click the **LabAdmin-_XXXXXXX_** user name.</span></span>

    ![Azure Active Directory-labbanvändare](../media-draft/4-aad-all-users-lab.png)

1. <span data-ttu-id="ff150-110">Klicka på **Azure-resurser** i avsnittet **Hantera**.</span><span class="sxs-lookup"><span data-stu-id="ff150-110">In the **Manage** section, click **Azure resources**.</span></span>

    ![Azure-resurser](../media-draft/4-aad-user-azure-resources.png)

    <span data-ttu-id="ff150-112">På bladet Azure-resurser ser du de resurser och roller som du har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="ff150-112">On the Azure resources blade, you can see the resources and roles you have access to.</span></span> <span data-ttu-id="ff150-113">Din lista ser annorlunda ut.</span><span class="sxs-lookup"><span data-stu-id="ff150-113">Your list will look different.</span></span>

## <a name="list-role-assignments-for-a-resource-group"></a><span data-ttu-id="ff150-114">Visa rolltilldelningar för en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="ff150-114">List role assignments for a resource group</span></span>

<span data-ttu-id="ff150-115">Följ stegen nedan om du vill se vilka roller som har tilldelats till omfånget för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ff150-115">Follow these steps to see what roles are assigned at the resource group scope.</span></span>

1. <span data-ttu-id="ff150-116">Klicka på **Resursgrupper** i navigeringslistan.</span><span class="sxs-lookup"><span data-stu-id="ff150-116">In the navigation list, click **Resource groups**.</span></span>

   ![Resursgrupper](../media-draft/4-resource-groups.png)

1. <span data-ttu-id="ff150-118">Klicka på resursgruppen med namnet **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="ff150-118">Click the resource group named **FirstUpConsultantsRG1-_XXXXXXX_**.</span></span>

1. <span data-ttu-id="ff150-119">Klicka på **Åtkomstkontroll (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="ff150-119">Click **Access control (IAM)**.</span></span>

   <span data-ttu-id="ff150-120">På bladet Åtkomstkontroll (IAM) kan du se vem som har åtkomst till den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ff150-120">On the Access control (IAM) blade, you can see who has access to this resource group.</span></span> <span data-ttu-id="ff150-121">Observera att vissa roller är begränsade till **Den här resursen** medan andra är **(Ärvda)** från ett överordnat omfång.</span><span class="sxs-lookup"><span data-stu-id="ff150-121">Notice that some roles are scoped to **This resource** while others are **(Inherited)** from a parent scope.</span></span>

   ![Åtkomstkontroll (IAM) för resursgrupp](../media-draft/4-resource-group-access-control.png)

## <a name="list-roles"></a><span data-ttu-id="ff150-123">Visa en lista över roller</span><span class="sxs-lookup"><span data-stu-id="ff150-123">List roles</span></span>

<span data-ttu-id="ff150-124">Som du lärde dig i den föregående kursdelen är en roll en uppsättning behörigheter.</span><span class="sxs-lookup"><span data-stu-id="ff150-124">As you learned in the previous unit, a role is a collection of permissions.</span></span> <span data-ttu-id="ff150-125">Azure har över 70 inbyggda roller som du kan använda i dina rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="ff150-125">Azure has over 70 built-in roles that you can use in your role assignments.</span></span> <span data-ttu-id="ff150-126">Följ stegen nedan för att visa en lista över rollerna.</span><span class="sxs-lookup"><span data-stu-id="ff150-126">Follow these steps to list the roles.</span></span>

- <span data-ttu-id="ff150-127">Klicka på **Roller** längst upp på bladet Åtkomstkontroll (IAM) för att visa en lista över alla inbyggda och anpassade roller.</span><span class="sxs-lookup"><span data-stu-id="ff150-127">At the top of the Access control (IAM) blade, click **Roles** to see a list of all the built-in and custom roles.</span></span>

   ![Alternativet Roller](../media-draft/4-roles-option.png)

   <span data-ttu-id="ff150-129">Du kan se hur många användare och grupper som är kopplade till varje roll.</span><span class="sxs-lookup"><span data-stu-id="ff150-129">You can see the number of users and groups that are assigned to each role.</span></span>

   ![Lista över roller](../media-draft/4-roles-list.png)

## <a name="summary"></a><span data-ttu-id="ff150-131">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ff150-131">Summary</span></span>

<span data-ttu-id="ff150-132">I den här kursdelen har du lärt dig hur du visar en lista över de roller som du har tilldelats på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ff150-132">In this unit, you learned how to list the role assignments for yourself in the Azure portal.</span></span> <span data-ttu-id="ff150-133">Du har också lärt dig hur du visar en lista över rolltilldelningarna i olika omfång.</span><span class="sxs-lookup"><span data-stu-id="ff150-133">You also learned how to list the role assignments at different scopes.</span></span> <span data-ttu-id="ff150-134">I nästa kursdel ska vi gå vidare och använda rollbaserad åtkomstkontroll (RBAC) för att bevilja åtkomst till en användare.</span><span class="sxs-lookup"><span data-stu-id="ff150-134">In the next unit, you take the next step and use RBAC to grant access to a user.</span></span>
