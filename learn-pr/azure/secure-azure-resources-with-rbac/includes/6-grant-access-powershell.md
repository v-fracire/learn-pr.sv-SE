<span data-ttu-id="6c0a0-101">Att använda bladet **Åtkomstkontroll (IAM)** i Azure Portal skulle ha fungerat bra, men du får många behörighetsbegäranden varje dag.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-101">Using the **Access control (IAM)** blade in the Azure portal has been working fine, but you are getting several permission requests each day.</span></span> <span data-ttu-id="6c0a0-102">Om du vill hålla jämna steg med åtkomsthanteringen, kan du använda PowerShell till att automatisera vissa av stegen.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-102">To keep up with the access management tasks, you decide to use PowerShell to help automate some of the steps.</span></span>

## <a name="open-cloud-shell-powershell"></a><span data-ttu-id="6c0a0-103">Öppna Cloud Shell PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c0a0-103">Open Cloud Shell PowerShell</span></span>

1. <span data-ttu-id="6c0a0-104">Kontrollera att du fortfarande är inloggad på Azure Portal som **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-104">Make sure you are still signed in to the Azure portal as **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span></span> <span data-ttu-id="6c0a0-105">Du hittar användarnamn och lösenord på fliken **Resurser** högst upp i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-105">You can find the username and password on the **Resources** tab at the top of this window.</span></span>

1. <span data-ttu-id="6c0a0-106">Överst i portalen klickar du på **Cloud Shell** för att öppna fönstret Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-106">At the top of the portal, click **Cloud Shell** to open the Cloud Shell pane.</span></span>

    ![Cloud Shell-knapp](../media-draft/6-cloud-shell-button.png)

1. <span data-ttu-id="6c0a0-108">Kontrollera att den är inställd på **PowerShell** i det övre vänstra hörnet i fönstret Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-108">In the upper left of the Cloud Shell pane, make sure it is set to **PowerShell**.</span></span> <span data-ttu-id="6c0a0-109">Om den är inställd på **Bash** ändrar du detta till **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-109">If it is set to **Bash**, change it to **PowerShell**.</span></span>

    <span data-ttu-id="6c0a0-110">Det kan ta en stund att läsa in den.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-110">It might take a few moments to load.</span></span> <span data-ttu-id="6c0a0-111">När det är klart ser det ut ungefär som följande:</span><span class="sxs-lookup"><span data-stu-id="6c0a0-111">When finished, it will look similar to the following:</span></span>

    ![Cloud Shell PowerShell](../media-draft/6-cloud-shell-powershell.png)

## <a name="grant-access"></a><span data-ttu-id="6c0a0-113">Bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="6c0a0-113">Grant access</span></span>

<span data-ttu-id="6c0a0-114">Om du vill bevilja åtkomst till en användare med Azure PowerShell använder du kommandot [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment).</span><span class="sxs-lookup"><span data-stu-id="6c0a0-114">To grant access to a user using Azure PowerShell, you use the [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) command.</span></span> <span data-ttu-id="6c0a0-115">Du måste ange säkerhetsobjekt, rolldefinition och omfång.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-115">You must specify the security principal, role definition, and scope.</span></span>

<span data-ttu-id="6c0a0-116">Följ dessa steg om du vill tilldela rollen Virtuell datordeltagare till **LabUser-_XXXXXXX_**-användaren i resursgruppsomfånget.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-116">Follow these steps to assign the Virtual Machine Contributor role to the **LabUser-_XXXXXXX_** user at the resource group scope.</span></span>

1. <span data-ttu-id="6c0a0-117">På fliken **Resurser** högst upp i det här fönstret kan du kopiera kommandot **Bevilja åtkomst till PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-117">On the **Resources** tab at the top of this window, copy the **Grant access PowerShell** command.</span></span>

1. <span data-ttu-id="6c0a0-118">Klistra in kommandot i PowerShell-fönstret och tryck på Retur-tangenten för att köra det.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-118">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="6c0a0-119">Nedan visas ett exempel på kommando och utdata:</span><span class="sxs-lookup"><span data-stu-id="6c0a0-119">The following shows an example command and the output:</span></span>

    ```Example
    PS Azure:\> New-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"

    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX
    DisplayName        : LabUser-XXXXXXX
    SignInName         : LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com
    RoleDefinitionName : Virtual Machine Contributor
    RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    <span data-ttu-id="6c0a0-120">Utdatan visar att rollen Virtuell datordeltagare har tilldelats till LabUser-_XXXXXXX_ i omfånget FirstUpConsultantsRG1-_XXXXXXX_.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-120">The output shows that the Virtual Machine Contributor role has been assigned to LabUser-_XXXXXXX_ at the FirstUpConsultantsRG1-_XXXXXXX_ scope.</span></span>

## <a name="list-access"></a><span data-ttu-id="6c0a0-121">Lista för åtkomst</span><span class="sxs-lookup"><span data-stu-id="6c0a0-121">List access</span></span>

<span data-ttu-id="6c0a0-122">Om du vill kontrollera åtkomsten för resursgruppen använder du kommandot [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) för att visa en lista med rolltilldelningarna.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-122">To verify the access for the resource group, you use the [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) command to list the role assignments.</span></span>

<span data-ttu-id="6c0a0-123">Följ dessa steg om du vill visa en lista med alla rolltilldelningar för **LabUser-XXXXXXX**-användaren i resursgruppsomfånget.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-123">Follow these steps to list all the role assignments for the **LabUser-XXXXXXX** user at the resource group scope.</span></span>

1. <span data-ttu-id="6c0a0-124">På fliken **Resurser** högst upp i det här fönstret kan du kopiera kommandot **Lista för åtkomst till PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-124">On the **Resources** tab at the top of this window, copy the **List access PowerShell** command.</span></span>

1. <span data-ttu-id="6c0a0-125">Klistra in kommandot i PowerShell-fönstret och tryck på Retur-tangenten för att köra det.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-125">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="6c0a0-126">Nedan visas ett exempel på kommando och utdata.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-126">The following shows an example command and the output.</span></span>

    ```Example
    PS Azure:\> Get-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"

    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX
    DisplayName        : LabUser-XXXXXXX
    SignInName         : LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com 
    RoleDefinitionName : Virtual Machine Contributor
    RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    <span data-ttu-id="6c0a0-127">Utdatan visar att rollen Virtuell datordeltagare har tilldelats till LabUser-_XXXXXXX_ i omfånget FirstUpConsultantsRG1-_XXXXXXX_.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-127">The output shows that the Virtual Machine Contributor role has been assigned to LabUser-_XXXXXXX_ at the FirstUpConsultantsRG1-_XXXXXXX_ scope.</span></span>

    <span data-ttu-id="6c0a0-128">Om du uppdaterar bladet **Åtkomstkontroll (IAM)** för resursgruppen i Azure Portal, ser rolltilldelningen ut så här:</span><span class="sxs-lookup"><span data-stu-id="6c0a0-128">If you refresh the **Access control (IAM)** blade for the resource group in the Azure portal, this is how the role assignment looks:</span></span>

    ![Rolltilldelningar för en användare med resursgruppsomfång](../media-draft/6-cloud-shell-access-control.png)

## <a name="remove-access"></a><span data-ttu-id="6c0a0-130">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="6c0a0-130">Remove access</span></span>

<span data-ttu-id="6c0a0-131">Om du vill ta bort åtkomst för användare, grupper och program använder du [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) och tar bort en rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-131">To remove access for users, groups, and applications, you use [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) to remove a role assignment.</span></span>

<span data-ttu-id="6c0a0-132">Följ dessa steg om du vill ta bort rolltilldelningen Virtuell datordeltagare för **LabUser-_XXXXXXX_**-användaren i resursgruppsomfånget.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-132">Follow these steps to remove the Virtual Machine Contributor role assignment for the **LabUser-_XXXXXX_** user at the resource group scope.</span></span>

1. <span data-ttu-id="6c0a0-133">På fliken **Resurser** högst upp i det här fönstret kan du kopiera kommandot **Ta bort åtkomst till PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-133">On the **Resources** tab at the top of this window, copy the **Remove access PowerShell** command.</span></span>

1. <span data-ttu-id="6c0a0-134">Klistra in kommandot i PowerShell-fönstret och tryck på Retur-tangenten för att köra det.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-134">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="6c0a0-135">Nedan visas ett exempel på kommandot.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-135">The following shows an example command.</span></span>

    ```Example
    PS Azure:\> Remove-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"
    ```

1. <span data-ttu-id="6c0a0-136">I PowerShell-fönstret klickar du på knappen Stäng (**X**) för att stänga fönstret.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-136">In the PowerShell pane, click the close (**X**) button to close the pane.</span></span>

    ![Knappen Stäng i Cloud Shell](../media-draft/6-cloud-shell-close.png)


## <a name="summary"></a><span data-ttu-id="6c0a0-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6c0a0-138">Summary</span></span>

<span data-ttu-id="6c0a0-139">I det här avsnittet lärde du dig att bevilja en användar åtkomst för att skapa och hantera virtuella datorer i en resursgrupp med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-139">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using Azure PowerShell.</span></span> <span data-ttu-id="6c0a0-140">I nästa avsnitt lär du dig att visa RBAC-ändringar över tid.</span><span class="sxs-lookup"><span data-stu-id="6c0a0-140">In the next unit, you learn how to view the RBAC changes over time.</span></span>
