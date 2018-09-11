<span data-ttu-id="e46e3-101">En medarbetare som heter Alain på First Up Consultants behöver kunna skapa och hantera virtuella datorer till ett projekt han arbetar med.</span><span class="sxs-lookup"><span data-stu-id="e46e3-101">A co-worker named Alain at First Up Consultants needs the ability to create and manage virtual machines for a project he is working on.</span></span> <span data-ttu-id="e46e3-102">Din chef har bett dig att hantera denna begäran.</span><span class="sxs-lookup"><span data-stu-id="e46e3-102">Your manager has asked that you handle this request.</span></span> <span data-ttu-id="e46e3-103">Utifrån regelverket om att bevilja användare lägsta möjliga behörighet för att utföra sitt arbete väljer du att skapa en ny resursgrupp och tilldela Alain rollen Virtuell datordeltagare.</span><span class="sxs-lookup"><span data-stu-id="e46e3-103">Using the best practice to grant users the least privileges to get their work done, you decide to create a new resource group and assign Alain the Virtual Machine Contributor role.</span></span>

## <a name="sign-in-to-the-azure-portal"></a><span data-ttu-id="e46e3-104">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e46e3-104">Sign in to the Azure portal</span></span>

- <span data-ttu-id="e46e3-105">Kontrollera att du fortfarande är inloggad på Azure Portal som **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="e46e3-105">Make sure you are still signed in to the Azure portal as **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span></span> <span data-ttu-id="e46e3-106">Du hittar användarnamn och lösenord på fliken **Resurser** högst upp i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="e46e3-106">You can find the username and password on the **Resources** tab at the top of this window.</span></span>

## <a name="grant-access"></a><span data-ttu-id="e46e3-107">Bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="e46e3-107">Grant access</span></span>

<span data-ttu-id="e46e3-108">Följ dessa steg om du vill tilldela rollen Virtuell datordeltagare till en användare i resursgruppomfånget.</span><span class="sxs-lookup"><span data-stu-id="e46e3-108">Follow these steps to assign the Virtual Machine Contributor role to a user at the resource group scope.</span></span>

1. <span data-ttu-id="e46e3-109">Klicka på **Resursgrupper** i navigeringslistan.</span><span class="sxs-lookup"><span data-stu-id="e46e3-109">In the navigation list, click **Resource groups**.</span></span>

1. <span data-ttu-id="e46e3-110">Hitta och klicka på resursgruppen **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="e46e3-110">Find and click the **FirstUpConsultantsRG1-_XXXXXXX_** resource group.</span></span>

   ![Lista över resursgrupper](../media-draft/5-resource-groups.png)

1. <span data-ttu-id="e46e3-112">Klicka på **Åtkomstkontroll (IAM)** för att visa den aktuella listan med rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="e46e3-112">Click **Access control (IAM)** to see the current list of role assignments.</span></span>

   ![Bladet Åtkomstkontroll (IAM) för resursgruppen](../media-draft/5-resource-group-access-control.png)

1. <span data-ttu-id="e46e3-114">Klicka på **Lägg till** högst upp för att öppna fönstret **Lägg till behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="e46e3-114">At the top, click **Add** to open the **Add permissions** pane.</span></span>

   ![Fönstret Lägg till behörigheter](../media-draft/5-add-permissions.png)

1. <span data-ttu-id="e46e3-116">I listrutan **Roll** väljer du **Virtuell datordeltagare**.</span><span class="sxs-lookup"><span data-stu-id="e46e3-116">In the **Role** drop-down list, select **Virtual Machine Contributor**.</span></span>

1. <span data-ttu-id="e46e3-117">I listan **Välj** väljer du **LabUser-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="e46e3-117">In the **Select** list, select **LabUser-_XXXXXXX_**.</span></span>

   ![Fönstret Lägg till behörigheter ifyllt](../media-draft/5-add-permissions-save.png)

1. <span data-ttu-id="e46e3-119">Klicka på **Spara** för att skapa rolltilldelningen.</span><span class="sxs-lookup"><span data-stu-id="e46e3-119">Click **Save** to create the role assignment.</span></span>

   <span data-ttu-id="e46e3-120">Efter en liten stund tilldelas användaren **LabUser-_XXXXXXX_** rollen Virtuell datordeltagare i resursgruppomfånget **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="e46e3-120">After a few moments, the **LabUser-_XXXXXXX_** user is assigned the Virtual Machine Contributor role at the **FirstUpConsultantsRG1-_XXXXXXX_** resource group scope.</span></span> <span data-ttu-id="e46e3-121">Användaren kan nu skapa och hantera virtuella datorer bara i den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e46e3-121">The user can now create and manage virtual machines just within this resource group.</span></span>

   ![Rolltilldelningen Virtuell datordeltagare](../media-draft/5-vm-contributor-assignment.png)

## <a name="remove-access"></a><span data-ttu-id="e46e3-123">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="e46e3-123">Remove access</span></span>

<span data-ttu-id="e46e3-124">I RBAC tar du bort en rolltilldelning för att ta bort åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e46e3-124">In RBAC, to remove access, you remove a role assignment.</span></span>

1. <span data-ttu-id="e46e3-125">I listan över rolltilldelningar väljer du användaren **LabUser-_XXXXXXX_** med rollen Virtuell datordeltagare.</span><span class="sxs-lookup"><span data-stu-id="e46e3-125">In the list of role assignments, select the **LabUser-_XXXXXXX_** user with the Virtual Machine Contributor role.</span></span>

1. <span data-ttu-id="e46e3-126">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e46e3-126">Click **Remove**.</span></span>

   ![Meddelandet Ta bort rolltilldelning](../media-draft/5-remove-role-assignment.png)

1. <span data-ttu-id="e46e3-128">När du ser meddelandet **Ta bort rolltilldelningar** klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="e46e3-128">In the **Remove role assignments** message that appears, click **Yes**.</span></span>

## <a name="summary"></a><span data-ttu-id="e46e3-129">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e46e3-129">Summary</span></span>

<span data-ttu-id="e46e3-130">I det här avsnittet lärde du dig att bevilja en användare åtkomst för att skapa och hantera virtuella datorer i en resursgrupp med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="e46e3-130">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using the Azure portal.</span></span> <span data-ttu-id="e46e3-131">I nästa avsnitt får du se hur du beviljar åtkomst med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e46e3-131">In the next unit, you look at how to grant access using PowerShell.</span></span>
