<span data-ttu-id="7d9c3-101">En medarbetare som heter Alain på First Up Consultants behöver kunna skapa och hantera virtuella datorer till ett projekt han arbetar med.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-101">A co-worker named Alain at First Up Consultants needs the ability to create and manage virtual machines for a project he is working on.</span></span> <span data-ttu-id="7d9c3-102">Din chef har bett dig att hantera denna begäran.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-102">Your manager has asked that you handle this request.</span></span> <span data-ttu-id="7d9c3-103">Utifrån regelverket om att bevilja användare lägsta möjliga behörighet för att utföra sitt arbete väljer du att tilldela Alain rollen Virtuell datordeltagare för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-103">Using the best practice to grant users the least privileges to get their work done, you decide to assign Alain the Virtual Machine Contributor role for a resource group.</span></span>

## <a name="grant-access"></a><span data-ttu-id="7d9c3-104">Bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="7d9c3-104">Grant access</span></span>

<span data-ttu-id="7d9c3-105">Följ dessa steg om du vill tilldela rollen Virtuell datordeltagare till en användare i resursgruppomfånget.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-105">Follow these steps to assign the Virtual Machine Contributor role to a user at the resource group scope.</span></span>

1. <span data-ttu-id="7d9c3-106">Klicka på **Resursgrupper** i navigeringslistan.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-106">In the navigation list, click **Resource groups**.</span></span>

1. <span data-ttu-id="7d9c3-107">Leta upp och klicka på resursgruppen **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-107">Find and click the **FirstUpConsultantsRG1-_XXXXXXX_** resource group.</span></span>

1. <span data-ttu-id="7d9c3-108">Klicka på **Åtkomstkontroll (IAM)** för att visa den aktuella listan med rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-108">Click **Access control (IAM)** to see the current list of role assignments.</span></span>

   ![Åtkomstkontroll – rolltilldelning för resursgrupp](../media/5-resource-group-role-assignment.png)

1. <span data-ttu-id="7d9c3-110">Klicka på **Lägg till** längst upp för att öppna fönsterrutan **Lägg till behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-110">At the top, click **Add** to open the **Add permissions** pane.</span></span>

   ![Fönstret Lägg till behörigheter](../media/5-add-permissions.png)

1. <span data-ttu-id="7d9c3-112">I listrutan **Roll** väljer du **Virtuell datordeltagare**.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-112">In the **Role** drop-down list, select **Virtual Machine Contributor**.</span></span>

1. <span data-ttu-id="7d9c3-113">I listan **Välj** väljer du **LabUser-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-113">In the **Select** list, select **LabUser-_XXXXXXX_**.</span></span>

    <span data-ttu-id="7d9c3-114">Du hittar användarnamnet på fliken **Resurser** bredvid anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-114">You can find the username on the **Resources** tab next to the instructions.</span></span>

   ![Fönstret Lägg till behörigheter ifyllt](../media/5-add-permissions-save.png)

1. <span data-ttu-id="7d9c3-116">Klicka på **Spara** för att skapa rolltilldelningen.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-116">Click **Save** to create the role assignment.</span></span>

   <span data-ttu-id="7d9c3-117">Efter en liten stund tilldelas användaren **LabUser-_XXXXXXX_** rollen Virtuell datordeltagare i resursgruppomfånget **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-117">After a few moments, the **LabUser-_XXXXXXX_** user is assigned the Virtual Machine Contributor role at the **FirstUpConsultantsRG1-_XXXXXXX_** resource group scope.</span></span> <span data-ttu-id="7d9c3-118">Användaren kan nu skapa och hantera virtuella datorer bara i den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-118">The user can now create and manage virtual machines just within this resource group.</span></span>

   ![Rolltilldelningen Virtuell datordeltagare](../media/5-vm-contributor-assignment.png)

## <a name="remove-access"></a><span data-ttu-id="7d9c3-120">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="7d9c3-120">Remove access</span></span>

<span data-ttu-id="7d9c3-121">I RBAC tar du bort en rolltilldelning för att ta bort åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-121">In RBAC, to remove access, you remove a role assignment.</span></span>

1. <span data-ttu-id="7d9c3-122">I listan över rolltilldelningar väljer du användaren **LabUser-_XXXXXXX_** med rollen Virtuell datordeltagare.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-122">In the list of role assignments, select the **LabUser-_XXXXXXX_** user with the Virtual Machine Contributor role.</span></span>

1. <span data-ttu-id="7d9c3-123">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-123">Click **Remove**.</span></span>

   ![Meddelandet Ta bort rolltilldelning](../media/5-remove-role-assignment.png)

1. <span data-ttu-id="7d9c3-125">När du ser meddelandet **Ta bort rolltilldelningar** klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-125">In the **Remove role assignments** message that appears, click **Yes**.</span></span>

<span data-ttu-id="7d9c3-126">I det här avsnittet lärde du dig att bevilja en användare åtkomst för att skapa och hantera virtuella datorer i en resursgrupp med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d9c3-126">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using the Azure portal.</span></span>