Att använda bladet **Åtkomstkontroll (IAM)** i Azure Portal skulle ha fungerat bra, men du får många behörighetsbegäranden varje dag. Om du vill hålla jämna steg med åtkomsthanteringen, kan du använda PowerShell till att automatisera vissa av stegen.

## <a name="open-cloud-shell-powershell"></a>Öppna Cloud Shell PowerShell

1. Kontrollera att du fortfarande är inloggad på Azure Portal som **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**. Du hittar användarnamn och lösenord på fliken **Resurser** högst upp i det här fönstret.

1. Överst i portalen klickar du på **Cloud Shell** för att öppna fönstret Cloud Shell.

    ![Cloud Shell-knapp](../media-draft/6-cloud-shell-button.png)

1. Kontrollera att den är inställd på **PowerShell** i det övre vänstra hörnet i fönstret Cloud Shell. Om den är inställd på **Bash** ändrar du detta till **PowerShell**.

    Det kan ta en stund att läsa in den. När det är klart ser det ut ungefär som följande:

    ![Cloud Shell PowerShell](../media-draft/6-cloud-shell-powershell.png)

## <a name="grant-access"></a>Bevilja åtkomst

Om du vill bevilja åtkomst till en användare med Azure PowerShell använder du kommandot [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment). Du måste ange säkerhetsobjekt, rolldefinition och omfång.

Följ dessa steg om du vill tilldela rollen Virtuell datordeltagare till **LabUser-_XXXXXXX_**-användaren i resursgruppsomfånget.

1. På fliken **Resurser** högst upp i det här fönstret kan du kopiera kommandot **Bevilja åtkomst till PowerShell**.

1. Klistra in kommandot i PowerShell-fönstret och tryck på Retur-tangenten för att köra det. Nedan visas ett exempel på kommando och utdata:

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

    Utdatan visar att rollen Virtuell datordeltagare har tilldelats till LabUser-_XXXXXXX_ i omfånget FirstUpConsultantsRG1-_XXXXXXX_.

## <a name="list-access"></a>Lista för åtkomst

Om du vill kontrollera åtkomsten för resursgruppen använder du kommandot [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) för att visa en lista med rolltilldelningarna.

Följ dessa steg om du vill visa en lista med alla rolltilldelningar för **LabUser-XXXXXXX**-användaren i resursgruppsomfånget.

1. På fliken **Resurser** högst upp i det här fönstret kan du kopiera kommandot **Lista för åtkomst till PowerShell**.

1. Klistra in kommandot i PowerShell-fönstret och tryck på Retur-tangenten för att köra det. Nedan visas ett exempel på kommando och utdata.

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

    Utdatan visar att rollen Virtuell datordeltagare har tilldelats till LabUser-_XXXXXXX_ i omfånget FirstUpConsultantsRG1-_XXXXXXX_.

    Om du uppdaterar bladet **Åtkomstkontroll (IAM)** för resursgruppen i Azure Portal, ser rolltilldelningen ut så här:

    ![Rolltilldelningar för en användare med resursgruppsomfång](../media-draft/6-cloud-shell-access-control.png)

## <a name="remove-access"></a>Ta bort åtkomst

Om du vill ta bort åtkomst för användare, grupper och program använder du [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) och tar bort en rolltilldelning.

Följ dessa steg om du vill ta bort rolltilldelningen Virtuell datordeltagare för **LabUser-_XXXXXXX_**-användaren i resursgruppsomfånget.

1. På fliken **Resurser** högst upp i det här fönstret kan du kopiera kommandot **Ta bort åtkomst till PowerShell**.

1. Klistra in kommandot i PowerShell-fönstret och tryck på Retur-tangenten för att köra det. Nedan visas ett exempel på kommandot.

    ```Example
    PS Azure:\> Remove-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"
    ```

1. I PowerShell-fönstret klickar du på knappen Stäng (**X**) för att stänga fönstret.

    ![Knappen Stäng i Cloud Shell](../media-draft/6-cloud-shell-close.png)


## <a name="summary"></a>Sammanfattning

I det här avsnittet lärde du dig att bevilja en användar åtkomst för att skapa och hantera virtuella datorer i en resursgrupp med hjälp av Azure PowerShell. I nästa avsnitt lär du dig att visa RBAC-ändringar över tid.
