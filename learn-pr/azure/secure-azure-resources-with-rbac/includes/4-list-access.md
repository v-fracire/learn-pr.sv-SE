> [!NOTE]
> <span data-ttu-id="f8712-101">När du har startat labbuppgiften finns användarnamnet och lösenordet som du behöver på fliken **Resurser** bredvid instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="f8712-101">After launching the lab, the username and password you need is located on the **Resources** tab next to the instructions.</span></span>

<span data-ttu-id="f8712-102">Du arbetar på First Up Consultants och har beviljats åtkomst till en resursgrupp för marknadsföringsteamet.</span><span class="sxs-lookup"><span data-stu-id="f8712-102">At First Up Consultants, you've been granted access to a resource group for the marketing team.</span></span> <span data-ttu-id="f8712-103">Du vill bekanta dig med Azure-portalen och se vilka roller som är tilldelade för närvarande.</span><span class="sxs-lookup"><span data-stu-id="f8712-103">You want to familiarize yourself with the Azure portal and see what roles are currently assigned.</span></span>

## <a name="list-role-assignments-for-yourself"></a><span data-ttu-id="f8712-104">Visa en lista med egna rolltilldelningar</span><span class="sxs-lookup"><span data-stu-id="f8712-104">List role assignments for yourself</span></span>

<span data-ttu-id="f8712-105">Följ stegen nedan om du vill se vilka roller som du har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="f8712-105">Follow these steps to see what roles are currently assigned to you.</span></span>

1. <span data-ttu-id="f8712-106">Öppna menyn genom att klicka på ditt användarnamn i det övre högra hörnet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f8712-106">In the upper-right corner of the Azure portal, click your user name to open the menu.</span></span>

    ![Menyn Mina behörigheter](../media/4-my-permissions-menu.png)

1. <span data-ttu-id="f8712-108">Klicka på **Mina behörigheter** för att öppna fönstret Mina behörigheter.</span><span class="sxs-lookup"><span data-stu-id="f8712-108">Click **My permissions** to open the My permissions pane.</span></span>

    ![Fönstret Mina behörigheter](../media/4-my-permissions-pane.png)

    <span data-ttu-id="f8712-110">I fönstret Mina behörigheter kan du se en lista över de roller som du har tilldelats samt omfattningen.</span><span class="sxs-lookup"><span data-stu-id="f8712-110">On the My permissions pane, you can see a list of the roles that you have been assigned and the scope.</span></span> <span data-ttu-id="f8712-111">Din lista ser annorlunda ut.</span><span class="sxs-lookup"><span data-stu-id="f8712-111">Your list will look different.</span></span>

## <a name="list-role-assignments-for-a-resource-group"></a><span data-ttu-id="f8712-112">Visa rolltilldelningar för en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f8712-112">List role assignments for a resource group</span></span>

<span data-ttu-id="f8712-113">Följ stegen nedan om du vill se vilka roller som har tilldelats till omfånget för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f8712-113">Follow these steps to see what roles are assigned at the resource group scope.</span></span>

1. <span data-ttu-id="f8712-114">Klicka på **Resursgrupper** i navigeringslistan.</span><span class="sxs-lookup"><span data-stu-id="f8712-114">In the navigation list, click **Resource groups**.</span></span>

   ![Resursgrupper](../media/4-resource-groups.png)

1. <span data-ttu-id="f8712-116">Leta reda på och klicka på resursgruppen med namnet **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="f8712-116">Find and click the resource group named **FirstUpConsultantsRG1-_XXXXXXX_**.</span></span>

1. <span data-ttu-id="f8712-117">Klicka på **Åtkomstkontroll (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="f8712-117">Click **Access control (IAM)**.</span></span>

   ![Åtkomstkontroll för resursgrupp](../media/4-resource-group-access-control.png)

    <span data-ttu-id="f8712-119">Du kan se vem som har åtkomst till den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f8712-119">You can see who has access to this resource group.</span></span> <span data-ttu-id="f8712-120">Observera att vissa roller är begränsade till **Den här resursen** medan andra är **(Ärvda)** från ett överordnat omfång.</span><span class="sxs-lookup"><span data-stu-id="f8712-120">Notice that some roles are scoped to **This resource** while others are **(Inherited)** from a parent scope.</span></span>

   ![Åtkomstkontroll – rolltilldelning för resursgrupp](../media/4-resource-group-role-assignment.png)

## <a name="list-roles"></a><span data-ttu-id="f8712-122">Visa en lista över roller</span><span class="sxs-lookup"><span data-stu-id="f8712-122">List roles</span></span>

<span data-ttu-id="f8712-123">Som du lärde dig i den föregående kursdelen är en roll en uppsättning behörigheter.</span><span class="sxs-lookup"><span data-stu-id="f8712-123">As you learned in the previous unit, a role is a collection of permissions.</span></span> <span data-ttu-id="f8712-124">Azure har över 70 inbyggda roller som du kan använda i dina rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="f8712-124">Azure has over 70 built-in roles that you can use in your role assignments.</span></span> <span data-ttu-id="f8712-125">Följ det här steget om du vill visa rollerna.</span><span class="sxs-lookup"><span data-stu-id="f8712-125">Follow this step to list the roles.</span></span>

- <span data-ttu-id="f8712-126">Klicka på **Roller** längst upp i fönstret för att visa en lista över alla inbyggda och anpassade roller.</span><span class="sxs-lookup"><span data-stu-id="f8712-126">At the top of the pane, click **Roles** to see a list of all the built-in and custom roles.</span></span>

   <span data-ttu-id="f8712-127">Du kan se hur många användare och grupper som är kopplade till varje roll.</span><span class="sxs-lookup"><span data-stu-id="f8712-127">You can see the number of users and groups that are assigned to each role.</span></span>

   ![Lista över roller](../media/4-roles-list.png)

<span data-ttu-id="f8712-129">I den här kursdelen har du lärt dig hur du visar en lista över de roller som du har tilldelats på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f8712-129">In this unit, you learned how to list the role assignments for yourself in the Azure portal.</span></span> <span data-ttu-id="f8712-130">Du har också lärt dig hur du visar en lista över rolltilldelningarna för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f8712-130">You also learned how to list the role assignments for a resource group.</span></span>